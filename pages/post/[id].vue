<template>
  <div class="card mb-3 m-5">
    <div class="row g-0">
      <div class="col-md-4">
        <img
          :src="blog?.image_preview ? blog.image_preview : ''"
          class="img-fluid rounded-start p-1"
          alt="..."
        />
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">
            {{ blog?.title ? blog.title : "" }}
          </h5>

          <span class="card-text">
            {{ blog?.content ? removeHtmlTags(blog.content) : "" }}
          </span>

          <p class="card-text">
            <small class="text-muted">
              {{ "Дата:" }} {{ translateDate(blog.published_at) }}
            </small>
          </p>

          <button type="button" class="btn btn-danger m-1" @click="like()">
            Нравиться: {{ blog?.likes_count ? blog.likes_count : 0 }}
          </button>
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
    const route = useRoute();

    const blogId: number = Number(route.params?.id ? route.params.id : 0);
    const blog: Blog | undefined = store.findBlogById(Number(blogId));

    const like = (): void => {
      store.inCrementLikesCount(blogId);
    };

    const removeHtmlTags = (str: string): string => {
      const div = document.createElement("div");
      div.innerHTML = str;
      return div.textContent || div.innerText || "";
    };

    const translateDate = (date: string): string => {
      if (!date) return "-";
      const newDate = new Date(date);
      const day = newDate.getDate().toString().padStart(2, "0");
      const month = (newDate.getMonth() + 1).toString().padStart(2, "0");
      const year = newDate.getFullYear();
      return `${day}.${month}.${year}`;
    };

    return {
      blog,
      translateDate,
      like,

      removeHtmlTags,
    };
  },
});
</script>
