<template>
  <div>    
    <v-app-bar id="navbar" color="transparent" absolute class="isolate">
      <img src="~/assets/sources/logos/logo_navbar.svg" alt="Logo Infinity Blocks">
    </v-app-bar>
  </div>
</template>

<script>
export default {
  name: "NavbarComponent",
  data() {
    return {
      dataNavbar: [
        {
          name: "portfolio",
          to: "/"
        },
        {
          name: "swap",
          to: "/"
        },
        {
          name: "farm",
          to: "/"
        },
        {
          name: "pools",
          to: "/"
        },
        {
          name: "xhpot",
          to: "/"
        },
      ],
      dataMenuLogin: [
        { key:"logout", name:"Log out" },
      ],
    };
  },
  // created() {
  //   const theme = localStorage.getItem("theme");
  //   if (theme) {
  //     setTimeout(() => {
  //       this.$store.commit("switchTheme", theme);
  //     }, 100);
  //   }
  //   if (theme === "light") {this.themeButton = true}
  //   else {this.themeButton = false}
  // },
  methods: {
    // changeTheme(theme) {
    //   this.$store.commit("switchTheme", theme);
    //   this.themeButton = !this.themeButton;
    // },
  },
};
</script>

<style src="~/assets/styles/components/navbar.scss" lang="scss" />
