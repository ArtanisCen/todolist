<template>
  <div class="todoapp">
    <TodoHeader @create="createFn" :arr="list" ></TodoHeader>
    <TodoMain :arr="showArr" @del="deleteFn"></TodoMain>
    <TodoFooter :farr="showArr" @changeType="typeFn" @clear="clearF"></TodoFooter>
  </div>
</template>

<script>
import "./styles/base.css"
import "./styles/index.css"

import TodoHeader from "./components/TodoHeader"
import TodoMain from "./components/TodoMain"
import TodoFooter from "./components/TodoFooter"
export default {
  data () {
    return {
       list: JSON.parse(localStorage.getItem('todoList')) || [],
      getSel:'all'
    }
  },
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,

},
  methods: {
    createFn(taskName){
      let id = this.list.length === 0 ? 100 : this.list[this.list.length - 1].id+1
      this.list.push({
        id:id,
        name:taskName,
        isDone:false
      })
    },
    deleteFn(theid){
      let index = this.list.findIndex(obj => obj.id === theid)
      this.list.splice(index,1)
    },
    typeFn(str){
      this.getSel = str
    },
    clearF(){
      this.list=this.list.filter(obj=>obj.isDone==false)
    }
    
  },
  computed:{
    showArr(){
      if(this.getSel === "yes"){
        return this.list.filter(obj => obj.isDone === true)
      }else if (this.getSel === 'no'){
        return this.list.filter(obj=>obj.isDone === false)
      }else{
        return this.list
      }
    }
  },
  watch: {
    list:{
      deep:true,
      handler(){
        localStorage.setItem('todoList',JSON.stringify(this.list))
      }
    }
  }
}
</script>

<style>

</style>