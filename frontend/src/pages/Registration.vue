<template>
  <v-app>
    <v-main class="d-flex align-center justify-center">
      <v-container>
        <v-responsive
          class="mx-auto border pa-6 rounded-lg bg-grey-lighten-5"
          max-width="1000"
        >
          <h1 class="text-center mb-6">Registration</h1>

          <v-form v-model="valid" fast-fail @submit.prevent="submit">
            <v-row>
              <v-col cols="12" md="6" class="border-e-md">
                <GenderSelect v-model:gender="gender" />

                <PersonalInfoForm
                  v-model:firstName="firstName"
                  v-model:lastName="lastName"
                  v-model:username="username"
                  v-model:email="email"
                />
              </v-col>

              <v-col cols="12" md="6" class="mt-7">
                <ContactInfoForm
                  v-model:dob="dob"
                  v-model:code="code"
                  v-model:phone="phone"
                  v-model:city="city"
                  v-model:country="country"
                  v-model:password="password"
                  v-model:confirmPassword="confirmPassword"
                />
              </v-col>
            </v-row>

            <div class="text-center">
              <v-btn
                class="mt-6 custom-width"
                type="submit"
                :disabled="!valid"
                color="#013D5A"
                rounded="pill"
                prepend-icon="mdi-pencil"
              >
                Submit
              </v-btn>
            </div>

            <p class="text-center mt-4">
              Already have an account?
              <RouterLink to="/login" class="text-decoration-underline">
                Login here
              </RouterLink>
            </p>
          </v-form>
        </v-responsive>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import GenderSelect from "@/components/GenderSelect.vue";
import PersonalInfoForm from "@/components/PersonalInfoForm.vue";
import ContactInfoForm from "@/components/ContactInfoForm.vue";

export default {
  components: {
    GenderSelect,
    PersonalInfoForm,
    ContactInfoForm,
  },
  data() {
    return {
      valid: false,
      gender: null,
      firstName: "",
      lastName: "",
      username: "",
      email: "",
      dob: null,
      code: "",
      phone: "",
      city: "",
      country: "",
      password: "",
      confirmPassword: "",
    };
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        alert("Form submitted successfully!");
      }
    },
  },
};
</script>

<style scoped>
.border {
  border: 1px solid #ccc;
}

.pa-6 {
  padding: 24px;
}

.custom-width {
  width: 120px;
}

.text-center {
  text-align: center;
}

.text-decoration-underline {
  text-decoration: underline;
}
</style>
