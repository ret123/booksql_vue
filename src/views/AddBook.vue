
<template>
  <div class="create container mx-auto mt-12">
    <h1 class="mb-4">Create Book</h1>
    <form action="#" method="POST" @submit.prevent="addBook">
      <div class="form-group">
        <label class="font-bold mb-2" for="title">Title</label>
        <input type="text" name="title" id="title" v-model="title" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="author">Author</label>
        <input type="text" name="author" id="author" v-model="author" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="image">Image</label>
        <input type="text" name="image" id="image" v-model="image" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="description">Description</label>
        <textarea name="description" id="description" cols="30" rows="10" v-model="description"></textarea>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link" />
      </div>
      <div class="form-group">
        <label class="font-bold mb-2">
          <input type="checkbox" name="featured" v-model="featured" class="mr-2" />Featured
        </label>
      </div>
      <div class="form-group">
        <ApolloQuery :query="require('@/graphql/query/Categories.gql')">
          <template slot-scope="{ result: { data, loading }, isLoading }">
            <div v-if="isLoading">Loading...</div>
            <select v-else v-model="category">
              <option
                v-for="category of data.categories"
                :key="category.id"
                :value="category.id"
              >{{ category.name }}</option>
            </select>
          </template>
        </ApolloQuery>
      </div>

      <div class="form-group">
        <button type="submit">Add book</button>
      </div>
    </form>
  </div>
</template>
<script>
import ADD_BOOK from "@/graphql/mutation/AddBook.gql";
import BOOKS_QUERY from "../graphql/query/Books.gql";

export default {
  data() {
    return {
      title: "",
      author: "",
      image: "",
      description: "",
      link: "",
      featured: false,
      category: 1,
     
    };
  },
  methods: {
    addBook() {
      this.$apollo
        .mutate({
          // Query
          mutation: ADD_BOOK,
          // Parameters
          variables: {
            title: this.title,
            author: this.author,
            image: this.image,
            description: this.description,
            link: this.link,
            featured: this.featured,
            category: this.category
          },
           update: (store, { data: { createBook } }) => {
        // Read the data from our cache for this query.
        const data = store.readQuery({ query: BOOKS_QUERY })
        
        // Add our tag from the mutation to the end
        data.books.push(createBook)
         
        // Write our data back to the cache.
        store.writeQuery({ query: BOOKS_QUERY, data })
         
        
      },
        })
        .then(data => {
          // Result
          
          console.log(data);
          this.$router.push('/');
        })
        .catch(error => {
          // Error
          console.error(error);
          // We restore the initial user input
         
        });
    }
  }
};
</script>

<style scoped>
  .form-group {
    margin-bottom: 32px;
  }
  input[type="text"], textarea {
    padding: 10px 14px;
    border: 1px solid lightgray;
    border-radius: 5px;
  }
  label {
    display: block;
  }
  button {
    padding: 16px;
    background: #027BFF;
    color: white;
    border-radius: 5px;
    font-size: 16px;
  }
</style>