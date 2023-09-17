<template>
  <div>

    <hr>
    <div class="filters text-center">
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 0 }" v-on:click="setFilter(0)">ALL</span>
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 7 }" v-on:click="setFilter(7)">CLASSIC</span>
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 2 }" v-on:click="setFilter(2)">FANTASY</span>
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 6 }" v-on:click="setFilter(6)">HORROR</span>
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 4 }" v-on:click="setFilter(4)">THRILLER</span>
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 1 }" v-on:click="setFilter(1)">SCIENCE
        FICTION</span>
      <span class="filter me-1" v-bind:class="{ active: currentFilter === 3 }" v-on:click="setFilter(3)">TECH</span>
    </div>

    <hr>

      <div class="card-group px-5">

        <transition-group class="py-3 d-flex flex-wrap" tag="div" appear name="books">

          <div v-for="b in this.books" :key="b.id">
            <div class="card mx-1 ms-1 mb-3 trips" style="width: 14rem;"
              v-if="b.genre_ids.includes(currentFilter) || currentFilter === 0">

              <router-link :to="`/books/${b.slug}`">
                <img :src="`${this.imgPath}/${b.slug}.jpg`" class="card-img-top" :alt="`cover for ${b.title}`">
              </router-link>

              <div class="card-body text-center">
                <h6 class="card-title">{{ b.title }}</h6>
                <span class="book-author">{{ b.author.author_name }}</span><br>
                <small class="text-muted book-genre" v-for="(g, index) in b.genres" v-bind:key="g.id">
                  <em class="me-1">{{ g.genre_name }}<template v-if="index !== (b.genres.length - 1)">,</template></em>
                </small>
              </div>
            </div>
          </div>

        </transition-group>
      </div>
    </div>

 
</template>

<script>
import { store } from './store.js'
export default {
  name: 'Books',
  data() {
    return {
      store,
      ready: false,
      imgPath: 'http://localhost:8081/static',
      books: {},
      currentFilter: 0,
    }
  },
  emits: ['error'],

  beforeMount() {
    fetch("http://localhost:8081/books")
      .then((response) => response.json())
      .then((response) => {
        if (this.error) {
          this.$emit('error', response.message);
        } else {
          this.books = response.data.books;
          this.ready = true;
        }
      })
      .catch((error) => {
        this.$emit('error', error);
      });
  },
  methods: {
    setFilter: function (filter) {
      this.currentFilter = filter;
    }
  }
}


</script>

<style scoped>
.book-author,
.book-genre {
  font-size: 0.8rem;
}

.filter {
  padding: 3px 6px;
  cursor: pointer;
  border-radius: 6px;
  transition: all 0.33s ease-in-out;
  border: 1px solid #313131;
  margin: 0 6px;
}

.filter:hover {
  background-color: #3a82f5 !important;
  color: #fff;
}

.filter.active {
  background-color: #49a1ef !important;
  color: #fefefe;
}

.books-move {
  transition: all 0.5s ease-in-out;
}

.books-enter-active {
  transition: all 0.5s ease-in-out;
}

.books-leave-active {
  transition: all 0.5s ease-in;
}

.books-enter,
.books-leave-to {
  opacity: 0;
  /* transform: translateY(30px); */
}
</style>