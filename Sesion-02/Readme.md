[`Programaci贸n con JavaScript`](../Readme.md) > `Sesi贸n 06`

---

# Sesi贸n 6: Webpack

馃幆 **Objetivos:**

Procesar aplicaciones modernas de JavaScript con Webpack para producir uno o m谩s bundles

---

## 馃捇 Tabla de Contenidos

- **[ES6 Modules](#es6-modules)**

- **[Webpack](#webpack)**

    - [Entry](#entry)
    
    - [Output](#output)
    
    - [Ejemplo 1: Instalaci贸n y configuraci贸n](./Ejemplo-01/Readme.md)
    
    - [Loaders](#loaders)
    
    - [Plugins](#plugins)
    
    - [Ejemplo 2: Webpack y CSS](./Ejemplo-02/Readme.md)
    
    - [Ejemplo 3: Webpack Dev Server](./Ejemplo-03/Readme.md)
    
    - [Reto 1: Find your GIF](./Reto-01/Readme.md)
    
    - [Reto 2: Find your GIF II](./Reto-02/Readme.md)

- **[Postwork](./Postwork/Readme.md)**

---

## ES6 Modules

En JavaScript, un m贸dulo es una unidad independiente de c贸digo que puede ser reutilizado. Los m贸dulos permiten exponer
ciertas partes del c贸digo que ser谩n usadas por otros m贸dulos. Esto nos da la flexibilidad suficiente para mantener 
nuestro c贸digo mejor organizado en m煤ltiples scripts. Dada la siguiente estructura:

```
app/
  |- app.js
  |- helpers.js
```

En `helpers.js` podemos tener algunas funciones que podr谩n ser usadas en otras partes del c贸digo:

```javascript
const sum = (a, b) => a + b;

const multiply = (a, b) => a * b;
```

Podemos exportar cada m贸dulo de manera independiente usando el keyword `export`.

```javascript
export const sum = (a, b) => a + b;

export const multiply = (a, b) => a * b;
```

O bien en una sola sentencia.

```javascript
const sum = (a, b) => a + b;

const multiply = (a, b) => a * b;

export { sum, multiply }
```

Para usar estas funciones en `app.js` usamos el keyword `import` junto con `from` para definir la ruta del archivo.

```javascript
import { sum, multiply } from './helpers.js'

console.log(sum(3, 2)); // 5

console.log(multiply(3, 2)); // 6
```

---

## Webpack

![Webpack](./assets/webpack.png)

Webpack es una herramienta muy usada en el desarrollo de aplicaciones en JavaScript modernas. Despu茅s de procesar la
aplicaci贸n, webpack genera internamente un grafo de dependencias lo que le permite generar uno o m谩s _bundles_. Este
_bundle_ contiene el c贸digo optimizado de todos los m贸dulos y dependencias de tu aplicaci贸n.

Algunos navegadores no soportan la nueva sintaxis de ES6 como `import` y `export` para el uso de m贸dulos. Detr谩s de 
escenas, Webpack transpila el c贸digo para que el uso de m贸dulos sea soportado en todos los navegadores. Es importante
tomar en cuenta que Webpack solo modifica sentencias `import` y `export`, para hacer uso de cualquier otra caracter铆stica
de ES6 es necesario usar un transpilador como Babel.

### Entry

El punto de entrada le dice a webpack cu谩l m贸dulo debe ser usado para comenzar a crear el grafo de dependencias. Webpack
se encargar谩 de buscar todos los m贸dulos y librerias que utiliza dicho punto de entrada. El valor por default es 
`./src/index.js`, pero podemos especificar una ruta diferente.

```javascript
module.exports = {
  entry: './path/to/entre/file.js'
}
```

### Output

La salida especifica d贸nde deben colocarse los bundles resultantes y c贸mo se deben llamar los archivos. Por default el
bundle principal es `./dist/main.js`. Para cambiar este valor agregamos una propiedad `output` cuyo valor es un objeto
que nos permite definir la ruta y el nombre del archivo.

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/entre/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
}
```

A diferencia del `entry`, cuando estamos definiendo la ruta del `output` necesitamos una ruta absoluta, por ello usamos
`path` que es un m贸dulo de Node.js. El m茅todo `path.resolve()` genera una ruta con los argumentos que le pasamos, 
`__dirname` es una variable cuyo valor es la ruta absoluta, `dist` es la carpeta donde queremos colocar el bundle final.

#### 馃暤 [Ejemplo 1: Instalaci贸n y configuraci贸n](./Ejemplo-01/Readme.md)

### Loaders

Webpack solamente entiende archivos de JavaScript y JSON. Usando `loaders` webpack puede procesar otro tipo de archivos
y convertirlos en m贸dulos que ser谩n agregados al grafo de dependencias.

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/entre/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ] 
  }
}
```

Cada `loader` tiene dos propiedades, la primera es `test`, aqu铆 definimos con una expresi贸n regular el tipo de archivos 
que deseamos procesar, con la segunda propiedad `use` indicamos el tipo de `loader` que debe ser usado. En el ejemplo
anterior estamos usando `raw-loader` para procesar todos los archivos `txt` del proyecto.

> Los loaders deben ser instalados previamente: `npm install --save-dev raw-loader`

### Plugins

Los `loaders` nos ayudan a transformar ciertos tipos de m贸dulos, mientras que los plugins permiten una amplia gama de
tareas como optimizaci贸n de los bundles, manejo de assets, inyecci贸n de variables de entorno, etc. Para usar un plugin
debemos importarlo y agregarlo al arreglo `plugins`.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: './path/to/entre/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' }
    ] 
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
}
```

La mayor铆a de los plugins pueden ser configurados. En este ejemplo, `html-webpack-plugin` genera un archivo html listo
para ser usado inyectando autom谩ticamente los bundles generados por webpack.

> Los plugins deben ser instalados previamente: `npm install --save-dev html-webpack-plugin`

#### 馃暤 [Ejemplo 2: Webpack y CSS](./Ejemplo-02/Readme.md)

#### 馃暤 [Ejemplo 3: Webpack Dev Server](./Ejemplo-03/Readme.md)

#### 馃捇 [Reto 1: Find your GIF](./Reto-01/Readme.md)

#### 馃捇 [Reto 2: Find your GIF II](./Reto-02/Readme.md)

#### 馃洝 [Postwork](./Postwork/Readme.md)
