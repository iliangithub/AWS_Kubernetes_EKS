# Introducción:

# Después de registrarme:

![image](https://github.com/user-attachments/assets/ea9a69cd-3a75-4e78-a60f-6eef584edcb9)

Esta sería la página principal que voy a ver

Para utilizar el EKS (Elastic Kubernetes Service) (El gestor de Kubernetes de Amazon)

# Crear una VPC (Amazon Virtual Private Cloud).

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

Vamos a crearlo crear nosotros una VPC nuestras.

![image](https://github.com/user-attachments/assets/c1e05ea4-6668-403f-909f-a4ca357daeea)

Ahora, nos mandará a esta otra pestaña:

![image](https://github.com/user-attachments/assets/58ea46e2-5d01-4048-a1b1-1ed6dcc1fc27)

vamos a pasar y crear del tirón.

![image](https://github.com/user-attachments/assets/25dd670a-00fb-4ba2-a983-0b000bdd08e7)


# Crear el clúster con EKS
![image](https://github.com/user-attachments/assets/a9aaf45e-b7bc-4b05-bae7-b03663c48ce1)
