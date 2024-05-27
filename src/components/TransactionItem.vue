<template>
  <div class="transaction-card">
    <div class="transaction-card__top-block">
      <div class="transaction-card__main-info">
        <p class="transaction-card__name">{{ transaction.name }}</p>
        <p
          :class="[
            transaction.type === 'expense'
              ? 'transaction-card__amount--expense'
              : 'transaction-card__amount--income',
            'transaction-card__amount',
          ]"
        >
          {{ transaction.type === "expense" ? "-" : "+" }}
          {{ transaction.amount }}
        </p>
      </div>
      <i
        @click="deleteTransaction"
        title="Delete"
        class="fa-regular fa-trash-can transaction-card__delete-icon"
      ></i>
    </div>
    <p class="transaction-card__date">{{ stringDate }}</p>
  </div>
</template>

<script setup>
import { toRefs } from "vue";
const props = defineProps({
  transaction: Object,
});

const emit = defineEmits(["delete-transaction"]);

const { transaction: transaction } = toRefs(props);

const stringDate = new Date(transaction.value.createdAt).toLocaleDateString();

const deleteTransaction = () => {
  emit("delete-transaction", transaction);
};
</script>

<style >

</style>
