# 05 · Tablero web interactivo

Réplica del informe que se abre en cualquier navegador, sin licencia de Power BI.

## Cómo funciona

1. **Lee el informe.** Un generador en Python recorre el PBIR de cada página: visuales, medidas, columnas, filtros, formato y posición.
2. **Pide las cifras al modelo.** Con Power BI Desktop abierto, un servidor MCP de modelado evalúa todas las medidas de cada página en cada contexto «año × un valor de filtro»: 687 contextos, comprimidos por página.
3. **Pinta.** Un motor visual propio (HTML, CSS y JavaScript sin frameworks) dibuja tarjetas, tablas, matrices, líneas, barras, mapa de hexágonos y narrativas con el contexto activo. La página **no recalcula**: muestra lo que el modelo calculó.
4. **Deriva lo que no puede hacer.** Selección múltiple, meses sueltos o detalle por material abren una ventana que explica por qué y lleva al informe de Power BI.

## Funciones

- Filtros de año, meses y cinco segmentaciones; valores vacíos mostrados como texto.
- Menú «⋯» por bloque: tamaño completo, descargar imagen y **ver información** (periodo, filtros, descripción y DAX de cada medida).
- Tablas con encabezados, primera columna, subtotales y total fijos al desplazar.
- Tema claro y oscuro, versión para celular, zoom, lupa, captura de pantalla y comentarios.
- Asistente de preguntas sobre los datos que solo redacta: las cifras las calcula la página.

## Pruebas automáticas (Playwright)

| Prueba | Qué verifica |
|---|---|
| Cifras | Cada texto de la página = valor del modelo, en las 16 vistas y con filtros (resultado: 0 diferencias) |
| Errores | 0 errores de JavaScript en todas las vistas |
| Desbordes | Ningún bloque se desplaza entero; solo los cuerpos con tablas largas |
| Tablas fijas | Encabezados, nombres, subtotales y total quedan fijos al desplazar |
| Tema y celular | Oscuro y móvil sin errores ni textos ilegibles |
| Herramientas | Menú «⋯», tamaño completo, barra de herramientas en cinco resoluciones |

Ninguna prueba reemplaza mirar las capturas: cada cambio se revisó en claro y en oscuro antes de publicarse.

## Galería

Ver `imagenes/vistas/` (16 vistas), `imagenes/oscuro/` y `imagenes/funciones/`.
