
---

## 🟦 **1. Cálculos básicos**

### **1. Total de Ventas**

```DAX
Total Ventas = SUM(Ventas[Precio Total])
```

### **2. Cantidad Total Vendida**

```DAX
Total Cantidad = SUM(Ventas[Cantidad])
```

### **3. Precio Promedio por Producto**

```DAX
Precio Promedio = AVERAGE(Ventas[Precio Unitario])
```

### **4. Ventas Distintas por Ticket**

```DAX
Tickets Únicos = DISTINCTCOUNT(Ventas[Ticket ID])
```

---

## 🟦 **2. Medidas basadas en condiciones**

### **5. Ventas solo de un producto específico (ej. ID = 4)**

```DAX
Ventas Producto 4 = 
CALCULATE(
    SUM(Ventas[Precio Total]),
    Ventas[Producto ID] = 4
)
```

### **6. Ventas de un vendedor específico (ej. vendedor 10)**

```DAX
Ventas Vendedor 10 =
CALCULATE(
    SUM(Ventas[Precio Total]),
    Ventas[Vendedor ID] = 10
)
```

### **7. Ventas mayores a $5000**

```DAX
Ventas > 5000 =
CALCULATE(
    SUM(Ventas[Precio Total]),
    Ventas[Precio Total] > 5000
)
```

---

## 🟦 **3. Cálculos por Tiempo**

*(Asegúrate de tener una tabla calendario.)*

### **8. Ventas del Mes Actual**

```DAX
Ventas Mes Actual =
CALCULATE(
    [Total Ventas],
    MONTH(Ventas[Fecha Venta]) = MONTH(TODAY()),
    YEAR(Ventas[Fecha Venta]) = YEAR(TODAY())
)
```

### **9. Ventas del Año Actual**

```DAX
Ventas Año Actual =
CALCULATE(
    [Total Ventas],
    YEAR(Ventas[Fecha Venta]) = YEAR(TODAY())
)
```

### **10. Ventas del Mes Pasado**

```DAX
Ventas Mes Pasado =
CALCULATE(
    [Total Ventas],
    PREVIOUSMONTH('Calendario'[Fecha])
)
```

### **11. Diferencia Mes Actual vs Mes Pasado**

```DAX
Crecimiento Mensual =
[Ventas Mes Actual] - [Ventas Mes Pasado]
```

---

## 🟦 **4. Funciones Iterativas (X)**

### **12. Suma de ventas con SUMX**

```DAX
Total Ventas SUMX =
SUMX(Ventas, Ventas[Cantidad] * Ventas[Precio Unitario])
```

### **13. Precio Unitario promedio ponderado**

```DAX
PU Promedio Ponderado =
DIVIDE(
    SUMX(Ventas, Ventas[Cantidad] * Ventas[Precio Unitario]),
    SUM(Ventas[Cantidad])
)
```

---

## 🟦 **5. Filtrado, Contexto y CALCULATE**

### **14. Ventas acumuladas (Running Total)**

```DAX
Ventas Acumuladas =
CALCULATE(
    [Total Ventas],
    FILTER(
        ALL(Ventas[Fecha Venta]),
        Ventas[Fecha Venta] <= MAX(Ventas[Fecha Venta])
    )
)
```

### **15. Ventas de todos los productos menos uno**

```DAX
Ventas Sin Producto 4 =
CALCULATE(
    [Total Ventas],
    REMOVEFILTERS(Ventas[Producto ID]),
    Ventas[Producto ID] <> 4
)
```

---

## 🟦 **6. Inteligencia de Clientes y Productos**

### **16. Clientes Únicos**

```DAX
Clientes Únicos = DISTINCTCOUNT(Ventas[Cliente ID])
```

### **17. Ticket Promedio**

```DAX
Ticket Promedio =
DIVIDE([Total Ventas], [Tickets Únicos])
```

### **18. Cantidad de Productos Distintos por Ticket**

```DAX
Productos Distintos =
DISTINCTCOUNT(Ventas[Producto ID])
```

---

## 🟦 **7. Funciones Avanzadas**

### **19. Top 5 Productos por Ventas**

```DAX
Top 5 Productos =
CALCULATE(
    [Total Ventas],
    TOPN(5, Ventas, [Total Ventas], DESC)
)
```

### **20. Porcentaje de Contribución por Producto**

```DAX
% Contribución por Producto =
DIVIDE([Total Ventas], CALCULATE([Total Ventas], ALL(Ventas)))
```

---
