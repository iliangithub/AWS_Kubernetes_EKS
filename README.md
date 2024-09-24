# 0.0 Introducción:
AWS (Amazon Web Services) es una plataforma en la nube que ofrece una amplia gama de servicios como almacenamiento, computación y bases de datos, permitiendo a las empresas construir y gestionar aplicaciones sin necesidad de infraestructura física.

EKS (Elastic Kubernetes Service) es uno de los muchos servicios dentro de AWS, que facilita el despliegue y la gestión de aplicaciones en contenedores mediante Kubernetes. Con EKS, los usuarios pueden ejecutar clústeres de Kubernetes de manera eficiente, escalando automáticamente y aprovechando la infraestructura segura y confiable de AWS sin tener que gestionar manualmente los componentes de Kubernetes.
# 0.1 Registrarme:

Voy a elegir este nivel de soporte:

![image](https://github.com/user-attachments/assets/b43328e4-4c52-4e83-8eb4-97f7593a96a3)


## 0.1.1 Después de registrarme por primera vez:

![image](https://github.com/user-attachments/assets/ea9a69cd-3a75-4e78-a60f-6eef584edcb9)

Esta sería la página principal que voy a ver

Para utilizar el EKS (Elastic Kubernetes Service) (El gestor de Kubernetes de Amazon)

# 0.2 Prerequisitos (Opcionales, pero importantes):

## 0.2.1 IAM (Identity and Access Management) y MFA

> [!IMPORTANT]
> Cosas que tenemos que tener en cuenta, de la misma manera, que en MySQL, no accedemos a la base de datos con el usuario root, si no que creamos un usuario con bastantes privilegios, >pues aquí es casi lo mismo.

**Cuando creamos una cuenta en AWS, lo que creamos es en realidad, una cuenta ROOT**. Y cada vez que accedemos, accederemos desde la root. Si cerramos sesión, y queremos volver a iniciar sesión:

![image](https://github.com/user-attachments/assets/b8729a36-8565-4532-b063-1cff1637eca9)

En esta página, tenemos dos opciones, el usuario raíz (root) o un usuario de la IAM. Si intentamos iniciar sesión con aquel usuario, que creamos al principio DESDE EL APARTADO DE IAM, no nos va a dejar, porque como hemos dicho, ese usuario no es de la IAM, es un usuario root.

Vamos a crear entonces, un usuario de la IAM.

![image](https://github.com/user-attachments/assets/9f5aa822-2601-49f8-b6f1-ba0ca27abf40)

Y vamos a meternos y este es el panel del IAM:

![image](https://github.com/user-attachments/assets/8b644efa-6b6a-4101-b537-a3159369fc0c)

**Lo primero que nos aparece es de hecho no crear el usuario si no, el Autenticación de múltiples factores**, se trata no solo inicies sesión con el correo y la contraseña, si no, un paso más, por temas de seguridad, para verificar que realmente la persona pues eres tú.

![image](https://github.com/user-attachments/assets/fc4f4fa8-8b89-4402-b5b8-a1e0082d6fe6)

De hecho, estoy obligado a hacerlo, de aquí a 29 días...
### MFA
Una vez, le haya dado click, a "Agregar MFA" y esté dentro tengo 3 opciones para autenticarme:
- Clave de paso o clave de seguridad
- **Aplicación del autenticador**
- Token de contraseña temporal de un solo uso (TOTP) de hardware

Pienso elegir la "Aplicación", necesitamos una app en el movil, esa app tendrá sus mecanismos para saber que eres tú (en mi caso, hace tiempo en una empresa, me llamaron, me pidieron una foto con mi DNI, etc. Depende mucho de la app, y de la persona que va a autenticarte), generará un token aleatorio, y cada vez que iniciemos sesión (probablemente con el root) pues nos lo pedirá.

También tengo que ponerle un nombre al dispositivo ¿? No sé realmente que es ni para que sirve.

![image](https://github.com/user-attachments/assets/03f01fde-728e-448f-a1a2-734c332311a3)

Le damos a siguiente, y ahora ya tenemos que instalar la aplicación. Entre algunas está el Google Authenticator

![image](https://github.com/user-attachments/assets/a2fd2e07-09af-459f-8f59-13093ff0fb91)

Entonces, lo hemos descargado y he iniciado sesión con la cuenta de GitHub. Para asociar el google authenticator, solo necesitamos escanear el QR, el QR es el que está en AWS.

Como he utilizado el Google Authenticator, le doy al "+" y escanear código QR.

![image](https://github.com/user-attachments/assets/97a06191-49fd-482a-9b7b-9530d4d1e9fc)

Y ya estará enlazado. Cada 20 segundos, creará un código aleatorio de 6 dígitos. Ponemos primero 1, esperamos los 20 segundos, generará otro y lo ponemos y eso es todo.

Volviendo al panel del IAM, tiene que aparecer así. (*Hay que refrescar*)

![image](https://github.com/user-attachments/assets/7bf2356c-9701-4ecc-9e62-a66f0c8cbc8f)

### Para crear un usuario

Una vez hemos puesto el MFA al root, en la parte izquierda del panel de IAM, encontraremos Usuarios, le damos click.
Y luego crear usuario.

![image](https://github.com/user-attachments/assets/2f003521-264b-45fd-a79b-f4970a1a4be6)

Nos piden, datos del User, nombre, contraseña, etc.

![image](https://github.com/user-attachments/assets/d8441da5-853b-4332-8589-5949fd9bb87a)

Le asignamos permisos:

![image](https://github.com/user-attachments/assets/60153cf9-dfdf-4129-893b-c9b18880cb75)

Es importante revisarlo, cuando estemos seguros lo creamos:

![image](https://github.com/user-attachments/assets/d94957f8-f169-45ba-9e47-fe609e98c27c)

Listo, esta es la pantlla que deberíamos ver:

![image](https://github.com/user-attachments/assets/7233fc09-22db-4aba-ba1a-6f015588e226)

> [!IMPORTANT]
> Vamos a descargar el archivo .csv. Recordemos, que la contraseña, no la hemos puesto nosotros, si no que es una generada aleatoriamente
> Dentro aparecerá la contraseña.
>

Ahora necesitamos crear un MFA para el usuario que acabamos de crear.

![image](https://github.com/user-attachments/assets/bddcd0bb-de62-4dea-aa5b-4f89a02884a4)

Le damos click al nombre, ahora vamos a "Credenciales de Seguridad" y en el apartado de MFA "asginar dispositivo".

![image](https://github.com/user-attachments/assets/7e9fc073-f65a-41d6-ae8b-fc05b5d46226)

![image](https://github.com/user-attachments/assets/ccc3e354-4ccb-4d20-9be2-c4acb94f9417)

El nombre que le voy a poner es, "mi_telefono2".

> [!WARNING]
> No se puede utilizar el mismo nombre que utilizamos antes.
>

## 0.2.2 Crear una alarma para la facturación. (CloudWatch).

El servicio que vamos a utilizar es **CloudWatch**
Pero antes, tenemos que habilitar un par de cosas, en el apartado de facturación

![image](https://github.com/user-attachments/assets/ad1e10b1-0f43-4d3c-858a-8e77df93c7eb)

Una vez dentro, me aparece pues lo que le debo a AWS (*Porque primero hice el clúster y luego todo esto 24/09/2024*)
![image](https://github.com/user-attachments/assets/33d3e12e-7516-4953-98bf-0912a68613dd)

Para ver a más detalle de donde sale eso, nos vamos al apartado de facturas:

![image](https://github.com/user-attachments/assets/0169569e-329c-4df8-ad19-f45f50475081)

>[!IMPORTANT]
> A mi, no me aparece el apartado "Billing Preferences", teniendo la página en español.
> La he cambiando en inglés, y si aparece. Y desde esa página he vuelto a cambiar el idioma.
>

![image](https://github.com/user-attachments/assets/6898eba6-3e44-4b47-b6be-1f11d2f24776)

Voy a darle a "editar" en el apartado de "Preferencias de entrega de facturas".

![image](https://github.com/user-attachments/assets/018b8a5e-2ab6-4347-803e-8d6abfcd1475)

Efectivamente, queremos que nos mande las facturas por correo.

También, en preferencias de alertas:

![image](https://github.com/user-attachments/assets/383e2bb8-2e8f-4d11-8706-043e7edfec94)

Ahora vamos a buscar el servicio **CloudWatch**

### CloudWatch

![image](https://github.com/user-attachments/assets/fe5d6d88-b6fa-464d-9b9a-d3dbc2072f6f)

Y este es el panel del CloudWatch, vamos a "Alarmas" --> "Todas las alarmas". He marcado arriba a la derecha que el servidor es "Estocolmo". (*No por casualidad*).

>[!WARNING]
> De hecho, **Tenemos que cambiarle el Estocolmo, o el servidor que tengamos y poner el EE.UU. Este (Norte de Virginia)
us-east-1**, Si no, más adelante, a la hora de crear los parámetros que quiere monitorear, no aparecerá el que queremos, aparecerá con el North Virginia, pero no con el Estocolmo.
>

![image](https://github.com/user-attachments/assets/7aac4820-3ba5-4062-b282-f45e8fa8540a)

Creamos la alarma:

![image](https://github.com/user-attachments/assets/016b89b4-c4dc-4e9d-bb78-9358a6731588)

Y seleccionamos la "métrica".

![image](https://github.com/user-attachments/assets/136d829e-f89b-4464-acf3-45583e2b062d)

Buscamos el "Cargo total estimado".

![image](https://github.com/user-attachments/assets/d857a855-df68-43bc-ad80-ade622c6b2c4)

Seleccionamos divisa, el USD y luego "Seleccionar Métrica".

Luego, voy a establacer **sólamente**, que me avise cuando supere los 5€:

![image](https://github.com/user-attachments/assets/f0082dbc-4855-47d7-9552-393b8aa4868b)

A continuación, tengo que crear el "Topic".

![image](https://github.com/user-attachments/assets/7e909bb3-1694-47b2-971c-a907b0c43c5e)

No voy a modificar nada más, le doy a siguiente.

![image](https://github.com/user-attachments/assets/92e15e51-f443-42ce-a8e3-1157256bfcd8)

Le doy un nombre a la alarma, y una descripción. Reviso en la vista previa y le doy a crear.

Nos tiene que haber llegado un correo a mi cuenta de correo del AWS, para confirmar:

![image](https://github.com/user-attachments/assets/63e886b4-4ded-4035-9882-07cedb139e35)

![image](https://github.com/user-attachments/assets/e4dc81c0-003b-4a04-92c3-a67ec6d7c83c)


## 0.2.3 Crear un certificado.

Ahora es turno de crear el certificado.


# 1.0 Crear una VPC (Amazon Virtual Private Cloud)(Parte IMPORTANTE, NO OPCIONAL)
> [!IMPORTANT]
> **ESTO ES UN REQUISITO ESENCIAL PARA PODER CREAR MI PROPIO CLÚSTER**
>

Primero necesitamos crear un VPC
![image](https://github.com/user-attachments/assets/ef3f9767-8f10-490a-8936-84766311986d)

Lo buscamos arriba en la barra de búsqueda y ahora pues nos aparecerá esto:

![image](https://github.com/user-attachments/assets/9e2be0f1-5aac-4240-8634-e4da7a2ba5db)

Pues ahora, vamos a crear una red privada.

![image](https://github.com/user-attachments/assets/559070af-3c4b-4fc1-aaef-0a7d562932d7)

![image](https://github.com/user-attachments/assets/afa01a11-12d5-4545-9154-45f281ac1cd0)

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

![image](https://github.com/user-attachments/assets/90ee6f9a-a470-456d-ba18-06e6e034bbb5)

También se ve muy pequeñito así que lo voy a desplegar/abrir para que se vea un poco mejor.

![image](https://github.com/user-attachments/assets/5f2c4978-9ddf-4901-8628-c7a6b4867f7e)

Y así ya se ve mejor, voy a darle al apartado de "Mapa de recursos".

![image](https://github.com/user-attachments/assets/83b3882c-55eb-4821-b4cb-9e072456f81d)

Voy a ponerle un nombre, porque sencillamente no lo tiene, como podemos comprobar.

![image](https://github.com/user-attachments/assets/9bdd8e5e-ff4f-4839-ac80-2de82c232038)


![image](https://github.com/user-attachments/assets/f1a9d52e-8d80-4528-b736-312cf7a54e3b)

Como podemos comprobar, ya se ha cambiado:

![image](https://github.com/user-attachments/assets/cf2f678d-2574-41eb-840e-91c768a1725c)

El resto de cosas, como gateway, etc. De momento, no las vamos a tocar. 

## 1.1 Vamos a crearlo crear nosotros una VPC propia.

![image](https://github.com/user-attachments/assets/c1e05ea4-6668-403f-909f-a4ca357daeea)

Ahora, nos mandará a esta otra pestaña, **voy a seleccionar VPC y más**:

![image](https://github.com/user-attachments/assets/d1307197-2783-45e3-b731-153ca128238d)

Vamos a darle un 
- Nombre
- La IP por defecto, si no le ponemos nosotros una
- NO vamos a utilizar IPv6
- Número de zonas de disponibilidad (AZ), por defecto, me viene 2, yo voy a seleccionar 3
- Poner a 0 las subredes privadas, solo públicas y están puesto a 3.


![image](https://github.com/user-attachments/assets/b87c670e-5bee-49f4-b842-5ddfcf9be0ee)

![image](https://github.com/user-attachments/assets/2d612b1b-8e49-45e2-bb25-1027cd6d22ea)

Y la creamos, abajo del todo botón naranja:

![image](https://github.com/user-attachments/assets/cfb57016-cc2c-47e3-9076-e627aaa7b9ff)


![image](https://github.com/user-attachments/assets/25dd670a-00fb-4ba2-a983-0b000bdd08e7)

Si quisieramos eliminarla, tan solo necesitamos:

![image](https://github.com/user-attachments/assets/d5b1333a-0473-4fe4-abaf-30ceda0a2b30)

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

![image](https://github.com/user-attachments/assets/f9e670ad-2f01-4031-989a-c33f6710df4c)

Voy a editar la subred 10.0.0.0 

![image](https://github.com/user-attachments/assets/257ee6cc-d9ed-44cf-bd17-21a67c25409f)

Y habilitamos el "Enable auto-assign public IPv4 address, abajo del todo le damos a guardar.

![image](https://github.com/user-attachments/assets/3353921f-685e-45d2-be16-dadcf0748ba1)

# 2.0 Crear el clúster con EKS

Nos vamos arriba a la barra de búsqueda y buscamos por EKS:

![image](https://github.com/user-attachments/assets/a9aaf45e-b7bc-4b05-bae7-b03663c48ce1)

También podríamos llegar a utilizar ECS:

![image](https://github.com/user-attachments/assets/fb3bcc0e-dc4b-4287-be62-0a5462201ec1)

Pero ahora mismo va a ser el EKS.

![image](https://github.com/user-attachments/assets/dc5c3107-c3f2-42ec-a9f7-42ba1edfcaff)

Así que vamos a darle a crear.

### Parte 1. Creación Clúster (Configurar clúster)

![image](https://github.com/user-attachments/assets/868e23e1-e20c-4505-b79c-a81f7b4cc051)

Lo primero que nos pide es ponerle un nombre al Clúster de Kubernetes y asignarle un rol.

![image](https://github.com/user-attachments/assets/c6e3d2e2-1717-4a5e-a8d0-f0085a592435)

Como no tenemos un rol vamos a crearlo:

![image](https://github.com/user-attachments/assets/04a2b773-d2d5-48c3-9801-5d59971360d8)

y caso de uso, pues EKS - Clúster:

![image](https://github.com/user-attachments/assets/3f5e63c7-1285-44df-9fb4-4fddb9bdc05c)

Le daremos a siguiente:

![image](https://github.com/user-attachments/assets/b8b6eb9a-dab0-41b2-b626-e9c905ae5c53)

y estos son los permisos que va a dar Amazon al rol, si lo desplegamos los podemos ver a detalle, es un archivo JSON:

![image](https://github.com/user-attachments/assets/dbbd26e2-7991-4faf-baa2-c2f9a69eac9c)

Le daremos a siguiente y pondremos el nombre del rol.

![image](https://github.com/user-attachments/assets/d00cce81-4374-4710-9e6b-c5f72fdc77cf)

Lo creamos. Vamos a volver a la página de creación del clúster, refrescamos la parte de roles y simplemente lo seleccionamos.

![image](https://github.com/user-attachments/assets/abf2ee01-aeec-4eda-b00a-a5e206b5d143)

El resto no lo pienso tocar:

- Configuración de la versión de Kubernetes
- Acceso al clúster
- Cifrado de secretos
- Etiquetas (0)

### Parte 2. Creación Clúster (Especificar redes)

Al dar siguiente, vamos a especificar las redes:

![image](https://github.com/user-attachments/assets/76936fa9-bba3-4e5a-8425-59fc9a4c3295)

y en cuanto a las subredes pues elegimos las que correspondan a ese VPC. También añadimos un SecurityGroup, el por defecto.

![image](https://github.com/user-attachments/assets/7b9a5d68-3170-4d68-b441-0dc4f9b40704)

y el acceso va a ser SOLO público.

![image](https://github.com/user-attachments/assets/e8594105-d864-4bbc-9665-a50a02db447c)

### Parte 3. Creación Clúster (Configurar la observabilidad)

Ahora estamos en esta otra pantalla:

![image](https://github.com/user-attachments/assets/89411d24-30a1-403b-b328-58109f39ae0a)

En observabilidad no vamos a hacer nada.

![image](https://github.com/user-attachments/assets/0ee057a6-c88f-438e-a6f5-c1bf7d3a6aca)

Tampoco vamos a habilitar ninguna opción de "Registro del plano de control"

### Parte 4. Creación Clúster (Seleccionar complementos)

Le damos a siguiente y ya estamos en el Paso 4, seleccionar complementos:

> Los complementos de Amazon EKS proporcionan una lista seleccionada de software operativo que se puede habilitar en su clúster. Todo el software incluye los últimos parches de seguridad y correcciones de errores y AWS lo valida para trabajar con EKS. Los complementos facilitan el aprovisionamiento de un clúster con las herramientas operativas necesarias para que pueda >comenzar a ejecutar sus aplicaciones.
> 
> - De forma predeterminada, los complementos que requieran acceso a otros servicios de AWS intentarán utilizar los permisos asociados al rol de IAM del nodo de trabajo. Como práctica recomendada, puede utilizar roles de IAM para las cuentas de servicio para asociar un rol de IAM a un complemento de EKS que requiera permisos de IAM. A continuación, ya no tendrá que proporcionar permisos extendidos al rol de IAM del nodo para que el complemento pueda llamar a las API de AWS. Puede transferir un rol de IAM al complemento como parte de su configuración al iniciarlo o en cualquier momento como una actualización.
> - Cuando se utilizan roles de IAM para cuentas de servicio, la relación de confianza se establece en el clúster y la cuenta de servicio, de modo que cada combinación de clústeres y complementos requiere un rol único.
>

![image](https://github.com/user-attachments/assets/a0286034-6a9e-449b-8987-32a25ee95b0b)

### Parte 5. Creación Clúster (Configurar las opciones de complementos seleccionados)

En esta parte es para configurar lo que antes hemos añadido de complementos, los voy a dejar por defecto:

![image](https://github.com/user-attachments/assets/99524350-26c2-498e-8e14-c9789e05fdc5)

### Parte 6. Creación Clúster (Revisar y crear)

Vamos a ver que tengamos todo como queríamos y lo creamos.

Ya está el clúster creado, ahora vamos a esperar que termine de "crearse".
![image](https://github.com/user-attachments/assets/14a6d849-1873-44dc-ad27-4c4c93d03c98)

Refrescamos la página hasta que aparezca, activo:

![image](https://github.com/user-attachments/assets/da350e42-fb10-438a-902d-e0e7f7453fc1)


## 2.1 Crear un nodo.

Ahora nos toca crear los nodos del clúster:

![image](https://github.com/user-attachments/assets/c4416a51-c67a-498a-b3d1-ffea7352efb0)

![image](https://github.com/user-attachments/assets/fa474902-1580-496c-95a3-73dc02ff3de0)

Y bajamos hasta llegar a Grupos de Nodo, **vamos a crear un Group Node**

![image](https://github.com/user-attachments/assets/f2d0814a-dd5b-4575-a67b-116f22aaab19)

y vamos a necesitar crear otro Rol, en nuestro caso es un EC2.

### Parte 1. Seleccionar entidad de confianza.

![image](https://github.com/user-attachments/assets/edd52329-3260-4e74-b4e6-6e7a3c1537d2)

![image](https://github.com/user-attachments/assets/2106de0c-6068-42fd-bf01-0ce4a2c29b38)

### Parte 2. Agregar permisos.

En cuanto a los permisos, buscamos este:

- AmazonEKSWorkerNodePolicy
- AmazonEKS_CNI_Policy
- AmazonEC2ContainerRegistryReadOnly

### Parte 3. Asignar nombre, revisar y crear.

Por último, asignarle un nombre.

![image](https://github.com/user-attachments/assets/d81dc888-1399-4371-8639-f5ff08875cc7)

Volvemos a la parte de crear un nodo:

## Parte 1. Crear Grupo de Nodos. Configurar grupo de nodos.

Solo voy a asignarle el nombre y el rol que acabamos de crear.

## Parte 2. Crear Grupo de Nodos. Establecer la configuración informática y de escalado.

> [!WARNING]
> Esta parte es importante y afecta directamente al precio:
>

![image](https://github.com/user-attachments/assets/99f45bb1-2452-489f-82dd-6df6ae4245a7)

![image](https://github.com/user-attachments/assets/b34c6647-80e8-4ab8-a913-dc8f7c75af73)


## Parte 3. Crear Grupo de Nodos. Especificar redes.

No tiene mucha ciencia, seleccionamos nuestras subredes.

## Parte 4. Crear Grupo de Nodos. Revisar y crear.

Y nada, revisamos, creamos y a esperar un rato.

**En mi caso, me da este error**

![image](https://github.com/user-attachments/assets/270fed85-107e-476d-9b4a-7e2d9b17224f)

Bastante sin sentido, porque antes hemos habilitado la opción esa de la subred.

Básicamente tengo que volver al VPC, luego buscar las 3 subredes y comprobar que las 3 tengan la opción esa, habilitada.

Así que eso, borramos el grupo y lo creamos de nuevo.

![image](https://github.com/user-attachments/assets/90f56e64-8903-44d6-8e5b-74ee90aa7158)

# 3. Usar el CloudShell de AWS para agregar el nodo al clúster.

![image](https://github.com/user-attachments/assets/e4aa74dd-843d-42b2-ba57-65b675f34695)

tarda un poco:

![image](https://github.com/user-attachments/assets/6ae3d900-9044-487e-a6c7-94c6df659e0b)

El CLI es diferente al de Linux, si hago un `aws help` me aparecerán pues todos los comandos, para salir de allí tengo que presionar `**Q**`

```
aws eks update-kubeconfig --name cluster-ilian-primero --alias prueba-ilian
```

en "name" va el nombre del cluster. Ahora hacemos un 

```
kubectl get nodes
```

![image](https://github.com/user-attachments/assets/37670426-5359-429a-9192-755b678a57c9)

# 4. Detener la máquina.

>[!WARNING]
>Cargos por el clúster de EKS:
>
Amazon te seguirá cobrando una tarifa fija por cada clúster de EKS que tengas activo, incluso si no tienes nodos en funcionamiento. El costo de un clúster de EKS es aproximadamente 0,10 USD por hora, lo que equivale a unos 72 USD al mes por clúster.
>

Amazon EKS no permite detener un clúster directamente, pero puedes eliminarlo o detener los nodos de trabajo (EC2) asociados.

```
aws eks delete-cluster --name cluster-ilian-primero
```

como el comando es de literalmente eliminar el clúster, me avisa que el comando es peligroso:

![image](https://github.com/user-attachments/assets/e213f9ac-36d9-475a-bdae-8c7db435c1d8)
