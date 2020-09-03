<template>
  <div class="align-center justify-center d-flex flex-column">
    <h2>Weekly Motivation:</h2>
    <h2>{{ avg }}%</h2>

    <v-icon size="150" :color="currentMood.color">
      {{ currentMood.icon }}
    </v-icon>
  </div>
</template>

<script>
export default {
  name: 'MotivationDash',
  data() {
    return {
      db: this.$firebase.firestore(),
      avg: -1,
    };
  },
  computed: {
    currentMood() {
      const moods = [
        {
          color: 'red',
          icon: 'sentiment_dissatisfied',
          value: 0,
          maxValue: 25,
        },
        {
          color: 'orange',
          icon: 'sentiment_neutral',
          value: 50,
          maxValue: 75,
        },
        {
          color: 'green',
          icon: 'sentiment_satisfied',
          value: 100,
          maxValue: 100,
        },
      ];

      const mood = moods.find((item) => this.avg <= item.maxValue);

      return mood;
    },
  },
  methods: {
    updateAvg() {
      this.db.collection('votes').get()
        .then((querySnapshot) => {
          let avg = 0;
          let elementCount = 0;

          querySnapshot.forEach((doc) => {
            avg += doc.data().vote;
            elementCount += 1;
          });

          avg /= elementCount;

          this.avg = Math.floor(avg);
        });
    },
  },
  created() {
    this.updateAvg();
  },
};
</script>
