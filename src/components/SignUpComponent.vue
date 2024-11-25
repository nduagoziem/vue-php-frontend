<script setup>
import { ref } from "vue";
import InputGroup from "primevue/inputgroup";
import InputGroupAddon from "primevue/inputgroupaddon";
import InputText from "primevue/inputtext";
import Password from "primevue/password";
import axios from "axios";
import { registerUrl } from "@/config/api";
import router from "@/router";
import { useToast } from "vue-toastification";

const fullName = ref();
const userName = ref();
const password = ref();
const confirmPassword = ref();

const toast = useToast();

const registration = async () => {
  try {
    const response = await axios.post(
      `${registerUrl}`,
      {
        fullName: fullName.value,
        userName: userName.value,
        password: password.value,
        confirmPassword: confirmPassword.value,
      },
      {
        headers: {
          "Content-Type": "Application/json",
        },
      }
    );

    if (response.data.success) {
      fullName.value = "";
      userName.value = "";
      password.value = "";
      confirmPassword.value = "";

      localStorage.setItem(response.data.tokenKey, response.data.tokenValue);
      router.push(response.data.redirect);
      toast.success(response.data.message);
    } else {
      toast.error(response.data.message);
    }
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <form @submit.prevent="registration">
    <div>
      <InputGroup class="mt-4 mb-4 py-2">
        <InputGroupAddon>
          <span class="pi pi-user"></span>
        </InputGroupAddon>
        <InputText
          placeholder="Full Name"
          name="fullName"
          required
          v-model="fullName"
        />
      </InputGroup>

      <InputGroup class="mt-4 mb-4 py-2">
        <InputGroupAddon>
          <span class="pi pi-user-edit"></span>
        </InputGroupAddon>
        <InputText
          placeholder="Username"
          name="userName"
          require
          v-model="userName"
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

      <InputGroup class="input mb-4 py-2">
        <InputGroupAddon>
          <span class="pi pi-lock"></span>
        </InputGroupAddon>
        <Password
          v-model="confirmPassword"
          toggleMask
          :feedback="false"
          placeholder="Confirm Password"
          name="confirmPassword"
          required
        />
      </InputGroup>
    </div>

    <button type="submit" class="py-2 text-center">Register</button>
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
