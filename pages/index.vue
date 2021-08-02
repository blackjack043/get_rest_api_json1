<template>
  <div>
    <button @click="zapros">получить данные</button>
    <button @click="unic">высчитать сотрудников</button>
    <button @click="filtr">Вывести значения</button>

    <ul>
      <li v-for="user in myUsers" class="checkbox">
        <input type="checkbox" id="" v-model="checked" v-bind:value="user" />
        <label for="checkbox">{{ user }}</label>
      </li>
    </ul>

    <!--     <table v-show="myUsers">
      <thead>
        <tr>
          <th>id</th>
          <th>UserID</th>
          <th>Task</th>
          <th>Completed</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in body" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.userId }}</td>

          <td>{{ user.title }}</td>
          <td>{{ user.completed }}</td>
        </tr>
      </tbody>
    </table> -->
    <table v-show="myUsers">
      <thead>
        <tr>
          <th>id</th>
          <th>UserID</th>
          <th>Task</th>
          <th>Completed</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in filterBody" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.userId }}</td>

          <td>{{ user.title }}</td>
          <td>{{ user.completed }}</td>
        </tr>
      </tbody>
    </table>

    <!-- classic design -->
  </div>
</template>

<script>
import { mapMutations } from "vuex";
import axios from "axios";

//import vue from "vue";
export default {
  data() {
    return {
      checked: [],
      filterBody: [],
      search: "aa",
      myUsers: "",
      newUsers: [],
      columns: ["name", "age"],
      tasks: [],
      body: "",
      body2: "",
      id: 1,
      user: [],
    };
  },

  computed: {
    todos() {
      return this.$store.state.todos.list;
    },
  },

  methods: {
    addTodo(event) {
      this.$store.commit("todos/add", event.target.value);
      event.target.value = "";
    },
    ...mapMutations({
      toggle: "todos/toggle",
    }),
    removeTodo(todo) {
      this.$store.commit("todos/remove", todo);
    },
    unic() {
      let userArray = this.body.map(function (item) {
        return item.userId;
      });
      console.log(userArray);
      this.myUsers = Array.from(new Set(userArray)); //получаем уникальные элементы массива
      console.log(this.myUsers);
    },
    filtr() {
      console.log(this.checked);
      var a2 = this.checked;
      this.filterBody = this.body.filter(function (itemBody) {
        return (
          a2.filter(function (itemUser) {
            return itemBody.userId == itemUser;
          }).length || 0
        );
      });
    },

    zapros(event) {
      axios
        .get("https://jsonplaceholder.typicode.com/todos")
        .then((response) => {
          this.body = response.data;
          alert("данные получены");
        });
    },

    zapros2(event) {
      const url1 =
        "https://jsonplaceholder.typicode.com/todos?userId=" + this.id + "";
      axios.get(url1).then((response) => (this.body2 = response.data));
    },
    zapros3(event) {
      axios
        .get(
          "https://don16obqbay2c.cloudfront.net/frontend-test-task/gallery-images.json"
        )
        .then((response) => (this.body = response.data));
    },
    mounted() {
      //this.zapros;
      console.log(this.zapros);
    },
  },
};
</script>

<style scoped>
body {
}

div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

ul {
  display: inline-flex;
  flex-direction: column;
  align-items: flex-start;
}

li {
  display: flex;
  align-items: center;
  margin-bottom: 0.5rem;
}

input[type="checkbox"] {
  margin: 0.5rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
}

.done {
  text-decoration: line-through;
}

ul {
  display: inline;
  justify-content: space-between;
  padding: 0;
  margin: 0;
}
li.checkbox {
  display: inline-block;
  position: relative;
  margin: 0;
  padding: 0;
}
</style>




  <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input
          :checked="todo.done"
          @change="toggle(todo)"
          type="checkbox"
          :id="todo.id"
        />
        <label :class="{ done: todo.done }" :for="todo.id">{{
          todo.text
        }}</label>
        <button @click="removeTodo(todo)">remove</button>
      </li>
    </ul>

        <input
      type="text"
      @keyup.enter="zapros"
      placeholder="What needs to be done?"
    />