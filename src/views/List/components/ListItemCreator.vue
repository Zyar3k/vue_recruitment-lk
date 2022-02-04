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
                     name="cords"
                     required
                  />
                  <input
                     class="input ml-1"
                     type="number"
                     placeholder="Please enter longitude"
                     name="cords"
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
                  />
               </div>
            </div>

            <div class="select m-5 mb-6">
               <select name="status" required>
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
export default {
   setup(_, { emit }) {
      const closeCreate = () => emit('close-create')
      return { closeCreate }
   }
}
</script>
<style lang="scss" scoped></style>
