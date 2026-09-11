# Observatorio de Actividad Turística de Santa Rosa de Cabal

**[Abrir el visor](05_entregas/visor_observatorio.html)**. Funciona sin conexión. La medición principal y las bases de entrega son exclusivamente de Santa Rosa de Cabal.

## Entregas vigentes

- `visor_observatorio.html`: consulta de series, temas, búsquedas, fuentes y catálogo.
- `base_indicadores_mensual.csv`: resultados numéricos mensuales con unidad, denominador, corte y naturaleza de la medición.
- `base_cualitativa_mensual.csv`: temas de prensa y consultas relacionadas, con nombres completos y evidencia.

Los controles de cobertura y la matriz de 90 indicadores están en `01_documentacion/control` y son consultables desde el visor. No se entrega ni se regenera `resenas_entre_cortes.csv`.

## Estado y definiciones vigentes

Revisión del 7 de septiembre de 2026: 564 filas numéricas, 22 indicadores numéricos y dos cualitativos. Las fechas de cobertura se consultan en el visor y la matriz; una misma fecha de reconstrucción no implica actualización simultánea de todas las fuentes.

El panel documentado para `ind_comp_19`, `ind_comp_20`, `ind_comp_21` e `ind_comp_22` conserva **Salento, Filandia, Jardín y Villa de Leyva**. Santa Rosa es la referencia. Manizales, Pereira y Armenia no sustituyen esos comparadores. Los insumos heredados con esos municipios permanecen internos, fuera de las entregas municipales.

`ind_comp_20` presenta una lectura exploratoria de los comparadores observados, siguiendo la regla de publicación de la base final documentada. La ficha original imponía un panel completo y umbrales más altos: esa diferencia se hace explícita aquí. El visor muestra los cinco destinos, los valores y el número de reseñas de cada uno, y advierte sobre muestras pequeñas y panel parcial. No se reemplazan comparadores sin información ni se imputa su balance.

`rs_6`: los valores originales **Aumento/Breakout** se conservan como categoría de crecimiento extraordinario, superior a 5.000 %. `valor` queda vacío porque Google no informa un crecimiento exacto. Se preservan resultado, valor original, período de consulta y fecha de captura. [Definición de Google](https://support.google.com/trends/answer/4355000?hl=es).

`rs_11`: temas de prensa por un diccionario visible, sin rankings de nombres personales. Una noticia puede tener varios temas. Los porcentajes usan todas las noticias del mes como denominador y la cobertura temática indica qué parte reconoce el diccionario. No interpretar los meses sin coincidencias temáticas como meses sin noticias.

AirDNA sigue pendiente de conciliar su período de origen. El catálogo conserva también los indicadores institucionales sin datos incorporados. No se convierten en ceros ni se ocultan.

## Dónde está cada cosa

| Carpeta | Contenido y uso |
|---|---|
| `01_documentacion` | Fichas de fuente, catálogo, inventario de rutinas y controles internos. No son entregables para compartir. |
| `02_datos` | Originales por fuente, capturas fechadas y tablas de trabajo. `indicadores` conserva la numeración que conecta cada fuente con su rutina. |
| `03_rutinas` | Programas propios de este proyecto. `Actualizar.ps1` es la entrada. `_dependencias` contiene las bibliotecas necesarias; no se ordena manualmente. |
| `04_visor` | Una única plantilla, estilos e interacciones. Cambiar aquí el diseño y reconstruir; no editar el HTML final. |
| `05_entregas` | Resultados vigentes para abrir, descargar o compartir. No crear variantes `final_v2`. |

Este README reúne la guía humana y el protocolo operativo. Las fichas técnicas se conservan como documentación de las fuentes, no como instrucciones alternativas de actualización. Los README dentro de bibliotecas o modelos pertenecen a esos componentes y conservan información de uso/licencia.

El proyecto es autónomo: no lee ni escribe el otro proyecto ni utiliza la antigua carpeta `oat/data`. Las copias iniciales de los insumos evolucionan por separado. No crear enlaces entre proyectos.

## Cómo ejecutar

Desde esta carpeta, con PowerShell:

```powershell
.\03_rutinas\Actualizar.ps1 -Modo Reconstruir
.\03_rutinas\Actualizar.ps1 -Modo Capturar -Desde AAAA-MM-DD -Hasta AAAA-MM-DD
```

`Reconstruir` usa únicamente los insumos locales. `Capturar` consulta las fuentes automáticas indicadas en el protocolo y después reconstruye. El lanzador busca primero `03_rutinas/.venv`, después el Python disponible en Codex y finalmente Python del sistema. Los requisitos particulares de cada fuente se conservan con su configuración; no reutilizar un entorno alojado en el otro proyecto.

Cada ejecución registra resultados por rutina y huellas de las entregas en `01_documentacion/actualizaciones`. Revisar el estado de cada fuente; una rutina opcional fallida puede conservar la última información válida. No afirmar que una fuente se actualizó porque el visor se reconstruyó.

## Protocolo mensual para un agente

Versión 3 · 7 de septiembre de 2026. La medición principal es Santa Rosa de Cabal; los comparadores conservan los paneles definidos por indicador.

## Principio de medición

Actualizar mensualmente no permite inventar frecuencia mensual. Usar la fecha real de la fuente: publicaciones y reseñas se agregan por mes; inventarios y precios conservan la última fotografía del mes; variaciones entre cortes conservan ambos extremos. No dividir un valor trimestral en tres ni rellenar meses sin dato. Los paneles y sus denominadores deben permanecer visibles.

## Secuencia de cada mes

1. Leer la sección de estado de este README, `01_documentacion/control/matriz_indicadores_90.csv` y la última auditoría. Elegir el mes terminado y el día real de captura. Examinar qué indicadores tienen más de un corte y cuáles dependen de una solicitud institucional.
2. Verificar las fuentes en la matriz y las rutinas en `01_documentacion/rutinas.csv`. Para cada familia, usar su ficha en `01_documentacion/fichas/` y la configuración de `02_datos/indicadores`. No usar rutas antiguas de los anexos.
3. Capturar los insumos disponibles según la tabla de abajo. Conservar originales con identificador de corrida y fecha; no sobrescribir un raw de otro corte. Toda fuente sin respuesta debe quedar pendiente, no en cero.
4. Para actualizar solo Google News, ejecutar desde la raíz `03_rutinas/Actualizar.ps1 -Modo Capturar -Desde AAAA-MM-DD -Hasta AAAA-MM-DD`. Esta orden descarga únicamente Santa Rosa de Cabal, mediante `config.yml` de rs_9, transforma sin reducir el histórico y después ensambla. **No actualiza por sí sola Google Trends, reseñas, RNT, Booking, Viator ni AirDNA.**
5. Tras actualizar otras fuentes con sus rutinas, ejecutar `03_rutinas/Actualizar.ps1 -Modo Reconstruir`. Genera las bases mensuales, la matriz de los 90 indicadores, auditoría y visor. Los registros se guardan en `01_documentacion/actualizaciones`.
6. Conciliar filas, claves, fechas, paneles y denominadores con la auditoría previa. Revisar los puntos nuevos del visor, al menos un indicador de flujo, uno de fotografía y uno cualitativo. Documentar cambios de consulta o metodología.
7. Actualizar la sección de estado de este README con el último mes por fuente, parciales, accesos pendientes y próxima acción. No llamar “corte mensual completo” a un conjunto con fuentes sin actualizar.

## Familias de insumo y orden

| Insumo | Rutina/configuración propia | Actualización y límite |
|---|---|---|
| Interés y términos Google Trends | 01 rs_5, 02 rs_6, 04 ind_comp_22 | Descarga asistida según configuración y referencia guardada. Conservar ventana, geografía, término y fecha. Ejecutar `transform_*.py --config RUTA`. No mezclar escalas de distintas extracciones dentro del mismo mes. |
| Prensa Google News | 03 rs_9 | `download_rss_rs_9.py --config RUTA --desde AAAA-MM-DD --hasta AAAA-MM-DD`, luego `transform_rs_9.py --config RUTA`. El punto de entrada ya hace esta secuencia para la configuración de Santa Rosa de Cabal. |
| Términos y sentimiento de prensa | 05 rs_11, 06 rs_10, 07 ind_comp_21 | Primero actualizar rs_9 y el RSS de comparadores de 07; después ejecutar sus `build_*.py` con configuración local. Verificar modelo y requisitos específicos antes de recalcular sentimiento. Un rs_9 nuevo no significa que rs_10 también sea nuevo. |
| Arriendos turísticos/locales | 08 ilt_18 | `download_fuentes_ilt_18.py` y `build_ilt_18.py`; revisar cobertura por uso y portal. Usa captura propia, nunca el Excel de Pulso. |
| Lugares y reseñas Google/otras fuentes | 09 ot_6 y 10 tv_8 | Verificar credenciales, límites y evidencia definidos en la ficha. No lanzar servicios de pago sin acceso y presupuesto ya autorizados. No confundir fecha de captura con fecha de cada reseña. |
| Panel de reseñas Tripadvisor | 11 rs_3 | Incorporar reseñas reales al panel conservando identificadores; ejecutar build. Los derivados 12 tv_22, 13 rs_7, 14 ind_comp_20, 19 rs_4 y 22 rs_8 dependen del panel y de sus clasificaciones. |
| Experiencias y tours | 15 ot_3, 16 ind_comp_19, 17 ot_8, 18 ot_7 | Captura del catálogo fuente según ficha y build por familia. Conservar universo de destinos y fecha real del inventario. |
| RNT | 20 ot_4 | Obtener nuevo archivo oficial, registrar vigencia y estado; ejecutar build. El inventario no es un flujo mensual de aperturas. |
| Booking | 21 ot_9 | Repetir canasta y condiciones de búsqueda; guardar tarifas visibles y fecha. Cambiar fechas de estancia o ocupación cambia la comparabilidad. |
| Seguridad y movilidad | 23 ilt_5, 24 ilt_7 | Ejecutar después de prensa y temas de reseñas; el mensual usa documentos y denominadores observados. |
| AirDNA | 25 ot_5, 26 ot_10 | Registrar mensualmente la fotografía pública de ventana móvil de 12 meses. No empalmarla con el raw heredado de junio mientras su base temporal no esté verificada. |
| Fuentes institucionales restantes | Matriz de 90 indicadores | Solicitud o integración pendiente según fuente. No crear una rutina ficticia ni presentar datos inexistentes. |

Cada script admite su configuración local o describe sus argumentos en la cabecera. `01_documentacion/rutinas.csv` entrega las rutas exactas. Usar `--help` antes de una captura especializada si se desconoce el contrato. La reconstrucción trimestral de `99_base_final` se conserva solo para conciliación histórica y sus 33 pruebas; no es la entrega vigente.

## Definiciones mensuales vigentes

- rs_5 y panel de búsquedas: promedio de semanas asignadas por fecha inicial, usando una extracción coherente por mes; no equivale a búsquedas absolutas.
- rs_9: documentos únicos por municipio y mes de publicación. rs_10: balance porcentual de textos positivos menos negativos sobre los textos clasificados del mes.
- rs_11 v3: hasta cinco temas por frecuencia documental mensual, con diccionario en `03_rutinas/temas_prensa.json`, participación, cobertura y enlaces de evidencia; excluye atribuciones al medio y no utiliza nombres personales aislados. rs_6: fotografía de términos en aumento capturada en el mes, no flujo exclusivo de ese mes.
- Inventarios y precios: última captura efectivamente observada en el mes.
- Reseñas: respetar meses y panel; rs_7 es acumulado desde el inicio del panel, no una proporción exclusiva del mes.
- tv_8: variación entre cortes. Solo intervalos que comienzan y terminan en el mismo mes aparecen en la base mensual, identificados como intervalos parciales. Los demás permanecen en el insumo interno de tv_8; no se genera una entrega adicional de intervalos.
- Comparadores: usar únicamente los observados, informar su denominador y no imputar destinos ausentes.

## Control y mantenimiento

No borrar raws, modelos, diccionarios, registros de ejecución ni pruebas activas. No sustituir vacíos por cero. No crear `final_v2`, `final_final` ni copias paralelas del visor: el vigente siempre está en `05_entregas`. No leer ni escribir la carpeta de Pulso. Si hay que ampliar una metodología o resolver un período ambiguo, registrarlo como cambio pendiente y no improvisarlo durante la actualización rutinaria. Los límites de gasto ya autorizados por el usuario se respetan; no repetir solicitudes de permiso para acciones cubiertas por esa autorización.


## Actualización programada en la aplicación

Conversación: **OAT · actualización mensual**. El día 5 de cada mes a las 9:00 a. m., hora de Colombia. Primera ejecución prevista: 5 de octubre de 2026. Cierra el mes anterior y recupera pendientes disponibles.

La programación está activa y retoma la misma conversación, siguiendo este README. Requiere el computador encendido, la aplicación abierta y acceso a los archivos y fuentes. Si una fuente falla, conserva su último corte e informa el pendiente; no garantiza datos que la fuente no publique.

Identificadores para localizar o modificar la programación sin duplicarla: automatización `oat-actualizaci-n-mensual`; conversación `01a07d19-2fb8-7383-8ef0-bd5657788b2f`. Los horarios se administran en la aplicación: editar este README por sí solo no cambia la programación.

## Estado de la actualización extraordinaria — 7 de septiembre de 2026

La conversación mensual está asignada a **GPT-5.6 Terra**. El corte cerró agosto y añadió septiembre solo como fotografía cualitativa de Trends hasta el 7 de septiembre; no se completa artificialmente ninguna serie semanal. La auditoría final fue correcta: 619 filas numéricas, 61 cualitativas y 24 indicadores con observaciones.

| Fuente o familia | Corte efectivo | Resultado |
|---|---|---|
| Google News de Santa Rosa | 7 de septiembre | 12.881 menciones; rs_9, rs_10, rs_11, seguridad y movilidad reconstruidos. |
| Sentimiento de Santa Rosa | 7 de septiembre | Clasificación completa con Robertuito, revisión `a2cc0f67ebd705c55191e25a05ba23d885fcc09b`, en el entorno propio `03_rutinas/.venv`. |
| Prensa de comparadores | 7 de septiembre | 161 consultas RSS y 2.067 menciones únicas para Santa Rosa, Salento, Filandia, Jardín y Villa de Leyva; `ind_comp_21` actualizado. |
| Arriendos locales | 7 de septiembre | Captura procesada con fuentes disponibles; Mercado Libre no entregó tarjetas públicas y quedó registrado como no disponible. |
| RNT y Booking | 7 de septiembre | Actualizados con el corte visible de la fuente. |
| Google Trends | 7 de septiembre | rs_5, rs_6 y el panel comparativo se recuperaron desde las tablas accesibles de la interfaz oficial. Agosto queda cerrado; la semana iniciada el 6 de septiembre se excluyó por parcial. |
| AirDNA | 7 de septiembre | Fotografía verificada: 281 alojamientos activos y ADR $40 para agosto, publicada el 5 de septiembre. Ambas métricas cubren una ventana móvil de 12 meses; el símbolo de moneda no se presenta como USD. |
| Reseñas y experiencias | Último corte conservado | Tripadvisor Terra, Viator y Google Places mantienen su último corte porque no había credenciales configuradas. |

Los archivos y manifiestos de esta ejecución están en `01_documentacion/actualizaciones`, y la matriz vigente conserva el último corte por indicador. En la siguiente corrida mensual, cerrar primero septiembre y volver a intentar únicamente las fuentes pendientes disponibles; no presentar este corte parcial como un mes completo.
