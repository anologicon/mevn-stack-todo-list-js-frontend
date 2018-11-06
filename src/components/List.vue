<template>
    <div class="list container">
        <div v-for="todo in todos" :key="todo.key" @click="makeDone(todo)" class="item">
            <div  :class="[todo.done ? 'done' : '']">{{ todo.name }}</div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import bus from './../../public/bus';

export default {
  name: 'list',
  data() {
    return {
      todos: [],
    };
  },
  mounted() {
    this.list();
  },
  created() {
    bus.$on('listar', () => {
      this.list();
    });
  },
  methods: {
    list() {
      axios.get('https://wat-do-i-do-now.herokuapp.com/api/all')
        .then((response) => {
          this.todos = response.data;
        }).catch((error) => {
          console.log(error);
        });
    },
    makeDone(todo) {
      const todoDoned = todo;

      const URL = `https://wat-do-i-do-now.herokuapp.com/api/update/${todo.id}`;
      let doneBool = false;
      if (!todo.done) {
        doneBool = true;
      }

      console.log('Atualizado');
      console.log(doneBool);
      todoDoned.done = doneBool;
      axios.post(URL, {
        done: doneBool,
        name: todoDoned.name,
      }).then(() => {
      }).catch((error) => {
        console.log(error);
      });
    },
  },
};
</script>
<style scoped>

    .item {
        display: flex;
        width: 150px;
        border: 5px solid rgba(168, 172, 255, 0.49019607843137253);; 
        -webkit-box-shadow: 
        inset 0 0 8px  rgba(0,0,0,0.1),
                0 0 16px rgba(0,0,0,0.1); 
        -moz-box-shadow: 
        inset 0 0 8px  rgba(0,0,0,0.1),
                0 0 16px rgba(0,0,0,0.1); 
        box-shadow: 
        inset 0 0 8px  rgba(0,0,0,0.1),
                0 0 16px rgba(0,0,0,0.1); 
        padding: 15px;
        background: rgba(255,255,255,0.5);
        margin: 0 0 10px 0;
        cursor: pointer;
    }

    .done {
        text-decoration: line-through;
    }
</style>
