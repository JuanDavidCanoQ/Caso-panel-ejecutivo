# 06 · Anonimización

Las imágenes del tablero web son capturas reales del diseño, tomadas sobre una **copia del modelo semántico cargada con datos sintéticos**. El modelo de producción no se modificó.

## Cómo se hizo

1. **Datos generados desde cero.** Un script crea ventas, presupuestos, compras, inventarios, importaciones y dimensiones con un catálogo genérico. Las cifras no son datos reales escalados ni transformados, para no conservar la estructura del negocio.
2. **Copia del modelo.** Las tablas de negocio leen esos archivos sintéticos. Se quitaron las conexiones externas, los comentarios y las descripciones internas de las medidas. Los textos fijos dentro del DAX (familias, canales, tipos de cliente, marcas) se cambiaron por el catálogo genérico.
3. **Extracción y tablero.** Las cifras y narrativas del tablero salen de consultas DAX contra la copia, igual que en la versión interna. El generador del tablero también se limpió: logotipo, enlaces internos y notas de desarrollo.
4. **Versión de demostración.** El tablero muestra el periodo por defecto (enero a octubre de 2026). Los filtros, que en la versión interna usan cálculos precalculados por el modelo, no se incluyeron.

| Elemento | Tratamiento |
|---|---|
| Montos, volúmenes y porcentajes | Sintéticos, generados desde cero |
| Años, fechas y meses | Calendario real del proyecto (corte enero–octubre 2026) |
| Clientes y tipos de cliente | «Cliente 1…»; «Segmento A…D» y consumidor final |
| Marcas | Propias: «Propia 1…6»; terceros: «Aliada 01…» |
| Canales | «Canal A…H» |
| Líneas y agrupaciones de producto | «Línea A…E», «Grupo A1…E2» |
| Formatos y tamaños de pieza | «Formato 01…11» (sin medidas reales) agrupados en S, M, L y XL |
| Geografía | Ciudades y regiones de Colombia con distribución sintética |
| Logotipo | Monograma genérico |
| Esquema del modelo | Nombres de tablas y columnas genéricos (`hechos_…`, `dim_…`), solo columnas representativas y medidas de los ejemplos renombradas; los conteos son reales |
| Capturas con nombres del modelo | Panel de datos de Power BI Desktop y ventana «Ver información»: nombres de tablas, columnas, medidas y DAX desenfocados |
| Capturas de Power BI Desktop | Lienzo desenfocado (muestra cifras reales) |
| Tablero original («antes») | Desenfocado hasta ser ilegible |
| Conexiones | Sin servidores, rutas de archivo ni nombres de cubos |

Las cifras de construcción del proyecto (páginas, objetos visuales, tablas, relaciones, medidas y pruebas) son reales.

**Verificación:** búsqueda automática de los nombres reales (comerciales y del esquema del modelo) en el HTML del tablero y del caso, en los documentos y en el PDF, y reconocimiento de texto (OCR) sobre las imágenes publicadas.
