<template>
  <div class="nav_style">
    <span>Hello</span>
    <span><NuxtLink to="/info">users</NuxtLink></span>
    <span><NuxtLink to="/page2">page2</NuxtLink></span>
  </div>
  <div>
    <slot />
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.nav_style {
  color: aliceblue;
  background-color: rgb(70, 97, 123);
  height: 45px;
}

span {
  margin-left: 10px;
}
</style>
