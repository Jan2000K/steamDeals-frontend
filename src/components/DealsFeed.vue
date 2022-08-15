<script lang="ts" setup>
import { onMounted, ref } from "vue"
import DealCard from "./DealCard.vue"

interface gameDeal {
    title: string
    originalPrice: number
    discountPrice: number
    thumbnailURL: string
    steamRating: string
}

const __API_URL__ = "https://sd.janezsite.com/api"

const gameDealsArr = ref<gameDeal[]>([])

const gameTitle = ref<HTMLInputElement | null>(null)

const minPrice = ref<HTMLInputElement | null>(null)

const maxPrice = ref<HTMLInputElement | null>(null)

const errorFetching = ref(false)

async function getDeals() {
    if (gameTitle !== null && minPrice !== null && maxPrice !== null) {
        if(minPrice.value?.value==="" ||maxPrice.value?.value===""){
            return gameDealsArr.value
        }
        try {
            const res = await fetch(
                `${__API_URL__}?minPrice=${minPrice.value?.value}&maxPrice=${maxPrice.value?.value}&title=${gameTitle.value?.value}`
            )
            errorFetching.value=false
            return (await res.json()) as gameDeal[]
        } catch {
            errorFetching.value = true
            return []
        }
    } else {
        return []
    }
}
onMounted(async () => {
    gameDealsArr.value = await getDeals()
})

async function handleSubmit() {
    const data = await getDeals()
    gameDealsArr.value = data
}

</script>

<template>
    <div class="bg-blue-100">
        <div>
            <form @submit.prevent>
            <label class="lg:ml-4" for="gameTitle">Game Title</label>
            <input
            ref="gameTitle"
            type="search"
            class="bg-gray-100 rounded mt-2 p-1 text-lg w-full text-center lg:w-1/5 lg:ml-2 lg:text-left"
            />
            <label class="lg:ml-2">Minimum Price(€)</label>
            <input
                type="number"
                max="100"
                min="0"
                value="0"
                ref="minPrice"
                required
                class="bg-gray-100 rounded mt-2 p-1 text-lg w-full text-center lg:w-min lg:ml-2"
            />
            <label class="lg:ml-2">Maximum Price(€)</label>
            <input
                ref="maxPrice"
                value="60"
                type="number"
                max="100"
                min="0"
                required
                class="bg-gray-100 rounded mt-2 p-1 text-lg w-full text-center  lg:w-min lg:ml-2 "
            />
            <div class="flex justify-center lg:inline-block ml-2">
                <button
                    class="ml-2 rounded-full transition-all mt-3 w-1/2 bg-slate-100 p-2 hover:cursor-pointer hover:bg-sky-400 hover:transition-all lg:w-auto"
                    @click="handleSubmit"
                >
                    Find Deals
                </button>
            </div>
            </form>
        </div>
        <div v-if="errorFetching">
            <h2 class="text-red-400 text-xl mt-2">Error fetching data</h2>
        </div>
        <div
            class="flex flex-wrap mt-4"
            v-else-if="
                gameDealsArr !== undefined &&
                gameDealsArr !== null &&
                gameDealsArr.length >= 1
            "
        >
            <DealCard
                v-for="deal in gameDealsArr"
                :base-price="deal.originalPrice"
                :new-price="deal.discountPrice"
                :rating="deal.steamRating"
                :thumbnail="deal.thumbnailURL"
                :title="deal.title"
            />
        </div>

        <div v-else>
            <h2 class="text-red-400 text-xl mt-2">
                No deals found for given parameters
            </h2>
        </div>
    </div>
</template>
