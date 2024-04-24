<template>
  <div class="contacts-directory">
    <!-- Barre de recherche -->
    <input type="text" v-model="searchQuery" placeholder="Rechercher un contact..." class="w-full p-2 mb-4 border rounded">

    <!-- Liste des contacts -->
    <ul>
      <li v-for="contact in filteredContacts" :key="contact.id" class="flex items-center justify-between mb-2 bg-gray-100 p-2 rounded">
        <!-- Affichage des détails du contact -->
        <div class="flex items-center">
          <div class="mr-2">{{ contact.name }}</div>
          <div>{{ contact.email }}</div>
        </div>
        <!-- Boutons d'action pour chaque contact -->
        <div>
          <button @click="editContact(contact)" class="px-3 py-1 bg-blue-500 text-white rounded">Modifier</button>
          <button @click="deleteContact(contact.id)" class="ml-2 px-3 py-1 bg-red-500 text-white rounded">Supprimer</button>
        </div>
      </li>
    </ul>

    <!-- Bouton d'ajout de contact -->
    <button @click="showAddModal" class="mt-4 px-4 py-2 bg-green-500 text-white rounded">Ajouter un contact</button>

    <!-- Modal pour ajouter ou éditer un contact -->
    <div v-if="showModal" class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-gray-900 bg-opacity-50">
      <div class="bg-white p-4 rounded shadow-lg">
        <h2 class="text-lg font-semibold mb-2">{{ modalTitle }}</h2>
        <input type="text" v-model="formData.name" placeholder="Nom" class="w-full p-2 mb-2 border rounded">
        <input type="text" v-model="formData.email" placeholder="Email" class="w-full p-2 mb-2 border rounded">
        <div class="flex justify-end">
          <button @click="saveContact" class="px-3 py-1 bg-blue-500 text-white rounded">Enregistrer</button>
          <button @click="closeModal" class="ml-2 px-3 py-1 bg-red-500 text-white rounded">Annuler</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      contacts: [
        { id: 1, name: 'John Doe', email: 'john@example.com' },
        { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
        // Ajoutez d'autres contacts ici...
      ],
      searchQuery: '',
      showModal: false,
      modalTitle: '',
      formData: {
        name: '',
        email: ''
      },
      editMode: false,
      selectedContactId: null
    };
  },
  computed: {
    filteredContacts() {
      return this.contacts.filter(contact => {
        return contact.name.toLowerCase().includes(this.searchQuery.toLowerCase()) || contact.email.toLowerCase().includes(this.searchQuery.toLowerCase());
      });
    }
  },
  methods: {
    showAddModal() {
      this.showModal = true;
      this.modalTitle = 'Ajouter un contact';
      this.editMode = false;
      this.formData = {
        name: '',
        email: ''
      };
    },
    editContact(contact) {
      this.showModal = true;
      this.modalTitle = 'Modifier le contact';
      this.editMode = true;
      this.formData = {
        name: contact.name,
        email: contact.email
      };
      this.selectedContactId = contact.id;
    },
    saveContact() {
      if (this.editMode) {
        const index = this.contacts.findIndex(contact => contact.id === this.selectedContactId);
        if (index !== -1) {
          this.contacts[index].name = this.formData.name;
          this.contacts[index].email = this.formData.email;
        }
      } else {
        const newId = this.contacts.length > 0 ? this.contacts[this.contacts.length - 1].id + 1 : 1;
        this.contacts.push({
          id: newId,
          name: this.formData.name,
          email: this.formData.email
        });
      }
      this.closeModal();
    },
    deleteContact(contactId) {
      this.contacts = this.contacts.filter(contact => contact.id !== contactId);
    },
    closeModal() {
      this.showModal = false;
      this.editMode = false;
      this.selectedContactId = null;
      this.formData = {
        name: '',
        email: ''
      };
    }
  }
};
</script>

<style scoped>
.contacts-directory {
  font-family: Arial, sans-serif;
}

/* Styles pour le modal */
.fixed {
  position: fixed;
}
.top-0 {
  top: 0;
}
.left-0 {
  left: 0;
}
.w-full {
  width: 100%;
}
.h-full {
  height: 100%;
}
.flex {
  display: flex;
}
.items-center {
  align-items: center;
}
.justify-between {
  justify-content: space-between;
}
.bg-gray-100 {
  background-color: #f3f4f6;
}
.p-2 {
  padding: 0.5rem;
}
.mb-4 {
  margin-bottom: 1rem;
}
.border {
  border: 1px solid #d1d5db;
}
.rounded {
  border-radius: 0.375rem;
}
.bg-green-500 {
  background-color: #34d399;
}
.bg-blue-500 {
  background-color: #3b82f6;
}
.bg-red-500 {
  background-color: #ef4444;
}
.text-white {
  color: #ffffff;
}
.shadow-lg {
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
}
.hidden {
  display: none;
}
</style>
