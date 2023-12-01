<template>
  <div
    class="pointer-events-auto z-10 w-[24.125rem] rounded-lg bg-white dark:bg-gray-700 text-[0.8125rem] leading-5 text-gray-700 shadow-xl shadow-black/5 ring-1 ring-slate-700/10 dark:text-gray-400"
  >
    <div class="mx-3">
      <h2 class="my-3 inline-block text-base leading-tight">Create Note</h2>
      <SuggestionsBox rows="5" :suggestions="suggestions" @update="updateNote"></SuggestionsBox>

      <div class="my-6 flex justify-between">
        <DefaultButton @click="$emit('cancel')">Cancel</DefaultButton>
        <DefaultButton @click="createNote()">Create</DefaultButton>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['field', 'params', 'anonymous'],

  emits: ['cancel'],

  data() {
    return {
      note: '',
      showValueRequirement: false,
    };
  },

  computed: {
    suggestions() {
      if (!this.field.suggestions) {
        return [];
      }

      return this.field.suggestions;
    },
  },

  methods: {
    updateNote(value) {
      this.note = value;
    },

    async createNote() {
      try {
        await Nova.request().post(`/nova-vendor/nova-notes/notes`, { note: this.note }, { params: this.params });
        this.$emit('fetchNotes');
        this.$emit('cancel');
      } catch (e) {
        Nova.error(this.__('There was a problem submitting the form.'));
      }

      this.note = '';
    },

    checkForValueRequirement() {
      if (!this.note) {
        this.showValueRequirement = true;
      }

      this.showValueRequirement = false;
    },
  },
};
</script>
