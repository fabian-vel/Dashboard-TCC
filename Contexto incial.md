# Contexto inicial

> Las secciones I a III son la conversación original con la que nació el tablero y se conservan como historia; parte de lo que dicen ya cambió (por ejemplo, ya no hay 7 escenarios y el archivo se llama `Dashboard-TCC.html`). **El estado vigente y las instrucciones para trabajar con una IA están en la sección IV, al final.**

**I. comoces tcc?**
Sí. En Colombia, **TCC** suele referirse a la empresa de logística y mensajería (Transportadora Comercial Colombia). Hace envíos de paquetes y carga a nivel nacional, ofrece servicios de logística para empresas (almacenamiento, distribución, e-commerce) y tiene puntos de atención en muchas ciudades. Desde su página web puedes rastrear guías, cotizar envíos y buscar oficinas.

También puede significar otras cosas según el contexto, como *Trabajo de Conclusión de Curso* (la tesis de pregrado en Brasil) o *Terapia Cognitivo-Conductual* en psicología.

¿A cuál te refieres? Si es la transportadora, te puedo ayudar a rastrear un envío, cotizar o encontrar una oficina en Bogotá.

**II. es nuestro cliente, somo proveedores de software, soportes, mejoras, etc. quisiera hacer un grafico de como es el flujo de negocio desde que se recoge un pedido (remesa) en la casa de un remitente haza llegar a la casa de un destinatario. este flujo tiene muchas caminos diferentes y bifurcaciones. pero no quiero cualquier grafico, quero un dashboard bien armado ilustrado que tenga animación que muestra cada etapa y muestre que esta ocurriendo en cada etapa por la que pasa, dependiendo el flujo o escenario que se seleccione. no quero algo ostentoso, si deseo algo novedoso, sobrio pero llamativo. que necesarito para hacerlo, que herramientas y tecnologías. algo fácil de manejar y portar.**

La buena noticia es que no necesitas una plataforma pesada. Lo más fácil de mantener y llevar a cualquier lado es **un solo archivo HTML** que se abre en cualquier navegador, sin servidor ni instalación. Lo puedes mandar por correo, proyectar en una reunión o publicar como enlace.

## La pieza clave: separar los datos del dibujo

Lo que hace que esto sea fácil de manejar es que el flujo no esté "dibujado a mano", sino **descrito como datos** (un JSON) y que el dashboard lo dibuje y anime a partir de ahí:

- **Etapas (nodos):** recolección, admisión en terminal, clasificación, despacho en línea troncal, terminal destino, reparto, entrega…
- **Conexiones (caminos):** qué etapa lleva a cuál.
- **Escenarios:** la secuencia que sigue una remesa en cada caso, y qué pasa en cada etapa (texto, actores, sistemas involucrados, estados de la guía, tiempos típicos).

Así, si mañana TCC agrega un paso o un escenario nuevo, editas unas líneas de texto y no tocas el diseño.

## Tecnologías recomendadas

- **SVG + JavaScript** para el diagrama. Se ve nítido en cualquier pantalla y cada etapa se puede animar e iluminar por separado.
- **GSAP** (librería de animación, gratuita) para mover el "paquete" por la ruta, resaltar la etapa activa y hacer las transiciones suaves entre escenarios. Es muy estable y se carga desde un CDN, así que no hay nada que instalar.
- **Íconos lineales** (por ejemplo Lucide o Phosphor) para las ilustraciones: camión, bodega, escáner, casa, oficina. Dan ese aire sobrio pero cuidado, mejor que dibujos recargados.
- **Opcional:** si más adelante quieres meterlo dentro de una aplicación tuya, **React Flow** es el estándar para diagramas de flujo interactivos. Para una pieza de presentación es innecesario.

Para el estilo sobrio pero llamativo te sugiero fondo neutro (claro u oscuro), una paleta corta con un solo color de acento, y la animación con significado: el paquete recorre la ruta, la etapa activa se ilumina y muestra su detalle a un lado, y las ramas no usadas quedan tenues pero visibles. Así se entiende que existen otros caminos.

## Escenarios típicos para arrancar

Estos son los escenarios comunes en mensajería de paquetes. Los ajustamos a la operación real de TCC:

1. Entrega exitosa estándar (puerta a puerta).
2. Destinatario ausente → reintento de entrega.
3. Ausente de nuevo → queda en oficina para reclamar.
4. Novedad: dirección errada o incompleta → gestión con el remitente.
5. Devolución al remitente.
6. Pago contraentrega (recaudo).
7. Entrega en oficina o punto aliado desde el origen.

## Lo que necesito de ti

Te lo puedo construir directamente aquí y entregártelo como página lista para abrir y compartir. Para que refleje la operación real, sería ideal que me compartas:

- Las etapas y escenarios reales (aunque sea en una lista o un documento).
- Si quieres mostrar los sistemas o módulos de software que intervienen en cada etapa. Siendo ustedes el proveedor, eso puede ser el diferencial del dashboard.
- Colores o logo si quieres alinearlo a tu marca o a la de TCC.

Si prefieres, arranco ya con un prototipo usando los escenarios típicos de arriba, y luego lo ajustamos con la información real. ¿Cómo lo prefieres?

**III. puedes jenerar una plantilla o jemplo de ocmo debe quedar, asi en el fututo te puedo ir pasando la logica de negocio e ir cresiendo el dashboard**

Voy a armarte una plantilla funcional: un dashboard en un solo archivo HTML, con el flujo definido como datos para que luego solo me pases la lógica y lo vayamos creciendo.

Ya está la plantilla **Recorrido de la remesa**. Es un tablero animado en el que eliges un escenario y una remesa (el cuadrito ámbar) recorre el mapa etapa por etapa. Mientras avanza, se va trazando el camino recorrido y en el panel inferior ves qué ocurre en cada etapa: responsable, sistema o módulo, estado de la guía, tiempo típico y los eventos que se registran.

**Qué trae:**
- **7 escenarios de ejemplo:** entrega estándar, reintento, retenida en oficina, dirección corregida, devolución, pago contraentrega y envío desde oficina.
- **Mapa por carriles operativos:** origen, terminal origen, red troncal, terminal destino y última milla. Las rutas que el escenario no usa quedan tenues, para que se note que existen otros caminos.
- **Controles:** reproducir y pausar, anterior y siguiente, velocidad, y clic en cualquier etapa o nodo para saltar a ella. También funciona con las flechas del teclado y la barra espaciadora.
- **Modo claro y oscuro**, y se adapta al celular.

**Cómo lo vamos creciendo:** toda la lógica de negocio está en un solo bloque de datos al inicio del archivo, llamado `FLUJO`. El diseño y la animación se generan solos a partir de ahí. Tiene cuatro partes:
- `nodos`: las etapas del mapa.
- `conexiones`: los caminos posibles entre etapas.
- `pasos`: qué pasa en cada etapa.
- `escenarios`: la secuencia de pasos de cada caso.

Si un escenario usa un camino que no existe, el propio tablero muestra un aviso.

Todo el contenido actual es de ejemplo. Cuando quieras, pásame la operación real de TCC: etapas, estados de guía, novedades y los módulos de su software en cada paso. Puede ser en lista, en un documento o como lo tengas, y lo voy incorporando. La página publicada es privada hasta que la compartas, y el archivo `recorrido-remesa.html` lo puedes abrir sin conexión o enviar por correo.

---

# IV. Contexto vigente para continuar con una IA

Esta sección es para quien retome el trabajo con un asistente de IA (Claude Code u otro). Pídele que lea este archivo y el `README.md` antes de hacer cambios.

## Quiénes somos y qué es esto

- Somos **Softcaribbean**, proveedor de software de **TCC** (transportadora colombiana de paquetería y mensajería): desarrollo, soporte y mejoras sobre sus sistemas.
- `Dashboard-TCC.html` es un tablero animado que muestra el recorrido de una remesa etapa por etapa.
- **Objetivo real:** que un desarrollador pueda crear una remesa y llevarla al estado que necesita (por ejemplo, dejarla planeada) sin apoyo de QA. Por eso lo más importante de cada etapa es **el estado que toma la remesa** y **el módulo del sistema por donde hay que pasarla para que cambie de estado**.
- Lo leen desarrolladores del equipo, no el cliente final.

## Decisiones ya tomadas (no volver a discutirlas)

- **Un solo archivo HTML**, sin servidor, sin compilación y sin librerías externas. Lo único que se carga de internet son las fuentes de Google Fonts.
- **Datos separados del dibujo:** la lógica de negocio vive en el objeto `FLUJO`; el motor la dibuja y la anima. Para crecer se editan datos.
- **Estilo sobrio:** fondo neutro, un solo color de acento (ámbar), modo claro y oscuro, adaptable a celular. Sin efectos recargados.
- **Solo el flujo estándar por ahora.** Se quitaron a propósito los demás escenarios de ejemplo (reintento, retenida en oficina, dirección corregida, devolución, contraentrega, envío desde oficina) para que el estándar sirva de plantilla y los flujos reales se implementen bien, uno por uno, con todas sus bifurcaciones.
- **Estado y módulo destacados:** en el detalle de la etapa hay dos recuadros, «Estado de la remesa» (letra grande, con el estado anterior debajo) y «Módulo donde cambia de estado». El módulo se muestra con borde punteado mientras valga «Por definir».

## Qué es real y qué es de ejemplo

| Dato | Situación |
|------|-----------|
| Secuencia general del flujo estándar | Típica de una transportadora de paquetería. **No está validada** contra la operación de TCC. |
| Nombres de los estados (`GUÍA GENERADA`, `RECOLECTADA`, …) | **De ejemplo.** No son los del sistema de TCC. |
| Módulos | Todos en `MODULO_POR_DEFINIR` («Por definir»). |
| Responsables, tiempos, eventos, remesa de muestra | De ejemplo. |

## Reglas para la IA

1. **No inventes lógica de negocio de TCC.** Estados, módulos, novedades y reglas deben venir de quien conoce la operación, del código fuente o de la documentación interna. Si falta un dato, pregunta o déjalo como «Por definir»; nunca lo rellenes con algo verosímil.
2. **Distingue lo confirmado de lo supuesto** al responder y al escribir en el tablero.
3. **Edita datos, no el motor.** Lo normal es tocar solo la sección «LÓGICA DE NEGOCIO». El motor se cambia únicamente cuando hace falta una capacidad nueva, y se explica por qué.
4. **Respeta el formato de un solo archivo** y no agregues dependencias ni pasos de compilación.
5. **El archivo es un fragmento:** empieza en `<title>` y no tiene `<!doctype>`, `<html>` ni `<body>`. Nació como página publicable y así se conserva; no lo «arregles» sin que se pida.
6. **Todo en español**, incluidos nombres de claves, comentarios y textos visibles, como está hoy.
7. **Verifica antes de dar por terminado:** que el script no tenga errores de sintaxis y que al abrir el tablero no aparezca el aviso «Revisa los datos del flujo». Si no pudiste abrirlo en un navegador, dilo.
8. **Mantén al día el `README.md`** (tabla «Estado actual») y esta sección cuando cambie lo que es real, lo pendiente o una decisión.

## Cómo pedirle un flujo nuevo a la IA

Entre más completo el insumo, menos supone la IA. Para cada flujo o bifurcación, entrégale:

- **Nombre del flujo** y en qué punto se separa del estándar.
- **Pasos en orden.** Para cada uno:
  - qué ocurre y quién lo hace;
  - **estado anterior y estado nuevo** de la remesa, con el nombre exacto del sistema;
  - **módulo, pantalla u opción de menú** donde se ejecuta el cambio;
  - condiciones o datos previos necesarios (por ejemplo, que la remesa ya esté planeada o en un manifiesto);
  - qué se registra (eventos, evidencias).
- **Cómo termina** el flujo y si vuelve a unirse al estándar.

Ejemplo de petición: «Agrega el flujo de destinatario ausente. Se separa en reparto. Pasos: … Estados: … Módulos: …».

## Pendientes conocidos

- Reemplazar los estados de ejemplo por los reales de TCC.
- Definir el módulo de cada etapa.
- Documentar la **planeación de la remesa**: qué es, en qué estado deja la remesa y en qué módulo se hace. Es el caso de uso que motivó el tablero.
- Agregar las bifurcaciones reales: novedades, reintentos, devolución, contraentrega, envío desde oficina y las que tenga la operación.
- Evaluar si cada paso necesita un campo de «requisitos previos» o «cómo hacerlo» para que el desarrollador pueda reproducirlo sin ayuda.
