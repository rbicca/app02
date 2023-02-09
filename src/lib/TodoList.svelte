<script>
    import {v4 as uuid} from "uuid";
    import { afterUpdate, beforeUpdate, createEventDispatcher, onDestroy, onMount } from "svelte";
    import Button from "./Button.svelte";
    import FaRegTrashAlt from 'svelte-icons/fa/FaRegTrashAlt.svelte'

    export let todos = [];
    let prevTodos = todos;
    export const readOnly = 'readonly';
    export let isLoading = false;
    export let error = null;
    export let disableAdding = false;

    let txtTodo;
    let ctrlInput;
    let todoListDiv;
    let autoScroll;
    let listHeight;

    export const clearInput = () => txtTodo = '';
    export const focusInput = () => ctrlInput.focus();

    const dispatch = createEventDispatcher();

    $:  { 
            autoScroll = todos.length !== prevTodos.length;
            prevTodos = todos; 
        }

    // onMount(()=>{
    //     console.log('onMount');
    //     return(() => {
    //         console.log('Destroyed por onMount');
    //     });
    // });

    // onDestroy(()=>{
    //     console.log('onDestroy');
    // });

    // beforeUpdate(() => {
    //     if(todoListDiv)
    //         console.log('beforeUpdate', todoListDiv.offsetHeight);
    // });

    afterUpdate(() => {
        //console.log('afterUpdate', todoListDiv.offsetHeight);
        if(autoScroll)
            todoListDiv.scrollTo(0, todoListDiv.scrollHeight);
        autoScroll = false;
    });

    const handleSubmit = (e) => {
        if(!txtTodo) return;

        const isNotCanc = dispatch('addTodo', {
            title: txtTodo
        }, { cancelable: true});

        if(isNotCanc){
            txtTodo = '';
            console.log('não foi cancelado com prevent default');
        }

    };

    const handleRemove = (id) => {
        dispatch('removeTodo', { id });
    }

    const handleToggle = (id, value) => {
        dispatch('toggleTodo', { id, value });
    };
</script>

<h1>Todo List</h1>
{listHeight}
<div class="todo-list-wrapper">
    {#if isLoading}
        <p>Loading...</p>
    {:else if error}
        <p>{error}</p>
    {:else if todos}
        <div class="todo-list" bind:this={todoListDiv} bind:offsetHeight={listHeight} >
        {#if todos.length ===0}
            <p class="no-items-text">No todos yet</p>
        {:else}
                <ul>
                    {#each todos as t, i (t.id)}
                        {@const ix = i +1}
                        <li class:done={t.done}> 
                            <label>
                                <input 
                                    type="checkbox" 
                                    checked={t.done} 
                                    on:input={(e) => { 
                                        e.currentTarget.checked = t.done;
                                        handleToggle(t.id, !t.done);
                                        }}
                                    /> 
                                {ix} - {t.title} feito: {t.done}
                            </label>
                            <button class="remove-button" on:click={() => handleRemove(t.id)} >
                                    <span style:width="10px" style:display="inline-block">
                                        <FaRegTrashAlt />
                                    </span>
                            </button>
                        </li>
                    {/each}
                </ul>    
            {/if}
        </div>
    {/if}    
        <form  on:submit|preventDefault={handleSubmit} class="add-todo-form">
        <input class="add-todo-btn"  bind:value={txtTodo} bind:this={ctrlInput} placeholder="New todo" disabled={disableAdding}/>
        <Button disabled={!txtTodo || disableAdding}>Add</Button>
    </form>    
</div>




<style lang="scss">
    .todo-list {
        max-height: 200px;
        overflow: auto;

        ul {
            margin: 0;
            padding: 10px;
            list-style: none;
            li {
                margin-bottom: 5px;
                display: flex;
                align-items: center;
                background-color: #303030;
                border-radius: 5px;
                padding: 10px;
                position: relative;
                label {
                    cursor: pointer;
                    font-size: 18px;
                    display: flex;
                    align-items: baseline;
                    padding-right: 20px;
                    input[type='checkbox']{
                        margin: 0 10px 0 0;
                        cursor: pointer;
                    }
                }
                &.done > label {
                    opacity: 0.5;
                    text-decoration: line-through;
                }
                .remove-button{
                    border: none;
                    background: none;
                    padding: 5px;
                    position: absolute;
                    right: 10px;
                    cursor: pointer;
                    display: none;
                    :global(svg){
                        fill: #bd1414;
                    }
                }
                &:hover{
                    .remove-button{
                        display: block;
                    }
                }
            }
        }
    }

    .todo-list-wrapper{
        background-color: #424242;
        border: 1px solid #4b4b4b;

        .no-items-text{
            margin: 0;
            padding: 15px;
            text-align: center;
        }
    }


    .add-todo-form{
        padding: 15px;
        background-color: #303030;
        display: flex;
        flex-wrap: wrap;
        border-top: 1px solid #4b4b4b;
        // :global(.add-todo-btn){
        //     background-color: aqua;
        // }
        input {
            flex: 1;
            background-color: #424242;
            border: 1px solid #4b4b4b;
            padding: 10px;
            color: #fff;
            border-radius: 5px;
            margin-right: 10px;
        }
    }

    

</style>