<template>
  <div>
    <attachment-select-box @selectionChanged="logValues" v-model="dataSource" />
  </div>
</template>

<script>
import AttachmentSelectBox from "../components/AttachmentSelectBox.vue";
export default {
  name: "Home",
  components: {
    "attachment-select-box": AttachmentSelectBox,
  },
  data() {
    return {
      dataSource: [],
    };
  },
  methods: {
    logValues(value) {
      if (value.deselectedItem) {
        console.log("You removed: ", value?.deselectedItem?.name);
      }
      if (value.selectedItem) {
        [...value.selectedItem].forEach((file) => {
          console.log("You added: ", file.name);
        });
      }
    },
  },
};
</script>

<style></style>
