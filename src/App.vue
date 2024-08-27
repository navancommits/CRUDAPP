<script setup>
import {ref,computed} from 'vue'

const name=ref('')
const dob=ref('')
const rowId=ref(-1)
const rowIndex=ref(-1)

const personList=ref([{'id':1,'name':'bdummy','dob':'2008-10-10'},{'id':2,'name':'adummy','dob':'2009-10-10'}])

const sortedList=computed(()=>[...personList.value].sort((a,b)=>a.name.localeCompare(b.name)))

function addData()
{
  if(validData())
  {
    personList.value.push({'id':Math.random(),'name':name.value,'dob':dob.value})
    name.value=dob.value=''
  }
}

function selectRow(id)
{
    //rowIndex.value=index
    let tmpPersonList=personList.value.filter(p=>p.id===id)
    rowId.value=tmpPersonList[0].id
    name.value=tmpPersonList[0].name
    dob.value=tmpPersonList[0].dob  
}

function editData()
{
  if(validData())
  {
    let tmpPersonList=personList.value.filter(p=>p.id===rowId.value)
    tmpPersonList[0].name=name.value
    tmpPersonList[0].dob=dob.value

    name.value=dob.value=''
  }
}

/*function deleteData()
{
  if(rowId.value>-1)
  {
    personList.value.splice(rowId.value,1)

    name.value=dob.value=''
  }
}*/

const deleteData=()=> {  
  personList.value=personList.value.filter(person=>person.id!==rowId.value)

  name.value=dob.value=''
}

const avgAge=computed(()=>{return personList.value.reduce(calculateTotalAge,0)/personList.value.length})

function calculateTotalAge(total,person)
{
  return (total + (new Date()).getYear()-(new Date(person.dob)).getYear())
}

/*
const getAge=computed(()=>{return personList.value.map(calculateAge)})

function calculateAge(person)
{
  return ((new Date()).getYear()-(new Date(person.dob)).getYear())
}
*/

const getAge=(dob)=>((new Date()).getYear()-(new Date(dob)).getYear())

function validData()
{
  return (name.value && dob.value)
}
</script>

<template>
<div class="Table">
    <div class="Title">
        <p>People List</p>
    </div>
    <div class="Heading">
        <div class="Cell">
            Name
        </div>
        <div class="Cell">
            DOB
        </div> 
         <div class="Cell">
            Age
        </div>        
    </div>
    <div class="Row" v-for="(person, index) in sortedList" :key="person.id" @click=selectRow(person.id)>
        <div class="Cell">
          {{person.name}}
        </div>
        <div class="Cell">
          {{person.dob}}
        </div> 
        <div class="Cell">
          {{getAge(person.dob)}}
        </div>       
    </div>       
</div>
Name: <input type='text' v-model='name'/>
DOB: <input type='date' v-model='dob'/>
<button @click="addData">Add</button>
<button @click="editData">Edit</button>
<button @click="deleteData">Delete</button>
<p>Average Age: {{(avgAge||0).toFixed(1)}}</p>
</template>


<style scoped>
.Table
    {
        display: table;
    }
    .Title
    {
        display: table-caption;
        text-align: center;
        font-weight: bold;
        font-size: larger;
    }
    .Heading
    {
        display: table-row;
        font-weight: bold;
        text-align: center;
    }
    .Row
    {
        display: table-row;
        cursor:pointer;
    }
    .Cell
    {
        display: table-cell;
        border: solid;
        border-width: thin;
        padding-left: 5px;
        padding-right: 5px;
      	width:500px;
    }
</style>
