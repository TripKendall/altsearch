<template>
  <Header />
  <div class="mb-5">
    <router-view 
      v-slot="{ Component }"
      :key="compentKey" 
      @success="success" 
      @error="error" 
      @warning="warning"
      @forceUpdate="forceUpdate">

      <keep-alive include="Books">
        <component :is="Component" />
      </keep-alive>  

    </router-view>

  </div>
  <Footer />
</template>

<script>
import Header from "./components/Header.vue"
import Footer from "./components/Footer.vue"
import { store } from './components/store.js'
import notie from 'notie'

const getCookie = (name) => {
  return document.cookie.split("; ").reduce((r, v) => {
    const parts = v.split("=");
    return parts[0] === name ? decodeURIComponent(parts[1]) : r;
  }, "");
}

export default {
  name: 'App',
  components: {
    Header,
    Footer,
  },
  data() {
    return {
      store,
      compentKey: 0,
    }
  },
  beforeMount() {
    // check for a cookie
    let data = getCookie("_site_data");

    if (data !== "") {
      let cookieData = JSON.parse(data);

      // update store
      store.token = cookieData.token.token;
      store.user = {
        id: cookieData.user.id,
        first_name: cookieData.user.first_name,
        last_name: cookieData.user.last_name,
        email: cookieData.user.email,
      }
    }
  },
  methods: {
    success(msg) {
      notie.alert({
        type: "success",
        text: msg,
      })
    },
    error(msg) {
      notie.alert({
        type: "error",
        text: msg,
      })
    },
    warning(msg) {
      notie.alert({
        type: "warning",
        text: msg,
      })
    },
    forceUpdate() {
      this.compentKey += 1;
    }
  }
}
</script>