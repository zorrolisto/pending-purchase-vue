<script setup>
import { ref, onMounted, defineComponent } from "vue";
import MyForm from "./components/MyForm.vue";
import MyList from "./components/MyList.vue";

const pendingPurchases = ref([]);

onMounted(() => {
  const pendingPurchasesLocalStorage = localStorage.getItem("pendingPurchases");
  if (pendingPurchasesLocalStorage) {
    pendingPurchases.value = JSON.parse(pendingPurchasesLocalStorage);
  }
});
function updatePendingPurchasesInLocalStorage() {
  localStorage.setItem(
    "pendingPurchases",
    JSON.stringify(pendingPurchases.value)
  );
}
function addNewPurchase(pendingPurchase) {
  pendingPurchases.value = [...pendingPurchases.value, pendingPurchase];
  updatePendingPurchasesInLocalStorage();
}
function manageCheckPurchase(idOfPurchase) {
  pendingPurchases.value = pendingPurchases.value.map((p) =>
    p.id === idOfPurchase ? { ...p, alreadyBought: !p.alreadyBought } : p
  );
  updatePendingPurchasesInLocalStorage();
}
function deletePurchase(idOfPurchase) {
  pendingPurchases.value = pendingPurchases.value.filter(
    (p) => p.id !== idOfPurchase
  );
  updatePendingPurchasesInLocalStorage();
}
</script>

<template>
  <div class="main">
    <h1>Pending Purchases</h1>
    <MyForm @addNewPurchase="addNewPurchase" />
    <MyList
      @manageCheckPurchase="manageCheckPurchase"
      @deletePurchase="deletePurchase"
      :pendingPurchases="pendingPurchases"
    />
  </div>
</template>

<style scoped>
.main {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  display: flex;
  flex-direction: column;
}

h1 {
  font-size: 3.2em;
  line-height: 0;
}
</style>
