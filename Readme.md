# Balanceador de carga con una infractuctura LAMP en tres niveles

# Índice
1. [Introducción](#introducción)
2. [Creación de las instancias](#creación-de-las-instancias)
3. [Configuración de cada una de ellas](#configuración-de-cada-una-de-ellas)
4. [Dominio](#dominio)
5. [CMS](#cms)

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
