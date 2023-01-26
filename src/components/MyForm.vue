<script setup lang="ts">
import { ref } from "vue";
import { PendingPurchase } from "../models/PendingPurchase";

defineProps<{
  addNewPurchase: (p: PendingPurchase) => void;
}>();


const nameOfPurchase = ref<string>("");
const priceOfPurchase = ref<number | null>(null);
const emit = defineEmits<{
  (e: "addNewPurchase", val: PendingPurchase): void;
}>();

function handleAddNewPurchase() {
  const newPurchase: PendingPurchase = {
    id: new Date().getTime(),
    name: nameOfPurchase.value,
    price: Number(priceOfPurchase.value),
    alreadyBought: false,
  };
  emit("addNewPurchase", newPurchase);
  nameOfPurchase.value = "";
  priceOfPurchase.value = null;
}
</script>

<template>
  <div class="myform">
    <input v-model="nameOfPurchase" placeholder="Name" />
    <input v-model="priceOfPurchase" type="number" placeholder="Price" />
    <button @click="handleAddNewPurchase">Add</button>
  </div>
</template>

<style scoped>
.myform {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  display: flex;
  gap: 0.25rem;
}

input {
  padding: 1rem;
  border-radius: 8px;
}

button {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;
}
button:hover {
  border-color: #646cff;
}
button:focus,
button:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}

@media (prefers-color-scheme: light) {
  button {
    background-color: #f9f9f9;
  }
}
</style>
