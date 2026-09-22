<script setup>

import axios from 'axios';
import { ref } from 'vue';

const cep = ref('')
const endereco = ref(null)



const buscarCep = async () => {


  
try {
   const resposta = await axios.get(`https://viacep.com.br/ws/${cep.value}/json/`)

  if (resposta.data.erro) {
    throw new Error('Erro ao buscar o CEP')

  }

  endereco.value = resposta.data

} catch (erro) {  
  alert(erro)
}
 
  
  
}




const escuro = ref(document.documentElement.classList.contains('dark'))

const alternarTema = () => {
  document.documentElement.classList.toggle('dark')
  escuro.value = document.documentElement.classList.contains('dark')
}

</script>

<template>
  <div class="min-h-screen w-full flex items-center justify-center px-4 py-8 bg-[var(--bg)] text-[var(--text)] transition-colors">
    <div class="w-full max-w-md rounded-xl p-6 sm:p-8 relative border-2 border-[var(--border)] shadow-[var(--shadow)]">

      <button
        @click="alternarTema"
        class="absolute top-4 right-4 text-xs font-bold rounded-lg px-2.5 py-1 border-2 border-[var(--border)] text-[var(--text)] hover:bg-[var(--code-bg)] transition-colors"
      >
        {{ escuro ? '☀ Claro' : '🌙 Escuro' }}
      </button>

      <h1 class="text-2xl sm:text-3xl font-extrabold text-center tracking-tight text-[var(--text-h)]">
        CepChecker
      </h1>
      <p class="text-sm text-center mt-1 mb-6 text-[var(--text)]">
        Consulte um endereço a partir do CEP
      </p>

      <div class="flex flex-col sm:flex-row gap-2">
        <input
          type="text"
          placeholder="Digite o CEP"
          v-model="cep"
          @keyup.enter="buscarCep"
          minlength="8"
          maxlength="8"
          class="flex-1 rounded-lg px-3 py-2.5 outline-none transition-colors
                 border-2 border-[var(--border)] bg-[var(--bg)] text-[var(--text-h)]
                 placeholder:text-[var(--text)]
                 focus:border-[var(--accent)] focus:ring-2 focus:ring-[var(--accent-bg)]"
        >
        <button
          @click="buscarCep"
          class="rounded-lg px-5 py-2.5 font-bold transition-colors active:translate-y-px
                 border-2 border-[var(--accent)] bg-[var(--accent)] text-white
                 hover:bg-[var(--accent-bg)] hover:text-[var(--accent)]"
        >
          Buscar
        </button>
      </div>

      <div v-if="endereco" class="mt-6 pt-4 flex flex-col gap-2 border-t-2 border-[var(--border)]">
        <div class="flex flex-col sm:flex-row sm:justify-between gap-0.5 sm:gap-3 py-2 border-b border-[var(--border)]">
          <span class="font-bold text-[var(--text-h)]">Logradouro</span>
          <span class="sm:text-right break-words text-[var(--text)]">{{ endereco.logradouro }}</span>
        </div>
        <div class="flex flex-col sm:flex-row sm:justify-between gap-0.5 sm:gap-3 py-2 border-b border-[var(--border)]">
          <span class="font-bold text-[var(--text-h)]">Bairro</span>
          <span class="sm:text-right break-words text-[var(--text)]">{{ endereco.bairro }}</span>
        </div>
        <div class="flex flex-col sm:flex-row sm:justify-between gap-0.5 sm:gap-3 py-2 border-b border-[var(--border)]">
          <span class="font-bold text-[var(--text-h)]">Complemento</span>
          <span class="sm:text-right break-words text-[var(--text)]">{{ endereco.complemento }}</span>
        </div>
        <div class="flex flex-col sm:flex-row sm:justify-between gap-0.5 sm:gap-3 py-2 border-b border-[var(--border)]">
          <span class="font-bold text-[var(--text-h)]">Estado</span>
          <span class="sm:text-right break-words text-[var(--text)]">{{ endereco.uf }}</span>
        </div>
        <div class="flex flex-col sm:flex-row sm:justify-between gap-0.5 sm:gap-3 py-2">
          <span class="font-bold text-[var(--text-h)]">UF</span>
          <span class="sm:text-right break-words text-[var(--text)]">{{ endereco.uf }}</span>
        </div>
      </div>
    </div>
  </div>
</template>