<template>
<header>
        <img src="https://readwrite.com/wp-content/uploads/2019/05/Mural-Paintings-and-the-Impact-of-Colors-in-Our-Lives-825x500.jpg" alt="fast food header" style = "height: 150px; width: 1600px;" >

        <section class="head_text">
            <h1>
                Welcome to BugerHeavan!
            </h1>
        </section>
        
    </header>
    
    
    <main>
        <section id="burger_info">
            <section id="burger_info_header">
                <h3>Select your burger</h3>
                <p>This is where you select and customize your burger</p>
            </section>
            
            <div class="burger_wrapper">
              
              <Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"
                v-on:orderedBurger= "addToOrder($event)"/>
            </div>

        </section>

        <section class="delivery_info">
          <h2> Customer information</h2>  
            <p> This is where you provide necessary information like allergies </p>

            <p>
                <label for="firstname">Full name</label><br>
                <input type="text" id="name" v-model="ne" required="required" placeholder="First- and last name">
            </p>
            <p>
                <label for="email">Email</label><br>
                <input type="email" id="email" v-model="em" placeholder="email">
            </p>
            <!--<p>
                <label for="street">Street</label><br>
                <input type="text" id="street" v-model="st" required="required" placeholder="street">
            </p>
            <p>
                <label for="housenr">House number</label><br>
                <input type="number" id="housenr" v-model="nr" required="required" placeholder="Street number">
            </p>-->

            <p>
                <label for="payment">Payment method</label><br>
                <select id="payment" v-model="p">
                    <option>Master Card</option>
                    <option>Swish</option>
                    <option>Paypal</option>
                    <option>Venmo</option>

                </select>
            </p>

            <p>

                <label for="gender">Man</label>
                <input type="radio" v-model="gr" value="female">
                <label for="gender">Woman</label>
                <input type="radio" v-model="gr" value="Male">
                <label for="gender">No thanks</label>
                <input type="radio" v-model="gr" checked="checked" value="other">
            </p>
        </section>

        <div id="mapWrap">
          click here
          <div id="map"  v-on:click="setLocation">
            <div v-bind:style="{left: this.location.x + 'px', top: this.location.y + 'px'}">
              <span>T</span>
            </div>
          </div>
        </div>
        
        <button type="submit" id="köpknapp" v-on:click="order">
            <img src="https://cdn5.vectorstock.com/i/1000x1000/34/29/basket-icon-in-modern-style-for-web-site-vector-26533429.jpg" style = "width: 30px;">
            Place order
        </button>
    </main>

    <footer>
        <hr>
        Burgers!  Burgers!     Burgers!     Burgers!     Burgers!     Burgers!     Burgers!     Burgers!         Burgers!      Burgers!      Burgers!      Burgers!      Burgers!      Burgers!      Burgers! 
    </footer> 

</template>

<script>
import Burger from '../components/OneBurger.vue'
import menu from '../Assets/menu.json'
import io from 'socket.io-client'

const socket = io();
/*
function MenuItem (name, url, kcal,gluten,lactose){
  this.productName = name;
  this.link = url;
  this.kCal = kcal;
  this.gluten = gluten;
  this.lactose = lactose;
}
  const arrayOfBurgers = [
    new MenuItem("Dobbel Puck deluxe","https://bbqlovers.se/wp-content/uploads/2017/04/Smashburgare2-300x251.jpg", 1200, true, true),
    new MenuItem("Breacky muffin", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcToFztV9KPQVABUUwJFbZKyo_Kz4yw4nrjB4g&usqp=CAU", 450, true, false ),
    new MenuItem("Big chicken","https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRcF_9i4edK1S56jzkObFRDAkSYvCti5n9MIg&usqp=CAU", 750, true, true )
  ];
  console.log(arrayOfBurgers);
*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      p:'',
      //st:'',
      ne:'',
      em:'',
      gr: '',
      orderedBurgers: {},
      location:{x: 0, y: 0}
  
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = {x: event.clientX - 20 - offset.x, y: event.clientY - 10 - offset.y}              
      console.log(this.location)
      //console.log(this.p,this.st,this.ne,this.em,this.gr)
    },

    order: function (event) {
      console.log(this.p,this.ne,this.em,this.gr),
      console.log(this.orderedBurgers),
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers,
                                personalInformation: {ne: this.ne,
                                                      em: this.em,
                                                      p: this.p,
                                                      gr: this.gr}
                                                      
                              }
                 );                         

    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers)
    },
  }
}
</script>

<style>
  #map {
    width: 1920px;
    height: 1078px;
    background:url("../../public/img/polacks.jpg");
  }
  
  #map div {
    position: relative;
  }

  #mapWrap{
    width: 500px;
    height: 500px;
    overflow: scroll;
  }
  
h3 {
  color: blue;
}

body{
  font-family: "Comic Sans MS", "Comic Sans",cursive;
}

.head_text{
  margin: 50px 50px 50px 50px; 
  overflow:hidden;
  height: 100px;
}

  #burger_info{
    color: white;
    background-color: black;
    padding: 20px 20px;
    border: 3px dashed white;
  }

  .burger_wrapper{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 400px 400px 400px; 
  }

  .burger_wrapper{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 400px 400px 400px;
  }

  .delivery_info{
    margin: 50px 50px 50px 50px;
    padding: 20px 20px;
    border: 3px dotted black;
  }

button:hover{
  background-color: aquamarine;
  cursor: progress;
}

#köpknapp{
  margin: 10px;
}
</style>