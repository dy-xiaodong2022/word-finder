<template>
  <nav id="topbar">
    <a-space size="middle">
      <span class="block"></span>
      <router-link to="/">Word Finder by dy-xiaodong2022</router-link>
      <div class="wd-icon blue">Beta</div>
      <router-link to="/">Home</router-link>
    </a-space>
    <a-space class="right" size="middle">
      <router-link to="/setting">Setting</router-link>
      <router-link to="/login" v-if="!isLogin">Login</router-link>
      <router-link to="/register" style="color: aqua" v-if="!isLogin">Register</router-link>
      <router-link to="/dashboard" v-if="isLogin">Dashboard</router-link>
      <span v-if="isLogin" @click="$refs.activate.open()">Activate</span><activate ref="activate"/>
      <router-link class="color-light" to="/dictionary" v-if="isLogin">Dictionary</router-link>
      <span @click="logout" v-if="isLogin">Logout</span>
      <span class="block"></span>
    </a-space>
  </nav>
</template>

<script>
import {mapState} from "vuex";
import {logout} from "@/api";
import cookie from "js-cookie";
import Activate from "@/components/activate.vue";
import {message} from "ant-design-vue";

export default {
  name: 'topbar',
  components: {Activate},
  computed: {
    ...mapState({
      isLogin: state => state.userdata.login
    })
  },
  methods: {
    logout() {
      let load = message.loading("Connecting to server...")
      logout().then(() => {
        load()
        cookie.remove("token")
        location.reload()
      }).catch((e) => {
        load()
        message.error("Network error")
      })
    }
  }
}
</script>

<style scoped lang="less">
#topbar {
  display: flex;
  width: 100%;
  padding: 10px;
  user-select: none;

  span {
    cursor: pointer;
  }

  > .right {
    margin-left: auto;
  }

  .block {
    width: 10px;
  }
}
</style>
