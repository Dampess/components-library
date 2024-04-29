<template>
  <div class="password-generator p-4 bg-gray-100 rounded-lg shadow-md">
    <label for="password-length" class="block mb-2"
      >Longueur du mot de passe :</label
    >
    <input
      type="number"
      id="password-length"
      v-model.number="passwordLength"
      min="1"
      :max="maxPasswordLength"
      class="border border-gray-300 rounded-md py-2 px-4 mb-4"
    />

    <div class="options mb-4">
      <label class="inline-flex items-center">
        <input
          type="checkbox"
          v-model="includeNumbers"
          class="form-checkbox mr-2"
        />
        Inclure des chiffres (0-9)
      </label>
      <label class="inline-flex items-center ml-4">
        <input
          type="checkbox"
          v-model="includeUppercase"
          class="form-checkbox mr-2"
        />
        Inclure des majuscules (A-Z)
      </label>
      <label class="inline-flex items-center ml-4">
        <input
          type="checkbox"
          v-model="includeSpecialChars"
          class="form-checkbox mr-2"
        />
        Inclure des caractères spéciaux (!@#$%^&*)
      </label>
    </div>

    <button
      @click="generatePassword"
      class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
    >
      Générer un mot de passe
    </button>

    <div v-if="password" class="generated-password mt-4">
      <label for="password" class="block mb-2">Mot de passe généré :</label>
      <div class="flex">
        <input
          type="text"
          id="password"
          :value="password"
          readonly
          class="border border-gray-300 rounded-md py-2 px-4 bg-gray-200 w-96"
        />
        <button
          @click="copyPassword"
          class="ml-2 py-2 px-4 bg-blue-500 text-white rounded-md"
        >
          Copier
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      passwordLength: 8,
      maxPasswordLength: 32,
      includeNumbers: true,
      includeUppercase: true,
      includeSpecialChars: false,
      password: null,
    };
  },
  methods: {
    generatePassword() {
      let charset = "abcdefghijklmnopqrstuvwxyz";
      if (this.includeNumbers) charset += "0123456789";
      if (this.includeUppercase) charset += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      if (this.includeSpecialChars) charset += "!@#$%^&*";

      let generatedPassword = "";
      for (let i = 0; i < this.passwordLength; i++) {
        const randomIndex = Math.floor(Math.random() * charset.length);
        generatedPassword += charset[randomIndex];
      }

      this.password = generatedPassword;
    },
    copyPassword() {
      const input = document.getElementById("password");
      input.select();
      document.execCommand("copy");
      // Vous pouvez ajouter un message ou une logique supplémentaire ici après la copie réussie
    },
  },
};
</script>

<style scoped>
.password-generator {
  margin-bottom: 1rem;
}

.options {
  margin-top: 0.5rem;
}

.generated-password {
  margin-top: 1rem;
}
</style>
