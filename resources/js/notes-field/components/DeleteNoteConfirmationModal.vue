<template>
    <modal max-width="sm" @modal-close="handleClose" @showing="handleShowingModal">
        <form
      class="mx-auto overflow-hidden rounded-lg bg-white shadow-lg dark:bg-gray-800"
            @submit.prevent="handleConfirm"
    >
            <heading
        :level="3"
        class="text-90 border-b border-gray-100 px-8 py-4 font-bold uppercase tracking-wide dark:border-gray-700 md:text-sm"
        >{{ __('novaNotesField.deleteNoteTitle') }}</heading
      >
            <div class="px-8 py-3">
                <p class="text-80 leading-normal">
                    {{ __('novaNotesField.deleteConfirmation') }}
                </p>
            </div>

            <div class="flex bg-gray-100 px-6 py-3 dark:bg-gray-700">
                <div class="ml-auto">
                    <button
            type="button"
            class="mr-3 inline-flex h-9 cursor-pointer appearance-none items-center justify-center rounded bg-transparent px-3 text-sm font-bold text-gray-400 hover:text-gray-300 focus:outline-none focus:ring active:text-gray-500 dark:text-gray-500 dark:hover:bg-gray-800 dark:hover:text-gray-400 dark:active:text-gray-600"
                        @click.prevent="handleClose"
          >
                        {{ __('novaNotesField.cancel') }}
                    </button>

                    <button
            id="confirm-delete-button"
            ref="confirmButton"
            type="submit"
            class="relative inline-flex h-9 cursor-pointer items-center justify-center rounded bg-red-500 px-3 text-sm font-bold text-white shadow hover:bg-red-800 focus:outline-none focus:ring active:bg-red-600"
          >
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
        this.$refs.confirmButton.focus();
      });
    },
  },
};
</script>
