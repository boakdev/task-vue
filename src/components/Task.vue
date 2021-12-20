<template>
  <div id="task">
    <form @submit.prevent="addTarefa">
      <input
        v-model="tarefa"
        type="text"
        placeholder="Escreva aqui sua tarefa"
      />

      <button type="submit">Adicionar</button>
    </form>

    <Item :lista="listaTarefas" :delete="deleteTask" />

    <span v-show="listaTarefas.length > 0">
      Você tem
      <strong :class="{ pend: pendente }"> {{ listaTarefas.length }}</strong>
      pendentes
    </span>
  </div>
</template>

<script>
import Item from "./Item.vue";

export default {
  name: "Task",
  components: {
    Item,
  },
  data() {
    return {
      tarefa: "",
      listaTarefas: [],
      pendente: false,
    };
  },
  methods: {
    addTarefa() {
      if (this.tarefa != "") {
        this.listaTarefas.push({
          text: this.tarefa,
          key: Date.now(),
        });
      } else {
        alert("Digite uma tarefa");
        return;
      }
      this.tarefa = "";
      console.log(this.listaTarefas);
    },

    deleteTask(key) {
      let filtro = this.listaTarefas.filter((item) => {
        return item.key != key;
      });
      return (this.listaTarefas = filtro);
    },
  },
  watch: {
    listaTarefas: {
      deep: true,
      handler() {
        localStorage.setItem("tasks", JSON.stringify(this.listaTarefas));
        this.listaTarefas.length > 4
          ? (this.pendente = true)
          : (this.pendente = false);
      },
    },
  },
  // Chamado de forma automática após criação do Vue
  created() {
    const listaJson = localStorage.getItem("tasks");
    this.listaTarefas = JSON.parse(listaJson) || [];
  },
};
</script>

<style scoped>
#task {
  max-width: 700px;
  background: #fff;
  border-radius: 5px;
  padding: 20px;
  margin: 20px auto;
  box-shadow: 0 0 20px rgb(0, 0, 0.3);
}

form {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
}

form button {
  cursor: pointer;
  background: #0f5959;
  border: 0;
  border-radius: 4px;
  margin-left: 10px;
  padding: 0 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
}

input {
  flex: 1;
  border: 1px solid #eee;
  padding: 6px 10px;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.pend {
  color: #ff0000;
}
</style>