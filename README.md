# SIG

Pagina institucional desenvolvida com Astro e componentes Vue para apresentar estrategia, indicadores e visao executiva em formato de dashboard.

## Visao Geral

- Nome do projeto: sig
- Stack principal: Astro 6 + Vue 3
- Visualizacao de dados: Chart.js
- Runtime Node: >= 22.12.0

## Como Rodar

1. Instale as dependencias:

```bash
npm install
```

2. Suba o ambiente de desenvolvimento:

```bash
npm run dev
```

3. Abra no navegador:

```text
http://localhost:4321/
```

Opcional para acessar de outro dispositivo na mesma rede:

```bash
npm run dev -- --host 0.0.0.0 --port 4321
```

## Scripts Disponiveis

| Script | Descricao |
| :-- | :-- |
| npm run dev | Inicia servidor local de desenvolvimento |
| npm run build | Gera build de producao em dist |
| npm run preview | Sobe a build de producao localmente |
| npm run astro | Executa comandos da CLI Astro |

## Estrutura do Projeto

```text
.
├── astro.config.mjs
├── index.legacy.html
├── public/
├── src/
│   ├── assets/
│   │   ├── astro.svg
│   │   └── background.svg
│   ├── components/
│   │   ├── DashboardSlides.vue
│   │   ├── KpiPanel.vue
│   │   ├── Welcome.astro
│   │   └── sections/
│   │       ├── BpmnSection.astro
│   │       ├── HeaderHero.astro
│   │       ├── SiteFooter.astro
│   │       └── StrategySection.astro
│   ├── layouts/
│   │   └── Layout.astro
│   ├── pages/
│   │   └── index.astro
│   └── styles/
│       └── site.css
├── package.json
└── tsconfig.json
```

## Arquitetura da Pagina

- Entrada principal em src/pages/index.astro
- Layout base em src/layouts/Layout.astro
- Secoes da landing em src/components/sections
- Componentes Vue para interatividade e KPIs em src/components
- Estilos globais em src/styles/site.css

## Build e Entrega

Para gerar artefatos de producao:

```bash
npm run build
```

Para validar a build localmente:

```bash
npm run preview
```
