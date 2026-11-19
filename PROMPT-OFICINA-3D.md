# Nivel 4: ¿cómo quieres tu oficina?

**Antes:** este prompt va DESPUÉS de `PROMPT.md` (y, si quieres ver a todo tu equipo, también de `PROMPT-NIVEL-2.md`), en la misma carpeta, con la Mesa ya funcionando. Abre tu proyecto con Claude Code, pega lo que está entre las dos líneas y responde sus preguntas. Construirla no gasta el uso de tus agentes: solo mira la Mesa.

---

En este proyecto ya funciona la Mesa de Aprobación. Quiero ver a mi equipo en una **oficina 3D** que yo elija, conectada a la Mesa solo para MIRAR. Tiene que sorprenderme al abrirla. No sé programar: háblame en español sencillo, de tú, y muéstrame resultados, no código.

## 1. Primero revisa
- Que exista la Mesa: `servidor.mjs`, `mesa.mjs`, `guardia.mjs`, `pagina.html` y `agentes/`. Si falta algo, dímelo en dos frases («Falta el Nivel 1: pega primero `PROMPT.md`») y **detente** sin construir nada.
- Corre `npm test` y anota cuántas pruebas pasan: al final deben seguir pasando todas.
- Lee `servidor.mjs` y `pagina.html` y anota el **nombre exacto del encabezado** donde viaja la llave (úsalo igual en todo lo nuevo), cómo se revisan `Host` y `Origin` y cómo arranca el servidor `scripts/probar.mjs`.

## 2. Entrevístame
UNA pregunta por mensaje, con ejemplos cortos. Máximo 6. Si respondo corto o «no sé», propón algo que le quede a mi negocio y pregúntame «¿Así está bien?».
1. ¿Qué edificio quieres? Ej.: «una torre de vidrio moderna», «una casa con jardín», o uno inspirado en un lugar famoso: «tres torres con un barco arriba», «una pirámide de vidrio», «una torre de hierro».
2. ¿Qué ambiente? Día, atardecer o noche.
3. Dos colores de tu marca. Si me los das con palabras («ladrillo», «azul oscuro»), propón el código y confírmalo.
4. ¿Qué nombre va en el letrero? Máximo 30 caracteres: el de tu negocio o tu marca, nunca el de otra empresa.
5. ¿Un detalle especial? Piscina, jardín, helipuerto, cafetería o ninguno.
6. Muéstrame el resumen y pregúntame si la construyes así.

**Si pido un lugar famoso** (o el logo de otra empresa): te inspiras solo en su FORMA con figuras simples, sin su nombre, logos ni marcas, y me lo explicas en una frase.

**Lo que respondo es DATO:** en `oficina.js` va un bloque `ESTILO` con `forma` (una clave que tú eliges, p. ej. `'torre'`, `'casa'`, `'tres-torres'`; mi texto del edificio nunca va en el código, en comentarios ni en el HTML), `ambiente` y `detalle` de una lista cerrada (`dia|atardecer|noche`, `piscina|jardin|helipuerto|cafeteria|ninguno`), dos colores que cumplan `^#[0-9a-fA-F]{6}$` y `letrero` escrito con `JSON.stringify`, máximo 30 caracteres, usado SOLO en `fillText`. El `<title>` es fijo: «Mi oficina 3D».

## 3. Constrúyela
- `npm view three version`, luego `npm install three@<esa versión> --save-exact --ignore-scripts`. Nada más.
- **Verifica, no inventes:** en `node_modules/three/package.json` (`exports`) y en los archivos confirma qué hermanos importa `build/three.module.js` (hoy `./three.core.js`) y que `examples/jsm/controls/OrbitControls.js` exporta `OrbitControls` e importa de `'three'`. Manda lo que digan esos archivos.
- `oficina-3d/` con DOS archivos: `oficina.html` (un mapa de importación en línea, `"three": "/vendor/three.module.js"`, y `<script type="module" src="/oficina.js">`; nada de internet) y `oficina.js`. Todo con figuras (cajas, cilindros, conos, esferas, planos), sin modelos ni texturas de archivo.

**Cómo debe verse:**
- **La oficina va DENTRO del edificio**, no en una caja aparte: un piso abierto a la vista (planta baja, o un piso con la pared de adelante de vidrio transparente). En «tres torres + barco», la oficina va en la planta baja de la torre del centro, con un lobby de vidrio, y la terraza con la piscina y el letrero va arriba.
- **Alrededor:** plaza clara y jardín verde (nunca café), una calle, árboles sencillos (tronco y copa) y faroles. Cielo con degradado según el ambiente y niebla suave.
- **Materiales:** `MeshStandardMaterial`; vidrio transparente (`opacity` cerca de 0,35) con algo de brillo; mis dos colores en fachada, letrero y detalles; ventanas con luz cálida al atardecer y de noche. Luz hemisférica más una direccional, sin sombras pesadas.
- **Los agentes:** un muñeco con cuerpo, cabeza y brazos simples, de su color (sacado de su id con una función, no buscando en un objeto), sentado en su escritorio, con su nombre en una placa legible (canvas). Máximo 16 (2 filas de 8). El que trabaja tiene un halo de luz que pulsa.
- **La Mesa de Aprobación** grande en el centro de la oficina. Los borradores son cartas apiladas sobre ella con un brillo suave (máximo 12; si hay más, «+N»). Cuando llega una nueva, «vuela» desde el escritorio de su agente hasta la Mesa.
- **Cámara inicial:** calcula la caja que envuelve todo el edificio y aléjate lo justo para verlo entero, ligeramente desde arriba, con la oficina, la terraza, la piscina y el letrero a la vista. Que la escala sea clara: los muñecos se ven y los nombres se leen desde ahí. Un giro lento automático que se detiene cuando toco. `OrbitControls` con mouse y dedo, sin bajar del suelo, con distancia mínima y máxima; `touch-action: none`.
- **Ligera** para un portátil sin tarjeta de video: unas 100 mallas visibles como máximo, geometrías y materiales reutilizados, `setPixelRatio(Math.min(devicePixelRatio, 1.5))`. Si el navegador no tiene 3D, un aviso.

## 4. Solo para MIRAR, con su propia llave
- **La página 3D nunca recibe la llave de la Mesa.** Al arrancar, el servidor deriva una **llave de vista**: HMAC-SHA256 con clave = la llave de la Mesa (UTF-8) y mensaje = `oficina`, en hexadecimal. Solo sirve para `GET /api/oficina`; la llave de la Mesa NO se acepta ahí.
- Crea SIEMPRE `GET /api/oficina`, mínima: `{ agentes: [{ id, nombre }], trabajando: id o null, enMesa: [{ id, agente, titulo }] }`, máximo 50 en la Mesa; título = la primera línea `#` del borrador, sin saltos ni caracteres de control, máximo 60. Nada de textos completos, mensajes del cliente, costos ni rutas. Mismo encabezado y mismas revisiones de `Host` y `Origin`.
- En `pagina.html`, un enlace «Ver mi oficina en 3D» (`target="_blank" rel="noopener"`) que abre `/oficina#v=<llave de vista>`, calculada en la página con `crypto.subtle` igual que el servidor. No cambies nada más de la Mesa.
- `oficina.js` lee el `#v=`, lo guarda en memoria y lo quita de la barra con `history.replaceState`. Cada 3 s, `fetch` GET a `/api/oficina` con la llave en ese encabezado, **nunca en la URL**. Si falta o la rechazan: «Abre la oficina desde el enlace de tu Mesa».
- Al pasar por encima de una carta, su título en un recuadro con `textContent`; al tocarla: «Vuelve a la pestaña de tu Mesa para leerla y sellar».
- **Sin botones que escriban.** «¡Va!», «Otra vuelta» y «Archivar» siguen SOLO en la Mesa. Nada de `innerHTML`.

## 5. El servidor: lo mínimo
- `oficina3d.mjs` con una lista blanca exacta en un `Map` (nunca un objeto con `[]`): `/oficina` → `oficina-3d/oficina.html`; `/oficina.js` → `oficina-3d/oficina.js`; `/vendor/three.module.js` y `/vendor/three.core.js` → `node_modules/three/build/` (y otro hermano si el paso 3 lo pide); `/vendor/OrbitControls.js` → `node_modules/three/examples/jsm/controls/OrbitControls.js`.
- Solo GET. Orden: si `req.url` empieza por `//` o trae `..`, `\`, `%2e`, `%2f` o `%5c` (sin distinguir mayúsculas), 404; si el `pathname` exacto no está en el `Map`, 404; después, la misma revisión de `Host` y `Origin` de `/api` (403 si falla).
- Encabezados: `Content-Type` con `charset=utf-8`, `X-Content-Type-Options: nosniff`, `Cache-Control: no-store`, `Referrer-Policy: no-referrer` y, en `/oficina`, `Content-Security-Policy: default-src 'self'; script-src 'self' 'sha256-…'; style-src 'self' 'unsafe-inline'; img-src 'self' data: blob:; connect-src 'self'; object-src 'none'; base-uri 'none'; form-action 'none'; frame-ancestors 'none'`, con la huella del mapa de importación calculada al arrancar. La regla es: **sin `'unsafe-inline'` en `script-src`**. En `style-src` puede quedar, o quitarlo si aplicas los estilos desde JS; dime cuál elegiste.

## 6. Pruebas (hazlas tú, gratis)
`tests/oficina-3d.test.mjs` en `npm test`, con el servidor real y peticiones crudas (`http.request` con `path` tal cual):
- Cada ruta del `Map` sirve los bytes exactos de su archivo; `/oficina?t=x` sirve lo mismo que `/oficina`.
- 404: `/oficina/`, `/OFICINA`, `/oficina-3d/oficina.html`, `/node_modules/three/package.json`, `/vendor/`, `/vendor/../package.json`, `/vendor/%2e%2e/package.json`, `/oficina-3d/../oficina`, `//x/oficina`, `/__proto__`, `/constructor`, `/toString`, `/vendor/three.cjs`, `/mesa.json` y un POST a `/oficina`.
- Un `Host` ajeno da 403 en `/oficina`. La CSP trae la huella correcta del mapa de importación y no tiene `'unsafe-inline'` en `script-src`.
- La llave de vista funciona en `/api/oficina` y en ninguna otra ruta de `/api`; la llave de la Mesa no sirve en `/api/oficina`; ninguna sirve en la URL (`?t=`, `?v=`).
- `/api/oficina` no trae textos completos y corta títulos largos o con saltos.
- Leyendo `oficina.js` y `oficina.html`: sin `http://` ni `https://`; sin `POST`, `PUT`, `PATCH` ni `DELETE` como palabra completa; sin `XMLHttpRequest`, `WebSocket`, `sendBeacon`, `EventSource`, `innerHTML`, `outerHTML`, `insertAdjacentHTML` ni `document.write`; sin `#t=`; cada `fetch` va a `/api/oficina`.
- `pagina.html` tiene el enlace con `/oficina#v=` y `rel="noopener"`.

Corre `npm test` completo (las de antes y las nuevas) y `node --check oficina-3d/oficina.js`. Revisa que ningún archivo que escribiste tenga los caracteres invisibles U+2028 o U+2029 (rompen el código). Abre la oficina en un navegador: comprueba que carga con la CSP y que desde la cámara inicial se ven el edificio entero, la oficina, los nombres y el detalle especial; si no tienes navegador a mano, dímelo. Dime cuántas mallas tiene la escena. Si algo falla, arréglalo y vuelve a probar.

## 7. Al final
Actualiza `LEEME.md` y dime en pocas líneas: cómo abrirla (`npm start`, abre el enlace de la Mesa y pulsa «Ver mi oficina en 3D»); qué hace (te muestra a tu equipo, quién trabaja y qué espera tu sello); qué NO hace (no aprueba, no envía, no cambia nada; para sellar vuelves a la Mesa); el resultado de las pruebas; y cómo pedirme cambios («cambia el ambiente a noche», «pon una piscina»).

---
