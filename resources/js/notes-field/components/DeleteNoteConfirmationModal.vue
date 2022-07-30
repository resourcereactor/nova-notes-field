<template>
  <modal max-width="sm" @modal-close="handleClose" @showing="handleShowingModal">
    <form @submit.prevent="handleConfirm" class="mx-auto bg-white dark:bg-gray-800 rounded-lg shadow-lg overflow-hidden">
      <heading :level="3" class="text-90 uppercase tracking-wide font-bold md:text-sm border-b border-gray-100 dark:border-gray-700 py-4 px-8">{{ __('novaNotesField.deleteNoteTitle') }}</heading>
      <div class="px-8 py-3">
        <p class="text-80 leading-normal">
          {{ __('novaNotesField.deleteConfirmation') }}
        </p>
      </div>

      <div class="bg-gray-100 dark:bg-gray-700 px-6 py-3 flex">
        <div class="ml-auto">
          <button type="button" @click.prevent="handleClose" class="mr-3 appearance-none bg-transparent font-bold text-gray-400 hover:text-gray-300 active:text-gray-500 dark:text-gray-500 dark:hover:text-gray-400 dark:active:text-gray-600 dark:hover:bg-gray-800 cursor-pointer rounded text-sm focus:outline-none focus:ring inline-flex items-center justify-center h-9 px-3">
            {{ __('novaNotesField.cancel') }}
          </button>

          <button id="confirm-delete-button" ref="confirmButton" type="submit" class="shadow relative bg-red-400 hover:bg-red-100 active:bg-red-600 text-red-50 hover:text-red-600 active:text-white dark:text-gray-900 cursor-pointer rounded text-sm font-bold focus:outline-none focus:ring inline-flex items-center justify-center h-9 px-3">
            {{ __('novaNotesField.delete') }}
          </button>
        </div>
      </div>
    </form>
  </modal>
</template>

<script>
export default {
  methods: {
    handleClose() {
      this.$emit('close');
    },
    handleConfirm() {
      this.$emit('confirm');
    },
    handleShowingModal() {
      this.$nextTick(() => {
        this.$refs.confirmButton.focus()
      })
    },
  },
};
</script>
