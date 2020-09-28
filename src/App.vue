<template>
  <div id="app" class="bg-blue-300 text-white min-h-screen">
    <!-- <div class="container mx-auto text-center text-6xl"> -->
    <div class="text-center text-6xl pt-12">
      <h1 class="font-serif">URL Shortener</h1>
    </div>
    <div class="py-10 text-center">
      <form @submit.prevent="createUrl">
        <div class>
          <label for="slug" class="block font-bold">Input slug</label>
          <input
            v-model="slug"
            type="text"
            name="slug"
            id="slug"
            class="px-4 py-2 rounded text-gray-800 sm:w-8/12 lg:w-5/12"
          />
        </div>
        <div class="mt-5">
          <label for="url" class="block font-bold">Input URL</label>
          <input
            v-model="url"
            type="text"
            name="url"
            id="url"
            required
            class="px-4 py-2 rounded text-gray-800 sm:w-8/12 lg:w-5/12"
          />
        </div>
        <button
          type="submit"
          class="my-5 bg-blue-500 py-2 px-4 rounded font-bold border-none hover:bg-blue-600"
        >Shorten</button>
      </form>
      <div
        v-if="shortedUrl"
        class="container mx-auto items-center py-10 min-h-60 bg-blue-500 text-center rounded"
      >
        <a :href="linkUrl" class="hover:text-gray-200 text-2xl">{{shortedUrl}}</a>
      </div>
      <div
        v-if="error"
        class="container mx-auto items-center py-10 min-h-60 bg-red-300 text-center rounded"
      >{{error}}</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = 'http://localhost:3000';

export default {
  name: 'App',
  data() {
    return {
      slug: '',
      url: '',
      shortedUrl: '',
      linkUrl: '',
      error: '',
    };
  },
  methods: {
    async createUrl() {
      this.error = '';
      this.shortedUrl = '';
      const config = {
        method: 'post',
        url: `${baseURL}/url`,
        headers: { 'Content-Type': 'application/json' },
        data: JSON.stringify({
          url: this.url,
          slug: this.slug || undefined,
        }),
      };
      axios(config)
        .then((response) => {
          this.shortedUrl = `${baseURL}/${response.data.slug}`;
          this.linkUrl = response.data.url;
          this.url = '';
          this.slug = '';
        })
        .catch((error) => {
          this.error = error.message;
        });
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
