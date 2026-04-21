Wear Leveling Driver – NXP S32K144 (AUTOSAR)
Problema
Las memorias flash tienen un número limitado de ciclos de escritura por sector. Sin una estrategia de distribución, las escrituras repetidas en los mismos sectores degradan la memoria prematuramente, reduciendo la vida útil del producto.

Solución
Driver de wear leveling desarrollado desde cero en C, integrado al stack AUTOSAR, que distribuye las operaciones de escritura uniformemente entre 8 sectores de memoria flash interna del NXP S32K144.

Resultado
El sistema extiende la vida útil de la memoria flash distribuyendo el desgaste de forma equitativa, mejorando la confiabilidad en aplicaciones embebidas de largo plazo.



El algoritmo distribuye las escrituras de forma uniforme entre 8 sectores de 2 KB en la región Data Flash, seleccionando siempre el sector con menor número de escrituras. Esto extiende la vida útil de la memoria Flash, que típicamente soporta ~100,000 ciclos de borrado por sector.

## Características

- Rotación automática de sectores basada en conteo de escrituras (write count)
- API sencilla: `WL_Init()`, `WL_WriteData()`, `WL_ReadData()`, `WL_Maintenance()`
- Metadata embebida en cada sector (ID, contador de escrituras, datos de usuario)
- Detección de sectores vírgenes (0xFF) para recuperación tras borrado
- Monitoreo de ciclos de vida con alerta al acercarse al límite de 100K ciclos
- Configuración de periféricos mediante EB Tresos (archivos .xdm)
- Toolchain: GCC ARM 10.2, Cortex-M4 con FPU
- Debugging vía SEGGER RTT

## Estructura del proyecto

```
├── main.c              # Lógica de wear leveling y ejemplo de uso
├── makefile            # Build system (GCC ARM cross-compilation)
├── Autosar/            # Módulos MCAL (Mem, Mcu, Port, Dio, etc.) y BSW
├── Build/              # Archivos de configuración generados por Tresos
├── Rtt/                # SEGGER RTT para debugging por consola
├── config/             # Configuraciones de Tresos (.xdm)
└── S32K144.svd         # Archivo SVD para debugging con registros
```

## Requisitos

- NXP S32K144 EVB
- GCC ARM Embedded 10.2
- EB Tresos (para modificar configuración MCAL)
- SEGGER J-Link (debugging)
