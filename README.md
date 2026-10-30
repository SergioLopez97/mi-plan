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
