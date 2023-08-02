<template>
  <v-footer id="footer" color="transparent" absolute class="center">
    <img class="mb-3" src="~/assets/sources/logos/logo_footer.svg" alt="Logo Infinity Blocks">
  </v-footer>
</template>

<script>
export default {
  name: "FooterComponent",
  data() {
    return {
      dataRedes: [
        { icon: "discord", to: "/" },
        { icon: "twitter", to: "/" },
        { icon: "youtube", to: "/" },
        { icon: "instagram", to: "/" },
      ],
      dataFooter: [
        {
          title: "featured",
          links: [
            { link: "Trending", to: "/" },
            { link: "Top", to: "/" },
            { link: "Collectibles", to: "/" },
          ]
        },
        {
          title: "explore",
          links: [
            { link: "Trending", to: "/" },
            { link: "Top", to: "/" },
            { link: "Collectibles", to: "/" },
          ]
        },
        {
          title: "stats",
          links: [
            { link: "Ranking", to: "/" },
            { link: "Activity", to: "/" },
          ]
        },
        {
          title: "resources",
          links: [
            { link: "Blog", to: "/" },
            { link: "Partners", to: "/" },
            { link: "Newsletter", to: "/" },
          ]
        },
        {
          title: "keep in contact",
          input: true
        },
      ],
      input: "",
    }
  },
  methods: {
    SendEmail() {
      alert('send')
    },
  }
}
</script>

<style src="~/assets/styles/components/footer.scss" lang="scss" />
