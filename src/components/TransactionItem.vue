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
.transaction-card__top-block {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.transaction-card__main-info {
  display: flex;
  justify-content: space-between;
  flex: 1;

  @media (max-width: 990px) {
    flex-direction: column;
  }
}

.transaction-card__name {
  max-width: 250px;
  font-weight: 500;
  line-height: 1.4;
  word-wrap: break-word;

  @media (max-width: 990px) {
    max-width: 200px;
  }

  @media (max-width: 480px) {
    max-width: 150px;
  }
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
  color: var(--accent-text);
  margin-left: 32px;
}
.transaction-card__date {
  margin-top: 12px;
  font-size: 14px;
  color: var(--text-color-secondary);
}
</style>
