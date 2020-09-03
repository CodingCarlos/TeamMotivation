<template>
  <div class="d-flex flex-column align-center">
    <p>How motivated are you right now?</p>

    <div class="motivation-selector d-flex">
      <v-btn
        v-for="(mood, i) in moods"
        :key="i"
        icon
        height="100"
        width="100"
        @click="setMoodValue(mood.value)"
      >
        <v-icon size="100" :color="mood.color">
          {{ mood.icon }}
        </v-icon>
      </v-btn>
    </div>

    <v-btn
      primary
      class="my-5"
      @click="vote()"
      :disabled="moodValue === -1"
    >
      Submit motivation
    </v-btn>
  </div>
</template>

<script>
// import firebase from 'firebase';

// const db = firebase.firestore();

export default {
  name: 'MotivationForm',
  data() {
    return {
      db: this.$firebase.firestore(),
      moods: [
        {
          color: 'red',
          icon: 'sentiment_dissatisfied',
          value: 0,
        },
        {
          color: 'orange',
          icon: 'sentiment_neutral',
          value: 50,
        },
        {
          color: 'green',
          icon: 'sentiment_satisfied',
          value: 100,
        },
      ],
      moodValue: -1,
    };
  },
  methods: {
    setMoodValue(val) {
      console.log(val);
      this.moodValue = val;
    },
    vote() {
      // La magia de guardar el value
      this.db.collection('votes').add({
        vote: this.moodValue,
        date: new Date(),
      })
        .then((docRef) => {
          this.moodValue = -1;
          console.log('Document written with ID: ', docRef.id);
        })
        .catch((error) => {
          console.error('Error adding document: ', error);
        });
    },
  },
};
</script>
