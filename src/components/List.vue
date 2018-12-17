<template>
    <div class="list container">
        <div v-for="todo in todos" :key="todo.key">
            <div class="container-itens">
              <div :class="[todo.done ? 'done' : '']"  class="item"  @click="makeDone(todo)">{{ todo.name }}</div>
              <div class="remover" @click="remove(todo)"><i class="fas fa-times" style="font-size: 1.5em; color: Tomato;"></i></div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import bus from './../../public/bus';
import url from './../../public/baseUrl';

console.log(url);

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
      axios.get(`${url}/api/all`)
        .then((response) => {
          this.todos = response.data;
        }).catch((error) => {
          console.log(error);
        });
    },
    remove(todo) {
      const URL = `${url}/api/delete/${todo.id}`;

      axios.delete(URL)
        .then((response) => {
          const resposta = response.data;
          if (resposta.type) {
            this.list();
          }
        }).catch((error) => {
          console.log(error);
        });
    },
    makeDone(todo) {
      const todoDoned = todo;

      const URL = `${url}/api/update/${todo.id}`;
      let doneBool = false;
      if (!todo.done) {
        doneBool = true;
      }

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

    .remover {
        display: flex;
        width: auto;
        border: 5px solid rgba(255, 151, 151, 0.49019607843137253); 
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
        margin: 0 5px 10px 0;
        cursor: pointer;
        align-content: center;
    }

    .done {
        text-decoration: line-through;
    }
</style>
