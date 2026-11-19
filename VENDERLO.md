# Cómo venderlo a un negocio local

No es «te vendo una IA». Es «te doy un ayudante que adelanta el trabajo y no dice nada sin tu permiso».

## 1. Lo que ofreces
- **El gancho, gratis:** una **vitrina** de una página con sus productos, precios y un botón de pedido por WhatsApp. Le sirve hoy y te da sus datos reales.
- **Uno o dos puestos, no diez:** lo que más tiempo le quita y puede esperar unas horas: respuestas modelo, **Pedidos**, **Fechas especiales** o **Reseñas** (paquete `comercio-local`). No es para contestar en vivo. Instalación: un pago único que tú defines.
- **La mensualidad** (la cifra la pones tú; una guía: menos que una persona medio tiempo): ajustar rol y lecciones, actualizar precios, sumar puestos. La IA va aparte, con la llave del cliente.

## 2. Por qué la Mesa da confianza
- **Nada sale sin su visto bueno y tu sello.** El agente solo escribe borradores.
- **Si no le gusta, vuelve con una nota**, y la nota puede volverse lección.
- **Lo aprobado queda sellado:** si un borrador cambia después de llegar a la Mesa, no se puede aprobar.
- **No inventa precios:** usa solo los datos del negocio y marca lo que falta.

## 3. El modelo: tu computador es la agencia
- Corres **una carpeta por cliente** (ej. `clientes/floristeria-las-rosas/`), cada una con su Mesa, su `datos/negocio.md`, su puerto y **la llave de API de Claude de ese cliente**, nunca la sesión de tu plan.
- **Sellas tú, con el visto bueno del dueño:** le mandas el borrador o una captura por WhatsApp; si dice que sí, «¡Va!»; si pide cambios, «Otra vuelta» con su nota.
- **Hoy no hay aprobación desde el celular:** la Mesa solo abre en tu computador.

**Cliente nuevo:** crea su carpeta vacía (nunca copies la carpeta de otro cliente: arranca con la Mesa vacía) y arma ahí la Mesa con `PROMPT.md` (y `PROMPT-NIVEL-2.md` si lleva más puestos); pon sus datos reales en `datos/negocio.md`, con su permiso; guarda su llave (sección 4); pega el encargo (sección 5); y haz con él la primera prueba.

## 4. La llave del cliente
1. **La crea el cliente** en console.anthropic.com, con un tope de gasto mensual, y te la comparte de forma segura: la escribe él mismo en tu computador o te la pasa con un enlace de un solo uso de un gestor de contraseñas. Nunca por chat ni por correo.
2. **La creas tú** en tu organización de Anthropic: un espacio de trabajo por cliente, con su tope y su llave, y le facturas ese consumo.

**Guárdala sin pegarla en el chat**, con un nombre por cliente que empiece por `MESA_LLAVE_`. Nunca `ANTHROPIC_API_KEY`: tu propio Claude Code la usaría y le cobraría al cliente tu trabajo.
- **Windows:** en PowerShell (fuera de Claude Code): `[Environment]::SetEnvironmentVariable('MESA_LLAVE_LAS_ROSAS', (Read-Host 'Pega la llave'), 'User')`. No uses `setx` con la llave en el comando: queda en el historial. O en Inicio: «variables de entorno» → «Variables de usuario» → «Nueva».
- **Mac:** en Terminal, crea un archivo solo tuyo para las llaves: `touch ~/.mesa-llaves && chmod 600 ~/.mesa-llaves && open -e ~/.mesa-llaves`. Escribe ahí `export MESA_LLAVE_LAS_ROSAS="la-llave"` y guarda. Luego, una sola vez: `echo 'source ~/.mesa-llaves' >> ~/.zshrc`. Nunca subas `~/.mesa-llaves` a ningún lado.

Luego cierra todas las terminales y abre una nueva. Ojo: cualquier programa de tu usuario puede leer estas variables. Usa un computador con clave y no grabes la pantalla mientras pegas la llave.

## 5. El encargo «modo cliente»
Pégalo en Claude Code abierto DENTRO de la carpeta del cliente:

```text
Esta carpeta es la Mesa de un cliente de mi agencia. Pásala a «modo cliente»: el agente se cobra a la llave de API del cliente, no a la sesión de mi plan. No sé programar: al final explícame en español sencillo.

1. Crea cliente.json con negocio, variable_llave (el NOMBRE de la variable, nunca el valor) y puerto fijo; pregúntame esos datos. Si cliente.json existe, es modo cliente. Rechaza un cliente.json que traiga algo que empiece por sk-ant-.
2. La llave se lee SOLO de esa variable de entorno del sistema, nunca de un archivo. El nombre empieza por MESA_LLAVE_ (mayúsculas, números y _).
3. Falla cerrado: si la variable falta, está vacía o no empieza por sk-ant-, o el puerto está ocupado, no arranca y dice por qué. Nunca vuelve a la sesión de mi plan. En el primer mensaje de cada corrida revisa apiKeySource (confírmalo en sdk.d.ts): si no es ANTHROPIC_API_KEY, cancela la corrida y dilo.
4. La llave va solo en el env del proceso del agente, como ANTHROPIC_API_KEY; nunca la copies a otra variable del servidor. Quita de ese env, sin importar mayúsculas: todas las MESA_LLAVE_ (las de otros clientes también), ANTHROPIC_AUTH_TOKEN, CLAUDE_CODE_OAUTH_TOKEN, toda CLAUDE_CODE_USE_* y un ANTHROPIC_BASE_URL que no sea https://api.anthropic.com. Si el primer mensaje de la corrida no es el de inicio (init), cancélala.
5. Ni un carácter de la llave en la consola, la página, mesa.json, los errores ni los archivos.
6. La página muestra siempre arriba «Modo cliente: el uso se cobra a la llave de <negocio> (variable <nombre completo de la variable>)», el negocio en el título, y la consola dice lo mismo al arrancar.
7. npm run probar acepta este modo (también con --guardia): avisa «Esta prueba gasta de la llave de <negocio>», pide apagar antes la Mesa porque usa el mismo puerto, y corre una vez. Sin cliente.json, todo como antes, pero las MESA_LLAVE_ tampoco llegan al agente.
8. npm test sigue pasando, gratis. Agrega pruebas con una llave FALSA inventada en la prueba: llega al env del agente; no aparece en consola, página, mesa.json ni errores; sin la variable o con un nombre inválido no arranca; las otras MESA_LLAVE_ no llegan al agente.
9. Todo lo demás igual: guardia, huellas, sello, «Otra vuelta» y «Archivar».
10. Nunca leas, imprimas ni me pidas el valor de la llave.
```

**Sin la llave real no se puede probar** que el cobro llegue a la cuenta del cliente: después del primer encargo, revisa en la consola de Anthropic que el uso aparezca ahí y no en tu plan.

## 6. Varios clientes
- **Una carpeta y un puerto por cliente:** `clientes/floristeria-las-rosas/` en el 3211, `clientes/panaderia-el-trigal/` en el 3212; el 3210 queda para tu Mesa de prueba.
- **Nombres:** carpeta en minúsculas, con guiones, sin tildes ni espacios; la variable igual, en mayúsculas (`MESA_LLAVE_EL_TRIGAL`).
- **Nunca mezcles datos:** no copies `salidas/`, `aprobadas/`, `publicado/`, `mesa.json`, `cliente.json` ni `lecciones.md` de un cliente a otro.
- Antes de sellar, mira el título: dice de qué negocio es. Apaga con Ctrl + C la Mesa que no uses.

## 7. Qué NO prometer
- **Envíos por WhatsApp.** WhatsApp no está conectado: se copia y se pega. Conectar WhatsApp Business es otro proyecto, con sus reglas y costos.
- **Aprobar desde el celular.** Hoy no existe.
- **IA gratis.** La paga el cliente, con su llave y su tope.
- **Que funciona solo** o **ventas garantizadas.** Si nadie revisa la Mesa, no sale nada; ahorra tiempo, no garantiza ventas.
- **Reseñas o testimonios inventados.** Nunca.
- **Que sus datos no pasan por ti.** Sí pasan: en Colombia aplica la Ley 1581. Pide su autorización por escrito, usa solo lo necesario (sin teléfonos ni documentos de sus clientes en los encargos) y, si manejas muchos datos, busca asesoría.
