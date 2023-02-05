<script>
  import { createEventDispatcher } from 'svelte';
  import { onMount } from 'svelte';
  const dispatch = createEventDispatcher();
  let nameEl;
  console.log('initilalizing:' , nameEl);

  onMount(() => {
    if (autofocus) {nameEl.focus()}
  })
  export let autofocus = false;

  let name = '';
  // if ( autofocus ) nameEl.focus();
  const addTodo = () => {
    dispatch('addTodo', name);
    name = '';
    nameEl.focus();
  }

  const onCancel = () => {
    name = '';
    nameEl.focus();
  }
</script>

<form 
  on:submit|preventDefault = {addTodo} 
  on:keydown={ e => e.key ==='Escape' && onCancel()}>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">What needs to be done?</label>
  </h2>
  <input bind:this={nameEl} bind:value={name} type="text" id="todo-0" autoComplete="off" class="input input__lg" />
  <button type="submit" disabled={!name} class="btn btn__primary btn__lg">Add</button>
</form>
