<script>
  import Title from "./Title.svelte";
  // import { onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";
  // onMount(() => {
  //   console.log("form has mounted");
  // });
  // beforeUpdate(() => {
  //   console.count("before update");
  // });
  // afterUpdate(() => {
  //   console.count("after update");
  // });
  // onDestroy(() => {
  //   console.log("form is hidden");
  // });
  export let name = "";
  export let amount = null;
  import { getContext } from "svelte";
  const addExpense = getContext("addnew")
  export let isEditing;
  export let editExpense;
  export let closeForm;
  ;
  //   $: console.log({name, amount});
  $: isEmpty = !name || !amount;
  function handleSubmit() {
    
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }

    name = "";
    amount = null;
    closeForm();
  }

  
</script>

<section class="form">
  <Title title="add expense" />
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">please fill out all form fields</p>
    {/if}
    <button
      type="submit"
      class="btn btn-block"
      class:disabled={isEmpty}
      disabled={isEmpty}>
      {#if isEditing}edit expense{:else}add expense{/if}

    </button>
    <button type="button" class="close-btn" on:click={closeForm}>
      <i class="fas fa-times" />
      close
    </button>
  </form>
</section>
