<template>
  <div id="list">
    <!-- {{ todolist }} -->
    <ul>
      <li v-for="(list, index) in todolist.list" v-bind:key="list.id">
        {{ list }}<button v-bind:data-index="index" v-on:click="removeList">delete</button>
      </li>
    </ul>
    <div id="input-area">
      <input type="text" id="input-text" v-model="inputText" v-on:keyup.enter="addList">
      <button v-on:click="addList">add</button>
    </div>
  </div>
</template>

<script>
import { db } from './firebaseset';
export default {
  name: 'HelloWorld',
  data() {
    return {
      inputText: '',
      todolist: {
        list: []
      }
    }
  },
  mounted() {
    db.collection("todolist").get().then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
        this.todolist.list = doc.data().list;
      });
    });
  },
  methods: {
    updatelist:function(){
      db.collection("todolist").get().then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          doc.ref.update(this.todolist);
        });
      });
    },
    addList: function () {
      this.todolist.list.push(this.inputText);
      this.updatelist();
      this.inputText = '';
    },
    removeList: function (e) {
      let indexElem = e.target.getAttribute('data-index');
      this.updatelist(); 
      this.todolist.list.splice(indexElem,1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
#list{
  max-width: 500px;
  width: 98vw;
  margin: 0 auto;
  border-radius: 5px;
  li { 
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 21x;
    color: white;
    padding: 5px;
    border-radius: 5px;
    box-sizing: border-box;
    background: #B0A8B9;
    margin-bottom: 10px;
  }
  #input-area {
    display: flex;
    justify-content: space-between;
    input {
      font-family: 'TTWanjudaedunsancheB';
      flex-grow: 1;
      height: 50px;
      padding: 0 0 0 10px;
      border-radius: 5px 0 0 5px;
      border: none;
      outline: none;
      box-sizing: border-box;
    }
    button {
      height: 50px;
      width: 80px;
      background: #845EC2;
      border-radius: 0 5px 5px 0;
    }
  }
  button {
    font-family: 'Sigmar', cursive;
    height: 30px;
    color: white;
    text-transform: uppercase;
    padding: 0 5px;
    border-radius: 5px;
    background: #FF8066;
    background-image: none;
    outline: none;
    border: 0;
  }
}
</style>
