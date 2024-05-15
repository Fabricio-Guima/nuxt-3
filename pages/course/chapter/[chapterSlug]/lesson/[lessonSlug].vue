<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <NuxtLink
          v-if="lesson.sourceUrl"
          class="font-normal text-md text-gray-500"
          :href="lesson.sourceUrl"
      >Download Source Code
      </NuxtLink
      >
    </div>
    <NuxtLink
        v-if="lesson.downloadUrl"
        class="font-normal text-md text-gray-500"
        :to="lesson.downloadUrl"
    >
      Download Video
    </NuxtLink>
    <VideoPlayer v-if="lesson.videoId" :videoId="lesson.videoId"/>
    <p>{{ lesson.text }}</p>
    <ClientOnly>
      <LessonCompleteButton
          :model-value="isLessonComplete"
          @update:model-value="toggleComplete"
      />
    </ClientOnly>
  </div>
</template>

<script setup>
const course = useCourse();
const route = useRoute();

definePageMeta({
  middleware: function (to, from) {
    const course = useCourse();
    const route = useRoute();

    const chapter = course.chapters.find(
        (chapter) => chapter.slug === to.params.chapterSlug
    );

    // error no server side
    if (!chapter) {
      return abortNavigation(
          createError({
            statusCode: 404,
            message: 'Chapter not found from definePageMeta'
          })
      )
    }
    console.log('url route', to.params.lessonSlug,)

    const lesson = chapter.lessons.find((lesson) => {
      return lesson.slug === to.params.lessonSlug;
    });
    console.log('lesson aqui', lesson)

    // error no server side
    if (!lesson) {
      return abortNavigation(
          createError({
            statusCode: 404,
            message: 'Lesson not found from definePageMeta'
          })
      )
    }

  }
});

const chapter = computed(() => {
  return course.chapters.find((chapter) => {
    return chapter.slug === route.params.chapterSlug;
  });
});

// error no server side
// if (!chapter.value) {
//   throw createError({
//     statusCode: 404,
//     message: "Chapter not found",
//   });
// }

const lesson = computed(() => {
  return chapter.value.lessons.find((lesson) => {
    return lesson.slug === route.params.lessonSlug;
  });
});

// error no server side
// if (!lesson.value) {
//   throw createError({
//     statusCode: 404,
//     message: "Lesson not found",
//   });
// }

const title = computed(() => {
  return `${lesson.value.title} - ${course.title}`;
});

useHead({
  title: title,
});

const progress = useLocalStorage("progress", []);

const isLessonComplete = computed(() => {
  if (!progress.value[chapter.value.number - 1]) {
    return false;
  }

  if (!progress.value[chapter.value.number - 1][lesson.value.number - 1]) {
    return false;
  }

  return progress.value[chapter.value.number - 1][lesson.value.number - 1];
});

const toggleComplete = () => {
  if (!progress.value[chapter.value.number - 1]) {
    progress.value[chapter.value.number - 1] = [];
  }

  progress.value[chapter.value.number - 1][lesson.value.number - 1] =
      !isLessonComplete.value;
};
</script>

<style lang="scss" scoped></style>
