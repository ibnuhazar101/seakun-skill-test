<template>
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
    <div class="modal-detail hidden" ref="modal">
      <h1>Tes modal</h1>
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
  mounted() {
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
    },
    showModal() {
      document.querySelector(".modal-detail").classList.remove("hidden");
    }
  }
};
</script>

<style>
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
</style>
