<template>
  <q-page padding class="row justify-center">
    <div class="col-12 col-sm-6">
      <h3>Login</h3>
      <q-form @submit.prevent="handleSubmit">
        <q-input
          v-model="email"
          label="Ingresa email"
          type="text"
          :rules="[
            (val) =>
              (val && /^[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(val)) ||
              'Formato de email incorrecto',
          ]"
        >
        </q-input>
        <q-input
          v-model="password"
          type="password"
          label="Ingresa contraseña"
          :rules="[
            (val) =>
              (val && val.length > 5) ||
              'La contraseña debe ser de 6 carácteres o más',
          ]"
        >
        </q-input>
        <div>
          <q-btn label="Login" type="submit"></q-btn>
        </div>
      </q-form>
    </div>
  </q-page>
</template>

<script setup>
import { useUserStore } from "../stores/user-store";
import { ref } from "vue";
import { useQuasar } from "quasar";
import { useRouter } from "vue-router";

const userStore = useUserStore();

const router = useRouter();

const $q = useQuasar();

const email = ref("alberto4@test.com");
const password = ref("123123");

const handleSubmit = async () => {
  try {
    await userStore.access(email.value, password.value);
    router.push("/");
    email.value = "";
    password.value = "";
  } catch (e) {
    console.log(e);
    $q.dialog({
      title: "Alert",
      message: e.error,
    });
  }
};
</script>
