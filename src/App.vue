<template>
  <section>
    <!-- //today date time  -->
    <div class="today" v-if="time">
      <h2>Today</h2>
      <p>{{ date }} {{ time }}</p>
    </div>
  </section>
  <main>
    <CountDown :year="2023" :month="4" :day="23" :hour="0" :minute="0" :second="0" />
  </main>
</template>

<script>
import CountDown from '@/Components/CountDown/CountDown.vue'

export default {
  name: 'App',
  components: {
    CountDown
  },
  data() {
    return {
      interval: null,
      date: null,
      time: null
    }
  },
  mounted() {
    this.date = new Date().toLocaleDateString('en-US', {
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    })
    this.interval = setInterval(() => {
      this.time = new Date().toLocaleTimeString('en-US', {
        hour12: false,
        hour: 'numeric',
        minute: 'numeric',
        second: 'numeric'
      })
    }, 1000)
  },
  beforeUnmount() {
    clearInterval(this.interval)
  }
}
</script>

<style scoped></style>
