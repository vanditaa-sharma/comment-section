<template>
  <h2>User Comments</h2>
  <CommentInput/>
</template>

<script>
import CommentInput from './components/CommentInput.vue'

export default {
  name: 'App',
  components: {
    CommentInput
  }
}
</script>
<style>

h2 {
    font-size: 30px;
    color: crimson;
    line-height: 40px;
    text-align: center;
}
</style>


