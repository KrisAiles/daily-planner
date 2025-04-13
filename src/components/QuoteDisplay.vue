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
    quote.value = quotes.value[index].quote;
    author.value = quotes.value[index].author;
    count++;
}

setInterval(cycleQuotes, 30000);

</script>

<template>
    <div id="quote-container">
        <div id="quote-content">
            <p>"{{ quote }}"</p>
            <span>{{ author }}</span>
        </div>
    </div>
    
</template>

<style lang="scss" scoped>
@use '../assets/variables.scss';

#quote-container {
    flex: 1 1 auto;
    width: 31.875rem;
    min-height: 20.5rem;
    padding: 1.4375rem;  
}

#quote-content {
    min-height: 17.625rem;
    background-image: url('/quote-background.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    padding: 1.4375rem;
    border-radius: 0.3125rem;
    box-shadow: variables.$box-shadow;  

    p {
        font-size: 2rem;
        font-weight: 700;
        color: white;
        text-shadow: 0.0625rem 0.0625rem 0.125rem black;
    }

    span {
        font-style: italic;
        font-size: 20px;
        color: white;
        text-shadow: 0.0625rem 0.0625rem 0.125rem black;
    }
}

</style>