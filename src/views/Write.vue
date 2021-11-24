<template>
  <div>
    <hand-write class="container"></hand-write>

  </div>
</template>

<script>
import HandWrite from '../components/HandWrite.vue'

export default {
  name: 'Home',
  components: {
    HandWrite

  }
}
</script>
<style scoped>
.container{
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  margin: 0;
  padding: 0;
}
</style>
