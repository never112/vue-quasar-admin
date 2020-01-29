<template>
  <div class="index">
    <div id="indexLizi">
    </div>
    <div class="full-height row justify-center">
      <div class="column justify-center" style="width:300px">
        <div style="text-align:left">
          <!-- <q-field icon="account_circle" :error="$v.form.username.$error" error-label="Oops, we got an error.">
            <q-input v-model="form.username" :float-label="$t('Please input Username')" />
          </q-field> -->
          <q-input dark type="text" v-model="form.username" :float-label="$t('Username')" clearable :error="$t($v.form.username.$error)" :before="[{icon: 'account_circle'}]" autocomplete="new-password" />

          <q-input dark type="password" v-model="form.password" :float-label="$t('Password')" clearable :error="$t($v.form.password.$error)" :before="[{icon: 'vpn_key'}]" autocomplete="new-password" />
        </div>
        <div class="row justify-around q-mt-md">
          <q-btn color="primary" :loading="loading" @click="submit">
            {{$t('Login')}}
            <span slot="loading">
              <q-spinner-hourglass class="on-left" /> {{$t('Loading')}}...
            </span>
          </q-btn>
          <q-btn color="negative" @click="clear">{{$t('Reset')}}</q-btn>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import THREE from "../libs/three/three";
import { required } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      form: {
        username: "admin",
        password: "123"
      },
      loading: false
    };
  },
  validations: {
    form: {
      username: { required },
      password: { required }
    }
  },
  methods: {

    submit() {
      this.$v.form.$touch();
      if (this.$v.form.$error) {
        this.$q.notify(this.$t("Account password cannot be empty") + '!');
        return;
      }
      this.loading = true;
      this.$store
        .dispatch("LoginByUserName", this.form)
        .then(() => {
          if (this.interval) {
            clearInterval(this.interval);
            this.interval = null;
          }
          this.loading = false;
          this.$router.push({
            name: "home_index"
          });
        })
        .catch(ex => {
          this.loading = false;
        });
    },
    clear() {
      this.form.username = "";
      this.form.password = "";
      this.$v.form.$reset();
    }
  },
  computed: {},
  mounted() {
    
  },
  beforeDestroy() {
    if (this.interval) clearInterval(this.interval);
  }
};
</script>

<style scoped>
#login-app {
  background: none;
}
.index {
  width: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  text-align: center;
  background-color: #0557b6;
  background-repeat: no-repeat;
  background-size: cover;
  overflow: hidden;
}

.index h1 {
  height: 150px;
}

.index h1 img {
  height: 100%;
}

.index h2 {
  color: #666;
  margin-bottom: 200px;
}

.index h2 p {
  margin: 0 0 50px;
}

.index .ivu-row-flex {
  height: 100%;
}

#indexLizi {
  position: absolute;
  width: 100%;
  top: 0;
  bottom: 0;
  overflow: hidden;
}

p {
  color: white;
}
</style>
