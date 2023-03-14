<script lang="ts">
	import { nanoid } from 'nanoid';

	type Deck = Array<string>;

	//
	let size = 40;
	let times = 7;

	// 1. Creazione "mazzo di carte"
	let deck: Deck = [];

	for (let i = 0; i < size; i++) {
		deck.push(nanoid(5));
	}

	// 2. Funzioni utilità

	function cutDeck(d: Deck): [Deck, Deck] {
		const halfIndex = size % 2 == 0 ? size / 2 : Math.floor(size / 2) + 1;
		const halfA = d.slice(0, halfIndex);
		const halfB = d.slice(halfIndex);

		return [halfA, halfB];
	}

	function riffleShuffle(d1: Deck, d2: Deck): Deck {
		// Controllare che la differenza tra i due mazzi sia solo di una carta
		if (!(d1.length > d2.length) && Math.abs(d1.length - d2.length) > 2) {
			throw new Error('La lunghezza dei mazzi non è corretta');
		}

		// Creiamo un mazzo nuovo "vuoto"
		const deck: Deck = [];

		const length = d1.length + d2.length;

		let count1 = 0;
		let count2 = 0;

		for (let i = 0; i < length; i++) {
			if (i % 2 == 0) {
				deck.push(d1[count1]);
				count1 += 1;
			} else {
				deck.push(d2[count2]);
				count2 += 1;
			}
		}

		return deck;
	}

	//

	function routine(deck: Deck, times: number, size: number): Array<number> {
		let shuffledDeck = deck;

		for (let i = 0; i < times; i++) {
			shuffledDeck = riffleShuffle(...cutDeck(shuffledDeck));
		}

		const distances: Array<number> = [];
		for (let i = 0; i < shuffledDeck.length - 1; i++) {
			const dist = Math.abs(shuffledDeck.indexOf(deck[i]) - shuffledDeck.indexOf(deck[i + 1])) - 1;
			distances.push(dist);
		}

		return distances;
	}

	//
	$: console.log(routine(deck, times, size), times);
</script>

<!--  -->
<input type="number" name="" id="" bind:value={size} />

{times}
<button
	class="p-4 bg-slate-400"
	on:click={() => {
		times++;
	}}>+</button
>
<button
	class="p-4 bg-slate-400"
	on:click={() => {
		times--;
	}}>-</button
>
