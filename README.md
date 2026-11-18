# Práctica PHP - Uso de include

En esta práctica he montado un entorno de desarrollo con Nginx y PHP 8.3 usando Docker para probar cómo funciona la instrucción `include` de PHP y así no tener que repetir la misma estructura HTML en todas las páginas.

## Estructura del proyecto

- `docker-compose.yml`: levanta el contenedor con la imagen `webdevops/php-nginx-dev:8.3`, mapea el puerto `8080` al `80` del contenedor y deja configurado Xdebug en el puerto `9003`.
- `.vscode/launch.json`: configuración de VS Code para escuchar las peticiones de Xdebug y poder depurar con breakpoints.
- `app/`: carpeta donde está el código web, montada como volumen en `/app`.

## Cómo funciona el código

La página principal es `app/index.php`, que se encarga de unir las distintas partes de la web:

1. Primero define la variable `$titulo = "Página con includes";` y llama a `include("encabezado.php")`. Como la variable se declara antes del `include`, dentro de `encabezado.php` se puede imprimir directamente en la etiqueta `<title>` de la pestaña del navegador.
2. Después muestra el encabezado principal `<h1>` con el mismo valor de `$titulo`.
3. Por último, carga `pie.html` con otro `include` para mostrar el pie de página y cerrar las etiquetas `</body>` y `</html>`.

## Puesta en marcha

Para arrancar el servidor desde la raíz del proyecto:

```bash
docker compose up
```

Una vez levantado, se comprueba entrando en `http://localhost:8080`. Para pararlo y eliminar el contenedor al terminar:

```bash
docker compose down
```

## Observaciones

Hay que tener cuidado si se viene de la práctica anterior y no se ha parado el contenedor, porque ambos comparten el mismo `container_name` (`php-practicas`) y Docker da conflicto hasta que se detiene o elimina el anterior. Por lo demás, separar la cabecera y el pie en archivos aparte resulta bastante cómodo para mantener el código ordenado.