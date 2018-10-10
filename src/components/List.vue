<template>
    <div class="list container">
        <div v-for="todo in todos" :key="todo" class="item">
            List Item
        </div>
    </div>
</template>
<script>

export default {
  name: 'list',
  data() {
    return {
      todos: [{}],
    };
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
</style>
