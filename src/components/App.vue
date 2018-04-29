<template>
  <div :class="$options.name">
    <div :class="`${$options.name}__messageBox`">
      <tweet-box v-model="tweet" />
    </div>
  </div>
</template>

<script>
import TweetBox from './TweetBox.vue';

export default {
  name: `App`,
  components: {
    TweetBox,
  },
  data() {
    return {
      tweet: ``,
    };
  },
};
</script>

<style lang="scss">
@import '~normalize.css';
@import '~reset-css/_reset.scss';

* {
  box-sizing: border-box;
}

html body {
  color: #444;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}

.App {
  &__messageBox {
    padding: 1em;
    background-color: #e5f6f8;
  }
}
</style>
