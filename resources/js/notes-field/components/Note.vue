<template>
  <div
    class="w-3/5 bg-white rounded-lg border border-gray-200 shadow-md hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700 px-2 py-2 flex mb-2 mt-2"
    :class="{ 'w-full': fullWidth, 'w-3/5': !fullWidth }"
  >
    <div v-if="!anonymous" class="rounded-full w-12 h-12 mr-3 overflow-hidden text-center" style="flex-shrink: 0">
      <div v-if="note.system" class="w-12 h-12 text-sm font-bold bg-60 text-40" style="line-height: 3rem">
        {{ __('novaNotesField.systemUserAbbreviation') }}
      </div>
      <img class="w-12 h-12" v-else-if="note.created_by_avatar_url" :src="note.created_by_avatar_url" alt="" />
      <div v-else class="w-12 h-12 text-sm font-bold bg-60 text-40" style="line-height: 3rem">
        {{ !!note.created_by_name ? (note.created_by_name || '').substr(0, 3).toUpperCase() : '??' }}
      </div>
    </div>

    <div>
      <!-- Title area -->
      <div class="mb-2 space-x-2">
        <span v-if="anonymous" class="font-bold text-lg text-90">Anonymous</span>
        <span v-else class="font-bold text-lg text-90">{{
          note.created_by_name ? note.created_by_name : __('novaNotesField.systemUserName')
        }}</span>
        <span class="px-1 py-0.5 text-xs text-white bg-primary-500 rounded dark:bg-primary-500">
          {{ formattedCreatedAtDate }}{{ note.system ? ` [${__('novaNotesField.systemUserName')}]` : '' }}
        </span>
        <svg
          v-if="!note.system && note.can_delete"
          @click="$emit('onDeleteRequested', note)"
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 inline text-red-600 hover:text-red-800 cursor-pointer"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          stroke-width="2"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
          />
        </svg>
        <!-- <span
          v-if="!note.system && note.can_delete"
          class="px-1 py-0.5 text-xs cursor-pointer text-white bg-red-600 hover:bg-red-700 rounded dark:bg-red-600 dark:hover:bg-red-700"
          @click="$emit('onDeleteRequested', note)"
          >{{ __('novaNotesField.delete') }}</span
        > -->
      </div>

      <!-- Content -->
      <p v-html="note.text" class="whitespace-pre-wrap"></p>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  props: ['note', 'dateFormat', 'fullWidth', 'anonymous'],
  computed: {
    formattedCreatedAtDate() {
      return moment(this.note.created_at).format(this.dateFormat || 'DD MMM YYYY HH:mm');
    },
  },
};
</script>
