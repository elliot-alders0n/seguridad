## Instalación

```
sudo apt-get install snort
```

## Configuración


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
Comentar(añadiendo un # a la izquierda) o eliminar todas las líneas que empiecen por ___include $RULE_PATH___

## Probar configuración

```
sudo snort -T -i <interfaz> -c <ruta_archivo_configuracion>
```

## Estructura de una regla

acción | protocolo | IP_origen | puerto_origen | -> | IP_destino | puerto_destino

