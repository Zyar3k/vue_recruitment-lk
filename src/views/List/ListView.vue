<template>
   <div class="p-2">
      <div class="field">
         <div class="control">
            <input
               @input="onInput"
               placeholder="Type here..."
               type="text"
               class="input"
            />
         </div>
      </div>
      <Table :content="tableContent" :config="tableConfig" @select="onSelect" />
      <Modal
         v-if="isModalOpen"
         :itemData="itemData"
         @submit="onSubmit"
         @close-modal="onCloseModal"
      />
   </div>
</template>
<script>
import Table from '@/components/Table.vue'
import Modal from '@/components/Modal.vue'
import { computed, onMounted, reactive, ref } from 'vue'
import { filterList, mapList } from './listHelper'
import dummy from '@/assets/dummy.json'
import timeout from 'q'
export default {
   components: { Table, Modal },
   setup() {
      const tableConfig = {
         columns: [
            { key: 'id', header: 'ID' },
            { key: 'name', header: 'Name' },
            { key: 'cords_display', header: 'Cords' },
            { key: 'tags_display', header: 'Tags' },
            { key: 'status', header: 'Status' }
         ]
      }
      const isModalOpen = ref(false)
      const state = reactive({
         currentItem: null,
         items: [],
         initLoading: true,
         search: '',
         timeout
      })
      const tableContent = computed(() =>
         state.items.filter(item => filterList(item, state.search)).map(mapList)
      )
      const itemData = computed(() => {
         if (!state.currentItem) return {}
         return state.currentItem
      })
      const onInput = ({ target: { value } }) => {
         clearTimeout(timeout)
         state.timeout = setTimeout(() => (state.search = value), 500)
      }
      const mockRequest = () => {
         return new Promise(resolve => {
            setTimeout(() => {
               state.items = dummy
               resolve()
            }, 500)
         })
      }
      onMounted(async () => {
         await mockRequest()
         state.loading = false
      })
      const onSelect = row => {
         state.currentItem = row
         isModalOpen.value = true
      }
      const onCloseModal = () => {
         isModalOpen.value = false
      }
      const onSubmit = ({ name, status }) => {
         const item = state.items.find(item => item.id === state.currentItem.id)
         if (name === '') {
            alert('Name is required')
         } else {
            item.name = name
            item.status = status
            isModalOpen.value = false
            state.currentItem = null
         }
      }
      return {
         isModalOpen,
         itemData,
         onCloseModal,
         onInput,
         onSelect,
         onSubmit,
         tableConfig,
         tableContent
      }
   }
}
</script>
