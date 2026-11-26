# Búsqueda de amistad aplicando Lógica Difusa

Proyecto Final de **Inteligencia Artificial**. Sistema web que encuentra a las personas más compatibles con el perfil de un usuario para entablar una amistad duradera, usando **lógica difusa** (intersección de conjuntos difusos).

## Integrantes

- Laila Elisa Castillo Castro
- Erik Issay Salazar Camargo
- Fernando Daniel Mora Cerritos
- Isaias Ezequiel Rivera Lopez
- Jorge Brayan Hernandez Simon

## ¿Cómo funciona?

1. El usuario escribe su nombre y responde **15 características** personales usando sliders y menús desplegables con lenguaje cotidiano (por ejemplo, “Muy reservado – Muy sociable”). Cada respuesta se convierte internamente en un valor difuso entre 0 y 1, que el usuario no ve.
2. El sistema compara ese perfil con el **banco de 20 perfiles** precargados.
3. El grado de coincidencia se calcula con la intersección difusa (mínimo) dividida entre la unión (máximo):

   ```
   S(U, P) = Σ min(μU(i), μP(i)) / Σ max(μU(i), μP(i))      i = 1..15
   ```

4. Si la mejor coincidencia alcanza el **umbral de confianza (0.60)**, se muestran hasta 3 perfiles con mayor afinidad. Si ninguno lo alcanza, el sistema informa que no hay coincidencia confiable.

## Características evaluadas

| # | Característica | Captura |
|---|---|---|
| 1 | Edad | Menú (5 rangos) |
| 2 | Personalidad | Slider |
| 3 | Paz interior | Menú |
| 4 | Actividad deportiva | Slider |
| 5 | Gusto por viajar | Slider |
| 6 | Gusto por leer mangas | Slider |
| 7 | Gusto por videojuegos | Slider |
| 8 | Música para relajarme | Slider |
| 9 | Música para animarme | Slider |
| 10 | Apego a la religión | Slider |
| 11 | Apego a la familia | Slider |
| 12 | Gusto por el cine | Slider |
| 13 | Gusto por leer | Slider |
| 14 | Importancia de la amistad | Slider |
| 15 | Cuidado de la salud | Slider |

## Base de datos

La base es **SQLite** (`base_de_datos/amistad.db`). Como GitHub Pages solo sirve archivos estáticos, el navegador la lee con [sql.js](https://github.com/sql-js/sql.js) (SQLite compilado a WebAssembly), sin necesidad de servidor.

| Tabla | Contenido |
|---|---|
| `perfil` | Identificador único (PER-001 a PER-020) y nombre |
| `caracteristica` | Las 15 características, su tipo (menú o slider) y etiquetas |
| `opcion` | Opciones de los menús desplegables con su valor difuso |
| `valor_perfil` | Valor (0 a 1) de cada característica por perfil: 20 × 15 = 300 filas |
| `configuracion` | Umbral de confianza (0.60) |

El diagrama de relaciones está en [`DIAGRAMA_ER.md`](DIAGRAMA_ER.md) y el script completo en [`base_de_datos/schema.sql`](base_de_datos/schema.sql).

## Estructura del repositorio

```
├── index.html              # Aplicación (interfaz y lógica difusa)
├── db_embebida.js          # Copia de respaldo de la base de datos
├── base_de_datos/
│   ├── amistad.db          # Base de datos SQLite
│   └── schema.sql          # Estructura y datos en SQL
├── imagenes/               # Fotos de los perfiles (opcional)
├── DIAGRAMA_ER.md          # Diagrama entidad-relación
└── README.md
```

## Ejecutar el proyecto

**En línea:** abre el enlace del sitio web de arriba.

**Local:** necesita conexión a internet (sql.js se descarga de cdnjs). Se puede abrir `index.html` con doble clic. Para usar directamente `amistad.db` en lugar de la copia de respaldo, levanta un servidor local:

```bash
python -m http.server 8000
# abre http://localhost:8000
```

## Publicar en GitHub Pages

1. Sube todo el contenido del proyecto al repositorio, con `index.html` en la raíz.
2. Ve a **Settings → Pages**.
3. En *Source* elige la rama `main` y la carpeta `/ (root)`, y guarda.
4. En unos minutos el sitio estará en `https://TU-USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/`.

## Fotos de los perfiles

Para mostrar fotos, coloca las imágenes en la carpeta `imagenes/` con el nombre indicado en los archivos `COLOCAR_AQUI_*.txt`. Si falta una imagen, se muestra un círculo con la inicial del nombre.

## Modificar los datos

Si editas `amistad.db` (por ejemplo, para cambiar un perfil o el umbral), regenera `db_embebida.js` para que la copia de respaldo coincida:

```bash
python -c "import base64;open('db_embebida.js','w').write('const DB_B64=\"'+base64.b64encode(open('base_de_datos/amistad.db','rb').read()).decode()+'\";')"
```

## Tecnologías

HTML, CSS y JavaScript · SQLite · sql.js · GitHub Pages

## Referencias

- Zadeh, L. A. (1965). Fuzzy sets. *Information and Control, 8*(3), 338–353.
- Jaccard, P. (1912). The distribution of the flora in the alpine zone. *New Phytologist, 11*(2), 37–50.
- Documentación de sql.js: https://sql.js.org
- Documentación de GitHub Pages: https://docs.github.com/pages
