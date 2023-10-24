``
<template>
  <p class="title">Users</p>
  <p class="sub-title">{{ filter.length }}</p>

  <input placeholder="Type here..." class="searchbar" v-model="input" />

  <table class="list" v-if="filter.length > 0">
    <tr class="table-row" v-for="user in filter" :key="user.id">
      <td class="table-title" @click="nameClick(user)">
        {{ user.name }}
      </td>
      <td>{{ user.url }}</td>
      <td>
        <button class="btn" @click="deleteUser(user)">Delete</button>
      </td>
    </tr>
  </table>

  <p v-else>Not Found</p>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      input: "",
    };
  },

  watch: {
    users(previous, next) {
      if (previous !== next) {
        localStorage.setItem("users", JSON.stringify(this.users));
      }
    },
  },

  mounted() {
    if (
      !JSON.parse(localStorage.getItem("users")) ||
      !JSON.parse(localStorage.getItem("users")).length
    ) {
      fetch("https://pokeapi.co/api/v2/pokemon")
        .then((r) => r.json())
        .then((data) => {
          data.results.forEach((el, index) => (el.id = index));
          this.users = data.results;
        });
      return;
    }

    this.users = JSON.parse(localStorage.getItem("users"));
  },

  methods: {
    deleteUser(post) {
      this.users = this.users.filter((el) => el.id !== post.id);
    },
    nameClick(post) {
      this.input = post.name;
    },
  },

  computed: {
    filter() {
      return this.users.filter((el) =>
        el.name.toLowerCase().includes(this.input.toLowerCase())
      );
    },
  },
};
</script>

<style>
body {
  padding: 40px;
}

.title {
  margin-bottom: 20px;
  font-size: 60px;
}

.sub-title {
  margin-bottom: 20px;
  font-size: 30px;
}

.searchbar {
  width: 100%;
  padding: 24px;
  border-radius: 10px;
  margin-bottom: 20px;
  font-size: 20px;
  border: 1px solid #bbbbbb;
}

.list {
  margin: auto;
  width: 100%;
}

.table-row {
  justify-content: space-between;
  display: flex;
  padding: 14px;
}

.table-title {
  cursor: pointer;
  font-size: 30px;
  font-weight: 500;
}

.btn {
  cursor: pointer;
  padding: 10px;
  border: none;
}
</style>
