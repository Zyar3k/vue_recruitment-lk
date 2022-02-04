<template>
   <div class="modal is-active">
      <div class="modal-background" @click="closeCreate"></div>
      <div class="modal-content">
         <div class="card p-6">
            <h2 class="has-text-black is-size-5 has-text-weight-bold">
               Create Nursery
            </h2>
            <div class="field">
               <label
                  class="label has-text-black is-size-6 has-text-left has-text-weight-bold"
                  >Name</label
               >
               <div class="control">
                  <input
                     class="input"
                     type="text"
                     placeholder="Please enter name"
                     name="name"
                     v-model="item.name"
                  />
               </div>
            </div>
            <div class="field">
               <label
                  class="label has-text-black is-size-6 has-text-left has-text-weight-bold"
                  >Cords</label
               >
               <div class="control is-flex">
                  <input
                     class="input mr-1"
                     type="number"
                     placeholder="Please enter latitude"
                     name="cordsLat"
                     v-model="item.cords[0].lat"
                     required
                  />
                  <input
                     class="input ml-1"
                     type="number"
                     placeholder="Please enter longitude"
                     name="cordsLng"
                     v-model="item.cords[0].lng"
                     required
                  />
               </div>
            </div>

            <div class="field">
               <label
                  class="label has-text-black is-size-6 has-text-left has-text-weight-bold"
                  >Tags</label
               >
               <div class="control">
                  <input
                     class="input"
                     type="text"
                     placeholder="Please enter tags"
                     name="tags"
                     v-model="item.tags"
                  />
               </div>
            </div>

            <div class="select m-5 mb-6">
               <select name="status" v-model="item.status" required>
                  <option value="verified">Verified</option>
                  <option value="unverified">Unverified</option>
               </select>
            </div>
            <div
               class="field is-grouped is-flex is-justify-content-space-around"
            >
               <div class="control">
                  <button
                     type="submit"
                     aria-label="close"
                     class="button is-link"
                     @click="onSubmit"
                  >
                     Submit
                  </button>
               </div>
               <div class="control">
                  <button
                     type="submit"
                     aria-label="close"
                     class="button is-light"
                     @click="closeCreate"
                  >
                     Discard
                  </button>
               </div>
            </div>
         </div>
         <button class="modal-close is-large" aria-label="close"></button>
      </div>
   </div>
</template>
<script>
import { ref } from '@vue/reactivity'
export default {
   setup(_, { emit }) {
      const closeCreate = () => emit('close-create')

      const item = ref({
         name: '',
         cords: [
            {
               lat: '',
               lng: ''
            }
         ],
         tags: [],
         status: 'unverified'
      })

      const onSubmit = () => {
         if (
            item.value.name &&
            item.value.tags.length !== 0 &&
            item.value.cords[0].lat &&
            item.value.cords[0].lng
         ) {
            emit('submit', item.value)
            item.value = {
               name: '',
               cords: [
                  {
                     lat: '',
                     lng: ''
                  }
               ],
               tags: [],
               status: 'unverified'
            }
         } else {
            alert('Please fill all fields')
         }
      }
      return { closeCreate, onSubmit, item }
   }
}
</script>
<style lang="scss" scoped></style>
