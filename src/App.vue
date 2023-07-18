<script setup>
import Card from './components/Card.vue'
</script>

<template>
	<div class="dealer">
		<Card suit="♠" :rank="1" :isOpen="true"></Card>
		<Card suit="♣" :rank="10" :isOpen="false"></Card>
		<Card suit="◆" :rank="11" :isOpen="true"></Card>
		<Card suit="♥" :rank="13" :isOpen="true"></Card>
	</div>
</template>

<style scoped>
.dealer {
	display: flex;
	flex-wrap: wrap;
}
</style>
