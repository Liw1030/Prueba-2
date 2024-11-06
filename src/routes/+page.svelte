<script>
  import { writable, derived } from 'svelte/store';


  // Inicializamos una lista vacía de tareas
  let items = writable([]);

  // Variable para agregar una nueva tarea
  let newItem = '';

  //Agregar li
  function addItem() {
    if (newItem.trim()) {
      items.update(currentItems => [...currentItems, newItem]);
      newItem = '';
    }
  };

  function handleKeyPress(event) {
    if (event.key === 'Enter') {
      addItem();
    }
  };
  
  //Eliminar li
  function deleteItem(index) {
    items.update(currentItems => currentItems.filter((_, i) => i !== index));
  };

  // Sección 2 - Calificación
  let rating = null;
  
  function selectRating(value) {
    rating = value;
  };
  
  function submit() {
    if (rating !== null) {
      console.log(`Calificación enviada: ${rating}`);
    } else {
      alert('Por favor selecciona una calificación.');
    }
  };


  //Cambiar clase del body al cambiar el estado del tema
  const isDarkMode = writable(false);

  $: {
    if ($isDarkMode) {
      document.body.classList.add('dark');
      document.body.classList.remove('light');
    } else {
      document.body.classList.add('light');
      document.body.classList.remove('dark');
    }
  };


  // Filtros de tareas
  let filter = "all"; // Filtro de tareas por estado (all, active, completed)

  $: taskCount = $items.length;
  


  </script>
  
  <header>
    <h1>Todo</h1>
    <div class="theme-block">
      <input type="checkbox" id="theme" bind:checked={$isDarkMode}>
      <label for="theme"></label>
    </div>
  </header>
  
  <main>
    <button class="add_button" on:click={addItem}>Agregar</button>
    <input type="text" bind:value={newItem} placeholder="Create new todo...." on:keypress={handleKeyPress} />
  </main>
  
  <section class="section1">
    <ul>
      {#each $items as item, index}
        <li>
          <input type="checkbox" name="todoItem" class="checkbox" id="item-{index}">
          <label for="item-{index}" class="checkmark"></label>
          <p class="text">{item}</p>
          <button class="remove" on:click={() => deleteItem(index)}>Click</button>
        </li>
      {/each}
    </ul>

    <div class="bottom-items">
      <!-- Contador de tareas filtradas -->
      <div class="item-left">
        <span>{taskCount}</span> task{taskCount !== 1 ? 's' : ''}
      </div>
  
      <!-- Filtros -->
      <div class="filter flex-row">
        <label>
          <input type="radio" name="filter" id="all" value="all" bind:group={filter}>
          <span>All</span>
        </label>
        <label>
          <input type="radio" name="filter" id="active" value="active" bind:group={filter}>
          <span>Active</span>
        </label>
        <label>
          <input type="radio" name="filter" id="completed" value="completed" bind:group={filter}>
          <span>Completed</span>
        </label>
      </div>

      <span>Clear completed</span>
    </div>

  </section>
  
  <section class="content-2 ligth">
    <div class="container">
      <div class="head">
        <img src="../src/public/images/icon-star.svg" class="icon" alt="Star Icon">
        <h2>How did we do?</h2>
      </div>
  
      <div class="text">
        <p>Please let us know how we did with your support request.
          All feedback is appreciated to help us improve our offering!</p>
      </div>
  
      <div class="button">
        {#each [1, 2, 3, 4, 5] as number}
          <button 
            class="number" 
            class:selected={rating === number} 
            on:click={() => selectRating(number)}
          >
            {number}
          </button>
        {/each}
      </div>
      
      <button class="sub" on:click={submit}>Submit</button>
    </div>
</section>
  
  
  <footer>
    <h2>Hecho por Johana Usaquen - FrontendMentor</h2>
  </footer>

<style>
    :root {
      --bag-top-image: url("/src/public/images/bg-desktop-dark.jpg");
      --main-bg: hsl(235, 21%, 11%);
      --todo-bg: hsl(235, 24%, 19%);
      --todo-shadow: hsl(235, 21%, 11%);
      --font-color: hsl(234, 39%, 85%);
      --font-color-hover: #FFF;
      --button-bg: hsl(234, 13%, 28%);
      --main-bg-lg: hsl(0, 0%, 98%);
      --todo-bg-lg: hsl(0, 0%, 98%);
      --todo-shadow-lg: hsl(235, 9%, 61%);
      --font-color-lg: hsl(235, 9%, 61%);
      --font-color-hover-lg: hsl(235, 21%, 11%);
      --Orange: hsl(25, 97%, 53%);
      --White: hsl(0, 0%, 100%);
      --Light-Grey: hsl(217, 12%, 63%);
      --Dark-Blue: hsl(213, 19%, 18%);
      --Very-Dark-Blue: hsl(216, 12%, 8%);
}

:global(body) {
    transition: background-color 0.3s ease, color 0.3s ease;
    background-color: var(--main-bg);
    color: var(--font-color);
}

:global(.light) {
    background-color: var(--main-bg-lg);
    color: var(--font-color-lg);

    main{
      background: var(--todo-bg-lg);
      color: var(--font-color-lg);
      input{
        background: transparent;
        color: var(--font-color-lg);
      }
    }

    section.section1{
      background: var(--todo-bg-lg);
      border: 1px solid var(--Light-Grey);
      li{
        color: var(--font-color-lg);
      }
    }

    section.content-2{
      background: var(--todo-bg-lg);
      border: 1px solid var(--Light-Grey);
      h2{
        color: rgba(0, 0, 0, 0.884);
      }
    }
}



header{
        display: flex;
        width: 100%;
        max-width: 545px;
        align-items: center;
        justify-content: space-between;

        h1{
          color: #FFF;
        }
}

.theme-block{
  margin: -10px 0 0;
}

.theme-block #theme{
  display: none;
}

.theme-block #theme + label::before{
  content: "";
  background: url('../public/images/icon-moon.svg');
  display: block;
  width: 26px;
  height: 26px;
  cursor: pointer;
}

.theme-block #theme:checked + label::before{
  background: url('../public/images/icon-sun.svg');
}

main {
      display: flex;
      background: var(--todo-bg);
      width: 100%;
      max-width: 545px;
      padding-block: 20px;
      border-radius: 5px;
}
  
input {
      background: var(--todo-bg);
      border: none;
      padding: 0 10px;
      color: var(--font-color);
      outline: none;
}
  
.add_button {
      border: 1px solid var(--font-color);
      border-radius: 50px;
      background: transparent;
      color: transparent;
      width: 20px;
      cursor: pointer;
      margin: 0 15px 0 20px;
}
  
.add_button:hover {
      background: center url('../public/images/icon-check.svg') no-repeat,
        linear-gradient(135deg, hsl(192, 100%, 67%), hsl(200, 87%, 65%));
}
  
ul {
      list-style: none;
      padding: 0;
      margin: 0;
}
  
  
li {
      display: flex;
      align-items: center;
      padding: 0 10px 0 0px;
      border-bottom: 1px solid var(--font-color);
      position: relative;
      justify-content: flex-start;
}
  
li:hover .remove {
      display: flex;
}
  
button.remove {
      background: url('../public/images/icon-cross.svg');
      width: 18px;
      height: 18px;
      cursor: pointer;
      position: absolute;
      color: transparent;
      right: 25px;
      border: none;
      display: none;
}
  
section.section1 {
      display: flex;
      flex-direction: column;
      background: var(--todo-bg);
      width: 100%;
      max-width: 545px;
      margin-top: 20px;
      border-radius: 5px;
}

.checkmark {
  top: -5px;
  left: 0;
  height: 20px;
  width: 20px;
  margin: 0 15px 0 20px;
  position: relative;
}

input.checkbox {
  display: none;
}

ul li input[type="checkbox"] + .checkmark { /* Cambié ~ por + para seleccionar el hermano directo */
  border: 1px solid var(--font-color);
  border-radius: 50%;
}

ul li input[type="checkbox"] + .checkmark:hover {
  border: 0;
  padding: 1px;
  background: center url('../public/images/icon-check.svg') no-repeat,
        linear-gradient(135deg, hsl(192, 100%, 67%), hsl(200, 87%, 65%));
}

ul li input[type="checkbox"] + .checkmark:hover::before {
  content: "";
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: block;
}

ul li input[type="checkbox"]:checked + .checkmark {
  background: center url('../public/images/icon-check.svg') no-repeat,
        linear-gradient(135deg, hsl(192, 100%, 67%), hsl(200, 87%, 65%));
}

ul li input[type="checkbox"]:checked ~ .text {
  text-decoration: line-through;
}


button.number.selected {
      background-color: var(--Orange);
      color: white;
}
  
.content-2 {
      width: 100%;
      max-width: 545px;
      background: var(--todo-bg);
      border-radius: 5px;
      margin: 10% auto;
      padding: 20px;
      border-radius: 10px;
}
  
.container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
}
  
.head {
      padding: 0 15px;
      display: flex;
      align-items: flex-start;
      width: 100%;
      gap: 30px;
      flex-direction: column;
}
  
img.icon {
      background: var(--Light-Grey);
      padding: 12px;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      margin-top: 20px;
}
  
h2 {
      color: var(--font-color);
      font-size: 1.5em;
      margin: 0;
}
  
p {
      padding: 0 15px;
      text-align: center;
}
  
.button {
      display: flex;
      gap: 10px;
      justify-content: center;
}
  
button.number {
      background: var(--Light-Grey);
      border: none;
      border-radius: 50%;
      width: 45px;
      height: 45px;
      cursor: pointer;
      font-size: 16px;
      margin: 0 20px;
}
  
button.number:hover {
      filter: brightness(80%);
}
  
button.sub {
      width: 100%;
      background-color: var(--Orange);
      border: none;
      height: 40px;
      border-radius: 5px;
      color: white;
      font-weight: bold;
      cursor: pointer;
      margin: 20px 0;
}
  
button.sub:hover {
      filter: brightness(80%);
}
  
footer {
      background: rgba(0, 0, 0, 0.5);
      font-size: 6px;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 10px 0;
      height: 50px;
      width: 100%;
}

div.bottom-items{
    display: flex;
    padding: 20px;
    justify-content: space-around;
}

</style>
  