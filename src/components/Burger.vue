<template>
  <div class = "Burgers">

    <h3 class = "burgerName">{{ burger.name }}</h3>
    <img class = "burgerImage" v-bind:src = "burger.url">


    <ul>
      <li>{{ burger.kCal }} kCal</li>
      <span v-if = "burger.Lactose == true"><li> Contains <span class = "ingredients"> lactose </span></li></span>
      <span v-if = "burger.Gluten == true"><li> Contains <span class = "ingredients"> gluten </span></li></span>
      <li> Price {{ burger.Price }} sek </li>

    </ul>
    <br>
        <div >
      Amount:{{amountOrdered}}
      <button v-on:click = "decreaseOrder" class="amountButton"> - </button>
      <button v-on:click = "increaseOrder" class="amountButton"> + </button>
          {{amountOrdered}}Orders
        </div>
  </div>
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
        decreaseOrder: function () {
          if (this.amountOrdered > 0) {
            this.amountOrdered -= 1;
            console.log(this.amountOrdered, this.burger.name)
            this.$emit('orderedBurgers', {name: this.burger.name, amount: this.amountOrdered});
          }
        },
        increaseOrder: function () {
          this.amountOrdered += 1;
          console.log(this.amountOrdered, this.burger.name)
          this.$emit('orderedBurgers', {name: this.burger.name, amount: this.amountOrdered});


        }
      }

    }


    </script>

    <!-- Add "scoped" attribute to limit CSS to this component only -->




<style scoped>

.burgerImage{
  height: 300px;
  width: 300px;

  display: block;
  margin: auto;


}
.burgerName{
  text-align: center;
}
.ingredients{
  font-weight: bold;
  color: deeppink;
}
.amountButton{
  width: 50px;
  height: 30px;
  font-family: "AppleGothic";
  font-size:15px ;

}



</style>
