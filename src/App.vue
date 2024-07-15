<script setup>
import { nextTick, ref } from 'vue';

const isOpen = ref(true);
const text = ref('');
const messagesRef = ref(null);
const messages = ref([
  {
    id: 1,
    text: 'Oh there!!!',
    is_bot: true,
    created_at: '10 seconds ago'
  },
  {
    id: 2,
    text: 'Hello, who are you?',
    is_bot: true,
    created_at: '10 seconds ago'
  },
  {
    id: 3,
    text: 'I want to buy some cat\'s food!',
    is_bot: false,
    created_at: '20 seconds ago'
  }
]);

const toggle = () => {
  isOpen.value = !isOpen.value;
};

const restart = () => {
  messages.value = [];
  setTimeout(() => {
    messages.value = [
      {
        id: 999999,
        text: 'Oke let start again',
        is_bot: true,
        created_at: 'now'
      }
    ]
  }, 500);
}

const onLeave = (el, done) => {
  el.classList.add('scale-down-bottom-right');
  setTimeout(() => {
    done()
  }, 400);
}

const submit = () => {
  if (text.value.trim() === '') return;

  messages.value.push({
    id: messages.value.length + 1,
    text: text.value,
    is_bot: false,
    created_at: 'now'
  });
  text.value = '';

  nextTick(() => {
    messagesRef.value.scrollTo(0, messagesRef.value.scrollHeight);
  });
}
</script>

<template>
  <main>
    <button
      class="w-12 h-12 rounded-full bg-emerald-600 text-white flex items-center justify-center fixed bottom-4 right-4 cursor-pointer text-xs"
      @click="toggle">
      <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
        <g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
          color="currentColor">
          <path
            d="M7.5 12h6m-6-4h3m-2 12c1.05.87 2.315 1.424 3.764 1.519c1.141.075 2.333.075 3.473 0a4 4 0 0 0 1.188-.268c.41-.167.614-.25.719-.237c.104.012.255.122.557.342c.533.388 1.204.666 2.2.643c.503-.012.755-.019.867-.208c.113-.19-.027-.452-.308-.977c-.39-.728-.636-1.561-.262-2.229c.643-.954 1.19-2.083 1.27-3.303c.043-.655.043-1.334 0-1.99A6.7 6.7 0 0 0 21.4 11" />
          <path
            d="M12.345 17.487c3.556-.234 6.388-3.08 6.62-6.653c.046-.699.046-1.423 0-2.122c-.232-3.572-3.064-6.418-6.62-6.652c-1.213-.08-2.48-.08-3.69 0c-3.556.234-6.388 3.08-6.62 6.652c-.046.7-.046 1.423 0 2.122c.084 1.302.665 2.506 1.349 3.524c.397.712.135 1.6-.279 2.377c-.298.56-.447.84-.327 1.042s.387.209.922.221c1.057.026 1.77-.271 2.336-.685c.321-.234.482-.351.593-.365c.11-.013.328.075.763.253c.392.16.846.258 1.263.286c1.21.08 2.477.08 3.69 0" />
        </g>
      </svg>
    </button>

    <Transition @leave="onLeave" mode="out-in">
      <div v-if="isOpen" :class="{ 'scale-up-bottom-right': isOpen }"
        class="w-80 bg-white border shadow-2xl rounded-lg fixed bottom-20 right-4 h-[32rem] max-h-[calc(100vh-7rem)] flex flex-col">
        <div class="border-b px-4 py-2 flex items-center gap-4 justify-between">
          <div class="flex items-center gap-1">
            <img src="https://avatars.githubusercontent.com/u/156633402?v=4" alt=""
              class="w-8 h-8 rounded-full border-2 hover:border-emerald-600">
            <div class="flex flex-col">
              <div class="text-sm font-semibold leading-none text-gray-700">
                Shop name
              </div>
              <div class="text-gray-300 text-xs italic leading-none">
                @username
              </div>
            </div>
          </div>
          <div class="flex items-center gap-4">
            <button class="text-gray-600 hover:text-emerald-600" @click="restart">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24">
                <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                  stroke-width="1.5"
                  d="M4 2v3.132c0 .294.367.427.555.201A9.97 9.97 0 0 1 12.005 2C17.525 2 22 6.477 22 12c0 3.958-2.299 7.38-5.633 9m-4.632 1q.7 0 1.376-.092M2.265 8.667Q2.097 9.263 2 9.869m.035 3.669q.105.6.274 1.186m1.524 3.272q.358.531.774 1.019M7.43 21.36q.53.304 1.103.547"
                  color="currentColor" />
              </svg>
            </button>
            <button class="text-gray-600 hover:text-emerald-600" @click="toggle">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24">
                <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                  stroke-width="1.5" d="M19 5L5 19M5 5l14 14" color="currentColor" />
              </svg>
            </button>
          </div>
        </div>
        <div class="grow overflow-y-auto p-4 space-y-1" ref="messagesRef">
          <TransitionGroup name="list" tag="ul">
            <li class="flex flex-col" :class="{ 'items-end': !message.is_bot }" :key="message.id"
              v-for="message in messages">
              <div :class="{ 'bg-emerald-600 !text-white': !message.is_bot }"
                class="p-2 rounded-md shadow w-fit text-sm text-gray-700">
                {{ message.text }}
              </div>
              <span class="text-[10px] text-gray-300 italic">{{ message.created_at }}</span>
            </li>
          </TransitionGroup>
        </div>
        <form class="border-t p-4 pr-16 relative" @submit.prevent="submit">
          <input type="text" placeholder="Enter your message" class="outline-none text-sm w-full" v-model="text">
          <button
            class="w-8 h-8 rounded-full bg-emerald-600 text-white flex items-center justify-center absolute top-1/2 -translate-y-4 right-4">
            <svg class="mr-0.5 mt-0.5 block" xmlns="http://www.w3.org/2000/svg" width="20" height="20"
              viewBox="0 0 24 24">
              <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                d="M21.048 3.053C18.87.707 2.486 6.453 2.5 8.55c.015 2.379 6.398 3.11 8.167 3.607c1.064.299 1.349.604 1.594 1.72c1.111 5.052 1.67 7.566 2.94 7.622c2.027.09 7.972-16.158 5.847-18.447M11.5 12.5L15 9"
                color="currentColor" />
            </svg>
          </button>
        </form>
      </div>
    </Transition>
  </main>
</template>

<style scoped></style>
