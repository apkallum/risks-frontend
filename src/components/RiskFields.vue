<template>
<b-container fluid>
<b-row>
 <b-dropdown id="ddown1" text="Select Risk Type" class="m-md-2">
<b-dropdown-item v-for="(riskType, index) in allRiskTypes" :key="index" :value="riskType.Title" @click="currentRiskType = riskType">
{{riskType.Title}}
</b-dropdown-item>
 </b-dropdown>
<div class="m-md-4" v-if="currentRiskType">{{currentRiskType.Title}}</div>
</b-row>

<b-row class="my-1" v-for="(field, index) in currentRiskType.RiskTypeGenericField" :key="index">
    <b-col sm="5"><b-input :value="field.FieldTitle"></b-input></b-col>
    <b-col sm="7"><b-input :id="`type-${field.Type.toLowerCase()}`" :type="field.Type.toLowerCase()" :value="getValueOfField(field)"></b-input></b-col>
</b-row>
</b-container>
</template>

<script>
import axios from 'axios';
    export default {
        data () {
            return {
                    allRiskTypes: [],
                    currentRiskType: {}
            }
        },
        methods: {
            getValueOfField: (Field) => {
                if (Field.Type === 'Text') {
                    return Field.RiskTypeTextFieldValue.Value;
                }
                if (Field.Type === 'Number') {
                    return Field.RiskTypeNumberFieldValue.Value;
                }
                if (Field.Type === 'Date') {
                    return Field.RiskTypeDateFieldValue.Value;
                }
                if (Field.Type === 'Enum') {
                    return Field.RiskTypeEnumFieldValue.Value;
                }
            },
            addNewFieldToRisk: (riskObject) => {
                this.riskObject.RiskTypeGenericField.push (
                    {
                        
                    }
                )
            },
               
        },
        // Add method to extract all riskObjects from API response 
        created () {
    // Assuming a logged in user return their RiskTypes & associated RiskTypeFields
    axios.get('http://backend.bcoreproject.info/api/risktype/?format=json')
   .then(response => this.allRiskTypes = response.data)

//  .then(response => console.log(response.data))

  }
                
    }
           
       


</script>

<style scoped>

</style>