<template>
  <div :id="$style.app">
    <h1>Posts</h1>

    <table class="k-table k-table-layout-fixed">
      <thead class="k-table-thead">
        <tr class="k-table-row">
          <th class="k-table-th">Post ID</th>
          <th class="k-table-th">User ID</th>
          <th class="k-table-th">Title</th>
          <th class="k-table-th">Excerpt</th>
        </tr>
      </thead>
      <tbody class="k-table-tbody">
        <tr class="k-table-row" v-for="post of posts" :key="post.id">
          <td class="k-table-td">{{ post.id }}</td>
          <td class="k-table-td">{{ post.userId }}</td>
          <td class="k-table-td">{{ post.title }}</td>
          <td class="k-table-td">{{ post.excerpt?.slice(0, 50) }}</td>
        </tr>
      </tbody>
    </table>
    <div class="k-mt-8">
      <Pager
        :skip="skip"
        :take="take"
        :total="allPosts.length"
        :buttonCount="5"
        type="numeric"
        info
        pageSizes
        previousNext
        @pagechange="onPageChange"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { Pager } from "@progress/kendo-vue-data-tools";

const skip = ref(0);
const take = ref(10);
const allPosts = ref([]);

fetch("https://jsonplaceholder.typicode.com/posts")
  .then(response => response.json())
  .then(json => {
    allPosts.value = json.map(item => {
      return {
        ...item,
        excerpt: item.body.slice(0, 50),
      };
    });
  });

const posts = computed(() =>
  allPosts.value.slice(skip.value, skip.value + take.value)
);

const onPageChange = event => {
  skip.value = event.skip;
  take.value = event.take;
};
</script>

<style module>
#app {
  text-align: center;
  color: #2c3e50;
  max-width: 1180px;
  margin: 50px auto;
}
</style>
