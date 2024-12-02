<script lang="ts">
	import { onMount } from 'svelte';
	import Particles, { particlesInit } from '@tsparticles/svelte';
	import { loadSlim } from '@tsparticles/slim';

	// initialise dates
	const currentDate = new Date();
	var xmasDate = new Date(currentDate.getFullYear(), 11, 25);

	// if date exceeds this year's xmas, increase year
	if (currentDate.getMonth() == 11 && currentDate.getDate() > 25) {
		xmasDate.setFullYear(xmasDate.getFullYear() + 1);
	}

	// one day = 1000 * (seconds * minutes * hours) to convert to ms
	const oneDay = 1000 * (60 * 60 * 24);

	// get the nearest integer of the deduction of the xmas date from the current one divided by one day, in ms
	let daysLeft = Math.ceil((xmasDate.getTime() - currentDate.getTime()) / oneDay);

	let particleOptions = {
		particles: {
			color: {
				value: '#ffffff'
			},

			move: {
				enable: true,
				direction: 'bottom',
				speed: 5
			},

			number: {
				density: {
					enable: true,
					area: 500
				},
				// kind of useless overriden
				value: 30
			},

			opacity: {
				value: 0.8
			},

			shape: {
				type: 'circle'
			},

			size: {
				value: 10
			},

			// wobbly wobbly right left
			wobble: {
				enable: true,
				distance: 20,
				speed: 1
			},

			// size randomness
			zIndex: {
				value: { min: 0, max: 150 }
			}
		}
	};

	if (daysLeft == 0) {
		particleOptions.particles.number.value = 60;
	} else {
		// dynamic properties dependant on days left
		// formula: minimumSnowAmount + abs(daysLeft - maxYearDays) * ratioMultiplier
		const snowToOutput = 1 + Math.abs((daysLeft - 366) * 0.4);

		particleOptions.particles.number.value = snowToOutput;
	}

	onMount(() => {
		// get days element
		const xmasDays = document.getElementById('xmas-days');

		// nothing else works (!, ?, as HTMLElement) ^
		if (xmasDays) {
			if (daysLeft == 0) {
				xmasDays.textContent = 'Merry Christmas!';
			} else {
				// adjust if last day
				daysLeft > 1
					? (xmasDays.textContent = daysLeft + ' days till Christmas!')
					: (xmasDays.textContent = daysLeft + ' day till Christmas!');
			}
		}
	});

	void particlesInit(async (engine) => {
		// call this once per app
		// you can use main to customize the tsParticles instance adding presets or custom shapes
		// this loads the tsparticles package bundle, it's the easiest method for getting everything ready
		// starting from v2 you can add only the features you need reducing the bundle size
		//await loadFull(engine);
		await loadSlim(engine);
	});
</script>

<Particles options={particleOptions} />

<div class="fixed bottom-0 left-0 right-0 top-0 m-auto h-max w-max text-center">
	<h1 class="text-4xl text-white md:text-6xl lg:text-8xl">{daysLeft} Days till christmas</h1>
</div>
