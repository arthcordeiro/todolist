<template>
  <div class="todolist">
      <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo()">
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div v-if="!todo.editing" :class="{ completed : todo.completed }" @dblclick="editTodo(todo)" class="todo-item-label">
                {{ todo.title }}
            </div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title"
                   @keyup.enter="doneEdit(todo)" @blur="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'todolist',
  data () {
      return {
          newTodo: '',
          idForTodos: 3,
          beforeEditCache: '',
          todos: [
              {
                  id: 1,
                  'title': 'Finish Vue Screencast',
                  'completed': false,
                  'edited': false,
                  'editing': false,
              },
              {
                  id: 2,
                  'title': 'Take over world',
                  'completed': false,
                  'edited': false,
                  'editing': false,
              }
          ]
      }
  },
  methods: {
      addTodo(){
          if(this.newTodo.trim().length == 0) return;

          this.todos.push({
              id: this.idForTodos,
              title: this.newTodo,
              completed: false,
              edited: false,
              editing: false,
          })
          
          this.newTodo = ''
          this.idForTodos++

      },
      removeTodo(index){
          this.todos.splice(index, 1)
      },
      editTodo(todo){
          this.beforeEditCache = todo.title
          todo.editing = true;
      },
      doneEdit(todo){
          if(todo.title.trim() == '') todo.title = this.beforeEditCache;
          todo.editing = false;
      },
      cancelEdit(todo){
          todo.title = this.beforeEditCache
          todo.editing = false;
      },
  },
  directives: {
      focus: {
          //directive definition
          inserted: function (el) {
              el.focus()
          }
      }
  },
  props: {
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    .todolist{
        width: 80%;
        margin-left: 8%;
    }
    .todo-input{
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;

        &:focus{
            outline: 0;
        }
    }
    .todo-item{
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .remove-item{
        cursor: pointer;
        margin-left: 14px;
        &:hover{
            color: black;
        }
    }
    .todo-item-left{
        display: flex;
        align-items: center;
    }
    .todo-item-label{
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-edit{
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Arial, Helvetica, sans-serif;

        &:focus{
            outline:none;
        }
    }
    .completed{
        text-decoration: line-through;
        color: grey;
    }
</style>
