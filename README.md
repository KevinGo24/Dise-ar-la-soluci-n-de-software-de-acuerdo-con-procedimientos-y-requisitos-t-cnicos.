# Dise-ar-la-soluci-n-de-software-de-acuerdo-con-procedimientos-y-requisitos-t-cnicos.

 220501095 
 
## . Documento de diseño de software

# Introducion 

El Sistema de Gestión de Asociados es una solución de software diseñada para centralizar, automatizar y optimizar la administración operativa y financiera de una cooperativa o fondo de empleados. Su objetivo principal es ofrecer un control integral sobre el registro de afiliados, sus saldos, el historial de operaciones y la generación de métricas clave para la toma de decisiones directivas.

Módulos Principales del Sistema
Administración de Asociados (CRUD): Permite registrar, consultar, actualizar y dar de baja la información personal e identificativa de los miembros.

- Gestión Transaccional: Facilita la ejecución inmediata de consignaciones y retiros, calculando automáticamente saldos actualizados y aplicando tarifas o comisiones operativas.

- Consultas Multi-moneda: Ofrece lectura instantánea de fondos en moneda local e integración para la conversión de saldos a divisas extranjeras (USD) según la TRM vigente.

- Auditoría e Historial de Movimientos: Mantiene la trazabilidad detallada de cada transacción efectuada (fecha, hora, tipo de movimiento, montos y comisiones).

Módulo de Reportes Gerenciales: Genera balances consolidados, métricas de asociatividad (mejores saldos y cuentas inactivas), análisis de flujo de caja por períodos de tiempo y ranking de transacciones de mayor impacto.

# Requisitos

Gestión de Asociados

RF-01 (Registro): El sistema debe permitir el registro de nuevos asociados capturando documento de identidad, nombre completo, correo electrónico y edad.

RF-02 (Consulta y Búsqueda): Debe permitir listar la totalidad de los asociados o realizar búsquedas específicas por número de documento o nombre.

RF-03 (Mantenimiento): Debe permitir la actualización de los datos personales y la eliminación (o inactivación) de un asociado existente.

Gestión Financiera y Transaccional

RF-04 (Consignaciones): El sistema debe registrar depósitos incrementando el saldo del asociado y generando la transacción correspondiente.

RF-05 (Retiros): Debe permitir el retiro de fondos previa validación de saldo disponible, calculando comisiones si aplican y actualizando el saldo neto.

RF-06 (Consulta de Saldo y TRM): Debe mostrar el saldo actual del asociado en moneda local y permitir su conversión a dólares (USD) según la TRM.

RF-07 (Historial de Movimientos): Debe registrar y mostrar el detalle de transacciones (tipo, fecha, hora, monto y tarifa) por cada asociado.

Informes de Gerencia

RF-08 (Consolidado General): Generar reportes con el saldo total en custodia, número total de asociados y saldo promedio.

RF-09 (Análisis de Cuentas): Identificar a los 5 asociados con mayor saldo (Top 5) y listar las cuentas inactivas/dormidas (sin movimientos).

RF-10 (Reportes por Período y Volúmenes): Calcular totales de ingresos/egresos en un rango de fechas (yyyy-MM-dd) y listar los mayores movimientos registrados.

2. Requisitos No Funcionales (RNF)
Rendimiento y Escalabilidad

RNF-01: Las operaciones transaccionales (depósitos/retiros) y consultas individuales deben responder en un tiempo inferior a 2 segundos.

RNF-02: La arquitectura debe permitir el crecimiento del número de asociados e historial de movimientos sin degradar el rendimiento de la base de datos.

Seguridad e Integridad de Datos

RNF-03 (Consistencia Financiera): Todas las transacciones deben realizarse bajo operaciones atómicas (ACID) para evitar descuadres en los saldos.

RNF-04 (Validación de Entradas): El sistema debe validar que los montos a retirar o consignar sean mayores a cero y que no se permitan retiros por encima del saldo disponible.

RNF-05 (Unicidad): El número de documento de identidad debe ser único por asociado.

Usabilidad e Interfaz

RNF-06: La interfaz (consola o GUI) debe incluir menús claros y mensajes de confirmación detallados tras cada acción exitosa o error.

RNF-07: Formateo estandarizado de valores numéricos y fechas en los reportes e historiales de cuenta.

# Arquitectura de Software

Para un Sistema de Gestión de Asociados desarrollado en .NET 10 en formato de Consola, la arquitectura ideal es una Arquitectura en Capas (Layered / Clean Architecture simplificada). Esta estructura separa las responsabilidades del menú interactivo, la lógica de negocio del fondo/cooperativa y la persistencia de datos.

## Arbol geneologico del proyecto
``
── PerformanceTest
│   ├── Interface
│   │   └── IMethods.cs
│   ├── Methods
│   │   └── Methods.cs
│   ├── Models
│   │   ├── Date.cs
│   │   ├── Person.cs
│   │   ├── Transaction.cs
│   │   └── User.cs
│   ├── PerformanceTest.csproj
│   ├── Program.cs
│   ├── Repository
│   ├── Service
│   │   ├── ManagementService.cs
│   │   └── TremService.cs
│   └── UI
│       ├── ManagementMenu.cs
│       └── Menu.cs
├── PerformanceTest.sln
└── README.md

```

 ### Dashboard Principal (Escritorio / Web)

 ```
+----------------------------------------------------------------------------------------------------+
|  [COOP] Sistema de Gestión de Asociados                                    [Admin] [Cerrar Sesión] |
+---------------------------------------+------------------------------------------------------------+
| MENU PRINCIPAL                        | RESUMEN GENERAL (INFORME 1)                                |
| ------------------------------------- | ---------------------------------------------------------- |
| [1]  (+) Registrar Asociado           |  +-------------------+ +-----------------+ +-------------+ |
| [2]  (::) Listar Asociados            |  | Saldo Total       | | Total Asociados | | Promedio    | |
| [3]  (?) Buscar por Documento         |  | $ 199.000,00      | | 1               | | $ 199.000,00| |
| [4]  (?) Buscar por Nombre            |  +-------------------+ +-----------------+ +-------------+ |
| [5]  (EDIT) Actualizar Datos          |                                                            |
| [6]  (DEL) Eliminar Asociado          | BÚSQUEDA RÁPIDA DE ASOCIADO                                |
| [7]  ($) Consultar Saldo              | ---------------------------------------------------------- |
| [8]  (USD) Consultar TRM              | [ Buscar por documento / nombre...                       ] |
| [9]  [+] Registrar Consignación       |                                                            |
| [10] [-] Registrar Retiro             | ASOCIADO SELECCIONADO                                      |
| [11] (REC) Ver Movimientos            | ---------------------------------------------------------- |
| [12] (REP) Informes Gerenciales       | ID: 1234 | Nombre: Kevin | Email: kevin@gmail.com |Edad: 23|
| [0]  (X) Salir                        | Saldo Actual: $ 199.000,00                                 |
+---------------------------------------+------------------------------------------------------------+
```

### Módulo 11: Historial de Movimientos

```
+----------------------------------------------------------------------------------------------------+
|  < Volver al Menú Principal           HISTORIAL DE MOVIMIENTOS                                    |
+----------------------------------------------------------------------------------------------------+
|  Asociado: 1234 - Kevin                                       Saldo Actual: 199.000,00 €          |
+----------------------------------------------------------------------------------------------------+
| FECHA Y HORA         | TIPO        | MONTO COMPLETO   | COMISIÓN  | SALDO RESULTANTE               |
| -------------------- | ----------- | ---------------- | --------- | ------------------------------ |
| 04/09/2026 18:58:33  | Depósito    | + 200.000,00 €   | 0,00 €    | 200.000,00 €                   |
| 04/09/2026 18:58:49  | Retiro      | - 1.000,00 €     | 0,00 €    | 199.000,00 €                   |
+----------------------------------------------------------------------------------------------------+
| [ Imprimir Extracto ]                                                   [ Exportar a PDF / Excel ] |
+----------------------------------------------------------------------------------------------------+
```

### Módulo 12: Informes Gerenciales (Gerencia)

```
+----------------------------------------------------------------------------------------------------+
|  INFORMES DE GERENCIA                                                                              |
+----------------------------------------------------------------------------------------------------+
|  [1. Resumen General]  [2. Mejores Asociados]  [3. Inactivos]  [4. Período]  [*5. Movimientos]    |
+----------------------------------------------------------------------------------------------------+
| FILTROS DE PERÍODO DE TIEMPO                                                                       |
| Fecha Inicial: [ 2026-09-04 ]    Fecha Final: [ 2026-09-04 ]    [ Aplicar Filtro ]                  |
| -------------------------------------------------------------------------------------------------- |
| METRICAS CLAVE                                                                                     |
| Total Consignaciones: 200.000,00 € (1) | Total Retiros: 1.000,00 € (1) | Balance Neto: 199.000,00 €|
| -------------------------------------------------------------------------------------------------- |
| TOP MOVIMIENTOS REGISTRADOS                                                                        |
|  * 2026-09-04 18:58 | Depósito | 200.000,00 € | Kevin (ID: 1234)                                   |
|  * 2026-09-04 18:58 | Retiro   |   1.000,00 € | Kevin (ID: 1234)                                   |
+----------------------------------------------------------------------------------------------------+
```
### Ventanas Emergentes (Modales para Depósito / Retiro)
#### Modal de Consignación
```
+----------------------------------------------------+
| REGISTRAR CONSIGNACIÓN                             |
+----------------------------------------------------+
| Documento del Asociado : [ 1234                  ] |
| Nombre del Asociado    : Kevin                     |
| Monto a Consignar      : [ 200000                ] |
| -------------------------------------------------- |
| [ Confirmar Depósito ]         [ Cancelar ]        |
+----------------------------------------------------+
```
#### Modal de Retiro:

```
+----------------------------------------------------+
| REGISTRAR RETIRO                                   |
+----------------------------------------------------+
| Documento del Asociado : [ 1234                  ] |
| Nombre del Asociado    : Kevin                     |
| Saldo Disponible       : 200.000,00 €              |
| Monto a Retirar        : [ 1000                  ] |
| Tarifa / Comisión      : 0,00 €                    |
| -------------------------------------------------- |
| [ Confirmar Retiro ]           [ Cancelar ]        |
+----------------------------------------------------+
```
