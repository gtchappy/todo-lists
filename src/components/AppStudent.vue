<template>
  <div class="student">
    <h2>学生姓名:{{ name }}</h2>
    <h2>学生性别:{{ sex }}</h2>
    <button @click="sendStudentName">把学生名给School组件</button>
  </div>
</template>

<script>
import pubsub from 'pubsub-js'

export default {
  name: "AppStudent",
  data() {
    return {
      name: '张三',
      sex: '18'
    }
  },
  mounted() {

  },
  methods: {
    sendStudentName() {
      pubsub.publish('hello',666)
    }
  }
}
</script>

<style scoped>
.student {
  padding: 5px;
  background-color: pink;
  margin-top: 30px;
}
</style>