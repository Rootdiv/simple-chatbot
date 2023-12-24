<script setup lang="ts">
	import { ref, toRefs } from 'vue';
	import { Message } from '../types';

	const props = defineProps<{
		messages: Message[];
	}>();

	const emit = defineEmits(['is-user-send']);

	const { messages } = toRefs(props);

	const userMessage = ref<string>('');

	const sendMessage = () => {
		if (userMessage.value.trim() === '' || !/[a-zёа-я]+/i.test(userMessage.value)) {
			userMessage.value = '';
			return;
		}

		messages.value.push({
			id: messages.value.length + 1,
			content: userMessage.value.trim(),
			bot: false,
		});
		userMessage.value = '';

		emit('is-user-send');
	};
</script>

<template>
	<div class="chat__form form">
		<form class="form__main" @submit.prevent="sendMessage">
			<input v-model="userMessage" class="form__input" type="text" placeholder="Введите Ваше сообщение" />
			<button class="form__button" type="submit">Отправить</button>
		</form>
	</div>
</template>

<style scoped lang="scss">
	.form {
		border-bottom-left-radius: 10px;
		border-bottom-right-radius: 10px;
		background-color: #58c3e7;
		text-align: center;
		padding: 20px;

		&__main {
			border-radius: 10px;
			display: flex;
			align-items: center;
			justify-content: space-between;
			gap: 20px;

			@media (max-width: 375px) {
				flex-wrap: wrap;
			}
		}

		&__input {
			flex-basis: 100%;
			border-radius: 7px;
			padding: 10px;
			border: none;
		}

		&__button {
			width: 100px;
			background-color: #d5d1f1;

			@media (max-width: 375px) {
				width: 100%;
			}

			&:hover {
				background-color: #c9c2fa;
			}
		}
	}
</style>
q
