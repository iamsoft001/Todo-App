<template>
  <div>
    <p class="blue accent-1">{{ timedate }}</p>
    <div class="home">
      <v-text-field
        v-model="newTaskTitle"
        @click:append="addTask"
        @keyup.enter="addTask"
        class="pa-3"
        outlined
        label="Add New Task"
        append-icon="mdi-plus"
        hide-details
        clearable
      ></v-text-field>
      <v-list class="pt-0" flat>
        <div v-for="task in tasks" :key="task.id">
          <v-list-item
            @click="doneTask(task.id)"
            :class="{ 'blue lighten-3': task.done }"
          >
            <template v-slot:default>
              <v-list-item-action>
                <v-checkbox
                  :input-value="task.done"
                  color="primary"
                ></v-checkbox>
              </v-list-item-action>

              <v-list-item-content>
                <v-list-item-title
                  :class="{ 'text-decoration-line-through': task.done }"
                >
                  {{ task.title }}
                </v-list-item-title>
              </v-list-item-content>
              <v-list-item-action>
                <v-btn @click.stop="deleteTask(task.id)" icon>
                  <v-icon color="blue">mdi-delete</v-icon>
                </v-btn>
              </v-list-item-action>
            </template>
          </v-list-item>
          <v-divider></v-divider>
        </div>
      </v-list>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      newTaskTitle: "",
      tasks: [
        {
          id: 1,
          title: "Wake up",
          done: false,
        },
        {
          id: 2,
          title: "Get workdone",
          done: false,
        },
        {
          id: 3,
          title: "Eat",
          done: false,
        },
      ],
      timedate: "",
    };
  },

  created() {
    setInterval(this.getNow, 1000);
  },

  methods: {
    getNow() {
      const now = new Date();

      const date =
        now.getFullYear() + "-" + (now.getMonth() + 1) + "-" + now.getDate();
      const time =
        now.getHours() + ":" + now.getMinutes() + ":" + now.getSeconds();
      const dateTime = date + " " + time;
      this.timedate = dateTime;
    },

    addTask() {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false,
      };
      this.tasks.push(newTask);
      this.newTaskTitle = "";
    },
    doneTask(id) {
      let task = this.tasks.filter((task) => task.id === id)[0];
      task.done = !task.done;
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
};
</script>
