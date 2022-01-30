<template>
   <div class="modal is-active">
      <div class="modal-background" @click="closeModal"></div>
      <div class="modal-content">
         <div @submit.prevent="" class="card p-6">
            <h2 class="has-text-black is-size-5 has-text-weight-bold">
               Edit Nursery
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
            <div class="select m-5 mb-6">
               <select name="status" v-model="item.status">
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
                     @click="closeModal"
                  >
                     Discard
                  </button>
               </div>
            </div>
         </div>
         <button
            class="modal-close is-large"
            aria-label="close"
            @click="closeModal"
         ></button>
      </div>
   </div>
</template>

<script>
import { ref } from 'vue'
export default {
   props: {
      itemData: {
         type: Object,
         default: () => ({})
      }
   },
   setup(props, { emit }) {
      const closeModal = () => emit('close-modal')
      const item = ref({
         name: props.itemData.name,
         status: props.itemData.status
      })
      const onSubmit = () => {
         emit('submit', item.value)
      }
      return {
         closeModal,
         item,
         onSubmit
      }
   }
}
</script>
