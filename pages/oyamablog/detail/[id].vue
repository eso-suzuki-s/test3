<template>
  <div>
    <UiPageHeader
      :path="[{ label: 'ニュース', to: '/oyamablog' }]"
      :subject="oyamablog.details.group_nm"
      subheading="News Release"
    />
    <div class="l-container--col-2 l-container--contents">
      <div class="l-container--col-2__main">
        <article class="c-article">
          <header>
            <h1 class="c-heading--lv1">
              {{ oyamablog.details.subject }}
            </h1>
            <time class="c-topics__date" :datetime="oyamablog.details.ymd">{{
              oyamablog.details.ymd
            }}</time>
            <span class="c-badge">
              {{ oyamablog.details.contents_type_nm }}
            </span>
          </header>
          <div class="l-container--contents">
            <div v-html="oyamablog.details.contents"></div>
          </div>

          <hr />
          <div class="l-container--contents u-pt-30 u-text-align-center">
            <NuxtLink :to="'/oyamablog/'" class="c-button">
              ニュースリリース一覧へ戻る
            </NuxtLink>
          </div>
        </article>
      </div>
      <ContentSideBar :conditions="oyamablogConditionMaster?.list" />
    </div>
  </div>
</template>

<script setup>
const config = useRuntimeConfig();
const route = useRoute();

const { data: oyamablog } = await useFetch(
  `${config.public.kurocoApiDomain}/rcms-api/1/oyamablog/details/${route.params.id}`,
  {
    credentials: "include",
    server: false,
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
