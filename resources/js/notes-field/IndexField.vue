<template>
    <div class="flex justify-between space-x-3">
        <div v-tooltip="field.value?.text">{{ truncateText(field.value?.text) }}</div>
        <Icon class="hover:text-primary-600 cursor-pointer text-gray-500" type="plus" @click.stop="createNotePopup = true">
        </Icon>
        <CustomModal :show="createNotePopup" max-width="screen-md" size="5xl" @close-via-escape="createNotePopup = false">
        <CreateNote
                :params="params"
                :anonymous="anonymous"
                :field="field"
                class="absolute right-0 top-[7.5rem]"
                @fetch-notes="fetchNotes"
                @cancel="createNotePopup = false"
                @click.stop
            >
            </CreateNote>
        </CustomModal>
    </div>
</template>

<script>
import CreateNote from './components/CreateNote.vue';

export default {
  components: { CreateNote },
  props: ['resourceName', 'resourceId', 'field'],

  data() {
    return {
      notes: [],
      loading: false,
      dataFormat: null,
      trixEnabled: false,
      fullWidth: true,
      anonymous: true,
      createNotePopup: false,
    };
  },

  computed: {
    params() {
      return {
        resourceId: this.resourceId,
        resourceName: this.resourceName,
        fetchTrashed: false,
      };
    },

    // latestNote() {
    //   if (!this.notes.length) {
    //     return;
    //   }
    //   return [...this.notes].shift();
    // },
  },

  mounted() {
    this.anonymous = this.field?.anonymize;
    this.fetchNotes();

    Nova.$on('note-deleted', () => {
      this.fetchNotes();
    });
  },

  unmounted() {
    Nova.$off('note-deleted', () => {
      this.fetchNotes();
    });
  },

  methods: {
    truncateText(text) {
      if (text && text.length > 50) {
        return `${text.slice(0, 50)}...`;
      }
      return text;
    },

    // async fetchNotes() {
    //   this.loading = true;

    //   const { data } = await Nova.request().get(`/nova-vendor/nova-notes/notes`, {
    //     params: this.params,
    //   });
    //   const { notes, date_format: dateFormat, trix_enabled: trixEnabled, full_width: fullWidth } = data;

    //   if (Array.isArray(notes)) this.notes = notes;
    //   this.dateFormat = dateFormat;
    //   this.trixEnabled = trixEnabled;
    //   this.fullWidth = fullWidth;

    //   this.loading = false;
    // },
  },
};
</script>
