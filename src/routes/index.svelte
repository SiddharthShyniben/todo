<script>
	import { onMount } from "svelte";

	let todos = [];
	let completedTodos = [];

	onMount(() => {
		todos = JSON.parse(localStorage.todos || '[]');
		completedTodos = JSON.parse(localStorage.completedTodos || '[]');
	});

	function cache() {
		localStorage.todos = JSON.stringify(todos);
		localStorage.completedTodos = JSON.stringify(completedTodos);
	}

	function handleSubmit() {}

	function todo(event) {
		if (event.code === 'Enter') {
			event.preventDefault();

			todos.push(event.target.value);
			todos = todos; // wtf

			event.target.value = '';

			cache()

			return false;
		}
	}	

	function deleteTodo(i) {
		return function() {
			todos.splice(i, 1);
			todos = todos;
			cache()
		}
	}

	function deleteCompletedTodo(i) {
		return function() {
			completedTodos.splice(i, 1);
			completedTodos = completedTodos;
			cache()
		}
	}

	function completeTodo(i) {
		return function() {
			completedTodos = [...completedTodos, todos[i]];
			todos.splice(i, 1);
			todos = todos;
			cache()
		}
	}

	function uncompleteTodo(i) {
		return function() {
			todos = [...todos, completedTodos[i]];
			completedTodos.splice(i, 1);
			completedTodos = completedTodos;
			cache()
		}
	}
</script>

<div class="container mt-5">
	<h1 class='title'>Todo</h1>

	<div class="box">
		<form on:submit|preventDefault={handleSubmit}>
			<div class="field">
				<label class="label">Add a todo</label>
				<div class="control">
					<input class="input" type="text" placeholder="ex. Wash the car" on:keyup|preventDefault={todo}>
				</div>
			</div>
		</form>
	</div>

	<div class="box">
		<h2 class='subtitle'>Stuff to do</h2>

		{#if todos.length === 0}
			<div class="notification"><em>You have no todos. Go make one!</em></div>
		{/if}

		{#each todos as todo, i}
			<div class="level box">
				<div class="level-left content vcenter">{todo}</div>
				<div class="level-right">
					<button class="button" on:click={completeTodo(i)}>
						<span class="icon">
							<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="green">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
							</svg>
						</span>
					</button>
					<button class="button" on:click={deleteTodo(i)}>
						<span class="icon">
							<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="red">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
							</svg>
						</span>
					</button>
				</div>
			</div>
		{/each}
	</div>

	<div class="box">
		<h2 class='subtitle'>Stuff done</h2>

		{#if completedTodos.length === 0}
			<div class="notification"><em>You have no completed todos. Go work!</em></div>
		{/if}

		{#each completedTodos as todo, i}
			<div class="level box">
				<div class="level-left content vcenter">{todo}</div>
				<div class="level-right">
					<button class="button" on:click={uncompleteTodo(i)}>
						<span class="icon">
							<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="green">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18.364 18.364A9 9 0 005.636 5.636m12.728 12.728A9 9 0 015.636 5.636m12.728 12.728L5.636 5.636" />
							</svg>
						</span>
					</button>
					<button class="button" on:click={deleteCompletedTodo(i)}>
						<span class="icon">
							<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="red">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
							</svg>
						</span>
					</button>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	@import "https://cdn.jsdelivr.net/npm/bulma@0.9.3/css/bulma.min.css";
	@import "https://unpkg.com/bulma-prefers-dark";

	.vcenter {
		margin-bottom: 0;
	}
</style>
