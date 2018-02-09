<template>
<b-container>
<app-header></app-header>
<app-selectrisk></app-selectrisk>
<app-riskfields :riskFieldsList = "risksObject.RiskTypeGenericField"></app-riskfields>
<app-footer></app-footer>
</b-container>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import SelectRisk from './components/SelectRisk.vue';
import RiskFields from './components/RiskFields.vue';
export default {
    components: {
        'app-header': Header,
        'app-selectrisk': SelectRisk,
        'app-riskfields': RiskFields,
        'app-footer': Footer

    },
  name: 'app',
  data () {
    return {
      risksObject: {
    "uri": "http://localhost:8000/api/risktype/1/",
    "User": 1,
    "Title": "RT1U1",
    "RiskTypeGenericField": [
        {
            "pk": 3,
            "Type": "Text",
            "FieldTitle": "GenericField1RT1U1",
            "RiskTypeTextFieldValue": {
                "Value": "The Zoo"
            }
        },
        {
            "pk": 4,
            "Type": "Number",
            "FieldTitle": "GenericField2RT1U1",
            "RiskTypeNumberFieldValue": {
                "Value": 9000
            }
        }
    ]
},
    }
  },
  created () {
    // Assuming a logged in user return their RiskTypes & associated RiskTypeFields
    // axios.get('https://swapi.co/api/people/1')
    // .then(response => console.log(response.data))
  }
}
</script>

<style>

</style>
