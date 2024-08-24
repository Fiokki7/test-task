# test-task

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

### JSON Server Setup
  This project uses JSON Server to serve mock API data.
  
### Install JSON Server
```sh
npm install -g json-server
```

### Setup and Run JSON Server
Create a db.json file in the root of your project (if it doesn't exist) with your mock data. An example structure:
```sh
{
  "questions": [
    {
      "id": 1,
      "question": "Что такое операционная система?",
      "answers": [
        { "text": "Это просто программа на компьютере, как и другие - Word или Chrome", "correct": false },
        { "text": "Это показатель того, какой процессор используется на компьютере. Например, 32-битный или 64-битный", "correct": false },
        { "text": "Это набор взаимосвязанных программ, осуществляющих управление компьютером и взаимодействие с пользователем", "correct": true },
        { "text": "Нет такого понятия, есть понятие 'файловая система'", "correct": false }
      ]
    }
    // Other questions...
  ]
}
```

### Run the JSON Server
```sh
json-server --watch db.json --port 3000
```
This will start the server and you can access the data at http://localhost:3000/questions.

### Accessing JSON Data in Vue.js
In your Vue components, you can fetch the data using axios or fetch:
```sh
import axios from 'axios';

axios.get('http://localhost:3000/questions')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('There was an error!', error);
  });
```

### JSON Server API Reference
To get all questions:
```sh 
GET /questions
```
To get a specific question:
```sh
GET /questions/{id}
```
To add a new question:
```sh
POST /questions
```
To update a question:
```sh
PUT /questions/{id}
```
To delete a question:
```sh
DELETE /questions/{id}
```
