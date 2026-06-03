# Etapa 1: Compilar la aplicación React (Vite)
FROM node:18-alpine AS builder
WORKDIR /app
# Copiamos primero los archivos de dependencias
COPY package.json package-lock.json ./
# Instalamos las dependencias
RUN npm install
# Copiamos todo el resto del código
COPY . .
# Compilamos el proyecto (Vite crea la carpeta 'dist')
RUN npm run build

# Etapa 2: Servir la aplicación con Nginx
FROM nginx:alpine
# Copiamos el resultado de la etapa anterior a la carpeta de Nginx
COPY --from=builder /app/dist /usr/share/nginx/html
# Exponemos el puerto 80
EXPOSE 80
# Arrancamos Nginx
CMD ["nginx", "-g", "daemon off;"]