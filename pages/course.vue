<template>
  <div class="p-12 bg-gray-100 w-full h-full min-h-screen flex flex-col">
    <div class="mb-4 flex justify-between items-center w-full">
      <h1 class="text-3xl">
        <span class="font-medium">
          <span class="font-bold">Course title</span>
        </span>
      </h1>
    </div>

    <div class="flex flex-row justify-center flex-grow">
      <div
        class="prose mr-4 p-8 bg-white rounded-md min-w-[20ch] max-w-[30ch] flex flex-col"
      >
        <h3 class="mb-4">Chapters</h3>
        <div
          class="space-y-1 mb-4 flex flex-col"
          v-for="chapter in chapters"
          :key="chapter.slug"
        >
          <h4 class="flex justify-between items-center">
            {{ chapter.title }}
            <!-- <span
              class="text-emerald-500 text-sm"
              v-if="percentageCompleted && user"
              >{{ percentageCompleted.chapters[index] }}%</span
            > -->
          </h4>

          <NuxtLink
            v-for="(lesson, index) in chapter.lessons"
            :key="lesson.slug"
            class="flex flex-row space-x-1 no-underline prose-sm font-normal py-1 px-4 -mx-4"
            :to="lesson.path"
            :class="{
              'text-blue-500': lesson.path === $route.fullPath,
              'text-gray-600': lesson.path !== $route.fullPath,
            }"
          >
            <span class="text-gray-500">{{ index + 1 }}.</span>
            <span>{{ lesson.title }}</span>
          </NuxtLink>
        </div>
        <!-- <div
          v-if="percentageCompleted"
          class="mt-8 text-sm foant-medium text-gray-500 flex justify-between items-center"
        >
          Course compeltion:
          <span>{{ percentageCompleted.course }}%</span>
        </div> -->
      </div>

      <div class="prose p-12 bg-white rounded-md w-[65ch]">
        <NuxtPage />
        <!-- <NuxtErrorBoundary>
          <NuxtPage />
          <template #error="{ error }">
            <p>
              Oh no, something went wrong with the lesson!
              <code>{{ error }}</code>
            </p>
            <p>
              <button
                class="hover:cursor-pointer bg-gray-500 text-white font-bold py-1 px-3 rounded"
                @click="resetError(error)"
              >
                Reset
              </button>
            </p>
          </template>
        </NuxtErrorBoundary> -->
      </div>
    </div>
  </div>
</template>

<script setup>
const { chapters } = useCourse();
</script>

<style scoped>
/* .router-link-active {
  @apply text-blue-500;
} */
</style>
