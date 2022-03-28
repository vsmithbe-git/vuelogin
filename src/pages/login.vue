<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";
import useAuth from '../composable/useAuth';
import useError from "../composable/useError";
import { useTimeout, promiseTimeout } from '@vueuse/core';

const {isAuthenticated,  login, signup, googleLogin} = useAuth();

const username = ref("");
const password = ref("");

const router = useRouter();

const logginIn = async () => {
  await login(username.value, password.value);
  goToHome();
};

const signingUp = async () => {
  await signup(username.value, password.value);
  goToHome();
 };

const google = async () => {
  await googleLogin();
  goToHome();
 };

const goToHome = () => {
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
    <div class="flex space-x-2">
    <button type="submit" @submit.prevent="logginIn" class="w-1/2 bg-yellow-600 text-yellow-200 py-2 rounded-lg">Login</button>
     <button  
     @click ="signingUp" 
     class="w-1/2 bg-green-600 text-green-200 py-2 rounded-lg">SignUp</button>
    </div>
    
    <button @click="google"
    class="flex bg-white justify-center py-2 rounded-lg hover:bg-gray-300"><img src="../assets/Google__G__Logo.svg"/></button>
</form>
  </div>
  </div>
  <div v-if="!ready && error" 
  class=" transition-opacity absolute bg-red-300 w-1/4 bottom-2 right-2 rounded-lg p-4 text-center text-red-800">
    {{error}}
  </div>
 </template>