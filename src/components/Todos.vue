<template>
  <div>
    <h3>Todos</h3>
    <div class="legend">
      <span>click to mark as complete</span>
      <span>
        <span class="incomplete-box"></span> = Incomplete
      </span>
      <span>
        <span class="complete-box"></span> = Complete
      </span>
    </div>
    <div class="todos">
      <div
        @click="onClick(todo)"
        class="todo"
        :class="{'is-complete':todo.completed}"
        v-for="todo in allTodos"
        :key="todo.id">
        {{todo.title}}
        <i class="fas fa-trash-alt" @click="deleteT(todo.id)"></i>
      </div>
    </div>
  </div>
</template>
<script>
import { mapGetters, mapActions} from 'vuex'
  export default{
    name:"Todos",
    computed: mapGetters(['allTodos']),
    methods:{
      ...mapActions(['fetchTodos', 'deleteTodo', 'updateTodo']),
      onClick(todo){
        this.$bar.start()
        const updTodo = {
          id: todo.id,
          title: todo.title,
          completed: !todo.completed
        }
        this.updateTodo(updTodo)
                .then(() =>{
                  this.$bar.finish()
                })

      },
      deleteT(id){
        this.$bar.start()
        this.deleteTodo(id)
                .then(() =>{
                  this.$bar.finish()
                })
      }
    },
    created(){
      this.$bar.start()
      this.fetchTodos()
              .then(() => {
                this.$bar.finish()
              })
    }
  }
</script>
<style scoped>
  .todos{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 1rem;
  }
  .todo{
    border: 1px solid #ccc;
    background: #41b883;
    padding: 1rem;
    border-radius: 5px;
    text-align:center;
    position: relative;
    cursor: pointer;
  }
  i{
    position: absolute;
    bottom: 10px;
    right: 10px;
    color: rgb(255, 255, 255);
    cursor: pointer;
  }
  .legend{
    display: flex;
    justify-content: space-around;
    margin-bottom: 1rem;
  }
  .complete-box{
    display: inline-block;
    width: 10px;
    height: 10px;
    background: #35495e;
  }
  .incomplete-box{
    display: inline-block;
    width: 10px;
    height: 10px;
    background: #41b833;
  }
  .is-complete{
    background: #35495e;
    color: #fff;
  }
  @media (max-width: 500px) {
    .todos{
      grid-template-columns: 1fr;
    }
  }
</style>
