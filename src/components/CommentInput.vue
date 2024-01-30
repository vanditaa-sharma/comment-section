<template>
  <div>
    <form @submit.prevent="addComment">
      <input v-model="newComment" placeholder="Enter Comments.." />
      <button type="submit">{{ editIndex === null ? "Add Comment" : "Save" }}</button>
    </form>
    <ul class="comment-list">
      <li v-for="(comment, index) in Comments" :key="index">
        {{ index + 1 }}. {{ comment }}
        <button @click="removeComment(index)">Remove</button>
        <button @click="editComment(index)">Edit</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const newComment = ref('');
    const Comments = ref([]);
    const editIndex = ref(null);
    // const isEditted = ref(false);

    const addComment = () => {
      if (newComment.value.trim() === '') {
        console.warn('Do not add empty comment');
        return;
      }
      if (editIndex.value !== null) {
        Comments.value[editIndex.value] = newComment.value.trim();
        editIndex.value = null;
      } else {
        Comments.value.push(newComment.value.trim());
      }
      saveCommentsToLocalStorage();
      newComment.value = '';
    };

    const removeComment = (index) => {
      Comments.value.splice(index, 1);
      saveCommentsToLocalStorage();
    };

    const editComment = (index) => {
      newComment.value = Comments.value[index];
      editIndex.value = index;
    };

    const saveCommentsToLocalStorage = () => {
      localStorage.setItem('Comments', JSON.stringify(Comments.value));
    };

    const getCommentsFromLocalStorage = () => {
      return JSON.parse(localStorage.getItem('Comments') || '[]');
    };

    onMounted(() => {
      Comments.value = getCommentsFromLocalStorage();
    });

    return { newComment, Comments, editIndex, addComment, removeComment, editComment };
  },
};

</script>

<style scoped>
.comment-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  text-align: center;
}

input {
  width: 95%;
  padding: 12px 20px;
  margin: 8px 0 1px;
  box-sizing: border-box;
}

.comment-list li {
  margin: 10px;
}

button {
  background-color: #04AA6D;
  border: none;
  border-radius: 10%;
  color: white;
  padding: 10px 10px;
  margin: 10px 1px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 10px;
}
</style>
