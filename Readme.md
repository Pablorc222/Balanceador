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
Aquí nombramos nuestras instancias y ponemos las diferentes etiquetas. Quedaría tal que así:

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/58441371-035f-4ad7-a589-811ac8cb31ce)

Estas son las reglas de entrada de cada una de las máquinas:
Balanceador:
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/3acd0d8a-fc7b-44c6-8aa0-95d0837a3be8)

Webs:
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/fda72e21-2f16-43fb-894c-c4b4e63ce743)

Mysql:
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/4d2311f0-95ae-4246-a84e-10b204a4f778)


# Configuración de cada una de ellas

  #### Balanceador

  Primero realizamos lo que hacemos siempre al instalar apache, actualizar repositorios, instalar apache y módulos php.

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/1bc9bc50-fc5c-4020-bee6-1ac6703dbd62)
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/25f8f999-6952-4688-b965-46329fdd8b79)
  
 Modificamos el archivo
  
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/4cf9649b-4e34-4154-92ed-ff1ba0e83d46)
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/826d1270-b653-4553-9271-1525adcfda74)


Activamos manualmente el proxy
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/7fc465fb-b25d-4e57-8666-df35bc85a1fd)
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/d7fd97c9-1be5-4190-ba3c-7b324830f728)

Le añadimos nuestro dominio al balanceador:
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/8afa258a-4008-4d09-908c-798c48da0681)

#### MySQL

Primero actualizamos repositorios e instalamos nuestro servidor mariadb
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/eaabc5c6-8244-44ef-865b-fecc28a33a1d)



Editamos el fichero
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/3586c146-1924-4f20-bb61-f5ae02b9e403)

Copiamos nuestro archivo
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/0a99478a-c45c-4467-9110-669e416f513f)




#### Servidores Webs

* Paso 1: Enviar clave desde el balanceador
scp -i "ruta_de_la_clave.pem" clave_ssh.pem usuario@direccion_ip_instancia:/ruta_en_instancia/

* Paso 2: Conectar vía SSH y configurar grupos de seguridad
ssh -i "ruta_de_la_clave.pem" usuario@direccion_ip_instancia

* Configurar grupos de seguridad para permitir acceso a Internet
* Descargar Apache y el paquete Git
sudo apt-get update
sudo apt-get install apache2 git

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/d4ef4eeb-f483-4e74-ac52-edcf7288b897)

Seguidamente hacemos esto:

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/68fff05b-f669-4fc1-967d-f9be4be2f476)

Luego hacemos una copia del fichero 000-default.conf en /etc/apache2/sites-available:

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/8dffb2fe-1de9-45bc-997e-d5e1984a26ec)


Encontramos y abrimos config.php en la aplicación. Buscamos las secciones relacionadas con la configuración de la base de datos. Editamos las variables como DB_HOST, DB_USER, y otros para reflejar la información de conexión correcta.

![image](https://github.com/Pablorc222/Balanceador/assets/146434694/b622e5bd-9229-4356-9281-8fc7f38ac993)

# Certificado

Utilizamos My No-IP y creamos nuestro dominio, asociándolo a nuestra IP elástica.
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/629ab8fd-3b6e-460a-893f-b5ac4ca2896a)

# Conclusión

Nuestra tabla quedaría asi:
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/fb69ae65-c155-41df-8f92-b4c7de632efb)
![image](https://github.com/Pablorc222/Balanceador/assets/146434694/15923c95-d4a6-4167-9c47-a109a17d7abf)

Me ha resultado dificil y muy desastrosa, después de tener todo y probarlo me meto en mis máquinas y me ponen que no puedo hacer a ellas por ssh fallo en el puerto 22, tengo todo bien configurado y antes me iba perfectamente.

