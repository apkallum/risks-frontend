<template>
<b-container>
<app-header></app-header>
<app-riskfields></app-riskfields>
<app-footer></app-footer>
</b-container>
</template>

<script>
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import RiskFields from './components/RiskFields.vue';
export default {
    components: {
        'app-header': Header,
        'app-riskfields': RiskFields,
        'app-footer': Footer

    },
  name: 'app', 
  data () {
    return {

    }
  },
  created () {

  }
}
</script>

<style>

</style>
