<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodoItem="addOneItem"></TodoInput> <!-- v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메서드 명" -->
    <TodoList v-bind:propsdata="todoItems" 
    v-on:removeItem="removeOneItem" 
    v-on:toggleItem="toggleOneItem"></TodoList> <!-- v-bind:내려보낼 프롭스 속성 이름="현재 위치의 컴포넌트 데이터 속성" -->
    <TodoFooter v-on:clearAll="clearAllItems"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'

export default {
  name: 'App',
   data() {
    return {
      todoItems: []
    }
  },
  methods: {
    addOneItem(todoItem) {
      const obj = {completed: false, item: todoItem}; 
      // 저장하는 로직
      localStorage.setItem(todoItem, JSON.stringify(obj)); //JSON.stringify : 문자열로 변환
      this.todoItems.push(obj)
    },
    removeOneItem(todoItem, index) {
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem(todoItem, index){
      // todoItem.completed =  !todoItem.completed; --> propsdata로 내린 데이터를 다시 받아서 직접 변경하는건 안티패턴이므로 주의
      this.todoItems[index].completed = !this.todoItems[index].completed //app의 데이터인 this.todoItems를 변경
      localStorage.removeItem(todoItem, index);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems() {
      localStorage.clear();
      this.todoItems = [];
    }
  },
  created() {
    if(localStorage.length > 0) {
      for(let i = 0; i < localStorage.length; i ++) {
        if(localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))))
          
        }
        
      }
    }
  },
  components: {
    TodoHeader,
    TodoFooter,
    TodoList,
    TodoInput
  }
}
</script>

<style>
body {
  text-align: center;
  background-color: #f6f6f6
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
