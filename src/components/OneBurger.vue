<template>
  <div class="burger-item">
    <h3>{{ burger.name }}</h3>
    <img :src="burger.url" :alt="burger.name">

    <div class="allergies">
      <ul>
        <li v-if="burger.gluten">Contains gluten</li>
        <li v-if="burger.lactose">Contains lactose</li>
      </ul>
    </div>

    <p>Ordered: {{ amountOrdered }}</p>

    <button @click="increase">+</button>
    <button @click="decrease">−</button>

    </div>
  </template>


<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },

  data() {
    return {
      amountOrdered: 0
    };
  },

methods: {
  increase() {
    this.amountOrdered++;
    this.$emit('update-order', {
      name: this.burger.name,
      amount: this.amountOrdered
    });
  },

    decrease() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('update-order', {
          name: this.burger.name,
          amount: this.amountOrdered
        });
      }
    }
  }
}
</script>



<style scoped>
.burger-item {
  padding: 1rem 0;
}

.burger-item img {
  width: 60%;
  display: block;
}
</style>