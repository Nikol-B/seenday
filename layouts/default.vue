
<template>
  <div id="app" theme="default">
    <div class="layout">
      <Header />

      <Navbar />

      <div class="content">
        <slot />
        <Done />
       <Footer /> <Footer />
      </div>
    </div>
  </div>
</template>

<script setup>
import Done from "../pages/lk/manager/Done.vue"
</script>

