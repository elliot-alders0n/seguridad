
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
#### Crear un fichero mostrar-grafico.p
```
sudo nano mostrar-grafico.p
```
#### Insertar el siguiente contenido:
```
set terminal png size 600
set output "res.png"
set title "Ataque DoS"
set size ratio 0.6
set grid y
set xlabel "peticiones"
set ylabel "tiempo de respuesta(ms)"
plot "resultado.csv" using 9 smooth sbezier with lines title "test"
```

#### Ejecutar gnuplot
```
gnuplot mostrar-grafico.p
```

#### Mostrar gráfico
(Instalar en caso de ser necesario)
```
display res.png
```
---

[manual oficial](https://httpd.apache.org/docs/2.4/programs/ab.html)
