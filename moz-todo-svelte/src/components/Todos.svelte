<script>
  export let todos = [];
  let newTodoId;
  $: {
        if ( totalTodos === 0 ) newTodoId = 0;
        else {
          newTodoId = Math.max(...todos.map(t => t.id)) + 1;
        }
    }
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter( todo => todo.completed).length;
  let newTodoName = '';

  function removeTodo(todo) {
    todos = todos.filter(t => t.id !== todo.id);
  }

  function addTodo() {
    todos = [...todos, {id: newTodoId, name: newTodoName, completed: false}]
    newTodoName = '';
  }
  let filter = 'all';

  const filterTodos = (filter, todos) => 
    filter === 'active' ? todos.filter(t => !t.completed):
    filter === 'completed' ? todos.filter(t => t.completed):
    todos
</script>
<div class="todoapp stack-large">
  <form on:submit|preventDefault = {addTodo}>
    <h2 class="label-wrapper">
      <label 
        for="todo-0"
        class="label__lg">
          What needs to be done?
      </label> 
    </h2>
    {newTodoName}
    <input 
      type="text" 
      id="todo-0"
      bind:value={newTodoName}
      autocomplete="off" 
      class="input input__lg" />
    <button 
      type="submit" 
      disabled="" 
      class="btn btn_primary btn__lg"
      >
        Add
      </button>
  </form>

  <div class="filters btn-group stack-exception">
    <button 
      class="btn toggle-btn" 
      aria-pressed="true"
      on:click={() => filter = 'all'}
      >
      <span class="visually-hidden">show</span>
      <sapn>All</sapn>
      <span class="visually-hidden">tasks</span>
    </button>
    <button 
      class="btn toggle-btn" 
      aria-pressed="true"
      on:click={() => filter = 'active'}
      >
      <span class="visually-hidden">Show</span>
      <span>Active</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button 
      class="btn toggle-btn" 
      aria-pressed="false"
      on:click={() => filter = 'completed'}
      >
      <span class="visually-hidden">Show</span>
      <span>Completed</span>
      <span class="visually-hidden">tasks</span>
    </button>
  </div>
  <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>
  <!-- Todos -->
  <ul  class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
    <li class="todo">
      <div class="stack-small">
        <div class="c-cb">
          <input
            type="checkbox"
            id="todo-{todo.id}"
            on:click={() => todo.completed = !todo.completed}
            checked={todo.completed} />
          <label for="todo-{todo.id}" class="todo-label"> {todo.name} </label>
        </div>
        <div class="btn-group">
          <button type="button" class="btn">
            Edit <span class="visually-hidden">{todo.name}</span>
          </button>
          <button 
            type="button" 
            class="btn btn__danger"
            on:click={() => removeTodo(todo)}
            >
            Delete <span class="visually-hidden">{todo.name}</span>
          </button>
        </div>
      </div>
    </li>
    {:else}
    <li>Nothing to do here!</li>
    {/each}
  </ul>
  <hr />

  <!-- MoreActions -->
  <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div>
</div>