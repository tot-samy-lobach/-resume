<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="submitForm">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value"
          rows="3"
          v-model.trim="inputContent">
        </textarea>
      </div>

      <app-button text="Добавить" :disabled="isDisable"></app-button>
    </form>

    <div class="card card-w70">
      <div v-if="contentObject.length !== 0">
      <app-content-list
        v-for="item in contentObject" :key="item.idx"
        :content="item.content"
        :typeContent="item.type">
      </app-content-list>
      </div>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
    
  </div>
  <div class="container">
    <p>
      <app-button
        text="Загрузить комментарии"
        v-if="comments.length === 0"
        @click="loadComments">
      </app-button>
    </p>
    <app-comment-list :comments="comments"></app-comment-list>
    <app-loader v-if="isloaded"></app-loader>
  </div>
</template>

<script>
import AppContentList from './AppContentList.vue';
import AppCommentList from './AppCommentList.vue';
import AppButton from './AppButton.vue';
import AppLoader from './AppLoader.vue';
import axios from 'axios';
export default {
  data() {
    return {
      inputContent: '',
      type: 'title',
      contentObject: [],
      comments: [],
      isloaded: false,
    };
  },
  methods: {
    submitForm() {
      this.contentObject.push({
        type: this.type,
        content: this.inputContent,
      });
      this.inputContent = '';
      this.type = 'title'
    },
    loadComments() {
      this.isloaded = true;
      setTimeout(async () => {
        try {
          const { data } = await axios.get(
            'https://jsonplaceholder.typicode.com/comments?_limit=42'
          );

          this.comments = Object.keys(data).map((key) => {
            return {
              id: data[key].id,
              email: data[key].email,
              body: data[key].body,
            };
          });
          this.isloaded = false;
        } catch (e) {
          console.log('loadComments error');
        }
      }, 2000);
    },
  },
  computed: {
    isDisable(){
      return this.inputContent.length < 4
    }
  },
  components: { AppContentList, AppCommentList, AppButton, AppLoader},
};
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
