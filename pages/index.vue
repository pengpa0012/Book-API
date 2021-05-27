<template>
  <div>
    <input type="search" v-model="inputValue" />
    <button @click="searchBook">Search</button>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      inputValue: ""
    };
  },
  methods: {
    searchBook() {
      const key = "AIzaSyBOL4MIwcR42oZwTaRvN6BXjIB1Memp4zo";

      fetch(
        `https://www.googleapis.com/books/v1/volumes?q=${this.inputValue}&key=${key}`
      )
        .then(res => res.json())
        .then(data => console.log(data))
        .catch(err => console.log(err));
    }
  }
};
</script>
