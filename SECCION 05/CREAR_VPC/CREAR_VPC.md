# Crear una VPC con una subred pública y dentro una instancia EC2

1. **Creamos la VPC**:
   - Ni bien se crea se le asigna una Route Table y un ACL.
2. **Creamos una subred**:
   - Creamos una subred y la asociados a nuestra VPC.
3. **Creamos un Internet Gateway para hacer que una subred private sea publica**:
   - Creamos una nueva Route table y la asignamos a nuestra subred, dentro de esa Route table debemos de definir una regla que indique que todo tráfico desde cualquier destino pueda entrar o salir por el intenet gateway que es nuestra puerta de enlace.
4. **Activar en nuestra VPC las siguientes opciones**:
   En una VPC (Virtual Private Cloud) de AWS, las opciones **"Enable DNS resolution"** y **"Enable DNS hostnames"** tienen roles específicos relacionados con la resolución de nombres de dominio dentro de la red.

   ### Enable DNS resolution

   Esta opción, cuando está habilitada, permite que las instancias dentro de la VPC usen el sistema de nombres de dominio (DNS) proporcionado por AWS para resolver nombres de dominio en direcciones IP. En otras palabras, las instancias pueden traducir nombres de dominio como `example.com` a direcciones IP a través del servicio DNS de AWS. 

   - **Si está habilitada**: Las instancias pueden resolver nombres de dominio tanto internos (nombres dentro de la VPC) como externos (nombres de Internet) usando el DNS de AWS.
   - **Si está deshabilitada**: Las instancias no pueden usar el DNS de AWS para la resolución de nombres de dominio.

   ### Enable DNS hostnames

   Esta opción controla si las instancias en la VPC recibirán nombres de host DNS basados en el nombre de dominio de la VPC. 

   - **Si está habilitada**:
   - A las instancias que se lanzan en la VPC se les asigna un nombre de host DNS que se puede resolver en una dirección IP.
   - Esto es útil para identificar y conectar instancias usando nombres de host en lugar de direcciones IP.
   - Las instancias con direcciones IP públicas también recibirán un nombre de host DNS público que se puede resolver en su dirección IP pública.
   - **Si está deshabilitada**:
   - Las instancias no reciben nombres de host DNS. No tendrán un nombre de dominio que las identifique en el DNS de la VPC.

   ### Uso conjunto

   Para que las instancias en una VPC usen nombres de dominio y nombres de host de manera efectiva, a menudo es necesario habilitar ambas opciones:

   - **Enable DNS resolution** permite a las instancias realizar consultas DNS.
   - **Enable DNS hostnames** asegura que las instancias reciban y utilicen nombres de host DNS.

   Por ejemplo, si deseas que una instancia pueda resolver nombres de host a direcciones IP y que las instancias mismas tengan nombres de host DNS asignados, debes habilitar ambas opciones.

   ### Ejemplo práctico

   1. **Enable DNS resolution habilitado y Enable DNS hostnames habilitado**:
      - Una instancia puede resolver `my-instance.amazonaws.com` a una dirección IP.
      - La instancia misma tiene un nombre de host como `ip-10-0-0-1.ec2.internal`.

   2. **Enable DNS resolution habilitado y Enable DNS hostnames deshabilitado**:
      - La instancia puede resolver nombres de dominio externos e internos.
      - Sin embargo, la instancia no tiene un nombre de host asignado, por lo que no puede ser referenciada por un nombre de dominio dentro de la VPC.

   3. **Enable DNS resolution deshabilitado y Enable DNS hostnames habilitado**:
      - Las instancias tienen nombres de host asignados.
      - Pero las instancias no pueden resolver estos nombres (o cualquier otro nombre de dominio) a direcciones IP usando el DNS de AWS.

   4. **Ambas opciones deshabilitadas**:
      - Las instancias no pueden usar el DNS de AWS para la resolución de nombres.
      - Las instancias no tienen nombres de host DNS asignados.

   Estas opciones te permiten configurar cómo las instancias dentro de tu VPC interactúan con el sistema DNS, lo que puede ser crucial para la conectividad y la gestión de recursos en tu infraestructura en la nube.

5. **Habilitar la opción de 'Enable auto-assign publiv IPv4 address' en la subnet**: En una subred (subnet) de una VPC en AWS, las opciones **"Enable auto-assign public IPv4 address"** y **"Hostname type"** tienen funciones específicas que afectan la asignación de direcciones IP y los nombres de host de las instancias lanzadas en esa subred.

   ### Enable auto-assign public IPv4 address

   Esta opción determina si las instancias que se lanzan en la subred reciben automáticamente una dirección IP pública.

   - **Si está habilitada**:
   - Cada instancia lanzada en la subred recibe automáticamente una dirección IP pública además de la dirección IP privada.
   - Esto es útil para instancias que necesitan ser accesibles desde Internet, como servidores web.

   - **Si está deshabilitada**:
   - Las instancias no reciben una dirección IP pública automáticamente. Solo tendrán direcciones IP privadas.
   - Esto es adecuado para instancias que solo necesitan ser accesibles dentro de la VPC, como bases de datos o servicios internos.

   ### Hostname type

   Esta opción controla el tipo de nombre de host DNS asignado a las instancias dentro de la subred. Hay dos valores posibles:

   1. **IP name**:
      - El nombre de host de la instancia se basa en la dirección IP privada.
      - Formato típico: `ip-10-0-0-1.ec2.internal` (para direcciones IP privadas).
      - Si la instancia tiene una dirección IP pública, también recibe un nombre de host basado en esa dirección: `ec2-203-0-113-25.compute-1.amazonaws.com`.

   2. **Resource name**:
      - El nombre de host de la instancia se basa en el ID del recurso de la instancia.
      - Formato típico: `i-1234567890abcdef0.ec2.internal`.

   ### Ejemplo práctico

   1. **Enable auto-assign public IPv4 address habilitada** y **Hostname type: IP name**:
      - Una instancia lanzada en la subred recibe automáticamente una dirección IP pública.
      - El nombre de host basado en IP privada podría ser `ip-10-0-0-1.ec2.internal`.
      - Si se accede desde Internet, el nombre de host basado en IP pública podría ser `ec2-203-0-113-25.compute-1.amazonaws.com`.

   2. **Enable auto-assign public IPv4 address deshabilitada** y **Hostname type: Resource name**:
      - Una instancia lanzada en la subred solo recibe una dirección IP privada.
      - El nombre de host basado en el ID del recurso podría ser `i-1234567890abcdef0.ec2.internal`.

   3. **Enable auto-assign public IPv4 address habilitada** y **Hostname type: Resource name**:
      - La instancia recibe una dirección IP pública automáticamente.
      - El nombre de host basado en el ID del recurso podría ser `i-1234567890abcdef0.ec2.internal`, y para la IP pública se asigna el nombre de host correspondiente.

   4. **Enable auto-assign public IPv4 address deshabilitada** y **Hostname type: IP name**:
      - La instancia solo recibe una dirección IP privada.
      - El nombre de host basado en la IP privada podría ser `ip-10-0-0-1.ec2.internal`.

   Estas configuraciones permiten controlar cómo se asignan las direcciones IP y los nombres de host a las instancias dentro de una subred, lo cual es esencial para la administración y conectividad de la infraestructura en la nube.