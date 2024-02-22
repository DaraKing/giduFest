<template>
  <div class="countdown-timer">
    <div v-if="timeLeft" class="time">
      <span>{{ time.days }}</span> Days
      <span>{{ time.hours }}</span> Hours
      <span>{{ time.minutes }}</span> Minutes
      <span>{{ time.seconds }}</span> Seconds
    </div>
    <div v-else class="expired">
      Time's up!
    </div>
  </div>
</template>

<script>
export default {
  props: {
    targetDate: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      timeLeft: true,
      time: {
        days: 0,
        hours: 0,
        minutes: 0,
        seconds: 0
      },
      interval: null // Store the interval ID so it can be cleared later
    };
  },
  mounted() { // Use mounted for client-side only operations
    this.calculateTimeLeft();
    this.interval = setInterval(this.calculateTimeLeft, 1000);
  },
  beforeDestroy() { // Clear the interval when the component is destroyed
    if (this.interval) {
      clearInterval(this.interval);
    }
  },
  methods: {
    calculateTimeLeft() {
      const now = new Date();
      const target = new Date("2024-03-15T12:00:00");
      const difference = target - now;

      if (difference <= 0) {
        this.timeLeft = false;
        clearInterval(this.interval);
        return;
      }

      this.time.days = Math.floor(difference / (1000 * 60 * 60 * 24));
      this.time.hours = Math.floor((difference / (1000 * 60 * 60)) % 24);
      this.time.minutes = Math.floor((difference / 1000 / 60) % 60);
      this.time.seconds = Math.floor((difference / 1000) % 60);
    }
  }
};
</script>

<style scoped>
.countdown-timer {
  text-align: center;
  font-family: 'Arial', sans-serif;
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(to bottom, #FF0000, #FFFFFF, #0000FF);
}

.time > span {
  display: inline-block;
  margin: 0 10px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.2);
  color: #000;
  border-radius: 10px;
  font-size: 2em;
  font-weight: bold;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  min-width: 50px;
}

@media (max-width: 768px) {
  .time {
    display: flex;
    flex-direction: column;
  }

  .time > span {
    font-size: 2.5em; /* Smaller font size for smaller screens */
    padding: 25px; /* Adjust padding to fit smaller screens */
    margin: 15px; /* Reduce margin to utilize space efficiently */
  }
}

/* Further adjustments for very small screens, less than 480px wide */
@media (max-width: 480px) {
  .time > span {
    font-size: 2em; /* Even smaller font size for very small screens */
    padding: 20px; /* Further reduced padding */
    margin: 15px; /* Minimize margin */
  }

  /* Optionally, adjust other elements for small screens */
  .countdown-timer {
    padding: 10px; /* Add padding to the overall container for small screens */
  }
}

.expired {
  color: red;
  font-size: 1.5em;
}
</style>
