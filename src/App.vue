<template>
  <div class="main">
      <h1 class="title">Parking Places Live</h1>
        <div class="parking-container">
            <div class="sectionA" id="sectionA">
              <div 
                  class="parking-place"
                  v-for="space in spacesA"
                  :key="space.space_number"
                  :class="{'occupied' : space.is_occupied, 'free' : !space.is_occupied}"
                  @click="isOccupied(space.doc_id, space.is_occupied)"
                  :title="space.space_sector+space.space_number"
                  >
                  {{space.space_sector+space.space_number}}
            </div>
            </div>
            <div class="parking-wrapper">
                <div class="sectionB" id="sectionB">
                  <div 
                  class="parking-place"
                  v-for="space in spacesB"
                  :key="space.space_number"
                  :class="{'occupied' : space.is_occupied, 'free' : !space.is_occupied}"
                  @click="isOccupied(space.doc_id, space.is_occupied)"
                  :title="space.space_sector+space.space_number"
                  >
                  {{space.space_sector+space.space_number}}
                  </div>
                </div>
                <div class="sectionC" id="sectionC">
                  <div 
                  class="parking-place"
                  v-for="space in spacesC"
                  :key="space.space_number"
                  :class="{'occupied' : space.is_occupied, 'free' : !space.is_occupied}"
                  @click="isOccupied(space.doc_id, space.is_occupied)"
                  :title="space.space_sector+space.space_number"
                  >
                  {{space.space_sector+space.space_number}}
                  </div>
                </div>
                <div>
                    <div class="sectionD" id="sectionD">
                      <div 
                      class="parking-place"
                      v-for="space in spacesD"
                      :key="space.space_number"
                      :class="{'occupied' : space.is_occupied, 'free' : !space.is_occupied}"
                      @click="isOccupied(space.doc_id, space.is_occupied)"
                      :title="space.space_sector+space.space_number"
                      >
                      {{space.space_sector+space.space_number}}
                      </div>
                    </div>
                    <div class="building"
                    :style="{ '--buildingHeight':buildingHeight() }"
                    @click="buildingHeight"
                    >
                        <h1>Steskal</h1>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
  @import 'assets/main.css';
</style>

<script setup>
  import {ref, onMounted} from 'vue';
  import {db} from '@/firebase';
  import {collection, onSnapshot, doc, updateDoc, query, orderBy} from 'firebase/firestore';

  const spacesA = ref([])
  const spacesB = ref([])
  const spacesC = ref([])
  const spacesD = ref([])
  const occupied = ref([])
  const collRef = collection(db, 'parking_spaces')
  const q = query (collRef, orderBy("space_number"))
  onMounted( ()=>{
    onSnapshot(q, (querySnapshot) =>{
      
      const fbSpacesA = []
      const fbSpacesB = []
      const fbSpacesC = []
      const fbSpacesD = []
      const fBoccupied = []

      querySnapshot.forEach((doc)=>{
        if(doc.data().space_sector=="A"){
          const space = {
          doc_id: doc.id,
          space_id: doc.data().space_id,
          space_number: doc.data().space_number,
          space_sector: doc.data().space_sector,
          is_occupied: doc.data().is_occupied
          }
          if(doc.data().is_occupied===true){
            fBoccupied.push(doc.data().space_id)
          }
          fbSpacesA.push(space)
          
        }
        if(doc.data().space_sector=="B"){
          const space = {
          doc_id: doc.id,
          space_id: doc.data().space_id,
          space_number: doc.data().space_number,
          space_sector: doc.data().space_sector,
          is_occupied: doc.data().is_occupied
          }
          if(doc.data().is_occupied===true){
            fBoccupied.push(doc.data().space_id)
          }
          fbSpacesB.push(space)
        }
        if(doc.data().space_sector=="C"){
          const space = {
          doc_id: doc.id,
          space_id: doc.data().space_id,
          space_number: doc.data().space_number,
          space_sector: doc.data().space_sector,
          is_occupied: doc.data().is_occupied
          }
          if(doc.data().is_occupied===true){
            fBoccupied.push(doc.data().space_id)
          }
          fbSpacesC.push(space)
        }
        if(doc.data().space_sector=="D"){
          const space = {
          doc_id: doc.id,
          space_id: doc.data().space_id,
          space_number: doc.data().space_number,
          space_sector: doc.data().space_sector,
          is_occupied: doc.data().is_occupied
          }
          if(doc.data().is_occupied===true){
            fBoccupied.push(doc.data().space_id)
          }
          fbSpacesD.push(space)
        }
      })
      spacesA.value = fbSpacesA
      spacesB.value = fbSpacesB
      spacesC.value = fbSpacesC
      spacesD.value = fbSpacesD
      occupied.value = fBoccupied
    })
  })
  const isOccupied = (id, isOccupied) =>{
    updateDoc(doc(db, 'parking_spaces', id), {
      is_occupied: !isOccupied
    })
    console.log("Zaktualizowano "+id)
  }
  const buildingHeight = ()=>{
    let height = Math.floor((occupied.value.length/114)*500)+"px"
    return height
  }


</script>