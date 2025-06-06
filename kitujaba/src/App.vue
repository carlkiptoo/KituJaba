<template>
<div>
  <NavBar />
<section id='home'><Home /></section>
<section id='about'><About /></section>
<section id='product'><Product /></section>
<section id='contact'><Contact /></section>
</div>

</template>

<script setup>
import NavBar from './components/NavBar.vue'
import Home from './pages/HomePage.vue'
import About from './pages/AboutUs.vue'
import Product from './pages/OurProducts.vue'
import Contact from './pages/ContactUs.vue'

</script>