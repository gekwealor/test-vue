<script setup>
  import gitHubIcon from './icons/gitHubIcon.vue';
</script>

<template>

  <nav>
    <div class="navbar">
      <gitHubIcon class="github-icon"/>
      <h1>Gina Ekwealor.</h1>
    </div>
  </nav>

</template>
 
<style scoped>

.navbar {
  display:flex; ;
  align-items: center;
  gap: 15px;
  justify-content: center;
  background-color: #0026bd7c;
  /* background-color: #1B9AAA; */
  text-align: center;
  padding: 23px;
  margin-bottom: 100px
}

.github-icon {
  font-size: 3rem;
}
</style>