# Proyecto: Páginas de Apostilla por Estado (SEO Local)

## Objetivo

Clonar la página principal de apostilla de diplomas para crear páginas **casi idénticas** dedicadas a cada estado de EE.UU. El objetivo es que Google indexe cada página de forma independiente con las keywords correctas del estado correspondiente, mejorando el posicionamiento orgánico por estado.

**Sitio:** apostillalegatille.com  
**Servicio:** Apostilla, Legalización y Traducción de documentos de USA  
**Idioma:** Español (es-MX)

---

## Estado de las páginas

### Estados existentes (ya publicados)
| Estado | URL base |
|---|---|
| Texas | `/texas/` |
| Florida | `/apostillar-documento-en-florida/` |
| California | `/california/` |
| New York | `/new-york/` |
| Illinois | `/illinois/` |
| Arizona | `/arizona/` |
| North Carolina | `/north-carolina/` |
| Georgia | `/georgia/` |
| Louisiana | `/louisiana/` |

### Páginas creadas (nuevas)
| Estado | Archivo | URL objetivo |
|---|---|---|
| Washington | `Washington_Diploma.html` | `/washington/apostilla-diploma/` |

---

## Arquitectura técnica

### Page Builder
La página usa **Bold Page Builder** (plugin `bold-page-builder` v5.6.9) con clases `bt_bb_*`. No usa bloques nativos de Gutenberg para el layout.

### Compatibilidad Gutenberg
Para introducir el contenido en el editor Gutenberg de WordPress:
1. El archivo generado está envuelto en un bloque **Custom HTML** de Gutenberg
2. Formato: `<!-- wp:html --> [contenido] <!-- /wp:html -->`
3. En WordPress: Crear nueva página → Agregar bloque "HTML Personalizado" → Pegar el contenido del archivo

> **Importante:** El tema Applauz y el plugin Bold Page Builder deben estar activos en el sitio WordPress para que los estilos `bt_bb_*` funcionen correctamente.

---

## Proceso para clonar a un nuevo estado

### Paso 1: Tomar el archivo de referencia
Usar `index.html` (página de Texas) como plantilla base.

### Paso 2: Ejecutar substituciones
Las substituciones siguen este orden (de más específico a más genérico):

#### URLs (cambiar primero)
```
/texas/apostilla-diploma-2/  →  /[estado]/apostilla-diploma/
/texas/                      →  /[estado]/
apostilla-de-diploma-en-texas  →  /[estado]/apostilla-diploma/
```

#### Ciudades de oficinas
| Texas | Nuevo estado |
|---|---|
| Houston, Texas | [Ciudad principal], [Estado] |
| Austin, Texas | [Capital], [Estado] |
| The Woodlands | [Ciudad secundaria] |
| 9595 Six Pines Dr | [Dirección real o genérica] |

#### Frases con el estado
Buscar y reemplazar todas las ocurrencias de:
- "en Texas" → "en [Estado]"
- "de Texas" → "de [Estado]"
- "Apostilla de Diploma en Texas" → "Apostilla de Diploma en [Estado]"
- "Apostilla en Texas" → "Apostilla en [Estado]"
- "documentos de Texas" → "documentos de [Estado]"
- "Notario de Texas" → "Notario de [Estado]"
- "apostillado en Texas" → "apostillado en [Estado]"
- "Guía básica de documentos de Texas" → "Guía básica de documentos de [Estado]"

#### Alt texts e imágenes
- Actualizar atributos `alt=""` y `title=""` en imágenes
- Las URLs de imágenes del CDN se mantienen igual (son imágenes compartidas del servidor)

#### Footer — agregar el nuevo estado
Agregar `<li>` con el nuevo estado en la lista "Apostilla Urgente" del footer.

### Paso 3: Naming del archivo
```
[NombreDeEstado]_Diploma.html
```
Ejemplos: `Nevada_Diploma.html`, `Colorado_Diploma.html`

### Paso 4: Configurar en WordPress
1. Crear nueva página en WordPress con slug: `/[estado]/apostilla-diploma/`
2. En el editor Gutenberg, agregar un bloque "HTML Personalizado"
3. Pegar el contenido completo del archivo `.html`
4. Configurar en SEO plugin (SEO Ultimate Pro o Yoast):
   - Title: `Apostilla Diploma [Estado] - Servicio de apostillado en [Estado]`
   - Meta description: `Obtener tu Apostilla de Diploma en [Estado] nunca fue tan sencillo. Manda solicitud, envía documentos y recibe tu Apostilla`
   - Canonical URL: `https://apostillalegatille.com/[estado]/apostilla-diploma/`

---

## Keywords principales por tipo de documento

Cada clon de página se enfoca en un tipo de documento. El patrón de nombre es:

| Tipo de documento | Archivo |
|---|---|
| Diploma / Título | `[Estado]_Diploma.html` |
| Acta de Nacimiento | `[Estado]_Acta_Nacimiento.html` |
| Acta de Matrimonio | `[Estado]_Acta_Matrimonio.html` |
| Antecedentes FBI | `[Estado]_FBI.html` |

---

## Casos especiales — qué NO cambiar

Al hacer las substituciones, hay ocurrencias de "Texas" que deben **permanecer intactas**:

1. **URLs de imágenes del CDN** — Los nombres de archivo físicos en el servidor (`/wp-content/uploads/2019/09/...-texas-movil.jpg`) no se modifican. Son imágenes compartidas entre todas las páginas.

2. **Lista de oficinas en el FAQ** — El texto "California, Arizona, Texas, Florida, North Carolina..." lista todos los estados donde hay oficinas. Este texto es genérico y correcto para todas las páginas.

3. **Dropdown de estados del formulario** — El dropdown contiene los 50 estados de USA incluyendo "Texas" como opción. No se elimina esa opción.

---

## Reglas de diseño obligatorias

### Sección de instrucciones para sacar una apostilla
Siempre que una página incluya una sección con instrucciones o pasos de cómo solicitar/obtener una apostilla, esa sección **debe tener dos columnas**:

- **Columna izquierda (`width="2/3"`)** — título, texto con los pasos e instrucciones, y botón de cotización.
- **Columna derecha (`width="1/3"`)** — bloque `[bt_bb_image]` vacío (`image=""`) para insertar una imagen desde la Biblioteca de Medios de WordPress.

Estructura Bold Page Builder a usar:
```
[bt_bb_row column_gap="30"]
  [bt_bb_column width="2/3" align="left" vertical_align="top" padding="15" lazy_load="yes" bb_version="4.7.6"]
    ... título, texto, separadores, botón ...
  [/bt_bb_column]
  [bt_bb_column width="1/3" align="center" vertical_align="middle" padding="normal" lazy_load="yes" bb_version="4.7.6"]
    [bt_bb_image lazy_load="yes" image="" size="full" shape="square" align="center" hover_style="simple" bb_version="5.6.9"][/bt_bb_image]
  [/bt_bb_column]
[/bt_bb_row]
```

---

## Notas técnicas

- **Formulario de cotización:** Usa el plugin `bt_cost_calculator` con Contact Form 7 (CF7 ID: 6973). El mismo formulario sirve para todos los estados.
- **reCAPTCHA:** Site key `6LeBcMcpAAAAAHI7hJNcLVjmMn4E3wA8AtbfAjZE` — se mantiene igual en todas las páginas.
- **WhatsApp widget:** WATI integration con número `15127824341` — se mantiene igual.
- **Call Now Button:** Tel `+18004184981` — se mantiene igual.
- **Scripts externos:** Slick slider, Magnific Popup, jQuery UI — se cargan desde el tema de WordPress, no hace falta incluirlos en el HTML.
