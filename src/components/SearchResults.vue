<template>
  <header class="ui">
    <div class="ui category search">
      <div class="ui icon input">
        <input
          class="prompt"
          type="text"
          placeholder="Search guides..."
          id="search-input"
          v-model="searchQuery"
          @input="SearchInput(), PaginationList()"
        />
        <i class="search icon"></i>
      </div>
      <div class="results"></div>
      <button class="ui button" @click="Search(), PaginationList()">
        Search
      </button>
    </div>
  </header>
  <section id="search-main" class="ui segment" v-if="searchPagesInit === 1">
    <ul id="search-results">
      <li
        v-for="item in searchResults[0]"
        :key="item.id"
        class="search-item ui card"
      >
        <span>
          <h2>{{ item.name }}</h2>

          <img :src="imgadress + item.thumbnailURL" alt="" />

          <p>{{ item.summary }}</p>

          <p>
            <em class="topics" v-for="i in item.topicNames" :key="i">
              {{ i }}</em
            >
          </p>
        </span>

        <footer>
          <div class="search-item-footer">
            <p class="date-field">
              <small> {{ item.publicationDate }}</small>
            </p>
            <a :href="item.fullURL"><i class="share alternate icon"></i></a>
          </div>
          <span class="author-field">
            <p>{{ item.FirstLastName }}</p>
            <p>
              <a :href="email">{{ item.authorEmail }}</a>
            </p>
          </span>
        </footer>
      </li>
    </ul>
    <footer id="pagination-footer" class="container">
      <button
        class="ui button"
        @click="
          (event) => {
            PageTurner(0), Search();
          }
        "
      >
        <i class="left chevron icon divider"></i>
      </button>
      <span class="pagination">
        <div v-for="item in currentPagingArray[0]" :key="item">
          <button
            :id="item"
            :value="item"
            class="ui button"
            @click="SearchPageSelect(item), PaginationList()"
          >
            {{ item }}
          </button>
        </div>
      </span>
      <button class="ui button" @click="PageTurner(1), Search()">
        <i class="right chevron icon divider"></i>
      </button>

      <!-- <pagination v-model="pageNr" :records="totalPages" :per-page="15" /> -->
      <!-- <pagination
      :totalPages="10"
      :perPage="10"
      :currentPage="currentPage"
      @pagechanged="onPageChange"
    /> -->
    </footer>
  </section>
</template>

<script>
// import Pagination from './Pagination.vue';

// import Pagination from 'v-pagination-3';

export default {
  name: "SearchResults",

  // components: {
  //   Pagination,
  // },
  data() {
    return {
      searchQuery: "",
      searchResults: [],
      urlBase: `https://support.infocaption.com/API/lucene/guidesearch?searchable=y&hitsPerPage=5&page=`,
      urlSearch: "&searchQuery=",
      email: "mailto:item.authorEmail",
      pageNr: 1,
      pageString: "1",
      totalPages: 0,
      totPagesArr: [],
      searchPagesInit: 0,
      currentPagingArray: [],
      imgadress: "https://support.infocaption.com/",
    };
  },

  methods: {
    onPageChange(page) {
      console.log(page);
      this.currentPage = page;
    },
    SearchInput() {
      console.log(document.getElementById("search-input").value);
      this.searchQuery = document.getElementById("search-input").value;
    },

    Search() {
      this.searchResults = [];
      this.pageString = this.pageNr.toString();
      fetch(this.urlBase + this.pageString + this.urlSearch + this.searchQuery)
        .then((res) => {
          return res.json();
        })
        .then((res) => {
          this.pageNr = res.currentPage;
          this.searchResults.unshift(res.results);

          this.totalPages = res.totalPages;
          //  console.log(res)
          //  console.log(this.totalPages)
        });

      this.searchPagesInit = 1;
      setTimeout(() => {
        this.PaginationList();
      }, 200);
    },

    SearchPageSelect(i) {
      console.log(i);

      this.searchResults = [];
      this.pageString = i.toString();
      fetch(this.urlBase + this.pageString + this.urlSearch + this.searchQuery)
        .then((res) => {
          return res.json();
        })
        .then((res) => {
          this.pageNr = res.currentPage;
          this.searchResults.unshift(res.results);

          this.totalPages = res.totalPages;
          //  console.log(res)
          //  console.log(this.totalPages)
        });

      setTimeout(() => {
        this.PaginationList();
      }, 200);
    },

    PaginationList() {
      while (this.totalPages > 0) {
        this.totPagesArr.push(this.totalPages);
        this.totalPages--;
        this.totPagesArr.sort(function (a, b) {
          return a - b;
        });
      }

      if (this.totPagesArr.length > 4) {
        this.currentPagingArray[0] = [1, 2, 3, 4, 5];
      }

      if (this.totPagesArr.length < 5) {
        this.currentPagingArray = this.totPagesArr;
      }

      console.log(this.totPagesArr);
    },

    PageTurner(direction) {
      switch (direction) {
        case 0:
          this.pageNr--;
          break;

        case 1:
          this.pageNr++;
          break;

        default:
          break;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header {
  width: 100vw;
  padding: 4rem;
  background-color: blanchedalmond;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 2rem;
}

#search-main h2,
#search-main input,
header .ui.category.search input,
#search-main button {
  margin: 1rem 0.2rem;
}
.ui.button,
#search-main button {
  background-color: rgb(255, 250, 245);
}

.ui.category.search {
  display: flex;
  flex-direction: column;
  align-items: center;
}

p {
  margin: 0.1rem;
  font-size: 1rem;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

img {
  width: 100%;
}

#search-main {
  display: flex;
  flex-direction: column;

  align-items: center;
  background-color: whitesmoke;
}

#search-results {
  display: flex;
  flex-flow: wrap;
    padding: 1rem;

}

.ui.card.search-item {
  margin: 1rem;
  padding: 1rem;
  border: 1px solid rgba(200, 200, 200, 0.5);
  border-radius: 5px;
  background-color: #fefefe;
  display: grid;

  grid-template-rows: 3fr 1fr;
  min-height: 70vh;
}

.search-item .author-field {
  display: flex;

  flex-direction: column;
  background-color: rgb(255, 250, 245);
  padding: 1rem;
  color: grey;
  margin: 0.5rem;
  border-radius: 5px;
}

.search-item .author-field p {
  margin: 0.4rem;
  font-size: 0.6rem;
}

.search-item .date-field {
  padding: 0.5rem;
  font-size: 1rem;
  color: grey;
}

.search-item-footer {
  display: flex;
  align-items: center;
  justify-content: space-around;
  align-self: flex-end;
}

.search-item-footer i {
  font-size: 1.5rem;
  color: blanchedalmond;
}

.pagination {
  display: flex;
}

.pagination button {
  border: 2px solid blanchedalmond;
  background-color: rgb(255, 250, 245);

  padding: 1rem;
  cursor: pointer;
}

#pagination-footer {
  margin: 0.5rem;
  display: flex;
}

#pagination-footer > button {
  border: 0;
  cursor: pointer;
}

#search-results .search-item .topics {
  color: rgb(114, 98, 98);
  display: flex;
  flex-direction: column;
  letter-spacing: 1.5px;
  font-size:0.7rem;
  line-height: 2;
  padding: 0.5rem;
  word-break: break-all;
  background: #fffbfb;
  margin: 1rem;
}
</style>
