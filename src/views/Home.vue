<template>
  <div class="home">
    <PickBaju />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import PickBaju from '@/components/PickBaju.vue'

export default {
  name: 'home',
  components: {
    HelloWorld,
    PickBaju
  }
}
</script>
