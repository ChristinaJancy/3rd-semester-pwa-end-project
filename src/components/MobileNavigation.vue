<template>
  <div>
    <v-app-bar app clipped-left dark class="d-lg-none d-md-none d-sm-none nolabelgreen">
      <v-spacer></v-spacer>
      <v-toolbar-title>
        <router-link to="/">
          <v-img width="140" src="../assets/nolabel-logo-white.png"></v-img>
        </router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-bottom-navigation
      class="d-lg-none d-md-none d-sm-none"
      app
      bottom
      fixed
      shift
    >
      <v-btn v-for="(navItem, i) in navItems" :key="i" :to="navItem.link">
        <span>{{ navItem.name }}</span>
        <v-icon>{{ navItem.icon }}</v-icon>
      </v-btn>
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn v-bind="attrs" v-on="on" depressed>
            <v-icon class="iconcolor--text">mdi-account-circle-outline</v-icon>
          </v-btn>
        </template>
        <v-list>
          <router-link to="/randomOutfit">
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title class="iconcolor--text"
                  >Random Outfit</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
          </router-link>
          <router-link to="/account">
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title class="iconcolor--text"
                  >Profile</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
          </router-link>

          <router-link to="/login" v-if="!currentUser">
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title class="iconcolor--text"
                  >Login</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
          </router-link>

          <v-router v-if="currentUser" @click="logout()">
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title class="iconcolor--text"
                  >Logout</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
          </v-router>
        </v-list>
      </v-menu>
    </v-bottom-navigation>
  </div>
</template>

<script>
/* eslint-disable */
import { db } from "../firebase";
// import { db } from "../../firebase";
import firebase from "firebase";
import "firebase/firestore";
import store from "../store/index.js";
firebase.auth().onAuthStateChanged(function (user) {
  if (user) {
    //user is signed in
    store.dispatch("setUser", user);
    //   ;debugger
  } else {
    //NO user is signed in
    store.dispatch("setUser", null);
  }
});
export default {
  data: () => ({
    drawer: null,
    navItems: [
      { name: "Favorites", link: "/favourites", icon: "mdi-google-fit" },
      { name: "Basket", link: "/basket", icon: "mdi-basket" },
      { name: "Products", link: "/products", icon: "mdi-tshirt-crew-outline" },
      { name: "About us", link: "/about", icon: "mdi-chat-alert-outline" },
     
    ],
  }),
  methods: {
    logout() {
      this.$store.dispatch("logout");
    },
  },
  computed: {
    currentUser() {
      return this.$store.getters.currentUser;
    },
  },
  beforeCreate() {},
};
</script>