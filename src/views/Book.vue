<template>
  <div class="book container mx-auto">
    <ApolloQuery :query="require('@/graphql/query/Book.gql')" :variables="{ id: $route.params.id}">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else class="flex mt-16 flex-col lg:flex-row">
          <div>
            <img :src="data.book.image" alt="book cover">
          </div>

          <div class="w-full lg:w-2/3 ml-0 mt-8 lg:mt-0 lg:ml-16">
            <div class="text-4xl font-bold">{{ data.book.title }}</div>
            <div class="text-2xl text-grey-800 mb-8">{{ data.book.author }}</div>
            <div class="text-lg text-grey-800 leading-normal">{{ data.book.description }}</div>
            <div class="my-12">
              <a :href="data.book.link" target="_blank" class="border border-purple-800 border-solid rounded text-purple-800 px-4 py-4 hover:bg-purple hover:text-black">View Link</a>
            </div>
            <router-link :to="`/books/${data.book.id}/edit`" href="#" class="">Edit</router-link>
            &middot;
            <a href="#" class="" @click.prevent="deleteBook">Delete</a>

          </div>
          <div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import DELETE_BOOK from '@/graphql/mutation/DeleteBook.gql';
export default {
  methods: {
    deleteBook() {
    this.$apollo.mutate({
      mutation: DELETE_BOOK,
      variables: {
        id: this.$route.params.id
      }
    }).then(data => {
      console.log(data)
      this.$router.push('/');
    }).catch(err => console.log(err));
  }
  }
  
}
</script>

<style lang="css" scoped>
.link-right {
  margin-right: 20px;
}
</style>