<template>
  <div class="contador-container">
    <h2>Contador em Vue.js</h2>
    
    <!-- Estilo condicional: verde para par, azul para ímpar -->
    <h3 :style="{ color: verificarParOuImpar === 'Par' ? 'green' : 'blue' }">
      Valor Atual: {{ contador }} ({{ verificarParOuImpar }})
    </h3>
    
    <div class="botoes">
      <!-- Desabilita o botão se for 0 para não permitir negativos -->
      <button @click="decrementar" :disabled="contador === 0">- Decrementar</button>
      <button @click="incrementar">+ Incrementar</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';

// 1. Prop para valor inicial
const props = defineProps({
  valorInicial: {
    type: Number,
    default: 0
  }
});

// 2. Emit de evento quando o valor mudar
const emit = defineEmits(['valorMudou']);

// 3. Estado reativo iniciado com a prop
const contador = ref(props.valorInicial);

// 4. Computed property para mostrar se é par ou ímpar
const verificarParOuImpar = computed(() => {
  return contador.value % 2 === 0 ? 'Par' : 'Ímpar';
});

// 5. Watch para alertar quando chegar a 10
watch(contador, (novoValor) => {
  if (novoValor === 10) {
    alert('Alerta: O contador atingiu o valor 10!');
  }
});

// Funções de interação
const incrementar = () => {
  contador.value++;
  emit('valorMudou', contador.value);
};

const decrementar = () => {
  // Garantia extra no JavaScript para não permitir valores negativos
  if (contador.value > 0) {
    contador.value--;
    emit('valorMudou', contador.value);
  }
};
</script>

<style scoped>
.contador-container {
  border: 2px solid #ccc;
  border-radius: 8px;
  padding: 20px;
  text-align: center;
  max-width: 300px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
}

.botoes button {
  padding: 10px 15px;
  margin: 0 5px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  background-color: #4CAF50;
  color: white;
}

.botoes button:hover:not(:disabled) {
  background-color: #45a049;
}

.botoes button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
</style>
