<script setup>
import { onBeforeUnmount, onMounted } from 'vue';
import Chart from 'chart.js/auto';

const props = defineProps({
  html: {
    type: String,
    default: ''
  }
});

let anualChart;
let mensalChart;
let carChart;
let vendChart;
let filialChart;
let keyHandler;

onMounted(() => {
  const BL = '#1a3d6e';
  const GR = '#1a5c3a';
  const GO = '#c9a84c';
  const RE = '#8b2635';
  const PU = '#5b2d82';
  const GRID = 'rgba(0,0,0,0.05)';
  const baseOpts = { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } };
  const tk = { color: '#6b6b6b', font: { family: "'Inter',sans-serif", size: 11 } };

  // Annual
  anualChart = new Chart('cAnual', {
    type: 'bar',
    data: {
      labels: ['2022', '2023', '2024'],
      datasets: [{ data: [42791, 45453, 45883], backgroundColor: [BL, GO, RE], borderRadius: 4, borderSkipped: false }]
    },
    options: {
      ...baseOpts,
      scales: {
        y: { ticks: { ...tk, callback: (v) => `${(v / 1000).toFixed(0)}mi` }, grid: { color: GRID }, border: { display: false } },
        x: { ticks: tk, grid: { display: false }, border: { display: false } }
      }
    }
  });

  // Monthly
  const mensalData = {
    2022: [2948, 2889, 3847, 3155, 2354, 5749, 4040, 3381, 3090, 2358, 4454, 4526],
    2023: [4764, 3561, 4054, 2947, 4845, 3325, 3493, 4094, 4285, 3191, 3271, 3623],
    2024: [2936, 4543, 2270, 3943, 3399, 4320, 3268, 4302, 4419, 4239, 4120, 4124]
  };
  const meses = ['Jan', 'Fev', 'Mar', 'Abr', 'Mai', 'Jun', 'Jul', 'Ago', 'Set', 'Out', 'Nov', 'Dez'];
  const mensalColors = { 2022: BL, 2023: GO, 2024: RE };

  mensalChart = new Chart('cMensal', {
    type: 'line',
    data: {
      labels: meses,
      datasets: [{ data: mensalData[2022], borderColor: BL, backgroundColor: 'rgba(26,61,110,0.06)', fill: true, tension: 0.35, pointRadius: 3, pointBackgroundColor: BL }]
    },
    options: {
      ...baseOpts,
      scales: {
        y: { ticks: { ...tk, callback: (v) => `${(v / 1000).toFixed(1)}mi` }, grid: { color: GRID }, border: { display: false } },
        x: { ticks: { ...tk, autoSkip: false, maxRotation: 0 }, grid: { display: false }, border: { display: false } }
      }
    }
  });

  window.swMes = (yr, el) => {
    document.querySelectorAll('#tabsMes .tab').forEach((t) => t.classList.remove('active'));
    el.classList.add('active');
    mensalChart.data.datasets[0].data = mensalData[yr];
    mensalChart.data.datasets[0].borderColor = mensalColors[yr];
    mensalChart.data.datasets[0].backgroundColor = mensalColors[yr] + '0f';
    mensalChart.data.datasets[0].pointBackgroundColor = mensalColors[yr];
    mensalChart.update();
  };

  // Cars
  const carData = {
    2022: { l: ['Gol', 'Virtus', 'Nivus', 'Amarok', 'Saveiro', 'Tiguan', 'T-Cross', 'Taos', 'Polo'], d: [42, 40, 40, 33, 31, 31, 30, 27, 26] },
    2023: { l: ['Amarok', 'Virtus', 'Gol', 'Taos', 'Nivus', 'T-Cross', 'Saveiro', 'Polo', 'Tiguan'], d: [52, 38, 36, 33, 29, 29, 29, 28, 26] },
    2024: { l: ['Amarok', 'Taos', 'Saveiro', 'T-Cross', 'Polo', 'Tiguan', 'Virtus', 'Gol', 'Nivus'], d: [43, 42, 39, 35, 35, 27, 27, 26, 26] }
  };
  const carColors = { 2022: BL, 2023: GO, 2024: RE };

  carChart = new Chart('cCarros', {
    type: 'bar',
    indexAxis: 'y',
    data: { labels: carData[2022].l, datasets: [{ data: carData[2022].d, backgroundColor: BL, borderRadius: 3 }] },
    options: {
      ...baseOpts,
      scales: {
        x: { ticks: tk, grid: { color: GRID }, border: { display: false } },
        y: { ticks: tk, grid: { display: false }, border: { display: false } }
      }
    }
  });

  window.swCar = (yr, el) => {
    document.querySelectorAll('#tabsCar .tab').forEach((t) => t.classList.remove('active'));
    el.classList.add('active');
    carChart.data.labels = carData[yr].l;
    carChart.data.datasets[0].data = carData[yr].d;
    carChart.data.datasets[0].backgroundColor = carColors[yr];
    carChart.update();
  };

  // Vendors
  const vendData = {
    2022: { l: ['Beatriz Rocha', 'Carlos Silva', 'Fernanda Gomes', 'Ana Costa', 'Mariana Souza'], d: [5792, 5354, 4800, 4444, 4270] },
    2023: { l: ['Lucas Almeida', 'Carlos Silva', 'Camila Araújo', 'Mariana Souza', 'Beatriz Rocha'], d: [5237, 5095, 4913, 4775, 4512] },
    2024: { l: ['Camila Araújo', 'Carlos Silva', 'Fernanda Gomes', 'Ana Costa', 'Lucas Almeida'], d: [6484, 6142, 5415, 5259, 4900] }
  };
  const vendColors = { 2022: BL, 2023: GO, 2024: RE };

  vendChart = new Chart('cVend', {
    type: 'bar',
    indexAxis: 'y',
    data: { labels: vendData[2022].l, datasets: [{ data: vendData[2022].d, backgroundColor: BL, borderRadius: 3 }] },
    options: {
      ...baseOpts,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: (v) => `R$ ${(v.raw / 1000).toFixed(0)} mil` } }
      },
      scales: {
        x: { ticks: { ...tk, callback: (v) => `${(v / 1000).toFixed(0)}k` }, grid: { color: GRID }, border: { display: false } },
        y: { ticks: tk, grid: { display: false }, border: { display: false } }
      }
    }
  });

  window.swVend = (yr, el) => {
    document.querySelectorAll('#tabsVend .tab').forEach((t) => t.classList.remove('active'));
    el.classList.add('active');
    vendChart.data.labels = vendData[yr].l;
    vendChart.data.datasets[0].data = vendData[yr].d;
    vendChart.data.datasets[0].backgroundColor = vendColors[yr];
    vendChart.update();
  };

  // Branches
  filialChart = new Chart('cFilial', {
    type: 'bar',
    data: {
      labels: ['Parnamirim RN', 'Feira Santana BA', 'Juazeiro CE', 'Fortaleza CE', 'Salvador BA'],
      datasets: [{ data: [2685, 2517, 2282, 2256, 2243], backgroundColor: [BL, GR, GO, RE, PU], borderRadius: 4, borderSkipped: false }]
    },
    options: {
      ...baseOpts,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: (v) => `R$ ${(v.raw / 1000).toFixed(2)} mi` } }
      },
      scales: {
        y: { ticks: { ...tk, callback: (v) => `${(v / 1000).toFixed(1)}mi` }, grid: { color: GRID }, border: { display: false } },
        x: { ticks: tk, grid: { display: false }, border: { display: false } }
      }
    }
  });

  // Slides
  let current = 0;
  const slides = document.querySelectorAll('.slide');
  const total = slides.length;
  const slideTotal = document.getElementById('sTotal');
  const dotsEl = document.getElementById('sDots');
  const btnPrev = document.getElementById('bPrev');
  const btnNext = document.getElementById('bNext');

  if (slideTotal) {
    slideTotal.textContent = total;
  }

  if (dotsEl) {
    slides.forEach((_, i) => {
      const d = document.createElement('div');
      d.className = 'dot' + (i === 0 ? ' active' : '');
      d.onclick = () => goTo(i);
      dotsEl.appendChild(d);
    });
  }

  function goTo(n) {
    slides[current].classList.remove('active');
    document.querySelectorAll('.dot')[current].classList.remove('active');
    current = n;
    slides[current].classList.add('active');
    document.querySelectorAll('.dot')[current].classList.add('active');
    document.getElementById('sNum').textContent = current + 1;
    if (btnPrev) btnPrev.disabled = current === 0;
    if (btnNext) btnNext.disabled = current === total - 1;
  }

  window.go = (dir) => {
    goTo(Math.max(0, Math.min(total - 1, current + dir)));
  };

  if (btnPrev) {
    btnPrev.disabled = true;
  }

  keyHandler = (e) => {
    if (e.key === 'ArrowRight') window.go(1);
    if (e.key === 'ArrowLeft') window.go(-1);
  };

  document.addEventListener('keydown', keyHandler);
});

onBeforeUnmount(() => {
  document.removeEventListener('keydown', keyHandler);

  if (anualChart) anualChart.destroy();
  if (mensalChart) mensalChart.destroy();
  if (carChart) carChart.destroy();
  if (vendChart) vendChart.destroy();
  if (filialChart) filialChart.destroy();

  delete window.swMes;
  delete window.swCar;
  delete window.swVend;
  delete window.go;
});
</script>

<template>
  <div v-html="props.html"></div>
</template>
