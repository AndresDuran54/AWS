# EBS (Elastic Block Store)

El servicio EBS (Elastic Block Store) en AWS es un servicio de almacenamiento de bloques que proporciona almacenamiento persistente para instancias de Amazon EC2 (Elastic Compute Cloud). En resumen, EBS te permite crear volúmenes de almacenamiento que puedes adjuntar a tus instancias EC2 para almacenar datos de forma persistente incluso después de que se detengan o se terminen las instancias.

Algunas características importantes de EBS incluyen:

1. **Durabilidad**: Los datos almacenados en los volúmenes de EBS se replican automáticamente dentro de su zona de disponibilidad para proporcionar una alta durabilidad y protección contra fallas de hardware.

2. **Disponibilidad**: Los volúmenes de EBS se pueden configurar para estar disponibles en una sola zona de disponibilidad o para ser accesibles desde múltiples zonas de disponibilidad dentro de una región.

3. **Escalabilidad**: Puedes ajustar el tamaño y el tipo de los volúmenes de EBS según tus necesidades de almacenamiento y rendimiento, y puedes crear múltiples volúmenes y adjuntarlos a una instancia EC2 según sea necesario.

4. **Snapshots**: Puedes crear snapshots (instantáneas) de tus volúmenes de EBS, lo que te permite hacer copias de seguridad de tus datos y restaurarlos fácilmente en caso de pérdida o corrupción.

5. **Tipos de almacenamiento**: EBS ofrece varios tipos de almacenamiento, incluyendo SSD (Solid State Drive) para aplicaciones que requieren un alto rendimiento y HDD (Hard Disk Drive) para aplicaciones con necesidades de almacenamiento de datos a gran escala pero con menor rendimiento requerido.

En resumen, EBS es un componente fundamental en la infraestructura de AWS que proporciona almacenamiento persistente y escalable para tus aplicaciones en la nube, lo que te permite gestionar tus datos de forma eficiente y segura.