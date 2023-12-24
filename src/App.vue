<script setup lang="ts">
	import MessagesBox from './components/MessagesBox.vue';
</script>

<template>
	<div class="chat chat__container">
		<div class="chat__title">Простой чат-бот</div>
		<MessagesBox />
	</div>
</template>

<style scoped lang="scss">
	.chat {
		padding: 10px;

		&__title {
			border-top-left-radius: 10px;
			border-top-right-radius: 10px;
			background-color: #58c3e7;
			text-align: center;
			padding: 10px;
		}

		&__container {
			display: grid;
			grid-template-rows: max-content 1fr max-content;
			max-width: 500px;
			width: 100%;
		}
	}
</style>
