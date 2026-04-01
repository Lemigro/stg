<script setup>
const kpis = [
  { label: 'Faturamento 2024', value: 'R$ 45,9 mi' },
  { label: 'Filiais mapeadas', value: '108' },
  { label: 'Transacoes', value: '900' },
  { label: 'Modelos analisados', value: '9' }
];
</script>

<template>
  <section class="panel">
    <h2>Componente Vue dentro do Astro</h2>
    <p>Este bloco e renderizado pelo Vue 3 usando a integracao oficial @astrojs/vue.</p>

    <div class="grid">
      <article v-for="item in kpis" :key="item.label" class="card">
        <strong>{{ item.value }}</strong>
        <span>{{ item.label }}</span>
      </article>
    </div>
  </section>
</template>

<style scoped>
.panel {
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 14px;
  padding: 1.25rem;
  background: rgba(255, 255, 255, 0.03);
}

h2 {
  margin: 0 0 0.25rem;
  font-size: 1.2rem;
}

p {
  margin: 0;
  color: #9ea3b0;
}

.grid {
  margin-top: 1rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 0.75rem;
}

.card {
  padding: 0.75rem;
  border-radius: 10px;
  background: rgba(74, 143, 255, 0.12);
  border: 1px solid rgba(74, 143, 255, 0.2);
  display: flex;
  flex-direction: column;
}

.card strong {
  font-size: 1rem;
}

.card span {
  color: #cad0dc;
  font-size: 0.82rem;
}
</style>
