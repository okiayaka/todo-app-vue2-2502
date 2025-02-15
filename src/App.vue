<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6"> Menu </v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" :to="item.to" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar dense height="56" style="max-height: 56px">
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <div>
        <v-text-field
          v-model="newTaskTitle"
          class="mt-3 ml-3"
          label="Add Task"
          @keyup.enter="addTask"
          clearable
          hide-details
          counter
          maxlength="20"
          prepend-icon="mdi-plus"
        ></v-text-field>

        <v-list>
          <div v-for="task in tasks" :key="task.id">
            <v-list-item
              @click="doneTask(task.id)"
              :class="{ 'blue lighten-5': task.done }"
            >
              <v-list-item-action>
                <v-checkbox :input-value="task.done" readonly></v-checkbox>
              </v-list-item-action>

              <v-list-item-content>
                <v-list-item-title
                  :class="{ 'text-decoration-line-through': task.done }"
                >
                  {{ task.title }}
                </v-list-item-title>
              </v-list-item-content>

              <v-list-item-icon>
                <v-icon color="primary" @click.stop="deleteTask(task.id)">
                  mdi-delete
                </v-icon>
              </v-list-item-icon>
            </v-list-item>
            <v-divider></v-divider>
          </div>
        </v-list>
      </div>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";

interface Task {
  id: number;
  title: string;
  done: boolean;
}

interface MenuItem {
  title: string;
  icon: string;
  to: string;
}

export default Vue.extend({
  data(): {
    drawer: boolean;
    newTaskTitle: string;
    items: MenuItem[];
    tasks: Task[];
  } {
    return {
      drawer: false,
      newTaskTitle: "",
      items: [
        { title: "Todo", icon: "mdi-format-list-checks", to: "/" },
        { title: "About", icon: "mdi-help-box", to: "/about" },
      ],
      tasks: [
        { id: 1, title: "起きる", done: false },
        { id: 2, title: "着替える", done: false },
      ],
    };
  },
  methods: {
    doneTask(id: number): void {
      const task = this.tasks.find((t) => t.id === id);
      if (task) {
        task.done = !task.done;
      }
    },
    deleteTask(id: number): void {
      const index = this.tasks.findIndex((task) => task.id === id);
      if (index !== -1) {
        this.tasks.splice(index, 1);
      }
    },
    addTask(): void {
      if (this.newTaskTitle.trim() !== "") {
        this.tasks.push({
          id: Date.now(),
          title: this.newTaskTitle,
          done: false,
        });
        this.newTaskTitle = "";
      }
    },
  },
});
</script>
