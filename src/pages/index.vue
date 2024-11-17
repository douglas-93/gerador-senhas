<template>
  <v-container fluid max-width="45vw">
    <v-card class="pa-10">
      <h1 class="text-center mb-5">Gerador de Senhas</h1>
      <v-text-field
        variant="outlined"
        readonly
        :append-inner-icon="'mdi-content-copy'"
        :append-icon="'mdi-refresh'"
        v-model="password"
        @click:append="gerar"
        @click:append-inner="copyToClipboard"
      ></v-text-field>
      <v-row>
        <v-checkbox v-model="numbersChecked" label="Incluir números"></v-checkbox>
        <v-checkbox v-model="lowercaseLettersChecked" label="Incluir minúsculas"></v-checkbox>
        <v-checkbox v-model="uppercaseLettersChecked" label="Incluir maiúsculas"></v-checkbox>
        <v-checkbox v-model="specialCharactersChecked" label="Incluir caracteres especiais"></v-checkbox>
      </v-row>

      <v-row>
        <v-col cols="12" md="8">
          <v-slider
            label="Tamanho"
            min="8"
            max="80"
            :step="1"
            v-model="length"
          ></v-slider>
        </v-col>
        <v-col cols="12" md="4">
          <v-label>{{ length }}</v-label>
        </v-col>
      </v-row>

      <v-row class="d-flex justify-center">
        <v-btn
          @click="gerar"
          color="success"
        >Gerar</v-btn>
      </v-row>
    </v-card>
  </v-container>

  <v-snackbar
    v-model="snackbar"
    :timeout="3000"
    color="success"
    variant="tonal"
  >
    {{ message }}

    <template v-slot:actions>
      <v-btn
        variant="text"
        @click="snackbar = false"
      >
        <v-icon icon="mdi-close"></v-icon>
      </v-btn>
    </template>
  </v-snackbar>
</template>

<script lang="ts" setup>
import {ref} from "vue";

const length = ref(12);
const message = ref('');
const snackbar = ref(false);
const uppercaseLetters = [
  'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M',
  'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'
];
const lowercaseLetters = [
  'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm',
  'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'
];
const numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
const specialCharacters = [
  '!', '@', '#', '$', '%', '^', '&', '*', '(', ')', '-', '_', '=',
  '+', '[', ']', '{', '}', '|', ';', ':', ',', '.', '<', '>', '/',
  '?', '\\', '~', '`'
];

const lowercaseLettersChecked = ref(true);
const uppercaseLettersChecked = ref(true);
const numbersChecked = ref(true);
const specialCharactersChecked = ref(true);
const password = ref('');

const gerar = () => {
  // Cria um novo array para evitar modificar os arrays originais
  const usedChars = [];

  if (lowercaseLettersChecked.value) {
    usedChars.push(...lowercaseLetters);
  }
  if (uppercaseLettersChecked.value) {
    usedChars.push(...uppercaseLetters);
  }
  if (numbersChecked.value) {
    usedChars.push(...numbers);
  }
  if (specialCharactersChecked.value) {
    usedChars.push(...specialCharacters);
  }

  let pass = '';
  // Use `usedChars.length` para garantir que o índice esteja dentro do comprimento correto
  for (let i = 0; i < length.value; i++) {
    pass += usedChars[Math.floor(Math.random() * usedChars.length)];
  }
  password.value = pass;
};

const copyToClipboard = () => {
  navigator.clipboard.writeText(password.value);
  message.value = 'Copiado com sucesso!';
  snackbar.value = true;
}

</script>
