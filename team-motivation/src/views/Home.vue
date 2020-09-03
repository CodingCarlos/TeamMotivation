<template>
  <div class="home">
    <MotivationDash />
    <MotivationForm />
  </div>
</template>

<style scoped>
  .align-space-around {
    align-items: space-around;
  }
</style>

<script>
// @ is an alias to /src
import MotivationDash from '@/components/MotivationDash.vue';
import MotivationForm from '@/components/MotivationForm.vue';

export default {
  name: 'Home',
  components: {
    MotivationDash,
    MotivationForm,
  },
};
</script>
