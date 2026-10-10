# Dashboard-TCC

Tablero animado que muestra el recorrido de una remesa de TCC, desde que el remitente pide la recogida hasta que se entrega al destinatario. En cada etapa indica qué ocurre, **qué estado toma la remesa** y **en qué módulo del sistema se hace ese cambio**.

## Para qué sirve

Que cualquier desarrollador del equipo pueda crear una remesa y llevarla al estado que necesita (planeación, despacho, reparto, etc.) sin depender de QA. El tablero es la guía: se elige el flujo, se sigue etapa por etapa y se ve por dónde hay que pasar la remesa para que cambie de estado.

## Cómo usarlo

Abre `Dashboard-TCC.html` en el navegador. No hay nada que instalar ni compilar. Solo necesita internet para cargar las fuentes; sin conexión funciona igual con las fuentes del sistema.

- **Reproducir / Pausar:** botón central o barra espaciadora.
- **Avanzar y retroceder:** botones o flechas del teclado.
- **Saltar a una etapa:** clic en la etapa del mapa o en la línea de etapas.

## Estado actual

| Tema | Situación |
|------|-----------|
| Flujos | Solo el **flujo estándar** (entrega puerta a puerta sin novedades). Sirve de ejemplo para implementar los demás. |
| Estados de la remesa | **De ejemplo.** Faltan los nombres reales del sistema de TCC. |
| Módulos | Todos en **«Por definir»** (recuadro con borde punteado). |
| Responsables, tiempos y eventos | De ejemplo. |

Pendiente por documentar: estados y módulos reales, planeación de la remesa, y las bifurcaciones (novedades, reintentos, devolución, contraentrega, envío desde oficina, entre otras).

## Archivos

| Archivo | Contenido |
|---------|-----------|
| `Dashboard-TCC.html` | El tablero completo: estilos, datos del flujo y motor de animación. |
| `Contexto incial.md` | Historia del proyecto y contexto para continuar el trabajo con una IA. Léelo antes de pedirle cambios a un asistente. |
| `README.md` | Este documento. |

## Cómo agregar lógica de negocio

Toda la lógica está en el objeto `FLUJO`, al inicio del `<script>` (sección «LÓGICA DE NEGOCIO»). El mapa, la animación y los paneles se dibujan solos a partir de esos datos; el «MOTOR DEL TABLERO» no se toca para agregar flujos.

| Parte de `FLUJO` | Qué define |
|------------------|-----------|
| `nodos` | Las etapas del mapa: posición (`x`, `y`), nombre e ícono. |
| `conexiones` | De qué etapa a qué etapa puede pasar una remesa. |
| `pasos` | Qué ocurre en cada etapa (ver campos abajo). |
| `escenarios` | Cada flujo, como una lista ordenada de pasos. |
| `carriles` | Las franjas de fondo (origen, terminales, troncal, última milla). |
| `lienzo.alto` | Alto del mapa. Hay que subirlo cuando una bifurcación se dibuja arriba o abajo de la línea principal. |

Campos de un paso:

| Campo | Significado |
|-------|-------------|
| `nodo` | Etapa del mapa donde ocurre. |
| `titulo`, `descripcion` | Qué pasa, en lenguaje de negocio. |
| `estado` | Estado que toma la remesa en este paso. Se muestra en grande. |
| `modulo` | Módulo del sistema donde se hace el cambio de estado. Usa `MODULO_POR_DEFINIR` mientras no se conozca. |
| `actor` | Quién ejecuta el paso. |
| `tiempo` | Duración típica. |
| `eventos` | Lista de lo que se registra en el paso. |
| `tono` | `normal`, `ok`, `novedad` o `alerta`; cambia el color del estado. |

Para agregar una bifurcación:

1. Crea los nodos nuevos en `nodos` y, si hace falta, sube `lienzo.alto`.
2. Agrega los caminos en `conexiones`.
3. Describe cada paso nuevo en `pasos`.
4. Agrega el escenario en `escenarios`. Puede reutilizar `TRAMO_COMUN` y pasos existentes, cambiando solo lo que difiere con `{ ref:'paso', ...cambios }`.

Si un escenario usa un paso, una etapa o un camino que no existe, el tablero muestra un aviso «Revisa los datos del flujo» con el detalle.

## Reglas para contribuir

- **No inventar lógica de negocio.** Lo que entra al tablero debe venir de la operación real de TCC o del código. Si algo no está confirmado, se deja marcado como ejemplo o «Por definir».
- **Un solo archivo, sin dependencias.** Debe seguir abriéndose con doble clic.
- **Cambiar datos antes que motor.** Solo se toca el motor cuando el tablero necesita una capacidad nueva.
- **Probar antes de subir.** Abre el archivo, recorre todos los escenarios y confirma que no aparece el aviso de datos.
- Al quitar un valor de ejemplo o definir un módulo, actualiza la tabla «Estado actual» de este README.
