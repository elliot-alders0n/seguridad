
## Instalación
```
sudo apt-get install suricata -y
```

## Comprobar estado del IDS
```
systemctl status suricata
```
Inicialmente debería aparecer como _disabled_.

## Descargarse reglas de la comunidad
```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
```

## Descomprimir reglas
```
tar zxvf emerging.rules.tar.gz;rm emerging.rules.tar.gz
```

## Mover directorio de reglas a su destino final
```
sudo mv rules /var/lib/suricata/
```

## Creación de regla de detección de ping

### Definición de la regla

```
alert icmp any any -> $HOME_NET any (msg:"ICMP connection attempt"; sid:1000002; rev:1;)
```



### Backup snort.conf
```
sudo cp /etc/snort/snort.conf /etc/snort/snort.conf.back
```

### Crear archivo de configuración personalizado
```
sudo cp /etc/snort/snort.conf /etc/snort/miconf.conf
```

### Editar archivo de configuración
```
sudo nano /etc/snort/miconf.conf
```

#### Añadir debajo de la línea ipvar HOMENET any:
```
ipvar HOMENET <red>
```
Ejemplo:
```
ipvar HOMENET 192.168.1.0/24
```
#### Eliminar reglas por defecto
Comentar(añadiendo un # a la izquierda) o eliminar todas las líneas que empiecen por ___include $RULE_PATH___ excepto la línea 
<br>
__include $RULE_PATH/local.rules__

## Estructura de una regla
| acción        | protocolo | IP_origen | puerto_origen | -> | IP_destino | puerto_destino
| ------------- |:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|

| acción|descripción|
| ------------- |:-------------:|
|alert| Generar una alerta|
|log| Registrar el evento (log)|
|pass|Ignorar el paquete|
|activate||
|dynamic||
|drop|Bloquear + log|
|reject|drop + envío de respuesta negativa al emisor|
|sdrop|bloquear el paquete (no log)|

| protocolo|
| ------------- |
|tcp|
|udp|
|icmp|
|ip|

### Ubicación de todas las reglas
```
cd /etc/snort/rules/
```

### Editar local.rules
```
sudo nano /etc/snort/rules/local.rules
```
#### Ejemplo: Añadir regla para detección de pings 
```
alert icmp any any -> $HOME_NET any (msg:"Prueba ICMP";sid:1000001;rev:1;)
```

## Probar configuración

### Cargar fichero de configuración
```
sudo snort -T -i <interfaz> -c <ruta_archivo_configuracion>
```
### Ejecutar snort (modo consola)

```
sudo snort -A console -q -i <interfaz> -c <ruta_archivo_configuración> -K ascii
```

## Ubicación de los logs de snort
```
cd /var/log/snort
```


Referencias
---
Snort Workshop : How to Install, Configure, and Create Rules. (2020). YouTube. Retrieved January 11, 2023, from https://youtu.be/8lOTUqfkAhQ. 
