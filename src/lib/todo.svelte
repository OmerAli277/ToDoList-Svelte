<script lang="js">
  import { createEventDispatcher } from 'svelte';


  export let toDoObject = {
    title: "",
    toDoItems: [],
  };

  const dispatch = createEventDispatcher();

  let nextItem = "";

  function deleteObj () {
    dispatch('delete');
  }
</script>

<div class="main">
  <div class="todo-container title">
    <input
      class="input-title"
      bind:value={toDoObject.title }
      placeholder="ToDo Title"
    />
  </div>
  <div class="todo-container">
    {#each toDoObject.toDoItems as tdItem, index}
      <div class="td-item">
        <span>•</span><input
          bind:value={tdItem}
        />
        <button
          class="cross"
          on:click={() => {
            let copyList = [...toDoObject.toDoItems];
            copyList.splice(index, 1);
            toDoObject.toDoItems = copyList;
          }}>&#x2715</button
        >
      </div>
    {/each}
    <div class="next-container">
      <input
        bind:value={nextItem}
        placeholder="Next item"
        on:keydown={(e) => {
          if (e.code === "Enter" && nextItem) {
            toDoObject.toDoItems = [...toDoObject.toDoItems, nextItem];
            nextItem = "";
          }
        }}
      />
      <button
        disabled={!nextItem}
        on:click={() => {
          toDoObject.toDoItems = [...toDoObject.toDoItems, nextItem];
          nextItem = "";
        }}>Add to list</button
      >
    </div>
  </div>
  <div class="btn-container">
    <button class="btn btn-remove" on:click={deleteObj}>
      delete
    </button>
  </div>
</div>

<style lang="scss">
  .main {
    width: 600px;
  }
  .todo-container {
    padding: 25px;
    border: 1px solid #d0cfcf;
    border-radius: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;

    .input-title {
      border: none;
      border-radius: 10px;
      padding: 0px;

      font-size: 1.69em;
      font-weight: bold;
    }

    .td-item {
      margin: 0;
      display: flex;
      align-items: center;
      gap: 6px;
      padding: 5px 15px;
      border-radius: 10px;
      background: #f2f2f2;

      input {
        flex: 1;
        border: none;
        padding: 4px 8px;

        font-size: 16px;
        line-height: 18px;
        background: #f2f2f2;
      }

      .cross {
        display: none;
        cursor: pointer;
        background: none;
        border: none;
      }

      &:hover {
        .cross {
          display: block;
        }
      }
    }

    .next-container {
      display: flex;
      align-items: center;
      gap: 6px;

      input {
        flex: 1;
        border: 1px solid #999999;
        border-radius: 10px;
        padding: 9px 23px;

        font-size: 16px;
        line-height: 18px;
      }

      button {
        margin-left: 10px;
        padding: 9px;

        font-size: 16px;
        line-height: 18px;
        border-radius: 10px;

        background: #000;
        color: #fff;
        cursor: pointer;
      }
    }
  }
  .todo-container.title {
    padding: 14px 25px;
  }
  .btn-container {
    max-width: 700px;

    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: 10px;

    border-radius: 20px;
    margin: 0 auto;
    margin-bottom: 20px;

    .btn {
      width: 100px;
      padding: 9px 15px;

      font-size: 16px;
      line-height: 18px;
      border-radius: 10px;

      background: #000;
      border: none;
      color: #fff;
      cursor: pointer;
    }

    .btn.btn-cancel {
      background: #cfcfcf;
      color: #000;
    }

    .btn.btn-remove {
      background: #c30010;
      color:#fff;
    }
  }
</style>
