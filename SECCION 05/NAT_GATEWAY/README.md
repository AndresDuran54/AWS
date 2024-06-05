# Elastic IP

Una **Elastic IP** (EIP) en AWS es una dirección IPv4 estática diseñada para la computación en la nube dinámica. Permite a los usuarios asociar una dirección IP fija con sus instancias EC2 (Elastic Compute Cloud) y otras instancias de la infraestructura de AWS. Esto es útil para manejar cambios en la infraestructura sin afectar la capacidad de los usuarios externos para conectarse a tus servicios.

### Características y beneficios de una Elastic IP

1. **Dirección IP fija**:
   - Una EIP proporciona una dirección IP estática, lo que significa que no cambia a lo largo del tiempo. Esto es útil para aplicaciones que requieren una dirección IP fija, como servidores web y aplicaciones con políticas de firewall estrictas.

2. **Movilidad**:
   - Puedes desasociar una EIP de una instancia y asociarla a otra instancia en la misma región. Esto permite mantener la dirección IP incluso si cambias la instancia subyacente. Es útil para gestionar fallos y realizar mantenimiento sin interrumpir el servicio.

3. **Reasignación rápida**:
   - Las EIP pueden ser reasignadas rápidamente a otra instancia en caso de que una instancia falle. Esto minimiza el tiempo de inactividad y ayuda a mantener la disponibilidad del servicio.

4. **Cobranza eficiente**:
   - AWS cobra por las EIP solo cuando no están asociadas con ninguna instancia. Esto incentiva el uso eficiente de las EIP, evitando el desperdicio de direcciones IP públicas.

### Uso típico de una Elastic IP

1. **Alta disponibilidad**:
   - Puedes asociar una EIP con una instancia EC2 en una zona de disponibilidad y, si esa instancia falla, reasociar la EIP con una instancia de respaldo en otra zona de disponibilidad.

2. **Cambio de infraestructura**:
   - Si necesitas actualizar o cambiar la instancia subyacente que está utilizando una EIP, puedes reasociar la EIP con una nueva instancia sin cambiar la dirección IP externa visible.

3. **Conectividad estática**:
   - Para aplicaciones que requieren una dirección IP fija para la integración con otros servicios o para configuraciones de firewall, una EIP es esencial.

### Limitaciones y consideraciones

- **Límite de EIP**:
  - AWS impone un límite en el número de EIPs que puedes tener por región (normalmente cinco por defecto). Este límite puede ser aumentado mediante una solicitud de aumento de límite.

- **Costos**:
  - Aunque la asignación de una EIP es gratuita cuando está asociada a una instancia en funcionamiento, AWS cobra por cada EIP no asociada para incentivar el uso eficiente.

- **Solo IPv4**:
  - Actualmente, las EIP están disponibles solo para direcciones IPv4. Para IPv6, AWS proporciona direcciones IP estáticas a través de otros mecanismos.

### Ejemplo práctico

1. **Asignar una EIP a una instancia EC2**:
   - Crear o asignar una EIP en la consola de administración de AWS.
   - Asociar la EIP con una instancia EC2 específica. La instancia ahora se puede acceder mediante la dirección IP estática proporcionada por la EIP.

2. **Reasignar una EIP**:
   - Desasociar la EIP de la instancia actual.
   - Asociar la EIP con una nueva instancia. La dirección IP permanece la misma, pero ahora apunta a la nueva instancia.

En resumen, una Elastic IP en AWS es una herramienta poderosa para gestionar direcciones IP estáticas, facilitando la administración de la infraestructura en la nube y mejorando la disponibilidad y flexibilidad de los servicios.

# NAT Gateway

Un **NAT Gateway** en AWS es un servicio administrado que facilita a las instancias en una subred privada conectarse a Internet u otros servicios de AWS, pero evita que estas instancias sean accesibles directamente desde Internet. "NAT" significa Network Address Translation (Traducción de Direcciones de Red), y el NAT Gateway realiza esta función traduciendo las direcciones IP privadas de las instancias a una dirección IP pública.

### Características y beneficios de un NAT Gateway

1. **Servicio administrado**:
   - AWS gestiona la infraestructura subyacente, lo que significa que no necesitas preocuparte por la configuración, administración, o mantenimiento del hardware o del software.

2. **Escalabilidad**:
   - NAT Gateways son escalables automáticamente para manejar grandes volúmenes de tráfico sin intervención manual. Pueden manejar hasta 45 Gbps de tráfico de entrada y salida.

3. **Alta disponibilidad**:
   - Al crear un NAT Gateway, AWS lo implementa en una zona de disponibilidad específica y automáticamente lo repara en caso de fallo. Para una mayor disponibilidad, se recomienda crear un NAT Gateway en cada zona de disponibilidad.

4. **Facilidad de uso**:
   - La configuración y administración de un NAT Gateway se realiza fácilmente a través de la consola de administración de AWS, CLI (Command Line Interface) o API. No requiere configuraciones complicadas ni mantenimiento continuo.

5. **Seguridad**:
   - Las instancias en las subredes privadas no son accesibles directamente desde Internet, mejorando la seguridad al mantener las instancias aisladas de accesos no autorizados.

### Uso típico de un NAT Gateway

1. **Acceso a Internet desde subredes privadas**:
   - Permite que las instancias en una subred privada (sin direcciones IP públicas) puedan acceder a servicios en Internet, como actualizaciones de software, repositorios de paquetes, etc., sin exponer estas instancias a accesos entrantes desde Internet.

2. **Conexión a otros servicios de AWS**:
   - Facilita la comunicación con otros servicios de AWS que requieren acceso a Internet, como Amazon S3, Amazon DynamoDB, o servicios externos a través de puntos finales públicos.

### Cómo funciona un NAT Gateway

1. **Creación del NAT Gateway**:
   - Se crea en una subred pública y se le asigna una Elastic IP (EIP) para permitir la traducción de direcciones.

2. **Configuración de rutas**:
   - Se actualiza la tabla de rutas de la subred privada para que el tráfico de salida con destino a Internet se dirija al NAT Gateway. Esto se logra añadiendo una ruta con el destino `0.0.0.0/0` apuntando al NAT Gateway.

3. **Traducción de direcciones**:
   - Cuando una instancia en la subred privada inicia una conexión a Internet, el tráfico se envía al NAT Gateway. Este traduce la dirección IP privada de la instancia a la dirección IP pública asignada al NAT Gateway y reenvía el tráfico al destino.
   - Las respuestas del tráfico de Internet llegan al NAT Gateway, que luego traduce la dirección IP pública de regreso a la dirección IP privada de la instancia original y reenvía las respuestas a la instancia.

### Ventajas sobre NAT Instances

- **Facilidad de administración**:
  - NAT Gateways no requieren la gestión de instancias EC2, configuración de seguridad o mantenimiento de software.

- **Rendimiento**:
  - Ofrecen mayor rendimiento y capacidad de manejo de tráfico en comparación con una instancia NAT personalizada, con una escalabilidad automática.

- **Disponibilidad y recuperación**:
  - AWS gestiona la alta disponibilidad y la recuperación ante fallos, lo que reduce la carga operativa sobre el usuario.

### Ejemplo práctico

1. **Creación de un NAT Gateway**:
   - En la consola de administración de AWS, navega a la sección de VPC.
   - Selecciona “NAT Gateways” y haz clic en “Create NAT Gateway”.
   - Selecciona la subred pública donde se creará el NAT Gateway y asigna una Elastic IP.

2. **Configuración de la tabla de rutas**:
   - En la sección de VPC, navega a “Route Tables” y selecciona la tabla de rutas asociada con la subred privada.
   - Añade una nueva ruta con el destino `0.0.0.0/0` y selecciona el NAT Gateway como el objetivo.

3. **Verificación**:
   - Las instancias en la subred privada deberían ahora poder acceder a Internet, pero no ser accesibles desde fuera.

En resumen, un NAT Gateway es una solución administrada y escalable para permitir que las instancias en subredes privadas de una VPC accedan a Internet sin exponerlas directamente a conexiones entrantes desde Internet, mejorando la seguridad y la eficiencia operativa.