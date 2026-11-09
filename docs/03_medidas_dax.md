# 03 · Medidas DAX

> 633 medidas, 365 con descripción. Conteos calculados desde el TMDL. En los ejemplos, los nombres de tablas, columnas y medidas son genéricos (ver `02_modelo_semantico.md`); la lógica DAX es la del modelo.

## Por sección del informe

| Sección | Medidas |
|---|---|
| Atributos | 116 |
| Compras | 91 |
| Geografia | 78 |
| Base común (volumen, precio, presupuesto, descomposición) | 70 |
| Tendencia | 58 |
| Inicio | 48 |
| Clientes | 47 |
| Categorias | 45 |
| Canales | 31 |
| Auxiliares | 28 |
| Portafolio | 21 |

## Por tipo de resultado

| Tipo | Medidas |
|---|---|
| Texto (narrativa, título, etiqueta) | 307 |
| Numérica | 279 |
| Imagen SVG | 42 |
| Color condicional | 5 |

Casi la mitad devuelve **texto**: titulares, narrativas, subtítulos con periodo y etiquetas. Ningún texto con cifras se escribe a mano en el informe (regla R17).

## Funciones más usadas (ocurrencias)

| Función | Usos |
|---|---|
| `VAR` | 1218 |
| `FORMAT` | 526 |
| `CALCULATE` | 390 |
| `DIVIDE` | 237 |
| `REMOVEFILTERS` | 210 |
| `SELECTEDVALUE` | 123 |
| `SWITCH` | 93 |
| `TOPN` | 50 |
| `SUMX` | 48 |
| `KEEPFILTERS` | 41 |
| `SAMEPERIODLASTYEAR` | 39 |
| `ALLSELECTED` | 13 |

## Convenciones

- Nombre: `<Página o tema> <Qué> <Formato>`. Ejemplos: `KPI Venta Txt`, `Canales Narrativa L1 Txt`, `Barra Cumplimiento Canal`.
- `Txt`: texto ya formateado con `FORMAT(…, "es-CO")` para que el formato no dependa del equipo de quien abre el informe.
- `SVG` / `Barra`: devuelven una imagen `data:image/svg+xml` (barras de cumplimiento, cascadas) que se muestra dentro de tablas.
- `Sub Txt`: subtítulo del visual con el periodo. `Narrativa Ln Txt`: línea n del bloque narrativo.
- Carpetas de presentación por fase y página (`Fase 0\…`, `Fase 1 Canales\…`).
- Cada medida nueva se propone, se aprueba y se valida con una consulta DAX contra el modelo antes de usarla en un visual. La descripción (`///` en TMDL) explica el porqué.

## Ejemplos

### Efecto precio sin mezclar unidades

Descompone la variación de venta en efecto precio sumando unidad por unidad, solo en unidades válidas (R19).

```dax
Efecto Precio Total =
SUMX (
    FILTER ( ALL ( 'dim_unidad'[unidad], 'dim_unidad'[es_valida] ), 'dim_unidad'[es_valida] ),
    VAR _v   = CALCULATE ( SUM ( 'hechos_ventas'[cantidad_ref] ) )
    VAR _vpy = CALCULATE ( SUM ( 'hechos_ventas'[cantidad_ref] ),
                           SAMEPERIODLASTYEAR ( 'dim_calendario'[fecha] ) )
    VAR _s   = CALCULATE ( SUM ( 'hechos_ventas'[ingreso_neto] ) )
    VAR _spy = CALCULATE ( SUM ( 'hechos_ventas'[ingreso_neto] ),
                           SAMEPERIODLASTYEAR ( 'dim_calendario'[fecha] ) )
    RETURN _v * ( DIVIDE ( _s, _v ) - DIVIDE ( _spy, _vpy ) )
)
```

### Texto con cifra (R17)

La variación se escribe desde el modelo, con flecha y formato colombiano; vacía si no hay año anterior.

```dax
Var Venta Flecha Txt =
VAR _v = [Var % Venta]
RETURN
    IF (
        ISBLANK ( [Ingreso Neto AA] ),
        BLANK (),
        IF ( _v >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _v ), "0.0%", "es-CO" )
    )
```

### Barra de cumplimiento en SVG

La barra de una tabla es una imagen SVG generada por DAX: escala con tope de 130 %, marca de meta y color por resultado.

```dax
Barra Cumplimiento Canal =
VAR _cumpl = [Cumplimiento Ppto Canal]
VAR _p = MAX ( MIN ( _cumpl, 1.3 ), 0 )
VAR _w = INT ( DIVIDE ( _p, 1.3 ) * 75 )
VAR _c = IF ( _cumpl >= 1, "#12823C", "#E07B39" )
RETURN
    IF ( [Canal Con Ppto] = 0 || ISBLANK ( _cumpl ), BLANK (),
        "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='75' height='8'>"
            & "<rect width='75' height='8' rx='4' fill='#E1E0D9'/>"
            & "<rect width='" & FORMAT ( _w, "0", "en-US" ) & "' height='8' rx='4' fill='" & _c & "'/>"
            & "<rect x='58' width='2' height='8' fill='#918F89'/></svg>" )
```

