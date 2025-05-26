<script lang="ts">
	const textToType =
		'The cunning fox jumped upon the lazy dog. The lazy dog then yawned and went back to sleeping';

	let typedText = $state('');
	let timer = $state(30);
	let comparisionResult = $state<boolean[]>([]);

    let wordsPerMinute = $state()

	$effect(() => {
		const interval = setInterval(() => {
			if (typedText.length > 0) {
				if (timer > 0) {
					timer--;
				} else {
					clearInterval(interval);
				}
			}
		}, 1000);
		return () => clearInterval(interval);
	});

	$effect(() => {
		comparisionResult = Array.from(typedText).map((char, index) => char === textToType[index]);        
        const cleanedTypedText = typedText.replace(/\s+/g, ' ').trim();
		if (cleanedTypedText === '') {
			comparisionResult = Array(textToType.length).fill(undefined);
		} else {
			comparisionResult = Array.from(cleanedTypedText).map(
				(char, index) => char === textToType[index]
			);
		}
         wordsPerMinute = typedText.length/timer
	});

	
</script>

<div class="flex flex-col gap-5">
	<span class="text-2xl text-yellow-500">Time Left : {timer}</span>

	<div class="relative">
		<p class="whitespace-pre-wrap">
			{#each Array.from(textToType) as char, i}
				{#if comparisionResult[i] === true}
					<span class="text-blue-500">{char}</span>
				{:else if comparisionResult[i] === false}
					<span class="text-red-500">{char}</span>
				{:else}
					<span class="text-gray-400">{char}</span>
				{/if}
			{/each}
		</p>
		<span class="w-0.5 h-5 inline-block bg-yellow-500"></span>
		<div
			contenteditable="true"
			bind:innerText={typedText}
			role="presentation"
			class="absolute top-0 left-0 w-full border-none text-2xl text-transparent outline-none"
		></div>
	</div>
    {wordsPerMinute}

	{typedText}
</div>
