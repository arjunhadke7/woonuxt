<template>
	<div class="inline-flex items-center">
		<svg v-for="i in 5" :key="i" width="14" height="14" :fill="rating < i ? '#ccc' : '#FBBE24'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class=" mr-[2px]">
			<path d="M394 480a16 16 0 0 1-9.39-3L256 383.76 127.39 477a16 16 0 0 1-24.55-18.08L153 310.35 23 221.2a16 16 0 0 1 9-29.2h160.38l48.4-148.95a16 16 0 0 1 30.44 0l48.4 149H480a16 16 0 0 1 9.05 29.2L359 310.35l50.13 148.53A16 16 0 0 1 394 480z" />
		</svg>
		<span v-if="count !== null" class="ml-1 text-xs text-gray-500">({{count}})</span>
	</div>
</template>

<script>
export default {
	props: ['rating', 'count']
}
</script>
