<template>
  <div id="orders">
    <div id="orderList">
      <div v-for="(order, key) in orders" v-bind:key="'order'+key">
        #{{ key }}: {{ order.orderItems }}<br>
                    {{order.details.Namn}}<br>
                    {{order.details.Email}}<br>
                    {{order.details.sex}}<br>
                    {{order.details.Betalmetod}}
      </div>
      <button v-on:click="clearQueue">Clear Queue</button>
    </div>
    <div id="dots">
        <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.loc.x + 'px', top: order.details.loc.y + 'px'}" v-bind:key="'dots' + key">
          {{ key }}
        </div>
    </div>
  </div>
</template>
<script>
import io from 'socket.io-client'
const socket = io();

export default {
  name: 'Dispatcher',
  data: function () {
    return {
      orders: null
    }
  },
  created: function () {
    socket.on('currentQueue', data =>
      this.orders = data.orders);
  },
  methods: {
    clearQueue: function () {
      socket.emit('clearQueue');
    }
  }
}
</script>
<style>
#orderList {
  top:1em;
  left:1em;
  position: absolute;
  z-index: 2;
  color:black;
  background: rgba(255,255,255, 0.9);
  padding: 1em;
}
#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background: url("https://i.pinimg.com/originals/ee/6c/95/ee6c95b85e6671453508336c8c5ff85f.jpg") no-repeat;
  width:1200px;
  height: 2750px;
  cursor: crosshair;
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
</style>
