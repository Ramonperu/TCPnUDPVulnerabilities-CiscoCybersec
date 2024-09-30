# TCP & UDP VULNERABILITIES

Bienvenidos a la guia de seguridad de activos realizada gracias al [curso](https://skillsforall.com/es/course/endpoint-security?courseLang=es-XL) de Cisco de Ciberseguridad 

## TCP

### **Encabezado T**CP

Si bien algunos ataques apuntan a IP, este tema trata los ataques que apuntan a TCP y UDP.

La información de segmento de TCP aparece inmediatamente después del encabezado de IP. En la figura se muestran los campos del segmento TCP y los Flags para el campo de bits de control.

<img align="center" src="/img/1ºimagenn.PNG"  />

Los siguientes son los seis bits de control del segmento TCP:

- **URG** - campo indicador urgente importante
- **ACK** - campo de reconocimiento significativo
- **PSH** - función de empuje
- **RST** - restablecer la conexión
- **SYN** - sincronizar números de secuencia
- **FIN** -no hay más datos del emisor

### Servicios TCP

1. **Entrega Confiable**: TCP asegura la entrega correcta de datos mediante acuses de recibo y retransmisión de datos en caso de errores. Protocolos como HTTP, SSL/TLS, FTP y DNS se benefician de esta confiabilidad.

2. **Control de Flujo**: TCP gestiona el ritmo de envío de datos para evitar que el receptor se vea abrumado. Esto se realiza mediante acuses de recibo de varios segmentos a la vez, en lugar de uno por uno.

3. **Comunicación con Estado**: TCP utiliza un proceso de "tres vías de conexion" (three-way handshake) para establecer una conexión. Esto involucra tres pasos:

<img align="center" src="/img/2ºimagenn.PNG"  />



1. El cliente de origen solicita una sesión de comunicación con el servidor.
2. El servidor reconoce la sesión de comunicación de cliente a servidor y solicita una sesión de comunicación de servidor a cliente.
3. El cliente que inicia reconoce la sesión de comunicación de servidor a cliente.



### Ataques TCP

**TCP SYN Flood**

El ataque TCP SYN Flood se dirige al proceso de tres vías de conexión (handshake) del TCP, abrumando un servidor al explotar su gestión de conexiones:

1. **Solicitud SYN**: El atacante envía múltiples solicitudes TCP SYN con direcciones IP de origen falsificadas al servidor objetivo.
2. **Respuesta SYN-ACK**: El servidor responde a cada solicitud SYN con un paquete SYN-ACK, esperando un paquete ACK correspondiente para completar el handshake.
3. **Denegación de Servicio**: El atacante no responde a los paquetes SYN-ACK, lo que provoca una acumulación de conexiones a medio abrir en el servidor. Esto abruma al servidor, impidiendo que los usuarios legítimos establezcan conexiones.

**TCP Reset**

Un ataque TCP Reset interrumpe abruptamente las comunicaciones TCP en curso entre dos hosts:

1. **Terminación de la Conexión**: Las conexiones TCP generalmente se cierran mediante un intercambio de cuatro vías que implica segmentos FIN y ACK. En cambio, un paquete de restablecimiento (RST) es una forma forzada de terminar una conexión.
2. **Falsificación**: Un atacante puede enviar un paquete RST falsificado a uno o ambos extremos, indicando que deben dejar de usar inmediatamente la conexión TCP

## UDP

El **User Datagram Protocol (UDP)** es un protocolo de transporte en la capa 4 del modelo OSI. A diferencia de TCP, UDP es un protocolo no orientado a la conexión, lo que significa que no establece una conexión antes de enviar datos. Su encabezado es más simple y contiene solo cuatro campos, cada uno de 2 bytes, que son:

<img align="center" src="/img/3ºimagenn.PNG"  />

**Puerto de origen (16 bits)**: Indica el puerto de donde se envían los datos.

**Puerto de destino (16 bits)**: Indica el puerto al que se envían los datos.

**Longitud (16 bits)**: Especifica la longitud total del datagrama UDP, que incluye el encabezado y los datos.

**Checksum (16 bits)**: Se utiliza para la detección de errores en el encabezado y los datos. Aunque no es obligatorio, es recomendado para garantizar la integridad.

### Funciones de UDP

- **Transporte de datos**: Permite el envío de datagramas sin necesidad de establecer una conexión.
- **Transmisión en tiempo real**: Ideal para aplicaciones que requieren una baja latencia, como videoconferencias y juegos en línea.
- **Simplicidad**: Su diseño liviano permite una implementación más sencilla y rápida, lo que es útil en situaciones donde la velocidad es más crítica que la confiabilidad.

### Ataques UDP

UDP no tiene cifrado por defecto, lo que permite que cualquier persona pueda interceptar, modificar y reenviar el tráfico. Aunque se puede agregar cifrado, no está integrado de forma predeterminada. La alteración de los datos en el tráfico afectará el checksum de 16 bits, que es opcional y no siempre se utiliza. 

**Ataques de inundación UDP**: Son más frecuentes. En un ataque de inundación UDP, se consumen todos los recursos de una red. Los atacantes utilizan herramientas como UDP Unicorn o Low Orbit Ion Cannon para enviar una gran cantidad de paquetes UDP, a menudo desde un host falsificado, a un servidor en la subred

Otros ataques 

**Reflejo UDP**: Los atacantes envían solicitudes UDP a servidores de terceros con la dirección IP de la víctima como origen, haciendo que los servidores respondan a la víctima.

**Spoofing de UDP**: Implica la falsificación de la dirección IP de origen en los paquetes UDP, permitiendo el envío de datos maliciosos sin ser detectado.

**Modificación de Datos**: Un atacante puede interceptar el tráfico UDP y alterar los datos. Si se utiliza el checksum, puede recalcularse para que coincida con los datos alterados, ocultando la modificación.

