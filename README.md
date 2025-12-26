# Evaluación Práctica – PMO

## Ejercicio 2

### 1. ¿Qué es un servidor HTTP?
Es el encargado de recibir solicitudes de diferentes clientes y responder a dichas solicitudes utilizando el protocolo HTTP.
Su objetivo es permitir el acceso a ciertos recursos o la ejecución de acciones sobre los mismos.

Por ejemplo, para un sistema universitario para alumnos se podría desde un navegador acceder al sitio web de la universidad y obtener el HTML e imágenes correspondientes a la landing page y poder visualizar así información sobre la universidad, o enviar información en un formulario de registro para dar de alta un usuario en el sistema.

### 2. ¿Qué son los verbos HTTP? Mencionar los más conocidos
Para ejemplificar me basaré en un sistema universitario para alumnos:
* `GET`: obtener información
    * *Obtener un listado de carreras universitarias a las cuales el usuario se puede inscribir en formato JSON para que la página luego lo presente en un formato adecuado*
* `POST`: enviar información para crear un recurso
    * *El usuario envía toda su información personal y la carrera a la cual desea inscribirse, de esa forma se genera un registro nuevo de alumno en la BBDD (para este caso puntual)*
* `PUT`: actualizar un recurso completo
    * *El usuario modifica los datos personales en su perfil*
* `PATCH`: actualizar parcialmente un recurso
    * *El usuario en su perfil modifica la carrera a la que desea inscribir*
* `DELETE`: eliminar un recurso
    * *El usuario solicita eliminar su inscripción a la carrera, eliminándose así el registro generado previamente*

### 3. ¿Qué es un request y un response en una comunicación HTTP? ¿Qué son los headers?
Request es la solicitud realizada por el cliente al servidor para acceder a un recurso o ejecutar una acción. Consiste en:
* `Método/Verbo HTTP`: GET, POST, PUT, etc.
* `Path`: "locación" del recurso al cual se desea acceder – GET /carreras
* `Versión del protocolo HTTP`
* `Headers` (explicado luego)
* `Body`: contenido a enviar en la solicitud

Response es la respuesta que devuelve el servidor luego de procesar la solicitud:
* `Versión del protocolo HTTP`
* `Código de estado`: indicando cuál fue el resultado de la solicitud
* `Mensaje de estado`: breve descripción del código de estado
* `Headers` (explicado luego)
* `Body`: contenido solicitado o respuesta generada por el servidor

Puntualmente, los headers se utilizarán para incluir información adicional sobre la comunicación (lenguaje, formato del contenido, autorización, etc.) tanto en el request como en el response.

Por ejemplo, en la request POST de la pregunta 2 se incluiría en el header `Content-Type: application/json`, indicando que la información correspondiente al registro del usuario se enviará en formato JSON en el body de la solicitud.
En el caso de una request GET que solicita la web de la landing page de la universidad, la response incluirá en el header `Content-Type: text/html`, indicando así que la respuesta del servidor incluye código HTML en el body de la respuesta.

### 4. ¿Qué es un queryString? (En el contexto de una url)
Es una parte de la URL que permite pasar cierta información (en formato clave=valor) al servidor al momento de acceder a un recurso en específico, de esta forma el servidor podrá acceder a los mismos y, de acuerdo a los valores de ciertos parámetros enviados en la URL, variar la respuesta devuelta.

Estos parámetros enviados variarán según lo que permita el servidor web; se podría utilizar para realizar búsquedas, aplicar filtros en búsquedas, ordenar un listado con información o cualquier otra información útil.

Para poder pasar varios parámetros se utilizará el carácter &.

Por ejemplo, al obtener el listado de carreras al cual me puedo inscribir como usuario tendré que acceder al recurso `/carreras`. En el caso de que quiera obtener un listado ordenado alfabéticamente, el servidor puede permitirlo mediante el parámetro ordenado, por lo cual se podrá acceder al mismo mediante `/carreras?ordenado=true`.
Un ejemplo completamente distinto sería indicarle al servidor web que se desea el contenido en inglés a través del parámetro lang, entonces para obtener el listado de carreras pero con su nombre en inglés se podría acceder mediante `/carreras?lang=en_US`.

En resumen, para este ejemplo la estructura sería:

`www.unlam.com/carreras?ordenado=true&lang=en_US`

- `www.unlam.com` → Dominio / host
- `/carreras` → Recurso (path)
- `?` → Inicio del queryString
- `ordenado=true` → Primer parámetro (clave=valor)
- `&` → Separador de parámetros
- `lang=en_US` → Segundo parámetro (clave=valor)

### 5. ¿Qué es el responseCode? ¿Qué significado tiene los posiblesvalores devueltos?
Son un campo enviado en una response HTTP cuyo valor pertenece a una lista de códigos numéricos de 3 dígitos establecidos en el estándar del protocolo HTTP para indicar el resultado de una solicitud realizada por un cliente a un servidor. De esta forma, el cliente obtiene información sobre si su solicitud fue realizada con éxito, si ocurrió un error o cualquier otra información de utilidad por si es necesario realizar alguna acción extra.

El significado de los valores devueltos se agrupa en 5 grupos principales, donde luego cada código tendrá un significado en concreto:
* `Respuestas informativas (100–199)`. Ej: 102 – Processing (el servidor recibió la solicitud y se encuentra procesándola)
* `Respuestas satisfactorias (200–299)`. Ej: 201 – Created (la solicitud fue procesada por el servidor con éxito y se creó el recurso)
* `Redirecciones (300–399)`. Ej: 301 – Moved Permanently (el recurso al que se desea acceder ya no se encuentra en el path indicado)
* `Errores del cliente (400–499)`. Ej: 401 – Unauthorized (el recurso al que se desea acceder necesita autenticación para obtener la respuesta)
* `Errores del servidor (500–599)`. Ej: 500 – Internal Server Error (el servidor no pudo procesar la solicitud por algún error)

### 6. ¿Cómo se envía la data en un Get y cómo en un POST?
En el caso de un `GET` se envía la data mediante *queryString*, como se explicó anteriormente, donde la información correspondiente se pasará en la misma URL que se escribirá al realizar la solicitud.
`www.unlam.com/carreras?ordenado=true&lang=en_US`. Un aspecto a resaltar es que acá la información enviada es más visible al usuario, ya que la ve directamente en la URL y además, en la práctica, no se suele recomendar para el envío de información que no desea exponerse y grandes cantidades de datos.

Por su lado, en un `POST` la data se envía en el *body* de la solicitud, lo que permite transmitir una mayor cantidad de datos. Esta forma se utilizará al solicitar la creación de nuevos recursos o la ejecución de acciones que generan cambios en el sistema, como el envío de formularios (registro de usuarios).
A diferencia de la forma anterior, al no exponer los datos en la URL, es una mejor práctica en términos de seguridad y privacidad (más allá de otras formas de asegurar estas características).

### 7. ¿Qué verbo http utiliza el navegador cuando accedemos a una página?
Al acceder a una página el navegador realiza una solicitud HTTP utilizando el verbo `GET`, ya que en sí lo que se quiere es obtener el recurso.
Luego el servidor devolverá en la respuesta el código HTML correspondiente a la misma en el body.

### 8. Explicar brevemente qué son las estructuras de datos JSON y XML dando ejemplo de estructuras posibles.
Ambos son formatos estándar utilizados para almacenar y transmitir datos mayormente en la web.
Su principal diferencia radica en la estructura utilizada para representar los datos, más allá de las ventajas que puede otorgar cada uno en lo que respecta a su legibilidad, tecnología utilizada, etc.

`JSON (JavaScript Object Notation)`

Utiliza una estructura basada en pares *clave-valor*.
Entrando más en detalle, JSON permite ciertos tipos de datos como objetos (llaves {}), listas (corchetes []), cadenas, booleanos, nulos y números.
```json
{
    "nombre": "Victor",
    "documento": "111111",
    "edad": null,
    "poseeAuto": true,
    "cantidadAutos": 3,
    "carreras": ["Ing. Informatica", "Ing. Electronica"]
}
```

`XML (Extensible Markup Language)`

Utiliza una estructura similar a HTML, haciendo uso de *etiquetas* donde cada una puede llevar el nombre que uno quiera.
No cuenta con tipos de datos ni una forma más ligada a la programación para representar listas de elementos, por lo que se utilizan etiquetas para hacerlo.
```xml
<persona>
     <nombre>Victor</nombre>
     <documento>111111</documento>
     <edad></edad>
     <poseeAuto>true</poseeAuto>
     <cantidadAutos>3</cantidadAutos>
     <carreras>
        <carrera>Ing. Informatica</carrera>
        <carrera>Ing. Electronica</carrera>
    </carreras>
</root>
```


### 9. Explicar brevemente el estándar SOAP
`SOAP (Simple Object Access Protocol)` es un estándar de comunicación que utiliza el formato XML para el intercambio de información y establece reglas más rígidas para poder realizar la comunicación entre diferentes sistemas, definiendo cómo deben construirse, enviarse y procesarse los mensajes.

Por ejemplo, dentro de esas reglas, SOAP utiliza contratos formales para la comunicación, llamados WSDL (Web Services Description Language), que definen los servicios disponibles, los tipos de datos utilizados y la forma en que deben llamarse.

En la práctica se lo utiliza principalmente en entornos empresariales donde se suele necesitar que la comunicación sea lo más segura posible, se requiere mantener la integridad de los datos y un cierto grado de fiabilidad, pero a costa de un mayor nivel de complejidad y costo de desarrollo y mantenimiento.

### 10.Explicar brevemente el estándar REST Full
`REST Full` es otro estándar de comunicación que, a diferencia de SOAP, utiliza el formato JSON y reglas más flexibles para el intercambio de información. Además, la comunicación es sin estado, es decir, que cada solicitud contiene la información necesaria para ser procesada por el servidor, sin que este retenga información entre solicitudes.

En la práctica es ampliamente utilizado para servicios web y en entornos donde se busca mayor flexibilidad, menor rigidez en las reglas y un menor costo de desarrollo y mantenimiento debido a su simplicidad.

### 11. ¿Qué son los headers en un request? ¿Para qué se utiliza el key Content-type en un header?
Los `headers` en un request permiten enviar información adicional al servidor sobre cómo debe interpretarse la solicitud. Se pueden entender como los metadatos que acompañan al mensaje principal de esa comunicación.

El `header Content-Type` indica el formato de los datos enviados en el body del request, por ejemplo *JSON* o *XML*. Esto permite que el servidor procese correctamente la información recibida y aplique la lógica adecuada de acuerdo al formato.

## Ejercicio 3

### ¿Qué diferencias se observan entre las llamadas el punto 1 y 3?
En primer lugar, las llamadas consisten en realizar una solicitud GET al recurso contacts.json, donde el servidor devuelve un JSON con un listado de contactos existentes hasta el momento de la consulta.

Al realizar el `POST`, se añade al recurso un nuevo contacto con mis datos incluidos en el body. A su vez, el servidor responde con lo que parece ser un identificador para dicho contacto.

La diferencia es que al realizar nuevamente la solicitud `GET` en el punto 3, el servidor devuelve el listado de contactos actualizado, incluyendo el nuevo contacto agregado por mi solicitud POST.