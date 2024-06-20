<template>
  <div class="app">
    <HeaderWeb title="The Anime Quoter" />
    <QuoteBlock :quote="quote" />
    <div class="button-container">
      <button @click="generateQuote">Generate Quote</button>
    </div>
  </div>
</template>

<script>
import HeaderWeb from './components/HeaderWeb.vue';
import QuoteBlock from './components/QuoteBlock.vue';

export default {
  name: 'App',
  components: {
    HeaderWeb,
    QuoteBlock
  },
  data() {
    return {
      quote: {
        content: 'Click the button to generate a quote',
        character: '',
        anime: ''
      },
      quotes: []
    }
  },
  methods: {
    async generateQuote() {
      try {
        const response = await fetch('https://type.fit/api/quotes');
        const quoteList = await response.json();
        const randomIdx = Math.floor(Math.random() * quoteList.length);
        const quoteText = quoteList[randomIdx].text;
        const auth = quoteList[randomIdx].author || 'Anonymous';

        this.quote.content = quoteText;
        this.quote.character = auth;
        this.quote.anime = ''; // You might want to assign an appropriate value here
      } catch (error) {
        console.error('Error fetching quote:', error);
      }
    }
  }
}
</script>

<style lang="scss">
:root {
  --primary: #D81E5B;
  --secondary: #8A4FFF;
  --tertiary: #32CBFF;
  --dark: #131A26;
  --light: #EEE;
  --grey: #848484;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}

.button-container {
  display: flex;
  justify-content: center;
  padding: 0px 32px;
  margin: 64px auto;

  button {
    border: none;
    outline: none;
    background-color: var(--primary);
    padding: 16px 32px;
    border-radius: 99px;

    color: var(--light);
    font-size: 1.5em;
    font-weight: 700;
    text-transform: uppercase;
    cursor: pointer;
    transition: 0.4s;

    &:hover {
      background-color: var(--secondary);
    }
  }
}
</style>
