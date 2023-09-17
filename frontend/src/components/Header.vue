<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">

    <a class="navbar-brand ml-2" href="/">
      <img alt="jTDataWorks" src="./../assets/images/logo.webp" class="img-fluid logo">
    </a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
      aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav mr-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
        </li>

        <li class="nav-item">
          <router-link class="nav-link active" to="/books">Latest</router-link>
        </li>

        <li v-if="store.token !== ''" class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navBarDropDown" role="button" data-bs-toggle="dropdown"
            aria-expanded="false">
            Admin
          </a>
          <ul class="dropdown-menu" aria-labelledby="navBarDropDown">
            <li>
              <router-link class="dropdown-item" to="/admin/users">Manage Users</router-link>
            </li>
            <li>
              <router-link class="dropdown-item" to="/admin/users/0">Add User</router-link>
            </li>
            <li>
              <router-link class="dropdown-item" to="/admin/books">Manage Books</router-link>
            </li>
            <li>
              <router-link class="dropdown-item" :to="{ name: 'BookEdit', params: { bookId: 0 } }">Add
                Book</router-link>
            </li>
          </ul>

        </li>

        <li class="nav-item">
          <router-link v-if="store.token == ''" class="nav-link" to="/login">Login</router-link>
          <a href="javascript:void(0);" v-else class="nav-link" @click="logout">
            Logout {{ store.user.first_name ?? '' }}
          </a>
        </li>
      </ul>

      <form class="form-inline my-lg-0">
        <input id="header-search" class="form-control mr-sm-2" type="text" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-info my-2 my-sm-0" type="submit">Search</button>
      </form>

    </div>

  </nav>
</template>

<script>
import { store } from './store.js'
import router from './../router/index.js'
import Security from './security'

export default {
  data() {
    return {
      store
    }
  },
  methods: {
    logout() {
      const payload = {
        token: store.token,
      }

      fetch("http://localhost:8081/users/logout", Security.requestOptions(payload))
        .then((response) => response.json())
        .then((response) => {
          if (response.error) {
            console.log(response.message);
          } else {
            store.token = "";
            store.user = {};

            document.cookie = '_site_data=; Path=/; ' +
              'SameSite=Strict; Secure; ' +
              'Expires=Thu, 01 Jan 1970 00:00:01 GMT;'

            router.push("/login");
          }
        })

    }
  }
}
</script>

<style scoped>
.logo {
  width: 36px;
  height: 36px;
}

#header-search {
  font-size: .7rem;
}
</style>