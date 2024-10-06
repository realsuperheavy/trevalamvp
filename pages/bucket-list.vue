<template>
  <div>
    <h2 class="text-2xl font-bold mb-4">My Bucket List</h2>
    <form @submit.prevent="addDestination" class="mb-4">
      <input v-model="newDestination" placeholder="New destination" required class="input input-bordered mr-2">
      <select v-model="newCategory" class="select select-bordered mr-2">
        <option value="Adventure">Adventure</option>
        <option value="Culture">Culture</option>
        <option value="Relaxation">Relaxation</option>
      </select>
      <button type="submit" class="btn btn-primary">Add</button>
    </form>
    <ul>
      <li v-for="item in bucketList" :key="item.id" class="mb-2">
        {{ item.destination }} - {{ item.category }}
        <button @click="markAsVisited(item)" class="btn btn-sm btn-secondary ml-2">Mark as Visited</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useFirebase } from '~/composables/useFirebase'
import { getFirestore, collection, addDoc, getDocs, updateDoc, doc } from 'firebase/firestore'

const { initializeFirebase } = useFirebase()
const { db } = initializeFirebase()

const newDestination = ref('')
const newCategory = ref('Adventure')
const bucketList = ref([])

const fetchBucketList = async () => {
  const querySnapshot = await getDocs(collection(db, 'bucketList'))
  bucketList.value = querySnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))
}

const addDestination = async () => {
  await addDoc(collection(db, 'bucketList'), {
    destination: newDestination.value,
    category: newCategory.value,
    status: 'pending'
  })
  newDestination.value = ''
  newCategory.value = 'Adventure'
  await fetchBucketList()
}

const markAsVisited = async (item) => {
  await updateDoc(doc(db, 'bucketList', item.id), { status: 'visited' })
  await fetchBucketList()
}

onMounted(fetchBucketList)
</script>