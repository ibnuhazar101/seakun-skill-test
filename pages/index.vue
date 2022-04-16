<template>
  <div class="main-page">
    <Navbar />
    <div class="order-page">
      <Sidebar />
      <div class="konten">
        <h1>Order List</h1>

        <div class="filter-order">
          <div class="search-order">
            <input
              v-model="searchKey"
              class="search-input"
              type="text"
              placeholder="Search by name/order-id/provider"
            />
            <input
              class="search-btn"
              type="submit"
              value="Submit"
              @click="getResults"
            />
          </div>

          <form>
            <select
              size="1"
              class="sort"
              v-model="selectedSort"
              @change="sortOption"
            >
              <option value="" disabled hidden>Sort order list</option>
              <option value="unsort">unsort</option>
              <option value="ascending">ascending</option>
              <option value="descending">descending</option>
            </select>
          </form>
        </div>

        <div v-for="orderList in dataOrder" :key="orderList.id">
          <OrderCard :order="orderList" />
        </div>

        <nav class="pagination" aria-label="Page navigation example">
          <ul class="page-list">
            <li class="page-item" @click="currentPage(page)">
              <a
                type="button"
                v-if="page > 1"
                class="page-link"
                @click="page--"
              >
                Previous
              </a>
            </li>
            <li
              v-for="pageNumber in pages"
              :key="pageNumber.id"
              class="page-item"
              @click="(page = pageNumber), openPage(page), currentPage(page)"
            >
              <a
                type="button"
                class="page-link current-link"
                v-bind:class="[
                  currentPages.includes(pageNumber) ? 'active' : ''
                ]"
              >
                {{ pageNumber }}
              </a>
            </li>
            <li class="page-item" @click="currentPage(page)">
              <a
                type="button"
                v-if="page < pages.length"
                @click="page++"
                class="page-link"
              >
                Next
              </a>
            </li>
          </ul>
        </nav>
      </div>
    </div>

    <div class="overlay hidden"></div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      searchKey: "",
      selectedSort: "",
      ascending: false,
      descending: false,
      dataOrder: [],
      page: 1,
      perPage: 5,
      totalPage: "",
      currentPages: [1],
      pages: []
    };
  },
  watch: {
    selectedSort() {
      this.sortOption();
      this.getDataOrder();
    },
    page() {
      this.getDataOrder();
      this.currentPage(this.page);
    },
    totalPage() {
      this.pagination();
    }
  },
  mounted() {
    this.getDataOrder();
  },
  methods: {
    getResults() {
      this.page = 1;
      this.selectedSort = "unsort";
      this.getDataOrder();
    },
    sortOption() {
      this.page = 1;
      if (this.selectedSort === "ascending") {
        this.ascending = true;
        this.descending = false;
      } else if (this.selectedSort === "descending") {
        this.ascending = false;
        this.descending = true;
      } else {
        this.ascending = false;
        this.descending = false;
      }
    },
    getDataOrder() {
      this.openPage(this.page);
      window.scrollTo({ top: 0, behavior: "smooth" });

      axios
        .get("https://demo3267455.mockable.io/order")
        .then(res => {
          this.dataOrder = res.data;

          if (this.searchKey.length > 0) {
            let searchLower = this.searchKey.toLowerCase();
            this.dataOrder = this.dataOrder.filter(order => {
              if (order.orderId.toLowerCase().includes(searchLower)) {
                return true;
              }
              if (order.provider.toLowerCase().includes(searchLower)) {
                return true;
              }
              if (
                order.personalAccount.name.toLowerCase().includes(searchLower)
              ) {
                return true;
              }
            });
          }

          const dateToNumber = function(value) {
            const newNum = value.slice(0, 10).replace(/-/g, "");
            return newNum;
          };

          if (this.ascending) {
            this.dataOrder = this.dataOrder.sort(
              (a, b) => dateToNumber(a.createdAt) - dateToNumber(b.createdAt)
            );
          } else if (this.descending) {
            this.dataOrder = this.dataOrder.sort(
              (a, b) => dateToNumber(b.createdAt) - dateToNumber(a.createdAt)
            );
          } else {
            this.dataOrder = this.dataOrder;
          }

          this.totalPage = this.dataOrder.length / this.perPage + 1;
          let from = this.page * this.perPage - this.perPage;
          let to = this.page * this.perPage;
          this.dataOrder = this.dataOrder.slice(from, to);
        })
        .catch(err => {
          console.log(err);
        });
    },
    pagination() {
      this.pages = [];
      for (let i = 1; i < this.totalPage; i++) {
        this.pages.push(i);
      }
    },
    openPage(page) {
      this.$router.push(`/?page=${page}`);
    },
    currentPage(page) {
      this.currentPages = [];
      this.currentPages.push(page);
    }
  }
};
</script>

<style>
.main-page {
  position: relative;
}

.order-page {
  background-color: #f5fffd;
  display: flex;
  min-height: 93vh;
}

.konten {
  position: relative;
  width: 80%;
  padding: 1rem 2rem;
}

.filter-order {
  display: flex;
  justify-content: space-between;
  padding: 2rem 0;
}

.search-order {
  display: flex;
}

input.search-input {
  width: 14rem;
  height: 2rem;
  padding: 0.5rem;
  margin-right: 0.2rem;
  border: 1px solid teal;
  border-radius: 5px;
}

input.search-btn {
  background: #4aae9b;
  color: white;
  height: 2rem;
  padding: 0.5rem;
  border: 1px solid #4aae9b;
  border-radius: 5px;
  cursor: pointer;
}

.sort {
  width: 8rem;
  height: 2rem;
  padding: 0.5rem;
  border: 1px solid teal;
  border-radius: 5px;
}

.hidden {
  display: none;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 5;
}

.pagination {
  float: right;
}

.page-list {
  display: flex;
}

nav ul {
  list-style-type: none;
}
.pagination a {
  text-decoration: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  margin: 0.25rem;
  border: 1px solid teal;
  border-radius: 5px;
}

a.page-link {
  display: inline-block;
  font-size: 20px;
  color: #4aae9b;
  font-weight: 500;
}

.active {
  background-color: #c9f1eb;
}
</style>
