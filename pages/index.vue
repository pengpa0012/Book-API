<template>
  <v-app id="inspire">
    <v-app-bar
      app
      shrink-on-scroll
      class="d-flex justify-center text-center blue white--text"
    >
      <v-toolbar-title class="display-1"
        ><span class="subtitle-1">Made by Godfrey Necesario</span><br />
        Google Book API</v-toolbar-title
      >
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row class="pt-5">
          <v-col xl="8" class="mx-auto">
            <v-text-field
              v-model="inputValue"
              solo
              label="Search for books"
              clearable
              :append-outer-icon="'mdi-book-search'"
              @click:append-outer="searchBook"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row cols="4" class="py-5">
          <v-col xl="8" class="d-flex justify-center flex-wrap mx-auto">
            <div class="lds-ellipsis loader" v-if="showLoader">
              <div></div>
              <div></div>
              <div></div>
              <div></div>
            </div>
            <v-card
              v-for="(book, index) in allBooks"
              :key="index"
              class="mx-2 my-3 align-start"
              max-width="250"
              min-width="250"
            >
              <v-img
                class="white--text align-end"
                height="200px"
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
                  <div class="caption">
                    Page Count:
                    {{ book.volumeInfo.pageCount }}
                  </div>
                  <div class="caption">
                    Published Date:
                    {{ new Date(book.volumeInfo.publishedDate).getFullYear() }}
                  </div>
                </v-card-text>
                <v-card-actions class="pl-4 pb-5">
                  <a :href="book.volumeInfo.infoLink" target="_blank">
                    <v-btn depressed color="yellow darken-1">
                      Visit Book
                    </v-btn>
                  </a>
                </v-card-actions>
              </div>
            </v-card>
          </v-col>
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
      allBooks: [],
      showLoader: true
    };
  },
  methods: {
    searchBook() {
      if (this.inputValue == "") {
        alert("Type something");
        return;
      }
      this.showLoader = true;
      this.allBooks = [];

      const key = "AIzaSyBOL4MIwcR42oZwTaRvN6BXjIB1Memp4zo";

      fetch(
        `https://www.googleapis.com/books/v1/volumes?q=${this.inputValue}&key=${key}`
      )
        .then(res => res.json())
        .then(data => {
          this.showLoader = false;
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
        this.showLoader = false;
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

<style scoped>
.lds-ellipsis {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ellipsis div {
  position: absolute;
  top: 33px;
  width: 13px;
  height: 13px;
  border-radius: 50%;
  background: #2196f3;
  animation-timing-function: cubic-bezier(0, 1, 1, 0);
}
.lds-ellipsis div:nth-child(1) {
  left: 8px;
  animation: lds-ellipsis1 0.6s infinite;
}
.lds-ellipsis div:nth-child(2) {
  left: 8px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(3) {
  left: 32px;
  animation: lds-ellipsis2 0.6s infinite;
}
.lds-ellipsis div:nth-child(4) {
  left: 56px;
  animation: lds-ellipsis3 0.6s infinite;
}
@keyframes lds-ellipsis1 {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes lds-ellipsis3 {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
@keyframes lds-ellipsis2 {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(24px, 0);
  }
}
</style>
