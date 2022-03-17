<template>
	<div class="app">
		<game-page />
	</div>
</template>

<script>
import '@/assets/css/normalize.css';
import GamePage from '@/views/GamePage.vue';

export default {
    name: 'App',
	components: {
		GamePage
	}
}
</script>

<style lang="scss" scoped>
body{
	background: #000;
}
</style>
