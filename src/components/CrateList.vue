<script setup>
import crates from "@data/crates.json";
import { CubeIcon, CheckBadgeIcon } from "@heroicons/vue/24/outline";
import { camelCase } from "yargs-parser";

const categorizedCrates = crates.categories.map((cat) => {
  return {
    name: cat.name,
    slug: cat.slug,
    crates: crates.crates.filter((c) => c.categories.includes(cat.slug)),
  };
});

function sortAxoUp(a, b) {
  if (a.axo_use) return -1;
  if (b.axo_use) return 1;
  return 0;
}
</script>

<template>
  <div :id="category.slug" v-for="category in crates.categories">
    <h2 class="font-serif font-bold text-3xl mt-10">{{ category.name }}</h2>
    <p class="max-w-prose text-lg my-4">{{ category.description }}</p>

    <div
      v-if="
        categorizedCrates.find((c) => c.slug === category.slug).crates
          .length === 0
      "
      class="border border-yellow-200 bg-yellow-100 rounded max-w-prose p-4 text-yellow-600"
    >
      No crates here yet!
    </div>
    <a
      v-for="crate in categorizedCrates
        .find((c) => c.slug === category.slug)
        .crates.sort(sortAxoUp)"
      :href="`https://crates.io/crates/${crate.name}`"
      target="_blank"
      class="border rounded flex max-w-prose gap-3 items-center p-1 my-2 shadow transition hover:shadow-lg"
    >
      <CubeIcon class="w-20 text-gray-100 self-start"></CubeIcon>
      <div>
        <h4 class="font-bold font-serif text-lg my-1">{{ crate.name }}</h4>
        <div
          v-if="crate.axo_use"
          class="text-green-700 font-semibold flex items-center gap-1"
        >
          <CheckBadgeIcon class="w-5 inline-block"></CheckBadgeIcon>
          <span>Used by axo</span>
        </div>

        <details v-if="crate.axo_reason" class="text-sm text-gray-600">
          <summary>Why we recommend it</summary>
          {{ crate.axo_reason }}
        </details>

        <p class="my-2">{{ crate.description }}</p>
      </div>
    </a>
  </div>
</template>
