<template>

  <div id="header">
    <img src="https://kidsartprojects101.com/wp-content/uploads/2019/09/abstract-2468874-1600x533.jpg" id="headerImage">
    <h1 id="header_text">Welcome to Classic Fantastic</h1>
  </div>
  <div >
    <h2>Burgermenu</h2>
    <div class="wrapperBurger">
      <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"
              v-on:orderedBurgers="addToOrder($event)"/>
    </div>
  </div>
  <h2> Customer information </h2>
  This is where you provide necessary information

  <h3> Delivery information </h3>

  <div id="backgroundForm">

    <form>
      <p>
        <label for="fullname"> Full name </label><br>
        <input type="text" id="fullname" v-model="fn" required="required" placeholder="First - and Last name">
      </p>
      <p>
        <label for="emailadress"> E-mail </label><br>
        <input type="email" id="emailadress" v-model="en" required="required" placeholder="E-mail address ">
      </p>


      <div id="v-model-select" class="payment">
        <label> Payment method </label>
        <select v-model="selected" >
          <option> Swish </option>
          <option> Creditcard </option>
          <option selected="selected"> Klarna </option>

        </select>
      </div>

      <br>

      <legend> Gender </legend>

      <input type="radio" id="male" v-model="gender" value="Male">
      <label for="male"> Male </label><br>
      <input type="radio" id="female" v-model="gender" value="Female">
      <label for="female"> Female </label><br>
      <input checked="checked" type="radio" id="nogender" v-model="gender" value="Do not wish to provide">
      <label for="nogender"> Do not wish to provide  </label><br>
    </form>

    <div class="scrollMap">
      <div id="map" v-on:click="setLocation" >
        <div id="dots">
          <div  v-bind:style="{ left: location.x +'px', top: location.y +'px'}"  >

            T
          </div>
        </div>

      </div>
    </div>
  </div>




  <button v-on:click="submitOrder(key)">
    <img src="https://media.istockphoto.com/vectors/green-check-mark-icon-tick-symbol-tick-icon-vector-illustration-vector-id1342603245?b=1&k=20&m=1342603245&s=170667a&w=0&h=fE035vYyGF6_AS5Q2y1Fc0vBs5G56YMo680yGDGKat8=" width="50px">
    Send info
  </button>



  <footer>
    <hr> &COPY; 2021 ClassicFantastic Inc.
  </footer>


</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/meny.json'
const socket = io();

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      selected: "Klarna",
      gender: "Do not wish to provide",
      fn: "",
      en: "",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addToOrder: function (event) {
      return this.orderedBurgers[event.name] = event.amount;
    },
    addOrder: function () {
      // var offset = {
      //   x: event.currentTarget.getBoundingClientRect().left, //Han ändrade från var till let :)
      //   y: event.currentTarget.getBoundingClientRect().top
      // };
      /*     socket.emit("addOrder", {
             orderId: this.getOrderNumber(),
             details: {
               x: event.clientX - 10 - offset.x,
               y: event.clientY - 10 - offset.y
             },
             orderItems: ["Beans", "Curry"],
             /!*orderedBurgers: this.addToOrder(),*!/
             form: [this.gender, this.fn, this.en, this.selected, this.orderedBurgers]
           });*/
      // this.location = {x: event.clientX - 10 - offset.x, y: event.clientY - 10 - offset.y}
    },
    submitOrder: function () {
      socket.emit('addOrder', {
            orderId: this.getOrderNumber(),
            details: {
              x: this.location.x,
              y: this.location.y
            },
            orderItems: this.orderedBurgers,
            form: {
              gender: this.gender,
              fn: this.fn,
              en: this.en,
              selected: this.selected,
            }
          },
      )},
    setLocation:function (event) {
      let offset = {
        x: event.currentTarget.getBoundingClientRect().left, //Från var till let
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location = {x: event.clientX - 10 - offset.x, y: event.clientY - 10 - offset.y}
    },
  }
}
</script>

<style>


#map {
  width: 1900px;
  height: 500px;
  background: url("/img/polacks.jpg");
}
.scrollMap{
  height: 500px;
  width: 500px;
  overflow: scroll;
}
body{
  font-family: "AppleGothic",serif;
  font-size:20px ;
}

button:hover {
  background-color: deeppink;
  cursor: pointer;
}
button{
  margin: 10px
}
#backgroundForm{
  background-color: lightcoral;
  border: 2px dotted deepskyblue;
  border-radius: 10px;
  color: black;
  display: grid;
  grid-template-columns: 50% 50%;
  grid-template-rows: 100%;
}
section{
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 30px;
}
#header{
  margin-left: 5px;
  margin-right: 5px;
  height: 400px;
  border-radius: 30px;
}

#headerImage{
  height: auto;
  width: 100%;
  opacity: 50%;
}
#header_text{
  margin-top: -200px;
  margin-bottom: 10px;
  position: center;
  padding-left: 10px;

}

.wrapperBurger{
  display: grid;
  grid-template-columns: 33% 33% 33%;
  grid-template-rows: 100%;
  grid-gap: 10px;
  background-color: lightsteelblue;
  border-radius: 20px;
}
#dots div{
  position: relative;
  background: white;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
}
#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg) no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}
</style>