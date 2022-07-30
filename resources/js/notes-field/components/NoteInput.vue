<template>
  <div class="mb-4 flex" :class="fullWidth ? 'w-full' : 'w-3/5'">
    <div v-if="trixEnabled">
      <trix-editor
        ref="trixEditor"
        @keydown.stop
        @trix-change="$emit('update:modelValue', $event.target.value.trim())"
        @trix-initialize="initialize"
        :value="modelValue"
        :placeholder="placeholder"
        class="trix-content w-full form-control form-input form-input-bordered py-3 h-auto"
      />
    </div>

    <textarea
      v-else
      rows="3"
      :placeholder="placeholder"
      class="form-control w-full form-input form-input-bordered py-3 h-auto"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value.trim())"
      @keydown.enter="onEnter"
    />

    <div class="whitespace-no-wrap flex-shrink-0 ml-2">
      <button
        class="btn btn-default btn-primary relative ml-auto shadow bg-primary-500 hover:bg-primary-400 active:bg-primary-600 text-white dark:text-gray-900 cursor-pointer rounded text-sm font-bold focus:outline-none focus:ring inline-flex items-center justify-center h-9 px-3 bg-primary-500 hover:bg-primary-400 active:bg-primary-600 disabled:opacity-50"
        @click="$emit('onSubmit')"
        type="button"
        :disabled="loading || !modelValue"
      >
        {{ __('novaNotesField.addNote') }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['placeholder', 'modelValue', 'loading', 'trixEnabled', 'fullWidth'],
  emits: ['update:modelValue'],
  methods: {
    initialize() {
      this.$refs.trixEditor.editor.loadHTML(this.modelValue);
    },

    onEnter(e) {
      if (e.shiftKey) return true;

      e.preventDefault();
      e.stopPropagation();
      this.$emit('onSubmit');
      return true;
    },
  },

  watch: {
    modelValue(newValue, oldValue) {
      if (this.trixEnabled && this.$refs.trixEditor) {
        if (!newValue && !!oldValue) this.$refs.trixEditor.editor.loadHTML('');
      }
    },
  },
};
</script>
