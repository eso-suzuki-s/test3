<template>
  <div class="l-container">
    <UiPageHeader subject="ニュース" subheading="News Release" />
    <section>
      <div class="l-container--col-2 l-container--contents">
        <div class="l-container--col-2__main">
          <template v-if="oyamablog">
            <ul v-if="oyamablog.list.length > 0" class="c-topics-list">
              <li v-for="oyamablog in oyamablog.list" :key="oyamablog.topics_id">
                <NuxtLink
                  :to="`/oyamablog/detail/${oyamablog.topics_id}`"
                  class="c-topics"
                >
                  <time class="c-topics__date" :datetime="oyamablog.ymd">{{
                    oyamablog.ymd
                  }}</time>
                  <span class="c-badge">
                    {{ oyamablog.contents_type_nm }}
                  </span>
                  <p class="c-topics__title">
                    {{ oyamablog.subject }}
                  </p>
                </NuxtLink>
              </li>
            </ul>
            <p v-else>記事が存在しません</p>
          </template>
        </div>
        <ContentSideBar :conditions="newsConditionMaster?.list" />
      </div>
    </section>
  </div>
</template>

<script setup>
const config = useRuntimeConfig();
const route = useRoute();
const filter = computed(() => route.query.filter);

const { data: oyamablog } = await useFetch(
  `${config.public.kurocoApiDomain}/rcms-api/1/oyamablog/list`,
  {
    credentials: "include",
    query: {
      filter,
    },
    watch: [filter],
    server: false, // in order to get query parameter, runs only client side
  }
);
const { data: oyamablogConditionMaster } = await useFetch(
  `${config.public.kurocoApiDomain}/rcms-api/1/master`,
  {
    credentials: "include",
    server: false,
  }
);
</script>
