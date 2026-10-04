> [!NOTE]
> Este repositorio fue creado por primera vez el 13 de septiembre de 2024.
>
> ![Captura de la ultima modificacion del repositorio](IMG/Captura%20de%20pantalla%202026-10-04%20235132.png)
>
> Se decidió borrar el anterior repositorio y subirlo en este para censurar datos, IDs, además de que aparecían en el historial de commits.
> 

# 0.0 Introducción (Opcional TODO, hasta el 1.0 ):
AWS (Amazon Web Services) es una plataforma en la nube que ofrece una amplia gama de servicios como almacenamiento, computación y bases de datos, permitiendo a las empresas construir y gestionar aplicaciones sin necesidad de infraestructura física.

EKS (Elastic Kubernetes Service) es uno de los muchos servicios dentro de AWS, que facilita el despliegue y la gestión de aplicaciones en contenedores mediante Kubernetes. Con EKS, los usuarios pueden ejecutar clústeres de Kubernetes de manera eficiente, escalando automáticamente y aprovechando la infraestructura segura y confiable de AWS sin tener que gestionar manualmente los componentes de Kubernetes.
# 0.1 Registrarme:

Voy a elegir este nivel de soporte:

![image](IMG/01.png)


## 0.1.1 Después de registrarme por primera vez:

![image](IMG/02.png)

Esta sería la página principal que voy a ver

Para utilizar el EKS (Elastic Kubernetes Service) (El gestor de Kubernetes de Amazon)

# 0.2 Prerequisitos (Opcionales, pero importantes):

## 0.2.1 IAM (Identity and Access Management) y MFA

> [!IMPORTANT]
> Cosas que tenemos que tener en cuenta, de la misma manera, que en MySQL, no accedemos a la base de datos con el usuario root, si no que creamos un usuario con bastantes privilegios, >pues aquí es casi lo mismo.

**Cuando creamos una cuenta en AWS, lo que creamos es en realidad, una cuenta ROOT**. Y cada vez que accedemos, accederemos desde la root. Si cerramos sesión, y queremos volver a iniciar sesión:

![image](IMG/03.png)

En esta página, tenemos dos opciones, el usuario raíz (root) o un usuario de la IAM. Si intentamos iniciar sesión con aquel usuario, que creamos al principio DESDE EL APARTADO DE IAM, no nos va a dejar, porque como hemos dicho, ese usuario no es de la IAM, es un usuario root.

Vamos a crear entonces, un usuario de la IAM.

![image](IMG/04.png)

Y vamos a meternos y este es el panel del IAM:

![image](IMG/05.png)

**Lo primero que nos aparece es de hecho no crear el usuario si no, el Autenticación de múltiples factores**, se trata no solo inicies sesión con el correo y la contraseña, si no, un paso más, por temas de seguridad, para verificar que realmente la persona pues eres tú.

![image](IMG/06.png)

De hecho, estoy obligado a hacerlo, de aquí a 29 días...
### MFA
Una vez, le haya dado click, a "Agregar MFA" y esté dentro tengo 3 opciones para autenticarme:
- Clave de paso o clave de seguridad
- **Aplicación del autenticador**
- Token de contraseña temporal de un solo uso (TOTP) de hardware

Pienso elegir la "Aplicación", necesitamos una app en el movil, esa app tendrá sus mecanismos para saber que eres tú (en mi caso, hace tiempo en una empresa, me llamaron, me pidieron una foto con mi DNI, etc. Depende mucho de la app, y de la persona que va a autenticarte), generará un token aleatorio, y cada vez que iniciemos sesión (probablemente con el root) pues nos lo pedirá.

También tengo que ponerle un nombre al dispositivo ¿? No sé realmente que es ni para que sirve.

![image](IMG/07.png)

Le damos a siguiente, y ahora ya tenemos que instalar la aplicación. Entre algunas está el Google Authenticator

![image](IMG/08.png)

Entonces, lo hemos descargado y he iniciado sesión con la cuenta de GitHub. Para asociar el google authenticator, solo necesitamos escanear el QR, el QR es el que está en AWS.

Como he utilizado el Google Authenticator, le doy al "+" y escanear código QR.

![image](IMG/09.png)

Y ya estará enlazado. Cada 20 segundos, creará un código aleatorio de 6 dígitos. Ponemos primero 1, esperamos los 20 segundos, generará otro y lo ponemos y eso es todo.

Volviendo al panel del IAM, tiene que aparecer así. (*Hay que refrescar*)

![image](IMG/10.png)

### Para crear un usuario

Una vez hemos puesto el MFA al root, en la parte izquierda del panel de IAM, encontraremos Usuarios, le damos click.
Y luego crear usuario.

![image](IMG/11.png)

Nos piden, datos del User, nombre, contraseña, etc.

![image](IMG/12.png)

Le asignamos permisos:

![image](IMG/13.png)

Es importante revisarlo, cuando estemos seguros lo creamos:

![image](IMG/14.png)

Listo, esta es la pantlla que deberíamos ver:

![image](IMG/15.png)

> [!IMPORTANT]
> Vamos a descargar el archivo .csv. Recordemos, que la contraseña, no la hemos puesto nosotros, si no que es una generada aleatoriamente
> Dentro aparecerá la contraseña.
>

Ahora necesitamos crear un MFA para el usuario que acabamos de crear.

![image](IMG/16.png)

Le damos click al nombre, ahora vamos a "Credenciales de Seguridad" y en el apartado de MFA "asginar dispositivo".

![image](IMG/17.png)

![image](IMG/18.png)

El nombre que le voy a poner es, "mi_telefono2".

> [!WARNING]
> No se puede utilizar el mismo nombre que utilizamos antes.
>

## 0.2.2 Crear una alarma para la facturación. (CloudWatch).

El servicio que vamos a utilizar es **CloudWatch**
Pero antes, tenemos que habilitar un par de cosas, en el apartado de facturación

![image](IMG/19.png)

Una vez dentro, me aparece pues lo que le debo a AWS (*Porque primero hice el clúster y luego todo esto 24/09/2024*)
![image](IMG/20.png)

Para ver a más detalle de donde sale eso, nos vamos al apartado de facturas:

![image](IMG/21.png)

>[!IMPORTANT]
> A mi, no me aparece el apartado "Billing Preferences", teniendo la página en español.
> La he cambiando en inglés, y si aparece. Y desde esa página he vuelto a cambiar el idioma.
>

![image](IMG/22.png)

Voy a darle a "editar" en el apartado de "Preferencias de entrega de facturas".

![image](IMG/23.png)

Efectivamente, queremos que nos mande las facturas por correo.

También, en preferencias de alertas:

![image](IMG/24.png)

Ahora vamos a buscar el servicio **CloudWatch**

### CloudWatch

![image](IMG/25.png)

Y este es el panel del CloudWatch, vamos a "Alarmas" --> "Todas las alarmas". He marcado arriba a la derecha que el servidor es "Estocolmo". (*No por casualidad*).

>[!WARNING]
> De hecho, **Tenemos que cambiarle el Estocolmo, o el servidor que tengamos y poner el EE.UU. Este (Norte de Virginia)
us-east-1**, Si no, más adelante, a la hora de crear los parámetros que quiere monitorear, no aparecerá el que queremos, aparecerá con el North Virginia, pero no con el Estocolmo.
>

![image](IMG/26.png)

Creamos la alarma:

![image](IMG/27.png)

Y seleccionamos la "métrica".

![image](IMG/28.png)

Buscamos el "Cargo total estimado".

![image](IMG/29.png)

Seleccionamos divisa, el USD y luego "Seleccionar Métrica".

Luego, voy a establacer **sólamente**, que me avise cuando supere los 5€:

![image](IMG/30.png)

A continuación, tengo que crear el "Topic".

![image](IMG/31.png)

No voy a modificar nada más, le doy a siguiente.

![image](IMG/32.png)

Le doy un nombre a la alarma, y una descripción. Reviso en la vista previa y le doy a crear.

Nos tiene que haber llegado un correo a mi cuenta de correo del AWS, para confirmar:

![image](IMG/33.png)

![image](IMG/34.png)


## 0.2.3 Crear un certificado (Opcional para esto).

Ahora es turno de crear el certificado.
### ¿Qué es un certificado y para qué lo necesitamos?

### Prerequisitos.
- Tener un dominio.
  
### Creando el certificado.
Para ello, vamos a utilizar este servicio:

![image](IMG/35.png)

# 1.0 Crear una VPC (Amazon Virtual Private Cloud)(Parte IMPORTANTE, NO OPCIONAL)
> [!IMPORTANT]
> **ESTO ES UN REQUISITO ESENCIAL PARA PODER CREAR MI PROPIO CLÚSTER**
>

Primero necesitamos crear un VPC
![image](IMG/36.png)

Lo buscamos arriba en la barra de búsqueda y ahora pues nos aparecerá esto:

![image](IMG/37.png)

Pues ahora, vamos a crear una red privada.

![image](IMG/38.png)

![image](IMG/39.png)

> [!IMPORTANT]
> Por algún motivo, en mis PVC podemos observar que ya hay una creada con la dirección IP 172.31.0.0
>
> No sé porqué está creada, luego creamos una desde 0.

Vamos a echar un rápido vistazo a lo que vemos en esta ventana y es básicamente una PVC con sus características el ID, el estado, el CIDR IPv4 etc.

> Nos aparece el CIDR (Classless Inter-Domain Routing) es un método para asignar y gestionar direcciones IP que permite una mayor flexibilidad en la forma en que se agrupan las direcciones de red. 
>
>CIDR se introdujo para reemplazar el antiguo sistema de clases de red (A, B, C) y ayudar a mejorar la eficiencia en la asignación de direcciones IP y la agregación de rutas en Internet.
>

Si le damos a la VPC esa en cuestión, si le damos a la casilla nos aparecerá abajo:

![image](IMG/40.png)

También se ve muy pequeñito así que lo voy a desplegar/abrir para que se vea un poco mejor.

![image](IMG/41.png)

Y así ya se ve mejor, voy a darle al apartado de "Mapa de recursos".

![image](IMG/42.png)

Voy a ponerle un nombre, porque sencillamente no lo tiene, como podemos comprobar.

![image](IMG/43.png)


![image](IMG/44.png)

Como podemos comprobar, ya se ha cambiado:

![image](IMG/45.png)

El resto de cosas, como gateway, etc. De momento, no las vamos a tocar. 

## 1.1 Vamos a crearlo crear nosotros una VPC propia.

![image](IMG/46.png)

Ahora, nos mandará a esta otra pestaña, **voy a seleccionar VPC y más**:

![image](IMG/47.png)

Vamos a darle un 
- Nombre
- La IP por defecto, si no le ponemos nosotros una
- NO vamos a utilizar IPv6
- Número de zonas de disponibilidad (AZ), por defecto, me viene 2, yo voy a seleccionar 3
- Poner a 0 las subredes privadas, solo públicas y están puesto a 3.


![image](IMG/48.png)

![image](IMG/49.png)

Y la creamos, abajo del todo botón naranja:

![image](IMG/50.png)


![image](IMG/51.png)

Si quisieramos eliminarla, tan solo necesitamos:

![image](IMG/52.png)

## 1.2 Ahora voy a editar las subredes o subnets:

A la izquierda aparece una lista larga de objetos, 

- Nube virtual privada
- Sus VPC
- **Subredes**
- Tablas de enrutamiento
- Puertas de enlace de Internet
- Puerta de enlace de Internet de solo salida
- Conjuntos de opciones de DHCP
- Direcciones IP elásticas
- Listas de prefijos administradas
- Puntos de conexión
- Servicios de punto de conexión

![image](IMG/53.png)

Voy a editar la subred 10.0.0.0 

![image](IMG/54.png)

Y habilitamos el "Enable auto-assign public IPv4 address, abajo del todo le damos a guardar.

![image](IMG/55.png)

# 2.0 Crear el clúster con EKS

Nos vamos arriba a la barra de búsqueda y buscamos por EKS:

![image](IMG/56.png)

También podríamos llegar a utilizar ECS:

![image](IMG/57.png)

Pero ahora mismo va a ser el EKS.

![image](IMG/58.png)

Así que vamos a darle a crear.

### Parte 1. Creación Clúster (Configurar clúster)

![image](IMG/59.png)

Lo primero que nos pide es ponerle un nombre al Clúster de Kubernetes y asignarle un rol.

![image](IMG/60.png)

Como no tenemos un rol vamos a crearlo:

![image](IMG/61.png)

y caso de uso, pues EKS - Clúster:

![image](IMG/62.png)

Le daremos a siguiente:

![image](IMG/63.png)

y estos son los permisos que va a dar Amazon al rol, si lo desplegamos los podemos ver a detalle, es un archivo JSON:

![image](IMG/64.png)

Le daremos a siguiente y pondremos el nombre del rol.

![image](IMG/65.png)

Lo creamos. Vamos a volver a la página de creación del clúster, refrescamos la parte de roles y simplemente lo seleccionamos.

![image](IMG/66.png)

El resto no lo pienso tocar:

- Configuración de la versión de Kubernetes
- Acceso al clúster
- Cifrado de secretos
- Etiquetas (0)

### Parte 2. Creación Clúster (Especificar redes)

Al dar siguiente, vamos a especificar las redes:

![image](IMG/67.png)

y en cuanto a las subredes pues elegimos las que correspondan a ese VPC. También añadimos un SecurityGroup, el por defecto.

![image](IMG/68.png)

y el acceso va a ser SOLO público.

![image](IMG/69.png)

### Parte 3. Creación Clúster (Configurar la observabilidad)

Ahora estamos en esta otra pantalla:

![image](IMG/70.png)

En observabilidad no vamos a hacer nada.

![image](IMG/71.png)

Tampoco vamos a habilitar ninguna opción de "Registro del plano de control"

### Parte 4. Creación Clúster (Seleccionar complementos)

Le damos a siguiente y ya estamos en el Paso 4, seleccionar complementos:

> Los complementos de Amazon EKS proporcionan una lista seleccionada de software operativo que se puede habilitar en su clúster. Todo el software incluye los últimos parches de seguridad y correcciones de errores y AWS lo valida para trabajar con EKS. Los complementos facilitan el aprovisionamiento de un clúster con las herramientas operativas necesarias para que pueda >comenzar a ejecutar sus aplicaciones.
> 
> - De forma predeterminada, los complementos que requieran acceso a otros servicios de AWS intentarán utilizar los permisos asociados al rol de IAM del nodo de trabajo. Como práctica recomendada, puede utilizar roles de IAM para las cuentas de servicio para asociar un rol de IAM a un complemento de EKS que requiera permisos de IAM. A continuación, ya no tendrá que proporcionar permisos extendidos al rol de IAM del nodo para que el complemento pueda llamar a las API de AWS. Puede transferir un rol de IAM al complemento como parte de su configuración al iniciarlo o en cualquier momento como una actualización.
> - Cuando se utilizan roles de IAM para cuentas de servicio, la relación de confianza se establece en el clúster y la cuenta de servicio, de modo que cada combinación de clústeres y complementos requiere un rol único.
>

![image](IMG/72.png)

### Parte 5. Creación Clúster (Configurar las opciones de complementos seleccionados)

En esta parte es para configurar lo que antes hemos añadido de complementos, los voy a dejar por defecto:

![image](IMG/73.png)

### Parte 6. Creación Clúster (Revisar y crear)

Vamos a ver que tengamos todo como queríamos y lo creamos.

Ya está el clúster creado, ahora vamos a esperar que termine de "crearse".
![image](IMG/74.png)

Refrescamos la página hasta que aparezca, activo:

![image](IMG/75.png)


## 2.1 Crear un nodo.

Ahora nos toca crear los nodos del clúster:

![image](IMG/76.png)

![image](IMG/77.png)

Y bajamos hasta llegar a Grupos de Nodo, **vamos a crear un Group Node**

![image](IMG/78.png)

y vamos a necesitar crear otro Rol, en nuestro caso es un EC2.

### Parte 1. Seleccionar entidad de confianza.

![image](IMG/79.png)

![image](IMG/80.png)

### Parte 2. Agregar permisos.

En cuanto a los permisos, buscamos este:

- AmazonEKSWorkerNodePolicy
- AmazonEKS_CNI_Policy
- AmazonEC2ContainerRegistryReadOnly

### Parte 3. Asignar nombre, revisar y crear.

Por último, asignarle un nombre.

![image](IMG/81.png)

Volvemos a la parte de crear un nodo:

## Parte 1. Crear Grupo de Nodos. Configurar grupo de nodos.

Solo voy a asignarle el nombre y el rol que acabamos de crear.

## Parte 2. Crear Grupo de Nodos. Establecer la configuración informática y de escalado.

> [!WARNING]
> Esta parte es importante y afecta directamente al precio:
>

![image](IMG/82.png)

![image](IMG/83.png)


## Parte 3. Crear Grupo de Nodos. Especificar redes.

No tiene mucha ciencia, seleccionamos nuestras subredes.

## Parte 4. Crear Grupo de Nodos. Revisar y crear.

Y nada, revisamos, creamos y a esperar un rato.

**En mi caso, me da este error**

![image](IMG/84.png)

Bastante sin sentido, porque antes hemos habilitado la opción esa de la subred.

Básicamente tengo que volver al VPC, luego buscar las 3 subredes y comprobar que las 3 tengan la opción esa, habilitada.

Así que eso, borramos el grupo y lo creamos de nuevo.

![image](IMG/85.png)

# 3. Usar el CloudShell de AWS para agregar el nodo al clúster.

![image](IMG/86.png)

tarda un poco:

![image](IMG/87.png)

El CLI es diferente al de Linux, si hago un `aws help` me aparecerán pues todos los comandos, para salir de allí tengo que presionar `**Q**`

```
aws eks update-kubeconfig --name cluster-demo-primero --alias prueba-demo
```

en "name" va el nombre del cluster. Ahora hacemos un 

```
kubectl get nodes
```

![image](IMG/88.png)

# 4. Detener la máquina.

>[!WARNING]
>Cargos por el clúster de EKS:
>
Amazon te seguirá cobrando una tarifa fija por cada clúster de EKS que tengas activo, incluso si no tienes nodos en funcionamiento. El costo de un clúster de EKS es aproximadamente 0,10 USD por hora, lo que equivale a unos 72 USD al mes por clúster.
>

Amazon EKS no permite detener un clúster directamente, pero puedes eliminarlo o detener los nodos de trabajo (EC2) asociados.

```
aws eks delete-cluster --name cluster-demo-primero
```

como el comando es de literalmente eliminar el clúster, me avisa que el comando es peligroso:

![image](IMG/89.png)
