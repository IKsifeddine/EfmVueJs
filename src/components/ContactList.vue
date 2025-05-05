<template>
  <div class="space-y-4">
    <div v-if="loading" class="card p-8 text-center">
      <p class="text-gray-600">Chargement des contacts...</p>
    </div>

    <div v-else-if="contacts.length === 0" class="card p-8 text-center">
      <p class="text-gray-600">Aucun contact trouvé.</p>
      <p class="text-sm text-gray-500 mt-2">Commencez par ajouter un nouveau contact.</p>
    </div>

    <div v-else class="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
      <div 
        v-for="contact in contacts" 
        :key="contact.id" 
        class="card p-6 transition-transform hover:scale-105"
      >
        <div class="flex justify-between items-start mb-4">
          <h3 class="text-xl font-semibold text-gray-800">{{ contact.name }}</h3>
          <div class="flex space-x-2">
            <button 
              @click="$emit('edit-contact', contact)" 
              class="text-blue-600 hover:text-blue-800"
              title="Modifier"
            >
              ✏️
            </button>
            <button 
              @click="$emit('delete-contact', contact.id)" 
              class="text-red-600 hover:text-red-800"
              title="Supprimer"
            >
              🗑️
            </button>
          </div>
        </div>
        
        <div class="space-y-2 text-gray-600">
          <p><span class="mr-2">📧</span><a :href="'mailto:' + contact.email">{{ contact.email }}</a></p>
          <p><span class="mr-2">📱</span><a :href="'tel:' + contact.phone">{{ contact.phone }}</a></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  contacts: Array,
  loading: Boolean
})

defineEmits(['edit-contact', 'delete-contact'])
</script>
