<template>
    <div class="container">
        <div class="row">
            <div class="col">
                <h1 class="mt-3">Admin</h1>
                <hr>
                <table v-if="this.ready" class="table table-striped table-compact">
                    <thead>
                        <tr>
                            <th>Title</th>
                            <th>User</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="b in this.books" v-bind:key="b.id">
                            <td>
                                <router-link :to="`/admin/books/${b.id}`">
                                    {{b.title}}
                                </router-link>
                            </td>
                            <td>{{b.author.author_name}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import Security from './security.js'

export default {
    name: 'BooksAdmin',

    data() {
        return {
            books: {},
            ready: false,
        }
    },
    mounted() { 
      Security.requireToken();
        fetch("http://localhost:8081/books")
            .then(response => response.json())
            .then(data => {
                if (data.error) {
                    this.$emit('error', data.error);
                } else { 
                this.books = data.data.books;
                this.ready = true;
                }
            })
            .catch(error => console.error(error));
    }

}
</script>