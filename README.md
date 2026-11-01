# OwnPace

Aplicación personal estática: hoy, entrenamientos, alimentación, menú por semana, progreso y compra.

## Regla del proyecto

Cero gasto. No habilitar planes de pago, servidores, cuentas, APIs facturables o sincronización remota sin un cambio explícito de esta condición. La versión actual no tiene backend, autenticación, IA ni conexión con Strava o Salud.

## Archivos

- `index.html`: contenido y lógica anterior conservados para compatibilidad de los datos existentes.
- `enhancements.js`: navegación, semanas fechadas, diario de comidas, medidas, histórico y despensa.
- `foods.js`: tabla genérica de alimentos y mapa de ingredientes del menú. No contiene datos verificados de una marca.
- `enhancements.css`: interfaz de las nuevas secciones.

## Persistencia y privacidad

Los datos permanecen en `localStorage` del mismo origen y navegador (`miRitmoV1`, `shopCheckV2`). Cambiar de semana no borra el histórico. Cambiar de dispositivo, borrar datos del navegador o abrir otro origen no conserva esos registros por sí solo. La exportación/restauración JSON incluye semanas, sesiones, pasos, peso, medidas, comidas, alimentos personalizados, despensa y extras.

La actualización migra el menú anterior a la semana de la primera apertura de esta versión. No se pueden reconstruir semanas anteriores que nunca se guardaron. Las semanas nuevas parten de las recetas base; se pueden copiar otras semanas explícitamente. La fecha actual la determina la hora local del dispositivo.

Los borradores de entrenamientos se guardan; las propuestas de alimentos NO se registran como consumidas hasta pulsar Guardar. Los datos nutricionales se guardan junto con cada comida, para que cambios posteriores de productos no recalculen retrospectivamente el diario.

Las calorías son aproximadas: ingredientes genéricos, cantidades corregibles y crudo/cocinado separados. La interpretación de frases es una regla local limitada, no IA. Nunca presenta los datos como una dieta renal validada ni calcula un déficit energético. Las fotos no se suben ni se analizan.

## Vista previa

Desde esta carpeta, `python -m http.server 8001 --bind 127.0.0.1` y abrir `http://localhost:8001` en el PC. GitHub Pages sirve los mismos archivos para el iPhone. Al publicar hay que subir el HTML y todos los JS/CSS nuevos juntos.

## Copias de seguridad

Guardar una copia antes de actualizar o borrar datos. La app recuerda hacerlo cada 30 días. Una importación validada conserva una copia local previa (`miRitmoBeforeImport`) y solicita confirmación antes de sustituir datos. Esto no sustituye una copia descargada fuera del navegador.

## Pendiente fuera del alcance actual

La integración directa de Strava usa OAuth y necesita una solución que proteja secretos. No se implementa un botón que finja estar conectado. Sincronización entre dispositivos y análisis de fotos requieren una decisión de arquitectura adicional; no están activos.

## Navegación y registros (revisión de usabilidad)

- `usability.js` y `usability.css`: rutas por fragmento de URL, Atrás/Adelante, retorno desde el logo, menú de cabecera en móvil (sin barra inferior), navegación lateral y contexto de desplazamiento.
- Una comida consumida se muestra en el día y momento correspondientes del menú. La planificación original sigue disponible como referencia y conserva la lista de compra planificada; no se sustituyen retrospectivamente los ingredientes de la compra por lo ya consumido.
- Las sesiones con al menos un ejercicio terminado se pueden guardar como parciales y cuentan como una visita. Sin ejercicios terminados se guarda un borrador, que no suma visitas. El borrador más reciente tiene prioridad al continuar, sin borrar el registro anterior hasta guardar una nueva sesión.
- Cada variante de máquina lleva identificador propio dentro del ejercicio. Cambiar de variante borra solo la carga/repeticiones de ese ejercicio del borrador, previa confirmación. Las gráficas comparan únicamente registros de la misma variante y ejercicios marcados como terminados.
- Referencias de interacción consultadas: diario por momentos de MyFitnessPal (https://support.myfitnesspal.com/hc/en-us/articles/360032274592-How-to-log-food-to-your-diary) e histórico/rutinas de Hevy (https://help.hevyapp.com/hc/en-us/articles/35361530647959-How-to-Log-a-Workout-in-the-Hevy-App-Step-by-Step-Guide). No se han usado sus recursos gráficos ni sus servicios.

## Simplificación y otros deportes (31/10/2026)

- Sin botón Atrás dentro de OwnPace y sin barra inferior. El logo vuelve a Hoy y el menú cambia de sección; el historial del navegador conserva ruta, semana, día y posición incluso al recargar.
- Menú diario más compacto. «He comido esto» registra expresamente la ración del plan una sola vez; «Deshacer registro» elimina únicamente esa entrada. «He comido otra cosa» abre el diario. Nada se registra automáticamente por navegar.
- `activities.js`/`activities.css`: registro local de deportes con fecha, tipo y minutos, edición, eliminación, resumen y copia de seguridad. Pádel no suma al objetivo de fuerza. Las visitas de fuerza son fechas únicas, evitando duplicarlas si se anotan tanto actividad de gimnasio como detalle de máquinas.
- Se conservan los paseos y bicis de la versión anterior, identificados como registros sin duración.
- Salud/Fitness/Apple Watch no están conectados. No hay acceso activo a HealthKit ni a Strava, ni importador de XML/ZIP implementado en esta versión. Una importación no debe presentarse como sincronización automática. No se han creado cuentas, servidores externos ni servicios de pago.
## Elegir otra comida de la dieta

«He comido otra cosa» abre un selector con los platos ya guardados y un buscador. Prioriza desayunos, platos o tentempiés según el momento elegido. La selección abre sus ingredientes para revisar cantidades antes de guardar; no sustituye el plan de la semana. También se puede introducir una comida que no figure en la dieta.

## Recetas y sugerencias

Los platos de base tienen instrucciones locales breves y tiempos orientativos, usando los ingredientes de la planificación. «Cambiar plato» muestra alternativas de la propia dieta y conserva el selector completo. Sustituir un plato modifica el menú y su compra, pero no el diario consumido. Las alternativas no se presentan como equivalencias nutricionales.

## Calorías por cantidad

El campo `kcal` de cada alimento se conserva normalizado **por 100 g/ml**, nunca como total de una ración. El total siempre se calcula como `cantidad consumida × kcal por 100 / 100`. Las referencias de etiqueta por otras cantidades se normalizan antes de guardarse; sus datos originales se conservan opcionalmente como `referenceGrams` y `referenceEnergy`.

La edición muestra el resultado de cada ingrediente y su fórmula al escribir, separado de los datos de etiqueta. El total de comida y diario se redondea a la unidad de kcal, no a decenas. Los valores genéricos siguen siendo orientativos y no son datos de una marca ni un objetivo clínico. Se conservan los valores de referencia junto con cada comida para que editar un producto no cambie registros anteriores.

## Vista del diario de comidas

El diario muestra tarjetas por momento del día. Registrar o editar abre una ventana con alimento, cantidad y energía de esa ración. Los datos de etiqueta, nombre y momento se mantienen plegados como opciones secundarias. Añadir un ingrediente abre un buscador en lugar de insertar colines por defecto.

Cerrar la ventana conserva el borrador en la memoria de la pestaña y no lo cuenta como consumido. Solo Guardar crea o actualiza el registro; recargar o cerrar el navegador no equivale a guardar un borrador de comida. Los registros guardados siguen usando el mismo almacenamiento local y formato de copia.


### Guardado local (noviembre 2026)

Se mantiene la clave existente `miRitmoV1`: no es necesario migrar los registros.
La validación conserva los registros válidos y archiva el original si detecta errores.
Los archivos originales se pueden descargar desde «Descargar datos de recuperación»;
son archivos de diagnóstico, no copias que se restauren directamente.
Cada escritura verifica el resultado y conserva el estado anterior. Si otra pestaña ha
modificado el historial, se bloquea la escritura antigua y se solicita recargar.
Un error de almacenamiento interrumpe el guardado y muestra un aviso.
Las semanas nuevas no heredan automáticamente los cambios de la anterior.
El almacenamiento está separado por origen y navegador: los puertos locales y GitHub
Pages no comparten datos. Borrar los datos del sitio puede eliminar el historial y sus
copias locales; exportar una copia sigue siendo necesario para conservarlo fuera del dispositivo.
