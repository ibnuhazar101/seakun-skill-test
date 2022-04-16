<template>
  <section>
    <div class="order-card" @click="showModal">
      <div class="card-header">
        <h4>Order Id: {{ order.orderId }}</h4>
        <h4>Created At: {{ dateToString(order.createdAt) }}</h4>
      </div>
      <div class="card-body">
        <div class="account-info">
          <p>Info Akun</p>
          <h4>{{ order.personalAccount.name }}</h4>
          <h4>{{ order.personalAccount.email }}</h4>
          <h4>{{ order.personalAccount.phonenumber }}</h4>
        </div>
        <div class="packet-info">
          <div class="packet-detail">
            <div class="packet-title">
              <p>Provider</p>
              <p>Paket</p>
              <p>Exp</p>
            </div>
            <div class="packet-desc">
              <h4>{{ order.provider }}</h4>
              <h4>{{ order.packet }}</h4>
              <h4>{{ dateToString(order.expired) }}</h4>
            </div>
          </div>
          <div class="packet-price">
            <h2>{{ setCurrency(order.payment.paymentTotal) }}</h2>
          </div>
        </div>
      </div>
    </div>

    <div v-show="isModalVisible" class="modal-detail">
      <div>
        <h1>Order Detail</h1>
        <div class="order-detail-card">
          <div class="card-header">
            <h4>Order Id: {{ order.orderId }}</h4>
            <h4>Created At: {{ dateToString(order.createdAt) }}</h4>
          </div>
          <div class="card-body">
            <div class="account-info">
              <p>Info Akun</p>
              <h4>{{ order.personalAccount.name }}</h4>
              <h4>{{ order.personalAccount.email }}</h4>
              <h4>{{ order.personalAccount.phonenumber }}</h4>
            </div>
            <div class="packet-info">
              <div class="packet-detail">
                <div class="packet-title">
                  <p>Provider</p>
                  <p>Paket</p>
                  <p>Exp</p>
                </div>
                <div class="packet-desc">
                  <h4>{{ order.provider }}</h4>
                  <h4>{{ order.packet }}</h4>
                  <h4>{{ dateToString(order.expired) }}</h4>
                </div>
              </div>
              <div class="packet-price">
                <h2>{{ setCurrency(order.payment.paymentTotal) }}</h2>
              </div>
            </div>
          </div>
        </div>

        <div class="payment-info">
          <div class="payment-title">
            <h4>Voucher Code</h4>
            <h4>Payment Status</h4>
            <h4>Payment Date</h4>
            <h4>Payment Bank</h4>
          </div>
          <div class="payment-detail">
            <h4>: {{ order.voucherCode }}</h4>
            <h4>: {{ order.payment.status }}</h4>
            <h4>: {{ dateToString(order.payment.paymentDate) }}</h4>
            <h4>: {{ order.payment.bank }}</h4>
          </div>
        </div>

        <button type="button" class="btn-green" @click="closeModal">
          Oke
        </button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      isModalVisible: false
    };
  },
  props: {
    order: Object
  },
  created() {
    console.log("created");
  },
  mounted() {
    console.log("mounted");
  },
  updated() {
    console.log("updated");
  },
  destroyed() {
    console.log("destroyed");
  },
  methods: {
    setCurrency(harga) {
      const price = harga.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
      return `Rp${price}`;
    },
    dateToString(value) {
      const newDate = new Date(value).toDateString();
      return newDate.slice(3, 15);
    },
    showModal() {
      this.isModalVisible = true;
      document.querySelector(".overlay").classList.remove("hidden");
    },
    closeModal() {
      this.isModalVisible = false;
      document.querySelector(".overlay").classList.add("hidden");
    }
  }
};
</script>

<style scoped>
.order-card {
  background-color: white;
  margin-bottom: 2rem;
  border: 1px solid teal;
  cursor: pointer;
  border-radius: 10px;
}

.order-card p,
.modal-detail p {
  color: teal;
  font-weight: bolder;
}

.card-header {
  display: flex;
  justify-content: space-between;
  padding: 1rem;
  border-bottom: 1px solid teal;
}

.card-body {
  display: flex;
  padding: 1rem;
}

.card-body h4,
.card-body p {
  margin-bottom: 0.5rem;
}

.account-info {
  width: 50%;
}

.packet-info {
  display: flex;
  width: 50%;
  padding: 0 1rem;
  border-left: 1px solid teal;
}

.packet-detail {
  display: flex;
  width: 50%;
  align-self: center;
}

.packet-title {
  width: 30%;
}

.packet-desc {
  width: 70%;
}

.packet-price {
  width: 50%;
  text-align: center;
  align-self: center;
}

.modal-detail {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 70%;
  background-color: #e3f5f2;
  padding: 1rem;
  border-radius: 10px;
  box-shadow: 0 3rem 5rem rgba(0, 0, 0, 0.3);
  z-index: 10;
}

.modal-detail h1 {
  margin-bottom: 1rem;
}

.order-detail-card {
  background-color: white;
  margin-bottom: 2rem;
  border: 1px solid teal;
}

.payment-info {
  background-color: white;
  margin-bottom: 2rem;
  display: flex;
  padding: 1.5rem 1rem;
  border: 1px solid teal;
}

.payment-info h4 {
  margin: 0.5rem;
}

.payment-title {
  width: 15%;
}

.btn-green {
  float: right;
  color: white;
  padding: 0.5rem 2rem;
  background: #4aae9b;
  border: 1px solid #4aae9b;
  border-radius: 5px;
  cursor: pointer;
}
</style>
