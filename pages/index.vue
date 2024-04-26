<template>
  <div class="container mx-auto px-4">
    <h1 class="text-3xl font-semibold mb-8 text-center">Components library</h1>
    <h2 class="text-xl font-semibold mb-4">Image Editor</h2>
    <ImageEditor @avatarSaved="handleImageCropped" :message="avatarMessage" />
    <div v-if="croppedImageUrl" class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Cropped Image</h2>
      <img :src="croppedImageUrl" alt="Cropped Image" class="max-w-lg" />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Scramble Text Demo</h2>
      <scranmble-text
        :texts="['Hello', 'Bonjour', 'Hola', 'Ciao']"
        :interval="2000"
        class="text-center text-4xl text-black bg-red-600"
      />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Progress Bar Demo</h2>
      <ProgressBarre :progress="progress" color="blue-500" height="h-6" />
      <button
        @click="startProgress()"
        class="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
      >
        Début
      </button>
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Chronometer Timer Demo</h2>
      <Timer />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Star Rating Demo</h2>
      <StarRating @rating-selected="handleRatingSelected" />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Table Demo</h2>
      <Table :data="tableData" :columns="tableColumns" />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Foire aux questions</h2>
      <Accordion :items="faqItems" />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Générateur de Mots de Passe</h2>
      <PasswordGenerator />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Horloge Analogique Interactive</h2>
      <Clock />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">ToDoList</h2>
      <ToDoList />
    </div>
    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Reactions</h2>
      <LikeDislike :custom-icons="customIcons"/>
    </div>
    <div class="mt-8 mb-8">
      <h2 class="text-xl font-semibold mb-4">Contacts Directory</h2>
      <Contacts/>
    </div>
  </div>
</template>

<script>
export default {
  name: "ImageEditorPage",

  data() {
    return {
      croppedImageUrl: null,
      progress: 0, // Exemple de progression (50%)
      startTime: null, // Ajoutez cette variable
      avatarMessage: "Choose an image to edit:",
      selectedRating: 2, // Stocker la note sélectionnée
      tableData: [
        { id: 1, name: "John", age: 30 },
        { id: 2, name: "Alice", age: 25 },
        { id: 3, name: "Bob", age: 35 },
      ],
      tableColumns: [
        { key: "id", label: "ID" },
        { key: "name", label: "Name" },
        { key: "age", label: "Age" },
      ],
      faqItems: [
        {
          title: "Qu'est-ce que Vue.js ?",
          content:
            "Vue.js est un framework JavaScript progressif pour la construction d'interfaces utilisateur.",
        },
        {
          title: "Comment commencer avec Vue.js ?",
          content:
            "Vous pouvez commencer avec Vue.js en suivant le guide officiel sur le site Web de Vue.js.",
        },
        // Ajoutez autant de questions et réponses que nécessaire
      ],
      customIcons: [
        { url: 'happy.png', label: 'Happy' },
        { url: 'sad.png', label: 'Sad' },
        { url: 'angry.png', label: 'Angry' }
      ]
    };
  },

  methods: {
    handleImageCropped(blob) {
      this.croppedImageUrl = URL.createObjectURL(blob);
    },
    startProgress() {
      // Mettre à jour startTime ici pour démarrer la progression à partir de ce moment-là
      this.startTime = Date.now();
      setInterval(this.progressBar, 100); // Démarrer l'intervalle pour mettre à jour la progression
    },
    progressBar() {
      const totalTime = 15000; // 15 secondes en millisecondes

      const elapsedTime = Date.now() - this.startTime; // Utiliser this.startTime
      let percentage = (elapsedTime / totalTime) * 100;
      percentage = Math.min(percentage, 100); // Limiter le pourcentage à 100%
      this.progress = +percentage.toFixed(2); // Mettre à jour la progression
    },
    handleRatingSelected(rating) {
      // Mettre à jour la note sélectionnée
      this.selectedRating = rating;
    },
  },
};
</script>
