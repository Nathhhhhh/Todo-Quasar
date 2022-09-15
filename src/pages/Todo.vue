<!-- eslint-disable no-undef -->
<template>
  <q-page class="bg-grey-3 column">
    <div class="flex row q-pa-sm bg-secondary">
      <q-input
        v-model="newTask"
        bg-color="white"
        label="Ajouter une tâche"
        class=" col"
        dense
        @keypress.enter="addTask">
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class=" bg-green-2" bordered separator>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{'done bg-blue-1' : task.done}"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class=" no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          @click.stop="deleteTask(index)"
          side
        >
        <q-btn round flat color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div
    class="no-tasks absolute-center"
    v-if="tasks.length === 0"
    >
      <div class="text-center text-primary text-h5 column">
        <q-icon
        name="check"
        size="100px"
        color="primary"
        />
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Voulez vous vraiment supprimer cette tâche ?',
        cancel: true,
        persistent: true,
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify({
          message: 'Tâche supprimée !',
          color: 'green',
        });
      }).onCancel(() => {
        this.$q.notify({
          message: 'Tâche non supprimée !',
          color: 'red',
        });
      });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = '';
    },
  },
  data() {
    return {
      tasks: [
        // {
        //   title: 'Exercice 1 PHP',
        //   done: false,
        // },
        // {
        //   title: 'Faire la vaiselle',
        //   done: false,
        // },
        // {
        //   title: 'Appeler Younap',
        //   done: false,
        // },
      ],
      newTask: '',
    };
  },
  name: 'IndexPage',
});
</script>
<style lang="scss">
  .done {
    .q-item__label{
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-tasks{
    opacity: .5;
  }
</style>
