<script setup>
import { ref, computed } from 'vue'
import Card from './components/Card.vue'

let deck = []
const isPlaying = ref(true)	// ゲームプレイ中 or 判定中
const dealerCards = ref([])	// ディーラーの持ち札
const playerCards = ref([])	// プレイヤーの持ち札
initialize()				// 初期化してゲームを開始

function initialize(){
	deck = []
	dealerCards.value = []
	playerCards.value = []

	const suit = ['♥','♠','♣','◆']
				
	// デッキ（トランプの束）の作成
	// デッキは各スート（♥♠♣◆）の1〜13までのカード（13*4=52枚）とし、ジョーカーは含まない
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

	// ディーラーとプレイヤーに２枚づつカードを配り、ゲームの初期状態を作る
	dealerCards.value.push(hit())
	dealerCards.value.push(hit())
	playerCards.value.push(hit())
	playerCards.value.push(hit())
}

// ヒット：カードを一枚引く
function hit(){
	return deck.shift()
}

// これ以上カードを引かず、この手で勝負する
function stand(){
	isPlaying.value = false
	// ディーラーは17以上になるまでカードを追加で引く
	dealerAction()
}

// もう一度最初からプレイ
function replay(){
	isPlaying.value = true
	initialize()
}

// 手札の点数を計算
function score(cards, isLower = false){
	if(cards.length===0) return 0
	let aces = 0
	let score = 0
	for(let i=0;i<cards.length;i++){
		switch(cards[i].rank) {
			case 1:
				aces++
				score+=1	// 現段階ではaceは1とみなす
				break;
			case 11:
			case 12:
			case 13:
				score+=10	// 絵札は+10
				break;
			default:
				score+=cards[i].rank
				break;
		}
	}
	if(isLower) return score	// ace をすべて1とみなした場合の数を返す
	// 21を超えない範囲でace を1→11とする(21まではOK)
	for(let i=0; i<aces; i++){
		if(score + 10 <= 21){
			score += 10
		}
	}
	
	return score
}

function printScore(score){
	if(score < 21){
		return score + '点'
	}else if(score === 21){
		return 'ブラックジャック'
	}else{
		return 'バスト'
	}
}

const dealerScore =	computed(() => score(dealerCards.value))
const playerScore = computed(() => score(playerCards.value))
const dealerScorePrint = computed(() => printScore(dealerScore.value))
const playerScorePrint = computed(() => printScore(playerScore.value))
const judge = computed(() => {
	// プレイヤーもディーラーも両方バスト
	if( playerScore.value > 21 && dealerScore.value > 21 ){
		return '引き分け'
	}
	// 親がバスト
	else if(dealerScore.value > 21){
		return 'プレイヤーの勝ち'
	}
	// プレイヤーがバスト
	else if(playerScore.value > 21){
		return 'ディーラーの勝ち'
	}
	// 両者ともバストしていない　
	else if( playerScore.value === dealerScore.value ){
		return '引き分け'
	}else if( playerScore.value > dealerScore.value ){
		return 'プレイヤーの勝ち'
	}else{
		return 'ディーラーの勝ち'
	}
})

// 親は17以上になるまでカードを引き続ける
function dealerAction(){
	while(score(dealerCards.value, true) < 17){
		console.log('（親：次のカードを引きます）')
		dealerCards.value.push(hit())
	}
}
</script>

<template>
	<h1>ブラックジャック</h1>
	<!-- ゲームのプレイ中の画面 -->
	<div v-if="isPlaying">
		<section>
			<h2>ディーラー</h2>
			<div class="cards">
				<!-- 二枚目のみ非表示 -->
				<Card v-for="(card,index) in dealerCards"
					:suit="card.suit"
					:rank="card.rank"
					:isOpen="index === 1 ? false : true"
				>
				</Card>
			</div>
		</section>
		<section>
			<h2>プレイヤー</h2>
			<div class="cards">
				<Card v-for="card in playerCards" :suit="card.suit" :rank="card.rank" :isOpen="true"></Card>
			</div>
			<div class="functions">
				<!--
				<button type="button" @click="hit">追加する</button>
				-->
				<button type="button" @click="stand">この手で勝負する</button>
			</div>
		</section>
	</div>
	<!-- ゲーム終了し、判定画面 -->
	<div v-else>
		<section>
			<h2>ディーラー</h2>
			<div class="cards">
				<Card v-for="(card,index) in dealerCards"
					:suit="card.suit"
					:rank="card.rank"
					:isOpen="true"
				>
				</Card>
			</div>
			<p>合計:{{dealerScorePrint}}</p>
		</section>
		<section>
			<h2>プレイヤー</h2>
			<div class="cards">
				<Card v-for="card in playerCards" :suit="card.suit" :rank="card.rank" :isOpen="true"></Card>
			</div>
			<p>合計:{{playerScorePrint}}</p>
		</section>
		<p>{{judge}}</p>
		<div class="functions">
			<button type="button" @click="replay">もう一度プレイする</button>
		</div>
	</div>
</template>

<style scoped>
.cards {
	display: flex;
	flex-wrap: wrap;
}

.functions {
	margin: 1rem;
	display: flex;
	gap: 1rem;
}

button {
	font-size: 2rem;
}
</style>
