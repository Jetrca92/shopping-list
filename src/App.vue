<script setup>
import { ref, computed } from 'vue'
import './main.css'

const header = ref('Shopping List App')
const characterCount = computed(() => {
	return newItem.value.length
})
const items = ref([
	{id: 1, label: "10 party hats", purchased: true, highPriority: true},
	{id: 2, label: "2 board games", purchased: false, highPriority: false},
	{id: 3, label: "20 cups", purchased: false, highPriority: false},
])
const newItem = ref("")
const newItemHighPriority = ref(false)
const saveItem = () => {
	items.value.push({id: items.value.length + 1, label: newItem.value, highPriority: newItemHighPriority.value})
	newItem.value = ""
	newItemHighPriority.value = false
}
const editing = ref(false)
const doEdit = (e) => {
	editing.value = e
	newItem.value = ""
}
const togglePurchased = (item) => {
	item.purchased = !item.purchased
}
</script>

<template>
	<div id="shopping-list">
		<div class="header">
			<h1>{{ header }}</h1>
			<button v-if="editing" class="btn" @click="doEdit(false)">
				Cancel
			</button>
			<button v-else class="btn btn-primary" @click="doEdit(true)">
				Add item
			</button>
		</div>
		<form 
			class="add-item-form"
			@submit.prevent="saveItem"
			v-if="editing"
		>
			<input 
				v-model.trim="newItem"
				type="text" 
				placeholder="Add an item"
			>
			<label>
				<input type="checkbox" v-model="newItemHighPriority">
				High Priority
			</label>
			<button 
				:disabled="newItem.length === 0"
				class="btn btn-primary"
			>
				Save item
			</button>
		</form>
		<p class="counter" v-if="editing">
				{{ characterCount }}/200
			</p>
		<ul>
			<li 
				v-for="item in items" 
				:key="item.id"
				:class="{ 
					strikeout: item.purchased,
					priority: item.highPriority
				}"
				@click="togglePurchased(item)"
			>
				{{ item.label }}
			</li>
		</ul>
	</div>
</template>
