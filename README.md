# moodlog

Um diário de humor minimalista feito com Vue 3.

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?style=flat-square&logo=vue.js)
![Vite](https://img.shields.io/badge/Vite-5-646cff?style=flat-square&logo=vite)

## sobre

Registre como você está se sentindo a cada dia com uma nota opcional. Os dados ficam salvos no navegador, sem backend, sem conta.

## funcionalidades

- Registrar humor diário com 5 níveis
- Adicionar uma nota curta
- Ver histórico de entradas
- Contador de dias seguidos
- Humor mais frequente
- Dados persistidos com localStorage

## tecnologias

- Vue 3 (Composition API)
- Vite
- localStorage (sem backend)

## como rodar

```bash
npm install
npm run dev
```

## estrutura

```
src/
├── App.vue                 # lógica principal
├── style.css               # variáveis e reset global
└── components/
    ├── MoodPicker.vue      # seletor de humor
    └── EntryCard.vue       # card de cada entrada
```