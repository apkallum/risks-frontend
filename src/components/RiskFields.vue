<template>
<b-container fluid>
<b-row>
 <b-dropdown id="ddown1" text="Select Risk Type" class="m-sm-2">
<b-dropdown-item v-for="(riskType, index) in allRiskTypes" :key="index" :value="riskType.Title" @click="currentRiskType = riskType, defaultRiskTitleVisibility = true, defaultAddFieldTypeButtonVisibility = true, defaultRiskTypeFieldsVisibility = true">
{{riskType.Title}}
</b-dropdown-item>
 </b-dropdown>
 <b-dropdown id="ddown1" v-show="defaultAddFieldTypeButtonVisibility" text="Add Field to Current Risk Type" class="m-sm-2">
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'text')">Add Text Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'number')">Add Number Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'date')">Add Date Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'enum')">Add Enum Field</b-dropdown-item>
</b-dropdown>
<br>
<b-button variant="primary" size="lg" class="m-sm-2" @click="addNewRiskType(allRiskTypes), defaultRiskTitleVisibility = true, currentRiskType = allRiskTypes.slice(-1)[0]">Add New Risk Type</b-button>
</b-row>
<br>
<div v-show="defaultRiskTitleVisibility">
<b-row><b-col class="m-sm-2"><strong>Risk Title:</strong></b-col></b-row>
<b-row>
<b-col sm="4"><b-input v-model="currentRiskType.Title" placeholder="Risk Title"></b-input></b-col>
</b-row>
</div>
<br>
<div v-show="defaultRiskTypeFieldsVisibility">
<strong>Risk Type Fields:</strong>
<b-row v-for="(field, index) in currentRiskType.RiskTypeGenericType" :key="index">
    <b-col sm="4"><b-input v-model="field.FieldTitle" placeholder="Field Title"></b-input></b-col>
    <b-col sm="4"><b-input :type="field.TypeAsText" v-model="field.FieldValue" placeholder="Field Value"></b-input></b-col>
    <b-col sm="1"><b-button size="sm" @click="deleteFieldFromRisk(currentRiskType, index)">X</b-button></b-col>
</b-row>
</div>
<br><br>
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
                    defaultRiskTitleVisibility: false,
                    defaultAddFieldTypeButtonVisibility: false,
                    defaultRiskTypeFieldsVisibility: false
            }
        },
        methods: {
            addNewFieldToRisk: (riskObject, Type) => {
                riskObject.RiskTypeGenericType.push (
                    {
                        'TypeAsText': Type,
                        'FieldTitle': '',
                        'FieldValue': ''
                    }
                )
            },
            addNewRiskType: (allRiskTypes) => {
                allRiskTypes.push (
                    {
                    'Title': '',
                    'RiskTypeGenericType': []
                    }
                )
            },
            deleteFieldFromRisk: (riskObject, index) => {
                riskObject.RiskTypeGenericType.splice(index, 1);
            }  
        },
        created () {
    // Assuming a logged in user return their RiskTypes & associated RiskTypeFields
            axios.get('http://backend.bcoreproject.info/api/risktype/?format=json')
            .then(response => this.allRiskTypes = response.data)

  },
        
    }
           
       


</script>

<style scoped>

</style>