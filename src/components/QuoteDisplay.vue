<script setup lang="ts">
import { ref } from 'vue';

const quotes = ref<{id: number, quote: string, author: string}[]>([]);
const quote = ref('');
const author = ref('');
let count = 0;

async function fetchQuotes() {
    const response = await fetch("https://qapi.vercel.app/api/quotes");
    const json = await response.json();
    for (let i = 0; i < json.length; i++) {
        quotes.value.push(json[i]);
    }
    cycleQuotes();
}

fetchQuotes();

function cycleQuotes() {
    const index = count % 30;
    console.log(index);
    quote.value = quotes.value[index].quote;
    author.value = quotes.value[index].author;
    count++;
}

//setInterval(cycleQuotes, 30000);

</script>

<template>
    <div id="quote-container">
        <p>"{{ quote }}"</p>
        <span>{{ author }}</span>
    </div>
</template>

<style lang="scss" scoped>
#quote-container {
    width: 100%;
    height: 282px;
    padding: 23px;
    background-image: url('/quote-background.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    margin: 23px;
    border-radius: 5px;
    box-shadow: 2px 2px 5px black;

    p {
        font-size: 32px;
        font-weight: 700;
        color: white;
        text-shadow: 1px 1px 2px black;
    }

    span {
        font-style: italic;
        font-size: 20px;
        color: white;
        text-shadow: 1px 1px 2px black;
    }
}

</style>