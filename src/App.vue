<template>
  <v-app>
    <div>
      <CompanyRegister/>
      <!-- <CompanyRegisterModal/> -->
    </div>
  </v-app>
</template>

<script>
import CompanyRegister from './components/CompanyRegister.vue';
// import CompanyRegisterModal from './components/CompanyRegisterModal.vue';
export default {
  name: 'App',
  components: {
    CompanyRegister,
    // // CompanyRegisterModal
  },
  data() {
  }
};
</script>
