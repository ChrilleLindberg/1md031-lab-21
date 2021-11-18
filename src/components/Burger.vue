<template>
  <section class="burgare">

  <h3>{{burger.name}} {{ burger.kCal }}</h3>
  <img v-bind:src="burger.img" alt="Burgare" style="width: 400px">
  <h4>Inehåll:</h4>
  <ul>
    <li>{{burger.gluten}}</li>
    <li>{{burger.lactose}}</li>
  </ul>
  </section>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.burgare {

  color: #ffaf6e;
  background-color:#20124d;

  padding: 10px 10px 500px 20px;

}
.wrapper {
  display: grid;
  height: 100px;
  grid-template-columns: 33% 33% 33%;
}
.a {
  grid-column: 1 ;
}
.b {
  grid-column: 2;
}
.c {
  grid-column: 3;
}
</style>
