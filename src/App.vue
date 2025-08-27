<script setup>
import {reactive} from "vue";
const estado = reactive({
	filtro: "todas",
	tarefa: [
		{
			titulo: "Estudar CSS",
			concluida: false,
		},
		{
			titulo: "Estudar Vue.js",
			concluida: true,
		},
		{
			titulo: "Estudar JavaScript",
			concluida: false,
		},
	],
});
const getTarefasPendentes = () => {
	return estado.tarefa.filter((tarefa) => !tarefa.concluida);
};
const getTarefasConcluidas = () => {
	return estado.tarefa.filter((tarefa) => tarefa.concluida);
};
const getTarefasFiltradas = () => {
	const {filtro} = estado;
	switch (filtro) {
		case "pendentes":
			return getTarefasPendentes();
		case "concluidas":
			return getTarefasConcluidas();
		default:
			return estado.tarefa;
	}
};
</script>

<template>
	<div class="container">
		<header class="p-5 mb-4 mt-4 bg-light rounded-3 shadow-sm">
			<h1>Minhas tarefas</h1>
			<p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
		</header>
		<form action="">
			<div class="row">
				<div class="col">
					<input type="text" class="form-control" placeholder="Digite uma tarefa" />
				</div>
				<div class="col-md-2">
					<button type="submit" class="btn btn-primary">Adicionar</button>
				</div>
				<div class="col-md-2">
					<select @change="(e) => (estado.filtro = e.target.value)" class="form-select">
						<option value="todas">Todas tarefas</option>
						<option value="pendentes">Pendentes</option>
						<option value="concluidas">Concluídas</option>
					</select>
				</div>
			</div>
		</form>
		<ul class="list-group mt-4">
			<li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
				<input
					@change="(e) => (tarefa.concluida = e.target.checked)"
					:checked="tarefa.concluida"
					:id="tarefa.titulo"
					type="checkbox"
				/>
				<label :class="{done: tarefa.concluida}" class="ms-3" :for="tarefa.titulo">{{
					tarefa.titulo
				}}</label>
			</li>
		</ul>
	</div>
</template>

<style scoped>
.done {
	text-decoration: line-through;
}
</style>
