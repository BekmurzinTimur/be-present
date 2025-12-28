<script lang="ts">
	let isActive = $state(false);
	let volume = $state(25);
	let interval = $state(1);
	let intervalId = $state<NodeJS.Timeout | null>(null);
	let audio: HTMLAudioElement | null = null;

	const intervals: number[] = [1, 3, 5, 10, 20];

	function playChime(): void {
		console.log('playChime');
		if (!audio) {
			audio = new Audio('/assets/bell.mp3');
		}
		audio.volume = volume / 100;
		audio.currentTime = 0;
		audio.play();
	}

	function startTimer(): void {
		if (intervalId) clearInterval(intervalId);
		playChime();
		intervalId = setInterval(playChime, interval * 60 * 1000);
	}

	function stopTimer(): void {
		if (intervalId) {
			clearInterval(intervalId);
			intervalId = null;
		}
	}

	const handleToggle = () => {
		isActive = !isActive;
		if (isActive) startTimer();
		else stopTimer();
	};
</script>

<div class="flex min-h-screen w-full flex-col items-center gap-6 bg-[#f5f0e8] p-8 font-sans">
	<header class="flex items-center gap-3">
		<h1 class="m-0 text-3xl font-extrabold tracking-wide text-[#2d3a2d]">BE PRESENT</h1>
		<div
			class="h-4 w-4 rounded-full border-2 border-[#4a7c59] transition-colors duration-300"
			class:bg-[#4a7c59]={isActive}
		></div>
	</header>

	<div class="flex w-full max-w-xs flex-col items-center gap-5">
		<div class="flex flex-col items-center gap-2">
			<button
				class="relative h-[38px] w-[70px] cursor-pointer rounded-full border-none p-[3px] transition-colors duration-300 {isActive
					? 'bg-[#4a7c59]'
					: 'bg-gray-300'}"
				onclick={handleToggle}
				aria-pressed={isActive}
				aria-label="Bell toggle"
			>
				<span
					class="absolute top-[3px] left-[3px] h-8 w-8 rounded-full bg-white shadow transition-transform duration-300 {isActive
						? 'translate-x-8'
						: ''}"
				></span>
			</button>
			<span class="text-base font-semibold text-[#4a7c59]">{isActive ? 'Active' : 'Inactive'}</span>
		</div>

		<div class="w-full rounded-2xl bg-white px-5 py-4 shadow-sm">
			<div class="mb-3 flex justify-between text-base text-[#5a6a5a]">
				<span>Volume</span>
				<span>{volume}%</span>
			</div>
			<input
				type="range"
				min="0"
				max="100"
				bind:value={volume}
				class="h-1.5 w-full cursor-pointer appearance-none rounded bg-gray-200 outline-none
					[&::-moz-range-thumb]:h-[18px] [&::-moz-range-thumb]:w-[18px] [&::-moz-range-thumb]:cursor-pointer [&::-moz-range-thumb]:rounded-full [&::-moz-range-thumb]:border-none [&::-moz-range-thumb]:bg-[#4a7c59] [&::-moz-range-thumb]:shadow
					[&::-webkit-slider-thumb]:h-[18px] [&::-webkit-slider-thumb]:w-[18px] [&::-webkit-slider-thumb]:cursor-pointer [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:bg-[#4a7c59] [&::-webkit-slider-thumb]:shadow"
			/>
		</div>

		<div class="w-full rounded-2xl bg-white p-5 shadow-sm">
			<p class="m-0 mb-4 text-center text-lg text-[#3d4a3d]">Ring every</p>
			<div class="flex justify-center gap-2">
				{#each intervals as mins}
					<button
						class="min-w-12 cursor-pointer rounded-lg border px-4 py-2.5 text-[0.95rem] transition-all duration-200 {interval ===
						mins
							? 'border-[#4a7c59] bg-[#4a7c59] text-white'
							: 'border-gray-200 bg-white text-[#5a6a5a] hover:bg-gray-100'}"
						onclick={() => (interval = mins)}
					>
						{mins}m
					</button>
				{/each}
			</div>
		</div>
	</div>

	<p class="mt-2 text-center text-[0.95rem] text-[#4a7c59]">
		Stay present and focused with gentle reminders
	</p>
</div>
