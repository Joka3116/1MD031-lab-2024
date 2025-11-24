<template>
<div>
    <div>
    <h1>Burgers</h1>
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
    </div>
    <input type="text" v-model="burgerText">
<div>
  {{ burgerText }}
</div>
    <div id="map" v-on:click="addOrder">
    click here
    </div>
</div>

<header id="main-header">
    <h1 id="header-title">Welcome to burgir online</h1>
    <img id="header-image" src="/img/hipsterbild.jpg" alt="">
</header>

  <main>
    <section id="burger">
      <h2 id="burgerselection">Our burger selection</h2>

      <div class="burger-grid">

        <div class="burger-item">
          <h3>Crispy burgir</h3>
          <img src="/img/crispyburgir.jpg" alt="Crispy burgir">
          <div class="allergies">
            <ul>
              <li>Contains alcohol</li>
              <li>Pure lactose</li>
              <li>Glutenfree bun, burger is not</li>
            </ul>
          </div>
        </div>

        <div class="burger-item">
          <h3>Vego burgir</h3>
          <img src="/img/90g.jpeg" alt="90g burgir">
          <div class="allergies">
            <ul>
              <li>May contain meat</li>
              <li>Lactosefree</li>
              <li>Glutenfree burgir, bun is not</li>
            </ul>
          </div>
        </div>

        <div class="burger-item">
          <h3>Non burgir burgir</h3>
          <img src="/img/taco.jpeg" alt="Crispy burgir">
          <div class="allergies">
            <ul>
              <li>Tomatoes not included</li>
              <li>High in caffeine</li>
              <li>Served in soft bread</li>
            </ul>
          </div>
        </div>

      </div>
    </section>


    </main>

    <section id="orderinformation">
      <h2>Customer information</h2>
      <p>Please provide your delivery information below.</p>
  
  <footer>

    <p>
      <label for="full-name">Full name</label><br>
      <input type="text" id="full-name" name="fn" required placeholder="First- and last name">
    </p>

    <p>
      <label for="email">E-mail</label><br>
      <input type="email" id="email" name="em" required placeholder="E-mail address">
    </p>

    <p>
      <label for="street-name">Street name</label><br>
      <input type="text" id="street-name" name="street" required placeholder="Street name">
    </p>

    <p>
      <label for="street-number">Street number</label><br>
      <input type="number" id="street-number" name="streetnr" required placeholder="Street number">
    </p>

    <p>
      <label for="recipient">Payment options</label><br>
      <select id="recipient" name="rcp">
        <option>Klarna</option>
        <option selected>SMS lån</option>
        <option>Swish</option>
        <option>Apple pay</option>
      </select>
    </p>

    <label>Gender</label><br>

    <input type="radio" id="burgir" name="gender" value="burgir" checked>
    <label for="burgir">Burgir</label><br>

    <input type="radio" id="male" name="gender" value="male">
    <label for="male">Male</label><br>

    <input type="radio" id="female" name="gender" value="female">
    <label for="female">Female</label><br>

    <input type="radio" id="non-disclosed" name="gender" value="non-disclosed">
    <label for="non-disclosed">Non disclosed</label><br>

    <button type="button">
      <img src="/img/foodora.jpg" alt="Foodora" height="50" width="100"><br>
      Send Info
    </button>

  </footer>
  </section>


</template>


<script>
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

const menu = [
  new MenuItem("Crispy Burgir", 650, "/img/crispyburgir.jpg", true, true),
  new MenuItem("Vego Burgir", 450, "/img/90g.jpeg", false, false),
  new MenuItem("Non Burgir Burgir", 300, "/img/taco.jpeg", false, false)
];

console.log(menu); 

export default {
  name: 'HomeView',
  components: {
    Burger
  },

data: function () {
  return {
    burgerText: "Välj en burgare",
    burgers: menu
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


@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

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
    max-height: 400px;
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
}


#orderinformation {
    margin: 1rem 0;
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



