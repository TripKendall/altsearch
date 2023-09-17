<template>
    <div class="container">

        <div class="row my-5">
            <div class="col singleton border rounded shadow-lg">
                <div class="row g-0 border rounded overflow-hidden flex-md-row mb-4 shadow-sm h-md-250 position-relative">
                    <template v-if="this.ready">
                        <div class="col p-4 d-flex flex-column position-static">
                            <span class="d-inline-block mb-2 text-success-emphasis">
                                <span v-for="g in book.genres" :key="g.id">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                        class="bi bi-dot danger" viewBox="0 0 16 16">
                                        <path d="M8 9.5a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z" />
                                    </svg>
                                    Filed Under: <span class="success">{{ g.genre_name }}</span>
                                </span>
                            </span>
                            <h3 class="mb-0">
                                {{ book.title }}
                            </h3>
                            <div class="my-2 text-body-secondary">
                                <strong>Author:</strong> {{ book.author.author_name }} .
                                <strong>Published:</strong> {{ book.publication_year }}
                            </div>
                            <p class="mb-auto">
                                {{ book.description }}
                            </p>

                            <router-link class="nav-link active align-middle" to="/books">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                    class="bi bi-arrow-left-square-fill" viewBox="0 0 16 16">
                                    <path
                                        d="M16 14a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V2a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v12zm-4.5-6.5H5.707l2.147-2.146a.5.5 0 1 0-.708-.708l-3 3a.5.5 0 0 0 0 .708l3 3a.5.5 0 0 0 .708-.708L5.707 8.5H11.5a.5.5 0 0 0 0-1z" />
                                </svg>
                                Back
                                <svg class="bi">
                                    <use xlink:href="#chevron-left" />
                                </svg>
                            </router-link>
                        </div>
                    </template>
                    <p v-else>Loading...</p>
                    <div class="mr-2 pb-3">
                        <img v-if="this.ready" :src="`${this.imgPath}/${book.slug}.jpg`"
                            class="card-img-top img-fluid main-image" :alt="`cover for ${book.title}`">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { store } from './store.js'

export default {
    data() {
        return {
            store,
            ready: false,
            imgPath: 'http://localhost:8081/static',
            book: {}
        }
    },

    mounted() {
        const slug = this.$route.params.bookName
        const url = `http://localhost:8081/books/${slug}`
        fetch(url)
            .then(response => response.json())
            .then(data => {
                if (data.error) {
                    this.$emit('error', data.message);
                } else {
                    this.book = data.data;
                    this.ready = true;
                }
            })
            .catch(error => console.error(error));
    },
 

}

</script>