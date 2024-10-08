<template>
  <v-app>
    <v-main class="d-flex align-center justify-center">
      <v-container>
        <h1 class="text-center mb-6">Mai Relaks</h1>
        <v-responsive
          class="mx-auto pa-6 rounded-lg profile-container bg-grey-lighten-5"
          max-width="400"
        >
          <v-form
            fast-fail
            ref="form"
            @submit.prevent="submitForm"
            class="pa-4"
          >
            <div class="d-flex align-center justify-center flex-column">
              <v-avatar
                color="secondary"
                size="80"
                class="mb-4"
                @click="selectFile"
                style="cursor: pointer"
              >
                <img
                  v-if="profileImage"
                  :src="profileImage"
                  alt="Profile"
                  class="profile-image"
                />
                <v-icon v-else icon="mdi-account-circle" size="40"></v-icon>
              </v-avatar>
              <v-file-input
                ref="fileInput"
                v-show="false"
                @change="onFileChange"
                accept="image/*"
                :rules="fileRules"
              ></v-file-input>
              <v-btn
                @click="removeProfileImage"
                v-if="profileImage"
                icon
                class="ml-2 mt-2"
              >
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </div>

            <!-- First Name and Last Name Section -->
            <h3 class="text-center mb-5 username">
              {{ firstName }} {{ lastName }}
              <v-icon class="edit-icon" @click="openDialog('name')"
                >mdi-pencil</v-icon
              >
            </h3>

            <!-- Username Row -->
            <v-row class="profile-row mb-4">
              <v-col cols="4"><h3>Username</h3></v-col>
              <v-col cols="8"><p>abc123</p></v-col>
            </v-row>

            <!-- Email Row -->
            <v-row class="profile-row mb-4">
              <v-col cols="4"><h3>Email</h3></v-col>
              <v-col cols="8"><p>abc@gmail.com</p></v-col>
            </v-row>

            <!-- Contact No. Row -->
            <v-row class="profile-row mb-4">
              <v-col cols="4"><h3>Contact No.</h3></v-col>
              <v-col cols="4"
                ><p>{{ contactNo }}</p></v-col
              >
              <v-col cols="4" class="d-flex justify-end">
                <v-icon class="edit-icon" @click="openDialog('contactNo')"
                  >mdi-pencil</v-icon
                >
              </v-col>
            </v-row>

            <!-- DOB Row -->
            <v-row class="profile-row mb-4">
              <v-col cols="4"><h3>DOB</h3></v-col>
              <v-col cols="4"
                ><p>{{ dob }}</p></v-col
              >
              <v-col cols="4" class="d-flex justify-end">
                <v-icon class="edit-icon" @click="openDialog('dob')"
                  >mdi-pencil</v-icon
                >
              </v-col>
            </v-row>

            <!-- Country Row -->
            <v-row class="profile-row mb-4">
              <v-col cols="4"><h3>Country</h3></v-col>
              <v-col cols="4"
                ><p>{{ country }}</p></v-col
              >
              <v-col cols="4" class="d-flex justify-end">
                <v-icon class="edit-icon" @click="openDialog('country')"
                  >mdi-pencil</v-icon
                >
              </v-col>
            </v-row>

            <div class="d-flex justify-center">
              <v-btn
                class="mt-5 rounded-pill btn-logout"
                append-icon="mdi-close-circle-outline"
                @click="logout"
              >
                Log Out
              </v-btn>
            </div>
          </v-form>
        </v-responsive>

        <!-- Dialog for editing fields -->
        <v-dialog v-model="dialog" max-width="400px">
          <v-card>
            <v-card-title>Edit {{ currentFieldLabel }}</v-card-title>
            <v-card-text>
              <!-- First and Last Name Dialog -->
              <v-if v-if="currentField === 'name'">
                <v-text-field
                  v-model="currentFirstName"
                  label="First Name"
                ></v-text-field>
                <v-text-field
                  v-model="currentLastName"
                  label="Last Name"
                ></v-text-field>
              </v-if>

              <!-- Other Fields Dialog -->
              <v-if v-if="currentField !== 'name'">
                <v-text-field
                  v-model="currentValue"
                  label="Enter new value"
                ></v-text-field>
              </v-if>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDialog"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="saveChanges"
                >Save</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      firstName: "Kumara",
      lastName: "Ruben",
      contactNo: "+60123456789",
      dob: "DD/MM/YYYY",
      country: "MY",
      profileImage: null,
      dialog: false,
      currentField: "",
      currentValue: "",
      currentFirstName: "",
      currentLastName: "",
      currentFieldLabel: "",
      fileRules: [
        (v) => !v || v.size <= 2000000 || "Image should be less than 2MB",
      ],
    };
  },
  methods: {
    selectFile() {
      this.$refs.fileInput.click();
    },
    onFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.profileImage = e.target.result;
        };
        reader.readAsDataURL(file);
      }
    },
    removeProfileImage() {
      this.profileImage = null;
    },
    openDialog(field) {
      this.currentField = field;
      if (field === "name") {
        this.currentFirstName = this.firstName;
        this.currentLastName = this.lastName;
        this.currentFieldLabel = "Name";
      } else {
        this.currentValue = this[field];
        this.currentFieldLabel = this.getFieldLabel(field);
      }
      this.dialog = true;
    },
    closeDialog() {
      this.dialog = false;
    },
    saveChanges() {
      if (this.currentField === "name") {
        this.firstName = this.currentFirstName;
        this.lastName = this.currentLastName;
      } else {
        this[this.currentField] = this.currentValue;
      }
      this.closeDialog();
    },
    getFieldLabel(field) {
      const labels = {
        name: "Name",
        contactNo: "Contact No.",
        dob: "DOB",
        country: "Country",
      };
      return labels[field] || "Field";
    },
    submitForm() {
      if (this.$refs.form.validate()) {
        alert("Form has been submitted.");
      }
    },
    logout() {
      window.location.href = "/registration";
    },
  },
};
</script>

<style scoped>
.profile-container {
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 15px;
}

.username {
  font-weight: 600;
  color: #013d5a;
}

.edit-icon {
  color: #757575;
  cursor: pointer;
  transition: color 0.3s ease;
}

.edit-icon:hover {
  color: #013d5a;
}

.profile-row h3 {
  font-size: 1rem;
  color: #666;
}

.profile-row p {
  font-size: 1rem;
  color: #333;
}

.btn-logout {
  background-color: #013d5a;
  color: #fff;
  width: 40%;
  min-width: auto;
  padding: 12px 24px;
  margin: auto;
  border-radius: 50px;
  text-align: center;
}

.btn-logout .v-btn__content {
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn-logout:hover {
  background-color: #012a40;
}

.profile-image {
  object-fit: cover; /* Ensures the image covers the avatar space */
  border-radius: 50%; /* Makes the image circular */
}
</style>
