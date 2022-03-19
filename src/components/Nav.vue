<script setup>
import { useRouter } from "vue-router";
import useAuth from "../composable/useAuth";

const {isAuthenticated, logout} = useAuth();

const router = useRouter();

const loggingOut = () => {
    logout()
    router.push("/");
}
</script>

<template>
<div class="bg-indigo-800 text-indigo-200">	
    <div class="container mx-auto flex justify-between items-center">
        <h1 class="tracking-tight text-3xl font-thin">Vue<span class="font-normal">FakeAuth</span>
        </h1>
        <nav>
            <ul class="flex space-x-4 w-full">
                <router-link to="/"><li class="py-8 px-4 hover:cursor-pointer hover:bg-indigo-500 hover:text-indigo-800">Home</li></router-link>
                <router-link to="/about"><li class="py-8 px-4 hover:cursor-pointer hover:bg-indigo-500 hover:text-indigo-800">About</li></router-link>
                <router-link v-if="!isAuthenticated" to="/login"><li class="py-8 px-4 hover:cursor-pointer hover:bg-indigo-500 hover:text-indigo-800">Login</li></router-link>
                <div v-else class="flex">
                 <router-link  :to="{name: 'Secret'}"><li class="py-8 px-4 hover:cursor-pointer hover:bg-indigo-500 hover:text-indigo-800">Secret</li></router-link>
                <button  @click="loggingOut"><li class="py-8 px-4 hover:cursor-pointer hover:bg-indigo-500 hover:text-indigo-800">Logout</li>
                </button>
                </div>
            </ul>
        </nav>
    </div>
</div>

</template>
