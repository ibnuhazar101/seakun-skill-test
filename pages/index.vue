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
            <select size="1" name="sort">
              <option value="" disabled selected hidden>Sort</option>
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
      dataOrder: []
    };
  },
  beforeMount() {
    this.getDataOrder();
  },
  methods: {
    getDataOrder() {
      axios
        .get("http://demo2687090.mockable.io/order")
        .then(res => {
          this.dataOrder = res.data;
          console.log(res.data);
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
