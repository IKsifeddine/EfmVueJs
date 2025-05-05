<template>
  <div class="min-h-screen bg-gray-100 py-8 px-4">
    <div class="max-w-6xl mx-auto space-y-8">
      <header class="text-center">
        <h1 class="text-4xl font-bold text-gray-900">Gestionnaire de Contacts</h1>
      </header>

      <main class="space-y-8">
        <!-- Form Section -->
        <section v-if="showForm" class="max-w-2xl mx-auto">
          <ContactForm
            :contact="selectedContact"
            :loading="loading"
            @save="handleSave"
            @cancel="showForm = false"
          />
        </section>

        <!-- List Section -->
        <section v-else>
          <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-semibold text-gray-800">Liste des contacts</h2>
            <button
              @click="showForm = true"
              class="bg-green-600 hover:bg-green-700 text-white px-6 py-2 rounded-lg"
            >
              Nouveau contact
            </button>
          </div>

          <ContactList
            :contacts="contacts"
            :loading="loading"
            @edit-contact="handleEdit"
            @delete-contact="handleDelete"
          />
        </section>
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import ContactForm from './components/ContactForm.vue'
import ContactList from './components/ContactList.vue'

// State
const contacts = ref([])
const loading = ref(false)
const showForm = ref(false)
const selectedContact = ref(null)

// API URL
const API_URL = 'http://localhost:3001/contacts'

// Fetch contacts
const fetchContacts = async () => {
  loading.value = true
  try {
    const response = await axios.get(API_URL)
    contacts.value = response.data
  } catch (error) {
    console.error('Error fetching contacts:', error)
    alert('Erreur lors du chargement des contacts')
  } finally {
    loading.value = false
  }
}

// Add or update contact
const handleSave = async (contact) => {
  loading.value = true
  try {
    if (contact.id) {
      // Update
      await axios.put(`${API_URL}/${contact.id}`, contact)
    } else {
      // Create
      await axios.post(API_URL, contact)
    }
    await fetchContacts()
    showForm.value = false
    selectedContact.value = null
  } catch (error) {
    console.error('Error saving contact:', error)
    alert('Erreur lors de l\'enregistrement du contact')
  } finally {
    loading.value = false
  }
}

// Edit contact
const handleEdit = (contact) => {
  selectedContact.value = contact
  showForm.value = true
}

// Delete contact
const handleDelete = async (id) => {
  if (!confirm('Êtes-vous sûr de vouloir supprimer ce contact ?')) return

  loading.value = true
  try {
    await axios.delete(`${API_URL}/${id}`)
    await fetchContacts()
  } catch (error) {
    console.error('Error deleting contact:', error)
    alert('Erreur lors de la suppression du contact')
  } finally {
    loading.value = false
  }
}

// Initial load
onMounted(fetchContacts)
</script>

<style>
.card {
  @apply bg-white rounded-xl shadow-md p-6;
}
</style>
