<template>
    <div class="home">
        <h1>COVID-19 Monitor</h1>
        <Monitor class='mt-5'/>
    </div>
</template>

<script>
// @ is an alias to /src
import Monitor from '@/components/Monitor.vue'

export default {
  name: 'Home',
  components: {
    Monitor
  }
}
</script>
