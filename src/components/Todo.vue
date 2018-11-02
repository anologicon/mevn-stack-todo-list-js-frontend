<template>
  <div class="todo">
    <h1>What i do now?</h1>
    <div class="container">
      <div class="box-input">
        <form @submit.prevent="save">
          <input type="text" placeholder="I have make a ..." v-model="todoName" id="todo-name" @keypress="typing = true">
          <span class="typing-press" v-show="typing">Press enter to save ...</span>
        </form>
        <list></list>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import List from '@/components/List';
import bus from './../../public/bus';

export default {
  name: 'todo',
  data() {
    return {
      todoName: '',
      typing: false,
    };
  },
  methods: {
    save() {
      axios.post('https://what-i-todo-now.herokuapp.com/api/add', {
        name: this.todoName,
        done: false,
      }).then(() => {
        this.typing = false;
        this.todoName = '';

        bus.$emit('listar');
      }).catch((error) => {
        console.log(error);
      });
    },
  },
  components: {
    List,
  },
};
</script>

<style>

  h1 {
    font-weight: 400;
    font-size: 2.2em;
    color: rgba(0, 0, 0, 0.69);
  }

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .item {
    padding: 20px;
  }

  input {
    display: flex;
    width: 300px;
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
  
  .typing-press {
    font-style: italic;
    font-size: 0.8em;
    box-shadow: inset 0 0 8px rgba(0,0,0,0.1), 0 0 16px rgba(0,0,0,0.1);
  }

</style>


