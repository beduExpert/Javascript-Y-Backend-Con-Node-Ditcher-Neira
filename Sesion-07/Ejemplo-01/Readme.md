# Ejercicio 1

## 馃懏 !Advertencia! Es muy importante que para esta actividad tengas un alto grado de concentraci贸n, por ello te hacemos las siguientes recomendaciones: evita lugares con distracciones, trata de esta lo m谩s c贸modo posible, elimina las distracciones de tu ordenador, pon en modo silencio tu celular, ya que esta clase requiere de un alto nivel de atenci贸n para su entendimiento. 

<br>
<br>
<br>
<br>
<br>

## Objetivo

Comprender el modelo as铆ncrono y no bloqueante de Javascript y la importancia de los callbacks en la asincronia.

## Requerimientos

Instalaci贸n de Node 

## Desarrollo

### 驴Qu茅 es un callback?

En Javascript un **Callback** , es aquella funci贸n que se envia como par谩metro a otra funci贸n.

![Alt Text](https://i.pinimg.com/originals/1e/02/8d/1e028db0ea4defc218cb913825174b53.gif)


Vamos a entender esto por partes, una funci贸n es una estructura en c贸digo que permite un envio, una entrada y una salida de datos.


### Veamos primero el proceso de entrada y salida de datos:

En javascript una funci贸n se declara de la manera siguiente : 

![img/funcion.png](img/funcion.png)


Cuando decimos que una funci贸n tiene entradas y salidas, hablamos de que cuando le envio cosas, la funcion recibira esa informaci贸n a traves de par谩metros.


![img/parametros.png](img/parametros.png)


Estos parametros pueden ser utilizados para realizar acciones dentro de la funci贸n.


![img/return.png](img/return.png)

y despues puede retornar algo, es decir, regresar informaci贸n del proceso que se ha enviado, pero tambien tiene la opci贸n de no regresar nada. 


En resumen, los datos entran a la funci贸n a traves de los parametros, dentro de la funci贸n podemos hacer cualquier cosa que querramos con esos datos y podriamos regresar un resultado o no.



### Ahora veamos la parte de envio de datos.


Para encender o accionar una funci贸n necesitamos usar el nombre de la funcion junto con un par de par茅ntesis.

![img/enviar.png](img/enviar.png)

en los par茅ntesis podemos enviar informaci贸n a la funci贸n, **SEPARADOS POR COMAS**, o podemos dejarlo vac铆o.

Eso quiere decir que el ciclo de vida de la funcion es esl siguiente : 

![img/ciclo_de_vida.png](img/ciclo_de_vida.png)


1. Mando llamar la funcion y envio par谩metros
2. La funcion recibe y guarda lo que el llamado envio en variables(a,b)
3. Realizo una operaci贸n o cualquier cosa con los datos que llegaron dentro de la funci贸n
4. Guardo los datos en una variable y los preparo para regresarlos.
5. Regreso la informaci贸n al llamado.

Hasta este momento todo lo que hemos entendido es el ciclo de vida de la funci贸n, ahora veremos como funcionan los callbacks.

En la misma funci贸n enviare un tercer par谩metro, pero este, sera otra funci贸n :

![img/callback1.png](img/callback1.png)

Cuando envio una funci贸n como par谩metro debe ser anonima, cuando hago esto, la funci贸n que recibe el par谩metro con la funci贸n anonima, puede mandarla llamar desde adentro y hacer algo totalmente diferente.   

Eso quiere decir que si sigo con las reglas de las funciones, entonces para mandar llamar la funci贸n anonima, tengo que usar el nombre del par谩metro  + par茅ntesis.

![img/callback2.png](img/callback2.png)

Entonces ya puedo hacer uso de la funci贸n anonima dentro de mi funci贸n holamundo.

Puedo accionar la funcion callback en cualquier parte del proceso de mi funci贸n holamundo, como si fuera una funci贸n com煤n y corriente. 


![img/callback3.png](img/callback3.png)



### 驴Por qu茅 necesitamos callbacks?

La asincronia de javascrit y node.js tiene ventajas sobre sus competidores, por que realiza las acciones o tareas al mismo tiempo, pero al hacer esto, perdemos el control de la espera en una ejecuci贸n.

Tal ves queremos que javascript se espere a que termine un proceso para poder continuar con el otro.

Para no romper el paradigma de la asincronia, utilizamos los callbacks y le decimos a javascript que espere...



Veamos, el siguiente ejemplo:

```jsx
function primero() {
  console.log("Soy el 1");
}

function segundo() {
  console.log("Soy el 2");
}

function tercero() {
  console.log("Soy el 3");
}

primero();
segundo();
tercero();
```
RESUTADO

```bash
Soy el 1
Soy el 2
Soy el 3
```


Tenemos el resultado que esperabamos se ejecuta *primero*, luego *segundo* y por 煤ltimo *tercero*

Ahora, supongamos que *primero* es una funci贸n que hace una petici贸n *http* a una base de datos con mucha informc贸n y tenemos que esperar por la respuesta de la petici贸n, para simular esto, usaremos *setTimeout* adaptando el c贸digo anterior

```jsx
function primero() {
  //Simula petici贸n a un servidor con muchos datos
  setTimeout(function () {
    console.log("Soy el 1");
  }, 1000);
}

function segundo() {
  console.log("Soy el 2");
}

function tercero() {
  console.log("Soy el 3");
}

primero();
segundo();
tercero();
```

RESULTADO

```bash
Soy el 2
Soy el 3
Soy el 1
```

Por ahora, no es necesario entender el funcionamiento de *setTimeout* s贸lo que en nuestro ejemplo simula una petici贸n a una API creando un retardo de *1 seg*. 

El resultado no est谩 en el orden en el mandamos a llamar a las funciones, lo que sucede es que Javascript no ha esperado a la respuesta *primero* para avanzar. En este ejemplo es importante esperar por la respuesta antes de avanzar en el c贸digo, ya que tendremos que saber si la petici贸n sucedi贸 con 茅xito o no, en caso de no hacerlo lo m谩s l贸gico ser铆a manejar el error.

Javascript utiliza un modelo **as铆ncrono y no bloqueante con un loop de eventos con un s贸lo hilo de ejecuci贸n.** Para que Javascript funcione de manera as铆ncrona existen los siguientes mecanismos que trataremos en esta sesi贸n:

- Callbacks
- Promises
- Async / await

### Callbacks

Como lo hemos dicho anteriormente, un callback no es m谩s que una funci贸n que pasa como argumento a otra funci贸n y es utilizado como un **modo para asegurar que cierto c贸digo no se ejecute hasta que otro c贸digo haya terminado de ejecutarse.**

### Callback Hell

Los callbacks tambi茅n pueden lanzar a su vez funciones as铆ncronas, lo que hace que pueda anidarse tanto como se desee

Podr铆amos tener un ejemplo c贸mo el siguiente:

```jsx
setTimeout(function () {
  console.log("Soy el 1");
  setTimeout(function () {
    console.log("Soy el 2");
    setTimeout(function () {
      console.log("Soy el 3");
      setTimeout(function () {
        console.log("Soy el 4");
        // Podr铆a a ver m谩s llamadas as铆ncronas
      }, 4000);
    }, 3000);
  }, 2000);
}, 1000);
```

```bash
Soy el 1
Soy el 2
Soy el 3
Soy el 4
```

Si se implementar谩 m谩s llamadas anidadas, sin duda tenemos una problem谩tica de identaci贸n, legibilidad, dificulta el mantenimiento, etc., a esto se le conoce como **Callback Hell** o tambi茅n **Pyramid of Doom**.

El siguiente post muestra este problema ya descrito y tambi茅n a c贸mo solucionarlo, es una gu铆a introductoria a lo que ser谩 el resto de la sesi贸n

[Callback Hell](http://callbackhell.com/)