<template>
  <section class="burgare">

    <h3 v-bind:key=burger.name>{{burger.name}} <span class="thin">{{ burger.kCal }} kCal </span></h3>
  <img v-bind:src="burger.url" alt="Burgare" style="width: 400px">
  <h4>Inehåll:</h4>
  <ul>
    <li v-if="burger.lactose"> Innehåller <span class="varning"> Laktos</span></li>
    <li v-if="burger.gluten"> Innehåller <span class="varning"> Gluten</span></li>
  </ul>

    <div class="center">
      <button v-on:click ="remove">-</button>  {{amountOrdered}}

      <button v-on:click="add">+</button>

    </div>
  </section>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },

  data: function () {
    return {
      amountOrdered: 0,
    }
  },

  methods: {
    add: function (){
      this.amountOrdered++;
      this.$emit('orderedBurger',
          {name: this.burger.name,
           amount: this.amountOrdered
          }
          );
    },
    remove: function(){
      if(this.amountOrdered > 0 ){
        this.amountOrdered--;
      }
      this.$emit('orderedBurger',
          {name: this.burger.name,
        amount: this.amountOrdered
      },
      )

    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.burgare {

  color: #ffaf6e;
  background-color:#20124d;

  padding: 0 5px 20px 5px;
}
.varning{
  font-weight: bolder;
}
.thin{
  font-size: smaller;
  font-weight: lighter;
}
.center{
  margin: 0 5px 0 50px;
}
</style>
