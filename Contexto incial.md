# Contexto inicial

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