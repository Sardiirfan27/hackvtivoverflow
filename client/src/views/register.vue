<template>
  <v-app id="inspire">
    <navbarout></navbarout>
    <v-content>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="4">
            <v-card class="elevation-12">
              <v-toolbar color="primary" dark flat>
                <v-toolbar-title>Register Form</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-tooltip bottom></v-tooltip>
                <v-tooltip right></v-tooltip>
              </v-toolbar>
              <v-form @submit.prevent="register">
                <v-card-text>
                  <v-text-field
                    label="Name"
                    name="name"
                    prepend-icon="person"
                    type="text"
                    v-model="name"
                  ></v-text-field>
                  <v-text-field
                    label="Email"
                    name="email"
                    prepend-icon="email"
                    type="text"
                    v-model="email"
                  ></v-text-field>
                  <v-text-field
                    id="password"
                    label="Password"
                    name="password"
                    prepend-icon="lock"
                    type="password"
                    v-model="password"
                  ></v-text-field>
                </v-card-text>
                <v-card-actions class="justify-center">
                  <v-btn color="primary" type="submit" class="mb-10">Register</v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import navbarout from "../components/navbarout";
import Swal from "sweetalert2";
import axios from "../config/axios";
export default {
  components: {
    navbarout
  },
  data() {
    return {
      name: "",
      password: "",
      email: ""
    };
  },
  methods: {
    register() {
      Swal.fire({
        title: "Registering....",
        allowOutsideClick: () => !Swal.isLoading()
      });
      Swal.showLoading();
      axios({
        method: "POST",
        url: "/user/register",
        data: {
          name: this.name,
          email: this.email,
          password: this.password
        }
      })
        .then(({ data }) => {
          Swal.close();
          Swal.fire("Success!", "Your Account is Created!", "success");
          this.$router.push("/login").catch(err => {});
          this.clear();
        })
        .catch(error => {
          let message =
            (error.response.data && error.response.data.message) ||
            "Failed to Register";
          Swal.fire("Error!", message, "error");
        });
    },
    clear() {
      this.name = "";
      this.password = "";
      this.email = "";
    }
  },
  created() {
    if (localStorage.getItem("access_token")) {
      this.$router.go(-1);
    }
  }
};
</script>

<style>
#inspire {
  background-image: url("https://i.pinimg.com/originals/77/84/ba/7784baa4303bd0103e243fa9428b81bb.jpg");
  background-repeat: no-repeat;
  background-size: 100%;
}
</style>