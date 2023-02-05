<script>
  import { createEventDispatcher } from "svelte";
  let dispatch = createEventDispatcher();

  let completed = true;

  const checkAll = () => {
    dispatch("checkAll", completed);
    completed = !completed;
  };
  const removeCompleted = () => dispatch("removeCompleted");
</script>

<div class="btn-group">
  <button type="button" class="btn btn__primary" on:click={checkAll}>{completed?'Check' : 'Uncheck'}</button>
  <button type="button" class="btn btn__primary" on:click={removeCompleted}>Remove Completed</button>
</div>