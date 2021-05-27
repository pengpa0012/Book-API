<template>
  <v-app id="inspire">
    <v-app-bar app shrink-on-scroll class="d-flex justify-center text-center">
      <v-toolbar-title class="display-1 pt-5"
        ><span class="subtitle-1">Made by Godfrey Necesario</span><br />
        Google Book API</v-toolbar-title
      >
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row class="pt-10 px-4">
          <v-text-field
            v-model="inputValue"
            solo
            label="Search for books"
            clearable
            :append-outer-icon="'mdi-book-search'"
            @click:append-outer="searchBook"
          ></v-text-field>
        </v-row>
        <v-row cols="3" class="d-flex justify-center py-5">
          <v-card
            v-for="(book, index) in allBooks"
            :key="index"
            class="mx-2 my-3 align-start"
            max-width="300"
            min-width="300"
          >
            <v-img
              class="white--text align-end"
              height="300px"
              :src="book.volumeInfo.imageLinks.thumbnail"
              alt="poster"
            ></v-img>
            <div class="d-flex flex-column align-stretch">
              <v-card-title class="subtitle-2 pb-0 text-truncate">
                <div class="text-truncate">
                  {{ book.volumeInfo.title }}
                </div>
              </v-card-title>
              <v-card-text class="text--primary">
                <div class="caption">
                  Publisher:
                  {{
                    book.volumeInfo.publisher
                      ? book.volumeInfo.publisher
                      : "Unknown"
                  }}
                </div>

                <div class="text-truncate">
                  {{
                    book.volumeInfo.description
                      ? book.volumeInfo.description
                      : "No description attached"
                  }}
                </div>
              </v-card-text>
              <v-card-actions>
                <a :href="book.volumeInfo.infoLink" target="_blank">
                  <v-btn color="orange" text>
                    Visit Book
                  </v-btn>
                </a>
              </v-card-actions>
            </div>
          </v-card>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      inputValue: "",
      allBooks: []
    };
  },
  methods: {
    searchBook() {
      if (this.inputValue == "") {
        alert("Type something");
        return;
      }
      this.allBooks = [];

      const key = "AIzaSyBOL4MIwcR42oZwTaRvN6BXjIB1Memp4zo";

      fetch(
        `https://www.googleapis.com/books/v1/volumes?q=${this.inputValue}&key=${key}`
      )
        .then(res => res.json())
        .then(data => {
          if (data.totalItems <= 0) {
            alert("try other keywords");
            this.inputValue = "";
            return;
          }
          let allVerifiedBooks = data.items.filter(el => {
            return el.volumeInfo.imageLinks;
          });
          allVerifiedBooks.forEach(el => {
            this.allBooks.push(el);
          });
        })
        .catch(err => console.log(err));
    }
  },
  created() {
    const key = "AIzaSyBOL4MIwcR42oZwTaRvN6BXjIB1Memp4zo";

    fetch(`https://www.googleapis.com/books/v1/volumes?q=javascript&key=${key}`)
      .then(res => res.json())
      .then(data => {
        if (data.totalItems <= 0) {
          alert("try other keywords");
          this.inputValue = "";
          return;
        }
        data.items.forEach(el => {
          this.allBooks.push(el);
        });
      })
      .catch(err => console.log(err));
  }
};
</script>
