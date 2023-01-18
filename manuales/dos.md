
# Apache HTTP server benchmarking tool


## Instalación
```
apt-get install apache2-utils
```


## Ejecución de las peticiones en masa
```
ab -g <ruta_fichero_salida> –n <numero_peticiones> –c <peticiones_concurrentes> <IPservidorWeb/URLservidorWeb>
```

Ejemplo:
```
ab -g resultado.csv –n 200 –c 15 http://www.example.com/
```

## Mostrar gráfico del test (gnuplot)
### Instalar gnuplot
```
sudo apt-get install gnuplot
```
### Crear script
```
a
b
c

```
---

[manual oficial](https://httpd.apache.org/docs/2.4/programs/ab.html)
