<template>
  <v-layout wrap justify-center align-center>
    <v-flex xs12 sm12 md12>
      <v-card elevation="4" light tag="section">
        <v-card-title>
          <v-layout align-center justify-space-between>
            <h3 class="headline">Woo Nuxt</h3>
            <v-flex>
              <v-img class="ml-3" contain height="48px" position="center right" src="/api-logo.png"></v-img>
            </v-flex>
          </v-layout>
        </v-card-title>
        <v-divider></v-divider>

        <v-stepper v-model="e1">
          <v-stepper-header>
            <v-stepper-step :complete="e1 > 1" step="1">Personal Information</v-stepper-step>

            <v-divider></v-divider>

            <v-stepper-step :complete="e1 > 2" step="2">Billing Information</v-stepper-step>

            <v-divider></v-divider>

            <v-stepper-step step="3">Shipping Information</v-stepper-step>
          </v-stepper-header>

          <v-stepper-items>
            <v-stepper-content step="1">
              <v-form class="mb-12">
                <v-text-field
                  outline
                  label="First Name"
                  type="text"
                  v-model="person.first_name"
                  :rules="validationRules.nameRules"
                  required
                ></v-text-field>
                <v-text-field
                  outline
                  label="Last Name"
                  type="text"
                  :rules="validationRules.nameRules"
                  v-model="person.last_name"
                  required
                ></v-text-field>
                <v-text-field
                  outline
                  label="Email"
                  type="email"
                  v-model="person.email"
                  :rules="validationRules.emailRules"
                  required
                ></v-text-field>
                <v-text-field
                  outline
                  label="Username"
                  type="text"
                  v-model="person.username"
                  :rules="validationRules.usernameRules"
                  required
                ></v-text-field>
                <v-text-field
                  outline
                  label="Password"
                  type="password"
                  v-model="person.password"
                  :rules="validationRules.passwordRules"
                  required
                ></v-text-field>
                <v-text-field
                  outline
                  label="Confirm Password"
                  type="password"
                  v-model="confirm_password"
                  :rules="[passwordConfirmationRule]"
                ></v-text-field>
              </v-form>

              <v-btn color="primary" @click="e1 = 2">Continue</v-btn>
            </v-stepper-content>

            <v-stepper-content step="2">
              <v-form class="mb-12">
                <v-textarea
                  v-model="billing.address_1"
                  label="Billing Address 1"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-textarea
                  v-model="billing.address_2"
                  label="Billing Address 1"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-select :items="countryItems" v-model="billing.country" label="Countries"></v-select>
                <v-select :items="stateItems" v-model="billing.state" label="State"></v-select>
                <v-text-field outline label="City" type="text" v-model="billing.city"></v-text-field>
                <v-text-field outline label="Postal Code" type="text" v-model="billing.postcode"></v-text-field>
                <v-text-field outline label="Phone" type="text" v-model="billing.phone"></v-text-field>
              </v-form>

              <v-btn color="primary" @click="e1 = 3">Continue</v-btn>

              <v-btn text @click="e1 = 1">Back</v-btn>
            </v-stepper-content>

            <v-stepper-content step="3">
              <v-form class="mb-12">
                <v-textarea
                  v-model="shipping.address_1"
                  label="Shipping Address 1"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-textarea
                  v-model="shipping.address_2"
                  label="Shipping Address 1"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-select :items="countryItems" v-model="shipping.country" label="Countries"></v-select>
                <v-select :items="stateItems" v-model="shipping.state" label="State"></v-select>
                <v-text-field outline label="City" type="text" v-model="shipping.city"></v-text-field>
                <v-text-field outline label="Postal Code" type="text" v-model="shipping.postcode"></v-text-field>
                <v-text-field outline label="Phone" type="text" v-model="shipping.phone"></v-text-field>
              </v-form>

              <v-btn color="primary" @click="saveData()">Register</v-btn>

              <v-btn text @click="e1 = 1">Back</v-btn>
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  data() {
    return {
      e1: 1,
      countryItems: ['USA', 'Japan', 'Philippines', 'India', 'Australia'],
      stateItems: ['Texas', 'Tokyo', 'Luzon', 'Mumbai', 'Victoria'],
      validationRules: {
        nameRules: [(v) => !!v || 'Name is required'],
        usernameRules: [
          (v) => !!v || 'Username is required',
          (v) => v.length >= 8 || 'Min 8 characters',
        ],
        passwordRules: [
          (v) => !!v || 'Password is required',
          (v) => v.length >= 8 || 'Min 8 characters',
        ],
        emailRules: [
          (v) => !!v || 'E-mail is required',
          (v) => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
      },
      person: {
        firstname: '',
        lastname: '',
        email: '',
        username: '',
        password: '',
      },
      confirm_password: '',
      billing: {
        address_1: '',
        address_2: '',
        coutry: '',
        state: '',
        postcode: '',
        city: '',
        phone: '',
      },
      shipping: {
        address_1: '',
        address_2: '',
        coutry: '',
        state: '',
        postcode: '',
        city: '',
        phone: '',
      },
    }
  },
  computed: {
    passwordConfirmationRule() {
      return (
        this.person.password === this.person.confirm_password ||
        'Password must match'
      )
    },
  },
}
</script>
<style scoped>
</style>