<template>
  <div class="sidebar-box">
    <ul>
      <li>
        <a><h3>Order List</h3></a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {};
</script>

<style>
.sidebar-box {
  background-color: #4aae9b;
  width: 20%;
  padding: 0 auto;
}

.sidebar-box ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #4aae9b;
  border-bottom: 1px solid white;
}

li {
  float: center;
}

li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
</style>
