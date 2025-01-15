<template>
  <div>
    <h1>Vue 3 & Spring Boot 통신 예제</h1>
    <p>Spring Boot에서 가져온 메시지: {{ message }}</p>
    <button @click="fetchMessage">메시지 가져오기</button>
  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";

export default {
  setup() {
    const message = ref("");

    const fetchMessage = async () => {
      try {
        const response = await axios.get("http://localhost:8081/api/message");
        message.value = response.data;
      } catch (error) {
        console.error("메시지 가져오는 중 오류 발생:", error);
      }
    };

    return {
      message,
      fetchMessage,
    };
  },
};
</script>

<style scoped>
h1 {
  color: #42b983;
}
</style>
