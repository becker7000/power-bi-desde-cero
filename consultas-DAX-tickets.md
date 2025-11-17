
**IMPORTANTE:** Todas empiezan con `EVALUATE` (porque así funcionan las *DAX queries*).

---

# 🟦 **🔵 NIVEL 1 — BÁSICAS (1–10)**

Consultas para explorar datos.

---

## **1. Ver las primeras 10 filas**

```DAX
EVALUATE
TOPN(10, Tickets)
```

---

## **2. Ver todas las filas ordenadas por Precio Total**

```DAX
EVALUATE
ORDERBY(Tickets, Tickets[Precio Total], DESC)
```

---

## **3. Seleccionar columnas específicas**

```DAX
EVALUATE
SELECTCOLUMNS(
    Tickets,
    "Ticket", Tickets[Ticket ID],
    "Producto", Tickets[Producto ID],
    "Precio Total", Tickets[Precio Total]
)
```

---

## **4. Filtrar ventas con precio total mayor a 5000**

```DAX
EVALUATE
FILTER(Tickets, Tickets[Precio Total] > 5000)
```

---

## **5. Clientes únicos**

```DAX
EVALUATE
ROW(
    "Clientes Unicos",
    DISTINCTCOUNT(Tickets[Cliente ID])
)
```

---

## **6. Productos únicos**

```DAX
EVALUATE
ROW(
    "Productos Unicos",
    DISTINCTCOUNT(Tickets[Producto ID])
)
```

---

## **7. Total de ventas**

```DAX
EVALUATE
ROW(
    "Total Ventas",
    SUM(Tickets[Precio Total])
)
```

---

## **8. Cantidad total vendida**

```DAX
EVALUATE
ROW(
    "Cantidad Total",
    SUM(Tickets[Cantidad])
)
```

---

## **9. Precio unitario promedio**

```DAX
EVALUATE
ROW(
    "Precio Promedio",
    AVERAGE(Tickets[Precio Unitario])
)
```

---

## **10. Mostrar filas sin duplicados**

```DAX
EVALUATE
DISTINCT(Tickets)
```

---

# 🟦 **🟢 NIVEL 2 — AGRUPACIONES Y SUMMARIZE (11–20)**

Empiezas a analizar de verdad.

---

## **11. Ventas por producto**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Producto ID],
    "Total Ventas", SUM(Tickets[Precio Total])
)
```

---

## **12. Ventas por vendedor**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Vendedor ID],
    "Total Ventas", SUM(Tickets[Precio Total])
)
)
```

---

## **13. Ventas por cliente**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Cliente ID],
    "Total Ventas", SUM(Tickets[Precio Total])
)
```

---

## **14. Cantidad vendida por producto**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Producto ID],
    "Cantidad Total", SUM(Tickets[Cantidad])
)
```

---

## **15. Precio promedio por producto**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Producto ID],
    "Precio Promedio", AVERAGE(Tickets[Precio Unitario])
)
```

---

## **16. Ventas por día**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Fecha Venta],
    "Ventas Día", SUM(Tickets[Precio Total])
)
```

---

## **17. Ventas por sucursal**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Sucursal ID],
    "Ventas Totales", SUM(Tickets[Precio Total])
)
```

---

## **18. Contar tickets por cliente**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Cliente ID],
    "Tickets Cliente", COUNTROWS(Tickets)
)
```

---

## **19. Ventas por combinación Producto–Vendedor**

```DAX
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Producto ID],
    Tickets[Vendedor ID],
    "Total Ventas", SUM(Tickets[Precio Total])
)
```

---

## **20. Promedio de ventas por ticket**

```DAX
EVALUATE
ROW(
    "Ticket Promedio",
    DIVIDE(SUM(Tickets[Precio Total]), DISTINCTCOUNT(Tickets[Ticket ID]))
)
```

---

# 🟦 **🔴 NIVEL 3 — INTERMEDIAS Y AVANZADAS (21–30)**

Cálculos más poderosos.

---

## **21. Top 10 productos más vendidos**

```DAX
EVALUATE
TOPN(
    10,
    SUMMARIZE(
        Tickets,
        Tickets[Producto ID],
        "Ventas", SUM(Tickets[Precio Total])
    ),
    [Ventas], DESC
)
```

---

## **22. Top 5 vendedores con más ventas**

```DAX
EVALUATE
TOPN(
    5,
    SUMMARIZE(
        Tickets,
        Tickets[Vendedor ID],
        "Ventas", SUM(Tickets[Precio Total])
    ),
    [Ventas], DESC
)
```

---

## **23. Ventas acumuladas por fecha (running total)**

```DAX
EVALUATE
VAR Tabla =
    ADDCOLUMNS(
        SUMMARIZE(Tickets, Tickets[Fecha Venta]),
        "Total Día", CALCULATE(SUM(Tickets[Precio Total])),
        "Acumulado",
            CALCULATE(
                SUM(Tickets[Precio Total]),
                FILTER(
                    ALL(Tickets[Fecha Venta]),
                    Tickets[Fecha Venta] <= EARLIER(Tickets[Fecha Venta])
                )
            )
    )
RETURN
Tabla
```

---

## **24. Ranking de productos por ventas**

```DAX
EVALUATE
ADDCOLUMNS(
    SUMMARIZE(
        Tickets,
        Tickets[Producto ID],
        "Ventas", SUM(Tickets[Precio Total])
    ),
    "Rank", RANKX(ALL(Tickets[Producto ID]), [Ventas], , DESC)
)
```

---

## **25. Ventas del producto 175**

```DAX
EVALUATE
CALCULATETABLE(
    Tickets,
    Tickets[Producto ID] = 175
)
```

---

## **26. Ventas mayores a $10,000 ordenadas**

```DAX
EVALUATE
ORDERBY(
    FILTER(Tickets, Tickets[Precio Total] > 10000),
    Tickets[Precio Total],
    DESC
)
```

---

## **27. Precio total calculado manualmente (SUMX)**

```DAX
EVALUATE
ROW(
    "Total Ventas Calc",
    SUMX(Tickets, Tickets[Precio Unitario] * Tickets[Cantidad])
)
```

---

## **28. Agregar columnas adicionales a Tickets**

```DAX
EVALUATE
ADDCOLUMNS(
    Tickets,
    "Precio x Cantidad", Tickets[Precio Unitario] * Tickets[Cantidad]
)
```

---

## **29. Promedio ponderado del precio unitario**

```DAX
EVALUATE
ROW(
    "PU Ponderado",
    DIVIDE(
        SUMX(Tickets, Tickets[Precio Unitario] * Tickets[Cantidad]),
        SUM(Tickets[Cantidad])
    )
)
```

---

## **30. Ventas por cliente + ranking del cliente**

```DAX
EVALUATE
ADDCOLUMNS(
    SUMMARIZE(
        Tickets,
        Tickets[Cliente ID],
        "Ventas", SUM(Tickets[Precio Total])
    ),
    "Ranking", RANKX(ALL(Tickets[Cliente ID]), [Ventas], , DESC)
)
```

---

