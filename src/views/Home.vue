<template>
  <header>
    <div class="head">
      <img id="pic" src="https://www.damhert.be/content/recipes/271/BURGERWebsite_Hoofding.jpg?v=1" alt="header" title="Header">
      <div class="center"><h1>Välkommen till MAXade burgare</h1></div>
    </div>
  </header>
  <main>
    <section class="burgarmeny">
      <h2>Välj Burgare</h2>
      <p>Här väljer du burgare</p>



    <div class="wrapper">
    <Burger class="box" v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>

    </div>
    </section>

    <section class="order">
      <h2>Kundinformation</h2>
      <p>Här anger du nödvändig information</p>
      <h3>Leveransinformation</h3>

      {{orderedBurgers}}
      <p>
        <label for="name">Fullständigt namn</label><br>
        <input type="text" id="name" v-model="fullname" required="required" placeholder="För- och efter namn">
      </p>
      <p>
        <label for="email">Email</label><br>
        <input type="email" id="email" v-model="email" required="required" placeholder="Epostadress">
      </p>

      <h3>Välj utkörningsadress</h3>
      <p> Markera på önskad plats på kartan</p>


      <div id = "mapbox">

        <div id="map" v-on:click="setLocation">
          <div v-bind:style="{left: location.x + 'px',top: location.y + 'px',}">T
          </div>
        </div>
      </div>

      <p>
        <label for="betalningmet">Betalningsmetod</label><br>
        <select id="betalningmet" v-model="betalmetod">
          <option>Swish</option>
          <option>Kort</option>
          <option>Faktura</option>
          <option selected="selected">Kontant</option>
        </select>
      </p>


      <h4>Kön</h4>
      <div>
        <input type="radio" id="man" v-model="valt" value="man">
        <label>Man</label>
      </div>
      <div>
        <input type="radio" id="kvinna" v-model="valt" value="kvinna">
        <label>Kvinna</label>
      </div>
      <div>
        <input type="radio" id="ib" v-model="valt" value="ickebinär"
               checked>
        <label>Ickebinär</label>
      </div>


      <div id="button">
        <button v-on:click="markDone" >
          <img src="https://e7.pngegg.com/pngimages/936/444/png-clipart-computer-icons-icon-design-order-icon-cdr-angle.png" title="beställning" style="width: 40px;" alt="Beställning">
          Bekräfta beställning
        </button>
      </div>
    </section>
  </main>
  <hr>
  <footer>
    &copy; MAXade burgare Christoffer Lindberg
  </footer>


</template>

<script>

import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();
/*
 function MenuItem (burgerName, calories, yourURL, gluteboolean, laktosboolean) {
   this.name=burgerName;
   this.kCal=calories;
   this.img=yourURL;
   this.gluten=gluteboolean;
   this.lactose=laktosboolean;

}
const Burger1 = new MenuItem("Orginalburgare", 295, "https://www.max.se/contentassets/7283b5a0bf324ba99d7fa7e0caf5ae7f/product_max-original.png", "false", "false");
const Burger2 = new MenuItem("Friscoburgare", 605, "https://www.max.se/contentassets/093fc9201de04f648ea20afee581bafe/product_frisco-cheese-n-bacon.png", "true", "false");
const Burger3 = new MenuItem("Dubbel Friscoburgare", 872, "https://www.max.se/contentassets/80851e9c275642b3834b216a880941ff/product_dubbel-friscoburgare.png", "true", "true");
const hamburgers= [ Burger1, Burger2, Burger3 ]
console.log(hamburgers)
*/



export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
            burgers: menu.menu,
      orderedBurgers:'',
       location: {x:0,
                  y:0},
           fullname:'',
              email:'',
         betalmetod:'Swish',
                Kön:''
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100);
    },

    addToOrder: function (event) {
     return this.orderedBurgers[event.name] = event.amount;
     /* console.log("event.amount: " + event.amount);*/
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,};
      this.location = {x: event.clientX - 10 - offset.x, //only storing the location of click
        y: event.clientY - 10 - offset.y,};
    },

    markDone:function() {

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details:{loc: this.location,
                Namn: this.fullname,
               Email: this.email,
          Betalmetod: this.betalmetod,
                 Kön: this.valt},
          orderItems: this.orderedBurgers
      });

      alert("Ordern har skickats")

    },/*
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y
        },
        orderItems: ["Beans", "Curry"]*/

  }
}
</script>

<style>
#mapbox{

  overflow:scroll;
  width: 1200px;
  height: 400px;
  border-style: solid; border-color: #eaf90a; border-width:7px;
}
#map {
  width: 1200px;
  height: 2750px;
  position: relative;
  background: url("https://i.pinimg.com/originals/ee/6c/95/ee6c95b85e6671453508336c8c5ff85f.jpg");
}
#map>div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}

  body {

    margin: 50px 50px 50px 50px;
    font-family: helvetica neue,serif;
  }
  .head {
    overflow:hidden;
    margin: 50px 0 -60px 0;
    padding: 10px 10px 60px 0;

  }
  #pic{
    overflow:hidden;
    opacity: 60%;
    width: 1269px;
    height: 250px;
  }
  .center {
    position: absolute;
    padding: 30px 90px 30px 90px;
    text-align: center;
    font-size: 36px;
    margin-top: -250px;
  }
  .burgarmeny {

    color: #ffaf6e;
    background-color:#20124d;

    padding: 10px 0 550px 20px;
    border: 3px dashed #eaf90a;

  }
  .wrapper {
    display: grid;
    height: 100px;
    grid-template-columns: 33% 33% 33%;

  }
  /*
  .a {
    grid-column: 1 ;
  }
  .b {
    grid-column: 2;
  }
  .c {
    grid-column: 3;
  }
   */
  .order {
    color: #20124d;
    background-color:#f2776e;
    margin: 10px 0 0 0;
    padding: 10px 10px 0 20px;
    border: 3px dashed #eaf90a;
  }
  #button{
    margin: 10px 20px 10px 10px;
  }
  button:hover {
    cursor: pointer;
    background-color:#eaf90a;
  }
#name{
  width: 12rem; height: 2rem;
}
#email{
  width: 12rem; height: 2rem;
}
#betalningmet{
  width: 12rem; height: 2rem;
}
#kvinna{
  border-radius: 15px;
  width:20px;
  height:20px;

}
#man{
  border-radius: 15px;
  width:20px;
  height:20px;
}
#ib{
  border-radius: 15px;
  width:20px;
  height:20px;
}
</style>
