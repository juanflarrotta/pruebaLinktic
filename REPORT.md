## Buenas Practicas
 En este proyecto se utilizó **sass** que es un preprocesador de css el cual nos ayuda a tener un código más limpio, simplificado y poder tener variables globales en los estilos, para que esto se implemente bien se utilizó la metodología **BEM** para el llamado de clases.

 En los componentes se implementó las novedades de vue 3 para la reactividad, lo que son los métodos **ref** y  **reactive**. También se utilizó **setup** el cual es un proceso nuevo al iniciar el componente.

## Estructura
 Se crea una carpeta **utils** donde se encuentra una carpeta scss que contiene las variables globales de estilos, el archivo **constants** tiene como objetivo tener variables globales que se pueden utilizar en todo el proyecto como en este caso la url del API y el svg del icono, y por ultimo un archivo **fetchData** que es una Funcion reutilizable que recibe una url, realiza el llamado al API y retorna la data.

## Proceso
 1. El consumo de API se realizó con el método fetch el cual retorna una promesa y si se resuelve retorna una data que se transforma con el método json() para poder ser manipulada.

 2. El proyecto antes de iniciar se pensó en utilizar las etiquetas de HTML5 como lo es el <datalist> el cual enlazado a un input se puede realizar el filtro de las letras que estemos escribiendo, pero como el diseño es personalizado descarte esta opción ya que un <datalist> no se puede personalizar los estilos, cada navegador lo interpreta de una forma diferente.

 3. Después de descartar la primera opción se realiza el filtro de las palabras mediante JS, el cual cada vez que cambie el valor en el input el ejecuta una funcion que hace un filter y compara con la lista de items devolviendo el array con los ítems que contengan las letras que escribimos, esto se refresca en una variable con método de reactividad lo que hace que actualice la información y haga un nuevo renderizado.

