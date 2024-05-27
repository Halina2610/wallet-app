<script setup>
import { ref, onMounted, watch, nextTick } from "vue";

const balance = ref("");
const transactions = ref([]);
const editingMode = ref(false);
const balanceInputField = ref();

const setBalance = () => {
  if (balance.value !== "") {
    editingMode.value = false;
  }
};

const editBalance = async () => {
  editingMode.value = true;
  await nextTick();
  balanceInputField.value.focus();
};

function limitNumericInputs() {
  if (balance.value > 99999999) {
    balance.value = 99999999;
  }
  if (balance.value < 0) {
    balance.value = 0;
  }
}

const addTransaction = (transaction) => {
  transactions.value.push(transaction);
  if (transaction.type == "expense") {
    balance.value -= transaction.amount;
  } else {
    balance.value = +balance.value + transaction.amount;
  }
  editingMode.value = false;
};

const deleteTransaction = (transaction) => {
  transactions.value = transactions.value.filter(
      (item) => item.id !== transaction.value.id
  );
  if (transaction.value.type == "expense") {
    balance.value = +balance.value + transaction.value.amount;
  } else {
    balance.value -= transaction.value.amount;
  }
};

watch(balance, (newVal) => {
  localStorage.setItem("balance", newVal);
});

watch(
    transactions,
    (newVal) => {
      localStorage.setItem("transactions", JSON.stringify(newVal));
    },
    { deep: true }
);

onMounted(() => {
  balance.value = localStorage.getItem("balance") || "";
  editingMode.value = balance.value === "" ? true : false;
  transactions.value = JSON.parse(localStorage.getItem("transactions")) || [];
});
</script>