# Etapa de construcción
FROM node:20-alpine as build

# Establecer el directorio de trabajo
WORKDIR /usr/src/app

# Copiar archivos necesarios
COPY package.json .
COPY yarn.lock .
COPY tsconfig.json .

# Instalar dependencias
RUN yarn install --frozen-lockfile

# Copiar los archivos de la aplicación
COPY .env .
COPY config ./config
COPY database ./database
COPY src ./src
COPY public ./public

# Compilar el proyecto
RUN yarn build

# Etapa de producción
FROM node:20-alpine as production

# Establecer el directorio de trabajo
WORKDIR /usr/src/app

# Copiar los archivos necesarios de la etapa de construcción
COPY --from=build /usr/src/app/package.json .
COPY --from=build /usr/src/app/yarn.lock .
COPY --from=build /usr/src/app/tsconfig.json .
COPY --from=build /usr/src/app/.env .
COPY --from=build /usr/src/app/config ./config
COPY --from=build /usr/src/app/database ./database
COPY --from=build /usr/src/app/src ./src
COPY --from=build /usr/src/app/public ./public
COPY --from=build /usr/src/app/node_modules ./node_modules

# Exponer el puerto que utiliza Strapi
EXPOSE 1337

# Comando para iniciar la aplicación
CMD ["yarn", "start"]
