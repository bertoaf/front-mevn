<template>
  <q-form @submit.prevent="addLink" class="q-mb-xl" ref="formAdd">
    <q-input
      v-model="link"
      label="Ingrese link"
      :rules="[(val) => (val && val.trim() !== '') || 'Escribe una URL']"
      lazy-rules
    ></q-input>

    <q-btn
      label="Agregar"
      color="primary"
      type="submit"
      class="q-mt-sm"
      :loading="loading"
    ></q-btn>
  </q-form>
</template>

<script setup>
import { ref } from "vue";
import { useLinkStore } from "src/stores/link-store";
import { useNotify } from "../composables/notify.js";

const formAdd = ref(null);
const link = ref("");
const loading = ref(false);

const useLink = useLinkStore();

const { showNotify } = useNotify();

const addLink = async () => {
  try {
    loading.value = true;

    await useLink.createLink(link.value);

    showNotify("URL agregada", "green");

    link.value = "";

    formAdd.value.resetValidation();
  } catch (e) {
    console.log(e);
    showNotify("Error al agregar URL");
  } finally {
    loading.value = false;
  }
};
</script>
