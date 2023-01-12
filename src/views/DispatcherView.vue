<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">
          #{{key}}: {{ order.orderItems }} 
          <dd>{{order.customerInformation}}</dd>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{ key }}
          </div>
      </div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();

  export default {
    name: 'DispatcherView',
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
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    width: 1400px;
    height: 1000px;
    background: url("https://valresultat.svt.se/2022/map/images/03800250-valdistrikt-834x585.jpeg");
    position:absolute;
    background-repeat: no-repeat;
    background-size: contain;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 50px;
    width:50px;
    height:20px;
    text-align: center;
  }
  </style>
  