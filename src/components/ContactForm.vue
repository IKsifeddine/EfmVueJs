<template>
  <div class="card">
    <h2 class="text-2xl font-bold mb-6 text-gray-800">
      {{ contact ? 'Modifier le contact' : 'Ajouter un contact' }}
    </h2>
    
    <form @submit.prevent="handleSubmit" class="space-y-6">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">Nom</label>
        <input 
          v-model="form.name" 
          type="text" 
          required
          placeholder="John Doe"
          class="w-full p-3 rounded-lg bg-gray-50 focus:bg-white"
          :disabled="loading"
        />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">Email</label>
        <input 
          v-model="form.email" 
          type="email" 
          required
          placeholder="john@example.com"
          class="w-full p-3 rounded-lg bg-gray-50 focus:bg-white"
          :disabled="loading"
        />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">Téléphone</label>
        <input 
          v-model="form.phone" 
          type="tel" 
          required
          placeholder="06 12 34 56 78"
          class="w-full p-3 rounded-lg bg-gray-50 focus:bg-white"
          :disabled="loading"
        />
      </div>

      <div class="flex gap-3 pt-2">
        <button 
          type="submit" 
          class="flex-1 bg-primary hover:bg-secondary text-black px-6 py-3 rounded-lg font-medium"
          :disabled="loading"
        >
          <span v-if="loading">Enregistrement...</span>
          <span v-else>{{ contact ? 'Mettre à jour' : 'Ajouter' }}</span>
        </button>
        <button 
          type="button" 
          @click="$emit('cancel')"
          class="px-6 py-3 rounded-lg font-medium text-gray-700 hover:bg-gray-100"
          :disabled="loading"
        >
          Annuler
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  contact: Object,
  loading: Boolean
})

const emit = defineEmits(['save', 'cancel'])

const form = ref({
  name: '',
  email: '',
  phone: ''
})

watch(() => props.contact, (newContact) => {
  if (newContact) {
    form.value = { ...newContact }
  } else {
    form.value = {
      name: '',
      email: '',
      phone: ''
    }
  }
}, { immediate: true })

const handleSubmit = () => {
  emit('save', {
    ...form.value,
    id: props.contact?.id
  })
}
</script>
