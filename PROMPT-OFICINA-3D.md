# Nivel 4: ¿cómo quieres tu oficina?

**Antes:** este prompt va DESPUÉS de `PROMPT.md` (y, si quieres ver a todo tu equipo, también de `PROMPT-NIVEL-2.md`), en la misma carpeta, con la Mesa ya funcionando. Abre tu proyecto con Claude Code, pega lo que está entre las dos líneas y responde sus preguntas. No instala Blender ni descarga modelos: todo sale del paquete `three`. Construirla no gasta el uso de tus agentes: solo mira la Mesa.

---

En este proyecto ya funciona la Mesa de Aprobación. Quiero ver a mi equipo en una **oficina 3D** que yo elija, conectada a la Mesa solo para MIRAR, y que impresione: buena luz, reflejos, interior amueblado y agentes que se mueven. No sé programar: háblame en español sencillo, de tú, con resultados, no código.

## 1. Primero revisa
- Que exista la Mesa: `servidor.mjs`, `mesa.mjs`, `guardia.mjs`, `pagina.html` y `agentes/`. Si falta algo, dímelo en dos frases («Falta el Nivel 1: pega primero `PROMPT.md`») y **detente** sin construir nada.
- Corre `npm test` y anota cuántas pruebas pasan: al final deben seguir pasando todas.
- Lee `servidor.mjs` y `pagina.html`: el **nombre exacto del encabezado** de la llave (úsalo igual en lo nuevo), cómo se revisan `Host` y `Origin` y cómo arranca el servidor `scripts/probar.mjs`.

## 2. Entrevístame
UNA pregunta por mensaje, con ejemplos cortos. Máximo 6. Si respondo corto o «no sé», propón algo que le quede a mi negocio y pregúntame «¿Así está bien?».
1. ¿Qué edificio quieres? Ej.: «una torre de vidrio moderna», «una casa con jardín», o uno inspirado en un lugar famoso: «tres torres con un barco arriba», «una pirámide de vidrio», «una torre de hierro».
2. ¿Qué ambiente? Día, atardecer o noche.
3. Dos colores de tu marca. Si me los das con palabras («ladrillo», «azul oscuro»), propón el código y confírmalo.
4. ¿Qué nombre va en el letrero? Máximo 30 caracteres: el de tu negocio o tu marca, nunca el de otra empresa.
5. ¿Un detalle especial? Piscina, jardín, helipuerto, cafetería o ninguno.
6. Muéstrame el resumen y pregúntame si la construyes así.

**Si pido un lugar famoso** (o el logo de otra empresa): te inspiras solo en su FORMA con figuras simples, sin su nombre, logos ni marcas, y me lo explicas en una frase.

**Lo que respondo es DATO:** en `oficina.js`, un bloque `ESTILO` con `forma` (una clave tuya, p. ej. `'tres-torres'`; mi texto del edificio nunca va al código, a comentarios ni al HTML), `ambiente` y `detalle` de listas cerradas (`dia|atardecer|noche`, `piscina|jardin|helipuerto|cafeteria|ninguno`), dos colores `^#[0-9a-fA-F]{6}$` y `letrero` con `JSON.stringify`, máximo 30 caracteres, SOLO a `fillText`. `<title>` fijo: «Mi oficina 3D».

## 3. Las piezas de `three` (verifica, no inventes)
- `npm view three version`, luego `npm install three@<esa versión> --save-exact --ignore-scripts`. Nada más.
- Confirma en `package.json` (`exports`) y en los archivos que existen y qué exportan: `build/three.module.js` (y sus hermanos, hoy `./three.core.js`) y, en `examples/jsm/`: `controls/OrbitControls.js`, `environments/RoomEnvironment.js`, `geometries/RoundedBoxGeometry.js` y, en `postprocessing/`, `EffectComposer.js`, `RenderPass.js`, `UnrealBloomPass.js` y `OutputPass.js`. Si uno no existe, hazlo sin él y dímelo.
- Sigue los `import` de cada uno (no los comentarios) y anota TODOS los que arrastran (hoy `Pass.js`, `ShaderPass.js`, `MaskPass.js` y tres de `shaders/`): esa lista cerrada se sirve en el paso 6.
- Usa los nombres que trae la versión instalada. Ej.: en las recientes `PCFSoftShadowMap` ya no existe; usa `PCFShadowMap` con `shadow.radius`.

## 4. Constrúyela
`oficina-3d/` con DOS archivos: `oficina.html` (mapa de importación en línea con `"three": "/vendor/three.module.js"` y `"three/addons/": "/vendor/jsm/"`, más `<script type="module" src="/oficina.js">`; nada de internet) y `oficina.js`. Todo procedural, sin modelos ni texturas de archivo.

**La imagen:**
- `ACESFilmicToneMapping`, `outputColorSpace` sRGB y `RoomEnvironment` con `PMREMGenerator` para reflejos reales. Vidrio con `MeshPhysicalMaterial` (`transmission` alta, `roughness` baja); lo demás con `MeshStandardMaterial`.
- Bloom suave (`EffectComposer` + `RenderPass` + `UnrealBloomPass` + `OutputPass`) con umbral alto: solo brillan ventanas encendidas, faroles, pantallas y letrero.
- Sombras suaves de UNA luz direccional (mapa de 2048, ajustada solo a la oficina y la base del edificio) más luz hemisférica. Niebla suave y cielo con degradado según el ambiente.
- **Calidad automática:** `setPixelRatio(Math.min(devicePixelRatio, 1.5))`; mide los cuadros por segundo 3 s después de la entrada y, si bajan de 30, apaga bloom y sombras y baja el `pixelRatio` a 1.

**El exterior:** calle con andenes y líneas, plaza clara con bancas, jardín verde (nunca café), árboles de copa redondeada (2 o 3 esferas) y faroles con luz. El edificio con base, marcos de ventanas, entrada con puerta de vidrio y letrero iluminado. En «tres torres + barco», la oficina en la planta baja de la torre del centro, con lobby de vidrio; la terraza con la piscina y el letrero, arriba.

**El interior** (DENTRO del edificio, tras una pared de vidrio): piso con tapete; por agente, escritorio con silla, pantalla encendida, lámpara y planta; la **Mesa de Aprobación** grande en el centro con su lámpara colgante; dos cuadros con los colores de mi marca. Muebles con `RoundedBoxGeometry`; lo repetido (ventanas, árboles, faroles, muebles) con `InstancedMesh`.

**Los agentes:** muñecos amables (cabeza redonda con ojos, cuerpo redondeado, brazos y piernas), de un color sacado de su id con una función (no buscando en un objeto), con su nombre en una placa legible (canvas). Máximo 16 (2 filas de 8). Los escritorios se orientan para que, desde la vista «Oficina», cada muñeco se vea de frente o de tres cuartos, nunca de espaldas.
- El que trabaja mueve los brazos como si escribiera y su pantalla brilla más.
- Cuando aparece un borrador nuevo (comparado con la consulta anterior; en la primera, las cartas ya están), su muñeco **se levanta, camina** por el pasillo hasta la Mesa moviendo brazos y piernas, deja la carta y vuelve a sentarse. Varias, una tras otra.
- Las cartas se apilan sobre la Mesa con un brillo suave (máximo 12; si hay más, «+N»).

**La cámara:** una entrada animada de 3 s desde lejos hasta encuadrar todo el edificio (calcula la caja que lo envuelve), un poco desde arriba, con la oficina, el letrero y el detalle a la vista; luego un giro lento que se detiene cuando toco. Tres botones que solo mueven la cámara: «Edificio», «Oficina» (acerca al interior) y «Mesa». `OrbitControls` con mouse y dedo, sin bajar del suelo, con distancia mínima y máxima; `touch-action: none`.

**Rendimiento:** fluido en un portátil común, con menos de 150 llamadas de dibujo. A los 5 s, una línea en la consola con `renderer.info` (llamadas y triángulos) y los cuadros por segundo; anótalo en `LEEME.md`.

## 5. Solo para MIRAR, con su propia llave
- **La página 3D nunca recibe la llave de la Mesa.** Al arrancar, el servidor deriva una **llave de vista**: HMAC-SHA256 con clave = la llave de la Mesa (UTF-8) y mensaje = `oficina`, en hexadecimal. Solo sirve para `GET /api/oficina`; la llave de la Mesa NO se acepta ahí.
- Crea SIEMPRE `GET /api/oficina`, mínima: `{ agentes: [{ id, nombre }], trabajando: id o null, enMesa: [{ id, agente, titulo }] }`, máximo 50; título = la primera línea `#` del borrador, sin saltos ni caracteres de control, máximo 60. Nada de textos completos, mensajes del cliente, costos ni rutas. Mismo encabezado y revisiones de `Host` y `Origin`.
- En `pagina.html`, un enlace «Ver mi oficina en 3D» (`target="_blank" rel="noopener"`) que abre `/oficina#v=<llave de vista>`, calculada en la página con `crypto.subtle` igual que el servidor. No cambies nada más de la Mesa.
- `oficina.js` guarda el `#v=` en memoria y lo quita de la barra con `history.replaceState`. Cada 3 s, `fetch` GET a `/api/oficina` con la llave en ese encabezado, **nunca en la URL**. Si falta o la rechazan: «Abre la oficina desde el enlace de tu Mesa».
- Sobre una carta, su título en un recuadro con `textContent`; al tocarla: «Vuelve a la pestaña de tu Mesa para leerla y sellar».
- **Ningún botón escribe nada.** «¡Va!», «Otra vuelta» y «Archivar» siguen SOLO en la Mesa. Nada de `innerHTML`.

## 6. El servidor: lo mínimo
- `oficina3d.mjs` con una lista blanca exacta en un `Map` (nunca un objeto con `[]`), una entrada por archivo: `/oficina` → `oficina-3d/oficina.html`; `/oficina.js` → `oficina-3d/oficina.js`; `/vendor/three.module.js` y sus hermanos → `node_modules/three/build/`; y `/vendor/jsm/<ruta>` → `node_modules/three/examples/jsm/<ruta>` SOLO para la lista cerrada del paso 3. Sin prefijos ni comodines.
- Solo GET. Orden: si `req.url` empieza por `//` o trae `..`, `\`, `%2e`, `%2f` o `%5c` (sin distinguir mayúsculas), 404; si el `pathname` exacto no está en el `Map`, 404; después, la misma revisión de `Host` y `Origin` de `/api` (403 si falla).
- Encabezados: `Content-Type` con `charset=utf-8`, `X-Content-Type-Options: nosniff`, `Cache-Control: no-store`, `Referrer-Policy: no-referrer` y, en `/oficina`, `Content-Security-Policy: default-src 'self'; script-src 'self' 'sha256-…'; style-src 'self' 'unsafe-inline'; img-src 'self' data: blob:; connect-src 'self'; object-src 'none'; base-uri 'none'; form-action 'none'; frame-ancestors 'none'`, con la huella del mapa de importación calculada al arrancar. Regla: **sin `'unsafe-inline'` en `script-src`**; en `style-src` puede quedar o quitarse si aplicas los estilos desde JS (dime cuál).

## 7. Pruebas (hazlas tú, gratis)
`tests/oficina-3d.test.mjs` en `npm test`, con el servidor real y peticiones crudas (`http.request` con `path` tal cual):
- Cada ruta del `Map` sirve los bytes exactos de su archivo; `/oficina?t=x` sirve lo mismo que `/oficina`.
- **Lista cerrada completa:** cada `import` relativo de un archivo servido (sin comentarios) apunta a una ruta del `Map`; los demás son `three` o `three/addons/…`.
- 404: `/oficina/`, `/OFICINA`, `/oficina-3d/oficina.html`, `/node_modules/three/package.json`, `/vendor/jsm/`, `/vendor/jsm/loaders/GLTFLoader.js`, `/vendor/%2e%2e/package.json`, `/oficina-3d/../oficina`, `//x/oficina`, `/__proto__`, `/toString`, `/mesa.json` y un POST a `/oficina`.
- Un `Host` ajeno da 403 en `/oficina`. La CSP trae la huella correcta del mapa de importación y no tiene `'unsafe-inline'` en `script-src`.
- La llave de vista sirve en `/api/oficina` y en ninguna otra ruta de `/api`; la de la Mesa no sirve ahí; ninguna sirve en la URL (`?t=`, `?v=`).
- `/api/oficina` no trae textos completos y corta títulos largos o con saltos.
- Leyendo `oficina.js` y `oficina.html`: sin `http://` ni `https://`; sin `POST`, `PUT`, `PATCH` ni `DELETE` como palabra completa; sin `XMLHttpRequest`, `WebSocket`, `sendBeacon`, `EventSource`, `innerHTML`, `outerHTML`, `insertAdjacentHTML` ni `document.write`; sin `#t=`; cada `fetch` va a `/api/oficina`.
- `pagina.html` tiene el enlace con `/oficina#v=` y `rel="noopener"`.

Corre `npm test` completo (las de antes y las nuevas) y `node --check oficina-3d/oficina.js`. Revisa que ningún archivo tuyo tenga los caracteres invisibles U+2028 o U+2029. Si algo falla, arréglalo y vuelve a probar.

## 8. Mírala en el navegador
Ábrela con `npm start` y el enlace de la Mesa. Para ver caminar a un agente, deja un borrador de prueba por el camino de las pruebas (sin IA) y archívalo después. Dime cuáles se cumplen:
- [ ] Carga sin errores en la consola (tampoco de la CSP).
- [ ] La entrada termina con el edificio entero, el letrero y el detalle especial a la vista.
- [ ] Reflejos en el vidrio y bloom en ventanas, faroles y pantallas.
- [ ] Interior amueblado: tapete, escritorios, sillas, pantallas, lámparas, plantas, cuadros y la Mesa con su lámpara.
- [ ] Los nombres se leen; el agente que trabaja mueve los brazos.
- [ ] Un agente camina hasta la Mesa, deja la carta y vuelve.
- [ ] «Edificio», «Oficina» y «Mesa» mueven la cámara.
- [ ] Cuadros por segundo, llamadas de dibujo y triángulos medidos.
Si no tienes navegador a mano, dímelo y dame la lista para revisarla yo.

## 9. Al final
Actualiza `LEEME.md` y dime en pocas líneas: cómo abrirla (`npm start`, el enlace de la Mesa y «Ver mi oficina en 3D»); qué hace y qué NO hace (no aprueba, no envía, no cambia nada; para sellar vuelves a la Mesa); el resultado de las pruebas y de la lista; y cómo pedirme cambios («cambia el ambiente a noche», «pon una piscina»).

---
