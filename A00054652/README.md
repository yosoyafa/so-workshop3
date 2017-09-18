### Taller 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Tema:** Llamadas al sistema (syscalls)  
**Correo:** daniel.barragan at correo.icesi.edu.co


### Objetivos
* Conocer y explicar la función de las llamadas al sistema en un sistema operativo

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Aplicativo strace

### Descripción
El tercer taller del curso sistemas operativos trata sobre las llamadas al sistema y su importancia para el sistema operativo

### Actividades

1. Empleando el aplicativo **strace** obtenga 5 llamadas al sistema para uno o varios comandos de linux. Explique por qué los comandos seleccionados emplean las llamadas al sistema encontradas, para ello debe emplear los manuales de Linux en Internet o del sistema operativo (comando **man**). Debe incluir la explicación de los parámetros que reciben las llamadas al sistema encontradas. Consigne capturas de pantalla donde muestre las llamadas al sistema obtenidas (sugerencia: emplear -etrace para filtrar los resultados)
______________________________
  
**Programa ejecutado: ls.**  
  
**execve**: ejecuta el programa.  
El proceso usa este llamado porque necesita una orden para iniciarse.  
Parámetros: filename: nombre del programa a ejecutar.  
            argv: serie de argumentos entre los que está el nombre del programa a ejecutar.  
            envp: cadenas de tipo llave=valor que son pasadas como ambiente al nuevo programa.  
  
**brk**: cambia la locación del program break, que determina el final del segmento de datos del proceso.  
El proceso hace este llamado para saber hasta que dirección de la memoria es que el proceso operará.  
Parámetros: addr: dirección hasta donde se ampliará el segmento de datos del proceso.  
  
**mmap**: crea un nuevo mapping en el espacio virtual de direcciones del proceso que está haciendo el llamado.  
El proceso usa este llamado porque debe ubicar datos de archivos, en memoria.  
Parametros: addr: determina en donde se pone el mapping.  
            length: tamaño del mapping.  
            prot: determina la protección que tendrá el mapping (lectura, escritura, etc).  
            flags: información que se le adiciona al llamado.  
            fd: descriptor de archivo.  
            offset: desfase para ubicar en memoria el mapping (debe ser múltiplo del tamaño de pagina).  
  
**open**: abre un descriptor de archivo dada la ruta de éste.  
El proceso usa este llamado porque debe abrir ciertos archivos que no están en memoria.  
Parameters: pathname: ruta del archivo a abrir.  
            flags: información que se le adiciona al llamado.  
  
**read**: lee los bytes dados y los almacena en un buffer.  
El proceso usa este llamado para pasara los datos del file descriptor a un recurso del sistema.  
Parámetros: fd: descriptor de archivo que determina los bytes a leer.  
            buf: buffer donde se almacenarán los bytes leídos.  
            count: tamaño de bytes a leer.  

**Descripción proceso**: el sistema ejecuta el programa ‘ls’ (execve), luego determina hasta donde se almacenarán los datos que usará el proceso (brk); después se crea un espacio de direcciones del proceso (mmap), para después abrir los archivos que se han pedido (open), y por último, se pasan los datos de estos archivos a memoria (read).  

______________________________

2. Realice la compilación del código fuente adjunto y su ejecución empleando el aplicativo **strace**. Identifique las llamadas al sistema encargadas de enviar y recibir datos a través de la red. A partir de los manuales de Linux en Internet o del sistema operativo explique las llamadas al sistema encontradas y sus parámetros.
______________________________
  
Los llamados al sistema que se usan para enviar y recibir datos de la red son:  
  
**sendto**: envía un mensaje a un socket.  
Parámetros: sockfd: descriptor de archivo del socket que envía.  
            buf: buffer donde se encuentra el mensaje a enviar.  
            len: tamaño del mensaje a enviar.  
            flags: información que se le adiciona al llamado.  
            dest_addr: dirección de destino.  
            addrlen: tamaño de la dirección de destino.  
  
**recvfrom**: recibe un mensaje desde un socket.  
Parámetros: sockfd: descriptor de archivo del socket que está recibiendo.  
            buf: buffer donde se encuentra el mensaje que se recibe.  
            len: tamaño del mensaje que se recibe.  
            flags: información que se le adiciona al llamado.  
            src_addr: dirección de la que el mensaje proviene (fuente).  
            addrlen: tamaño de la dirección de fuente.  
______________________________  

**Nota:** Cuando compile programas tenga en cuenta que estos pueden necesitar la instalación de librerías para su compilación y ejecución.

**Debian**
```
# apt-get install libcurl4-openssl-dev
$ gcc -o curl curl.c -lcurl
```
**CentOS**
```
# yum install libcurl-devel
$ gcc -o curl curl.c -lcurl
```

### Nota

El informe debe ser entregado en formato README.md y debe ser subido a un repositorio de github. El repositorio de github debe ser un fork de https://github.com/ICESI-Training/so-workshop3 y para la entrega deberá hacer un Pull Request (PR) respetando la estructura definida. El código fuente y la url de github deben incluirse en el informe.  

## Referencias

* http://man7.org/linux/man-pages/man2/syscalls.2.html  
* https://jvns.ca/blog/2014/09/18/you-can-be-a-kernel-hacker/
