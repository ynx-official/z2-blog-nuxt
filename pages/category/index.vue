<script setup lang="ts">
import {OuOLanding} from "@ahzoo/ouo";
import type {CategoryMap} from "@/types/categoryInterface";
import {OuOTag} from "@ahzoo/ouo";
import type {PreviewColumn} from "@/types/columnInterface";
import {listCategory} from "@/api/category";
import {listColumnByCategoryId} from "@/api/column";
import ColumnItem from "@/components/list/ColumnItem.vue";

const categoryList = ref<CategoryMap[]>([]);
const columnList = ref<PreviewColumn[]>([]);

getCategoryList();

async function getCategoryList() {
  const newCategoryList = await listCategory();
  categoryList.value = unref(newCategoryList);
}

async function getColumnListByCategoryId(categoryId: string, pagination: number) {
  const newColumnList = await listColumnByCategoryId(categoryId, pagination);
  columnList.value = unref(newColumnList);
}

useSeoMeta({
  title: "分类",
  description: "分类页"
})
</script>

<template>
  <div class="w-full">
    <Header/>
  </div>
  <OuOLanding/>
  <div id="main" class="page flex">
    <div class="page-content w-full">
      <div v-if="categoryList.length>0" class="box-header">
        <OuOTag class="mr-2" :size="'small'" v-for="category in categoryList"
                @click="getColumnListByCategoryId(category.id, 1)">
          {{ category.name }}
        </OuOTag>
      </div>
      <div class="category-column grid column-grid gap-6 gap-y-4 pc:gap-4">
        <ColumnItem v-for="column in columnList" :column="column"/>
      </div>
    </div>
    <Sidebar class="w-1/3 mt-5"
             v-if="!$viewport.isLessThan('sm')"/>
  </div>
</template>
