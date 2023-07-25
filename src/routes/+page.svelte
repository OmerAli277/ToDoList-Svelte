<script lang="js">
  import ToDO from "../lib/todo.svelte";

  let countID = 2;
  let toDoList = [
    {
      id: 1,
      title: "Sample List",
      toDoItems: [
        "Team meeting to discuss project updates.",
        "Prioritize tasks for the week.",
        "Start frontend website redesign.",
      ],
    },
  ];
  let activeToDo;
  let activeIndex;
  $: toDoList[activeIndex] = activeToDo;

  function deleteToDo () {
    let copyList = [...toDoList];
    copyList.splice(activeIndex, 1);
    toDoList = copyList;
    activeToDo = undefined;
    activeIndex = undefined;
  }
</script>

<h1>Welcome to ToDo List</h1>
<div class="main-container">
  <div class="list-container">
    {#each toDoList as toDoObject, index}
      <p
        class={activeToDo && activeToDo.id === toDoObject.id ? 'active' : ''}
        on:click={() => {
          activeToDo = toDoObject;
          activeIndex = index;
        }}
      >
        {toDoObject.title ? toDoObject.title : "Not set"}
      </p>
    {/each}
    <p
      class="plus"
      on:click={() => {
        let sampleToDo = {
          id: countID++,
          title: "",
          toDoItems: [],
        };
        activeToDo = sampleToDo;
        toDoList = [...toDoList, sampleToDo];
        activeIndex = toDoList.length-1
      }}
    >
      +
    </p>
  </div>
  {#if activeToDo}
    <ToDO bind:toDoObject={activeToDo} on:delete={deleteToDo}/>
  {:else}
    <p class="no-selected">No TODO to show</p>
  {/if}
</div>

<style lang="scss">
  h1 {
    margin: 0 auto;
    text-align: center;
    margin-bottom: 20px;
  }
  .main-container {
    display: flex;
    gap: 10px;
    margin: 0 auto;
    width: 1000px;
  }
  .list-container {
    display: flex;
    flex-direction: column;
    gap: 12px;

    min-width: 400px;

    p {
      font-size: 1.69em;
      font-weight: bold;
      border: 1px solid #d0cfcf;
      border-radius: 10px;
      padding: 12px 20px;
      margin: 0;
      cursor: pointer;
    }

    p.active {
      background: #000;
      color: #fff;
    }

    p.plus {
      text-align: center;
      cursor: pointer;
    }
  }
  .no-selected {
    width: 600px;
    text-align: center;
    font-size: 18px;
  }
</style>
