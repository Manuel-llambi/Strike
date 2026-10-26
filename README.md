Sistema visual para los mockups de alta fidelidad del ejercicio multi-asset. Se construyó mirando strike.sh y la captura de la plataforma que publica la web; no es el design system oficial de Strike. Todo lo marcado como **propuesta** es decisión de diseño de este challenge.

## Fundamentos de contenido

- Escribí en español, en oración (solo la primera palabra con mayúscula). Las mayúsculas completas se reservan para los antetítulos de sección en mono; badges, botones y encabezados de tabla van en oración.
- El producto habla en frases cortas y técnicas, sin exclamaciones ni emojis. Copy real de la web: "Agenda una demo", "Prueba Strike gratis", "Iniciar sesión", "Visibilidad en tiempo real de cada activo".
- Los botones empiezan con verbo: "Crear ejercicio", "Importar assets", "Volver a valor original".
- Decí siempre el alcance de un campo global: en el tooltip de su planeta ("Valor global: se aplica a todos los assets…") o junto al campo al editarlo ("No afecta a 3 assets con valor propio").
- Un estado nunca se comunica solo con color: se muestra como badge con su palabra (Activo, Borrador, Pausado, Listo, Pendiente, Heredado, Modificado) y un tooltip que explica qué significa.
- "Borrador" es solo para el ejercicio. Un asset al que le faltan datos esenciales es "Pendiente".
- Campos obligatorios: asterisco rojo (`danger-border`) pegado al label, sin texto "Obligatorio". Los campos opcionales o recomendados no llevan marca.
- Placeholders: dominios y URLs ficticios (`tienda.ejemplo.com`, `api.tienda.ejemplo.com`).

## Fundamentos visuales

**Dos superficies, una marca.** La web de marketing es oscura (`surface-page` en tema `dark`: `#050a0f`). La plataforma que muestra la web es clara, con un riel lateral oscuro de íconos (`surface-rail`). Diseñá los mockups en tema `light`; usá `dark` para el login, la portada o una variante oscura.

**Color.** Cuatro colores de marca verificados: `brand-black`, `brand-white`, `brand-cyan` y `brand-purple`. El cian es la acción: relleno del botón primario con texto `on-brand-cyan`. Sobre fondo claro el cian no se usa como texto: usá `accent-text`.

**Significado de color propio de este challenge (propuesta).**
- Cian (`global-*`) = valor heredado del ejercicio, "afecta a todos". Su marca visual es el ícono de planeta en `global-ink` (ver Marca de global). Ningún badge usa cian. También usan `global-bg` la barra de selección y las filas seleccionadas de las tablas (decidido: se probó junto al planeta y se mantiene).
- Violeta (`modified-*`) = excepción: asset o campo que pisó un valor global. El tag y el borde del campo pisado usan el mismo `modified-border`.
- Ámbar (`warning-*`) = Borrador (ejercicio) o Pendiente (asset). Verde (`success-*`) = Activo o Listo. Rojo (`danger-*`) = error. Gris (`surface-sunken`) = Pausado.

**Tipografía.** Tres familias: `heading` (Aeonik Fono), `sans` (Aeonik) y `mono` (Aeonik Mono). Aeonik es una fuente comercial que no está en Google Fonts: los stacks caen a JetBrains Mono y DM Sans como sustitutas. En Figma Make o AI Studio usá las sustitutas. En la app, `body` para valores y `field-label` para badges, encabezados de tabla y enlaces de acción (nunca todo en mayúsculas) y `label-mono` solo para antetítulos de sección. Títulos de la web en `web-display` y `web-h2`.

**Forma.** Esquinas rectas: `radius-none` en todo. Única excepción de control: el switch (pista y perilla) usa `radius-full`. Bordes de 1px (`border` decorativo, `border-strong` en controles). Sin sombras salvo `shadow-overlay` en modal y panel lateral. El motivo gráfico es el mosaico de cuadrados de píxel; no se usa como marcador de estado.

**Badges.** Todo estado o conteo de estado se muestra como badge: solo palabra, sin cuadrado ni ícono. Texto en `field-label` (12px medium, en oración), padding de 2px vertical y `space-2` horizontal, borde de 1px y `radius-none`. Colores por estado:
- Activo / Listo: fondo `success-bg`, texto `success-ink`, borde `success-border`.
- Borrador / Pendiente / pendientes: fondo `warning-bg`, texto `warning-ink`, borde `warning-border`. Tooltip de Pendiente: "Faltan credenciales para poder testearlo".
- Pausado: fondo `surface-sunken`, texto `ink-muted`, borde `border-strong`.
- Modificado: fondo `modified-bg`, texto `modified-ink`, borde `modified-border`. Tooltip: "Cambió al menos un valor global del ejercicio".
- Heredado: fondo `surface-card`, texto `ink-muted`, borde `border-strong`. Es el caso normal, por eso va neutro. Tooltip: "Usa todos los valores globales del ejercicio".

Heredado y Modificado van en la columna "Herencia" de la tabla de assets del detalle; Listo y Pendiente, en "Estado".

**Marca de global.** Cada campo global lleva un ícono de planeta (Lucide `globe`, 16px, color `global-ink`) antes del label, con tooltip al pasar el mouse o con foco de teclado (`tabindex="0"`, `role="tooltip"`, más un texto oculto "Valor global." para lectores de pantalla). Reemplaza al badge "Global". El tooltip dice el alcance y se parte en líneas con un ancho de 280px:
- Modal de creación: "Valor global: se aplica a todos los assets que agregues. Después podés modificarlo desde cada asset."
- Detalle y panel del ejercicio: "Valor global: se aplica a todos los assets. Un asset que lo cambia queda como Modificado."
- Panel de un asset: "Valor global del ejercicio. Si lo cambiás acá, este asset queda como Modificado."

Como el planeta significa "global", no se usa para otra cosa: el tipo de asset "Web app" usa `app-window`. Íconos de tipo: Web app `app-window`, API `braces`, App móvil `smartphone`, Dominio `link`, IP o red `network`.

Estados de fila en la tabla de assets del modal de creación:
- Existente: fondo `surface-card`, texto `ink-muted`, borde `border-strong`.
- Nuevo · borrador: mismos colores que Borrador (`warning-*`).
- Ya existe: mismos colores que Pausado (`surface-sunken`, `ink-muted`, `border-strong`), para que el cian quede reservado a lo global.
- Error: fondo `danger-bg`, texto `danger-ink`, borde `danger-border`.

Dentro de una tabla, el tooltip se abre a la izquierda del badge para no cortarse, y el badge con hover o foco sube de capa para que su tooltip no quede debajo del badge vecino.

Un badge de conteo dice solo la cantidad y el estado ("2 pendientes", "1 pendiente") y va al lado del dato que acompaña ("5 assets [2 pendientes]"), en la misma línea. Todo badge tiene tooltip con el significado del estado, visible con hover y con foco de teclado (`tabindex="0"`, `role="tooltip"`). El tooltip aparece en `duration-hover`, con fondo `brand-black` y texto `brand-white` en 12px.

**Layout de la web.** Grilla visible: líneas de 1px de `border` dividen secciones, con cruces `+` en las intersecciones y esquinas en `[`. Usalo solo en pantallas de marca, no en la tabla.

**Espaciado.** Escala `space-1` a `space-16`. Fila de tabla de `space-10` (40px). Padding de tarjeta y panel `space-4`; modal `space-8`.

**Modal.** Encabezado con el título y, debajo, una descripción corta en `ink-muted` que dice qué hacer en el paso actual; cambia con cada paso. Alto máximo de 944px (o el alto de la ventana menos 32px, si es menor); lo que no entra scrollea por dentro y el modal no crece. Un paso de tabla puede fijar su alto en ese máximo para no saltar al cambiar de paso.

**Panel lateral.** Entra desde la derecha, ocupa todo el alto y mide 480px (en mobile, todo el ancho), con `shadow-overlay`. Encabezado con antetítulo, título y metadato (por ejemplo, el tipo de asset), sin badges. Cuerpo con secciones separadas por una línea de `border`, cada una con título y descripción corta en `ink-muted`. Pie con "Cancelar" y la acción primaria. Se cierra con la X, "Cancelar", Esc o clic en el fondo, y el foco vuelve al control que lo abrió.

**Diálogo de confirmación.** Modal central chico (440px) sobre el panel, para confirmar un cambio con consecuencias. Muestra el antes y el después: valor anterior tachado, flecha y valor nuevo. Una frase dice el efecto ("… queda como Modificado. El resto de los assets no cambia."). Botones "Volver" (secundario, con el foco inicial, conserva lo editado) y "Confirmar y guardar".

**Toast.** Confirmación breve al pie y centrada, con fondo `ink` y texto claro, que desaparece sola. Frase en pasado con el objeto: "Guardaste los cambios de tienda.ejemplo.com.". Si el guardado cambia otro estado, lo suma en la misma frase ("El ejercicio pasó a Activo.").

**Campo pisado.** Un campo global con un valor distinto al del ejercicio toma el borde `modified-border` (borde de 1px más 1px interior). Debajo aparece el enlace "Volver a valor original" en `modified-ink`, con tooltip "Valor original: X". Al volver al valor del ejercicio, el borde y el enlace desaparecen.

**Sugerencia del sistema.** Un valor sugerido se explica en el texto de ayuda ("Sugerido para Web app: 10. Bajalo si el sistema es sensible a la carga.") con subrayado punteado, cursor de ayuda y tooltip "Recomendación del sistema" con hover o foco.

**Pestañas.** Un solo estilo: labels en `ink-muted`, el activo en `ink`, con un indicador de 2px que se desliza bajo la pestaña activa. Se usa en la lista de ejercicios, en el paso 2 del modal y en las credenciales del panel de un asset ("Del búnker" / "Nuevas").

**Estados de interacción.** Foco: anillo sólido de 2px en `focus-ring` con 2px de separación. Hover de botón primario: oscurecer el cian un 8%. Hover de fila: `surface-sunken`. Fila seleccionada y barra de selección: `global-bg` (la barra con borde `global-border` y texto `global-ink`).

**Filtros.** Un solo botón "Filtrar" con ícono `list-filter` abre un menú con grupos (por ejemplo, Estado y Herencia), cada opción con casilla y conteo. Con filtros activos el botón pasa a fondo `ink` con el número de filtros. El menú tiene "Limpiar filtros" y se cierra con Esc o al hacer clic afuera.

**Movimiento.** Todo es suave y nada salta. Hover en 250ms con `ease-standard` (`duration-hover`); entrada del panel lateral y aparición del modal en 400ms con `ease-enter` (`duration-enter`); salida en 200ms (`duration-exit`). Con `prefers-reduced-motion` no hay transiciones.

## Íconos

Librería **Lucide** (`lucide` 1.61.0, instalada con `pnpm add lucide`), consumida desde `components/bundle.js` como `Strike.Icon` / `Strike.svg` / `Strike.mount`. Trazo de 1.5px, 16px por defecto, color heredado (`currentColor`). Sin emojis ni glifos de texto (✕, ▾) ni otra librería. Ver el componente Icon para el set y el uso.

## Logo

No se redibuja. Para los mockups usá el SVG oficial que publica strike.sh o, si no lo tenés, la palabra STRIKE en `heading` como marcador.

## Pendiente de verificar

- Rosa de las pestañas de strike.sh: se ve en la web pero no se midió su hex.
- Valores semánticos (verde, ámbar, rojo, violeta de excepción) y escala de la app: son propuesta; los verdes y rojos de la plataforma real se estimaron solo a ojo desde una captura.
- La pantalla de login no se pudo leer (bloquea lectura automática): pasá una captura si querés alinearla.
