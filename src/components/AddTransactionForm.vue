<template>
  <form class="form" @submit.prevent="addTransaction">
    <div class="form__error-message" v-show="errorMessage">
      {{ errorMessage }}
    </div>
    <div class="form__input-group">
      <label class="form__label">Name</label>
      <input
          class="form__input"
          @focus="removeError"
          @input="name = name.slice(0, 30)"
          type="text"
          placeholder="Enter transaction name..."
          v-model="name"
          ref="nameInput"
      />
    </div>
    <div class="form__input-group">
      <label class="form__label">Amount</label>
      <input
          class="form__input"
          @focus="removeError"
          @input="limitNumericInputs"
          type="number"
          placeholder="Enter amount..."
          v-model="amount"
          ref="amountInput"
      />
    </div>

    <div class="form__transaction-types">
      <label class="form__radio-label">
        <input
            type="radio"
            name="type"
            value="income"
            v-model="type"
            class="form__radio-btn"
        />
        <p>Income</p>
      </label>
      <label class="form__radio-label">
        <input
            type="radio"
            name="type"
            value="expense"
            v-model="type"
            class="form__radio-btn"
        />
        <p>Expense</p>
      </label>
    </div>
    <button class="form__submitBtn" type="submit">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits();

const name = ref("");
const amount = ref("");
const type = ref("expense");
const errorMessage = ref("");
const nameInput = ref();
const amountInput = ref();

function limitNumericInputs() {
  if (amount.value > 99999999) {
    amount.value = 99999999;
  }
  if (amount.value < 0) {
    amount.value = 1;
  }
}

const addTransaction = () => {
  if (
      name.value === "" ||
      amount.value === "" ||
      amount.value === 0 ||
      type.value === null
  ) {
    nameInput.value.classList[name.value === "" ? "add" : "remove"](
        "form__input--error"
    );
    amountInput.value.classList[
        amount.value === "" || amount.value === 0 ? "add" : "remove"
        ]("form__input--error");

    errorMessage.value = "Please  fill in all fields ";

    return;
  }

  errorMessage.value = "";

  nameInput.value.classList.remove("form__input--error");
  amountInput.value.classList.remove("form__input--error");

  const transaction = {
    id: new Date().getTime(),
    name: name.value,
    amount: amount.value,
    type: type.value,
    createdAt: new Date().getTime(),
  };

  emit("add-transaction", transaction);

  name.value = "";
  amount.value = "";
};

const removeError = (e) => {
  e.target.classList.remove("form__input--error");
  errorMessage.value = "";
};
</script>

<style scoped>
.form {
  margin-top: 22px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form__input-group {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}

.form__input {
  margin-top: 8px;
  padding: 8px 12px;
  width: 100%;
  border-radius: 5px;
  outline: 1px solid var(--secondary);
}

.form__input:focus {
  outline: 1px solid var(--accent-text);
}

.form__transaction-types {
  margin-top: 10px;
  display: flex;
  justify-content: center;
  gap: 60px;
}

.form__radio-label {
  text-align: center;
  cursor: pointer;
}

.form__radio-btn {
  height: 14px;
  width: 14px;
  accent-color: var(--accent-text);
}

.form__error-message {
  color: var(--error);
  font-weight: 500;
  border-radius: 5px;
}
.form__input--error {
  outline: 1px solid var(--error);
  background-color: var(--error-backgraund);
}

.form__submitBtn {
  margin-top: 10px;
  padding: 12px 16px;
  border-radius: 8px;
  font-weight: 500;
  color: var(--white);
  transition: 0.3s ease-in-out;
  background: var(--gradient-elements);
  opacity: 0.9;
  box-shadow: var(--shadow-s);
}

.form__submitBtn:hover {
  opacity: 1;
}
</style>
