# Strike challenge: ejercicio multi-asset

Registro vivo de decisiones de diseño y referencias del benchmark.
Última actualización: 25 de octubre de 2026.

Convención: **Decidido** = lo definiste vos. **Propuesta** = idea mía que no confirmaste de forma explícita. **Pendiente** = falta decidir.

---

## 1. Glosario (términos de trabajo)

| Término | Significado |
|---|---|
| Ejercicio | Revisión de seguridad que agrupa varios assets y los testea como una unidad |
| Asset | Sistema a proteger (sitio web, app, API, servidor) |
| Valores del ejercicio | Campos que solo existen a nivel ejercicio: título, objetivo, fechas y recurrencia, hacker humano |
| Valores globales | Campos del ejercicio que los assets heredan: profundidad y franjas horarias |
| Valores propios del asset | Campos que solo existen a nivel asset: credenciales, rate limit, VPN |
| Asset modificado | Asset que pisó al menos un valor global. En la columna "Herencia" dice "Modificado" |
| Asset heredado | Asset que usa todos los valores globales del ejercicio. En la columna "Herencia" dice "Heredado" |
| Asset pendiente | Asset al que le faltan datos esenciales para el pentest. En la interfaz dice "Pendiente"; "Borrador" es solo del ejercicio |
| Ejecución | Cada vez que el ejercicio corre, según sus fechas y recurrencia; tiene sus propios resultados |

Nota: "propios" se usó para dos cosas distintas (del ejercicio y del asset). Conviene usar siempre "del ejercicio" y "propios del asset".

---

## 2. Decisiones tomadas

### 2.1 Modelo de dos niveles y herencia
- **Decidido.** El ejercicio es el padre y los assets son los hijos.
- **Decidido.** Los assets comparten la configuración global del ejercicio (vínculo vivo): si cambia un valor global, cambia en los assets que no lo pisaron.
- **Decidido.** Un asset puede pisar un valor global. En ese caso queda como "asset modificado".
- **Decidido.** Si el valor del asset coincide con el del ejercicio, no se muestra como modificado.
- **Decidido.** El tag "Modificado" aparece cuando se pisa un campo con valor heredado. No muestra cuántos campos: el detalle se ve al entrar al asset.
- **Decidido.** Volver a heredar: acción "Volver a valor original" junto al campo pisado. Al pasar el mouse muestra cuál es el valor original.
- **Decidido.** El campo pisado lleva un borde del mismo color que el tag.
- **Decidido.** Marca de los valores globales: ícono de planeta (Lucide `globe`, `global-ink`) antes del label de cada campo global, con tooltip al pasar el mouse o con foco. Reemplaza al badge "Global".
- **Decidido.** Los assets que no pisaron ningún global se marcan "Heredado"; los que pisaron alguno, "Modificado". Ambos van en la columna "Herencia".

### 2.2 Clasificación de campos

| Campo | Nivel | Estado |
|---|---|---|
| Título | Ejercicio | Decidido |
| Objetivo | Ejercicio | Decidido |
| Fechas y recurrencia | Ejercicio | Decidido |
| Hacker humano | Ejercicio | Decidido |
| Profundidad | Global (heredable) | Decidido |
| Franjas horarias | Global (heredable) | Decidido |
| Credenciales | Propio del asset | Decidido |
| Rate limit | Propio del asset | Decidido |
| VPN | Propio del asset | Decidido |

- **Decidido.** Los campos nombrados en el brief pueden quedar pendientes al crear, para que el usuario los complete después.
- **Decidido.** Al crear solo se piden dos globales: profundidad y franjas horarias. Credenciales, rate limit y VPN siguen siendo propios del asset y se completan después, por asset o en lote.

### 2.3 Panel lateral
- **Decidido.** La edición usa un único componente de panel lateral. Lo que cambia es el contenido: ejercicio, un asset, varios assets en lote.
- **Decidido.** La creación de un ejercicio usa un modal central (overlay), no el panel lateral. Comunica que es una acción de crear y no de editar, y da más espacio para revisar assets.
- **Decidido.** En el panel del ejercicio, los valores del ejercicio y los globales van juntos en una lista, y los globales llevan un diferencial visual que indica que afectan a todos los assets. Ese diferencial es el ícono de planeta con tooltip, ya usado en el modal de creación, en el detalle y en el panel de un asset; falta aplicarlo en los paneles del ejercicio y del lote.
- **Decidido.** En el panel de un solo asset aparecen los campos propios y también los globales. Editar un global convierte al asset en modificado.
- **Decidido.** Panel de un asset construido en `prototipo/detalle-ejercicio.html`:
  - Se abre con el lápiz de la fila. Entra desde la derecha, ocupa todo el alto y mide 480px (en mobile, todo el ancho). Se cierra con la X, "Cancelar", Esc o clic en el fondo; el foco vuelve al lápiz. Si el asset está Pendiente, el foco cae en las opciones de credenciales.
  - Encabezado: antetítulo "Editar asset", nombre y tipo (ícono y nombre). Sin badges de estado ni de herencia: el estado ya se ve en la fila y el aviso dice qué falta.
  - Aviso de pendiente con la paleta de advertencia mientras falten credenciales; el texto cambia según lo que falta ("Faltan credenciales. Elegí cómo se resuelven para que el asset quede Listo.", "Elegí una credencial del búnker…" o "Completá usuario y contraseña…").
  - "Propios del asset" ("Solo aplican a este asset."): credenciales (ver 2.8); rate limit numérico en "solicitudes por segundo", con default por tipo (Web app 10, API 20, App móvil 10, Dominio 5, IP o red 5; valores en propuesta), ayuda "Sugerido para <tipo>: N. Bajalo si el sistema es sensible a la carga." con subrayado punteado y tooltip "Recomendación del sistema", y error "Ingresá un número mayor a 0."; VPN en desplegable ("Sin VPN" o una existente).
  - "Valores globales" ("Vienen del ejercicio. Si cambiás uno, este asset queda como Modificado."): Profundidad y Franjas horarias, con planeta y tooltip "Valor global del ejercicio. Si lo cambiás acá, este asset queda como Modificado." Si el valor difiere del ejercicio, el control toma el borde `modified-border` y aparece "Volver a valor original" con tooltip "Valor original: X". Volver al valor del ejercicio lo deja sin modificar.
  - Pie: solo "Cancelar" y "Guardar cambios". Descartado: la vista previa "Al guardar" con badges, porque sumaba ruido y el resultado ya se comunica al guardar.
  - Al guardar: Listo si las credenciales están resueltas, si no Pendiente; Modificado si algún global difiere del ejercicio, si no Heredado. La fila se actualiza y un toast confirma "Guardaste los cambios de <asset>.". Si el ejercicio estaba en Borrador, tiene fechas y no queda ningún asset Pendiente, pasa a Activo y el toast suma "El ejercicio pasó a Activo."
  - Si cambió algún global respecto de lo que tenía el asset, "Guardar cambios" abre un diálogo central "Modificaste un valor global": por campo, el valor anterior tachado, una flecha y el nuevo; luego "<asset> deja de usar el valor del ejercicio y queda como Modificado. El resto de los assets no cambia." (o "…vuelve a usar los valores del ejercicio y queda como Heredado."). Botones "Volver" (conserva lo editado; foco inicial) y "Confirmar y guardar". Sin cambios de global, guarda directo. Es distinto de la confirmación del panel del ejercicio (2.4) y no muestra resumen de impacto.
- **Decidido.** El modal de creación solo configura el punto de partida. Después se edita por asset o en lote.

### 2.4 Edición de valores globales desde el ejercicio
- **Decidido.** Mientras se edita un campo global, el panel muestra un resumen de impacto en vivo. Ese resumen solo dice a cuántos assets no afecta (los que tienen valor propio).
- **Decidido.** El resumen cumple el rol de advertencia.
- **Decidido.** Al guardar aparece una confirmación breve que menciona cuántos assets no se verán afectados y una lista corta de sus nombres. Todavía no está construida. El resumen de impacto y esta confirmación son del panel del ejercicio; el panel de un asset no los usa.

### 2.5 Edición en lote
- **Decidido.** Cada fila de asset se puede seleccionar. Al seleccionar aparece la opción de editar.
- **Decidido.** Si los assets seleccionados tienen valores distintos en un campo, se muestra "Múltiples valores". El usuario elige un valor único que se aplica a todos.

### 2.6 Lista, detalle, búsqueda y diagrama
- **Decidido.** Pantalla inicial: lista de ejercicios en formato de filas. Las filas no se despliegan: para ver los assets de un ejercicio hay que hacer clic y entrar al detalle. CTA claro "Crear ejercicio".
- **Decidido.** Clic (o Enter) en la fila de un ejercicio abre su detalle: configuración, assets y cambio de vista entre lista y diagrama.
- **Decidido.** Buscador dentro del ejercicio por nombre, tipo o VPN.
- **Decidido.** Detalle construido en `prototipo/detalle-ejercicio.html`:
  - Ruta "Ejercicios / nombre"; título y badge de estado con tooltip.
  - Acciones según el estado: Activo, "Editar ejercicio", "Pausar" y "Corrida manual"; Pausado, "Editar ejercicio" y "Reanudar"; Borrador, solo "Editar ejercicio". Sin botón "Activar".
  - En Activo y Pausado: "Última ejecución · Próxima · Historial de corridas" ("En pausa" como próxima si está pausado).
  - En Borrador: aviso "N assets pendientes: faltan credenciales." (y "Faltan las fechas." si corresponde) "El ejercicio pasa a Activo cuando esté completo.", con "Ver pendientes", que aplica el filtro.
  - Configuración en una grilla de cinco elementos (Objetivo, Fechas y recurrencia, Hacker humano; Profundidad y Franjas horarias con planeta y tooltip "Valor global: se aplica a todos los assets. Un asset que lo cambia queda como Modificado."). Sin enlace "Editar" y sin línea divisoria.
  - Assets: total, cambio Lista / Diagrama y "Agregar assets".
  - Botón "Filtrar" con ícono que abre un menú: Estado (Listo, Pendiente) y Herencia (Heredado, Modificado), con conteos; se suman dentro de un grupo y se combinan entre grupos; contador en el botón, "Limpiar filtros", cierre con Esc o clic afuera. Reemplaza a los filtros separados "Modificados" y "Borradores".
  - Casillas con "seleccionar todos" sobre lo visible y barra "N seleccionados · Quitar selección · Editar". La barra y las filas seleccionadas quedan en `global-bg` (se probó junto al planeta y se mantiene).
  - Columnas: casilla, Asset (con la VPN debajo), Tipo (ícono y nombre), Estado, Herencia y un lápiz al final que abre el panel de ese asset (ver 2.3). Al pie, "Mostrando N de M assets".
  - Ícono del tipo "Web app": `app-window`, porque el planeta quedó para lo global.
- **Decidido.** Vista diagrama: ejercicio (padre) y assets (hijos), con un nivel intermedio de grupos. Por defecto los assets se agrupan por tipo. Los assets modificados se conectan de forma distinta (punteada u otro color).
- **Decidido.** La relación entre assets queda para más adelante.

### 2.7 Agregar assets
- **Decidido.** Duplicar un asset: nace con los mismos valores del original, esté o no modificado.
- **Decidido.** Añadir un asset: empieza de cero.
- **Decidido.** Se puede guardar la configuración de valores propios de un asset para reutilizarla en otros.

### 2.8 Estados y credenciales
- **Decidido.** Asset pendiente = le faltan datos esenciales. El sistema muestra con claridad esos estados: "Listo" o "Pendiente" (tooltip "Faltan credenciales para poder testearlo"). "Borrador" queda reservado para el estado del ejercicio.
- **Decidido.** "Búnker de credenciales": permite buscar credenciales ya usadas para reutilizarlas.
- **Decidido.** Se revisó la seguridad de guardar credenciales (ver sección 7.5).
- **Decidido.** Elección explícita de credenciales en el panel del asset: "Cargar credenciales" ("Elegí una del búnker o cargá nuevas.") o "No requiere login" ("Se testea sin iniciar sesión."). Descartado: "Las aporta otra persona", porque dejaba el asset pendiente igual; con dos opciones, las dos lo dejan Listo.
- **Decidido.** "Cargar credenciales" usa pestañas "Del búnker" y "Nuevas", con el mismo diseño de las de la lista y del modal. Del búnker: desplegable "Credencial guardada" ("Se buscan por nombre, nunca por valor."). Nuevas: usuario, contraseña enmascarada con botón para mostrarla y "Guardar en el búnker para reutilizarlas" marcado por defecto. Debajo: "Se guardan cifradas y siempre enmascaradas. Revelarlas o copiarlas queda registrado."

### 2.9 Creación del ejercicio
- **Decidido.** Los assets ya existen en un ítem aparte del menú, y también se pueden crear nuevos mientras se crea un ejercicio.
- **Decidido.** Caminos para agregar assets: dos opciones. "Elegir existentes" e "Importar nuevos" (escribir o pegar una lista, o subir un archivo; cargar a mano es escribir una línea). Ambas formas de crear expresan lo mismo: traer assets desde fuera del sistema.
- **Decidido.** Si hay filas con error en una importación, se importan las válidas y se señalan las demás.
- **Decidido.** El tipo de asset lo infiere el sistema y el usuario puede modificarlo.
- **Decidido.** La verificación de propiedad del asset no entra en este flujo.
- **Decidido.** El modal de creación tiene dos pasos: (1) valores del ejercicio y valores globales (profundidad y franjas horarias), (2) solo assets: elección de existentes o importación de nuevos. Reemplaza a la división anterior, en la que los globales iban en el paso 2 (ver sección 4).
- **Decidido.** Bajo el título del modal va una descripción corta de qué hacer en cada paso. Línea de pasos: "1 Valores del ejercicio y globales > 2 Assets".
- **Decidido.** Tamaño: paso 1 de 600px de ancho que crece hasta un alto máximo de 944px; paso 2 de 880px de ancho y alto fijo de 944px (en pantallas más bajas, el alto de la ventana menos 32px). Si el contenido no entra, scrollea por dentro.
- **Decidido.** Descartado: la franja fija de globales al pie del paso 2 y la línea "Se aplica a N assets". El alcance lo explica el tooltip de la marca de global (hoy, el planeta junto a cada campo).
- **Decidido.** "Confirmar assets" ocurre dentro del mismo paso, sin ventanas encima del modal: la tabla de assets hace de confirmación.
- **Decidido.** Objetivo: desplegable de opciones (no etiquetas), opcional y sin marca de "Recomendado". Al elegirlo, el sistema infiere un nivel de profundidad y lo aclara al usuario, para mostrar que el sistema es inteligente.
- **Decidido.** Opciones del desplegable de objetivo: antes de un lanzamiento, auditoría o cumplimiento, revisión periódica, cambio importante en el sistema, otro.
- **Decidido.** Con "Otro" aparece un área de texto obligatoria para describir el objetivo, y el sistema no sugiere profundidad.
- **Decidido.** Título obligatorio, marcado con asterisco rojo; recurrencia "Una vez" por defecto; hacker humano con switch redondo, apagado por defecto. Fechas opcionales: sin fechas el ejercicio queda en borrador.
- **Decidido.** Bloque de globales en el paso 1. Cada campo global lleva el planeta antes del label, con tooltip: "Valor global: se aplica a todos los assets que agregues. Después podés modificarlo desde cada asset."
- **Decidido.** Profundidad: toma el valor sugerido por el objetivo y muestra "Sugerida por el sistema", con tooltip que dice que se infirió a partir del objetivo.
- **Decidido.** Franjas horarias: "Cualquier horario" o un rango Desde/Hasta. El fin tiene que ser distinto del inicio; se permite cruzar la medianoche.
- **Decidido.** Paso 2: pestañas "Elegir existentes" e "Importar nuevos" con el diseño de las pestañas de la lista y un contador de assets sumados desde cada una. Por defecto se abre "Elegir existentes".
- **Decidido.** "Elegir existentes" es un combobox: despliega los assets disponibles al hacer foco, filtra al escribir, oculta los ya sumados y se usa con teclado.
- **Decidido.** Tabla de assets con scroll interno y encabezado fijo; estado vacío que llena el espacio disponible. Una fila "Ya existe" cuenta como asset recién al tocar "Usar existente".
- **Decidido.** Se puede crear un ejercicio sin assets: queda en borrador.
- **Decidido.** VPN y rate limit no se suman al modal ni a un tercer paso: son propios del asset y se completan después, en lote cuando se repiten. Las credenciales se reutilizan desde el búnker.
- **Decidido.** El ejercicio se programa al crearse; no hay un paso de lanzar. Cada vez que corre es una ejecución.
- **Decidido.** Estados del ejercicio: Borrador, Activo y Pausado. Pausado suspende temporalmente la corrida configurada.
- **Decidido.** El detalle del ejercicio muestra la última ejecución.
- **Decidido.** Hay una opción de historial de corridas de pentest, para ver fechas, insights del sistema y más. El contenido queda por definir.
- **Decidido.** Al pasar el mouse sobre el badge de estado aparece un tooltip que explica qué significa ese estado.
- **Decidido.** La acción del encabezado para correr el pentest a mano se llama "Corrida manual".
- **Decidido.** Clic en un nodo del diagrama abre el panel lateral de edición del asset, igual que desde la lista.
- **Decidido.** En el diagrama no hay assets plegados: se muestran todos. Vienen agrupados por tipo de asset por defecto.
- **Decidido.** El usuario podrá crear grupos personalizados a su criterio, para dar más control y personalización. Los grupos solo organizan: no llevan configuración, así que la herencia sigue siendo de dos niveles.
- **Decidido.** Los assets que se crean o importan al crear el ejercicio nacen con valores por defecto del sistema según su tipo. Las credenciales quedan pendientes, así que nacen como assets pendientes y se completan después (en la tabla del modal la fila todavía dice "Nuevo · borrador").
- **Decidido.** Lo anterior solo ocurre con assets nuevos. Si se eligen assets ya cargados, conservan sus datos.

---

### 2.10 Alcance del challenge
- **Decidido.** Es un challenge de diseño para un puesto de product designer: se busca profundidad en lo central y no cubrir todas las variables de la funcionalidad.
- **Decidido.** Núcleo (mockup final y análisis): lista de ejercicios; modal de creación (paso 1 con valores del ejercicio y globales; paso 2 con assets, con importar y confirmar integrados); detalle con lista y diagrama; panel de edición (ejercicio con globales marcados, resumen de impacto y confirmación; asset con campo pisado; lote con "Múltiples valores").
- **Decidido.** Se muestran simples, como detalles dentro de los mockups del núcleo y sin pantalla propia: estados con tooltip, última ejecución, columna "Herencia" y filtro, objetivo que sugiere profundidad.
- **Decidido.** Solo se mencionan, como "Extensiones pensadas, fuera del alcance de los mockups": historial de corridas, corrida manual, grupos personalizados, búnker de credenciales, relación entre assets, conectores con la nube y múltiples credenciales por asset.
- **Decidido.** Pantallas recortadas: diálogo de la corrida manual, selector del búnker, lista de assets agrupada, resumen final antes de confirmar, confirmación al cerrar el modal y versión responsive.
- **Decidido.** El set de pantallas quedó cerrado por ahora (ver 2.11).
- **Decidido.** Simplificaciones: el paso "Revisar cambios" en el lote se descarta y las medidas del búnker se resumen en dos líneas.
- El documento `solucion-challenge-fuente-de-verdad.md` contiene la versión consolidada.

---

### 2.11 Set de pantallas cerrado por ahora
- **Decidido.** Pantallas y estados incluidos: lista de ejercicios; paso 1 del modal de creación (valores del ejercicio y globales); paso 2 (estado vacío, elección de existentes, ingreso de "Importar nuevos" y tabla de confirmación); detalle del ejercicio en Activo y sus variantes en Pausado y Borrador; vista diagrama con grupos por tipo; panel lateral de edición (ejercicio con globales marcados e impacto, asset con campo pisado, confirmación al guardar); panel de edición en lote.
- **Decidido.** Quedan fuera, como extensiones sin mockup: historial de corridas, diálogo de la corrida manual, selector del búnker, lista de assets agrupada, resumen final antes de confirmar, confirmación al cerrar el modal, versión responsive, estado vacío de la lista de ejercicios y estados de lectura del archivo.
- Siguiente paso: pasar los bocetos a mockups finales. El modal de creación ya está cerrado en `prototipo/lista-ejercicios.html` y el detalle, con el panel de edición de un asset, en `prototipo/detalle-ejercicio.html`. Faltan panel del ejercicio, edición en lote, diagrama, corrida manual e historial; en el detalle esas acciones todavía no están conectadas. El documento `solucion-challenge-fuente-de-verdad.md` es la versión completa y vigente.

---

## 3. Propuestas sin confirmación explícita
- Exclusiones aditivas: el asset solo puede sumar exclusiones; quitar una global requiere confirmación. (Descartado: se quitó Exclusiones del alcance.)
- En la edición en lote, solo se aplican los campos que el usuario tocó.
- Paso "Revisar cambios" antes de guardar en lote. (Descartado: alcanza con la confirmación al guardar.)
- La configuración guardada de un asset no incluye credenciales (se vinculan aparte desde el búnker).
- Búnker: buscar por nombre o etiqueta (nunca por valor), mostrar dónde se usa cada credencial, sugerir vencimiento ligado al fin del ejercicio, valores enmascarados y copiar o revelar queda registrado.
- Credenciales: elección explícita entre "Cargar", "No requiere login" y "Las aporta otra persona", para que un asset sin login no quede pendiente para siempre. (Confirmado en el panel del asset con dos opciones; "Las aporta otra persona" se descartó, ver 2.8.)
- Un ejercicio con assets pendientes no se puede lanzar hasta completarlos.
- Hacker humano a nivel ejercicio, con posible refinamiento por asset en el futuro.
- Conectores con la nube y descubrimiento automático: mencionarlos en el documento de análisis como extensiones futuras, sin diseñarlos.
- Importar nuevos (confirmado en el prototipo, ver 2.9): área de texto con un asset por línea, "Subir archivo" y "Descargar plantilla"; las filas entran a la misma tabla de assets, que hace de confirmación; estados de fila Existente, Nuevo · borrador, Ya existe (con "Usar existente") y Error (se omite); resumen con conteos; el área de texto se compacta tras agregar.
- Estado vacío del paso 2 (confirmado en el prototipo): zona punteada para soltar un archivo o hacer clic, con enlaces para escribir o pegar y descargar la plantilla; bloque vacío que llena el espacio con un mensaje. Pendiente: estados de lectura del archivo (leyendo, error con reintentar, listo).
- Al cerrar el modal con datos cargados, pedir confirmación o guardar un borrador automático.
- En pantallas chicas, el modal pasa a pantalla completa.
- Corrida manual: disponible solo en estado Activo; es una ejecución extra que no mueve la programación; pide confirmación mostrando qué se va a ejecutar. Útil, por ejemplo, para volver a testear después de corregir hallazgos.
- Transiciones: Borrador pasa a Activo cuando queda completo, sin botón "Activar" (confirmado en el detalle, ver 2.6); Activo y Pausado se alternan con "Pausar" y "Reanudar". Un ejercicio está en Borrador mientras falten fechas o tenga assets pendientes.
- Programado, En curso y Finalizado se tratan como estados de cada ejecución, no del ejercicio.
- Textos de tooltip de estado: Borrador (faltan datos para activarlo), Activo (corre según sus fechas y recurrencia), Pausado (suspendido hasta que lo reanudes).
- Vista diagrama: árbol horizontal en tres niveles (ejercicio, grupos, assets); líneas punteadas del color del tag para los modificados; controles de zoom y leyenda al pie. El zoom y el desplazamiento resuelven la escala sin plegar. Control "Agrupar por" y botón "Nuevo grupo" en la barra. Alternativa: disposición vertical.
- Lista de ejercicios: cada fila muestra nombre y objetivo, estado (con tooltip), frecuencia, última ejecución y composición de assets por tipo (íconos con conteo), con marcas de modificados y de assets pendientes ("N pendientes"). Toda la fila es clicable (también con Enter) y abre el detalle; una flecha al final lo indica. Arriba: buscador, filtro por estado y botón "Crear ejercicio".
- Un grupo sirve como atajo para seleccionar todos sus assets en la edición en lote.

---

## 4. Opinión sobre la división de pasos del modal de creación

División vigente (Decidido tras probarla en el prototipo): paso 1 con campos del ejercicio y globales; paso 2 solo con assets.

Antes era: paso 1 con campos del ejercicio y paso 2 con assets y globales, con una franja fija de globales al pie. Al verla en el prototipo, el paso 2 quedaba cargado con una tarea de tabla y una de formulario.

A favor de la división vigente:
- Cada paso hace una sola cosa: configurar en el 1, sumar assets en el 2.
- Valores del ejercicio y globales quedan juntos, igual que en el panel de edición del ejercicio.
- El paso 2 tiene todo el alto para la tabla, que escala mejor con muchos assets.

A cuidar:
- Los globales se definen antes de ver a qué assets se aplican. Lo compensan el tooltip del planeta de cada campo global y la posibilidad de modificarlos desde cada asset.
- Ya no se puede sugerir profundidad o franjas según los tipos de asset detectados; la sugerencia sale solo del objetivo.
- Los assets nuevos nacen con defaults por tipo y credenciales pendientes (ver 2.9).

---

## 5. Pendientes y preguntas abiertas

Diseño y flujo
- Si "Confirmar assets" sobra cuando se importa un solo asset (podría mostrar el tipo inferido directamente).
- Estados del ejercicio (borrador, listo, en ejecución, etc.) y cómo se ven en la lista.
- Qué datos resume cada fila en la lista de ejercicios (hay una propuesta en la sección 3).
- Pantalla de revisión antes de lanzar.
- Qué pasa si el filtro cambia con assets ya marcados: hoy la selección se conserva y "seleccionar todos" actúa sobre lo visible. El contador ya está en la barra de selección.
- Si "Volver a valor original" usa el mismo texto en la edición en lote.

Visual
- Cómo se aplica el planeta de global en los paneles del ejercicio y del lote (en el panel de un asset ya está resuelto) y cuál es el diferencial de los campos solo del ejercicio.
- El marcador de global significa cosas distintas en cada panel ("afecta a todos" en el ejercicio, "viene del ejercicio" en el asset). Conviene que se lean como parte de la misma familia.
- Cuando los assets no afectados son muchos: cuántos nombres mostrar y adónde lleva "+N más".
- Accesibilidad del tooltip del valor original (teclado y pantallas táctiles).

Alcance
- Escala: cómo se ven la lista y el diagrama con 40 assets.
- Perfiles de usuario: defaults, explicaciones y nivel de ayuda para quien no es técnico.
- Casos borde: un asset en más de un ejercicio, ejercicios recurrentes cuando cambian los assets, guardar borrador y retomar.
- Relación entre assets en el diagrama (recordatorio pedido; ahora ligada a los grupos).
- Cómo se ve la lista de assets agrupada ("Agrupar por") y qué pasa al seleccionar un grupo en la edición en lote.
- Qué estado tiene un ejercicio de una sola vez cuando ya corrió, con solo tres estados (Borrador, Activo y Pausado).
- Si la corrida manual respeta las franjas horarias y si está disponible en Pausado.
- Qué pasa al cambiar assets o valores globales mientras una ejecución está en curso.
- Reformular la "pantalla de revisión antes de lanzar" como resumen final antes de confirmar la creación, o integrarla al detalle.
- Redibujar el flujo general: las cajas "Advertencia" y "Elegir qué hacer" quedaron obsoletas.
- Elegir el momento "aha" para el prototipo.

---

## 6. Supuestos
- Los assets ya existen en la plataforma y se gestionan en un ítem aparte del menú.
- No se diseña la verificación de propiedad del asset.
- La clasificación de campos sale de razonar el problema, no de conocer el funcionamiento interno de Strike.
- Se usan textos placeholder (el brief lo permite).
- Hay un solo tipo de usuario por ahora; los roles distintos quedan sin explorar.

---

## 7. Ideas candidatas para el "¡Sorprendenos!"
- Resumen de impacto en vivo al editar un campo global.
- Columna "Herencia" (Heredado, Modificado) con su filtro: herencia visible.
- Defaults inteligentes: el sistema infiere el tipo de asset y sugiere valores.
- Alta masiva de assets con revisión previa.
- Vista diagrama del ejercicio.

---

## 8. Referencias del benchmark

### 8.1 Pentest y agrupación de assets

| Plataforma | Qué hace | Qué tomamos |
|---|---|---|
| Cobalt | Un pentest se lanza sobre un solo asset. El asset se define una vez y se reutiliza. Al copiar un pentest se puede heredar el scope. Ofrece modos de pentest (acotado vs. amplio) | Asset desacoplado del trabajo. Defaults por intención. Es la limitación que Strike quiere superar |
| Horizon3 NodeZero | Scope con alta guiada o carga masiva por CSV o texto (crear o reemplazar). Límites explícitos. Opción de autorizar assets descubiertos. Plantillas y ejecuciones recurrentes | Alta masiva. Decisión que el sistema propone y el usuario confirma |
| HackerOne | Gestión de scope unificada entre programas y pentests | Scope como entidad reutilizable |
| Strobes | El scope funciona como "resumen del pedido" y se revisa antes de enviar | Modelo para la pantalla de revisión antes de lanzar |
| Bugcrowd | Separa targets dentro y fuera de scope, y reglas de engagement aparte | Separar qué se testea de las reglas |

### 8.2 Herencia de configuración
| Plataforma | Qué hace | Qué tomamos |
|---|---|---|
| Burp Suite DAST | Configuración por carpetas, subcarpetas y sitios, donde los hijos heredan. La configuración a nivel schedule pisa la del sitio. Varias configuraciones se pueden apilar de general a específica. Muestra solo el delta de cada configuración | Herencia padre a hijo, el más específico gana. Mostrar solo lo que cambia. Lección: los overrides dispersos son difíciles de mantener |
| Intruder | Targets con etiquetas y alta masiva (incluye rangos CIDR). Vista de superficie que resalta cambios desde el último escaneo | Agrupar por etiquetas. Señalar qué cambió |

### 8.3 Importación de assets
| Plataforma | Qué hace | Qué tomamos |
|---|---|---|
| Burp Suite DAST | Importación por CSV con plantilla descargable y carpeta de destino. Carga masiva de APIs con valores comunes para todos, revisión de lo que se va a crear, casillas para quitar, registro de errores de lectura y herencia de la carpeta | Valores comunes al importar. Paso de revisión. Quitar filas |
| Detectify | Alta manual, conectores con la nube (recomendados), archivo de zona o transferencia DNS, y API para sincronizar en ambos sentidos. Apex Discovery propone candidatos para revisar y confirmar | El sistema propone, el usuario decide |
| Intruder | Alta manual y sincronización con AWS, Azure y GCP. Reglas de sincronización selectiva, importación de etiquetas, aviso cuando un asset deja de existir, detección de APIs y logins | Extensiones futuras. Inferencia del tipo de asset |
| Qualys CSAM | Lo importado llega como "no gestionado" hasta que se activa. Reglas para evitar duplicados | Estado intermedio (borrador). Detección de duplicados |

### 8.4 Edición en lote
- Basis Design System: contador de seleccionados, encabezado con contexto, vista previa para ediciones complejas y confirmación.
- Backup Radar: cada campo tiene una casilla para cambiar; solo se aplican los marcados.
- SmartPass (pedido de usuarios): reclamo porque una edición en lote sobrescribió ajustes que no se habían tocado.

### 8.5 Importación por archivo (UX) y credenciales
- Patrón: archivo, mapear columnas, validar y enviar. Errores por fila y columna, CSV de errores descargable, errores vs. advertencias, decisión de bloquear o importar las válidas.
- Credenciales: OWASP indica centralizar, mínimo privilegio y vencimiento. Orderful y Simpplr muestran valores enmascarados, también después de guardar, y copiar se audita igual que revelar. Cobalt y Pensar recomiendan cuentas dedicadas por rol, nunca de producción, y borrarlas al terminar.

### 8.6 Enlaces
- Cobalt: https://docs.cobalt.io/assets/create-asset · https://docs.cobalt.io/articles/assets-are-what-we-pentest-09RgsvQ7zv · https://docs.cobalt.io/articles/scope-test-period-dvnBEgzJgm · https://docs.cobalt.io/articles/pentest-details-LXoQDryEPD · https://docs.cobalt.io/getting-started/pentest-objectives/test-credentials
- Horizon3: https://docs.horizon3.ai/portal/test_types/external/create_asset_group/ · https://docs.horizon3ai.com/portal/test_types/external/create_asset_group
- Burp Suite DAST: https://portswigger.net/burp/documentation/scanner/scan-configurations · https://portswigger.net/burp/documentation/enterprise/working/scans/scans-page · https://portswigger.net/burp/documentation/enterprise/working/scans/scan-configs · https://portswigger.net/burp/documentation/dast/user-guide/scanning-web-apps/importing-sites-in-bulk · https://portswigger.net/burp/documentation/dast/user-guide/scanning-apis/bulk-uploading-apis
- Intruder: https://help.intruder.io/portal-help/getting-started · https://help.intruder.io/portal-help/what-is-cloudbot · https://blog.intruder.io/blog/whats-new-product-updates-from-intruder-april-2024 · https://truto.one/integrations/detail/intruder.md
- Detectify: https://docs.detectify.com/getting-started/adding-assets · https://docs.detectify.com/getting-started/asset-verification · https://blog.detectify.com/product-updates/apex-discovery-to-secure-every-domain · https://support.detectify.com/support/solutions/articles/48001061324-adding-a-new-asset
- Qualys: https://blog.qualys.com/product-tech/2024/02/05/csam-strengthens-attack-surface-coverage-and-risk-assessment-with-third-party-connectors
- HackerOne (resumen): https://ciso2ciso.com/?p=149126
- Strobes: https://help.strobes.co/en/article/defining-engagement-scope
- Bugcrowd: https://docs.bugcrowd.com/researchers/engagement-management/details/
- Edición en lote: https://design.basis.com/bulk-editing · https://help.backupradar.com/hc/en-us/articles/54753747259035-Understanding-Bulk-Edit-behavior-in-Backup-Radar · https://smartpass.nolt.io/1761
- UX de importación: https://blog.csvbox.io/file-upload-patterns/ · https://blog.csvbox.io/row-level-errors-csv/ · https://blog.csvbox.io/skip-invalid-rows/ · https://appmaster.io/blog/safe-bulk-imports-preview-validate-commit · https://samvera.atlassian.net/wiki/spaces/hyku/pages/3572826114/Bulk+Import+Improvements+Bulkrax+FUNDRAISING+ACTIVE
- Credenciales: https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet · https://docs.orderful.com/docs/credential-vault · https://help.simpplr.com/en-US/simpplr/article/rUBClIbH-masked-credential-fields-for-integrations-and-app-tile-configurations · https://docs.pensar.dev/console/features/authentication
