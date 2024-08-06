- [Inicio](https://github.com/samsdial/bootcamp/)

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



Si observamos los dos ejemplos de código anteriores, podemos ver un marcado contraste. El bloque superior está escrito como React y solo dice ""



- [Siguente - ](./content/2024/06/README.md)

Aquí va el contenido de la entrada del blog. Puedes usar Markdown para agregar **negritas**, _cursivas_, [enlaces](https://github.com), y más.

![Imagen](../../assets/isotipo.png)
