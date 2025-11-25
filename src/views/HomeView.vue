<template>

<div> 
<header id="main-header">
    <h1 id="header-title">Welcome to burgir online</h1>
    <img id="header-image" src="/img/hipsterbild.jpg" alt="">
</header>

  <main>
    <section id="burger">
      <h2 id="burgerselection">Our burger selection</h2>

<div class="burger-grid">
  <Burger 
    v-for="burger in burgers"
    :key="burger.name"
    :burger="burger"
    @update-order="updateOrderedBurgers"
  />
</div>

    </section>

    </main>

    <section id="orderinformation">
      <h2>Customer information</h2>
      <p>Please provide your delivery information below.</p>
  
  <footer>

    <input 
      type="text" 
      id="full-name" 
      v-model="fullName" 
      required 
      placeholder="First- and last name"
    >

    <input 
      type="email" 
      id="email" 
      v-model="email" 
      required 
      placeholder="E-mail address"
    >
    <input 
      type="text" 
      id="street-name"
      v-model="streetName" 
      required 
      placeholder="Street name"
    >

    <input 
      type="number" 
      id="street-number" 
      v-model="streetNumber" 
      required 
      placeholder="Street number"
    >

    <p>
      <label for="recipient">Payment options</label><br>
      <select id="recipient" v-model="payment">
        <option>Klarna</option>
        <option>SMS lån</option>
        <option>Swish</option>
        <option>Apple pay</option>
      </select>
    </p>

    <label>Gender</label><br>

    <input type="radio" id="burgir" value="burgir" v-model="gender">
    <label for="burgir">Burgir</label><br>

    <input type="radio" id="male" value="male" v-model="gender">
    <label for="male">Male</label><br>

    <input type="radio" id="female" value="female" v-model="gender">
    <label for="female">Female</label><br>

    <input type="radio" id="non-disclosed" value="non-disclosed" v-model="gender">
    <label for="non-disclosed">Non disclosed</label><br>

    <button type="button" @click="placeOrder">
      <img src="/img/foodora.jpg" alt="Foodora" height="50" width="100"><br>
      Send Info
    </button>



  </footer>
  </section>
</div>

</template>


<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItem(name, kCal, url, gluten, lactose) {
    this.name = name;
    this.kCal = kCal;
    this.url = url;
    this.gluten = gluten;
    this.lactose = lactose;
}

export default {
  name: 'HomeView',
  components: {
    Burger
  },

data() {
  return {
    burgerText: "Välj en burgare",
    burgers: menu,
    fullName: "",
    email: "",
    streetName: "",
    streetNumber: "",
    payment: "SMS lån",
    gender: "burgir",
    orderedBurgers: {} 
  }
},

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
  updateOrderedBurgers(data) {
    this.orderedBurgers[data.name] = data.amount;
    console.log(this.orderedBurgers);
  },

    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              });
    },

  placeOrder() {
    console.log("---- NEW ORDER ----");
    console.log("Name:", this.fullName);
    console.log("Email:", this.email);
    console.log("Street:", this.streetName, this.streetNumber);
    console.log("Payment:", this.payment);
    console.log("Gender:", this.gender);
    console.log("Ordered Burgers:", this.orderedBurgers);
  },
},
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }

body {
    font-family: 'Times New Roman';
    font-size: 14pt;
}

#main-header {
    position: relative;      
    max-width: 100rem;       
    margin: 0 auto;
    height: 150px;         
    overflow: hidden;        
}

#header-image {
    width: 100%;
    height: 100%;
    object-fit: cover;       
    opacity: 0.55;          
    display: block;
}

#header-title {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    font-family: 'Agbalumo';
    font-size: 48px;          
    color: rgba(255,255,255,0.95);
    text-align: center;
    margin: 0;
    z-index: 10;
}

#burger {
    max-height: auto;
    max-width: 100rem;
    margin: 0 auto;
    overflow: hidden;
    background-color: black;
    color: white;
    padding: 1rem;  
}

#burgerselection {
    font-family: 'Agbalumo';
    font-size: 1rem;

}

.burger-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0.2rem;
}

.burger-grid img {
    width: 45%;
    height: auto;
    display: block;
}

.burger-item {
    padding: 0.5rem 0;
    min-height: 500px;
}

#orderinformation {
    margin: auto;

}

button {
    display: block;
    margin: 1rem auto;
}

button:hover {
    background-color: green;
    cursor: pointer;
}

.allergies {
    font-weight: bold;
}

#orderinformation {
    background-color: #fff4e6;  
    color: black;
    padding: 1rem;
}

section {
    margin: 2rem auto;
    max-width: 100rem;
}

#burger,
#orderinformation {
    border: 3px dashed grey;
}
</style>



