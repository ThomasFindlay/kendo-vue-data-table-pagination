<template>
  <div :id="$style.app">
    <h1>Posts</h1>

    <Grid :data-source="posts">
      <GridColumn field="id" title="Post ID" />
      <GridColumn field="userId" title="User ID" />
      <GridColumn field="title" title="Title" />
      <GridColumn field="excerpt" title="Excerpt" />
    </Grid>
    <!-- <table class="k-table">
      <thead>
        <tr>
          <th>Post ID</th>
          <th>User ID</th>
          <th>Title</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="post of posts" :key="post.id">
          <td>{{ post.id }}</td>
          <td>{{ post.userId }}</td>
          <td>{{ post.title }}</td>
          <td>{{ post.description?.slice(0, 50) }}</td>
        </tr>
      </tbody>
    </table> -->
  </div>
</template>

<script setup>
import { ref } from 'vue';
import '@progress/kendo-ui/js/kendo.grid';
import { Grid, GridColumn } from '@progress/kendo-grid-vue-wrapper';

const posts = ref([]);

fetch('https://jsonplaceholder.typicode.com/posts')
  .then((response) => response.json())
  .then((json) => {
    console.log('test', json);
    posts.value = json.map((item) => {
      return {
        ...item,
        excerpt: item.body.slice(0, 50),
      };
    });
  });
</script>

<style module>
#app {
  text-align: center;
  color: #2c3e50;
  max-width: 1180px;
  margin: 50px auto;
}
</style>
