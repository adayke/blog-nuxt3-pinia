<template>
  <div
    class="row row-cols-1 row-cols-md-2 mt-1 mb-5 g-4 justify-content-center"
  >
    <div class="col" style="width: 18rem" v-for="blog of blogs" :key="blog.uid">
      <div class="card">
        <img :src="blog.image_preview" class="card-img-top" alt="..." />
        <div class="card-body">
          <NuxtLink
            :to="`/post/${blog.id}`"
            class="link-primary"
            aria-current="page"
          >
            <h5 class="card-title">{{ blog.title ? blog.title : "" }}</h5>
          </NuxtLink>

          <p class="card-text">
            {{ blog.short ? blog.short : "" }}
          </p>
        </div>
        <div class="card-footer">
          <small class="text-muted"
            >{{ "Дата:" }}
            {{ blog.published_at ? translateDate(blog.published_at) : "" }}
          </small>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
interface Blog {
  _type: string;
  id: number;
  uid: string;
  title: string;
  slug: string;
  short: string;
  likes_count: number;
  me_liked: boolean;
  published_at: string;
  image_preview: string;
  content: string;
}

import { defineComponent } from "vue";
import { useBlogsStore } from "@/store/index";

export default defineComponent({
  setup() {
    const store = useBlogsStore();
    const blogs: Blog[] = store.getBlogs;

    const translateDate = (date: string): string => {
      if (!date) return "-";
      const newDate = new Date(date);
      const day = newDate.getDate().toString().padStart(2, "0");
      const month = (newDate.getMonth() + 1).toString().padStart(2, "0");
      const year = newDate.getFullYear();
      return `${day}.${month}.${year}`;
    };

    return {
      blogs,
      translateDate,
    };
  },
});
</script>
