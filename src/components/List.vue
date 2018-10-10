<template>
    <div class="list container">
        <div v-for="todo in todos" :key="todo.key" class="item">
            <div @click="makeDone(todo)" :class="[{ done: todo.done }]">{{ todo.name }}</div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

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
  methods: {
    list() {
      axios.get('http://localhost:7000/api/all')
        .then((response) => {
          this.todos = response.data;
        }).catch((error) => {
          console.log(error);
        });
    },
    makeDone(todo) {
      const todoDoned = todo;

      const URL = `http://localhost:7000/api/update/${todoDoned._id}`;
      const doneBool = todo.done ? 'false' : 'true';
      console.log('Atualizado');
      todoDoned.doneBool = true;
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
    }

    .done {
        text-decoration: line-through;
    }
</style>
