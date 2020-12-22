<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        class="col"
        square
        bg-color="white"
        placeholder="Añade una tarea"
        dense
      >
        <template v-slot:append>
          <q-btn @keyup.enter="addTask" @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{'done bg-blue-1' : task.done}"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" class="no-poiner-events" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center">
      <div class="text-h5 text-primary text-center">No hay tareas pendientes</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        /*  {
          title: "Banana",
          done: false
        },
        {
          title: "Dormir",
          done: false
        },
        {
          title: "Comer",
          done: false
        } */
      ]
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Eliminar",
          message: "Seguro que quieres eliminarlo?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Haz eliminado una tarea");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      this.newTask = "";
    }
  }
};
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>