<template lang="">
<!-- <div>
    <div>
        <input v-on:keyup="getInputData" ref="search" placeholder="Country & City" type="text">
        <button v-on:click="getInputData">Get</button>
    </div>
    <div>
        <ul>
            <li v-for="item in list" :key="item.rank">{{item.city}}</li>
        </ul>
    </div>
</div> -->
<div>
    <section class="dropdown-wrapper">
        <div @click="isVisible=!isVisible" class="selected-item">
            <span v-if="selectedItem">{{selectedItem.city}}</span>
            <span v-else>Select User</span>
            <svg :class="isVisible ? 'dropdown':''" class="select-item-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24">
                <path fill="none" d="M0 0h24v24H0z" />
                <path d="M12 10.828l-4.95 4.95-1.414-1.414L12 8l6.364 6.364-1.414 1.414z" /></svg>
        </div>
        <div v-show="isVisible" class="dropdown-popover">
            <input type="text" v-model="searchQuery" placeholder="Search for User">
            <br>
            <span v-if="filteredUser.length===0">No Data Available</span>
            <div class="options">
                <ul>
                    <li @click="selectItem(user)" v-for="(user,index) in filteredUser" :key="`user-${index}`"><a class="itemNum" href="#">{{user.city}}</a></li>

                </ul>
            </div>
        </div>
    </section>
    <section>
        <div>
            <h1 v-if="selectedItem">City: <span class="selected-city-state">{{selectedItem.city}}</span> </h1>
            <h3 v-if="selectedItem">State: <span class="selected-city-state">{{selectedItem.state}}</span> </h3>
            <h4 v-if="selectedItem">Population: <span class="selected-city-state">{{selectedItem.population}}</span></h4>
            <h3 v-else>Name</h3>

        </div>
    </section>
</div>
</template>

<script>
export default {
    name: 'Home',
    data() {
        return {
            searchQuery: '',
            selectedItem: null,
            isVisible: false,
            userArray: []
        }
    },
    computed: {
        filteredUser() {
            const query = this.searchQuery.toLowerCase();
            if (this.searchQuery === "") {
                return this.userArray
            }
            return this.userArray.filter((user) => {
                return Object.values(user).some((word) => String(word).toLowerCase().includes(query))
            })
        }
    },
    methods: {
        selectItem(user) {
            this.selectedItem = user;
            this.isVisible = false;
        }
    },
    mounted() {
        fetch("https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json")
            .then(res => res.json())
            .then((json) => {
                console.log(json);
                this.userArray = json;
            })
    },

}
</script>

<style scoped>
.dropdown-wrapper {
    max-width: 350px;
    position: relative;
    margin: 0 auto;
}

.selected-item {
    height: 40px;
    border: 2px solid lightgray;
    border-radius: 5px;
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 18px;
    font-weight: 400;
}

.dropdown-popover {
    position: absolute;
    border: 2px solid lightgray;
    top: 46px;
    left: 0;
    right: 0;
    background-color: #fff;
    max-width: 100%;
    padding: 10px;
}

.select-item-icon {
    transform: rotate(0deg);
}

.select-item-icon.dropdown {
    transform: rotate(180deg);
    transition: all 0.5s ease;
}

input {
    width: 90%;
    height: 30px;
    border: 2px solid lightgray;
    font-size: 16px;
}

.options {
    width: 100%;
}

ul {
    list-style: none;
    text-align: left;
    padding-left: 8px;
    max-height: 200px;
    overflow-y: scroll;
    overflow-x: hidden;
}

li {
    width: 100%;
    border-bottom: 1px solid lightgray;
    padding: 10px;
    cursor: pointer;
    font-size: 16px;
}

li:hover {
    background: #70878a;
    color: #fff;
    font-weight: bold;
}

.itemNum {
    text-decoration: none;
    color: black;
}
.selected-city-state{
    color: red;
}
</style>
