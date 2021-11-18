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
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
    </div>
    </section>
    <div id="map" v-on:click="addOrder">
      click here.
    </div>

    <section class="order">
      <h2>Kundinformation</h2>
      <p>Här anger du nödvändig information</p>
      <h3>Leveransinformation</h3>
      <p>
        <label for="name">Fullständigt namn</label><br>
        <input type="text" id="name" name="n" required="required" placeholder="För- och efter namn">
      </p>
      <p>
        <label for="email">Email</label><br>
        <input type="text" id="email" name="e" placeholder="Epostadress">
      </p>
      <p>
        <label for="adress">Adress</label><br>
        <input type="text" id="adress" name="g" placeholder="Gatunamn">
      </p>
      <p>
        <label for="adressnr"></label><br>
        <input type="number" id="adressnr" name="gnr" placeholder="Husnummer">
      </p>
      <p>
        <label for="betalning">Betalningsmetod</label>
        <select id="betalning" name="bet">
          <option>Swish</option>
          <option>Natura</option>
          <option>Kort</option>
          <option>Faktura</option>
          <option>Kontant</option>
        </select>
      </p>
      <h4>Kön</h4>
      <div>
        <input type="radio" id="man" name="kön" value="man">
        <label>Man</label>
      </div>
      <div>
        <input type="radio" id="kvinna" name="kön" value="kvinna">
        <label>Kvinna</label>
      </div>
      <div>
        <input type="radio" id="ib" name="kön" value="ickebinär"
               checked>
        <label>Ickebinär</label>
      </div>
      <div id="button">
        <button type="submit">
          <img src="https://wolfcoding.com/wp-content/uploads/2021/06/food-delivery.jpg" title="beställning" style="width: 40px;">
          Bekräfta beställning
        </button>
      </div>
    </section>
  </main>
  <hr>
  <footer>
    &copy; MAXade burgare
  </footer>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();


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




export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: hamburgers
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
  body {

    margin: 50px 50px 50px 50px;
    font-family: helvetica neue;
  }
  .head {
    overflow:hidden;
    margin: 50px 0px -60px 0px;
    padding: 10px 10px 60px 0px;

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

    padding: 10px 0px 450px 20px;
    border: 3px dashed #eaf90a;

  }
  .wrapper {
    display: grid;
    height: 100px;
    grid-template-columns: 33% 33% 33%;
    margin: 0px 0px 0px -20px;
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
  #ingrediens {
    font-family: helvetica neue;
    text-transform: uppercase;
  }
  .order {
    color: #20124d;
    background-color:#f2776e;
    margin: 10px 0px 0px 0px;
    padding: 10px 10px 0px 20px;
    border: 3px dashed #eaf90a;
  }
  #button{
    margin: 10px 20px 10px 10px;
  }
  button:hover {
    cursor: pointer;
    background-color:#eaf90a;
  }
</style>
