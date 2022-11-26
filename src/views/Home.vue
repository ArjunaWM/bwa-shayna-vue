<template>
  <div class="home">
    <HeaderShayna />

    <HeroShayna />
    <WomanShayna />
    <GalleryShayna />
    <PartnerShayna />

    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "../components/HeaderShayna.vue";
import HeroShayna from '../components/HeroShayna.vue';
import WomanShayna from '../components/WomanShayna.vue';
import GalleryShayna from '../components/GalleryShayna.vue';
import PartnerShayna from '../components/PartnerShayna.vue';
import FooterShayna from "../components/FooterShayna.vue";

export default {
  name: "home",
  components: {
    HeaderShayna,
    HeroShayna,
    WomanShayna,
    GalleryShayna,
    PartnerShayna,
    FooterShayna,
  },
};
</script>
