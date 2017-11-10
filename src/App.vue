<template>
  <div>
    <h1>{{title}}</h1>
    <input v-model ="newItem"   v-on:keyup.enter="addNew">
     <ul>
       <li v-for="item in items"  v-bind:class="{finished:item.isFinished}"  v-on:click="toggleFinish(item)">
         {{ item.label}}
       </li>

     </ul>
  </div>
</template>

<script>
import Store from "./store";

export default {
  data: function() {
    return {
      title: "this is a todo list!",
      items: Store.fetch(),
      newItem: ""
    };
  },
  watch: {
    items: {
      handler: function(items) {
        Store.save(items);
      },
      deep: true
    }
  },
  methods: {
    toggleFinish: function(item) {
      item.isFinished = !item.isFinished;
    },
    addNew: function(){
      this.items.push({
        label: this.newItem,
        isFinished: false
      });
      // console.log(this.newItem);
      this.newItem = "";
      Store.save();
    }
  }
};
</script>

<style lang="scss">
.finished {
  text-decoration: underline;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
