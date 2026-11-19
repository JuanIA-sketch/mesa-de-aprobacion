# Cómo venderlo a un negocio local

Esto no es «te vendo una IA». Es «te doy un ayudante que adelanta el trabajo y no dice nada sin tu permiso». Así se entiende y así se confía.

## 1. El gancho: una vitrina gratis
Empieza regalando algo que el dueño vea funcionando hoy: una **vitrina** o **catálogo** de una página con sus productos, sus precios y un botón de pedido por WhatsApp en cada uno.

- Le sirve desde el primer día y no le cuesta nada.
- Te da sus datos reales (productos, precios, horario, domicilios), que son justo lo que el agente necesita.
- Abre la conversación: «¿Y si además alguien te deja listas las respuestas a esos pedidos?».

Precio: gratis. Es la puerta de entrada.

## 2. La oficina con 1 o 2 puestos
No llegues con diez agentes. Llega con uno o dos que resuelvan lo que más le quita tiempo:

- **Respuestas de WhatsApp:** deja lista la respuesta a cada cliente, con los precios reales.
- **Pedidos** o **Fechas especiales** (del paquete `comercio-local`).

Instalación y ajuste a su negocio: un pago único. Tú pones el precio según el tamaño del negocio y cuántos puestos lleve.

## 3. La mensualidad
Cobra por mantener la oficina viva, no por «la IA»:

- Ajustar el rol y las lecciones con lo que él corrige en «Otra vuelta».
- Actualizar precios, productos y fechas.
- Agregar un puesto nuevo cuando lo necesite.

Mensualidad: también la defines tú. Un buen punto de partida es cobrar menos de lo que le costaría una persona medio tiempo haciendo lo mismo, y aparte la IA, que la paga el cliente con su llave.

## 4. Por qué la Mesa da confianza
Es lo que más pesa en la venta. Explícalo así:

- **Nada sale sin el dueño.** El agente escribe un borrador; el dueño lo lee y decide.
- **Si no le gusta, lo devuelve con una nota** y el agente lo rehace. Esa nota se puede volver lección para la próxima vez.
- **Lo aprobado queda guardado y sellado.** Si alguien cambia un borrador después de llegar a la Mesa, no se puede aprobar.
- **El agente no inventa precios:** solo usa los datos del negocio. Lo que falta lo marca para que el dueño lo complete.

## 5. Qué NO prometer
- **Envíos automáticos por WhatsApp.** El agente no envía nada: el dueño copia y pega. Conectar WhatsApp Business es otro proyecto, con sus propias reglas y costos.
- **Que la IA es gratis.** La IA la paga el cliente, con **su propia llave de API** de Claude y un **tope** de gasto mensual que él decide. Nunca uses la sesión de tu plan para el trabajo de un cliente.
- **Que funciona solo.** Nada sale sin el dueño. Si él no revisa la Mesa, no pasa nada.
- **Resultados de ventas.** Ahorra tiempo y ordena las respuestas; no garantiza ventas.
- **Reseñas o testimonios.** El agente nunca inventa reseñas ni clientes.
- **Datos de sus clientes en tu poder.** Esos datos son del negocio. En Colombia aplica la ley de protección de datos (Ley 1581).

## Antes de entregárselo
- El prompt de `PROMPT.md` usa la sesión de Claude Code de quien lo arma, y a propósito quita las llaves del entorno del agente. Para un cliente es al revés: SIEMPRE con **su propia llave de API**, en **su computador** o en un servidor suyo. Pídele a Claude Code ese cambio.
- La llave va como **variable de entorno del sistema** (`ANTHROPIC_API_KEY`), nunca en un archivo de la carpeta.
- El **tope de gasto** se pone en la consola de Anthropic, en la cuenta del cliente.
- Cambia `datos/negocio.md` por los datos reales del negocio, con su permiso.
- Haz con él la primera prueba: un mensaje real, el borrador en la Mesa y su primer «¡Va!».
