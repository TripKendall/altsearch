<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <h1 class="mt-4">Add/Edit Data</h1>
        <hr>

        <form-tag @bookEditEvent="submitHandler" name="bookForm" event="bookEditEvent">

          <div v-if="this.book.slug != ''" class="mb-3">
            <img :src="`${this.imgPath}/covers/${this.book.slug}.jpg`" class="img-fluid img-thumbnail book-cover"
              alt="cover">
          </div>

          <div class="mb-3">
            <label for="formFile" class="form-label">
              Main Image:
            </label>
            <input v-if="this.book.id === 0" ref="coverInput" class="form-control" type="file" id="formFile" required
              accept="image/jpeg" @change="loadCoverImage">
            <input v-else ref="coverInput" class="form-control" type="file" id="formFile" accept="image/jpeg"
              @change="loadCoverImage">
          </div>

          <text-input v-model="book.title" type="text" required="true" label="Title" :value="book.title"
            name="title"></text-input>

          <select-input name="author-id" v-model="this.book.author_id" :items="this.authors" required="required"
            label="Author">
          </select-input>

          <text-input v-model="book.publication_year" type="number" required="true" label="Publication Year"
            :value="book.publication_year" name="publication-year"></text-input>

          <div class="mb-3">
            <label for="description" class="form-label">Description</label>
            <textarea required v-model="book.description" class="form-control" id="description" rows="3"></textarea>
          </div>

          <div class="mb-3">
            <label for="genres" class="form-label block w-100">Genres</label>
            <select ref="genres" id="genres" class="form-select dark" required size="7" v-model="this.book.genre_ids"
              multiple>
              <option v-for="g in this.genres" :value="g.value" :key="g.value">
                {{ g.text }}
              </option>
            </select>
          </div>

          <hr>

          <div class="float-start">
            <input type="submit" class="btn btn-outline-primary mr-2" value="Save" />
            <router-link to="/admin/books" class="btn btn-outline-secondary">Cancel</router-link>
          </div>
          <div class="float-end">
            <a v-if="this.book.id > 0" class="btn btn-danger" href="javascript:void(0);"
              @click="confirmDelete(this.book.id)">
              Delete
            </a>
          </div>

        </form-tag>
      </div>
    </div>
  </div>
</template>

<script>
import Security from './security.js'
import FormTag from './forms/FormTag.vue'
import TextInput from './forms/TextInput.vue'
import SelectInput from './forms/SelectInput.vue'
import router from '../router/index.js'
import notie from 'notie'

export default {
  name: "BookEdit",
  beforeMount() {
    Security.requireToken();
    // get the book id from the route
    if (this.$route.params.bookId > 0) {
      // edit mode

    } else {
      // add mode

    }

    // get authors
    fetch('http://localhost:8081/admin/authors/all', Security.requestOptions(""))
      .then(response => response.json())
      .then(data => {
        if (data.error) {
          this.$emit('error', data.message)
        } else {
          this.authors = data.data;
        }
      })
      .catch((error) => {
        this.$emit('error', error)
      });


  },
  components: {
    'form-tag': FormTag,
    'text-input': TextInput,
    'select-input': SelectInput,
  },
  data() {
    return {
      book: {
        id: 0,
        title: "",
        author_id: 0,
        publication_year: null,
        description: "",
        cover: "",
        slug: "",
        genres: [],
        genre_ids: [],
      },
      authors: [],
      imgPath: 'http://localhost:8081/static',

      // this should be a db call?...
      genres: [
        { value: 1, text: "Science Fiction" },
        { value: 2, text: "Fantasy" },
        { value: 3, text: "Tech" },
        { value: 4, text: "Thriller" },
        { value: 5, text: "Mystery" },
        { value: 6, text: "Horror" },
        { value: 7, text: "Classic" },
      ]
    }
  },
  methods: {
    submitHandler() {
      const payload = {
        id: this.book.id,
        title: this.book.title,
        author_id: parseInt(this.book.author_id, 10),
        publication_year: parseInt(this.book.publication_year, 10),
        description: this.book.description,
        cover: this.book.cover,
        slug: this.book.slug,
        genre_ids: this.book.genre_ids,
      }

      // console.log(payload);

      fetch('http://localhost:8081/admin/books/save', Security.requestOptions(payload))
        .then(response => response.json())
        .then(data => {
          if (data.error) {
            this.$emit('error', data.message)
          } else {
            this.$emit('success', 'changes saved')
            router.push('/admin/books')
          }
        })
        .catch((error) => {
          this.$emit('error', error)
        });

    },

    loadCoverImage() {
      const file = this.$refs.coverInput.files[0];
      const reader = new FileReader();
      // reader.addEventListener('load', (event) => {
      //   this.book.cover = event.target.result;
      // });
      // reader.readAsDataURL(file);
      reader.onloadend = () => {
        const base64String = reader.result.replace('data:', '').replace(/^.+,/, '');
        this.book.cover = base64String;
        // alert(this.book.cover);
      }
      reader.readAsDataURL(file);
    },

    confirmDelete(id) {
      notie.confirm({
        text: "Are you sure you want to delete this book?",
        submitText: "Delete",
        submitCallback: () => {
          let payload = {
            id: id,
          }

          fetch(`http://localhost:8081/admin/books/delete`, Security.requestOptions(payload))
            .then((response) => response.json())
            .then((data) => {
              if (data.error) {
                this.$emit('error', data.message);
              } else {
                this.$emit('success', "Book deleted");
                router.push("/admin/books");
              }
            })
        }
      })
    }
  }
}
</script>

<style scoped>
#genres {
  background-color: #111;
  padding: 1rem;
  color: #fefefe;
  border: 5px solid #2c313a !important;
  border-radius: 0.5rem;
}

#formFile{ 
  background-color: #111;
  padding: 1rem 0 2rem 1rem;
  color: #fefefe;
  border: 5px solid #2c313a !important;
  border-radius: 0.5rem;
  min-height: 70px;
}

input[type=file]::file-selector-button {
  border: 2px solid #7856ff;
  padding: .1em .4em;
  border-radius: .2em;
  background-color: #7856ff;
  transition: 1s;
  color: #fff!important;
  margin-right: 1rem;
}

input[type=file]::file-selector-button:hover {
  background-color: #81ecec;
  border: 2px solid #00cec9;
}
</style>