<template>
<b-container fluid>
<b-row>
 <b-dropdown id="ddown1" text="Select Risk Type" class="m-sm-2">
<b-dropdown-item v-for="(riskType, index) in allRiskTypes" :key="index" :value="riskType.Title" @click="currentRiskType = riskType, defaultRiskTitleVisibility = true">
{{riskType.Title}}
</b-dropdown-item>
 </b-dropdown>
 <b-dropdown id="ddown1" text="Add Field to Current Risk Type" class="m-sm-2">
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'Text')">Add Text Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'Number')">Add Number Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'Date')">Add Date Field</b-dropdown-item>
<b-dropdown-item @click="addNewFieldToRisk(currentRiskType, 'Enum')">Add Enum Field</b-dropdown-item>
</b-dropdown>
<br>
<b-button variant="primary" size="lg" class="m-sm-2" @click="addNewRiskType(allRiskTypes), defaultRiskTitleVisibility = true, currentRiskType = allRiskTypes.slice(-1)[0]">Add New Risk Type</b-button>
</b-row>
<br>
<b-row>
<b-col m="4"><b-input v-show="defaultRiskTitleVisibility"  v-model="currentRiskType.Title" placeholder="Risk Title"></b-input></b-col>
</b-row>

<b-row class="my-1" v-for="(field, index) in currentRiskType.RiskTypeGenericType" :key="index">
    <b-col sm="5"><b-input v-model="field.FieldTitle"></b-input></b-col>
    <b-col sm="7"><b-input :id="`type-${field.Type.toLowerCase()}`" :type="field.Type.toLowerCase()" v-model="field.FieldValue"></b-input></b-col>
</b-row>
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
                    defaultRiskTitleVisibility: false

            }
        },
        methods: {
            addNewFieldToRisk: (riskObject, Type) => {
                riskObject.RiskTypeGenericType.push (
                    {
                        'Type': Type,
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