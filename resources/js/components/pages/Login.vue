<template>
  <div id="login-bg" class="c-app flex-row align-items-center">
    <CContainer>
      <CRow class="justify-content-center">
        <CCol md="6">
          <CCard class="p-4 login-card">
            <CCardBody>
              <CForm class="text-center" @submit.prevent="postLogin">
                <CRow class="mb-4">
                  <CCol sm="4">
                      <CImg src="/assets/images/pesantren.png" :height="84"></CImg>
                  </CCol>
                  <CCol sm="6">
                    <h2 class="text-muted">LOGIN</h2>
                    <h4 class="custom-login">PESANTREN</h4>
                  </CCol>
                </CRow>
                <CInput
                  placeholder="Username"
                  autocomplete="username"
                  v-model="data.username"
                >
                  <template #prepend-content
                    ><CIcon name="cil-user"
                  /></template>
                </CInput>
                <CInput
                  placeholder="Password"
                  type="password"
                  autocomplete="curent-password"
                  v-model="data.password"
                >
                  <template #prepend-content
                    ><CIcon name="cil-lock-locked"
                  /></template>
                </CInput>
                <CRow>
                  <CCol class="text-center">
                    <CButton
                      block
                      color="primary"
                      class="px-4"
                      @click.prevent="postLogin"
                      type="submit"
                      >Login</CButton
                    >
                  </CCol>
                </CRow>
              </CForm>
            </CCardBody>
          </CCard>
        </CCol>
      </CRow>
    </CContainer>
  </div>
</template>

<style>
  .login-card {
    background: linear-gradient(145deg, #ffffff, #e6e6e6);
    box-shadow: 20px 20px 60px #d9d9d9, 
             -20px -20px 60px #ffffff;
  }
  #login-bg {
    background: url('/assets/images/ethnic-floral-seamless-pattern.jpg') repeat;
  }
</style>

<script>
import { mapActions, mapMutations, mapGetters, mapState } from "vuex";

export default {
  name: "Login",
  data() {
    return {
      data: {
        username: "",
        password: "",
      },
    };
  },
  created() {
    if (this.isAuth) {
      this.$router.push({
        name: "Home",
      });
    }
  },

  computed: {
    ...mapGetters(["isAuth"]),
    ...mapState(["errors"]),
  },
  methods: {
    ...mapActions("auth", ["submit"]),
    ...mapMutations(["CLEAR_ERRORS"]),
    async postLogin() {
      // this.isLoading = true;
    this.$store.commit('loadingOn')
    await  this.submit(this.data).then((res) => {
        if (this.isAuth) {
          this.$toasted.global.success_toast({
            message: 'Login Berhasil...'
          })
          this.CLEAR_ERRORS();
          
          setTimeout(() => {
          this.$store.commit('loadingOff')
          this.$router.push({
          name: "Home",
          });
          }, 2000)
        } else {
          this.$toasted.global.failed_toast({
            message: 'Gagal login'
          })
          this.$store.commit('loadingOff')
        }
      })
    }, 
  },
};
</script>
