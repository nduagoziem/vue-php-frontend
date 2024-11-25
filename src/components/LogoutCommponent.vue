<script setup>
import axios from "axios";
import { useToast } from "vue-toastification";
import { logoutUrl } from "@/config/api";
import router from "@/router";

const toast = useToast();

const logout = async () => {
  try {
    const response = await axios.post(
      `${logoutUrl}`,
      {
        body: null,
      },
      {
        headers: { "Content-Type": "Application/json" },
      }
    );

    if (response.data.success) {
      localStorage.removeItem("t%20o%20k%20e%20n");

      toast.success(response.data.message);
      router.push("/");
    }
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <div class="container-fluid mt-4 mb-3">
    <div class="container py-4">
      <div class="d-flex justify-content-center">
        <button
          class="px-3 py-2 bg-dark logoutBtn"
          data-bs-toggle="modal"
          data-bs-target="#logoutModal"
        >
          <span class="pi pi-home px-1"></span>Logout
        </button>

        <!-- Confirmation Modal For Logging Out -->
        <div
          class="modal fade"
          id="logoutModal"
          tabindex="-1"
          aria-labelledby="logout"
          aria-hidden="true"
        >
          <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
              <div class="modal-header">
                <h1 class="modal-title fs-5" id="logoutModal">Logout</h1>
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                ></button>
              </div>
              <div class="modal-body">Are you sure you want to logout?</div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="cancel-modal px-3 py-1"
                  data-bs-dismiss="modal"
                >
                  Cancel
                </button>
                <button
                  @click="logout"
                  type="button"
                  data-bs-dismiss="modal"
                  class="delete-modal px-3 py-1"
                >
                  Logout
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: aliceblue;
}

.logoutBtn {
  border: none;
  outline: none;
  border-radius: 10px;
}

.cancel-modal {
  background-color: black;
  border-radius: 4px;
  color: white;
  border: none;
}

.delete-modal {
  background-color: rgb(150, 7, 7);
  border-radius: 4px;
  border: none;
  color: white;
}
</style>
