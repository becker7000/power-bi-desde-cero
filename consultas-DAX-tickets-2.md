
# Consultas DAX, segunda ronda:

---

# **1. Mostrar las primeras 50 filas de Tickets**

```DAX
// Muestra los primeros 50 registros de la tabla Tickets
EVALUATE
TOPN(
    50,            -- Número de filas a devolver
    Tickets,       -- Tabla origen
    Tickets[Ticket ID], ASC  -- Orden ascendente por Ticket ID
)
```

---

# **2. Filtrar ventas menores a 1000**

```DAX
// Filtra solo las ventas cuyo total sea menor a 1000
EVALUATE
FILTER(
    Tickets,
    Tickets[Precio Total] < 1000
)
```

---

# **3. Contar cuántos productos distintos se han vendido**

```DAX
// Devuelve el número de productos únicos vendidos
EVALUATE
ROW(
    "Productos únicos",
    DISTINCTCOUNT(Tickets[Producto ID])
)
```

---

# **4. Suma total de ventas por sucursal**

```DAX
// Agrupa por sucursal y suma el total vendido
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Sucursal ID],                 -- Grupo por sucursal
    "Total Ventas", SUM(Tickets[Precio Total])  -- Medida calculada
)
```

---

# **5. Promedio de cantidad por Ticket**

```DAX
// Calcula el promedio de la columna Cantidad
EVALUATE
ROW(
    "Promedio Cantidad",
    AVERAGE(Tickets[Cantidad])
)
```

---

# **6. Obtener los productos con ventas totales mayores a 2000**

```DAX
// Filtra productos donde la suma del precio total supera 2000
EVALUATE
FILTER(
    SUMMARIZE(
        Tickets,
        Tickets[Producto ID],
        "TotalVentas", SUM(Tickets[Precio Total])
    ),
    [TotalVentas] > 2000
)
```

---

---

# **8. Total de ventas, ordenado por fecha**

```DAX
// Suma ventas por fecha y ordena cronológicamente
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Fecha Venta],
    "Total Ventas", SUM(Tickets[Precio Total])
)
ORDER BY
    [Fecha Venta] ASC
```

---

# **9. Contar cuántos tickets emitió cada vendedor**

```DAX
// Devuelve el número de tickets emitidos por cada vendedor
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Vendedor ID],
    "Tickets emitidos", COUNTROWS(Tickets)
)
```

---

# **10. Top 5 vendedores por total vendido**

```DAX
// Obtener a los 5 vendedores con más ventas
EVALUATE
TOPN(
    5,
    SUMMARIZE(
        Tickets,
        Tickets[Vendedor ID],
        "TotalVentas", SUM(Tickets[Precio Total])
    ),
    [TotalVentas], DESC
)
```

---

# **11. Conteo de ventas por cliente filtrando > 3 compras**

```DAX
// Devuelve solo clientes con más de 3 compras
EVALUATE
FILTER(
    SUMMARIZE(
        Tickets,
        Tickets[Cliente ID],
        "Compras", COUNTROWS(Tickets)
    ),
    [Compras] > 3
)
```

---

# **12. Productos cuya cantidad promedio comprada sea mayor a 1**

```DAX
// Filtra productos cuyo promedio de cantidad vendida sea mayor a 1
EVALUATE
FILTER(
    SUMMARIZE(
        Tickets,
        Tickets[Producto ID],
        "PromedioCantidad", AVERAGE(Tickets[Cantidad])
    ),
    [PromedioCantidad] > 1
)
```

---

# **13. Ventas del producto más vendido**

```DAX
// Encuentra el producto con más ventas (cantidad) y devuelve su total
EVALUATE
TOPN(
    1,
    SUMMARIZE(
        Tickets,
        Tickets[Producto ID],
        "CantidadTotal", SUM(Tickets[Cantidad])
    ),
    [CantidadTotal], DESC
)
```

---

# **14. Ventas acumuladas por fecha**

```DAX
// Cálculo de ventas acumuladas
EVALUATE
ADDCOLUMNS(
    SUMMARIZE(
        Tickets,
        Tickets[Fecha Venta]
    ),
    "Ventas Acumuladas",
        CALCULATE(
            SUM(Tickets[Precio Total]),
            FILTER(
                Tickets,
                Tickets[Fecha Venta] <= EARLIER(Tickets[Fecha Venta])
            )
        )
)
```

---

# **15. Desviación estándar del precio unitario**

```DAX
// Devuelve la desviación estándar del precio unitario
EVALUATE
ROW(
    "STD Precio Unitario",
    STDEVX.S(
        Tickets,
        Tickets[Precio Unitario]
    )
)
```

---

# **16. Obtener la fecha de venta más reciente**

```DAX
// Devuelve la última fecha registrada
EVALUATE
ROW(
    "Última Fecha",
    MAX(Tickets[Fecha Venta])
)
```

---

# **17. Ventas por cliente y por producto (doble agrupación)**

```DAX
// Agrupa por cliente y producto
EVALUATE
SUMMARIZE(
    Tickets,
    Tickets[Cliente ID],
    Tickets[Producto ID],
    "TotalVentas", SUM(Tickets[Precio Total])
)
```

---

# **18. Ranking de vendedores por monto total**

```DAX
// Agrega un ranking de vendedores
EVALUATE
ADDCOLUMNS(
    SUMMARIZE(
        Tickets,
        Tickets[Vendedor ID],
        "TotalVentas", SUM(Tickets[Precio Total])
    ),
    "Ranking",
        RANKX(
            SUMMARIZE(
                Tickets,
                Tickets[Vendedor ID],
                "TotalVentas", SUM(Tickets[Precio Total])
            ),
            [TotalVentas],
            ,
            DESC
        )
)
```

---

---

