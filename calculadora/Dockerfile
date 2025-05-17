# Etapa de construcción
FROM node:20.17.0-alpine AS builder

# Crear directorio de la aplicación
WORKDIR /app

# Copiar archivos necesarios para instalar dependencias
COPY package*.json ./
COPY tsconfig*.json ./

# Instalar dependencias
RUN npm ci

# Copiar todo el código fuente
COPY . .

# Construir la aplicación
RUN npm run build

# Etapa de producción
FROM node:20.17.0-alpine

# Crear directorio de la aplicación
WORKDIR /app

# Copiar solo lo necesario desde la etapa de construcción
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

# Variables de entorno
ENV NODE_ENV production
ENV PORT 3000

# Exponer el puerto
EXPOSE 3000

# Comando para ejecutar la aplicación
CMD ["node", "dist/main"]
