<script setup>
const props = defineProps({
  suit: String,	// ♠♥♣◆
  rank: Number,	// 1〜13
  isOpen: Boolean,
})
</script>

<template>
	<div class="card">
		<p v-if="isOpen === 'true'">
			<span v-if="rank === '1'">A</span>
			<span v-else-if="rank === '11'">J</span>
			<span v-else-if="rank === '12'">Q</span>
			<span v-else-if="rank === '13'">K</span>
			<span v-else>{{rank}}</span>
			{{suit}}
		</p>
		<p v-else>？？？</p>
	</div>
</template>

<style scoped>
.card {
	border: 1px solid;
	border-radius: 10px;
	width: 120px;
	height: 180px;
	background: lightyellow;
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>
