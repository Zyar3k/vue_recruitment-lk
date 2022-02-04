<template>
   <div class="p-2">
      <div class="field">
         <div class="control is-flex">
            <input
               @input="onInput"
               placeholder="Type here..."
               type="text"
               class="input"
            />
            <button @click="openCreate" class="ml-2 button is-link ">
               Create New Nursery
            </button>
         </div>
      </div>
      <Table :content="tableContent" :config="tableConfig" @select="onSelect" />
      <Modal
         v-if="isModalOpen"
         :itemData="itemData"
         @submit="onSubmit"
         @close-modal="onCloseModal"
      />
      <ListItemCreator
         v-if="isCreateMode"
         @close-create="onCloseCreate"
         @submit="onCreate"
      />
   </div>
</template>
<script>
import Table from '@/components/Table.vue'
import Modal from '@/components/Modal.vue'
import ListItemCreator from '@/views/List/components/ListItemCreator.vue'
import { computed, onMounted, reactive, ref } from 'vue'
import { filterList, mapList } from './listHelper'
import dummy from '@/assets/dummy.json'
import timeout from 'q'
export default {
   components: { Table, Modal, ListItemCreator },
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
      const isCreateMode = ref(false)
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
            }, 5)
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

      const openCreate = () => {
         isCreateMode.value = true
      }
      const onCloseCreate = () => {
         isCreateMode.value = false
      }
      const onCreate = ({ name, status, cords, tags }) => {
         console.log(name, status, cords, tags)

         const latitude = cords[0].lat
         const longitude = cords[0].lng
         const cordsArray = [latitude, longitude]

         let tagsArray
         if (tags.length > 0) {
            tagsArray = tags.split(',')
         } else {
            tagsArray = []
         }

         const newItem = {
            id: state.items.length + 1,
            name: name,
            cords: cordsArray,
            fib: 8,
            ratings: 123,
            status,
            egassem: 'mainevni',
            tags: tagsArray,
            img:
               'https://www.agkolkata.org/wp-content/uploads/2017/08/Kingdom-kids-logo.png'
         }

         state.items.push(newItem)
         isCreateMode.value = false
      }
      return {
         isCreateMode,
         isModalOpen,
         itemData,
         onCloseModal,
         onCloseCreate,
         onCreate,
         openCreate,
         onInput,
         onSelect,
         onSubmit,
         tableConfig,
         tableContent
      }
   }
}
</script>
