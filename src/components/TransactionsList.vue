<template>
  <section class="transactions-list">
    <p class="list__title">Trasnsactions</p>
    <div class="list__container">
      <template
          class="transaction-card"
          v-for="transaction in sortedByDate"
          :key="transaction.createdAt"
      >
        <TransactionItem
            :transaction="transaction"
            @delete-transaction="deleteTransaction"
        />
      </template>
    </div>
  </section>
</template>

<script setup>
import { computed, toRefs } from "vue";
import TransactionItem from "./TransactionItem.vue";

const props = defineProps({
  transactions: Array,
});

const { transactions: transactions } = toRefs(props);

const emit = defineEmits(["delete-transaction"]);

emits: ["delete-transaction"];

const sortedByDate = computed(() => {
  return transactions.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  });
});

const deleteTransaction = (transaction) => {
  emit("delete-transaction", transaction);
};
</script>

<style scoped>
.transaction-card {
  margin-right: 10px;
  background-color: var(--white);
  border-radius: 8px;
  box-shadow: var(--shadow-s);
  padding: 16px 20px;
  margin-top: 10px;
  display: flex;
  flex-direction: column;

  @media (max-width: 990px) {
    padding: 12px 16px;
    object-fit: contain;
  }
}



  @media (max-width: 990px) {
    max-width: 200px;
  }

  @media (max-width: 480px) {
    max-width: 150px;
  }

.transaction-card__amount {
  font-weight: 500;

  @media (max-width: 990px) {
    margin-top: 8px;
  }
}

.transaction-card__amount--expense {
  color: var(--error);
}

.transaction-card__amount--income {
  color: var(--success);
}

.transaction-card__delete-icon {
  color: var(--secondary);
  margin-left: 32px;
}
.transaction-card__date {
  margin-top: 12px;
  font-size: 14px;
  color: var(--text-color-secondary);
}
</style>
