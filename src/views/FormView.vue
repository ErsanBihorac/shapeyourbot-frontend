<script setup lang="ts">
import { onMounted, ref } from "vue";
import axios from "axios";

interface Document {
  name: string;
  status: string;
}

const documents = ref<Document[]>([]);
const fileInput = ref<HTMLInputElement | null>(null);

const selectFile = () => {
  const files = fileInput.value?.files;
  if (!files) return;
  Array.from(files).forEach((file) => {
    upload(file);

    documents.value.push({
      name: file.name,
      status: "is uploading",
    });
  });
};

// replace any type with proper type
const upload = (file: any) => {
  performUpload(file)
    .then((response) => {
      documents.value.forEach((document) => {
        if (document.name === file.name) {
          document["status"] = "is uploaded";
        }
      });
    })
    .catch((error) => {
      documents.value.forEach((document) => {
        if (document.name === file.name) {
          document["status"] = "failed";
        }
      });
    });
};

// replace any type with proper type
const performUpload = async (file: any) => {
  let formData = new FormData();
  formData.append("document", file);

  try {
    const response = await axios.post(
      "http://127.0.0.1:8000/api/documents/",
      formData,
      {
        headers: {
          "Content-Type": "multipart/form-data",
        //   "X-CSRFTOKEN": csrfToken,
        },
      }
    );
  } catch (error) {
    console.error(error);
  }
};
</script>

<template>
  <h1>Uploader</h1>

  <div class="file">
    <label class="file-label">
      <input
        type="file"
        ref="fileInput"
        class="file-input"
        @change="selectFile"
        multiple
      />
    </label>
  </div>

  <div class="notification mt-6" v-if="documents.length">
    <p v-for="document in documents">
      {{ document.name }} {{ document.status }}
    </p>
  </div>
</template>

<style scoped>
.form {
  width: 500px;
}
</style>
