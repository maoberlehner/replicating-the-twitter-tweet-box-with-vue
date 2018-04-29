<template>
  <div :class="$options.name">
    <div
      :class="`${$options.name}__htmlarea`"
      aria-hidden
      v-html="htmlValue"
    />
    <textarea
      ref="textarea"
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
    limit: {
      type: Number,
      default: 140,
    },
    value: {
      type: String,
    },
  },
  computed: {
    htmlValue() {
      return `${this.value.slice(0, this.limit)}<em>${this.value.slice(this.limit)}</em>`;
    },
    limitStatus() {
      return (this.value.length / this.limit) * 100;
    },
    textareaStyle() {
      return getComputedStyle(this.$refs.textarea);
    },
  },
  // It might be tempting to use a watcher instead of
  // triggering `textareaGrow()` in both, the `mounted()`
  // lifecycle hook and in the `updateValue()` method
  // but because watchers, which are set to run immediately,
  // are triggered before evaluating computed properties,
  // a watcher wouldn't work.
  mounted() {
    this.textareaGrow();
  },
  methods: {
    updateValue(e) {
      this.textareaGrow();
      this.$emit(`input`, e.target.value);
    },
    // Update the size of the textarea to fit the number
    // of lines of text.
    textareaGrow() {
      const paddingTop = parseInt(this.textareaStyle.getPropertyValue(`padding-top`), 10);
      const paddingBottom = parseInt(this.textareaStyle.getPropertyValue(`padding-bottom`), 10);
      const lineHeight = parseInt(this.textareaStyle.getPropertyValue(`line-height`), 10);

      // Resetting the row count to `1` is necessary for
      // recalculating the `scrollHeight` of the textarea.
      this.$refs.textarea.rows = 1;

      // We're calculating the inner height of the textare
      // and take this value to also calculate the number
      // of rows needed to fit the currently entered text.
      const innerHeight = this.$refs.textarea.scrollHeight - paddingTop - paddingBottom;
      this.$refs.textarea.rows = innerHeight / lineHeight;
    },
  },
};
</script>

<style lang="scss">
.TweetBox {
  $color-border: #99dde6;
  $color-danger-light: #ffb8c2;

  position: relative;

  // 1. Account for the width of the remaining characters
  //    and visual counters.
  // 2. Harmonize differences between <div> and <textarea>.
  &__htmlarea,
  &__textarea {
    padding: 1em;
    padding-right: 3.75em; // 1
    width: 100%; // 2
    line-height: 1.25; // 2
    border: 2px solid transparent;
    border-radius: 0.5em;
  }

  // 1. Remove the element from the normal document flow,
  //    so the <textarea> lies above this element, and
  //    make the HTML area <div> as tall as the <textarea>.
  // 2. Make word breaks behave exactly like in a textarea.
  &__htmlarea {
    position: absolute; // 1
    height: 100%; // 1
    background-color: #fff;
    white-space: pre-wrap; // 2
    word-wrap: break-word; // 2
  }

  // 1. Make the <textarea> a block level element to make
  //    its sizing behave like that of a <div>.
  // 2. By making the background color and the font color
  //    transparent, the user sees the content of the HTML
  //    area <div> behind the <textarea>. Because by default
  //    the caret color is the same as the font color, we
  //    explicitly set the `caret-color` property, otherwise
  //    the caret would be transparent too.
  &__textarea {
    display: block; // 1
    position: relative;
    border-color: $color-border;
    outline: 0;
    background-color: transparent; // 2
    color: transparent; // 2
    caret-color: #444; // 2
    resize: none;

    &:focus {
      border-color: darken($color-border, 20%);
    }
  }

  em {
    background: $color-danger-light;
  }
}
</style>
