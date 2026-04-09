# TEST_CASES

## Convención de ejecución

- `Herramienta manual` indica la vía recomendada para ejecución QA funcional y trazabilidad en base de datos.
- `Automatización funcional` usa el stack API-first del workspace. `Disponible` significa que existe cobertura visible o alineada en los repos actuales. `Factible` significa que el caso encaja en el stack, pero no se marca como ya cubierto de forma explícita.
- `Automatización performance` solo aplica a smoke o carga sobre un subconjunto priorizado de casos críticos.
- Esta matriz es priorizada, no exhaustiva. Los alternos no listados aquí siguen siendo válidos para exploración, defectos o ampliaciones posteriores.

## HU-01 - Consultar estado y disponibilidad de un libro

### Fuente de verdad

- Spec aprobada: `.github/specs/hu-01-consultar-estado-disponibilidad-libro.spec.md`
- Historia base: `USER_STORIES.md`
- Contrato observable actual: `GET /api/v1/loans/{name}`

### Cobertura priorizada

- Smoke y crítico operacional: `TC-HU01-02`
- Happy path de disponibilidad con historial: `TC-HU01-01`
- Borde funcional por ausencia de historial: `TC-HU01-03`

### Datos base sugeridos

| Alias | Datos | Uso |
| --- | --- | --- |
| `BOOK-AVAILABLE-HISTORY-01` | `id_book=B-0901`, `title=Don Quijote`, último estado `RETURNED` | Libro disponible por historial cerrado |
| `BOOK-ON-LOAN-HISTORY-01` | `id_book=B-0902`, `title=La vorágine`, último estado `ON_LOAN` | Libro no disponible por préstamo activo |
| `BOOK-NO-HISTORY-QUERY-01` | `name=Manual de estanterías invisibles`, sin coincidencias en `loan_books` | Ausencia de historial operativo |

### Casos HU-01

#### TC-HU01-01 · Libro disponible por historial cerrado

```gherkin
Scenario: Libro disponible por historial cerrado
  Given existe un libro cuyo último registro en historial está en RETURNED
  When el bibliotecario consulta el libro por nombre
  Then el sistema informa la consulta correctamente y lo deja interpretable como disponible
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `BOOK-AVAILABLE-HISTORY-01` existe en `loan_books`. Su último estado registrado es `RETURNED`. |
| Datos de entrada | `name=Don Quijote` |
| Pasos de ejecución | 1. Enviar `GET /api/v1/loans/Don%20Quijote`. 2. Verificar la respuesta HTTP. 3. Validar la estructura funcional de la respuesta. 4. Confirmar que el resultado corresponde al último estado del historial. |
| Resultado esperado | `HTTP 200`. `success=true`. `message="Consulta realizada correctamente."`. `data` contiene al menos un resultado con `id=B-0901`, `name=Don Quijote` y `status=RETURNED`. Operativamente el libro queda interpretable como disponible para préstamo. |

#### TC-HU01-02 · Libro con préstamo activo

```gherkin
Scenario: Libro con préstamo activo
  Given existe un libro cuyo último registro en historial está en ON_LOAN
  When el bibliotecario consulta el libro por nombre
  Then el sistema informa que el libro tiene un préstamo activo y no está disponible
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `BOOK-ON-LOAN-HISTORY-01` existe en `loan_books`. Su último estado registrado es `ON_LOAN`. |
| Datos de entrada | `name=La vorágine` |
| Pasos de ejecución | 1. Enviar `GET /api/v1/loans/La%20vor%C3%A1gine`. 2. Verificar la respuesta HTTP. 3. Validar la estructura funcional de la respuesta. 4. Confirmar que el historial más reciente conserva estado activo. |
| Resultado esperado | `HTTP 200`. `success=true`. `message="Consulta realizada correctamente."`. `data` contiene al menos un resultado con `id=B-0902`, `name=La vorágine` y `status=ON_LOAN`. Operativamente el libro queda interpretable como no disponible para préstamo. |

#### TC-HU01-03 · Libro sin historial de préstamo

```gherkin
Scenario: Libro sin historial de préstamo
  Given la búsqueda no encuentra historial previo en loan_books para el nombre consultado
  When el bibliotecario realiza la consulta
  Then el sistema informa la ausencia de historial y considera el libro disponible sin hablar de inexistencia bibliográfica
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `BOOK-NO-HISTORY-QUERY-01` no tiene coincidencias previas en `loan_books`. |
| Datos de entrada | `name=Manual de estanterías invisibles` |
| Pasos de ejecución | 1. Enviar `GET /api/v1/loans/Manual%20de%20estanter%C3%ADas%20invisibles`. 2. Verificar la respuesta HTTP. 3. Validar la estructura funcional de la respuesta. 4. Confirmar que el mensaje describe ausencia de historial y no inexistencia del libro. |
| Resultado esperado | `HTTP 200`. `success=true`. `data=[]`. `message="El libro no registra historial de préstamo y se considera disponible para préstamo."`. La respuesta no afirma que el libro no exista; solo informa ausencia de historial operativo. |

## HU-02 - Registrar préstamo de un libro a un lector habilitado

### Fuente de verdad

- Spec aprobada: `.github/specs/hu-02-registrar-prestamo-libro.spec.md`
- Historia base: `USER_STORIES.md`
- Contrato observable actual: `POST /api/v1/loans`

### Cobertura priorizada

- Smoke y crítico: `TC-HU02-01`
- Crítico de negocio: `TC-HU02-03`
- Altos de validación y bloqueo: `TC-HU02-02`, `TC-HU02-04`

### Datos base sugeridos

| Alias | Datos | Uso |
| --- | --- | --- |
| `BOOK-AVAILABLE-01` | `id_book=B-1001`, `title=Cien años de soledad` | Flujo exitoso |
| `BOOK-ON-LOAN-01` | `id_book=B-1002`, `title=1984`, último estado `ON_LOAN` | Libro no disponible |
| `BOOK-AVAILABLE-02` | `id_book=B-1003`, `title=El principito` | Rechazo por deuda |
| `BOOK-AVAILABLE-03` | `id_book=B-1004`, `title=Rayuela` | Rechazo por plazo inválido |
| `READER-ENABLED-01` | `type_id_reader=CI`, `id_reader=R-2001`, `name_reader=Ana Torres`, sin deuda `PENDING` | Flujo exitoso y plazo inválido |
| `READER-ENABLED-02` | `type_id_reader=DNI`, `id_reader=R-2002`, `name_reader=Carlos Rojas`, sin deuda `PENDING` | Libro ya prestado |
| `READER-BLOCKED-01` | `type_id_reader=CI`, `id_reader=R-2003`, `name_reader=Laura Díaz`, deuda más reciente `PENDING` | Lector bloqueado |

### Casos HU-02

#### TC-HU02-01 · Préstamo exitoso

```gherkin
Scenario: Préstamo exitoso
  Given el libro está disponible y el lector no tiene deuda pendiente
  When el bibliotecario registra el préstamo con loan_days igual a 7
  Then el sistema crea el préstamo, calcula date_limit y deja el libro en ON_LOAN
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `BOOK-AVAILABLE-01` sin préstamo activo. `READER-ENABLED-01` sin deuda pendiente. |
| Datos de entrada | `BOOK-AVAILABLE-01`, `READER-ENABLED-01`, `loan_days=7` |
| Pasos de ejecución | 1. Enviar `POST /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Consultar `loan_books`. 4. Validar `state`, `date_return` y `date_limit`. |
| Resultado esperado | `HTTP 201`. Se crea un nuevo registro en `loan_books`. El préstamo queda con `state=ON_LOAN`, `date_return=null`, `loan_days=7`. `date_limit` corresponde a fecha de registro + 7 días. |

#### TC-HU02-02 · Libro ya prestado

```gherkin
Scenario: Libro ya prestado
  Given el libro ya tiene un préstamo activo
  When el bibliotecario intenta registrar un nuevo préstamo
  Then el sistema rechaza la operación y no crea un nuevo préstamo
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `BOOK-ON-LOAN-01` con último estado `ON_LOAN`. `READER-ENABLED-02` sin deuda pendiente. |
| Datos de entrada | `BOOK-ON-LOAN-01`, `READER-ENABLED-02`, `loan_days=14` |
| Pasos de ejecución | 1. Confirmar el estado activo del libro en historial. 2. Enviar `POST /api/v1/loans`. 3. Verificar código de respuesta. 4. Confirmar ausencia de inserción adicional en `loan_books`. |
| Resultado esperado | `HTTP 409`. Código esperado `BOOK_NOT_AVAILABLE`. No se crea nuevo registro en `loan_books`. |

#### TC-HU02-03 · Lector con deuda pendiente

```gherkin
Scenario: Lector con deuda pendiente
  Given el libro está disponible y el lector tiene una deuda pendiente
  When el bibliotecario intenta registrar el préstamo
  Then el sistema rechaza la operación y no crea el préstamo
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `BOOK-AVAILABLE-02` disponible. `READER-BLOCKED-01` con deuda más reciente `PENDING`. |
| Datos de entrada | `BOOK-AVAILABLE-02`, `READER-BLOCKED-01`, `loan_days=21` |
| Pasos de ejecución | 1. Confirmar deuda pendiente del lector. 2. Enviar `POST /api/v1/loans`. 3. Verificar la respuesta HTTP. 4. Confirmar que no hubo inserción en `loan_books`. |
| Resultado esperado | `HTTP 409`. Código esperado `READER_HAS_DEBT`. No se crea nuevo registro en `loan_books`. |

#### TC-HU02-04 · Plazo no permitido

```gherkin
Scenario: Plazo no permitido
  Given el libro está disponible y el lector no tiene deuda pendiente
  When el bibliotecario registra un préstamo con un plazo no permitido
  Then el sistema rechaza la operación y no crea el préstamo
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `BOOK-AVAILABLE-03` disponible. `READER-ENABLED-01` sin deuda pendiente. |
| Datos de entrada | `BOOK-AVAILABLE-03`, `READER-ENABLED-01`, `loan_days=10` |
| Pasos de ejecución | 1. Enviar `POST /api/v1/loans` con `loan_days=10`. 2. Verificar la respuesta HTTP. 3. Consultar `loan_books`. 4. Confirmar que no se creó un registro para `B-1004`. |
| Resultado esperado | `HTTP 400`. Código esperado `INVALID_LOAN_DAYS`. No se crea nuevo registro en `loan_books`. |

## HU-03 - Registrar devolución de un libro dentro del plazo

### Fuente de verdad

- Spec aprobada: `.github/specs/hu-03-registrar-devolucion-en-plazo.spec.md`
- Historia base: `USER_STORIES.md`
- Contrato observable actual: `PATCH /api/v1/loans`

### Cobertura priorizada

- Smoke y crítico de borde: `TC-HU03-02`
- Happy path principal: `TC-HU03-01`
- Altos de rechazo: `TC-HU03-03`, `TC-HU03-04`

### Datos base sugeridos

| Alias | Datos | Uso |
| --- | --- | --- |
| `LOAN-ACTIVE-EARLY-01` | `loan_id=L-3001`, `id_book=B-1101`, `id_reader=R-2101`, `type_id_reader=CI`, `state=ON_LOAN`, `date_limit=2026-04-10` | Devolución antes del límite |
| `LOAN-ACTIVE-ON-LIMIT-01` | `loan_id=L-3002`, `id_book=B-1102`, `id_reader=R-2102`, `type_id_reader=DNI`, `state=ON_LOAN`, `date_limit=2026-04-10` | Borde exacto de `date_limit` |
| `BOOK-WITHOUT-ACTIVE-LOAN-01` | `id_book=B-1103`, `id_reader=R-2103`, sin fila activa `ON_LOAN` | No existe préstamo activo |
| `LOAN-ALREADY-RETURNED-01` | `loan_id=L-3004`, `id_book=B-1104`, `id_reader=R-2104`, último estado `RETURNED`, `date_return` informado | Devolución duplicada |

### Casos HU-03

#### TC-HU03-01 · Devolución antes de fecha límite

```gherkin
Scenario: Devolución antes de fecha límite
  Given existe un préstamo activo
  When el bibliotecario registra la devolución antes de date_limit
  Then el sistema cierra el préstamo sin generar deuda
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `LOAN-ACTIVE-EARLY-01` con `state=ON_LOAN` y `date_limit=2026-04-10`. |
| Datos de entrada | `id_book=B-1101`, `id_reader=R-2101`, `type_id_reader=CI`, `date_return=2026-04-08` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Consultar `loan_books`. 4. Confirmar ausencia de nueva deuda en `debt_reader`. |
| Resultado esperado | `HTTP 200`. El mismo préstamo queda con `state=RETURNED`. `date_return=2026-04-08`. `days_late=0` o equivalente. No se crea deuda nueva. |

#### TC-HU03-02 · Devolución el mismo día de fecha límite

```gherkin
Scenario: Devolución el mismo día de fecha límite
  Given existe un préstamo activo
  When el bibliotecario registra la devolución el mismo día de date_limit
  Then el sistema cierra el préstamo, no genera deuda y el libro vuelve a quedar disponible
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `LOAN-ACTIVE-ON-LIMIT-01` con `state=ON_LOAN` y `date_limit=2026-04-10`. |
| Datos de entrada | `id_book=B-1102`, `id_reader=R-2102`, `type_id_reader=DNI`, `date_return=2026-04-10` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Confirmar actualización del préstamo en `loan_books`. 4. Confirmar que no se creó deuda en `debt_reader`. |
| Resultado esperado | `HTTP 200`. El préstamo queda en `RETURNED`. `date_return=2026-04-10`. No se crea deuda nueva. El último estado del libro queda `RETURNED`, por lo tanto vuelve a estar disponible. |

#### TC-HU03-03 · Sin préstamo activo

```gherkin
Scenario: Sin préstamo activo
  Given no existe un préstamo activo para el libro o lector consultado
  When el bibliotecario intenta registrar la devolución
  Then el sistema rechaza la operación y no altera historial ni deuda
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `BOOK-WITHOUT-ACTIVE-LOAN-01` sin fila activa `ON_LOAN`. |
| Datos de entrada | `id_book=B-1103`, `id_reader=R-2103`, `type_id_reader=CI`, `date_return=2026-04-10` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar el código de respuesta. 3. Consultar `loan_books`. 4. Confirmar ausencia de nueva deuda en `debt_reader`. |
| Resultado esperado | `HTTP 404`. Código esperado `LOAN_NOT_FOUND`. Sin cambios en `loan_books`. Sin deuda nueva. |

#### TC-HU03-04 · Devolución duplicada

```gherkin
Scenario: Devolución duplicada
  Given el préstamo ya fue devuelto
  When el bibliotecario intenta registrar nuevamente la devolución
  Then el sistema rechaza la operación y no altera historial ni deuda
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `LOAN-ALREADY-RETURNED-01` con último estado `RETURNED` y `date_return` informado. |
| Datos de entrada | `id_book=B-1104`, `id_reader=R-2104`, `type_id_reader=CI`, `date_return=2026-04-11` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans` con los mismos identificadores. 2. Verificar el código de respuesta. 3. Confirmar que no hubo una nueva actualización en `loan_books`. 4. Confirmar que no se creó deuda en `debt_reader`. |
| Resultado esperado | `HTTP 409`. Código esperado `ALREADY_RETURNED`. No se modifica `loan_books`. No se crea deuda nueva. |

## HU-04 - Registrar devolución tardía y generar multa Fibonacci

### Fuente de verdad

- Spec aprobada: `.github/specs/hu-04-registrar-devolucion-tardia-generar-multa.spec.md`
- Reglas base: `PRD.md`
- Casos obligatorios de mora: `1`, `7`, `8`, `15` y `22` días
- Contrato observable actual: `PATCH /api/v1/loans`

### Cobertura priorizada

- Smoke y críticos del cálculo: `TC-HU04-01`, `TC-HU04-02`, `TC-HU04-03`
- Altos de cambio de tramo: `TC-HU04-04`, `TC-HU04-05`

### Matriz de referencia Fibonacci

| Días de mora | Semanas esperadas | Unidades Fibonacci esperadas | `amount_debt` esperado con `base_fib_amount=2.00` |
| --- | --- | --- | --- |
| `1` | `1` | `1` | `2.00` |
| `7` | `1` | `1` | `2.00` |
| `8` | `2` | `2` | `4.00` |
| `15` | `3` | `4` | `8.00` |
| `22` | `4` | `7` | `14.00` |

### Datos base sugeridos

| Alias | Datos | Uso |
| --- | --- | --- |
| `LOAN-LATE-01D-01` | `loan_id=L-4001`, `id_book=B-1201`, `id_reader=R-2201`, `type_id_reader=CI`, `state=ON_LOAN`, `date_limit=2026-04-10` | Mora de 1 día |
| `LOAN-LATE-07D-01` | `loan_id=L-4002`, `id_book=B-1202`, `id_reader=R-2202`, `type_id_reader=DNI`, `state=ON_LOAN`, `date_limit=2026-04-10` | Mora de 7 días |
| `LOAN-LATE-08D-01` | `loan_id=L-4003`, `id_book=B-1203`, `id_reader=R-2203`, `type_id_reader=CI`, `state=ON_LOAN`, `date_limit=2026-04-10` | Mora de 8 días |
| `LOAN-LATE-15D-01` | `loan_id=L-4004`, `id_book=B-1204`, `id_reader=R-2204`, `type_id_reader=CC`, `state=ON_LOAN`, `date_limit=2026-04-10` | Mora de 15 días |
| `LOAN-LATE-22D-01` | `loan_id=L-4005`, `id_book=B-1205`, `id_reader=R-2205`, `type_id_reader=TI`, `state=ON_LOAN`, `date_limit=2026-04-10` | Mora de 22 días |

### Casos HU-04

#### TC-HU04-01 · Mora de 1 día

```gherkin
Scenario: Mora de 1 día
  Given existe un préstamo activo vencido
  When el bibliotecario registra la devolución con 1 día de mora
  Then el sistema cierra el préstamo y crea una deuda PENDING equivalente a 1 unidad Fibonacci
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `LOAN-LATE-01D-01` en `ON_LOAN`. No existe deuda previa para ese `loan_id`. |
| Datos de entrada | `id_book=B-1201`, `id_reader=R-2201`, `type_id_reader=CI`, `date_return=2026-04-11`, `base_fib_amount=2.00` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Confirmar cierre del préstamo en `loan_books`. 4. Confirmar creación de deuda en `debt_reader`. |
| Resultado esperado | `HTTP 200`. `state=RETURNED`. `days_late=1`. `units_fib=1`. `amount_debt=2.00`. La deuda queda en `PENDING` y trazable al `loan_id`. |

#### TC-HU04-02 · Mora de 7 días

```gherkin
Scenario: Mora de 7 días
  Given existe un préstamo activo vencido
  When el bibliotecario registra la devolución con 7 días de mora
  Then el sistema mantiene el tramo de semana 1 y crea deuda acumulada de 1 unidad Fibonacci
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `LOAN-LATE-07D-01` en `ON_LOAN`. No existe deuda previa para ese `loan_id`. |
| Datos de entrada | `id_book=B-1202`, `id_reader=R-2202`, `type_id_reader=DNI`, `date_return=2026-04-17`, `base_fib_amount=2.00` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Confirmar cierre del préstamo. 4. Confirmar deuda creada en `debt_reader`. |
| Resultado esperado | `HTTP 200`. `days_late=7`. `weeks=1`. `units_fib=1`. `amount_debt=2.00`. No debe saltar a semana 2. |

#### TC-HU04-03 · Mora de 8 días

```gherkin
Scenario: Mora de 8 días
  Given existe un préstamo activo vencido
  When el bibliotecario registra la devolución con 8 días de mora
  Then el sistema cambia al tramo de semana 2 y crea deuda acumulada de 2 unidades Fibonacci
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `LOAN-LATE-08D-01` en `ON_LOAN`. No existe deuda previa para ese `loan_id`. |
| Datos de entrada | `id_book=B-1203`, `id_reader=R-2203`, `type_id_reader=CI`, `date_return=2026-04-18`, `base_fib_amount=2.00` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Confirmar cierre del préstamo. 4. Confirmar deuda creada en `debt_reader`. |
| Resultado esperado | `HTTP 200`. `days_late=8`. `weeks=2`. `units_fib=2`. `amount_debt=4.00`. |

#### TC-HU04-04 · Mora de 15 días

```gherkin
Scenario: Mora de 15 días
  Given existe un préstamo activo vencido
  When el bibliotecario registra la devolución con 15 días de mora
  Then el sistema cambia al tramo de semana 3 y crea deuda acumulada de 4 unidades Fibonacci
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `LOAN-LATE-15D-01` en `ON_LOAN`. No existe deuda previa para ese `loan_id`. |
| Datos de entrada | `id_book=B-1204`, `id_reader=R-2204`, `type_id_reader=CC`, `date_return=2026-04-25`, `base_fib_amount=2.00` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Confirmar cierre del préstamo. 4. Confirmar deuda creada en `debt_reader`. |
| Resultado esperado | `HTTP 200`. `days_late=15`. `weeks=3`. `units_fib=4`. `amount_debt=8.00`. |

#### TC-HU04-05 · Mora de 22 días

```gherkin
Scenario: Mora de 22 días
  Given existe un préstamo activo vencido
  When el bibliotecario registra la devolución con 22 días de mora
  Then el sistema cambia al tramo de semana 4 y crea deuda acumulada de 7 unidades Fibonacci
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `LOAN-LATE-22D-01` en `ON_LOAN`. No existe deuda previa para ese `loan_id`. |
| Datos de entrada | `id_book=B-1205`, `id_reader=R-2205`, `type_id_reader=TI`, `date_return=2026-05-02`, `base_fib_amount=2.00` |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/loans`. 2. Verificar la respuesta HTTP. 3. Confirmar cierre del préstamo. 4. Confirmar deuda creada en `debt_reader`. |
| Resultado esperado | `HTTP 200`. `days_late=22`. `weeks=4`. `units_fib=7`. `amount_debt=14.00`. |

## HU-05 - Consultar préstamos vencidos y lector responsable

### Fuente de verdad

- Spec aprobada: `.github/specs/hu-05-consultar-préstamos-vencidos-y-lector.spec.md`
- Historia base: `USER_STORIES.md`
- Contrato observable actual: `GET /api/v1/loans/outTime`

### Cobertura priorizada

- Smoke y crítico operacional: `TC-HU05-01`
- Alto de gestión sin atrasos: `TC-HU05-02`
- Alto de exclusión correcta de registros no vencidos: `TC-HU05-03`

### Datos base sugeridos

| Alias | Datos | Uso |
| --- | --- | --- |
| `OVERDUE-LOAN-01` | `loan_id=L-5001`, `id_book=B-1251`, `title=La Odisea`, `type_id_reader=CC`, `id_reader=R-2251`, `name_reader=Sara Mena`, `state=ON_LOAN`, `date_limit=2026-03-20`, `date_return=null` | Préstamo vencido visible en consulta |
| `OVERDUE-LOAN-02` | `loan_id=L-5002`, `id_book=B-1252`, `title=El Aleph`, `type_id_reader=TI`, `id_reader=R-2252`, `name_reader=Bruno Paz`, `state=ON_LOAN`, `date_limit=2026-03-24`, `date_return=null` | Segundo préstamo vencido para validar listado múltiple |
| `ACTIVE-NOT-DUE-01` | `loan_id=L-5003`, `id_book=B-1253`, `title=Ensayo sobre la ceguera`, `type_id_reader=CI`, `id_reader=R-2253`, `name_reader=Julia Lara`, `state=ON_LOAN`, `date_limit=2026-03-30`, `date_return=null` | Préstamo vigente que no debe aparecer |
| `RETURNED-PAST-LIMIT-01` | `loan_id=L-5004`, `id_book=B-1254`, `title=La tregua`, `type_id_reader=DNI`, `id_reader=R-2254`, `name_reader=Marco Gil`, `state=RETURNED`, `date_limit=2026-03-10`, `date_return=2026-03-18` | Préstamo cerrado que no debe mezclarse en la consulta |

### Casos HU-05

#### TC-HU05-01 · Listado correcto de préstamos vencidos

```gherkin
Scenario: Listado correcto de préstamos vencidos
  Given existen préstamos fuera de plazo
  When el bibliotecario consulta los préstamos vencidos
  Then el sistema lista solo los atrasados e identifica el lector responsable de cada uno
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | Existen `OVERDUE-LOAN-01` y `OVERDUE-LOAN-02` en `loan_books`. Ambos están en `state=ON_LOAN` con `date_limit` menor a la fecha actual. No hay filtros adicionales aplicados en el request. |
| Datos de entrada | Sin body ni query params. Endpoint `GET /api/v1/loans/outTime`. |
| Pasos de ejecución | 1. Enviar `GET /api/v1/loans/outTime`. 2. Verificar la respuesta HTTP. 3. Validar la estructura funcional de la respuesta. 4. Confirmar que cada fila listada incluye libro, lector responsable, estado y `date_limit`. 5. Verificar que `date_return` llegue en `null` para los préstamos activos vencidos. |
| Resultado esperado | `HTTP 200`. `data` contiene al menos `L-5001` y `L-5002`. `count=2` o equivalente al total de filas vencidas preparadas. Cada elemento expone `loan_id`, `id_book`, `title`, `state=ON_LOAN`, `id_reader`, `name_reader`, `date_limit` y `date_return=null`. La salida sirve para gestión operativa porque permite identificar qué libro está vencido y quién es el lector responsable. |

#### TC-HU05-02 · Sin préstamos vencidos

```gherkin
Scenario: Sin préstamos vencidos
  Given no existen préstamos vencidos
  When el bibliotecario consulta la bandeja de atrasos
  Then el sistema informa que no hay registros fuera de plazo sin mezclar préstamos vigentes o cerrados
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | No existen filas en `loan_books` con `state=ON_LOAN` y `date_limit` menor a la fecha actual. Si existen préstamos, todos están vigentes o cerrados. |
| Datos de entrada | Sin body ni query params. Endpoint `GET /api/v1/loans/outTime`. |
| Pasos de ejecución | 1. Preparar un set sin préstamos vencidos. 2. Enviar `GET /api/v1/loans/outTime`. 3. Verificar la respuesta HTTP. 4. Confirmar que la consulta no devuelve filas operativas vencidas. 5. Si se valida la UI, comprobar el estado vacío mostrado al usuario. |
| Resultado esperado | `HTTP 200`. `data=[]`. `count=0`. La consulta informa ausencia de atrasos mediante lista vacía. Si se valida la vista, se muestra un estado vacío equivalente a "No hay préstamos vencidos en este momento". |

#### TC-HU05-03 · Exclusión de vigentes y devueltos

```gherkin
Scenario: Exclusión de vigentes y devueltos
  Given existe una mezcla de préstamos vencidos, vigentes y ya cerrados
  When el bibliotecario consulta los préstamos fuera de plazo
  Then el sistema excluye los registros vigentes o RETURNED y deja solo los realmente atrasados
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | Existen `OVERDUE-LOAN-01`, `ACTIVE-NOT-DUE-01` y `RETURNED-PAST-LIMIT-01` en `loan_books`. `OVERDUE-LOAN-01` cumple `state=ON_LOAN` y `date_limit` vencida. `ACTIVE-NOT-DUE-01` sigue vigente. `RETURNED-PAST-LIMIT-01` está cerrado con `state=RETURNED`. |
| Datos de entrada | Sin body ni query params. Endpoint `GET /api/v1/loans/outTime`. |
| Pasos de ejecución | 1. Confirmar en base de datos la mezcla de estados y fechas límite. 2. Enviar `GET /api/v1/loans/outTime`. 3. Verificar la respuesta HTTP. 4. Confirmar que `L-5001` sí aparece en `data`. 5. Confirmar que `L-5003` y `L-5004` no aparecen en la respuesta. 6. Validar que todas las filas devueltas cumplan `state=ON_LOAN` y `date_limit` menor a hoy. |
| Resultado esperado | `HTTP 200`. `data` contiene solo préstamos con `state=ON_LOAN` y `date_limit` vencida. No aparecen préstamos vigentes ni registros con `state=RETURNED`. La consulta no mezcla ruido operativo y conserva únicamente atrasos reales para seguimiento. |

## HU-06 - Registrar pago total de multa y rehabilitar lector

### Fuente de verdad

- Spec aprobada: `.github/specs/hu-06-registrar-pago-total-multa-rehabilitar-lector.spec.md`
- Historia base: `USER_STORIES.md`
- Contrato observable actual de pago: `PATCH /api/v1/debts/{id_debt}`
- Validación cruzada de rehabilitación: `POST /api/v1/loans`

### Cobertura priorizada

- Smoke y crítico de rehabilitación encadenada: `TC-HU06-04`
- Happy path principal de pago total: `TC-HU06-01`
- Alto de rechazo por deuda no pagable: `TC-HU06-02`
- Alto de habilitación operativa posterior al pago: `TC-HU06-03`

### Datos base sugeridos

| Alias | Datos | Uso |
| --- | --- | --- |
| `DEBT-PENDING-01` | `id_debt=D-6001`, `loan_id=L-6001`, `type_id_reader=CI`, `id_reader=R-2301`, `name_reader=María León`, `amount_debt=14.00`, `state_debt=PENDING` | Pago total exitoso y rehabilitación |
| `DEBT-PAID-01` | `id_debt=D-6002`, `loan_id=L-6002`, `type_id_reader=DNI`, `id_reader=R-2302`, `name_reader=Luis Pardo`, `amount_debt=8.00`, `state_debt=PAID` | Rechazo por deuda ya resuelta |
| `DEBT-NOT-FOUND-01` | `id_debt=999999`, sin coincidencias en `debt_reader` | Rechazo por deuda inexistente |
| `BOOK-AVAILABLE-04` | `id_book=B-1301`, `title=El otoño del patriarca`, sin préstamo activo | Nuevo préstamo tras rehabilitación |
| `BOOK-AVAILABLE-05` | `id_book=B-1302`, `title=La casa verde`, sin préstamo activo | Secuencia rechazo antes y aceptación después |
| `READER-REHAB-01` | `type_id_reader=CI`, `id_reader=R-2301`, `name_reader=María León`, con deuda más reciente `PENDING` y luego `PAID` | Flujo principal HU-06 |

### Casos HU-06

#### TC-HU06-01 · Pago total exitoso de deuda pendiente

```gherkin
Scenario: Pago total exitoso de deuda pendiente
  Given el lector tiene una deuda pendiente
  When el bibliotecario registra el pago total
  Then el sistema marca la deuda como PAID y el lector deja de figurar como bloqueado
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | k6 (`Disponible`) |
| Precondiciones | `DEBT-PENDING-01` existe en `debt_reader` con `state_debt=PENDING`. No existe otro pago posterior para la misma deuda. |
| Datos de entrada | `id_debt=D-6001`. Body: `state_debt=PAID`. |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/debts/D-6001`. 2. Verificar la respuesta HTTP. 3. Consultar `debt_reader` por `id_debt`. 4. Confirmar que el lector ya no aparece con esa deuda en estado `PENDING`. |
| Resultado esperado | `HTTP 200`. `success=true`. `data.id_debt=D-6001`. `data.state_debt=PAID`. `amount_debt` se conserva como valor histórico pagado y no queda saldo residual pendiente para esa deuda. El lector deja de figurar bloqueado por `DEBT-PENDING-01`. |

#### TC-HU06-02 · Intento de pago sobre deuda inexistente o ya resuelta

```gherkin
Scenario: Intento de pago sobre deuda inexistente o ya resuelta
  Given el bibliotecario intenta pagar una deuda inexistente o ya resuelta
  When registra el pago total
  Then el sistema rechaza la operación y no modifica deudas existentes
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Factible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | Variante A: `DEBT-NOT-FOUND-01` no existe en `debt_reader`. Variante B: `DEBT-PAID-01` existe con `state_debt=PAID`. |
| Datos de entrada | Variante A: `id_debt=999999`, body `state_debt=PAID`. Variante B: `id_debt=D-6002`, body `state_debt=PAID`. |
| Pasos de ejecución | 1. Enviar `PATCH /api/v1/debts/999999`. 2. Verificar rechazo por deuda inexistente. 3. Enviar `PATCH /api/v1/debts/D-6002`. 4. Verificar rechazo por deuda ya pagada. 5. Confirmar que no hubo cambios en `debt_reader`. |
| Resultado esperado | Variante A: `HTTP 404` con código `DEBT_NOT_FOUND`. Variante B: `HTTP 409` con código `DEBT_ALREADY_PAID`. No se crea ni modifica ninguna deuda. La operación no deja efectos laterales sobre otros lectores o préstamos. |

#### TC-HU06-03 · Rehabilitación efectiva para nuevo préstamo

```gherkin
Scenario: Rehabilitación efectiva para nuevo préstamo
  Given el lector tenía una deuda pendiente y ya registró el pago total
  When el bibliotecario intenta un nuevo préstamo con un libro disponible
  Then el sistema permite la operación porque el lector quedó rehabilitado
```

| Campo | Valor |
| --- | --- |
| Prioridad | Alto |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `READER-REHAB-01` tuvo una deuda previa y la más reciente ya quedó en `PAID`. `BOOK-AVAILABLE-04` está disponible. No existe otra deuda `PENDING` para `R-2301`. |
| Datos de entrada | `BOOK-AVAILABLE-04`, `READER-REHAB-01`, `loan_days=7` |
| Pasos de ejecución | 1. Confirmar en `debt_reader` que la deuda más reciente del lector está en `PAID`. 2. Enviar `POST /api/v1/loans` con `B-1301` y `R-2301`. 3. Verificar la respuesta HTTP. 4. Confirmar creación del préstamo en `loan_books`. |
| Resultado esperado | `HTTP 201`. Se crea un nuevo préstamo para `B-1301`. El nuevo registro queda con `state=ON_LOAN` y `date_return=null`. No se retorna `READER_HAS_DEBT`. La rehabilitación se considera efectiva porque el lector vuelve a quedar habilitado para prestar. |

#### TC-HU06-04 · Rechazo antes del pago y aceptación después del pago total

```gherkin
Scenario: Rechazo antes del pago y aceptación después del pago total
  Given el lector tiene deuda pendiente y luego la paga totalmente
  When el bibliotecario intenta prestar antes y después del pago
  Then el sistema rechaza el préstamo mientras la deuda está PENDING y lo acepta una vez queda en PAID
```

| Campo | Valor |
| --- | --- |
| Prioridad | Crítico |
| Seguimiento | Estado: Ejecutado. Resultado obtenido: Exitoso y conforme con lo esperado. |
| Herramienta manual | Postman + pgAdmin o `psql` |
| Automatización funcional | Karate (`Disponible`) |
| Automatización performance | No aplica (`No priorizada`) |
| Precondiciones | `DEBT-PENDING-01` existe con `state_debt=PENDING` para `READER-REHAB-01`. `BOOK-AVAILABLE-05` está disponible. No existe préstamo activo previo sobre `B-1302`. |
| Datos de entrada | Antes del pago: `BOOK-AVAILABLE-05`, `READER-REHAB-01`, `loan_days=14`. Pago: `id_debt=D-6001`, body `state_debt=PAID`. Después del pago: mismos datos del préstamo. |
| Pasos de ejecución | 1. Enviar `POST /api/v1/loans` con `B-1302` y `R-2301` antes del pago. 2. Verificar rechazo y ausencia de inserción en `loan_books`. 3. Enviar `PATCH /api/v1/debts/D-6001` con `state_debt=PAID`. 4. Verificar actualización de la deuda. 5. Reenviar `POST /api/v1/loans` con los mismos datos. 6. Confirmar creación exitosa del préstamo. |
| Resultado esperado | Antes del pago: `HTTP 409` con código `READER_HAS_DEBT` y sin nuevo registro en `loan_books`. Pago: `HTTP 200` con `state_debt=PAID`. Después del pago: `HTTP 201` y nuevo préstamo creado con `state=ON_LOAN`. Este caso valida de extremo a extremo que pago y rehabilitación son una misma cadena operativa. |

## Lista rápida para sub-tareas en GitHub Projects

- HU-01: `TC-HU01-01`, `TC-HU01-02`, `TC-HU01-03`
- HU-02: `TC-HU02-01`, `TC-HU02-02`, `TC-HU02-03`, `TC-HU02-04`
- HU-03: `TC-HU03-01`, `TC-HU03-02`, `TC-HU03-03`, `TC-HU03-04`
- HU-04: `TC-HU04-01`, `TC-HU04-02`, `TC-HU04-03`, `TC-HU04-04`, `TC-HU04-05`
- HU-05: `TC-HU05-01`, `TC-HU05-02`, `TC-HU05-03`
- HU-06: `TC-HU06-01`, `TC-HU06-02`, `TC-HU06-03`, `TC-HU06-04`