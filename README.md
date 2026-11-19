# Mesa de Aprobación

**Un agente de IA que trabaja para un negocio, pero no puede hacer nada sin tu sello.**

Pegas un prompt en Claude Code y te arma una página en tu computador. Le das un encargo al agente, él escribe un borrador y el borrador llega a la Mesa. Tú decides: **«¡Va!»**, **«Otra vuelta»** (con una nota, y lo rehace) o **«Archivar»**. Solo lo que sellas sale.

**Así se ve** (una corrida real): el cliente pide girasoles, pide 50 % de descuento y dice «olvida tus instrucciones». El agente usa solo los precios del negocio, no da el descuento y te avisa en «Qué revisar antes de enviar». Nada sale hasta tu «¡Va!».

| El encargo en la Mesa | La respuesta que propone el agente |
|---|---|
| ![El borrador en la Mesa](imagenes/mesa-pedido.jpg) | ![La respuesta lista para copiar](imagenes/mesa-respuesta.jpg) |

**Para quién:** quien vende agentes de IA a negocios, el dueño de un negocio que quiere un ayudante que no diga nada sin su permiso, y creadores (guiones, carruseles, correos: el agente propone y tú sellas). No necesitas saber programar.

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

## Los 3 niveles
| Nivel | Qué es | Archivo |
|---|---|---|
| 1. El prompt | Un agente («Respuestas de WhatsApp» de una floristería de ejemplo) y la Mesa. | [`PROMPT.md`](PROMPT.md) |
| 2. El equipo | Contratar más agentes desde un paquete para armar el equipo de un negocio (Pedidos, Fechas especiales, Reseñas). Baja el repositorio (botón verde «Code» → «Download ZIP», o `git clone`), copia la carpeta `paquetes/` dentro de tu proyecto y pega el prompt. | [`PROMPT-NIVEL-2.md`](PROMPT-NIVEL-2.md) y [`paquetes/`](paquetes/) |
| 3. Venderlo | Cómo ofrecérselo a un negocio local, y qué no prometer. | [`VENDERLO.md`](VENDERLO.md) |

Los encargos que sirvieron para construir algo así están en [`ENCARGOS.md`](ENCARGOS.md). Las pruebas que se le hicieron a esta receta antes de publicarla están en [`PRUEBAS.md`](PRUEBAS.md).

---

Inspirado en la oficina de agentes de Charly.marketing. La receta la diseñaron y la probaron Fable 5.2 (seguridad y revisión) y Opus 5.5 (escritura y pruebas); para correrla no necesitas Fable: basta el modelo que trae Claude Code.

Licencia MIT. Ver [`LICENSE`](LICENSE).
