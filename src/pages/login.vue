<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";
import useAuth from '../composable/useAuth';
import useError from "../composable/useError";
import { useTimeout, promiseTimeout } from '@vueuse/core';

const {isAuthenticated,  login} = useAuth();
const username = ref("");
const password = ref("");

const router = useRouter();

const logginIn = async () => {
  await login(username.value, password.value);
  if(isAuthenticated.value){
    router.push("/");
  } else {
    setError("Invalid user name and/or password");
    start();
  }

};

const {error, setError} = useError();


const { ready, start } = useTimeout(3000, { controls: true }, false);


</script>

<template>
<div class="flex flex-col space-y-12 items-center justify-center min-h-screen-nonav">


  <div class="bg-gray-200 flex shadow-2xl rounded-lg justify-center items-center overflow-hidden"> 
    
      <img class = "h-64" src="../assets/bglogin.png" alt="Login BG">
  <form @submit.prevent="logginIn" class="flex flex-col p-4 space-y-4">
    <input type="text" class="border-2 p-2 rounded-lg" placeholder="Username" v-model = "username"/>
    <input type="password" class="border-2 p-2 rounded-lg" placeholder="Password" v-model = "password"/>
    <button type="submit" @submit.prevent="logginIn" class="bg-indigo-600 text-indigo-200 py-2 rounded-lg">Login</button>
  </form>
  </div>
  </div>
  <div v-if="!ready && error" 
  class=" transition-opacity absolute bg-red-300 w-1/4 bottom-2 right-2 rounded-lg p-4 text-center text-red-800">
    {{error}}
  </div>
 </template>