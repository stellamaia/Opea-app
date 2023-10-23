<template>
  <v-app>
    <div>
      <CompanyRegister/>
    </div>
  </v-app>
</template>

<script>
import CompanyRegister from './components/CompanyRegister.vue';
export default {
  name: 'App',
  components: {
    CompanyRegister,
  },
  data() {
  }
};
</script>
