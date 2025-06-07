# Contenedor Fronted.
## 1. Tiempo de duración:
El tiempo que me tomo en realizar es de 2 horas.
## 2. Fundamentos:
Este proyecto se basa en los principios de desarrollo de aplicaciones web distribuidas en contenedores, siguiendo el modelo cliente-servidor. Se implementa una arquitectura de microservicios sencilla, donde el frontend (React) se comunica con un backend (Node.js/Express) a través de una API REST. Ambos servicios son contenerizados usando Docker y gestionados con Docker Compose, lo que facilita su despliegue, aislamiento y escalabilidad.
## 3. Conocimientos previos:
- Conocimientos básicos de JavaScript.
- Uso de React.js para construir interfaces web.
- Fundamentos de Node.js y Express para APIs REST.
- Conocimientos básicos de Docker y docker-compose.
- Uso de la terminal o consola de comandos.
## 4. Objetivo a alcanzar: 
Construir y contenerizar una aplicación web compuesta por dos servicios: un frontend en React y una API backend en Express. La aplicación debe mostrar una tabla con datos de una entidad específica del proyecto de titulación, accediendo a dicha información desde la API a través de una red de Docker.
## 5. Equipo necesario:
- Computadora con Windows, Linux o macOS.
- Acceso a Internet.
- Software instalado:
Node.js
Docker Desktop
Visual Studio Code (u otro editor de código)
## 6. Material de apoyo:
- Documentación oficial de React: https://reactjs.org/
- Documentación de Express.js: https://expressjs.com/
- Guía oficial de Docker: https://docs.docker.com/
- Tutoriales sobre Docker Compose: https://docs.docker.com/compose/
## 7. Procedimiento:
### Paso 1: Crear la carpeta base del proyecto.
```
mkdir cliente-app
````
### Evidencia:
<imag!![1](https://github.com/user-attachments/assets/6ea8d246-8794-44b6-bfd1-a77930cf9834)

### Paso 2: Crear el Backend (API con Node.js y Express)
```
cd cliente-app
````
### Evidencia:
<imag!![2 1](https://github.com/user-attachments/assets/ac40352f-0352-4bfc-b691-e7d51fd00f6e)

```
mkdir backend
````
### Evidencia:
<imag!![2 2](https://github.com/user-attachments/assets/0bdad7d7-87d7-4925-9daf-02c0c237c90c)

```
cd backend
````
### Evidencia:
<imag!![2 3](https://github.com/user-attachments/assets/c5431f81-06c2-4f92-828a-9e08f6205399)

```
npm init -y
````
### Evidencia:
<imag!![2 4](https://github.com/user-attachments/assets/fb253b7d-b028-4119-81df-e2377f86aa32)

```
npm install express cors
````
### Evidencia:
<imag!![2 5](https://github.com/user-attachments/assets/74fc323d-f17f-4aa7-b078-6e27a619fded)

### Paso 3: Escribir la API en server.js
```
const express = require('express');
const cors = require('cors');
const app = express();
const PORT = 4000;

app.use(cors());

const clientes = [
  { id: 1, nombre: 'Juan', correo: 'juan@example.com' },
  { id: 2, nombre: 'Ana', correo: 'ana@example.com' },
];

app.get('/api/clientes', (req, res) => {
  res.json(clientes);
});

app.listen(PORT, () => {
  console.log(`Servidor API corriendo en http://localhost:${PORT}`);
});
````
### Evidencia:
<imag!![3](https://github.com/user-attachments/assets/b500798e-6a28-4e48-8a06-1badbe3ae033)

### Paso 4: Agregar script al package.json
```
"scripts": {
  "start": "node server.js"
}
````
### Evidencia:
<imag!![5](https://github.com/user-attachments/assets/9c9b7465-6281-4da9-9ce6-223047a0cd05)

### Paso 5: Crear el Frontend (React)
```
cd ..
````
```
npx create-react-app frontend
````
### Evidencia:
<imag!![5 1](https://github.com/user-attachments/assets/4342e4b6-da80-435a-b810-c4f1a897af7f)

### Paso 6: Editar el archivo App.js para mostrar la tabla de clientes
```
import React, { useEffect, useState } from 'react';

function App() {
  const [clientes, setClientes] = useState([]);

  useEffect(() => {
    fetch('http://backend:4000/api/clientes')
      .then(res => res.json())
      .then(data => setClientes(data))
      .catch(err => console.error(err));
  }, []);

  return (
    <div style={{ padding: '2rem' }}>
      <h1>Lista de Clientes</h1>
      <table border="1" cellPadding="10">
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Correo</th>
          </tr>
        </thead>
        <tbody>
          {clientes.map(cliente => (
            <tr key={cliente.id}>
              <td>{cliente.id}</td>
              <td>{cliente.nombre}</td>
              <td>{cliente.correo}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default App;
````
### Evidencia:
<imag!![7](https://github.com/user-attachments/assets/cdee518d-9545-4124-a0a3-275d0884df36)

### Paso 7: Crear Dockerfiles en backend/Dockerfile:
```
# backend/Dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 4000
CMD ["npm", "start"]
````
### Evidencia:
<imag!![backend](https://github.com/user-attachments/assets/79567e2d-a722-4e18-9043-ab8631851eb8)

### Paso 8: Frontend (frontend/Dockerfile)
```
# frontend/Dockerfile
FROM node:18 as build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
````
### Evidencia:
<imag!![8](https://github.com/user-attachments/assets/b6637ee1-ea4e-42a3-8151-75197d9954b3)

### Paso 9: Crear docker-compose.yml:
```
version: '3.8'

services:
  backend:
    build: ./backend
    ports:
      - "4000:4000"
    networks:
      - red-cliente

  frontend:
    build: ./frontend
    ports:
      - "3000:80"
    depends_on:
      - backend
    networks:
      - red-cliente

networks:
  red-cliente:
````
### Evidencia:
<imag!![9](https://github.com/user-attachments/assets/b1d62f60-1299-4b70-8f0c-08047fcd95d7)

### Paso 10: Agregar script al package.json
```
docker-compose up --build
````
### Evidencia:
<imag!![10](https://github.com/user-attachments/assets/f74d108f-b0e9-4dcb-b5d8-32aec77631b2)

## 8. Resultado esperado:
### Evidencia:
<imag!![resultado 1](https://github.com/user-attachments/assets/fb2528ab-5ca3-4673-957e-e0bb38002f43)

### Evidencia:
<imag!![resultado 2](https://github.com/user-attachments/assets/2186867d-11ce-4e7d-bbbd-3f09f46e2915)

<imag!![resultado 2 1](https://github.com/user-attachments/assets/62f84a93-196a-461e-a02e-ee625978e54e)

## 9. Conclusión:
Este ejercicio permite comprender cómo se integran tecnologías modernas para el desarrollo de aplicaciones distribuidas. Usar Docker para contenerizar y comunicar componentes frontend y backend mejora el despliegue y la portabilidad del sistema. Además, refuerza conceptos de arquitectura modular, consumo de APIs y trabajo con React como tecnología de interfaz.
## 10. Bibliografía
- Docker Docs. “Get started with Docker.” https://docs.docker.com/get-started/
- React Documentation. https://reactjs.org/docs/getting-started.html
- Express.js Guide. https://expressjs.com/en/starter/installing.html
- MDN Web Docs. “Fetch API.” https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
