# Redes1-Practica2_201602916
## Ricardo Antonio Menéndez Tobías
## 201602916


***
## Topología

<img src="./img/topologia.png" width="600x">  

### Subredes
La topología esta compuesta por 3 subredes que estan conectadas entre sí por medio del router. Cada subred esta compuesta de un switch y 1 , 2 o 4 ordenadores.
#### Componentes
    - 1 Router C3725 con 3 interaces FastEthernet
    - 3 Ethernet Switch
    - 5 VPCs
    - 1 Maquina Virtual de Tiny Linux 

Ahora, el router conectara en su interfaz fe0/0 la subred 192.168.12.0/26 teniendo la IP 192.168.12.1 (siendo este el gateway para la respectiva subred). Esta interfaz va conectada a un puerto del Switch, el cual luego se comunicara con 2 VPC teniendo la IP 192.168.12.6 y 192.168.12.9 con un mascara de /26.

En la otra interfaz fe0/1, se realiza lo mismo, con la diferencia que su subred sera: 192.168.12.64/26 teniendo la IP 192.168.12.65 (siendo este el gateway para la respectiva subred). Esta interfaz va conectada a un puerto del Switch, el cual luego se comunicara con 2 VPC teniendo la IP 192.168.12.66-69-71 con un mascara de /26.

Y por ultimo la interfaz f1/0 tiene la subred: 192.168.12.128/26 con la gateway de 192.168.12.129. Esta tiene conectada la VM de Tinylinux con la IP de 192.168.12.136/26

##### Router
    - f0/0: 192.168.12.0/26 192.168.12.1
    - f0/1: 192.168.12.64/26 192.168.12.65
    - f0/1: 192.168.12.128/26 192.168.12.129

##### Switch 1
    - f0/1: 192.168.12.1 (Router)
    - f0/1: 192.168.12.6 (VPC)
    - f0/1: 192.168.12.9 (VPC)

##### Switch 2
    - f1/0: 192.168.12.65 (Router)
    - f1/0: 192.168.12.66 (VPC)
    - f1/0: 192.168.12.69 (VPC)
    - f1/0: 192.168.12.71 (VPC)

##### Switch 2
    - e0/0: 192.168.12.129 (Router)
    - e0/1: 192.167.12.136 (VM-Tiny Linux)

***
## Configuración de Componentes
### Router
Muestra la sección de interfaces:  
 ```sh ip interface brief```  
 Muestra la configuración de rutas:    
 ```sh ip route```    
 entra al modo de configuración:  
 ```configure terminal```  
 entra a la interfaz Fastethernet seleccionada para su configuración:  
 ```int f(0/1)/(0/1)```  
 *Se define la IP de la interfaz y su mascara de subred:  
 ```ip add 192.168.12.(1/65/129) 255.255.255.192```  
 Establecer velocidad de transferencia:  
 ```speed 100```  
 Establecer tipo de transmisión:  
 ```full-duplex```  
 Se prende la maquina:  
 ```no shu```  
 Salida de la edición de la interfaz:  
 ```exit```  
 Salida de la edición del modo de configuración (o si se desea configurar otra interfaz regresar al comando *):  
 ```exit```  
 Copiar la configuración y ejecutarla en arranque:  
 ```copy run start```  
### VPC
Setear la IP y mascara de la maquina  
``` ip 192.168.12.X 255.255.255.192 ```  
Setear el DNS de la maquina  
``` ip dns 192.168.12.X (siendo X la ip del gateway) ```  
Guarda la configuración
``` save ```

### VM TinyLinux
Se debe iniciar la maquina, acceder al panel de control y network.

En este menu procedemos a ingresar la IP de la maquina _192.168.12.136_ , marcara de subred de _255.255.255.192_, broadcast _192.168.12.192 y el gateway _192.168.12.129_.

<img src="./img/pc.jpg" width="600">


***
## Anexos
### Agregar Router y Maquina Virtual
Acceder al menu de configuración  
<img src="./img/Conf.PNG" width="600x"> 
Agregar al router dandole todo a next    
<img src="./img/AddRouter.PNG" width="600x">   
Agregar la maquina virtual  
<img src="./img/Conf2.PNG" width="600x">   

### Configurar Router
Se prende el router, y le damos doble click para acceder a la consola del router via SolarPutty.   
<img src="./img/rf1.PNG" width="600x"> 
<img src="./img/rf2.PNG" width="600x"> 

### Configurar VPC
Se prende la maquina, y le damos doble click para acceder a la consola del router via SolarPutty.   
<img src="./img/VPCConfig.PNG" width="600x"> 

***
## Vocabulario  
### Router
El router es un dispositivo encargado de enrutar o interconectar dispositivos en la red a la que este pertenece. Este establece una ruta que destinará a cada paquete de datos dentro de una red informática.  

 
<img src="https://www.linksys.com/images/product_webdam/89485728/renditions/webdam.web.372.372.jpeg" width="250x">

Esto lo hace almacenando los paquetes recibidos y procesa el origen y destino de estos. Una vez procesados estos datos, se reenvia el paquete creando un encaminamiento con destino al anfitrión final. Este se encarga en decidir el siguiente salto en función de la tabla de encaminamiento, la cual es generado por algoritmos de Dijkstra.

#### Partes Físicas
    - Puertos de entrada
    - Entrada de conmutación
    - Puertos de salida
    - Procesador de encaminamiento
    
### Switch
Es un dispositivo digital lógico de interconexión de equipos que es de la parte de la capa de enlace de datos del modelo OSI. Este se encarga de interconectar dos o más hosts de manera similar a los puentes de red, pasando de un segmento a otro de acuerdo con la dirección MAC de destino de las tramas en la red y eliminando la conexión una vez terminada esta.  

<img src="https://brain-images-ssl.cdn.dixons.com/6/6/21429266/u_21429266.jpg" width="250x">

### VPC
El termino VPC viene de Virtual Private Cloud, donde se refiere a un conjunto computacional configurable por demanda en una nube. En el caso de este ejercicio, es una "nube privada" creada con los recursos del ordenador. 

### Maquina Virtual
Es un software que simula un sistema de computación y puede ejecutar programas como si fuese una computadora real. En el caso de la práctica se utilizó Tiny Linux, una distribución de Linux que reduce el kernel del mismo a lo mas básico para poder usarse en dispositivos con recursos muy muy bajos.