<!-- Essa opção deixa objetos imutaveis. todos=todos nao funciona mais -->
<svelte:options  immutable={true} />

<script>
  import TodoList from "./lib/TodoList.svelte";
  import {v4 as uuid} from "uuid";
  import { tick } from "svelte";
  import { exclude_internal_props, onMount } from "svelte/internal";
  
  let todos = [];
  // [
  //   { id: uuid(), title: 'Todo 1', done: true },
  //   { id: uuid(), title: 'Todo 2', done: false },
  //   { id: uuid(), title: 'Todo 3', done: true },
  //   { id: uuid(), title: 'Todo long long long long long long long long long long long long long long long long long long long long ', done: false }
  // ];
  let todoList;
  let showList = true;
  let error = null;
  let isLoading = false;

  //$: console.log(todos);

  const loadTodos = async () => {
    isLoading = true;
    await fetch('https://jsonplaceholder.typicode.com/todos')
      .then(resp => {
        if(resp.ok){
          return resp.json();
        } else {
          isLoading = false;
          error = 'Erro getting data';
          throw new Error('Erro getting data');
        }
      })
      .then(async data => {
        let newData = [];  
        for(const key in data){
          newData.push({...data[key], done: data[key].completed});
        }
        isLoading = false;
        todos =  await newData;
    });
  };

  onMount(() => {
    loadTodos();
  });
  const handleAddTodo = async (e) => {

    e.preventDefault();
    //Essa alternativa não funciona com immutable = true
    /*
    
    todos.push({
      id: uuid(),
      title: e.detail.title,
      done: false
    });
    todos = todos;
    */

    //Com immutable true temos que usar outra opcao

    //console.log(document.querySelectorAll('.todo-list ul li'));

    //setTimeout(() => {
      todos = [
        ...todos,
        {
        id: uuid(),
        title: e.detail.title,
        done: false
      }];
      todoList.clearInput();

    //}, 2);

    await tick();
    //console.log(document.querySelectorAll('.todo-list ul li'));

  };

  const handleRemoveTodo = (e) => {
    todos = todos.filter(t => t.id !== e.detail.id);
  };

  const handleToggleTodo = (e) => {
    
    todos = todos.map( t => {
      if (t.id === e.detail.id){
        return { ...t, done: e.detail.value };
      } else {
        return { ...t };
      }
    });
  };

</script>

<!-- <h2>{todos.length} todos</h2> -->

<!-- { todoList && todoList.readOnly} -->

<label>
  <input type="checkbox" bind:checked={showList} />
  Show / Hide List
</label>

{#if showList}
  
    <div style:max-width="350px">
      <TodoList {todos} 
        bind:this={todoList}
        on:addTodo={handleAddTodo}
        on:removeTodo={handleRemoveTodo}
        on:toggleTodo={handleToggleTodo}
        {error}
        {isLoading}
      />
    </div>
  
{/if}

<button on:click={() => todoList.focusInput()} >Ford Focus</button>


<style>
  
</style>