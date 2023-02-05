<script>
  import FilterButton from "./FilterButton.svelte";
  import Todo from "./Todo.svelte";
  import MoreAction from "./MoreAction.svelte";
  import NewTodo from "./NewTodo.svelte";
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

  function removeTodo(todo) {
    todos = todos.filter(t => t.id !== todo.id);
  }

  function addTodo(name) {
    todos = [...todos, {id: newTodoId, name, completed: false}]
  }
  
  const checkAllTodos = (completed) => todos = todos.map((t) => ({ ...t, completed }));

  const removeCompletedTodos = () => todos = todos.filter( t => !t.completed);
  let filter = 'all';
  let autofocus = true;
  const filterTodos = (filter, todos) => 
    filter === 'active' ? todos.filter(t => !t.completed):
    filter === 'completed' ? todos.filter(t => t.completed):
    todos
</script>
<div class="todoapp stack-large">
  <NewTodo 
    on:addTodo = {e => addTodo(e => e.detail)} 
    autofocus
    />
  <FilterButton 
    bind:filter ={filter} 
    />
  <MoreAction 
    on:checkAll = {(e) => checkAllTodos(e.detail)}
    on:removeCompleted = {removeCompletedTodos}
    />
  <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>
  <!-- Todos -->
  <ul  class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
    <li class="todo">
      <Todo {todo} on:update={(e) => updateTodo(e.detail)} on:remove={(e) =>
        removeTodo(e.detail)} />
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