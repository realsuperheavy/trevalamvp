<template>
  <div>
    <h2 class="text-2xl font-bold mb-4">Travel Progress</h2>
    <p class="mb-4">{{ completedCount }} out of {{ totalCount }} destinations visited</p>
    <div class="w-full bg-gray-200 rounded-full h-2.5 dark:bg-gray-700">
      <div class="bg-blue-600 h-2.5 rounded-full" :style="{ width: `${progressPercentage}%` }"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import { useFirebase } from '~/composables/useFirebase'
import { getFirestore, collection, getDocs } from 'firebase/firestore'

const { initializeFirebase } = useFirebase()
const { db } = initializeFirebase()

const completedCount = ref(0)
const totalCount = ref(0)

const progressPercentage = computed(() => {
  return totalCount.value > 0 ? (completedCount.value / totalCount.value) * 100 : 0
})

const fetchProgress = async () => {
  const querySnapshot = await getDocs(collection(db, 'bucketList'))
  const bucketList = querySnapshot.docs.map(doc => doc.data())
  totalCount.value = bucketList.length
  completedCount.value = bucketList.filter(item => item.status === 'visited').length
}

onMounted(fetchProgress)
</script>