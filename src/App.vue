<template>
    <div id="app" class="small-container">
      <Test/>
    </div>
</template>

<script>
import Test from "@/components/Test.vue"

export default {
  name: 'App',
  components: {
    Test
  },
  data() {
    return {
      questionNumber: 1
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.small-containter {
  max-width: 500px;
}

</style>
