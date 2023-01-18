
## Instalación
```
sudo apt-get install suricata -y
```

## Comprobar estado del IDS
```
systemctl status suricata
```
Inicialmente debería aparecer como _disabled_.

## Descarga, descompresión y limpieza de reglas de la comunidad
```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz;tar zxvf emerging.rules.tar.gz;rm emerging.rules.tar.gz
```

## Mover directorio de reglas a su destino final
```
sudo mv rules /var/lib/suricata/
```

## Creación de regla de detección de ping

### Definición de la regla

```
alert icmp any any -> $HOME_NET any (msg:"Intento de conexión ICMP"; sid:1000002; rev:1;)
```

| acción|descripción|
| ------------- |:-------------:|
|alert| Generar una alerta|

| protocolo|descripción|
| ------------- |:-------------:|
|icmp| Ping|

| IP_origen|descripción|
| ------------- |:-------------:|
|any| cualquiera |

| puerto_origen|descripción|
| ------------- |:-------------:|
|any| cualquiera |

| IP_destino|descripción|
| ------------- |:-------------:|
|$HOME_NET| red/es especificadas en el archivo  /etc/suricata/suricata.yaml |

| puerto_destino|descripción|
| ------------- |:-------------:|
|any| cualquiera |

__msg:__ Mensaje de alerta <br>
__sid:__ Identificador <br>
__rev:__ Versión <br>

### Creación del archivo que contiene la regla
```
echo 'alert icmp any any -> $HOME_NET any (msg:"Intento de conexión ICMP"; sid:1000002; rev:1;)' > /var/lib/suricata/rules/ping.rules
```


## Creación de regla de detección de intento de conexión SSH

### Definición de la regla

```
alert tcp any any -> $HOME_NET 22 (msg:"SSH connection attempt"; sid:1000003; rev:1;)
```

| acción|descripción|
| ------------- |:-------------:|
|alert| Generar una alerta|

| protocolo|descripción|
| ------------- |:-------------:|
|tcp| Transport Control Protocol|

| IP_origen|descripción|
| ------------- |:-------------:|
|any| cualquiera |

| puerto_origen|descripción|
| ------------- |:-------------:|
|any| cualquiera |

| IP_destino|descripción|
| ------------- |:-------------:|
|$HOME_NET| red/es especificadas en el archivo  /etc/suricata/suricata.yaml |

| puerto_destino|descripción|
| ------------- |:-------------:|
|22| puerto ssh por defecto |

__msg:__ Mensaje de alerta <br>
__sid:__ Identificador <br>
__rev:__ Versión <br>

### Creación del archivo que contiene la regla
```
echo 'alert tcp any any -> $HOME_NET 22 (msg:"SSH connection attempt"; sid:1000003; rev:1;)' > /var/lib/suricata/rules/ssh.rules
```


## Creación de regla de detección de denegación de servicio (DoS) por el puerto 80

### Definición de la regla

```
alert tcp any any -> $HOME_NET 80 (msg:"Potential DDoS por el puerto 80"; flags: S,12; threshold: type both, track by_dst, count 500, seconds 5; classtype:misc-activity; sid:6;)
```

| acción|descripción|
| ------------- |:-------------:|
|alert| Generar una alerta|

| protocolo|descripción|
| ------------- |:-------------:|
|tcp| Transport Control Protocol|

| IP_origen|descripción|
| ------------- |:-------------:|
|any| cualquiera |

| puerto_origen|descripción|
| ------------- |:-------------:|
|any| cualquiera |

| IP_destino|descripción|
| ------------- |:-------------:|
|$HOME_NET| red/es especificadas en el archivo  /etc/suricata/suricata.yaml |

| puerto_destino|descripción|
| ------------- |:-------------:|
|80| puerto http por defecto |

__msg:__ Mensaje de alerta <br>
__count:__ Número de peticiones <br>
__seconds:__ Intervalo de tiempo <br>
__sid:__ Identificador <br>

### Creación del archivo que contiene la regla
```
echo 'alert tcp any any -> $HOME_NET 80 (msg:"Potential DDoS por el puerto 80"; flags: S,12; threshold: type both, track by_dst, count 500, seconds 5; classtype:misc-activity; sid:6;)' > /var/lib/suricata/rules/dos.rules
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

## Ubicación de los logs de suricata
```
cd /var/log/suricata
```
