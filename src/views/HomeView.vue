<template>

  <div>
    <header id="main-header">
      <h1 id="header-title">Welcome to burgir online</h1>
      <img id="header-image" src="/img/burgeronline.png" alt="">
    </header>

    <main>
      <section id="burger">
        <h2 id="burgerselection">Our burger selection</h2>

        <div class="burger-grid">
          <Burger v-for="burger in burgers" :key="burger.name" :burger="burger" @update-order="updateOrderedBurgers" />
        </div>

      </section>



    </main>

<section id="orderinformation" class="customer-section">

      <h2>Customer information</h2>
      <p>Please provide your delivery information below.</p>

      <footer>
<p> 
        <input type="text" id="full-name" v-model="fullName" required placeholder="First- and last name">
</p>
        <p>
        <input type="email" id="email" v-model="email" required placeholder="E-mail address">
</p>
        <p>
          <label for="recipient">Payment options</label><br>
          <select id="recipient" v-model="payment">
            <option>Klarna</option>
            <option>SMS lån</option>
            <option>Swish</option>
            <option>Apple pay</option>
          </select>
        </p>

        <p>Where living?</p>
              <div id="map-container">
        <div id="map" @click="setLocation">

<div class="target" 
     :style="{ left: location.x + 'px', top: location.y + 'px' }">
  T
</div>

        </div>
      </div>

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
      orderedBurgers: {},

      location: {
        x: 0,
        y: 0
      }
    }
  },
methods: {
    getOrderNumber() {
      return Math.floor(Math.random() * 100000);
    },

    updateOrderedBurgers(data) {
      this.orderedBurgers[data.name] = data.amount;
    },

setLocation(event) {
  const offset = {
    x: event.currentTarget.getBoundingClientRect().left,
    y: event.currentTarget.getBoundingClientRect().top,
  };

  this.location.x = event.clientX - offset.x;
  this.location.y = event.clientY - offset.y;

  console.log("New location", this.location)
},

placeOrder() {

  const items = Object.entries(this.orderedBurgers)
  .filter(([name, amount]) => amount > 0)
  .map(([name, amount]) => `${name} (${amount})`);

  console.log("Sending order:", {
  orderId: this.getOrderNumber(),
  details: { x: this.location.x, y: this.location.y },
  orderItems: items,
  customer: {
    name: this.fullName,
    email: this.email,
    payment: this.payment,
    gender: this.gender
  }
});

socket.emit("addOrder", {
  orderId: this.getOrderNumber(),
  details: {
    x: this.location.x,
    y: this.location.y
  },
  orderItems: items,
  customer: {
    name: this.fullName,
    email: this.email,
    payment: this.payment,
    gender: this.gender
  }
});

    alert("Order placed")
}}}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

#map-container {
  width: auto;
  height: 400px;
  overflow: scroll;
  border: 3px dashed grey;
  margin: 1rem auto;
}

#map {
  position: relative;
  width: 1920px;
  height: 1080px;
  background: url("/img/polacks.jpg");
  background-size: contain;
}

body {
  font-family: 'Times New Roman';
  font-size: 14pt;
}

#main-header {
  position: relative;
  margin: 0 auto;
  height: 200px;
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
  color: rgba(255, 255, 255, 0.95);
  text-align: center;
  margin: 0;
  z-index: 10;
}

#burger {
  max-height: auto;
  max-width: 100rem;
  margin: 10 auto;
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
  background-color: white;
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

.target {
  position: absolute;
  width: 20px;
  height: 20px;
  background: black;
  color: white;
  border-radius: 4px;
  text-align: center;
  line-height: 20px;
  font-weight: bold;
}


</style>
