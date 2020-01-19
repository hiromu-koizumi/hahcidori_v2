<template>
  <div class="login-view">
    <h1>hachidoriV2</h1>
    <!-- <HdLoginForm :onlogin="handleLogin" /> -->
    <HdLoginForm />
  </div>
</template>
<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";
import HdLoginForm from "@/components/molecules/HdLoginForm.vue";
@Component({
  components: {
    HdLoginForm
  }
})
export default class HdLoginView extends Vue {
  // public handleLogin(authInfo) {
  //   return this.$store
  //     .dispatch("login", authInfo)
  //     .then(() => {
  //       this.$router.push({ path: "/" });
  //     })
  //     .catch(err => this.throwReject(err));
  // }
  // throwReject(err) {
  //   return Promise.reject(err);
  // }
}
</script>

<style scoped>
.login-view {
  width: 320px;
  margin: auto;
}
</style>
