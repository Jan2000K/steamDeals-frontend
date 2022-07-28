<script lang="ts" setup>
const props = defineProps({
  title: String,
  thumbnail : String,
  basePrice : Number,
  newPrice : Number,
  rating : String
})
</script>

<template>

<div class="justify-center w-full sm:w-1/3 lg:w-1/4">
    <div class="text-center">
        <div class="flex justify-center py-2">
            <img v-bind:src="props.thumbnail" class="w-4/5">
        </div>
        <h3 title="Game title" class="text-lg font-semibold inline-block">{{props.title}}</h3>
        <span title="Steam community rating" class="text-orange-400 font-bold ml-2">{{props.rating}}</span>
        <br/>
        <span title="Original Price" class="ml-2 font-semibold line-through">{{props.basePrice}}€</span>
        <span title="Discount Price" class="ml-2 text-green-500 font-semibold">{{props.newPrice}}€</span>
    </div>
</div>

</template>