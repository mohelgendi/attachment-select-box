<template>
  <div class="attachment-select-box">
    <div class="custom-file-upload" @click="showDrop = !showDrop">
      <span class="place-holder" v-if="listLocal.length === 0"
        >Select file...</span
      >
      <span class="place-holder" v-if="listLocal.length > 0">
        {{ listLocal.length }}
        file selected
      </span>
      <i
        class="myicon myicon-up-arrow"
        :class="{ rotated: showDrop, arrow: true }"
      ></i>
      <input
        type="file"
        name="attachment-select-input"
        id="attachment-select-input"
        @change="fileDetector"
        multiple
      />
    </div>
    <li v-if="showDrop" @mouseleave="showDrop = false">
      <ul class="file-ul empty-ul" v-if="listLocal.length === 0">
        No files
        <i class="myicon myicon-empty"></i>
      </ul>
      <ul class="file-ul" v-for="(file, i) in listLocal" :key="i">
        <span>{{ file.name }}</span>
        <i class="myicon myicon-minus" @click="removeFile(i)"></i>
      </ul>
      <ul class="upload-ul" @click="openUpload">
        <i class="myicon myicon-plus"></i>
        Upload
      </ul>
    </li>
  </div>
</template>

<script>
export default {
  name: "AttachmentSelectBox",
  props: {
    dataSource: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  model: {
    prop: "dataSource",
    event: "listchange",
  },
  computed: {
    listLocal: {
      get: function () {
        return this.dataSource;
      },
      set: function (value) {
        this.$emit("listchange", value);
      },
    },
  },
  data() {
    return { showDrop: false };
  },
  methods: {
    openUpload() {
      document.getElementById("attachment-select-input").click();
    },
    fileDetector(e) {
      const files = e.target.files;
      this.$emit("selectionChanged", {
        selectedItem: files,
        deselectedItem: null,
      });
      this.listLocal = [...this.listLocal, ...files];
    },
    removeFile(index) {
      const removedFile = this.listLocal[index];
      this.listLocal.splice(index, 1);
      this.$emit("selectionChanged", {
        selectedItem: null,
        deselectedItem: removedFile,
      });
    },
  },
};
</script>
