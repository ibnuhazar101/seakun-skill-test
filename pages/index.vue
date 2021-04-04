<template>
  <div class="main-page">
    <Navbar />
    <div class="order-page">
      <Sidebar />
      <div class="konten">
        <h1>Order List</h1>

        <div class="filter-order">
          <div class="search-order">
            <input type="text" placeholder="Search by name" />
            <input type="submit" value="Submit" />
          </div>

          <form>
            <select
              size="1"
              name="sort"
              id="sort-option"
              v-model="selectedSort"
              @change="sortOption()"
            >
              <option value="" disabled>Sort</option>
              <option value="ascending">ascending</option>
              <option value="descending">descending</option>
            </select>
          </form>
        </div>

        <div v-for="orderList in dataOrder" :key="orderList.id">
          <OrderCard :order="orderList" />
        </div>
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
      selectedSort: "",
      ascending: false,
      descending: false,
      dataOrder: []
    };
  },
  watch: {
    selectedSort() {
      this.getDataOrder();
    }
  },
  mounted() {
    this.getDataOrder();
  },
  methods: {
    sortOption() {
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
      axios
        .get("http://demo2687090.mockable.io/order")
        .then(res => {
          const dateToNumber = function(value) {
            const newNum = value.slice(0, 10).replace(/-/g, "");
            return newNum;
          };
          if (this.ascending) {
            this.dataOrder = res.data.sort(
              (a, b) => dateToNumber(a.createdAt) - dateToNumber(b.createdAt)
            );
          } else if (this.descending) {
            this.dataOrder = res.data.sort(
              (a, b) => dateToNumber(b.createdAt) - dateToNumber(a.createdAt)
            );
          } else {
            this.dataOrder = res.data;
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

<style>
.main-page {
  position: relative;
}

.order-page {
  display: flex;
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
</style>
