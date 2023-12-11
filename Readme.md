# Balanceador de carga con una infractuctura LAMP en tres niveles.

# Índice
1. [Introducción](#introducción)
2. [Creación de las instancias](#creación-de-las-instancias)
3. [Configuración de cada una de ellas](#configuración-de-cada-una-de-ellas)
4. [Certificado](#certificado)
5. [Conclusión](#conclusión)

# Introducción

Vamos a realizar una pila LAMP en tres niveles con las máquinas virtuales de AWS, nos encontraremos con:
-La primera máquina en un primer nivel el balenceador de carga.
-La segunda máquina en un segundo nivel dos servidores web
-Y por último la tercera en máquina en tercer nivel que será el servidor de la base de datos.

 
# Creación de las instancias

#### Creación de VPC en AWS

* **Ve al menú de servicios de AWS y elige VPC.**
* **Haz clic en Crear VPC.**
* **Selecciona la opción VPC y más para crear la VPC junto con subredes, tablas de enrutamiento y puertas de enlace automáticamente.**
* **Completa los detalles necesarios, como nombre de la VPC, rango de direcciones IP, etc.**
 
* **Confirma y crea la VPC.**
  
![1](https://github.com/Pablorc222/Balanceador/assets/146434694/c46c400b-c985-47dc-8680-e919ad19c77c)

Necesitamos una IP elástica para asignársela al balanceador. Nos metemos en VPC y en direcciones IP elásticas creamos una y le asignamos un nombre.

![2](https://github.com/Pablorc222/Balanceador/assets/146434694/1b9b41e4-b819-4bcb-821b-b27256956237)

# Creación de las instancias.

Montamos nuestra estructura de cuatro máquinas. El balanceador de carga será una de ellas, dos servidores y web y por último una para nuestra base de datos.
El balanceador será la subred pública y las otras privadas. En el menú de servicios AWS, selecciona EC2. Haz clic en Lanzar instancias para acceder al menú de creación de instancias.
Aquí nombramos nuestras instancias y ponemos las diferentes etiquetas.

![3](https://github.com/Pablorc222/Balanceador/assets/146434694/511739ee-f2fc-4a29-8180-13feb337a4b5)

Quedaría tal que así:

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/f10b3da1-f83f-41db-bb97-c08b19f89ac5)


# Configuración de cada una de ellas

  #### Servidores Apache

  Primero realizamos lo que hacemos siempre al instalar apache, actualizar repositorios, installar apache y módulos php.

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/62db06dd-a4a5-47a9-9ec6-17be7f1e55ef)

  
 Modificamos el archivo
  
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/4cf9649b-4e34-4154-92ed-ff1ba0e83d46)

Activamos manualmente el proxy
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/38522604-4031-469f-97aa-8e678002de5a)


Reiniciamos nuestro servidor apache
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/89c4e1bb-cc2a-4144-86f7-cf7cabfb790f)


 1.Instalamos Git en tu sistema.
 2.Creamos una carpeta para tu aplicación.
 3.En la terminal, navega a la carpeta recién creada.
 4.Clona el repositorio utilizando el comando git clone <URL_del_repositorio>


![image](https://github.com/Pablorc222/Balanceador/assets/146434694/28e9bfac-0582-4755-bfa6-e5258ecc9fec)


Encontramos y abrimos config.php en la aplicación. Buscamos las secciones relacionadas con la configuración de la base de datos. Editamos las variables como DB_HOST, DB_USER, y otros para reflejar la información de conexión correcta.

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/acc94ce2-5bb8-48a7-8f39-aacefc0bd481)


Le damos la propiedad de nuestros archivos al usuario apache y reinicimaos el servicio.

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/8cffe856-ae38-4d99-9b80-ee4d82d02af9)
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/074f2622-f520-4242-b6af-4768515853a2)


Seguimos realizando las tareas que hacemos habitualmente al hacer este tipo de proyectos, instalar el cliente mariadb y copiar la base de datos.

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/432aeb41-96c4-411e-92e7-34c75aa68995)




#### MySQL

Primero actualizamos repositorios e instalamos nuestro servidor mariadb
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/de190473-890e-4b08-9a22-436fc750df61)


Editamos el fichero
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/3586c146-1924-4f20-bb61-f5ae02b9e403)

Copiamos nuestro archivo
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/0a99478a-c45c-4467-9110-669e416f513f)



#### Balanceador

Le añadimos nuestra IP elástica

Instalamos el servidor apache y activamos todos sus módulos
Estas son las proxys que le tenemos que añadir al archivo:
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/2ba1721a-8764-462a-8c1f-65d5c49d574b)


Activamos nuestro módulo y reiniciamos el servicio.
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/b8416778-761e-4207-8ef4-e8f00423a0ee)



# Certificado

Utilizamos My No-IP y creamos nuestro dominio, asociándolo a nuestra IP elástica.
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/629ab8fd-3b6e-460a-893f-b5ac4ca2896a)

# Conclusión

Nuestra tabla quedaría asi:

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/15923c95-d4a6-4167-9c47-a109a17d7abf)
