<script setup lang="ts">
import { KnowledgeItem, KnowledgeTab, KnowledgeTabsList } from "~/utils/types";
import ManageListTable from "~/comps/manage-list-table.vue";

const filterType = ref<KnowledgeTab>();
const registryFilter = (customFilter) => {
  watch(filterType, () => {
    customFilter((item) => {
      return !filterType.value || item.type === filterType.value;
    });
  });
};

const toggleFilterType = (type: KnowledgeTab) => {
  filterType.value = filterType.value === type ? undefined : type;
};

const searchFn = (item: KnowledgeItem, s: string) => item.title.includes(s);
</script>

<template>
  <div class="manage-knowledge">
    <manage-list-table
      :registry-filter="registryFilter"
      :show-filter="!!filterType"
      col-prefix="knowledge-"
      :search-fn="searchFn"
    >
      <template #filter>
        <span
          class="filter-type flex"
          :title="KnowledgeTabsList.find((i) => i.key === filterType)?.name"
          @click="toggleFilterType(filterType)"
        >
          <svg-icon v-if="filterType" :name="filterType" />
        </span>
      </template>
      <template #title="{ data: title, dataUrl }">
        <nuxt-link :to="dataUrl">
          {{ title }}
        </nuxt-link>
      </template>
      <template #type="{ data: type }">
        <span
          class="filter-type"
          :title="KnowledgeTabsList.find((i) => i.key === type).name"
          @click="toggleFilterType(type)"
        >
          <svg-icon :name="type" />
        </span>
      </template>
    </manage-list-table>
  </div>
</template>

<style lang="scss">
@import "assets/style/var";

.manage-knowledge {
  .knowledge-title {
    flex-basis: 45%;
    font-weight: bold;
    font-size: 15px;
  }

  .knowledge-type {
    flex-basis: 15%;
  }

  .filter-type {
    cursor: pointer;

    svg {
      @include square(30px);
    }
  }
}
</style>
