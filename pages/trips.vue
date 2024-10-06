<template>
  <div>
    <h2 class="text-2xl font-bold mb-4">Plan Your Trip</h2>
    <form @submit.prevent="saveTrip" class="mb-4">
      <input v-model="destination" placeholder="Destination" required class="input input-bordered mr-2">
      <input v-model="startDate" type="date" required class="input input-bordered mr-2">
      <input v-model="endDate" type="date" required class="input input-bordered mr-2">
      <button type="submit" class="btn btn-primary">Save Trip</button>
    </form>
    <ul>
      <li v-for="trip in trips" :key="trip.id" class="mb-2">
        {{ trip.destination }} ({{ trip.startDate }} - {{ trip.endDate }})
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useFirebase } from '~/composables/useFirebase'
import { getFirestore, collection, addDoc, getDocs } from 'firebase/firestore'
import { format } from 'date-fns'

const { initializeFirebase } = useFirebase()
const { db } = initializeFirebase()

const destination = ref('')
const startDate = ref('')
const endDate = ref('')
const trips = ref([])

const fetchTrips = async () => {
  const querySnapshot = await getDocs(collection(db, 'trips'))
  trips.value = querySnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))
}

const saveTrip = async () => {
  await addDoc(collection(db, 'trips'), {
    destination: destination.value,
    startDate: format(new Date(startDate.value), 'yyyy-MM-dd'),
    endDate: format(new Date(endDate.value), 'yyyy-MM-dd')
  })
  destination.value = ''
  startDate.value = ''
  endDate.value = ''
  await fetchTrips()
}

onMounted(fetchTrips)
</script>