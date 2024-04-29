<template>
  <div class="todo-list max-w-md mx-auto p-4">
    <h2 class="text-2xl font-bold text-blue-700 mb-4">ToDo List</h2>
    <!-- Input pour ajouter une nouvelle tâche -->
    <div class="flex items-center">
      <input
        type="text"
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Ajouter une tâche..."
        class="w-full p-2 mb-4 border rounded"
      />
      <button @click="addTask" class="px-4 py-2 bg-blue-500 ml-2 mb-4 text-white rounded">
        +
      </button>
    </div>
    <!-- Liste des tâches -->
    <ul>
      <li
        v-for="(task, index) in tasks"
        :key="index"
        class="flex items-center mb-2 bg-gray-100 p-2 rounded"
      >
        <!-- Checkbox pour marquer la tâche comme complétée -->
        <input
          type="checkbox"
          v-model="task.completed"
          class="mr-2 form-checkbox text-blue-500"
        />
        <span
          :class="{ 'line-through': task.completed }"
          class="flex-1 text-lg"
          >{{ task.description }}</span
        >
        <!-- Bouton pour supprimer une tâche -->
        <button
          @click="deleteTask(index)"
          class="ml-2 px-3 py-1 bg-red-500 text-white rounded"
        >
          Supprimer
        </button>
      </li>
    </ul>

    <!-- Bouton pour effacer toutes les tâches complétées -->
    <button
      @click="deleteCompletedTasks"
      :disabled="tasks <= 0"
      :class="{ 'opacity-50 cursor-not-allowed': tasks <= 0 }"
      class="mt-4 px-4 py-2 bg-blue-500 text-white rounded"
    >
      Effacer les tâches complétées
    </button>

    <!-- Bouton pour télécharger la liste en PDF -->
    <button
      @click="downloadPDF"
      :disabled="tasks <= 0"
      :class="{ 'opacity-50 cursor-not-allowed': tasks <= 0 }"
      class="mt-4 px-4 py-2 bg-green-500 text-white rounded"
    >
      Télécharger la liste en PDF
    </button>
  </div>
</template>

<script>
import jsPDF from "jspdf";

export default {
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({ description: this.newTask.trim(), completed: false });
        this.newTask = "";
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    deleteCompletedTasks() {
      this.tasks = this.tasks.filter((task) => !task.completed);
    },
    downloadPDF() {
      const doc = new jsPDF();

      const title = "ToDo List";
      const taskCount = this.tasks.length;

      // Définition des marges et des dimensions de la page
      const marginLeft = 10;
      const marginTop = 20;
      const contentWidth = doc.internal.pageSize.width - 2 * marginLeft;
      const lineHeight = 10;
      const checkboxSize = 7; // Taille du carré pour la checkbox
      let currentY = marginTop;

      // Fond coloré pour le titre
      doc.setFillColor("#ff7e5f");
      doc.rect(marginLeft, currentY, contentWidth, 30, "F");

      // Titre
      doc.setFontSize(24);
      doc.setTextColor("#fff");
      doc.setFont("bold");
      doc.text(title, marginLeft + 10, currentY + 20);

      currentY += 40; // Espacement après le titre

      // Ajout de la liste des tâches
      doc.setFontSize(14);
      doc.setTextColor("#333");
      this.tasks.forEach((task, index) => {
        // Dessiner la checkbox
        doc.setDrawColor("#333");
        doc.setLineWidth(0.5);
        doc.roundedRect(
          marginLeft,
          currentY - 6,
          checkboxSize,
          checkboxSize,
          1,
          1,
          "D"
        );

        if (task.completed) {
          doc.setFillColor("#333"); // Couleur de remplissage pour les tâches complétées
          doc.roundedRect(
            marginLeft + 1,
            currentY - 5,
            checkboxSize - 2,
            checkboxSize - 2,
            1,
            1,
            "F"
          );
        }

        // Ajouter la description de la tâche
        const text = `${index + 1}. ${task.description}`;
        doc.text(text, marginLeft + 20, currentY);

        currentY += lineHeight;
      });

      // Ajout de la ligne séparatrice en bas de la liste des tâches
      doc.setLineWidth(0.5);
      doc.setDrawColor("#333");
      doc.line(marginLeft, currentY, marginLeft + contentWidth, currentY);

      // Ajout du footer avec le nombre de tâches
      doc.setFontSize(12);
      doc.setTextColor("#333");
      const footer = `Total des tâches : ${taskCount}`;
      doc.text(footer, marginLeft, currentY + 20);

      // Téléchargement du PDF
      doc.save("ToDoList.pdf");
    },
  },
};
</script>

<style scoped>
.todo-list {
  font-family: Arial, sans-serif;
}
</style>
