<script setup lang="ts">
	import { onMounted, ref, watch } from 'vue';
	import { Message } from '../types';
	import MessageForm from './MessageForm.vue';

	const messages = ref<Message[]>([]);

	const options: string[] = ['Заказать пиццу', 'Установить будильник', 'Вывести погоду'];

	const bottomScroll = ref<HTMLDivElement>();

	const sendOption = (option: string) => {
		messages.value.push({
			id: messages.value.length + 1,
			content: option,
			bot: false,
		});

		let response: string = '';
		switch (option) {
			case 'Заказать пиццу':
				response = 'Хорошо, я заказываю пиццу. Что еще могу сделать?';
				break;
			case 'Установить будильник':
				response = 'Будильник установлен. Что еще могу сделать?';
				break;
			case 'Вывести погоду':
				response = 'Сейчас я покажу погоду. Что еще могу сделать?';
				break;
		}

		messages.value.push({
			id: messages.value.length + 1,
			content: response,
			bot: true,
		});
	};

	const userSended = () => {
		messages.value.push({
			id: messages.value.length + 1,
			content: 'Не понял Вашего запроса. Что еще могу сделать?',
			bot: true,
		});
	};

	watch(bottomScroll, () => {
		const options = {
			rootMargin: '0px',
			threshold: 1.0,
		};

		const observer = new IntersectionObserver(([entry]) => {
			if (entry && !entry.isIntersecting && bottomScroll.value?.parentElement) {
				const chatContainer = bottomScroll.value.parentElement;
				chatContainer.scrollTop = chatContainer.scrollHeight;
			}
		}, options);

		if (bottomScroll.value) {
			observer.observe(bottomScroll.value);
		}
	});

	onMounted(() => {
		setTimeout(() => {
			messages.value.push({
				id: 1,
				content: 'Привет! Что я могу для Вас сделать?',
				bot: true,
			});
		}, 500);
	});
</script>

<template>
	<div class="chat__messages">
		<TransitionGroup name="chat">
			<div v-for="item in messages" :key="item.id">
				<div :class="[item.bot ? 'chat__bot' : 'chat__user']">{{ item.content }}</div>
				<ul v-if="item.bot" class="chat__options options" ref="optionsRef">
					<li v-for="(option, index) in options" :key="index" class="options__item">
						<button type="button" @click="sendOption(option)" class="options__button">{{ option }}</button>
					</li>
				</ul>
			</div>
		</TransitionGroup>
		<div ref="bottomScroll"></div>
	</div>
	<MessageForm :messages="messages" @isUserSend="userSended" />
</template>

<style scoped lang="scss">
	.chat {
		&__messages {
			height: 500px;
			overflow: auto;
			scroll-behavior: smooth;
			border: 1px solid #58c3e7;
			padding: 15px;
			color: #ffffff;
		}

		&__message {
			display: flex;
			align-items: center;
			gap: 5px;
			margin-bottom: 10px;
		}

		&__image {
			width: 60px;
			height: 60px;
		}

		&__bot {
			padding: 18px;
			border-radius: 18px 18px 18px 0;
			background-color: #6e48aa;
			width: 50%;
		}

		&__user {
			padding: 18px;
			margin-left: auto;
			border-radius: 18px 18px 0;
			background-color: #d5d1f1;
			color: #000000;
			width: 50%;
		}

		&-enter-active {
			transition: all 0.4s ease;
		}

		&-enter-from {
			opacity: 0;
			transform: translateY(30px);
		}
	}

	.options {
		padding: 0;
		display: flex;
		align-items: center;
		gap: 10px;

		@media (max-width: 375px) {
			flex-wrap: wrap;
		}

		&__item {
			list-style: none;
			width: 130px;
			margin: 0;
		}

		&__button {
			border: none;
			background-color: #6e48aa;
			color: inherit;

			&:hover {
				background-color: rgba(110, 72, 170, 0.85);
			}
		}
	}
</style>
