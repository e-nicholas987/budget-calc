<script>
  // import Github from './Github.svelte';
    import GithubAwait from './GithubAwait.svelte';

  import { setContext, onMount, afterUpdate} from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import ClearExpense from "./ClearExpense.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
    import Modal from "./Modal.svelte";
//data
  // import expensesData from "./expenses";

  // variables
  let expenses = [];

  //set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  //toggleFormVariables
  let isFormOpen = false;

  //reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return acc + curr.amount;
  }, 0);

  // functions

  function showForm() {
    isFormOpen = true;
  }
  function closeForm() {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = null;
  }

  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  }

  function setModifiedExpense(id) {
    showForm();
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map(item => {
      if (item.id === setId) {
        return { ...item, name: name, amount: amount };
      }
      if (item.id !== setId) {
        return { ...item };
      }
    });
    setId = null;
    setAmount = null;
    setName = "";
  }

  //context
  setContext("remove", removeExpense);
  setContext("clear", clearExpenses);
  setContext("addnew", addExpense);
  setContext("modify", setModifiedExpense);
//
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(()=> {
console.log("after update")
setLocalStorage()
  })
</script>

<Navbar {showForm} />
<main class="content">
  <!-- <GithubAwait/> -->

  {#if isFormOpen}
  <Modal>
    <ExpenseForm
      name={setName}
      amount={setAmount}
      {editExpense}
      {isEditing}
      {closeForm} />
      </Modal>
  {/if}
  <Totals title="total expense" {total} />
  <ExpensesList {expenses} />
  <ClearExpense />

</main>

