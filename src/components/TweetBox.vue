<template>
  <div :class="$options.name">
    <textarea
      :class="`${$options.name}__textarea`"
      :value="value"
      rows="1"
      @input="updateValue"
    />
  </div>
</template>

<script>
export default {
  name: `TweetBox`,
  props: {
    value: {
      type: String,
    },
  },
  methods: {
    updateValue(e) {
      this.$emit(`input`, e.target.value);
    },
  },
};
</script>

<style lang="scss">
.TweetBox {
  $color-border: #99dde6;

  &__textarea {
    padding: 1em;
    width: 100%;
    border: 2px solid $color-border;
    border-radius: 0.5em;
    outline: 0;
    resize: none;

    &:focus {
      border-color: darken($color-border, 20%);
    }
  }
}
</style>
