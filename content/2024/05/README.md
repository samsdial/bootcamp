- [Inicio](./README.md)

# Fundamentos de programación funcional

### ¿Cuando en más util la programación funcional?

Antes de explicar **qué** es la programación funcional, creo que es útil definir cuando la utilizamos.

La programación funcional es más útil cuando realizamos transformaiones de datos 1 a 1.

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

- [Siguente - ](./content/2024/06/README.md)

Aquí va el contenido de la entrada del blog. Puedes usar Markdown para agregar **negritas**, _cursivas_, [enlaces](https://github.com), y más.


![Imagen](../../assets/isotipo.png)
