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
    <b-col sm="5"><b-input v-model="field.FieldTitle"></b-input></b-col>
    <b-col sm="7"><b-input :id="`type-${field.Type.toLowerCase()}`" :type="field.Type.toLowerCase()" :value="getValueOfField(field)"></b-input></b-col>
</b-row>
<br><br>
<b-row>
<b-btn-group>
<b-button>Add Text Field</b-button>
<b-button>Add Number Field</b-button>
<b-button>Add Date Field</b-button>
<b-button>Add Enum Field</b-button>
</b-btn-group>
</b-row>
<br><br>
</b-container>
</template>

<script>
import axios from 'axios';
    export default {
        data () {
            return {
                    allRiskTypes: [],
                    currentRiskType: {},
                    RiskTypeFieldValue: 'RiskTypeFieldValue'

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
            getNameOfFieldValue: function(Type) {
                if (Type === 'Text') {
                    return 'RiskTypeTextFieldValue';
                }
                if (Type === 'Number') {
                    return 'RiskTypeNumberFieldValue';
                }
                if (Type === 'Date') {
                    return 'RiskTypeDateFieldValue';
                }
                if (Type === 'Enum') {
                    return 'RiskTypeEnumFieldValue';
                }
            },
            unifyFieldValueNames: function(allRiskTypes){
                for (var i = 0, len1 = allRiskTypes.length; i < len1; i++) {
                    for (var j = 0, len2 = allRiskTypes[i].length; j < len2; j++) {
                        allRiskTypes[i].RiskTypeGenericField[j][RiskTypeFieldValue] = allRiskTypes[i].RiskTypeGenericField[j][getNameOfFieldValue(allRiskTypes[i].RiskTypeGenericField[j].Type)]
                        delete allRiskTypes[i].RiskTypeGenericField[j][getNameOfFieldValue(allRiskTypes[i].RiskTypeGenericField[j].Type)]    
                    }
                }                
            }    
        },
        created () {
    // Assuming a logged in user return their RiskTypes & associated RiskTypeFields
            axios.get('http://backend.bcoreproject.info/api/risktype/?format=json')
            .then(response => this.allRiskTypes = response.data)

  },
        mounted () {
            this.unifyFieldValueNames(this.allRiskTypes)
            }

            
    }
           
       


</script>

<style scoped>

</style>