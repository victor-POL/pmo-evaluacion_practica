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

### 5. ¿Qué es el responseCode? ¿Qué significado tiene los posibles valores devueltos?
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

## Ejercicio 4
[Click para accdeder al Perfil Trailhead](https://www.salesforce.com/trailblazer/u5sz9xs3a2g7w2qjb9)

## Ejercicio 5
> [!WARNING]
> ALGUNOS OBJETOS NO ESTABAN EN MI ENTORNO POR LO TANTO NO AGREGUES LOS CAMPOS

![Diagrama](https://github.com/victor-POL/pmo-evaluacion_practica/blob/main/ejercicio_5/diagrama.jpg?raw=true)

1. `Lead`
Representa un potencial cliente que aún no fue calificado. Es el primer registro que se crea cuando una persona o empresa muestra interés, pero todavía no se confirmó si existe una oportunidad real de negocio.
    - Address
    - Annual Revenue
    - Campaign
    - Clean Status
    - Company
    - Company D-U-N-S Number
    - Created By
    - Current Generator(s)
    - D&B Company
    - Data.com Key
    - Description
    - Do Not Call
    - Email
    - Email Opt Out
    - Fax
    - Fax Opt Out
    - Gender Identity
    - Individual
    - Industry
    - Last Modified By
    - Last Transfer Date
    - Lead Owner
    - Lead Source
    - Lead Status
    - Mobile
    - Name
    - No. of Employees
    - Number of Locations
    - Phone
    - Primary
    - Product Interest
    - Pronouns
    - Rating
    - SIC Code
    - Title
    - Website
2. `Account`
Representa una empresa, organización o cliente con el que la compañía mantiene o puede mantener una relación comercial.
    - Account Name
    - Account Number
    - Account Owner
    - Account Site
    - Account Source
    - Active
    - Annual Revenue
    - Billing Address
    - Clean Status
    - Created By
    - Customer Priority
    - D&B Company
    - Data.com Key
    - Description
    - D-U-N-S Number
    - Einstein Account Tier
    - Employees
    - Fax
    - Industry
    - Last Modified By
    - NAICS Code
    - NAICS Description
    - Number of Locations
    - Ownership
    - Parent Account
    - Phone
    - Rating
    - Shipping Address
    - SIC Code
    - SIC Description
    - SLA
    - SLA Expiration Date
    - SLA Serial Number
    - Ticker Symbol
    - Tradestyle
    - Type
    - Upsell Opportunity
    - Website
    - Year Started
3. `Contact`
Representa una persona asociada a una Account. Puede ser un cliente, usuario final, conocido, etc.
    - Account Name
    - Assistant
    - Asst. Phone
    - Birthdate
    - Buyer Attributes
    - Clean Status
    - Contact Owner
    - Created By
    - Creation Source
    - Data.com Key
    - Department
    - Description
    - Do Not Call
    - Email
    - Email Opt Out
    - Fax
    - Fax Opt Out
    - Gender Identity
    - Home Phone
    - idprocontacto
    - Individual
    - Languages
    - Last Modified By
    - Last Stay-in-Touch Request Date
    - Last Stay-in-Touch Save Date
    - Lead Source
    - Level
    - Loan Amount
    - Mailing Address
    - Mobile
    - Name
    - Other Address
    - Other Phone
    - Phone
    - Prequalified?
    - Pronouns
    - Reports To
    - Title
4. `Opportunity`
Representa una posible venta en curso. Permite hacer seguimiento del proceso comercial desde la identificación hasta el cierre, incluyendo monto y probabilidad.
    - Account Name
    - Amount
    - Close Date
    - Contract
    - Created By
    - Current Generator(s)
    - Delivery/Installation Status
    - Description
    - Expected Revenue
    - Forecast Category
    - Last Modified By
    - Lead Source
    - Main Competitor(s)
    - Next Step
    - Opportunity Name
    - Opportunity Owner
    - Opportunity Score
    - Order Number
    - Price Book
    - Primary Campaign Source
    - Private
    - Probability (%)
    - Quantity
    - Stage
    - Tracking Number
    - Type
5. `Product`
Representa un articulo que la empresa ofrece a sus clientes.
    - Active
    - Created By
    - Display URL
    - External Data Source
    - External ID
    - Last Modified By
    - Product Code
    - Product Description
    - Product Family
    - Product Name
    - Product SKU
    - Quantity Unit of Measure
    - Seller
    - Source Product
6. `PriceBook`
Representa una lista de precios que define el valor de los productos.
    - Active
    - Created By
    - Description
    - Is Standard Price Book
    - Last Modified By
    - Price Book Name
7. `Quote`
8. `Asset`
Representa un roducto que ya fue vendido y que pertenece a un cliente.
    - Account
    - Address
    - Asset Level
    - Asset Name
    - Asset Owner
    - Asset Provided By
    - Asset Serviced By
    - Competitor Asset
    - Consequence Of Failure
    - Contact
    - Created By
    - Current Amount
    - Current Lifecycle End Date
    - Current Monthly Recurring Revenue
    - Current Quantity
    - Description
    - Digital Asset Status
    - External Id
    - Install Date
    - Internal Asset
    - Last Modified By
    - Lifecycle End Date
    - Lifecycle-managed asset
    - Lifecycle Start Date
    - Location
    - Manufacture Date
    - Parent Asset
    - Price
    - Product
    - Product Code
    - Product Description
    - Product Family
    - Product SKU
    - Purchase Date
    - Quantity
    - Root Asset
    - Serial Number
    - Status
    - Status Reason
    - Total Lifecycle Amount
    - Unique Identifier
    - Usage End Date
9. `Case`
Representa una solicitud de soporte, reclamo o incidente reportado por un cliente.
    - Account Name
    - Asset
    - Business Hours
    - Case Number
    - Case Origin
    - Case Owner
    - Case Reason
    - Closed When Created
    - Contact Email
    - Contact Fax
    - Contact Mobile
    - Contact Name
    - Contact Phone
    - Created By
    - Date/Time Closed
    - Date/Time Opened
    - Description
    - Engineering Req Number
    - Entitlement Name
    - Escalated
    - Internal Comments
    - Last Modified By
    - Milestone Status
    - Milestone Status Icon
    - Parent Case
    - Potential Liability
    - Priority
    - Product
    - Product
    - Service Contract
    - SLA Policy End Time
    - SLA Policy Start Time
    - SLA Violation
    - Status
    - Stopped
    - Stopped Since
    - Subject
    - Type
    - Web Company
    - Web Email
    - Web Name
    - Web Phone
10. `Article`


## Ejercicio 6

### Soluciones de Salesforce

#### A. ¿Qué es Salesforce?
En un principio, se podría decir que es un CRM en la nube que permite gestionar relaciones con clientes, ventas, atención al cliente, marketing y cualquier otro proceso de negocio en un solo lugar, pero lo visto en el Trailhead demuestra que permite un alto nivel de personalización y crear lo que sea necesario según diferentes casos de uso.
#### B. ¿Qué es Sales Cloud?
Es una app de Salesforce Platform que está orientada a la parte de gestión de ventas de una empresa/negocio, incluyendo lo que respecta a administrar leads, oportunidades, cuentas, contactos, etc.
#### C. ¿Qué es Service Cloud?
Es otra app de Salesforce Platform enfocada a la atención al cliente, permitiendo gestionar casos de soporte, reportes, contactos, cuentas, etc. Su objetivo es que todos los puntos de contacto con los clientes estén en una plataforma unificada.
#### D. ¿Qué es Health Cloud?
Es otra app que ofrece una plataforma para la atención de la salud, lo cual lo logra reuniendo datos clínicos y no clínicos en un solo lugar. 
#### E. ¿Qué es Marketing Cloud?
Por último, también es una app que se centra en el marketing digital para ayudar en toda la interacción con el cliente a lo largo de toda su relación con el mismo. Permite gestionar campañas, automatizaciones, emails y realizar comunicaciones personalizadas, entre otras funcionalidades.


### Funcionalidades de Salesforce

#### A. ¿Qué es un RecordType?
Un Record Type permite definir diferentes tipos de registros dentro de un mismo objeto, adaptando el sistema a distintos procesos de negocio. Cada Record Type puede tener asociados distintos Page Layouts, valores de picklists y procesos.
#### B. ¿Qué es un ReportType?
Un Report Type define qué datos pueden analizarse en un reporte y cómo se relacionan los objetos involucrados. Determina qué objetos están disponibles, si los registros relacionados son obligatorios u opcionales, y qué campos se pueden utilizar.
#### C. ¿Qué es un Page Layout?
Controla la información de un objeto que se muestra al usuario, especificando qué campos se muestran, su orden, botones, listas relacionadas, etc. Además, permite establecer los campos que son visibles, de solo lectura y obligatorios.
#### D. ¿Qué es un Compact Layout?
Sería una versión del Page Layout orientada a mostrar los campos más importantes de un objeto y poder visualizarlos de forma más rápida y fácil, ya sea en la aplicación móvil de Salesforce, Lightning Experience y en las integraciones de Outlook y Gmail.
#### E. ¿Qué es un Perfil?
Al crear usuarios, se les asigna un perfil, el cual puede ser editado para modificar los permisos y configuraciones del mismo. La idea es controlar a qué objetos pueden acceder (read, create, edit, delete), configurar el tiempo de inactividad para cerrar su sesión, políticas sobre la contraseña, etc.
#### F. ¿Qué es un Rol?
Define la jerarquía organizacional dentro de Salesforce y controla la visibilidad de los registros entre usuarios, es decir, determinar si los usuarios tienen acceso a registros que no les pertenecen. La idea es que los usuarios asignados a roles superiores tengan acceso a los registros que pertenecen a sus subordinados o que comparten con ellos.
#### G. ¿Qué es un Validation Rule?
Es una regla que mejora la calidad de los datos a través de la verificación de los mismos al momento de que un usuario inserta un registro. Puede ser una fórmula o expresión que va a evaluar los datos del registro y retornar verdadero o falso.
#### H. ¿Qué diferencia hay entre una relación Master Detail y Lookup?
En una relación Lookup, los objetos funcionan de forma independiente, lo cual permite que se pueda eliminar uno de los objetos y el otro siga existiendo. Se puede ver como una relación débil.

En el caso de Master Detail no es así, ya que tener un objeto sin el otro "no tiene sentido", como una relación fuerte. Por lo tanto, al eliminar, si elimino el objeto principal, también se eliminará el objeto relacionado.
#### I. ¿Qué es un Sandbox?
Es un entorno que replica el entorno productivo para poder desarrollar, probar o cualquier otra acción o propósito sin afectar datos reales o un entorno que no queremos afectar involuntariamente con cambios no deseados.
#### J. ¿Qué es un ChangeSet?
Es una herramienta que permite migrar las personalizaciones realizadas en una organización Salesforce a otra. Por ejemplo, al crear un objeto en un sandbox de prueba, este luego puede ser enviado a la organización de producción.
#### K. ¿Para qué sirve el import Wizard de Salesforce?
Sirve para importar cuentas, contactos, clientes potenciales, soluciones, objetos personalizados, cuentas personales, etc. Este asistente solo va a permitir la importación si el archivo viene en formato CSV.
#### L. ¿Para qué sirve la funcionalidad Web to Lead?
Web to Lead permite capturar información de potenciales clientes desde los visitantes del sitio web de una empresa cuando proporcionan información de contacto y así crear automáticamente registros de tipo Lead, e incluso redirigirlos a otra página web para alguna campaña.
#### M. ¿Para qué sirve la funcionalidad Web to Case?
Web to Case permite crear casos de soporte automáticamente desde el sitio web de una empresa. Cada solicitud enviada genera un registro de caso en Salesforce para su seguimiento.
#### N. ¿Para qué sirve la funcionalidad Omnichannel?
Omnichannel permite distribuir automáticamente el trabajo (casos, chats, tareas) entre las diferentes representantes de asistencias según reglas, prioridades y disponibilidad en el centro de llamadas.
#### O. ¿Para qué sirve la funcionalidad Chatter?
Chatter es una herramienta de colaboración entre usuarios en oportunidades de ventas, casos de servicios, campañas, etc. Permite comunicarse, compartir información y trabajar de forma colaborativa.

### Conceptos generales

#### A. ¿Qué significa SaaS?
SaaS (Software as a Service) se refiere a un modelo donde el software es distribuido a los clientes a través de internet, los cuales generalmente pagan por su uso y no se requiere de una instalación local del software, sino que se utiliza, por ejemplo, mediante un navegador web.
#### B. ¿Salesforce es Saas?
Sí, ya que se accede y utiliza a través de la nube sin necesidad de descargar o instalar un programa adicional; simplemente desde un navegador se puede comenzar a utilizar.
#### C. ¿Qué significa que una solución sea Cloud?
Significa que la misma se encuentra en una infraestructura remota, es decir, el sistema no se encuentra instalado localmente, sino que para hacer uso de la misma y de su procesamiento o servicios que ofrece, uno debe acceder a la misma mediante internet y no va a requerir que la empresa que la use necesite un hardware en específico o que instale otro software que use dicho hardware. Además, esa solución es gestionada por proveedores externos a la empresa.
#### D. ¿Qué significa que una solución sea On-Premise?
En este caso, la solución se encuentra instalada en una infraestructura propia de la empresa y además es gestionada por la misma. Entonces la empresa debe encargarse de tener el hardware y software necesario para poder instalar y utilizar la solución desarrollada, darle el mantenimiento correspondiente, etc.
#### E. ¿Qué es un pipeline de ventas?
Es una herramienta que va a definir las oportunidades de venta que se le van a ir presentando desde el primer contacto hasta el cierre, permitiendo visualizar más fácilmente en qué punto se encuentra cada potencial cliente.
#### F. ¿Qué es un funnel de ventas?
Es un sistema que analiza el camino que recorren los potenciales clientes hasta convertirse en clientes, es decir, un proceso en el cual las primeras etapas consisten en generar algún atractivo para los futuros clientes y que logren conocernos, para luego poder arrancar una relación y finalmente otorgarle una oferta de valor al mismo. Entonces, al ir avanzando, se va reduciendo el número de potenciales clientes que llegan cada vez más filtrados e informados a nuestra oferta final. Se va a centrar más en la cantidad o volumen de personas que llegan al final y mejorar cada una de las etapas.
#### G. ¿Qué significa Customer Experience?
Es toda la experiencia general del cliente en todos los puntos de interacción que tiene con nuestra marca, abarcando toda la experiencia completa desde que nos conoce hasta la posventa. Principalmente, se basa en el uso de conocimientos de marketing para poder aumentar los ingresos a través de la publicidad, mejorar el servicio al cliente y crear una marca que se centra en cómo esos clientes perciben a nuestra empresa y qué imagen tienen de nosotros.
#### H. ¿Qué significa omnicanalidad?
Significa que una empresa ofrece una experiencia integrada y consistente a través de los diferentes canales de contacto que los clientes tienen con la misma, por ejemplo, a través de una página web, teléfono, email, redes sociales, atención presencial, etc.
#### I. ¿Qué significa que un negocio sea B2B?¿Qué significa que un negocio sea B2C?¿Qué es un KPI?
- `B2B`: una empresa ofrece productos o servicios a otras empresas. EJ: fabrica de maquinas de coser le vende a fabricantes de ropa
- `B2C`: una empresa vende productos o servicios directamente al consumidor final. EJ: tienda online de productos electronicos los vende a usuarios finales
- `KPI`: es un indicador clave de desempeño que se utiliza para medir el nivel de cuplimiento de objetivos de un proceso, área o negocio. En base a eso se pueden identificar desvíos, evaluar resultados y tomar decisiones basadas en datos.

#### J. ¿Qué es una API y en qué se diferencia de una Rest API?
`API` (Application Programming Interface) hace referencia al conjunto de reglas establecidas para permitir que diferentes sistemas se puedan comunicar entre sí y hacer uso de funcionalidades de la misma de una forma estructurada y controlada.

`REST API` es un tipo específico de API que hace uso del protocolo HTTP y principios REST, que anteriormente se describieron, como los verbos HTTP, que la comunicación sea stateless, código de estado, etc.
#### K. ¿Qué es un Proceso Batch?
Es un proceso que se ejecuta en horarios programados, en segundo plano y generalmente con volúmenes de datos recopilados hasta ese momento. Por ejemplo, todas las transferencias realizadas en un banco se podrían procesar al mismo tiempo al finalizar el día para evitar que las mismas se realicen en tiempo real.
#### L. ¿Qué es Kanban?
Es una metodología ágil de trabajo que se destaca por el uso de tableros visuales para representar tareas y su estado. La idea es que todo el equipo pueda visualizar fácilmente el trabajo a realizar y el punto en el que se encuentra el proyecto para así poder tomar mejores decisiones.
#### M. ¿Qué es un ERP?
Un ERP (Enterprise Resource Planning) es un sistema que integra y gestiona los procesos internos de una organización, generalmente los más conocidos como finanzas, compras, inventario, logística, RRHH, etc. Su objetivo sería centralizar toda esa información en un único sitio.
#### N. ¿Salesforce es un ERP?
No, ya que en un principio está enfocado a la gestión de clientes, ventas, marketing y servicio, pero, como vimos, permite una gran personalización y crear nuevas funcionalidades para diferentes casos de uso.

## Ejercicio 7
_El codigo del ejercicio se encuentra en la carpeta `ejercicio_6`_
Archivos:
- ContactEmailQueueable.apxc
- ContactTrigger
- ProContactoCallouts

> [!WARNING]
> Se utilizo el playground `My Trailhead Playground 1`.
> El otro playground `Data modeling - Playground` lo cree recién para el modulo de `Modelado de datos`.

