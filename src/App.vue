<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data() {
    return {
      todoItems: []
    }
  },
  created() {
    if (localStorage.length > 0) {
        for (var i = 0; i < localStorage.length; i++) {
            this.todoItems.push(localStorage.getItem(localStorage.key(i)));
            //로컬 스토리지 순서가 바뀌는 현상을 해결하기 위해 시간 순서대로 배열에 추가
        }
    }//정상 작동
  },
  methods: {
    addTodo(todoItem) {
      localStorage.setItem(Math.floor(Date.now()), todoItem);
      //시간을 키로 지정하여 문자열 순서대로 재배열되는 현상을 방지
      this.todoItems.push(todoItem);
      //정상 작동
    },
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
      //정상 작동
    },
    removeTodo(index) {
      localStorage.removeItem(localStorage.key(index));//작동하지 않음
      this.todoItems.splice(index, 1);//todoItems의 데이터가 정상 삭제됨을 확인하여 정상 작동
    }
  },
  components: {
    'TodoHeader' : TodoHeader,
    'TodoInput' : TodoInput,
    'TodoList' : TodoList,
    'TodoFooter' : TodoFooter
  }
}
</script>

<style>
  body {
    text-align: center;
    background-color: #F6F6F8
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