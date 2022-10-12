<template>
  <div>
    <div v-if="$fetchState.pending"><ProgressSpinner /></div>
    <div v-else-if="$fetchState.error">
      <Message severity="error">Error while fetching data</Message>
    </div>
    <div v-else>
      <Paginator
        v-model="currentPage"
        :first="first"
        :rows="rows"
        :totalRecords="count"
        :pageLinkSize="3"
        @page="onPage($event)"
      />
      <div v-for="(post, index) in posts" :key="index">
        <Card>
          <template #title>
            {{ post.title }}
          </template>
          <template #content>
            <Chip
              :label="
                post.User?.firstname +
                ' - ' +
                moment(post.created).format('DD/MM/YYYY')
              "
              :image="post.User?.image"
            />
            <p v-html="post.message">{{ post.message }}</p>
            <Image v-if="post.image" src="post.image" alt="post.image" />
          </template>
        </Card>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "primevue/message";
import ProgressSpinner from "primevue/progressspinner";
import Paginator from "primevue/paginator";
import Card from "primevue/card";
import Chip from "primevue/chip";
import moment from "moment";
export default {
  name: "PostsIndex",
  components: {
    Paginator,
    Card,
    Chip,
    Message,
    ProgressSpinner,
  },
  data() {
    return {
      currentPage: 1,
      rows: 10,
      first: 1,
      data: [],
      posts: [],
      count: null,
      totalPages: null,
    };
  },
  async fetch() {
    this.data = await fetch(
      `${process.env.VUE_APP_URL_BACKEND}/api/posts?page=${this.currentPage}`
    )
      .then((res) => res.json())
      .then((data) => {
        this.currentPage = data.currentPage;
        this.posts = data.posts.rows;
        this.count = data.posts.count;
        this.totalPages = data.totalPages;
      });
  },
  methods: {
    moment() {
      return moment();
    },
    onPage(event) {
      this.currentPage = event.page + 1;
      this.first = event.first;
      this.rows = event.rows;
      this.totalPages = event.pageCount;
      this.$fetch();
    },
  },
};
</script>
