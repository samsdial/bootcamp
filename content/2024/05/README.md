Bootcamp para fullStack Developer - [Inicio](https://github.com/samsdial/bootcamp/)

![Imagen](../../assets/makup.jpg)

# Fundamentos de programación funcional

### ¿Cuando en más util la programación funcional?

Antes de explicar **qué** es la programación funcional, creo que es útil definir cuando la utilizamos.

La programación funcional es más útil cuando realizamos transformaciones de datos 1 a 1.

```javascript
// This is the data store
type UseMap = {[userId: number]: User};
// This is the funtional layer
type convertUseMapToArray: (userMap: UserMap) => User[];
// This is the presentation layer
type Component = ({ user: User[] }) => JSX.Element
```

En el código anterior tenemos algunos tipos enumerados para un almacén de datos, un componente y una capa funcional entre ellos.

Esta capa funcional `convertUserMapToArray` convierte los datos de un formato que tiene sentido para la tienda a un formato que tiene sentido para la interfaz de usuario.

### ¿Qué es la programación funcional?

**La programación funcional** (a menudo abreviada como FP) es el proceso de creación de software mediante la composición de **funciones puras.** Una **función pura** es una función que, dadas las mismas entradas, siempre devuelve la misma salida y no tiene efectos secundarios.

input => output

Por ejemplo, podemos confiar en que 2 + 2 siempre es 4 y 3 x 3 siempre es 9

-----------

### Efectos secundarios

La parte de que no haya efectos secundarios es particularmente importante, porque es lo que nos permite confiar en que una función siempre se comportará de la misma manera en cualquier entorno. Esto será de ayuda para conceptos futuros.

Ahora bien, los efectos secundarios no son intrísecamente malos, pero debes aislarlos en partes de tu código base donde puedes indentidicarlo fácilmente.

Veamos algunos ejemplos de efectos secundarios.

### Mutación

Modificando el argumento que se pasa en:

```javascript
// Mutates the given array
function pop(arr) {
    retun arr.splice(0, 1);
}

const arr = [1,2,3,4];
pop(arr);

console.log(arr); // [2, 3, 4]
```

En el ejemplo anterior, cambiamos el valor de `arr` la referencia en la que se encuentra. Como resultado, no podemos predecir qué devolverá esta función en cualquier momento. ¿Qué sucede cuando al `arr` se le agotan los valores?

### Estado compartido

Usando alguna forma de estado global

```javascript
let i = 0;
function increment() {
    return i++;
}
function decrement() {
    return i--;
}
```

En este ejemplo, no podemos predecir qué devolverán esta funciones porque dependen de algún valor externo. El Orden de las llamada a las funciones sí será importante.

Además, ¿qué sucede si alguien más cambia el valor de `i`? ¿Te gustaría buscar en Google que `string++` es?

### 

### Código asincrónico

Código que no se ejecuta inmediatamente

```javascript
let i = 0;
function incrementAsync(obj) {
    setTimeout(()=> {
        i++;
    }, 0)
}
incrementAsync();
```

Este merece una mención especial porque es una necesidad. Tenemos que hacer algunas cosas de forma asincrónica. Tenemos que accerder a las API  y optener datos.

Esto me lleva de nuevo al punto que comenté antes. Los efectos secundarios no son malos en sí mismo, pero deben aislarse adecuadamente para que el código sea más predecible.

### Ejemplo de tiempo

```javascript
// This is a pure funcition
function clone(obj) {
    return {...obj};
}


// This mutates the given object
function killParents(wizard) {
    wizard.parents = "Dead";
    return wizard;
}
```

```javascript
// This mutates the given object
function addScar(wizard) {
  wizard.scar = true;
}

const a = {name: "Harry Potter"};
const b = clone(a);
const c = killParents(b);
const d = addScar(c);
```

Mirando el código anterior, esperaríamos que produjera lo siguiente:

```javascript
console.log(a) // {name: "Harry Potter"};
console.log(b) // {name: "Harry Potter"};
console.log(c) // {name: "Harry Potter", parents: "Dead"};
console.log(d) // {name: "Harry Potter", scar: true, parents: "Dead"};
```

Lamentablemente, esto no es lo que obtenemos. Esto es:

```javascript
console.log(a) // {name: "Harry Potter"};
console.log(b) // {name: "Harry Potter", parents: "Dead"};
console.log(c) // {name: "Harry Potter", parents: "Dead"};
console.log(d) // undefined
```

#### ¿Que pasó?

Bueno, la primera función `clone` es una función pura y funciona como se esperaba. Produjo un nuevo objeto en una nueva referencia.

`killParents` no es una función pura. Muta el argumento dado y marca al padre como muerto. Sin emgargo, devuelve el objeto, por lo que _parece_  que estamos ovjeniendo una nueva copia.

`addScar` realmente no le importa. Muta el objeto original y luego no devuelve nada, por lo que `addScar(c)` devuelve `undefined` aunque también modifica `c`.

Como resultado, `a` está apuntando a la referencia original, `b` y `c` está apuntando a la copia clonada (con padres muertos y una cicatriz), y  `d` no está apuntando a nada.

------

### Declarativo e imperativo

El código declartivo descrive lo que hace

El código imperativo describe cómo lo hace

Si observamos los dos ejemplos de código anteriores, podemos ver un marcado contraste. El bloque superior está escrito como React y solo dice "queremos un contador en la página". Este códogo confía en React, la biblioteca decalrativa para hacerlo bien.

El bloque inferior utiliza JS estándar. Busca Explícitamente un nodo DOM y lo actualiza. Si bien esto está bien para un ejemplo tan simple, no se escalára bien, ¿Qué sucede cuando queremos múltiples contadores en múltimples ubicaciones? React está listo para eso, con JS estándar tenemos mucho trabajo por hacer.

Ahora buen, vale la pena señalar que el código declarativo siempre terminará compilándose o siendo procesado por algo imperativo. ¿Que quiero decir con eso? Bueno, _algo_ tien que ver con la mutación del DOM. En este caso, se trata de React. Incluso con lenguajes funcionales como Liso o Haskell, **_Eventualmente_** se compilan en código de máquina imperativo.

#### Ejemplo

##### Imperativo

```javascript
function getFileMapById(files) {
    const fileMap = {};
    for (let i=0; i<files.length; i++) {
        const file = file[i];
        fileMap[file.id] = file;
    }
    return fileMap;
}
```

##### Declarativo

```javascript
function getFileMapById(files) {
    return lodash.keyBy(files, 'id');
}
```

Ahora bien, estas dos funciones logran exactamente lo mismo: toman una lista de archivos `files` y devuelven un diccionario de archivo donde la clave es `file.id`

Pero el imperativo es más descuidado. Son 8 lineas de códogp en ligar de 3. También deja mucho  margen de error.

¿Qué sucede si un archivo no tiene un `id`?¿Qué sucede si nos equivocamos en la cláusula de salida? Y un dato curioso: hay formas más rápidas de hacer bucles for (son feos), pero podemos confiar en que lodash los haga en segundo plano.

----

### Conceptos funcionales

Ahora es el momento de pasar a los conceptos funcionales. Analicémoslos en pares: los dos promeros son **separación** y **composición**.

#### Separación

> Si intentas realizar efectos y lógica al mismo tiempo, puedes crear efectos secundarios ocultos que provquen errores en la lógica. Mantén las funciones pequeñas. Haz una cosa a la vez y hazla bien.

#### 

#### Composición

> Planifique la composición. Escriba funciones cuyas salidas funcionen naturalmente comom entradas para muchas otras funciones. Mantenga las firmas de las funciones lo más simple posible.

Saqué estas citas de un artículo llamado [El Tao de la inmutabilidad](./content/2024/06/README.md). Lo que estamos diciendo aquí es que queremos funciones pequeñas que se puedan encadenar fácilmente para formar funciones más grandes. Veamos un ejemplo:

```javascript
function sortFilesByName(files) {
    return lodash.sortBy(files, 'name');
}
```

```javascript
function getPdfFiles(files) {
    return lodash.filter(files, {extension: PDF});
}
```

```javascript
funtion getFileNames(files) {
    return lodash.map(files, 'name');
}
```

Estas tres funciones son muy sencillas. Toman un argumento y relizan una transformación sobre ese argumento. La primera función ordena la lista de archivos.

La segunda función es un término de FP `filter` que `filter` significa filtrar hacia adentro. en ligar de hacia afuera. Por lo tanto, este código devuelve una matriz de archivos cuyas extensiones son PDF.

La última función es simplemente un `map`.`map` es otro termino de FP, una transformación 1:1 de una lista a una nueva versión de esa lista. En este caso, estamos recorriendo la lista de archivos y devolviendo la clave `name` de cada uno de ellos.

Ahora podemos combinarlos juntos:



```javascript
const getSortedPDFFileNames = lodash.flow(
    getPdfFiles,
    getFileNames,
    lodash.sortBy
);
// alternative
const getSortedPdfFileNames = (files) => (
    lodash.sortBy(
        getFileNames(
            getPdfFiles(files)
        )
    )
);
```

Ahora bien, ambas funciones son equivalentes. Toman una lista de archivos, filtran los archivos PDF, obtiene sus nombres y devuelven una lista ordenada de nombres de archivos.

`lodash.flow` Tiene algunas optimizaciones, pero la segunda sintaxis puede resultar más legible.



Pasemos al siguiente conjunto de conceptos.



### Inmutabilidad

> la verdadera constante es el cambio. La mutación oculta el cambio. El cambio oculto manifiesta el caos. Por eso, los sabios abrazan la historia.

### Memorización

> La memorización es una tecnica de optimizacion utilizada principalmente para acelerar los programas informáticos almacenando los resultados de llamadas de funciones costosas y devolviendo el resultado en caché cuando se producen nuevamente las mismas entradas.

Estas combinaciones se combinan muy bien. La inmutabilidad indica que nunca vamos a mutar un argumento, solo delvolver uno nuevo, y la memorización nos permite recordar los resultados. Veamos un ejemplo de combinación de ambos.



```javascript
function killSibling(wizard) {
    return {
        ...wizard,
        numSibling: wizard.numSiblings - 1,
    };
}
const killSiblingMemoized = lodash.memorize(killSibling);
const ron = { name: "Ron Weasley", siblings: 6};
const ronAfterFredDies = KillSiblingMemorized(ron);
ron === ronAfterFredDies // false, he's a different person now.

const ronAfterFredDiesAgain = killSiblingMemoized(ron);
ronAfterFredDies === ronAfterFredDiesAgain // true
```



¿Que esta pasando aquí? Bueno, tenemos una función llamada `killSling` (oscura, lo sé), que toma un asistente. Copia el asistente y disminuye la cantidad de hermanos que tiene ese asistente. Ignoren los errores evidentes que hay aquí, quería mantener esto simple.



Luego pasamos `KillSibling` a `lodash.memoriza`, lo que produce una nueva funcion llamada `killSiblingMemorized`. Ahora, cuando llamamos `killSiblingMemorized` a `ron`, devuelve un objeto completamente nuevo. Si relizamos una verificación de igualdad estricta, devuelce falso. Por supuesto que lo hace, su hermano murió, ahora es una persona diferente.



Como hemos memorizado esto, si repetimos esta llamada `killSiblingMemorized`  obtenemos exactamente la misma versión que `ron` obtuvimos después de la primera llamda.



Hemos cubierto mucho, ahora pasemos a algunos temas avanzados.



---

## Funciones de orden superior

En matemáticas y ciencias de la computación, una **función de orden superior** es una función que realiza al menos una de las siguientes acciones:



- Toma una o más funciones como argumentos (es decir, parámetros de procedimiento)

- Devuelve una función como su resultado.

Esto parece mucho más intimidante de lo que es en realidad. De hecho, apuesto a que has escrito algunas de estas cosas antes sin date cuenta.



### Función que toma una función

```javascript
fetch('user', { userId: 1 }).theme((response) => {
    persistUser(response);  
})
```

Se trata simplemente **de devoluciones de llamadas.** Es este caso, `then` se trata de una función de orden superior que toma una función anónima como argunmento.

### Función que devuelve una función

```javascript
function counterGenerator() {
    let i = 0;
    return function(){
    console.log(++i)
    }
}

// Usage
const counter = counterGenerator();
counter(); // => 1
counter(); // => 2
counter(); // => 3
```

Otra cosa que probablemente hayas hecho antes **son los cierres.** En este caso, `counterGenerator` se trata de una función de orden superior porque devuelve una función.

Tanto `memoize` como `flow` eran funciones de orden superior que toman una función (o varias) como argumentos y devuelven una nueva función.

---





- [Siguente - ](./content/2024/06/README.md)

Aquí va el contenido de la entrada del blog. Puedes usar Markdown para agregar **negritas**, _cursivas_, [enlaces](https://github.com), y más.

![Imagen](../../assets/isotipo.png)
