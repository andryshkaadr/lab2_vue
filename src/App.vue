<template>
  <div>
    <h1>Users</h1>
    <div class="user-counter">{{ users.length }} users</div>
    <div class="search-field">
      <input type="text" v-model="searchQuery" placeholder="Search for pokemon..." @input="filterController">
      <ul class="users-list">
        <li v-for="user in filteredUsers" :key="user.name">
          <a class="name">{{ user.name }}</a>
          <button class="delete-button" @click="deleteUser(user.name)">Delete</button>
        </li>
        <li v-if="filteredUsers.length === 0 && users.length === 0">
          <span>Not Found</span>
        </li>
      </ul>
    </div>
  </div>
</template>



<script>
const API = 'https://pokeapi.co/api/v2/pokemon/';

export default {
  data() {
    return {
      users: [],
      searchQuery: '',
    };
  },
  computed: {
    filteredUsers() {
      return this.users.filter((user) =>
        user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
  methods: {
    templateBuilder() {

    },
    filterController() {
      this.templateBuilder();
    },
    deleteUser(name) {
      this.users = this.users.filter((user) => user.name !== name);
      localStorage.setItem('users', JSON.stringify(this.users));
      this.templateBuilder();
    },
    getDataFromApi() {
      fetch(API)
        .then((response) => response.json())
        .then((data) => {
          this.users = data.results;
          localStorage.setItem('users', JSON.stringify(this.users));
          this.templateBuilder();
        });
    },
    notEmptyChecking() {
      if (this.filteredUsers.length === 0 && this.users.length === 0) {
        this.getDataFromApi();
      }
    },
  },
  mounted() {
    const storedUsers = localStorage.getItem('users');
    if (storedUsers) {
      this.users = JSON.parse(storedUsers);
      this.templateBuilder();
    } else {
      this.getDataFromApi();
    }
  },
};
</script>



<style lang="less">
@import './assets/less/index.less';
</style>