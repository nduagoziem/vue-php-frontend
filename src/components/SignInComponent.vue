<script setup>
import { ref } from "vue";
import InputGroup from "primevue/inputgroup";
import InputGroupAddon from "primevue/inputgroupaddon";
import InputText from "primevue/inputtext";
import Password from "primevue/password";
import router from "@/router";
import axios from "axios";
import { loginUrl } from "@/config/api";
import { useToast } from "vue-toastification";

const userName = ref();
const password = ref();

const toast = useToast();

const login = async () => {
  try {
    const response = await axios.post(
      `${loginUrl}`,
      {
        userName: userName.value,
        password: password.value,
      },
      {
        headers: {
          "Content-Type": "Application/json",
        },
      }
    );

    if (response.data.success) {
      userName.value = "";
      password.value = "";

      const ii = localStorage.setItem(
        response.data.tokenKey,
        response.data.tokenValue
      );
      router.push(response.data.redirect);
      toast.success(response.data.message);
      return ii;
    } else {
      toast.error(response.data.message);
    }
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <form @submit.prevent="login">
    <div>
      <InputGroup class="mt-4 mb-4 py-2">
        <InputGroupAddon>
          <span class="pi pi-user-edit"></span>
        </InputGroupAddon>
        <InputText
          v-model="userName"
          placeholder="Username"
          name="userName"
          required
        />
      </InputGroup>

      <InputGroup class="input mb-4 py-2">
        <InputGroupAddon>
          <span class="pi pi-lock"></span>
        </InputGroupAddon>
        <Password
          v-model="password"
          toggleMask
          :feedback="false"
          placeholder="Password"
          name="password"
          required
        />
      </InputGroup>

      <button type="submit" class="py-2 text-center">Login</button>
    </div>
  </form>
</template>

<style scoped>
button {
  background-color: black;
  width: 100%;
  border: none;
  border-radius: 3px;
  outline: none;
  color: rgb(123, 183, 235);
}
</style>
