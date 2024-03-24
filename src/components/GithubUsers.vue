<template>

  <div class="w-[500px] max-[500px]:w-[90%] mx-auto p-3 border border-gray-200 shadow-md">

    <h1 class="text-4xl max-[500px]:text-2xl text-center font-bold my-4">GitHub User Profile</h1>
    <input  class="p-3 border border-blue-200 shadow-sm rounded-md my-2 w-full outline-blue-300"
    type="text" placeholder="Enter Github-user's Name" @keydown.enter="getUser" @input="getUser" v-model="userName" >


    <div v-if="userProfile">
      <img :src="userProfile.avatar_url" class="w-52 mx-auto" /> <!--avatar_url got from https://api.github.com/users/Bloodbunn-->
      <div class="mt-5 text-left">
        <p class="font-bold">Name: <span class="font-normal">{{ userProfile.name }}</span></p>
        <p class="font-bold">Followers: <span class="font-normal">{{userProfile.followers}}</span></p>
        <p class="font-bold">Following: <span class="font-normal">{{userProfile.following}}</span></p>
      </div>
      

    </div>

    <p> {{ error }}</p>

  </div>
</template>

<script setup>
import {ref} from 'vue'

const userName = ref('')
const userProfile = ref(null)
const error = ref(null)

const getUser = async () => {
  try {
    const response = await fetch(`https://api.github.com/users/${userName.value}`) //have to use `` for string interpolation. using this interpolation, we add the name we enterd in the input into the url and search for that username such as /users/Bloodbunn
    const data = await response.json()

    if (response.ok){
      userProfile.value = data
      error.value = null
    }else {
      userProfile.value = null 
      error.value = `Error: ${data.message}`
    }
    
  } catch (err) {
    console.log("error fetching data,", err)
    error.value = "An error occurred while fetching data"
  }
}

</script>