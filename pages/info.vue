<!-- pages/index.vue -->
<template>
  <div class="container">
    <h3><ion-icon name="alarm-outline"></ion-icon>{{ items.length }}</h3>
    <div style="display: flex; justify-content: center">
      <div class="card" style="width: 600px">
        <div class="card-body">
          <div style="display: flex; justify-content: space-between">
            <h3>Text here</h3>

            <form class="d-flex mb-3" role="search">
              <input
                class="form-control me-2 inputText"
                type="search"
                placeholder="Search"
                aria-label="Search"
                @input="performSearch"
                v-model="searchQuery"
              />
            </form>
            <!-- <button class="btn btn-danger" @click="toggleValue">Click: {{ currentValue }}</button> -->
          </div>

          <div style="height: 200px">
            <table class="table table-bordered">
              <thead>
                <tr>
                  <th scope="col">First</th>
                  <th scope="col">Last</th>
                  <th scope="col">Handle</th>
                  <th class="text-center" scope="col" colspan="2">Option</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in displayedItems" :key="index">
                  <td>
                    <div class="long">{{ item.name }}</div>
                  </td>
                  <td>
                    <div class="long">{{ item.name }}</div>
                  </td>
                  <td>
                    <div class="long">{{ item.name }}</div>
                  </td>
                  <td>
                    <button class="btn btn-warning">
                      <Icon name="uil:pen" size="15" />
                      edit
                    </button>
                  </td>
                  <td>
                    <button class="btn btn-danger">
                      <Icon name="prime:trash" size="20" />
                      delete
                    </button>
                  </td>
                </tr>

                <tr v-if="totalPages === 0">
                  <td
                    class="text-center"
                    colspan="5"
                    style="color: rgb(122, 122, 122)"
                  >
                    Not found
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <nav aria-label="Page navigation example">
            <ul class="pagination justify-content-end mt-3">
              <li class="page-item disabled" v-if="currentPage === 1">
                <a class="page-link">Previous</a>
              </li>

              <li class="page-item" v-else>
                <a class="page-link" href="#" @click="prevPage">Previous</a>
              </li>

              <li class="page-item">
                <div class="page-link">{{ currentPage }}</div>
              </li>

              <li class="page-item disabled" v-if="currentPage === totalPages">
                <a class="page-link">Next</a>
              </li>

              <li class="page-item" v-else>
                <a class="page-link" href="#" @click="nextPage">Next</a>
              </li>
            </ul>
          </nav>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      value: 1,
      items: [],
      itemsPerPage: 3,
      currentPage: 1,
      searchQuery: "",
      script: [],
    };
  },
  computed: {
    currentValue() {
      return this.value;
    },

    filteredItems() {
      return this.items.filter((item) =>
        item.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    displayedItems() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.filteredItems.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.filteredItems.length / this.itemsPerPage);
    },
  },
  mounted() {
    axios
      .get("https://jsonplaceholder.typicode.com/users", {
        headers: {
          "ngrok-skip-browser-warning": true,
        },
      })
      .then((res) => {
        this.items = res.data;
      });
  },

  methods: {
    toggleValue() {
      this.value = this.value === 1 ? 0 : 1;
    },

    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    performSearch() {
      this.currentPage = 1; // Reset to the first page when searching
    },
  },
};
</script>

<style>
.inputText {
  width: 100px;
  transition: width 0.3s;
}

.inputText:hover {
  width: 250px;
  background-color: white;
}

::placeholder {
  color: aliceblue;
  opacity: 1;
}

.long {
  width: 100px;
  overflow: hidden; /* make sure it hides the content that overflows */
  white-space: nowrap; /* don't break the line */
  text-overflow: ellipsis; /* give the beautiful '...' effect */
}
</style>
