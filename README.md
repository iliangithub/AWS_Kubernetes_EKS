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
![image](https://github.com/user-attachments/assets/a9aaf45e-b7bc-4b05-bae7-b03663c48ce1)
