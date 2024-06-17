<template>
  <div class="not-prose">
    <section v-if="error">Something went wrong... Try again!</section>
    <section v-else>
      <ul class="grid grid-cols-1 gap-4">
        <li
          v-for="repos in repositories"
          :key="repos.id"
          class="border border-gray-200 dark:border-gray-800 rounded-sm p-4 hover:bg-gray-200 dark:hover:bg-gray-800 font-mono"
        >
          <a :href="repos.html_url" target="_blank">
            <div class="flex items-center justify-between text-sm">
              <div class="font-semibold">{{ repos.name }}</div>
              <div>{{ repos.stargazers_count }} *</div>
            </div>
            <p class="text-sm">{{ repos.description }}</p>
          </a>
        </li>
      </ul>
    </section>
  </div>
</template>
<script setup>
const { data, error } = await useFetch(
  "https://api.github.com/users/piotr-jura-udemy/repos"
);
const repositories = computed(() =>
  data.value
    .filter((repo) => repo.description)
    .sort((a, b) => b.stargazers_count - a.stargazers_count)
);
if (error.value) {
  console.error("Error fetching data:", error.value);
} else {
  console.log("Fetched data:", data.value);
}
</script>
