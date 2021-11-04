<template>
    <div>
		<transition-group name="list" tag="div">
			<div v-for="item in items" :key="item.id" class="list-group-item rounded mb-1 p-2 w-100">
				<item-component :item="item" v-on:itemchanged="$emit('reloadlist')"/>
			</div>
		</transition-group>
    </div>
</template>

<script>
import ItemComponent from "./ItemComponent.vue"

	export default 
  	{
		props: ['items'],
		components: { ItemComponent },
	}
</script>

<style>
	.list-enter-active, .list-leave-active {
		transition: all 1s;
	}
	.list-enter, .list-leave-to {
		opacity: 0;
		transform: translateX(30px);
	}
	.list-leave-out {
		transform: translateY(30px);
	}
</style>