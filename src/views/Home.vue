<template>
  <div id="home">
    <h1>Github Typeahead</h1>

    <div class="search-and-results">
      <input type="text" placeholder="search for a github user" v-model="searchTerm" @keyup="searchUsers(searchTerm)"/>
      <ul v-for="match in matches" :key="match.id" >
        <router-link class="router-link" :to="{name: 'User', 
        params: {
          name: match.login, 
          image: match.avatar_url,
          githubLink: match.html_url
          }
        } 
        ">
          <li v-if="searchTerm"><img :src="match.avatar_url" alt="profile"><p>{{match.login}}</p></li>
        </router-link>
      </ul>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';
import axios from 'axios';

export default {
  name: 'Home',

  setup() {
    let matches = ref(null);
    const searchTerm = ref('');
    

    const searchUsers = async (searchTerm) => {
      try {
        const response = await axios.get('https://api.github.com/users');
        const results = response.data;
        matches.value = results.filter((result) => {
          const regex = new RegExp(`^${searchTerm}`, 'gi');
          const matchedSearchItem =  result.login.match(regex);
          return matchedSearchItem
        });

        if(searchTerm.length === 0){
          matches.value = [];
        };
      } catch (error) {
         alert('there is a problem check your network access', error)
       
      }
    }

    return{
      searchTerm,
      matches,
      searchUsers
    }
  }
  
}
</script>
