<template>
  <div id="main_com" v-if="loading">
    <main class="w_messsage">
      <section v-if="expired">
        <h1>Expired</h1>
      </section>
      <section v-else>
        <h1>Remaining</h1>
      </section>
    </main>
    <main class="timer_style">
      <section v-for="item in items" :key="item.name">
        <h1>{{ item[item.name] }}</h1>
        <span>{{ item.name.toUpperCase() }}</span>
      </section>
    </main>
  </div>
</template>
<script>
export default {
  name:"CountDown",
  props: {
    year: {
      type: Number,
      required: true
    },
    month: {
      type: Number,
      required: true
    },
    day: {
      type: Number,
      required: true
    },
    hour: {
      type: Number,
      required: true
    },
    minute: {
      type: Number,
      required: true
    },
    second: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      items: [
        { day: 0, name: 'day' },
        { hour: 0, name: 'hour' },
        { minute: 0, name: 'minute' },
        { second: 0, name: 'second' }
      ],
      loading: false,
      expired: false
    }
  },

  computed: {
    _second: () => 1000,
    _minute() {
      return this._second * 60
    },
    _hour() {
      return this._minute * 60
    },
    _day() {
      return this._hour * 24
    },
    eventDate() {
      return new Date(this.year, this.month - 1, this.day, this.hour, this.minute, this.second)
    }
  },

  methods: {
    addZero: (n) => (parseInt(n, 10) < 10 ? '0' : '') + n,
    _showRemainingTime() {
      let timer = setInterval(() => {
        const now = new Date()
        const eventDate = this.eventDate
        const currentTime = now.getTime()
        const eventTime = eventDate.getTime()

        const distance = eventTime - currentTime

        if (distance < 0) {
          // this.expired = true
          clearInterval(timer)
          this.loading = true
          this.items = [
            { day: 0, name: 'day' },
            { hour: 0, name: 'hour' },
            { minute: 0, name: 'minute' },
            { second: 0, name: 'second' }
          ]
          return (this.expired = true)
        }

        const day = Math.floor(distance / this._day)
        const hour = Math.floor((distance % this._day) / this._hour)
        const minute = Math.floor((distance % this._hour) / this._minute)
        const second = Math.floor((distance % this._minute) / this._second)

        this.items = [
          { day: this.addZero(day), name: 'day' },
          { hour: this.addZero(hour), name: 'hour' },
          { minute: this.addZero(minute), name: 'minute' },
          { second: this.addZero(second), name: 'second' }
        ]

        this.loading = true
        this.expired = false
      }, 1000)
    }
  },
  mounted: function () {
    this._showRemainingTime()
  }
}
</script>

<style scoped>
#main_com {
  text-align: center;
  margin-top: 2rem;
}

.timer_style {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}
.timer_style section > h1 {
  display: inline-block;
  font-size: 2rem;
  font-weight: 700;
  margin: 0 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  background-color: #f1f1f1;
}

.timer_style section > span {
  display: block;
  margin-top: 0.5rem;
  font-weight: 600;
}

.w_messsage section {
  margin-bottom: 2rem;
  font-size: 2rem;
  font-weight: 700;
  font-family: 'Roboto', sans-serif !important;
}
</style>
