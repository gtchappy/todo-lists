<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <AppHeader @addTodo="addTodo"/>
        <AppList
            :todos="todos"
        />
        <AppFooter
            :todos="todos"
            @checkAllTodo="checkAllTodo"
            @clearAllTodo="clearAllTodo"
        />
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
import AppFooter from './components/AppFooter'
import AppHeader from './components/AppHeader'
import AppList from "./components/AppList"

export default {
  name: 'App',
  components: {AppFooter, AppHeader, AppList},
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: {
    //添加一个todo
    addTodo(x) {
      this.todos.unshift(x)
    },
    //勾选or取消勾选一个todo
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.done = !todo.done
        }
      })
    },
    //更新一个todo
    updateTodo(id,titles) {
      console.log('hi')
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.titles = titles
        }
      })
    },
    //删除一个todo
    deleteTodo(_, id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    },
    //全选or取消全选
    checkAllTodo(done) {
      this.todos.forEach((todo) => {
        todo.done = done
      })
    },
    //清除所有已经完成的todo
    clearAllTodo() {
      this.todos = this.todos.filter((todo) => {
        return !todo.done
      })
    }
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem('todos', JSON.stringify(value))
      }
    }
  },


  mounted() {
    //全局事件总线
    this.$bus.$on('checkTodo', this.checkTodo)
    this.$bus.$on('updateTodo', this.updateTodo)
    //消息订阅与发布
    this.pubId = pubsub.subscribe('deleteTodo', this.deleteTodo)
  },
  beforeDestroy() {
    //取消全局事件总线
    this.$bus.$off('checkTodo')
    this.$bus.$off('updateTodo')
    //取消消息订阅与发布
    pubsub.unsubscribe(this.pubId)
  }
}


</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-edit {
  color: #fff;
  background-color: #0c8aad;
  border: 1px solid #12449f;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>






