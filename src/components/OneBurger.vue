<template>
    <div>
      <h4>{{ burger.name }}</h4>
      <img v-bind:src="burger.url" alt="Span" title="test" style="width: 200px height: 200px;">
          <ul>
                <span v-if="burger.gluten">
                  <li> Innehåller gluten</li>
                </span>
                <span v-if="burger.lactose">
                  <li> Innehåller lactose</li>
                </span>
                <li>{{ burger.kcal}}</li>

                <button type="submit" id= "increasebutton" v-on:click="amountOrdered++" style="width: 20px;">
                  +
                </button>
                <button type="submit" id= "decreasebutton" v-on:click="amountOrdered--" style="width: 20px;">
                  -
                </button>
                <h4>{{ amountOrdered }}</h4>
                <button type="submit" id= "order" v-if ="amountOrdered > 0 " v-on:click = "addBurger" style="width: 50px;">
                  Order
                </button>
                
          </ul>
    </div>

  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function (){
      return {
        amountOrdered:0,
      }
    },
    methods:{
      addBurger: function(){
        this.amountOrdered;
        this.$emit('orderedBurger', { name: this.burger.name,
                                      amount: this.amountOrdered
                                    }
      );
      }
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  .burger_wrapper{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 400px 400px 400px;
  }
  div *{
    margin-left:10px;
    margin-top: 10px;
  }

  </style>
  