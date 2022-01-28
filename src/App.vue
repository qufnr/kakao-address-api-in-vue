<template>
    <div id="app">
        <q-header></q-header>
        <q-address></q-address>
        <q-footer></q-footer>
    </div>
</template>

<script>
import Address from '@/components/Address'
import Footer from '@/components/Footer'
import Header from '@/components/Header'

export default {
    name: 'App',
    components: {'q-address': Address, 'q-footer': Footer, 'q-header': Header}
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /* text-align: center; */
    color: #2c3e50;
    margin-top: 60px;

    margin: 0 auto;
    max-width: 450px;
}
a {text-decoration: none; color: #41B883; cursor: pointer;}
.vue {color: #41B883; font-weight: bold;}
.title {font-size: 18px; font-weight: 500; text-align: center;}
button {border: 1px solid #696969; border-radius: 0px; background-color: #fff; padding: 4px 16px; font-weight: 600;}
.button-primary {background-color: #41B883; color: #fff; border: 1px solid #2b7755;}
</style>