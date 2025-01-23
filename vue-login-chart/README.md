# Technical Test - Vue Dólar Dashboard

Este proyecto es parte de un test técnico para la empresa **ZensiAI**. La aplicación muestra un gráfico interactivo del valor histórico del dólar anual, utilizando datos obtenidos de una API externa. Está construida con **Vue 3**, **TypeScript**, y utiliza **ApexCharts** para la visualización del gráfico.

## Repositorios

- **Repositorio original**: [ZensiAI/technical_test](https://github.com/ZensiAI/technical_test)
- **Fork**: [nvargas-mus/technical_test](https://github.com/nvargas-mus/technical_test)

Este repositorio es una versión personalizada de la aplicación, adaptada y mejorada según los requerimientos del test.

## Requisitos

Antes de ejecutar el proyecto, asegúrate de tener instalados los siguientes programas:

- [Node.js](https://nodejs.org/) (v16 o superior recomendado)
- [npm](https://www.npmjs.com/) (v7 o superior)

## Instalación

### 1. Clona este repositorio en tu máquina local:

- Abre una terminal en Visual Studio Code o tu editor de preferencia.
- Clona el repositorio desde GitHub ejecutando el siguiente comando:

   ```bash
   git clone https://github.com/nvargas-mus/technical_test.git
   ```

- Verifica que todo se clonó correctamente, usando el comando:
  ```bash
  git status
  ```

### 2. Abre la terminal y agrega el siguiente comando:

- Estos comandos abriran las carpetas para poder entrar al proyecto:

   ```
   cd technical_test
   cd vue-login-chart
   ```

### 3. Instalar las dependencias

- En la terminal, instala las dependencias de Node.js utilizando:

  ```bash
  npm install
  ```

- Si al instalar genera un reporte de vulnerabilidades, intenta solucionarlo automáticamente ejecutando:

  ``` bash
  npm audit fix
  ```

  ### 4. Instalar Bootstrap

- Para incluir Bootstrap en el proyecto, ejecuta el siguiente comando en la terminal:

  ```bash
  npm install bootstrap
  ``` 

### 4. Pasos posteriores a la instalación

Después de ejecutar los comandos y asegurarte de que todas las dependencias están correctamente instaladas, sigue los siguientes pasos para asegurarte de que todo esté listo y funcionando:

- Verificar las dependencias instaladas: Asegúrate de que todas las dependencias necesarias están instaladas correctamente ejecutando:

```bash
npm list
```

- Este comando mostrará una lista de las dependencias instaladas en el proyecto. Si no ves errores o advertencias significativas, significa que todo está listo para el siguiente paso.

### 5. Traer los últimos cambios

- Para asegurarte de que estás trabajando con la versión más reciente, sigue estos pasos:

#### 1. Verificar la rama actual

- Asegúrate de estar en la rama correcta utilizando el siguiente comando:

  ```bash
  git branch
  ```

- Esto mostrará una lista de las ramas existentes y marcará con un asterisco (\*) la rama en la que estás actualmente.

#### 2. Cambiar de rama (si es necesario)

- Para cambiar de rama, usa el comando:
  ```bash
  git checkout nombre-de-la-rama
  ```

#### 3. Traer los últimos cambios

- Una vez que estés en la rama correcta, utiliza el siguiente comando para traer los últimos cambios del repositorio remoto:
  ```bash
  git pull origin nombre-de-la-rama
  ```


### 6. Ejecutar el proyecto

- Una vez que las dependencias están instaladas, puedes ejecutar el proyecto para asegurarte de que todo funcione correctamente. Usa el siguiente comando para iniciar el servidor de desarrollo:

``` bash
npm run dev
```
- Este comando iniciará el servidor local de desarrollo utilizando Vite. Por defecto, la aplicación abrirá en **http://localhost:5173/** Abre este enlace en tu navegador para ver si la aplicación está funcionando correctamente.

### 8. Pruebas de funcionalidad

- Si la aplicación se está ejecutando correctamente, realiza algunas pruebas para verificar que todo funcione como se espera:
   - Verifica que la autenticación y la sesión de usuario funcionen como se espera (**Username: emilys, contraseña: emilyspass**)
	- Verifica que el gráfico de datos del dólar se carga correctamente.
	

### 9. ESLint

Es importante mantener el código limpio y estandarizado. En este proyecto se utiliza **ESLint** para garantizar que el código siga las mejores prácticas y evitar errores de estilo.

#### ¿Cómo funciona ESLint?

ESLint analiza el código y busca posibles errores o inconsistencias con las reglas definidas en el archivo de configuración. Si encuentra algún problema, mostrará un mensaje de advertencia o error en la consola. Además, puedes configurar **Prettier** para que el código siga un formato consistente.

#### Comando para ejecutar el linter

Para ejecutar ESLint y verificar el código, utiliza el siguiente comando:

```bash
npm run lint
```

Este comando ejecutará el linter en el proyecto y mostrará cualquier advertencia o error de estilo que se deba corregir. Si deseas corregir automáticamente algunos de los problemas de estilo, puedes añadir la opción --fix al comando:

```bash
npm run lint -- --fix
```

Esto intentará corregir automáticamente los problemas que pueden ser arreglados sin intervención manual.
