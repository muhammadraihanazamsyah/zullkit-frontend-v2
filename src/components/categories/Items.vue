<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'

import ItemCard from './../ItemCard.vue'

const items = ref([])
const category = ref({});
const route = useRoute()

async function getItemsData() {
    try {
        const response = await axios.get('http://localhost:8080/api/categories?id=' + route.params.id + '&show_product=1')
        items.value = response.data.data.products
        category.value = response.data.data
    } catch (error) {
        console.error(error)
    }
}

onMounted(() => {
    getItemsData()
})


</script>

<template>
    <div class="container px-4 mx-auto my-16 md:px-12">
        <h2 class="mb-4 text-xl font-medium md:mb-0 md:text-lg">{{ category.name }}</h2>
        <div class="flex flex-wrap -mx-1 lg:-mx-4">
            <template v-if="items.length > 0">
                <ItemCard v-for="item in items" :key="item.id" :id="item.id" :title="item.name"
                    :description="item.subtitle" :image="item.thumbnails" />
            </template>

            <template v-else>
                <div class="w-full px-1 my-1 text-center lg:my-4 lg:px-4">
                    <div class="p-4 border border-gray-200 rounded-xl">
                        Item not available
                    </div>
                </div>
            </template>
        </div>
    </div>
</template>