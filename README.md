# jenkins-playground-app | Web Sample para Pipeline Jenkins

Este es un proyecto de ejemplo diseñado para demostrar un pipeline de CI/CD con Jenkins. Consiste en una página web simple que muestra un mensaje "Hola Mundo" con Bootstrap y Font Awesome.

## Características

- 🎨 Diseño responsive usando Bootstrap 5.3.2
- 📦 Sin dependencias de construcción local (usa CDN)
- ✨ Iconos de Font Awesome 6.5.1
- 🔍 Validación HTML incorporada
- 📅 Muestra la fecha y hora de actualización
- 🚀 Preparado para CI/CD

## Estructura del Proyecto

```
web-sample/
├── src/
│   └── index.html    # Página web principal
├── package.json      # Configuración del proyecto
└── Jenkinsfile      # Pipeline de CI/CD
```

## Scripts Disponibles

El proyecto utiliza npm como gestor de tareas. Los siguientes scripts están disponibles:

- `npm run clean`: Elimina la carpeta build/
  ```bash
  npm run clean
  ```

- `npm run lint`: Valida el HTML usando html-validate
  ```bash
  npm run lint
  ```

- `npm run mkdir`: Crea la carpeta build/
  ```bash
  npm run mkdir
  ```

- `npm run copy`: Copia los archivos HTML de src/ a build/
  ```bash
  npm run copy
  ```

- `npm run build`: Ejecuta la secuencia completa de construcción
  ```bash
  npm run build
  ```
  Este comando ejecuta las siguientes tareas en orden:
  1. Limpia la carpeta build/
  2. Valida el HTML
  3. Crea la carpeta build/
  4. Copia los archivos

## Configuración de HTML Validate

El proyecto usa html-validate con las siguientes reglas personalizadas:

```json
{
  "extends": ["html-validate:recommended"],
  "rules": {
    "element-required-attributes": "error",
    "no-trailing-whitespace": "error",
    "void-style": ["error", {"style": "selfclosing"}]
  }
}
```

## Dependencias de Desarrollo

- `copyfiles`: Para copiar archivos manteniendo la estructura
- `html-validate`: Linter HTML
- `mkdirp`: Creación recursiva de directorios
- `rimraf`: Eliminación recursiva de directorios

## Uso en Desarrollo Local

1. Instalar dependencias:
   ```bash
   npm install
   ```

2. Validar HTML:
   ```bash
   npm run lint
   ```

3. Construir el proyecto:
   ```bash
   npm run build
   ```

4. Servir localmente (opcional):
   ```bash
   npx serve build
   ```

## Integración con Jenkins

Este proyecto está diseñado para trabajar con un pipeline de Jenkins que:

1. Clona el repositorio
2. Instala dependencias
3. Ejecuta la validación HTML
4. Construye el proyecto
5. Despliega en un servidor web

## Recursos Externos

El proyecto utiliza los siguientes recursos vía CDN:

- [Bootstrap 5.3.2](https://getbootstrap.com/docs/5.3/getting-started/introduction/)
- [Font Awesome 6.5.1](https://fontawesome.com/v6/docs/)

