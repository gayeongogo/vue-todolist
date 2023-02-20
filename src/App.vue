<template>
    <nav class="navbar">
        <ul>
            <!-- <li><router-link to="/">HOME</router-link></li>
            <li><router-link to="/todos">TODOS</router-link></li> -->

            <li><router-link :to="{name: 'Home'}">HOME</router-link></li>
            <li><router-link :to="{name:'Todos'}">TODOS</router-link></li>
        </ul>
    </nav>
    <router-view />
</template>

<script>
    export default {

    }
</script>

<style>
    .navbar{border: 1px solid #ddd; width: 100%; padding: 20px;}
    .navbar ul{max-width: 1024px; width: 100%; display: flex; margin: 0 auto;}
    .navbar ul li{list-style: none;}
    .navbar ul li a{text-decoration: none; padding: 10px 20px; font-weight: bold; color: #0f5d68;}
</style>