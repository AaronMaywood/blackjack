<script setup>
import { ref } from 'vue'
import Card from './components/Card.vue'

// デッキ（トランプの束）の作成
// デッキは各スート（♥♠♣◆）の1〜13までのカード（13*4=52枚）とし、ジョーカーは含まない
let deck = []
const suit = ['♥','♠','♣','◆']

for(let i=0;i<suit.length;i++){
	for(let j=1;j<=13;j++){
		deck.push({
			rank:j,
			suit:suit[i]
		})
	}
}

// デッキのシャッフル
// 別のやり方：https://ja.javascript.info/task/shuffle
for(let i=0; i<deck.length ; i++){
	const rand = Math.floor(Math.random()*deck.length)
	// [deck[i],deck[rand]] = [deck[rand],deck[i]] // なぜかエラーになる
	const tmp = deck[i]
	deck[i] = deck[rand]
	deck[rand] = tmp
}

// ヒット：カードを一枚引く
function hit(){
	return deck.shift()
}

const dealerCards = ref([])
const playerCards = ref([])
dealerCards.value.push(hit())
dealerCards.value.push(hit())
playerCards.value.push(hit())
playerCards.value.push(hit())
</script>

<template>
	<h1>ブラックジャック</h1>
	<section>
		<h2>ディーラー</h2>
		<div class="cards">
			<Card v-for="card in dealerCards" :suit="card.suit" :rank="card.rank" :isOpen="true"></Card>
		</div>
	</section>
	<section>
		<h2>プレイヤー</h2>
		<div class="cards">
			<Card v-for="card in playerCards" :suit="card.suit" :rank="card.rank" :isOpen="true"></Card>
		</div>
	</section>
</template>

<style scoped>
.cards {
	display: flex;
	flex-wrap: wrap;
}
</style>
