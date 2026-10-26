# Solución del challenge: ejercicio multi-asset (Strike)

Fuente de verdad de la solución. Reúne lo pensado, lo definido y los requisitos elegidos. Es un documento vivo: cuando algo cambia, se cambia acá.
Última actualización: 25 de octubre de 2026 (modal de creación y detalle del ejercicio cerrados a partir del prototipo; panel de edición de un asset construido). Set de pantallas cerrado por ahora (ver secciones 14 y 15.1).

**Cómo leer los estados**
- **Decidido:** lo definiste vos.
- **Propuesta:** idea mía que no confirmaste de forma explícita. Hay que validarla o descartarla.
- **Pendiente:** falta decidir o diseñar.

---

## 1. Resumen

**Problema.** Hoy la plataforma trabaja asset por asset, pero los clientes quieren una revisión de seguridad coordinada sobre varios sistemas conectados (web, app, API).

**Solución en una frase.** Un ejercicio que agrupa assets y convive con ellos en dos niveles de configuración: lo global se define una sola vez en el ejercicio y los assets lo heredan; cada asset puede tener excepciones visibles y reversibles.

**Qué la distingue**
1. La herencia es visible: un tag marca los assets con excepciones, un filtro los encuentra y una acción vuelve al valor original.
2. El impacto se ve antes de guardar: al cambiar un valor global, el panel dice a cuántos assets no afecta.
3. El sistema propone y el usuario decide: infiere el tipo de asset, sugiere una profundidad según el objetivo y arma defaults por tipo.
4. Un solo componente de edición (panel lateral) sirve para el ejercicio, un asset y varios en lote.
5. Escala de 2 a 40 assets: importación masiva, edición en lote, buscador, filtros, grupos y vista diagrama.

---

## 1.1 Alcance del challenge: qué se diseña y qué solo se menciona

**Decisión de enfoque (Decidido).** Es un challenge de diseño para un puesto de product designer. Se busca profundidad en lo central y no cubrir todas las variables posibles de la funcionalidad.

**Criterio.** Cada elemento pasa por una pregunta: ¿es necesario para contar cómo se resuelven los dos niveles, la escala o los perfiles distintos? Si sí, se diseña. Si no, se menciona en una línea como decisión o próximo paso, sin pantalla propia.

| Nivel | Qué entra | Cómo se presenta |
|---|---|---|
| Núcleo | Lista de ejercicios. Modal de creación (paso 1 con valores del ejercicio y globales; paso 2 con assets, con importar y confirmar integrados). Detalle con lista y diagrama. Panel de edición: ejercicio con globales marcados, resumen de impacto y confirmación; asset con campo pisado; lote con "Múltiples valores" | Mockup final y análisis |
| Se muestra, pero simple | Estados con tooltip. Última ejecución. Tag "Modificado" y filtro. Objetivo que sugiere profundidad | Como detalles visibles dentro de los mockups del núcleo, sin pantalla propia (Decidido). Ya aparecen en los bocetos vigentes |
| Solo mencionado | Historial de corridas, corrida manual, grupos personalizados, búnker de credenciales, relación entre assets, conectores con la nube, múltiples credenciales por asset | Sección "Extensiones pensadas, fuera del alcance de los mockups" |

**Se recortan** (se mencionan, sin mockup): diálogo de la corrida manual, selector del búnker, lista de assets agrupada, resumen final antes de confirmar, confirmación al cerrar el modal y versión responsive.

**Se simplifican:** el paso "Revisar cambios" en el lote se descarta (alcanza con la confirmación al guardar); las medidas de seguridad del búnker se resumen en dos líneas en el análisis.

**Quedan fuera por ahora (Decidido):** estado vacío de la lista de ejercicios, historial de corridas simple y los estados de lectura del archivo (leyendo, error con reintentar, listo). Se mencionan como extensiones.

**Ideas consideradas y no incorporadas** (pueden mencionarse como siguientes pasos): múltiples credenciales por asset según el rol, configuración guardada ligada al tipo, aviso de tipos mezclados en el lote y mostrar qué defaults recibió cada tipo.

Las decisiones sobre lo que solo se menciona siguen vigentes y registradas en este documento. Lo único que cambia es que no se diseñan pantallas para ellas.

---

## 2. Contexto y problema (del brief)

- **Asset:** sistema que el cliente quiere proteger (sitio web, app, API, servidor).
- **Ejercicio:** revisión de seguridad (pentest) que agrupa varios assets y los testea como una unidad.
- **Desafío central:** conviven dos niveles de configuración. Algunas decisiones son del ejercicio completo y otras de cada asset.
- **Tres desafíos:** (1) dos niveles que conviven sin confundir al usuario, (2) alcance variable de 2 a 40 assets, (3) perfiles técnicos y no técnicos.
- **Objetivo:** diseñar la experiencia de crear un ejercicio multi-asset, desde el alta hasta agregar y configurar assets.
- **Se evalúa:** flujo de creación, pensamiento estratégico, usabilidad y UX, calidad visual y de UI.
- **Entregables:** análisis y propuesta (PDF), wireframes o mockups, prototipo interactivo (opcional). Plazo: 3 días.
- **No se necesita experiencia en ciberseguridad.** Se pueden usar textos placeholder.

---

## 3. Usuarios

- **Usuario técnico:** sabe qué es un rate limit o una VPN. Quiere control y velocidad.
- **Usuario no técnico:** no conoce esos términos. Necesita defaults, explicaciones cortas y que el sistema lo guíe.
- **Por ahora hay un solo tipo de usuario.** Los roles distintos (por ejemplo, quien decide el alcance y quien aporta credenciales) quedan sin explorar.

---

## 4. Principios de diseño (derivados de las decisiones)

1. **Definir una vez, mostrar las excepciones.** Lo común se configura arriba; lo distinto se marca.
2. **El sistema propone, el usuario decide.** Inferencia de tipo, profundidad sugerida, defaults por tipo.
3. **El impacto, siempre a la vista.** Se ve qué se afecta antes de guardar.
4. **Solo cambia lo que se toca.** Sobre todo en la edición en lote.
5. **Un componente, distinto contenido.** El panel lateral es siempre el mismo.
6. **Lo incompleto se dice con claridad.** Borradores y estados visibles, sin bloquear el avance.
7. **Las credenciales se tratan como sensibles desde el diseño.**

---

## 5. Modelo conceptual y glosario

| Término | Significado |
|---|---|
| Ejercicio | El plan: qué se testea (assets), cómo (profundidad) y cuándo (fechas, recurrencia) |
| Ejecución (corrida) | Cada vez que el ejercicio corre, según sus fechas y recurrencia. Tiene sus propios resultados |
| Asset | Sistema a proteger |
| Grupo | Agrupación de assets para organizarlos. No lleva configuración |
| Valores del ejercicio | Campos que solo existen en el ejercicio: título, objetivo, fechas y recurrencia, hacker humano |
| Valores globales | Campos del ejercicio que los assets heredan: profundidad y franjas horarias |
| Valores propios del asset | Campos que solo existen en el asset: credenciales, rate limit, VPN |
| Asset modificado | Asset que pisó al menos un valor global. En la columna "Herencia" se marca "Modificado" |
| Asset heredado | Asset que usa todos los valores globales del ejercicio, sin pisar ninguno. En la columna "Herencia" se marca "Heredado" |
| Asset pendiente | Asset al que le faltan datos esenciales para el pentest (por ejemplo, credenciales). En la interfaz se marca "Pendiente"; "Borrador" queda reservado para el estado del ejercicio |

Nota de lenguaje: usar siempre "del ejercicio" y "propios del asset", no "propios" a secas.

**Relación ejercicio y ejecuciones (Decidido).** El ejercicio es el plan y se programa al crearse; no hay un paso de lanzar. Cada vez que corre, es una ejecución. Un ejercicio de una sola vez tiene una ejecución; uno recurrente tiene varias. Los resultados viven en cada ejecución. Agregar o quitar assets, o cambiar un valor global, afecta a las próximas ejecuciones, no a las pasadas (Propuesta).

---

## 6. Modelo de dos niveles

### 6.1 Clasificación de campos (Decidido)

| Campo | Nivel | Ubicación en el brief | Cómo se resolvió |
|---|---|---|---|
| Título | Ejercicio | Ejercicio | Igual |
| Objetivo | Ejercicio | Ejercicio | Igual |
| Fechas y recurrencia | Ejercicio | Ejercicio | Igual |
| Hacker humano | Ejercicio | Ejercicio | Igual |
| Profundidad | Global (heredable) | Ejercicio | El ejercicio da el valor base y el asset puede pisarlo |
| Franjas horarias | Global (heredable) | Ejercicio | Ídem |
| Credenciales | Propio del asset | Asset | Igual |
| Rate limit | Propio del asset | Asset | Igual |
| VPN | Propio del asset | Asset | Igual |

Criterio usado: si la decisión depende del trabajo contratado es del ejercicio; si depende del sistema es del asset; si depende de ambos, es heredable.

**Dónde se configura cada campo al crear (Decidido).** El modal de creación pide los valores del ejercicio y dos globales: profundidad y franjas horarias. Credenciales, rate limit y VPN siguen siendo propios del asset y se completan después, por asset o en lote (9.5).

### 6.2 Reglas de herencia
- Los valores globales se comparten con los assets mediante vínculo vivo (Decidido).
- Un asset puede pisar un valor global y queda "modificado" (Decidido).
- Si el valor del asset coincide con el del ejercicio, no se marca como modificado (Decidido).
- El tag "Modificado" aparece al pisar un campo heredado. No muestra cuántos campos; el detalle por campo se ve dentro del asset (Decidido).
- Volver a heredar: acción "Volver a valor original" junto al campo pisado. Al pasar el mouse, muestra el valor original (Decidido).
- El campo pisado lleva un borde del mismo color que el tag (Decidido).
- La marca visual de los campos globales es un ícono de planeta (Lucide `globe`, color `global-ink`) antes del label de cada campo global, con tooltip al pasar el mouse o con foco que explica el alcance (Decidido). Reemplaza al badge "Global". Se usa en el paso 1 del modal, en la configuración del detalle y en el panel de un asset (tooltip "Valor global del ejercicio. Si lo cambiás acá, este asset queda como Modificado."); falta aplicarla en los paneles del ejercicio y del lote, y definir la marca de los campos solo del ejercicio (Pendiente).
- Los assets que no pisaron ningún global se marcan "Heredado" en la columna "Herencia"; los que pisaron alguno, "Modificado" (Decidido).

---

## 7. Estados

### 7.1 Estados del ejercicio (Decidido)
| Estado | Significado | Tooltip propuesto |
|---|---|---|
| Borrador | Faltan datos para que corra | Faltan datos para activarlo |
| Activo | Corre según sus fechas y recurrencia | Corre según sus fechas y recurrencia |
| Pausado | Corrida suspendida temporalmente por el usuario | Suspendido hasta que lo reanudes |

- Al pasar el mouse sobre el badge de estado aparece un tooltip que explica el estado (Decidido). Los textos son propuesta.
- Transiciones (Propuesta): un ejercicio está en Borrador mientras falten fechas o tenga assets pendientes, y pasa a Activo cuando queda completo, sin botón "Activar". Activo y Pausado se alternan con "Pausar" y "Reanudar".
- Programado, En curso y Finalizado son estados de cada ejecución, no del ejercicio (Propuesta).

### 7.2 Estados del asset
- **Listo** o **Pendiente** (faltan datos esenciales, por ejemplo credenciales). La interfaz lo muestra con claridad (Decidido).
- En la interfaz el asset dice "Pendiente" (paleta `warning-*`, tooltip "Faltan credenciales para poder testearlo") y nunca "Borrador", que queda reservado para el estado del ejercicio. Coincide con el badge "N pendientes" de la lista de ejercicios (Decidido).
- Herencia, aparte del estado: "Heredado" o "Modificado" (ver 6.2).

### 7.3 Corrida manual
- Nombre de la acción: "Corrida manual" en el encabezado del detalle (Decidido).
- Reglas (Propuesta): solo en estado Activo; es una ejecución extra que no mueve la programación; pide confirmación mostrando qué se ejecutará; sirve, por ejemplo, para volver a testear tras corregir hallazgos.

---

## 8. Credenciales y seguridad

- Los campos nombrados en el brief pueden quedar pendientes al crear (Decidido).
- Las credenciales pendientes dejan al asset como pendiente (Decidido).
- **Búnker de credenciales:** permite buscar credenciales ya usadas para reutilizarlas (Decidido). Se revisó la seguridad de guardarlas.
- Medidas propuestas a partir de esa revisión (Propuesta). En el análisis se resumen en dos líneas: valores enmascarados y acceso con registro de uso. El detalle completo queda como extensión:
  - valores enmascarados por defecto, también después de guardar;
  - copiar y revelar quedan registrados;
  - mínimo privilegio: no todos los usuarios ven todo el búnker;
  - buscar por nombre o etiqueta, nunca por valor;
  - mostrar dónde se usa cada credencial;
  - sugerir vencimiento ligado al fin del ejercicio;
  - elección explícita entre "Cargar credenciales" y "No requiere login", para que un asset sin login no quede pendiente para siempre (Decidido; construido en el panel del asset, ver 9.4). La opción "Las aporta otra persona" se descartó (ver sección 17).
- En el panel del asset, "Cargar credenciales" ofrece dos pestañas: "Del búnker" (desplegable "Credencial guardada", con la ayuda "Se buscan por nombre, nunca por valor.") y "Nuevas" (usuario y contraseña enmascarada, con botón para mostrarla, y la casilla "Guardar en el búnker para reutilizarlas", marcada por defecto). Debajo: "Se guardan cifradas y siempre enmascaradas. Revelarlas o copiarlas queda registrado." (Decidido).
- La configuración guardada de un asset no incluye credenciales (Propuesta).

---

## 9. Flujos y pantallas definidas

### 9.1 Lista de ejercicios (primera pantalla)
- Lista de filas. **Las filas no se despliegan**: para ver los assets hay que hacer clic y entrar al detalle (Decidido). En el prototipo la fila abre el detalle con clic o con Enter (Decidido).
- CTA claro "Crear ejercicio" (Decidido).
- Propuesta de contenido por fila: nombre y objetivo, estado con tooltip, frecuencia, última ejecución y composición de assets por tipo (íconos con conteo), con marcas de modificados y de assets pendientes ("N pendientes"). Una flecha al final indica que toda la fila abre el detalle. Arriba, buscador y filtro por estado con conteos.

### 9.2 Creación del ejercicio
- Se abre desde el botón "Crear ejercicio" en un **modal central** (overlay) de dos pasos. Es creación y no edición, y da más espacio para revisar assets (Decidido).
- El modal solo configura el punto de partida; después se edita por asset o en lote (Decidido).
- **Encabezado:** título "Crear ejercicio" y, debajo, una descripción corta de qué hacer en el paso actual. Abajo, la línea de pasos "1 Valores del ejercicio y globales > 2 Assets". Al pie, "Paso N de 2" y las acciones (Decidido).
- **Tamaño:** paso 1 de 600px de ancho que crece con el contenido hasta un alto máximo de 944px; paso 2 de 880px de ancho y alto fijo de 944px. En pantallas más bajas el tope es el alto de la ventana menos 32px. Si el contenido supera el tope, scrollea por dentro y el modal no crece (Decidido).

**Paso 1: valores del ejercicio y globales**
- Descripción: "Definí qué es el ejercicio, cuándo corre y los valores globales que van a heredar sus assets." (Decidido).
- Campos del ejercicio: título, objetivo, fechas y recurrencia, hacker humano (Decidido).
- Título obligatorio, marcado con un asterisco rojo junto al label. Si falta, "Siguiente" no avanza y muestra el error (Decidido).
- Objetivo: desplegable opcional, sin marca de "Recomendado". Opciones: antes de un lanzamiento, auditoría o cumplimiento, revisión periódica, cambio importante en el sistema, otro (Decidido). Al elegirlo, el sistema infiere un nivel de profundidad y lo explica en una franja: "Para este objetivo sugerimos una profundidad alta. La podés ajustar abajo, en Profundidad." (Decidido).
- Objetivo "Otro": aparece un área de texto para describirlo, obligatoria con asterisco; en ese caso el sistema no sugiere profundidad (Decidido).
- Fechas: opcionales. Desplegable "Elegir fechas / Definir después", con Inicio, Fin y Repetir ("Una vez" por defecto). Con "Definir después" se avisa que el ejercicio queda en borrador. El fin no puede ser anterior al inicio (Decidido).
- Hacker humano: switch redondo, apagado por defecto, con una línea que explica qué es (Decidido).
- **Bloque de valores globales**, debajo de una línea divisoria (Decidido):
  - Cada campo global lleva un ícono de planeta antes del label, con tooltip: "Valor global: se aplica a todos los assets que agregues. Después podés modificarlo desde cada asset." Reemplaza al badge "Global" que encabezaba el bloque.
  - **Profundidad** (baja, media, alta). Toma el valor sugerido por el objetivo y muestra "Sugerida por el sistema", con tooltip "Se infirió a partir del objetivo: …". Si el usuario la cambia, la nota desaparece. Sin sugerencia arranca en media.
  - **Franjas horarias:** "Cualquier horario" o "Elegir rango" con Desde y Hasta (09:00 a 18:00 por defecto). El fin tiene que ser distinto del inicio; se permite cruzar la medianoche.

**Paso 2: assets**
- Descripción: "Sumá los assets que vas a testear. Heredan los valores globales del paso anterior; después podés ajustarlos uno por uno." (Decidido).
- Dos formas de agregar assets, en pestañas con el mismo diseño que las de la lista de ejercicios. Cada pestaña muestra al lado del label cuántos assets válidos se sumaron desde ella (Decidido). Por defecto se abre "Elegir existentes" (Decidido).
  - **Elegir existentes.** Los assets ya existen en un ítem aparte del menú y conservan sus datos. Se eligen con un buscador tipo combobox: al hacer foco se despliegan los assets disponibles, la lista se filtra a medida que se escribe y se navega con flechas, Enter y Esc. Los ya sumados no aparecen; si se quitan de la tabla vuelven a estar disponibles (Decidido).
  - **Importar nuevos.** Se escribe, se pega una lista o se sube un archivo. Cargar un asset a mano es escribir una línea.
- El sistema infiere el tipo de asset y el usuario puede modificarlo (Decidido).
- Filas con error: se importan las válidas y se señalan las demás (Decidido).
- Los assets nuevos nacen con valores por defecto según su tipo y con credenciales pendientes, o sea como assets pendientes (en la tabla del modal la fila todavía dice "Nuevo · borrador"). Solo ocurre con assets nuevos (Decidido). Credenciales, rate limit y VPN no se piden en el modal: se completan después, por asset o en lote (Decidido).
- "Importar nuevos" (Decidido):
  - Estado inicial: zona punteada para soltar un archivo o hacer clic ("Elegí un archivo o soltalo acá", CSV o TXT), con enlaces "Escribir o pegar una lista" y "Descargar plantilla".
  - Área de texto con un asset por línea (dominios, URLs o enlaces de tiendas), más "Subir archivo". Muestra cuántas líneas detectó. El botón "Agregar a la lista" las suma.
  - Después de agregar, el área se compacta en una línea con "Agregar más".
- **Tabla "Assets del ejercicio"** (Decidido):
  - Las filas de las dos pestañas entran a la misma tabla. **La tabla cumple el rol de confirmación**: no hay pantalla ni ventana aparte.
  - Cada fila muestra el tipo (editable solo en los nuevos), un estado con tooltip y la acción de quitar. Estados: Existente, Nuevo · borrador, Ya existe (con la acción "Usar existente") o Error (formato no reconocido o repetido en la lista; se omite).
  - Una fila "Ya existe" no cuenta como asset del ejercicio hasta que se toca "Usar existente" (Decidido).
  - Una línea de resumen cuenta las filas por estado. Debajo, el aviso de cuántos assets nuevos quedarán en borrador hasta cargar sus credenciales.
  - La tabla crece hasta ocupar el espacio libre y después scrollea por dentro, con el encabezado fijo. Los tooltips de las filas se abren a la izquierda del badge para no cortarse.
  - Estado vacío: un bloque que llena el espacio disponible con ícono, "Todavía no hay assets" y "Buscá los que ya tenés cargados o importá nuevos desde un archivo o una lista."
- Se puede crear un ejercicio sin assets: queda en borrador (Decidido). También queda en borrador si no tiene fechas o si tiene assets nuevos; si no, nace Activo (ver 7.1).
- "Volver" regresa al paso 1 sin perder lo cargado; "Crear ejercicio" lo suma a la lista.
- Layout anterior descartado: la franja fija de valores globales al pie del paso 2 (ver sección 17).
- Duplicar un asset lo crea con los mismos valores del original; añadir empieza de cero; se puede guardar la configuración de valores propios para reutilizarla (Decidido).
- No hay verificación de propiedad del asset en este flujo (Decidido).
- Extensiones (no se diseñan por ahora): confirmación al cerrar con datos cargados (o guardar un borrador automático) y modal a pantalla completa en pantallas chicas.

### 9.3 Detalle del ejercicio
Construido en el prototipo HTML (`prototipo/detalle-ejercicio.html`). Todo lo que sigue está Decidido.
- Se abre haciendo clic en la fila del ejercicio, o con Enter sobre la fila.
- Contiene la configuración, los assets y el cambio de vista entre lista y diagrama.
- **Encabezado:** ruta "Ejercicios / nombre"; título y badge de estado con tooltip. Acciones según el estado:
  - Activo: "Editar ejercicio", "Pausar" y "Corrida manual".
  - Pausado: "Editar ejercicio" y "Reanudar".
  - Borrador: solo "Editar ejercicio".
  - No hay botón "Activar": el ejercicio pasa a Activo al completarse (E8).
- **Ejecuciones:** en Activo y Pausado, una línea "Última ejecución · Próxima · Historial de corridas". En Pausado, la próxima dice "En pausa". El contenido del historial sigue pendiente.
- **Aviso en Borrador:** "N assets pendientes: faltan credenciales." y, si corresponde, "Faltan las fechas.", seguido de "El ejercicio pasa a Activo cuando esté completo." El enlace "Ver pendientes" aplica el filtro de Estado "Pendiente" y lleva a la tabla.
- **Configuración:** tarjeta con una sola grilla de cinco elementos: Objetivo, Fechas y recurrencia, Hacker humano; abajo Profundidad y Franjas horarias, cada una con el ícono de planeta y el tooltip "Valor global: se aplica a todos los assets. Un asset que lo cambia queda como Modificado." Sin enlace "Editar" (se edita desde "Editar ejercicio") y sin línea divisoria entre las filas.
- **Assets:** encabezado con el total, cambio de vista Lista / Diagrama y botón "Agregar assets".
  - Buscador por nombre, tipo o VPN.
  - Botón "Filtrar" con ícono de filtro. Abre un menú con dos grupos, **Estado** (Listo, Pendiente) y **Herencia** (Heredado, Modificado), cada opción con su conteo. Dentro de un grupo las opciones se suman; entre grupos se combinan. Con filtros activos el botón se marca y muestra cuántos hay. Tiene "Limpiar filtros" y se cierra con Esc o al hacer clic afuera. Reemplaza a los filtros separados "Modificados" y "Borradores".
  - Selección con casillas y "seleccionar todos" sobre las filas visibles (con estado intermedio). Al seleccionar aparece una barra "N seleccionados · Quitar selección · Editar". La barra y las filas seleccionadas usan `global-bg`, como pide el README.
  - Columnas: casilla, Asset (con la VPN debajo del nombre), Tipo (ícono y nombre), Estado (Listo o Pendiente), Herencia (Heredado o Modificado) y, al final, un botón de lápiz para editar ese asset solo.
  - Al pie: "Mostrando N de M assets".
- Íconos de tipo: Web app `app-window`, API `braces`, App móvil `smartphone`, Dominio `link`, IP o red `network`. El planeta queda reservado para la marca de global.
- El lápiz de cada fila abre el panel lateral de edición de ese asset (ver 9.4). Al guardar, la fila se actualiza.
- Todavía no conectados en el prototipo: "Editar" de la barra de selección, "Editar ejercicio", "Agregar assets", "Corrida manual", "Historial de corridas" y la vista Diagrama.

### 9.4 Panel lateral de edición (un solo componente)
- Es siempre el mismo componente; cambia el contenido (Decidido):
  - **Ejercicio:** campos del ejercicio y campos globales juntos; los globales con un diferencial visual que indica que afectan a todos los assets (el ícono de planeta con tooltip, el mismo del modal y del detalle).
  - **Un asset:** campos propios y también los globales. Editar un global lo convierte en asset modificado.
  - **Varios assets (lote):** ver 9.5.
- **Resumen de impacto (panel del ejercicio):** al editar un campo global aparece, junto al campo, una línea que dice a cuántos assets no afecta (los que tienen valor propio). Solo dice eso. Cumple el rol de advertencia (Decidido). El panel de un asset no lo muestra.
- **Confirmación al guardar (panel del ejercicio):** menciona cuántos assets no se verán afectados y una lista corta de sus nombres (Decidido). Si son muchos, se acorta con "+N más" (Propuesta). Todavía no está construida; es distinta de la confirmación del panel del asset (abajo).

**Panel de un asset.** Construido en el prototipo HTML (`prototipo/detalle-ejercicio.html`). Todo lo que sigue está Decidido, salvo donde se indica.
- **Apertura y cierre:** se abre con el lápiz de la fila. Entra desde la derecha, ocupa todo el alto y mide 480px de ancho (en mobile, todo el ancho). Se cierra con la X, "Cancelar", Esc o clic en el fondo; el foco vuelve al lápiz de la fila. Es el mismo componente para todos los assets; si el asset está Pendiente, el foco cae en las opciones de credenciales.
- **Encabezado:** antetítulo "Editar asset", nombre del asset y tipo (ícono y nombre). Sin badges de estado ni de herencia en el encabezado.
- **Aviso de pendiente:** mientras falten credenciales, arriba del cuerpo aparece un aviso con la paleta de advertencia. El texto se adapta: "Faltan credenciales. Elegí cómo se resuelven para que el asset quede Listo.", "Elegí una credencial del búnker para que el asset quede Listo." o "Completá usuario y contraseña para que el asset quede Listo."
- **Sección "Propios del asset"** ("Solo aplican a este asset."):
  - Credenciales: dos tarjetas de opción, "Cargar credenciales" ("Elegí una del búnker o cargá nuevas.") y "No requiere login" ("Se testea sin iniciar sesión."). Con "Cargar", pestañas "Del búnker" y "Nuevas" con el mismo diseño de indicador deslizante de la lista de ejercicios y del modal de creación (detalle en la sección 8).
  - Rate limit: campo numérico con "solicitudes por segundo". El valor por defecto depende del tipo: Web app 10, API 20, App móvil 10, Dominio 5, IP o red 5 (valores Propuesta). Ayuda: "Sugerido para <tipo>: N. Bajalo si el sistema es sensible a la carga.", con subrayado punteado y tooltip "Recomendación del sistema" con hover o foco. Tiene que ser un entero mayor a 0; si no, "Ingresá un número mayor a 0."
  - VPN: desplegable con "Sin VPN" o una de las VPN existentes.
- **Sección "Valores globales"** ("Vienen del ejercicio. Si cambiás uno, este asset queda como Modificado."): Profundidad y Franjas horarias ("Cualquier horario" o "Elegir rango" con Desde y Hasta; error "Elegí un horario de fin distinto al de inicio."). Cada uno con el planeta y el tooltip "Valor global del ejercicio. Si lo cambiás acá, este asset queda como Modificado." Si el valor difiere del ejercicio, el control toma el borde violeta de modificado (`modified-border`, H8) y aparece debajo el enlace "Volver a valor original", con tooltip "Valor original: X" (H7). Volver al valor del ejercicio lo deja sin modificar (H5).
- **Pie:** solo "Cancelar" y "Guardar cambios". Se quitó la vista previa "Al guardar" con badges (ver sección 17).
- **Al guardar:** el asset queda Listo si las credenciales están resueltas (credencial del búnker elegida, usuario y contraseña nuevos, o "No requiere login"); si no, Pendiente. La herencia queda Modificado si algún global difiere del ejercicio; si no, Heredado. La fila se actualiza y un toast abajo confirma "Guardaste los cambios de <asset>.". Si el ejercicio estaba en Borrador, tiene fechas y ya no queda ningún asset Pendiente, pasa a Activo solo y el toast suma "El ejercicio pasó a Activo." (aplica E8).
- **Confirmación de cambio de global:** si algún valor global quedó distinto del que tenía el asset antes de editar, "Guardar cambios" abre un diálogo central sobre el panel, con el título "Modificaste un valor global". Por cada campo cambiado, un bloque con el planeta, el label, el valor anterior tachado, una flecha y el valor nuevo. Después, una frase: "<asset> deja de usar el valor del ejercicio y queda como Modificado. El resto de los assets no cambia." o, si volvió a todos los valores del ejercicio, "<asset> vuelve a usar los valores del ejercicio y queda como Heredado." Botones "Volver" (vuelve al panel sin perder lo editado; tiene el foco inicial) y "Confirmar y guardar". Si no cambió ningún global, guarda directo. No reemplaza a la confirmación del panel del ejercicio: esta habla de un solo asset y no lista assets no afectados.

### 9.5 Edición en lote
- Selección por casillas; aparece la opción de editar (Decidido).
- Si los assets seleccionados tienen valores distintos en un campo, se muestra "Múltiples valores" y el usuario elige un valor único que se aplica a todos (Decidido).
- Solo se aplican los campos que el usuario tocó (Propuesta).
- Descartado: el paso "Revisar cambios" antes de guardar. Alcanza con la confirmación al guardar.
- Los campos heredables ofrecen "Volver a valor original" (Propuesta de texto).

### 9.6 Vista diagrama y grupos
- Árbol horizontal en tres niveles: ejercicio, grupos y assets (Decidido el concepto; la disposición horizontal es Propuesta).
- **Sin assets plegados**: se muestran todos. Por defecto se agrupan por tipo de asset (Decidido).
- El usuario puede crear **grupos personalizados** para tener más control. **Los grupos solo organizan**: no llevan configuración, así que la herencia sigue siendo de dos niveles (Decidido).
- Los assets modificados se conectan con una línea distinta (punteada del color del tag) (Decidido).
- Clic en un nodo abre el panel lateral de edición del asset (Decidido).
- Propuestas: controles de zoom y desplazamiento para la escala, leyenda de las líneas, control "Agrupar por", botón "Nuevo grupo", un grupo como atajo para seleccionar todos sus assets en lote.
- La **relación entre assets** queda para más adelante, ahora ligada a los grupos (Pendiente).

---

## 10. Requisitos

| ID | Requisito | Estado |
|---|---|---|
| H1 | El ejercicio es el padre y los assets son los hijos | Decidido |
| H2 | Clasificación de campos según 6.1 | Decidido |
| H3 | Los valores globales se heredan con vínculo vivo | Decidido |
| H4 | Un asset puede pisar un valor global y queda modificado | Decidido |
| H5 | Valor igual al del ejercicio no cuenta como modificado | Decidido |
| H6 | El tag no muestra cantidad de campos; el detalle está en el asset | Decidido |
| H7 | "Volver a valor original" con el valor original en tooltip | Decidido |
| H8 | Campo pisado con borde del color del tag | Decidido |
| H9 | Exclusiones aditivas | Descartado (se quitó Exclusiones del alcance) |
| E1 | Estados del ejercicio: Borrador, Activo, Pausado | Decidido |
| E2 | Tooltip en el badge de estado | Decidido |
| E3 | El ejercicio se programa al crearse, sin paso de lanzar | Decidido |
| E4 | Mostrar la última ejecución en el detalle | Decidido |
| E5 | Opción de historial de corridas (contenido pendiente) | Decidido |
| E6 | Acción "Corrida manual" | Decidido |
| E7 | Reglas de la corrida manual | Propuesta |
| E8 | Borrador hasta estar completo; pasa a Activo sin botón | Decidido (el detalle no tiene botón "Activar") |
| E9 | Asset "Pendiente" cuando faltan datos esenciales, con claridad visual; "Borrador" solo para el ejercicio | Decidido |
| C1 | Crear desde el botón "Crear ejercicio" de la lista | Decidido |
| C2 | Modal central de dos pasos, con descripción del paso bajo el título; alto máximo 944px y paso 2 fijo en 944px | Decidido |
| C3 | Paso 1: título, objetivo, fechas y recurrencia, hacker humano y valores globales (profundidad y franjas horarias) | Decidido |
| C4 | Objetivo en desplegable opcional; infiere profundidad, lo explica y la marca como "Sugerida por el sistema" con tooltip; con "Otro" pide describirlo y no sugiere | Decidido |
| C5 | Fechas opcionales; sin fechas el ejercicio queda en borrador | Decidido |
| C6 | Título obligatorio con asterisco rojo, recurrencia "Una vez" por defecto, hacker humano apagado | Decidido |
| C7 | Paso 2 solo de assets: elegir existentes o importar nuevos, en pestañas con contador | Decidido |
| C8 | Importar: escribir, pegar o subir; tipo inferido y editable | Decidido |
| C9 | Filas con error se omiten y las válidas se importan | Decidido |
| C10 | Assets nuevos con defaults por tipo y credenciales pendientes | Decidido |
| C11 | Paso 2 en una columna con franja fija de globales e impacto visible | Descartado (los globales pasaron al paso 1) |
| C12 | Duplicar, añadir de cero y guardar configuración reutilizable | Decidido |
| C13 | Sin verificación de propiedad | Decidido |
| C14 | Paso "Confirmar assets" dentro del paso 2 | Decidido (la tabla hace de confirmación) |
| C15 | Confirmación al cerrar con datos y pantalla completa en pantallas chicas | Extensión (no se diseña por ahora) |
| C16 | Importar nuevos: texto de un asset por línea, subir archivo, descargar plantilla; detecta el tipo | Decidido |
| C17 | Las filas importadas entran a la misma tabla; la tabla hace de confirmación | Decidido |
| C18 | Estados de fila: Existente, Nuevo · borrador, Ya existe (Usar existente), Error (se omite) | Decidido |
| C19 | Resumen con conteos y aviso de cuántos assets nuevos quedan en borrador | Decidido |
| C20 | Estado vacío del paso 2 que llena el espacio, con zona para soltar archivo y enlaces para escribir o pegar | Decidido |
| C21 | Franjas horarias: cualquier horario o rango Desde/Hasta | Decidido |
| C22 | Ícono de planeta con tooltip antes del label como marca de los valores globales (reemplaza al badge "Global") | Decidido |
| C23 | Elegir existentes con combobox que filtra y oculta los ya sumados | Decidido |
| C24 | Pestaña por defecto: "Elegir existentes" | Decidido |
| C25 | Se puede crear un ejercicio sin assets; queda en borrador | Decidido |
| C26 | "Ya existe" cuenta como asset solo después de "Usar existente" | Decidido |
| C27 | Exclusiones fuera del modal: se configuran en el panel del ejercicio | Descartado (se quitó Exclusiones del alcance) |
| C28 | VPN, rate limit y credenciales no se piden en el modal; se cargan después por asset o en lote | Decidido |
| C29 | Tabla de assets con scroll interno y encabezado fijo | Decidido |
| L1 | Filas de la lista no desplegables | Decidido |
| L2 | Clic o Enter en la fila abre el detalle | Decidido |
| L3 | Columnas y composición por tipo | Propuesta |
| L4 | Buscador y filtro por estado | Propuesta |
| D1 | Detalle con configuración, assets y vista lista o diagrama | Decidido |
| D2 | Buscador por nombre, tipo o VPN | Decidido |
| D3 | Selección múltiple con "seleccionar todos" y barra "N seleccionados · Quitar selección · Editar" | Decidido |
| D4 | Aviso de assets pendientes y fechas faltantes en Borrador, con "Ver pendientes" | Decidido |
| D5 | Menú "Filtrar" con Estado (Listo, Pendiente) y Herencia (Heredado, Modificado), con conteos; reemplaza los filtros separados | Decidido |
| D6 | Columna "Herencia" con "Heredado" y "Modificado" | Decidido |
| D7 | Botón de lápiz al final de cada fila para editar un asset | Decidido |
| D8 | Acciones del encabezado según el estado, sin "Activar"; línea de última y próxima ejecución con historial | Decidido |
| D9 | Configuración en una grilla de cinco elementos con planeta en los globales, sin "Editar" ni línea divisoria | Decidido |
| D10 | Selección y filas seleccionadas en `global-bg` | Decidido |
| M1 | Un solo componente de panel lateral con contenido variable | Decidido |
| M2 | Panel del ejercicio con globales marcados | Decidido |
| M3 | Panel de asset con propios y globales editables | Decidido (construido en el prototipo) |
| M4 | Resumen de impacto: cuántos assets no se afectan | Decidido (solo en el panel del ejercicio) |
| M5 | Confirmación al guardar con cantidad y lista corta | Decidido (panel del ejercicio; todavía no construida) |
| M6 | Lote con "Múltiples valores" y valor único | Decidido |
| M7 | Lote: solo se aplican los campos tocados | Propuesta |
| M8 | Lote: paso "Revisar cambios" | Descartado (alcanza con la confirmación al guardar) |
| M9 | Panel de asset de 480px que entra desde la derecha; se cierra con X, "Cancelar", Esc o el fondo, y el foco vuelve al lápiz | Decidido |
| M10 | Encabezado del panel de asset con nombre y tipo, sin badges de estado ni herencia | Decidido |
| M11 | Aviso de pendiente con texto según lo que falta para quedar Listo | Decidido |
| M12 | Rate limit con default por tipo, ayuda con tooltip "Recomendación del sistema" y validación de entero mayor a 0 | Decidido (valores por tipo en Propuesta) |
| M13 | Pie con "Cancelar" y "Guardar cambios", sin vista previa "Al guardar" | Decidido |
| M14 | Al guardar: estado y herencia recalculados, toast de confirmación y paso a Activo automático si el ejercicio queda completo | Decidido |
| M15 | Diálogo "Modificaste un valor global" con valor anterior y nuevo, solo si cambió algún global; "Volver" conserva lo editado | Decidido |
| G1 | Diagrama con ejercicio, grupos y assets, sin plegar | Decidido |
| G2 | Agrupación por tipo por defecto | Decidido |
| G3 | Grupos personalizados que solo organizan | Decidido |
| G4 | Modificados con conexión distinta | Decidido |
| G5 | Clic en nodo abre el panel de edición | Decidido |
| G6 | Zoom y desplazamiento para escalar | Propuesta |
| G7 | Relación entre assets | Pendiente |
| K1 | Búnker de credenciales reutilizables | Decidido |
| K2 | Medidas de seguridad del búnker (dos líneas en el análisis) | Propuesta |
| K3 | Elección explícita de credenciales: "Cargar credenciales" o "No requiere login" | Decidido (se quitó "Las aporta otra persona") |
| K4 | Configuración guardada sin credenciales | Propuesta |
| K5 | Cargar credenciales en pestañas "Del búnker" y "Nuevas", con contraseña enmascarada y "Guardar en el búnker" marcado por defecto | Decidido |
| P1 | Defaults inteligentes: tipo inferido, defaults por tipo, objetivo a profundidad | Decidido |

---

## 11. Escala y perfiles

**De 2 a 40 assets**
- Importación masiva con confirmación previa.
- Edición en lote con "Múltiples valores".
- Buscador y menú "Filtrar" por estado (Listo, Pendiente) y herencia (Heredado, Modificado).
- Grupos por tipo y personalizados.
- Diagrama sin plegar, con zoom y desplazamiento.
- Defaults por tipo: con 40 assets nuevos no hay 40 formularios.

**Perfiles técnicos y no técnicos**
- Defaults del sistema por tipo de asset.
- Profundidad sugerida a partir del objetivo, con la razón explicada.
- Tooltips que explican estados y valores originales.
- Aclaración de alcance en los campos globales.
- Lo pendiente no bloquea: el asset queda pendiente, el ejercicio en borrador, y se completa después.

Pendiente: textos de ayuda por campo y una vista más simple para quien no es técnico.

---

## 12. Correspondencia con los criterios del brief

| Criterio | Cómo responde la solución | Falta |
|---|---|---|
| Flujo de creación | Modal de dos pasos con importación y confirmación | Pasar los bocetos a mockups finales |
| Pensamiento estratégico | Modelo de dos niveles, herencia visible, ejercicio y ejecuciones, benchmark | Redactar el análisis |
| Usabilidad y UX | Impacto previo, edición en lote, defaults, estados | Perfiles y textos de ayuda |
| Calidad visual y de UI | Bocetos de las pantallas principales y prototipo HTML de la lista, el modal de creación, el detalle y el panel de un asset | Mockups finales del resto de las pantallas y planeta de global en los paneles del ejercicio y del lote |

---

## 13. Candidatos para el "¡Sorprendenos!"
- Resumen de impacto en vivo al editar un valor global.
- Herencia visible y filtro de "assets modificados".
- Sugerencia de profundidad según el objetivo, explicada al usuario.
- Importación masiva con confirmación previa.
- Vista diagrama con grupos.
- Decisión pendiente: cuál de estos será el momento estrella del prototipo.

---

## 14. Estado de los bocetos hechos

| Boceto | Estado |
|---|---|
| Recorrido de creación con modal central | Vigente |
| Paso 1 (valores del ejercicio) con desplegables | Reemplazado por el prototipo: el paso 1 suma el bloque de valores globales |
| Paso 2 detallado con dos opciones (esquema de flujo) | Vigente, salvo los globales, que pasaron al paso 1 |
| Paso 2 (wireframe de la tabla y franja de globales) | Reemplazado por los dos bocetos siguientes |
| Paso 2 con "Importar nuevos": escribir, pegar o subir | Vigente |
| Paso 2 con "Importar nuevos": tabla de confirmación | Vigente, sin la franja de globales |
| Paso 2: estado vacío con zona para soltar archivo | Vigente, sin la franja de globales |
| Prototipo HTML del modal de creación (`prototipo/lista-ejercicios.html`) | Vigente: es la referencia del flujo de creación |
| Panel de edición del ejercicio y del asset, con confirmación | Vigente para el ejercicio. El del asset se reemplazó por el prototipo HTML (`prototipo/detalle-ejercicio.html`): sin badges en el encabezado, sin vista previa "Al guardar" y con diálogo de confirmación de cambio de global |
| Panel de edición en lote con "Múltiples valores" (primer boceto de dos paneles) | Vigente, salvo el texto del enlace: decía "Usar valor del ejercicio" y ahora es "Volver a valor original" |
| Detalle del ejercicio en estado Activo, con tooltip y historial | Reemplazado por el prototipo HTML (`prototipo/detalle-ejercicio.html`): sin "Activar", filtro en menú, columna "Herencia" y lápiz por fila |
| Vista diagrama con grupos por tipo | Vigente |
| Lista de ejercicios sin filas desplegables | Vigente |
| Estados del ejercicio y corrida manual (esquema) | Vigente, con la acción "Activar" ya reemplazada por la regla de pasar a Activo al completar |
| Flujo de edición con "Advertencia" y "Elegir qué hacer" | Obsoleto: se reemplazó por resumen de impacto y confirmación |
| Modelo de ejercicio y ejecuciones (esquema) | Vigente |

Nota de coherencia: en el boceto del detalle la última ejecución figuraba como 14 nov 2026, fecha futura. En el prototipo ya está alineado: última 14 ago 2026, próxima 14 nov 2026.

---

## 15. Pendientes

### 15.1 Set de pantallas (cerrado por ahora)

Pantallas y estados incluidos (Decidido):
1. Lista de ejercicios.
2. Modal de creación, paso 1: valores del ejercicio y valores globales.
3. Modal de creación, paso 2: estado vacío, ingreso de la lista de "Importar nuevos", elección de existentes y tabla de confirmación.
4. Detalle del ejercicio en estado Activo (con tooltip de estado y última ejecución) y sus variantes en Pausado y Borrador. Construido en el prototipo HTML.
5. Vista diagrama del ejercicio, con grupos por tipo.
6. Panel lateral de edición: ejercicio con globales marcados y resumen de impacto; asset con campo pisado y "Volver a valor original"; confirmación al guardar.
7. Panel lateral de edición en lote con "Múltiples valores".

Fuera de este set, se mencionan como extensiones y sin mockup: historial de corridas, diálogo de la corrida manual, selector del búnker de credenciales, lista de assets agrupada, resumen final antes de confirmar la creación, confirmación al cerrar el modal, versión responsive, estado vacío de la lista de ejercicios y estados de lectura del archivo.

Próximo paso: pasar los bocetos de baja fidelidad a mockups finales. La lista, el flujo de creación, el detalle y el panel de edición de un asset (con su confirmación de cambio de global) ya están en el prototipo HTML. Falta: panel lateral del ejercicio, edición en lote, vista diagrama, diálogo de corrida manual e historial de corridas; en el detalle esas acciones todavía no están conectadas. En los paneles que faltan, aplicar el planeta como diferencial de los campos globales y el texto del enlace del lote actualizado.

### 15.2 Decisiones abiertas
- Forma visual de los campos solo del ejercicio y cómo se muestra el planeta de global en los paneles del ejercicio y del lote. En el modal, en el detalle y en el panel de un asset ya se decidió el ícono de planeta con tooltip. La marca significa cosas distintas en cada panel ("afecta a todos" y "viene del ejercicio"); conviene que sean una misma familia.
- Estado de un ejercicio de una sola vez que ya corrió, con solo tres estados.
- Si la corrida manual respeta las franjas horarias y si está disponible en Pausado.
- Qué pasa al cambiar assets o valores globales mientras una ejecución está en curso.
- Cuántos nombres muestra la lista corta de la confirmación y adónde lleva "+N más".
- Comportamiento del filtro con assets ya marcados: hoy la selección se conserva al filtrar y "seleccionar todos" actúa solo sobre las filas visibles. El contador de seleccionados ya está resuelto en la barra de selección.
- Si "Volver a valor original" usa el mismo texto en la edición en lote.
- Accesibilidad del tooltip (teclado y pantallas táctiles).
- Si seleccionar un grupo selecciona todos sus assets en lote.
- Casos borde: un asset en más de un ejercicio, ejercicios recurrentes cuando cambian los assets, guardar un borrador y retomarlo.
- Relación entre assets.
- Si "Confirmar assets" sobra al importar un solo asset.
- Contenido del historial de corridas.
- Estados de lectura del archivo (leyendo, error con reintentar, listo), inspirados en la referencia de subida de recursos. Extensión: no se diseña por ahora.

---

## 16. Supuestos y límites
- Los assets ya existen en la plataforma, en un ítem aparte del menú, y también se pueden crear al crear un ejercicio.
- No se diseña la verificación de propiedad.
- La clasificación de campos sale de razonar el problema, no de conocer el funcionamiento interno de Strike.
- Se usan textos y datos placeholder.
- El diseño termina en crear, configurar y administrar el ejercicio. La experiencia de ejecución y de resultados queda fuera del alcance.
- Los conectores con la nube y el descubrimiento automático de assets se mencionan como extensiones futuras, sin diseñarlos.

---

## 17. Decisiones que cambiaron en el proceso (razonamiento)

| Antes | Ahora | Por qué |
|---|---|---|
| Creación en un panel lateral | Modal central | Distingue crear de editar y da espacio para revisar assets |
| Paso 1 con valores heredados y paso 2 con valores propios del asset | Paso 1 con valores del ejercicio; paso 2 con assets y globales | Los globales importan en relación con los assets que los heredan, y se puede mostrar el impacto |
| Advertencia separada al guardar | Resumen de impacto en el panel y confirmación breve | Evita decir dos veces lo mismo |
| "Usar valor del ejercicio" | "Volver a valor original" | Más claro, con el valor visible al pasar el mouse |
| Filas de la lista desplegables | Filas que abren el detalle, con composición por tipo | Evita empujar filas y centraliza la vista de assets |
| Botón "Activar" | "Corrida manual" y activación automática al completar | El encabezado muestra acciones de uso, no de transición |
| Tres formas de agregar (lista, a mano, existentes) | Dos: existentes e importar nuevos | Escribir a mano y pegar una lista son la misma idea |
| Diagrama de dos niveles con "N assets más" | Tres niveles con grupos, sin plegar | Control y orden para el usuario |
| Paso 2 con assets y globales, con franja fija de globales al pie | Globales en el paso 1, debajo de los valores del ejercicio; paso 2 solo de assets | Al probarlo en el prototipo, el paso 2 quedaba cargado. Separado, cada paso hace una sola cosa y lo que se configura queda junto. El costo es que los globales se definen antes de ver a qué assets se aplican; la marca de global y su tooltip lo compensan |
| Línea "Se aplica a N assets" junto a los globales | Tooltip de la marca de global (hoy, el planeta junto a cada campo) | Con los globales en el paso 1 todavía no hay assets que contar; el tooltip dice el alcance y que se pueden modificar desde cada asset |
| Exclusiones como valor global heredable (el brief las ubicaba en el asset) | Exclusiones fuera del alcance | Decisión del owner del producto |
| Franjas horarias con opciones fijas | "Cualquier horario" o rango Desde/Hasta | Las ventanas reales de testeo no encajan en dos o tres opciones |
| Sumar VPN y rate limit al modal, o un tercer paso para valores de los assets | Se mantienen fuera del modal, que sigue en dos pasos | Son propios del asset según el criterio de 6.1 (dependen del sistema) y casi nunca se comparten entre todos los assets. Se completan después: en lote (9.5) cuando se repiten, y las credenciales con el búnker (8). Los assets existentes ya los traen |
| Lista visible de assets existentes con casillas | Combobox que despliega y filtra al escribir | Ahorra espacio para la tabla y escala mejor con muchos assets cargados |
| Badge "Ya existe" en cian | Badge "Ya existe" en gris | El cian queda reservado para lo global |
| Badge "Global" que encabeza el bloque de globales | Ícono de planeta antes del label de cada campo global, con tooltip | La marca queda pegada a cada campo, así que se entiende igual en el modal, en el detalle y en los paneles, aunque los globales no estén agrupados. Es más liviana que un badge y libera el badge para estados |
| Ícono de globo para el tipo "Web app" | Ícono de ventana (`app-window`) | El planeta pasó a significar "global"; repetirlo en la tabla confundía |
| Filtros separados "Modificados" y "Borradores" | Un botón "Filtrar" con menú de Estado y Herencia | Agrupa los filtros por dimensión, permite combinarlos y deja lugar para sumar más sin llenar la barra |
| Assets "en borrador" | Assets "Pendiente" | "Borrador" queda solo para el ejercicio; así el estado del asset y el del ejercicio no se confunden, y coincide con el badge "N pendientes" de la lista |
| Solo el tag "Modificado" | Columna "Herencia" con "Heredado" y "Modificado" | Nombra también el caso normal, así la columna se lee completa y se puede filtrar por los dos valores |
| "Editar" dentro de la tarjeta de configuración | Solo "Editar ejercicio" en el encabezado | Evita dos accesos a la misma acción |
| Botón "Activar" en el detalle (boceto) | Sin botón; acciones según el estado | Aplica E8: el ejercicio pasa a Activo al completarse |
| Duda sobre el cian de la selección | Selección y filas seleccionadas en `global-bg` | Se probó junto al planeta y se decidió mantenerlo |
| Credenciales con tres opciones: "Cargar", "No requiere login" y "Las aporta otra persona" | Dos opciones: "Cargar credenciales" y "No requiere login" | "Las aporta otra persona" no resolvía nada: el asset seguía pendiente igual. Con dos opciones la elección es clara y las dos dejan el asset Listo |
| Encabezado del panel de asset con badges de estado y herencia | Solo nombre y tipo | El estado ya se ve en la fila y el aviso de pendiente dice qué falta; los badges repetían información y competían con el contenido |
| Pie del panel con una vista previa "Al guardar" con badges | Solo "Cancelar" y "Guardar cambios" | La vista previa sumaba ruido. El resultado se comunica al guardar, con la fila actualizada y el toast, y el cambio de global con el diálogo de confirmación |
| Una sola confirmación al guardar, la del panel del ejercicio | Además, un diálogo "Modificaste un valor global" en el panel del asset | Cambiar un global desde un asset lo deja Modificado; el diálogo muestra el antes y el después y aclara que el resto de los assets no cambia. Solo aparece si cambió un global, así que no frena las ediciones comunes |
| Credenciales cargadas en un solo formulario | Pestañas "Del búnker" y "Nuevas", con el mismo diseño de las pestañas de la lista y del modal | Prioriza reutilizar lo que ya está en el búnker y reutiliza un componente conocido |
| Rate limit sin referencia | Default por tipo con la ayuda "Sugerido para <tipo>" y tooltip "Recomendación del sistema" | Quien no es técnico tiene un punto de partida y sabe de dónde sale el número |

---

## 18. Benchmark y referencias

### 18.1 Qué tomamos de cada una

| Plataforma | Qué hace | Qué tomamos |
|---|---|---|
| Cobalt | Un pentest se lanza sobre un solo asset, que se define una vez y se reutiliza. Al copiar un pentest se puede heredar el scope. Modos de pentest por intención. Estados del pentest | Asset desacoplado del trabajo, defaults por intención, ciclo de estados. Es la limitación que Strike quiere superar |
| Horizon3 NodeZero | Scope con alta guiada o carga masiva por CSV o texto. Límites explícitos. Plantillas y ejecuciones recurrentes | Alta masiva, decisión que el sistema propone y el usuario confirma |
| HackerOne | Gestión de scope unificada | Scope como entidad reutilizable |
| Strobes | El scope funciona como resumen del pedido, revisado antes de enviar | Modelo para el resumen final |
| Bugcrowd | Targets dentro y fuera de scope, con reglas aparte | Separar qué se testea de las reglas |
| Burp Suite DAST | Herencia por carpetas, el más específico gana, configuraciones apiladas, vista del delta. Importación por CSV con plantilla. Carga masiva de APIs con valores comunes, revisión y errores | Herencia, "solo lo que cambia", valores comunes al importar, revisión previa |
| Intruder | Etiquetas, alta masiva, sincronización con la nube, detección de APIs y logins, programaciones recurrentes | Agrupar, inferir el tipo, ejecuciones programadas |
| Detectify | Alta manual, conectores, archivos de zona, API; propone candidatos para revisar; verifica propiedad | El sistema propone y el usuario decide |
| Qualys CSAM | Lo importado llega como "no gestionado" hasta activarse; reglas anti duplicado | Estado intermedio y detección de duplicados |

### 18.2 Patrones de UX
- **Edición en lote:** casilla por campo, solo se aplican los marcados; contador, contexto y vista previa.
- **Importación por archivo:** archivo, mapear, validar y enviar; errores por fila con sugerencia; errores contra advertencias; decidir si se bloquea o se importan las válidas.
- **Credenciales:** enmascaradas, copiar auditado igual que revelar, mínimo privilegio, vencimiento, cuentas dedicadas por rol y no de producción.

- **Subida de archivos (referencia aportada):** panel de recursos con dos acciones (subir o explorar la biblioteca), zona punteada para soltar o hacer clic, y una lista de archivos con progreso, error con reintento y confirmación. Se adapta al paso 2: "Importar nuevos" y "Elegir existentes".

### 18.3 Enlaces
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

---

## 19. Entregables y avance

| Entregable | Estado |
|---|---|
| Análisis y propuesta de solución (PDF) | Material reunido en este documento; falta redactar |
| Wireframes o mockups | Set de pantallas cerrado por ahora, con bocetos de baja fidelidad; falta pasar a mockups finales |
| Prototipo interactivo (opcional) | En curso: `prototipo/lista-ejercicios.html` (lista y modal de creación de dos pasos, cerrado) y `prototipo/detalle-ejercicio.html` (detalle en Activo, Pausado y Borrador, cerrado, con el panel de edición de un asset y su confirmación de cambio de global). Falta panel del ejercicio, lote, diagrama, corrida manual, historial y elegir el momento estrella |
