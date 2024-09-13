# Introducción:

# Después de registrarme:

![image](https://github.com/user-attachments/assets/ea9a69cd-3a75-4e78-a60f-6eef584edcb9)

Esta sería la página principal que voy a ver

Para utilizar el EKS (Elastic Kubernetes Service) (El gestor de Kubernetes de Amazon)

# 1.0 Crear una VPC (Amazon Virtual Private Cloud).

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

Ahora nos toca crear los nodos del clúster:

![image](https://github.com/user-attachments/assets/c4416a51-c67a-498a-b3d1-ffea7352efb0)

![image](https://github.com/user-attachments/assets/fa474902-1580-496c-95a3-73dc02ff3de0)
