<template>
	<li class="flex py-4 gap-4 items-center">
		<button @click="removeFromWishlist" title="Remove Item"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" width="24" height="24">
				<path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="32" d="M368 368L144 144M368 144L144 368" />
			</svg></button>
		<NuxtLink :to="`/product/${product.slug}`">
			<img class="rounded-xl h-12 w-12 md:h-20 md:w-20" v-if="product.image" :src="product.image.sourceUrl" :alt="product.image.altText || product.name" :title="product.image.altText || product.name" width="80" height="80" />
		</NuxtLink>
		<NuxtLink class=" text-lg leading-tight" :to="`/product/${product.slug}`">{{ product.name }}</NuxtLink>
		<SaleBadge :node="product" />
		<ProductPrice :salePrice="product.salePrice" :regularPrice="product.regularPrice" class="ml-auto" />
	</li>
</template>

<script>
export default {
	props: {
		product: { type: Object, required: true },
	},
	methods: {
		removeFromWishlist() {
			this.$store.commit('removeFromWishlist', this.product);
		},
	},
};
</script>