<template>
  <v-layout wrap justify-center align-center>
    <v-flex xs12 sm12 md12>
      <v-overlay absolute="absolute" :value="showLoader">
        <v-progress-circular indeterminate size="64"></v-progress-circular>
      </v-overlay>

      <v-snackbar
        v-model="snackbarMsg"
        :color="snackbarColor"
        right="right"
        timeout="6000"
        top="top"
      >
        {{ snackbarContent }}
        <v-btn @click="snackbarMsg = false" color="error">
          Close
          <v-icon dark right>mdi-close-circle</v-icon>
        </v-btn>
      </v-snackbar>

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
              <v-form class="mb-12" ref="formPD" v-model="validPersonalDetails">
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
                  v-on:blur="usernameUniqueRule()"
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

              <v-btn color="primary" :disabled="!validPersonalDetails" @click="e1 = 2">Continue</v-btn>
            </v-stepper-content>

            <v-stepper-content step="2">
              <v-form class="mb-12" v-model="validBilling">
                <v-textarea
                  v-model="billing.address_1"
                  label="Billing Address 1"
                  :rules="validationRules.billingAddress"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-textarea
                  v-model="billing.address_2"
                  label="Billing Address 1"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-select
                  :items="countryItems"
                  v-model="billing.country"
                  label="Country"
                  :rules="validationRules.countryRules"
                ></v-select>
                <v-select
                  :items="stateItems"
                  v-model="billing.state"
                  label="State"
                  :rules="validationRules.stateRules"
                ></v-select>
                <v-text-field
                  outline
                  label="City"
                  type="text"
                  v-model="billing.city"
                  :rules="validationRules.cityRules"
                ></v-text-field>
                <v-text-field
                  outline
                  label="Postal Code"
                  type="text"
                  v-model="billing.postcode"
                  :rules="validationRules.postcodeRules"
                ></v-text-field>
                <v-text-field
                  outline
                  label="Phone"
                  type="text"
                  v-model="billing.phone"
                  :rules="validationRules.phoneRules"
                ></v-text-field>
              </v-form>

              <v-btn color="primary" :disabled="!validBilling" @click="e1 = 3">Continue</v-btn>

              <v-btn text @click="e1 = 1">Back</v-btn>
            </v-stepper-content>

            <v-stepper-content step="3">
              <v-checkbox
                v-model="copyBilling"
                label="Same as Billing"
                color="indigo"
                @change="billingToShipping"
              ></v-checkbox>
              <v-form class="mb-12" v-model="validShipping">
                <v-textarea
                  v-model="shipping.address_1"
                  label="Shipping Address 1"
                  rows="2"
                  :rules="validationRules.shippingAddress"
                  :auto-grow="true"
                ></v-textarea>

                <v-textarea
                  v-model="shipping.address_2"
                  label="Shipping Address 1"
                  rows="2"
                  :auto-grow="true"
                ></v-textarea>

                <v-select
                  :items="countryItems"
                  v-model="shipping.country"
                  :rules="validationRules.countryRules"
                  label="Countries"
                ></v-select>
                <v-select
                  :items="stateItems"
                  v-model="shipping.state"
                  :rules="validationRules.stateRules"
                  label="State"
                ></v-select>
                <v-text-field
                  outline
                  label="City"
                  type="text"
                  :rules="validationRules.cityRules"
                  v-model="shipping.city"
                ></v-text-field>
                <v-text-field
                  outline
                  label="Postal Code"
                  type="text"
                  :rules="validationRules.postcodeRules"
                  v-model="shipping.postcode"
                ></v-text-field>
                <v-text-field
                  outline
                  label="Phone"
                  type="text"
                  :rules="validationRules.phoneRules"
                  v-model="shipping.phone"
                ></v-text-field>
              </v-form>

              <v-btn color="primary" :disabled="!validShipping" @click="saveData()">Register</v-btn>

              <v-btn text @click="e1 = 1">Back</v-btn>
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
import WooCommerceRestApi from '@woocommerce/woocommerce-rest-api'

export default {
  data() {
    return {
      WooCommerce: {},
      snackbarContent: '',
      snackbarColor: 'success',
      snackbarMsg: false,
      showLoader: false,
      e1: 1,
      countryItems: ['USA', 'Japan', 'Philippines', 'India', 'Australia'],
      stateItems: ['Texas', 'Tokyo', 'Luzon', 'Mumbai', 'Victoria'],
      validPersonalDetails: true,
      validBilling: true,
      validShipping: true,
      copyBilling: false,
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
        billingAddress: [
          (v) => !!v || 'Billing address required',
          (v) => v.length >= 12 || 'Address to short',
        ],
        shippingAddress: [
          (v) => !!v || 'Shipping address required',
          (v) => v.length >= 12 || 'Address to short',
        ],
        countryRules: [(v) => !!v || 'Country is required'],
        stateRules: [(v) => !!v || 'State is required'],
        cityRules: [(v) => !!v || 'City is required'],
        postcodeRules: [(v) => !!v || 'Post Code is required'],
        phoneRules: [(v) => !!v || 'Phone is required'],
      },
      person: {
        first_name: '',
        last_name: '',
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
        this.person.password === this.confirm_password || 'Password must match'
      )
    },
  },
  created() {
    this.WooCommerce = new WooCommerceRestApi({
      url: process.env.wooURL,
      consumerKey: process.env.consumerKey,
      consumerSecret: process.env.consumerSecret,
      version: process.env.wooVersion,
      verifySsl: process.env.verifySSL,
      queryStringAuth: process.env.queryStringAuth,
    })
  },
  methods: {
    billingToShipping() {
      if (this.copyBilling) {
        this.shipping.address_1 = this.billing.address_1
        this.shipping.address_2 = this.billing.address_2
        this.shipping.country = this.billing.country
        this.shipping.state = this.billing.state
        this.shipping.postcode = this.billing.postcode
        this.shipping.city = this.billing.city
        this.shipping.phone = this.billing.phone
      } else {
        this.shipping.address_1 = ''
        this.shipping.address_2 = ''
        this.shipping.country = ''
        this.shipping.state = ''
        this.shipping.postcode = ''
        this.shipping.city = ''
        this.shipping.phone = ''
      }
    },
    usernameUniqueRule() {
      let message = ''
      let emailAlreadyRegistered = ''
      this.WooCommerce.get('customers?email=' + this.person.email)
        .then((response) => {
          if (response.data[0].email) {
            this.snackbarColor = 'error'
            message = 'This email is not available'
            emailAlreadyRegistered = true
          }
        })
        .catch((error) => {
          console.log('Error Data:', error.data)
        })
        .finally(() => {
          if (!emailAlreadyRegistered) {
            this.snackbarColor = 'success'
            message = 'Email is available, you can use this email address'
          }

          this.snackbarMsg = true
          this.snackbarContent = message
        })
    },
    saveData() {
      if (this.validShipping) {
        const userFormFields = {
          first_name: this.person.first_name,
          last_name: this.person.last_name,
          email: this.person.email,
          username: this.person.username,
          password: this.person.password,
          shipping: this.shipping,
          billing: this.billing,
        }
        this.showLoader = true
        this.WooCommerce.post('customers', userFormFields)
          .then((response) => {
            this.snackbarColor = 'success'
            this.snackbarMsg = true
            this.snackbarContent =
              'Registration successful, You can now login with that account'
            // Successful request
          })
          .catch((error) => {})
          .finally(() => {
            this.showLoader = false
            // Always executed.
          })
      } //make sure stage 3 of registration is A-OKAY
    },
  },
}
</script>
<style scoped>
</style>