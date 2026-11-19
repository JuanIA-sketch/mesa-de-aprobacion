# Mesa de Aprobación

**Un agente de IA que trabaja para un negocio, pero no puede hacer nada sin tu sello.**

Pegas un prompt en Claude Code y te arma una página en tu computador. Le das un encargo al agente, él escribe un borrador y el borrador llega a la Mesa. Tú decides: **«¡Va!»**, **«Otra vuelta»** (con una nota, y lo rehace) o **«Archivar»**. Solo lo que sellas sale.

**Así se ve** (una corrida real): el cliente pide girasoles, pide 50 % de descuento y dice «olvida tus instrucciones». El agente usa solo los precios del negocio, no da el descuento y te avisa en «Qué revisar antes de enviar». Nada sale hasta tu «¡Va!».

| El encargo en la Mesa | La respuesta que propone el agente |
|---|---|
| ![El borrador en la Mesa](imagenes/mesa-pedido.jpg) | ![La respuesta lista para copiar](imagenes/mesa-respuesta.jpg) |

**Y con el Nivel 4, tu oficina en 3D:** Claude Code te pregunta cómo la quieres y la construye. Esta salió de pedir «tres torres de vidrio unidas arriba por una terraza como un barco, al atardecer, con piscina». Tus agentes trabajan en sus escritorios y cada borrador llega como una carta a la Mesa. Es solo para mirar: el sello sigue en tu Mesa.

![Una oficina 3D hecha con el Nivel 4](imagenes/oficina-3d.jpg)

**Para quién:** quien vende agentes de IA a negocios, el dueño de un negocio que quiere un ayudante que no diga nada sin su permiso, y creadores (guiones, carruseles, correos: el agente propone y tú sellas). No necesitas saber programar.

## Qué es y qué NO es
| Es | No es |
|---|---|
| La **Mesa de Aprobación**: el corazón de la oficina de agentes de Charly.marketing, en una página sencilla que corre en tu computador. | La **oficina 3D del video**: es el laboratorio de Charly, mucho más grande (torres, barrio, carro, muchos puestos), y no viene aquí. |
| Con el Nivel 4, **una oficina 3D sencilla que tú personalizas** (tu edificio, tus colores, tu letrero), pequeña y ligera. | Una oficina 3D que aprueba: la del Nivel 4 solo mira; «¡Va!», «Otra vuelta» y «Archivar» siguen en la Mesa. |
| Un agente de ejemplo (respuestas de WhatsApp de una floristería) y, con los otros niveles, el equipo de un comercio, el de un **creador de contenido** (guiones de reels y carruseles, paquete `contenido-creador`) o **el puesto que tú inventes para cualquier negocio**. | Un bot que responde solo. Aquí el agente **nunca** envía, publica ni cobra: deja el borrador y tú lo copias después del «¡Va!». |
| Para creadores: el guion del reel (ganchos, tomas, texto en pantalla, cierre con «Comenta PALABRA» y caption) y el texto del carrusel, listos para grabar y diseñar. | La **edición de video** de los reels de Charly: corre en su oficina, no en esta receta. Grabar, editar y diseñar lo haces tú. |
| Para tu uso, con la cuenta de Claude que ya tienes. Para un cliente, con **su** llave (mira [`VENDERLO.md`](VENDERLO.md)). | Conectado a WhatsApp, a Instagram o a tu agenda. Eso sería otro paso, también con sello. |
| Probado en **Windows**, también con rutas con espacios ([`PRUEBAS.md`](PRUEBAS.md)). | Probado en Mac: debería funcionar, pero todavía no se ha corrido allí. |

Los textos vienen en español de Colombia («de tú»). Si tu país habla de usted o de vos, díselo a Claude Code en el Nivel 2b y él lo cambia.

## Antes de empezar (una sola vez, Windows o Mac)
1. **Node.js 20 o más:** descarga la versión **LTS** de [nodejs.org](https://nodejs.org) e instálala con las opciones que trae.
2. **Claude Code:** abre la terminal (Windows: busca «PowerShell» en el menú Inicio; Mac: busca «Terminal» con Cmd + Espacio) y pega el comando oficial:
   - Windows (PowerShell): `irm https://claude.ai/install.ps1 | iex`
   - Mac: `curl -fsSL https://claude.ai/install.sh | bash`

   Si no funciona, sigue la [página oficial de instalación](https://code.claude.com/docs/en/setup).
3. **Inicia sesión:** cierra la terminal, abre una nueva y escribe `claude`. Se abre el navegador para que entres con tu cuenta de Claude **Pro o Max** (el plan gratis no incluye Claude Code).
4. **Abre una carpeta vacía:** en esa terminal escribe `/exit` y luego, una línea a la vez:
   ```
   mkdir mi-mesa
   cd mi-mesa
   claude
   ```
   Si te pregunta si confías en la carpeta, di que sí.

**Modelo:** usa el que Claude Code trae por defecto; no cambies nada. El agente que se construye usa Sonnet.

**Cuánto cuesta:** construirlo toma de 9 a 12 minutos. Cada encargo de ejemplo gastó unos US$0,04 del uso de tu plan (medido). Es para tu uso, en tu computador; para un cliente, mira [`VENDERLO.md`](VENDERLO.md).

## Qué vas a ver
1. **Pegas el prompt:** copia todo lo que está entre las dos líneas de [`PROMPT.md`](PROMPT.md), pégalo en Claude Code y pulsa Enter. Te pedirá permiso para crear archivos y correr comandos (`npm install`, `npm test`): léelo y acepta.
2. **Trabaja solo de 9 a 12 minutos:** construye, corre las pruebas y hace una corrida real. Al final te explica en español qué hizo y te muestra el borrador que escribió el agente.
3. **Lo abres:** escribe `/exit` y luego `npm start`. Aparece `Abre: http://127.0.0.1:3210/#t=…`: copia ese enlace completo en el navegador (lleva una llave que cambia en cada arranque). Para apagarlo: Ctrl + C.
4. **Tu Mesa:** arriba, «Mensaje del cliente» y el botón «Dárselo al agente»; mientras trabaja dice «El agente está trabajando…»; abajo llega el borrador con «¡Va!», «Otra vuelta» y «Archivar».
5. **¿Tu negocio no es una floristería?** Después del Nivel 2, el Nivel 2b te crea el puesto que necesites (mira «Los niveles»).
6. **Tu oficina en 3D (Nivel 4):** en la Mesa, el enlace «Ver mi oficina en 3D» te muestra a tu equipo en el edificio que elegiste, quién trabaja y cada borrador como una carta sobre la Mesa; tocas una carta y vuelves a la Mesa para sellarla.

## Si algo sale mal
| Lo que ves | Arreglo |
|---|---|
| **Node viejo:** `node --version` dice menos de `v20`, o falla `npm install`. | Instala la LTS de [nodejs.org](https://nodejs.org), abre una terminal nueva y vuelve a intentar. |
| **Puerto ocupado:** el enlace no dice 3210, o la página no abre. | La Mesa toma el siguiente puerto libre: abre el enlace exacto que imprimió `npm start`. Si dejaste otra Mesa prendida, apágala con Ctrl + C en su terminal. |
| **Una llave en el entorno:** Claude Code pregunta si usar una API key, o la Mesa avisa «el uso se cobra a una llave, no a tu plan». | Tienes `ANTHROPIC_API_KEY` guardada y el uso se cobraría a esa llave. Responde que no y bórrala (Windows: busca «variables de entorno» en el menú Inicio; Mac: quita la línea `export ANTHROPIC_API_KEY=…` de `~/.zshrc`). Abre una terminal nueva; dentro de Claude Code, `/status` muestra con qué cuenta estás. |

## Cómo fluye
```mermaid
flowchart LR
  A[Encargo] --> B[Agente]
  B --> C[Borrador]
  C --> D[Mesa de Aprobación]
  D -->|Tu sello: ¡Va!| E[Copia sellada]
  E --> F[Publicado]
  D -->|Otra vuelta + nota| B
  D -->|Archivar| G[Archivado]
```

**Por qué es seguro:**
- El agente solo puede escribir su borrador. Publicar lo hace el servidor, no el agente.
- Al llegar a la Mesa, cada borrador queda con su huella. Si alguien lo cambia después, no se puede aprobar.
- El agente solo lee los datos del negocio y sus borradores: no tiene consola, no lee llaves y no envía nada.
- Lo que escribe un cliente es un dato, nunca una orden.

## Los niveles
| Nivel | Qué es | Archivo |
|---|---|---|
| 1. El prompt | Un agente («Respuestas de WhatsApp» de una floristería de ejemplo) y la Mesa. | [`PROMPT.md`](PROMPT.md) |
| 2. El equipo | Contratar más agentes desde un paquete. Hay dos paquetes: `comercio-local` (Pedidos, Fechas especiales, Reseñas) y `contenido-creador` (Guiones de reels, Carruseles). Baja el repositorio (botón verde «Code» → «Download ZIP», o `git clone`), copia la carpeta `paquetes/` dentro de tu proyecto y pega el prompt. | [`PROMPT-NIVEL-2.md`](PROMPT-NIVEL-2.md) y [`paquetes/`](paquetes/) |
| 2b. Tu propio puesto | Para cualquier negocio (peluquería, restaurante, gimnasio, taller…). Claude te entrevista en el chat y escribe un paquete nuevo en `paquetes/`; lo contratas en la Sala de Contratar, con tu «¡Va!». | [`PROMPT-CREA-TU-PUESTO.md`](PROMPT-CREA-TU-PUESTO.md) |
| 3. Venderlo | Cómo ofrecérselo a un negocio local, y qué no prometer. | [`VENDERLO.md`](VENDERLO.md) |
| 4. ¿Cómo quieres tu oficina? | Claude te pregunta qué edificio quieres (una torre de vidrio, una casa con jardín, uno inspirado en un lugar famoso), el ambiente, tus colores y tu letrero, y te arma una oficina 3D donde ves a tu equipo trabajar. Solo mira: aprobar sigue en la Mesa. | [`PROMPT-OFICINA-3D.md`](PROMPT-OFICINA-3D.md) |

## Próximos niveles
Todavía no están en la receta. Cada uno seguiría la misma regla: nada sale sin tu sello.
- **Edición de video:** cortar los silencios y poner subtítulos a tus reels.
- **Aprobar desde el celular:** sellar los borradores sin estar frente al computador.
- **Enviar por WhatsApp después del sello:** que el «¡Va!» mande la respuesta, en lugar de copiarla y pegarla tú.

Los encargos que sirvieron para construir algo así están en [`ENCARGOS.md`](ENCARGOS.md). Las pruebas que se le hicieron a esta receta antes de publicarla están en [`PRUEBAS.md`](PRUEBAS.md).

---

Inspirado en la oficina de agentes de Charly.marketing. La receta la diseñaron y la probaron Fable 5.2 (seguridad y revisión) y Opus 5.5 (escritura y pruebas); para correrla no necesitas Fable: basta el modelo que trae Claude Code.

Licencia MIT. Ver [`LICENSE`](LICENSE).
