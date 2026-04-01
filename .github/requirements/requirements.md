# Requirements
# .github/requirements/requirements.md
#
# Este archivo es la fuente de verdad para todos los requerimientos del proyecto.
# Es redactado por el equipo de producto y no debe ser modificado por ningún agente.
# Los agentes lo leen como input; nunca escriben en él.

---

# Requerimientos de Negocio

## Descripción del sistema
Sistema de gestión interna para bibliotecas que permite administrar préstamos de libros,
controlar disponibilidad, calcular multas por retraso y gestionar la habilitación de lectores.

## Usuarios del sistema
**Único perfil**: Personal administrativo de la biblioteca.
No existe un portal para lectores. Todas las operaciones las realiza un bibliotecario
en nombre del lector o del libro.

## Dominio y entidades principales

### Libro
- Se identifica por un código o identificador interno.
- No existe un catálogo maestro de validación bibliográfica (fuera de alcance del MVP).
- Su disponibilidad se deriva del último estado conocido en su historial de préstamos.
- Si un libro no registra historial, se considera disponible para préstamo.
- Mantiene un historial completo de todos los préstamos realizados.
- Estados posibles de un préstamo: `ON_LOAN` y `RETURNED`.

### Lector
- Se identifica con documento oficial: cédula o DNI.
- Puede estar en dos condiciones: habilitado (sin deuda) o bloqueado (con deuda pendiente).
- Un lector bloqueado no puede recibir nuevos préstamos hasta saldar su deuda completa.

### Préstamo
- Registra: libro, lector, fecha de inicio, plazo elegido y fecha límite de devolución.
- Plazos válidos exclusivamente: 7, 14 o 21 días. Ningún otro valor es aceptado.
- La fecha límite se calcula sumando el plazo elegido a la fecha de inicio.
- Si la devolución ocurre en o antes de la fecha límite, no se genera multa.
- Si la devolución ocurre después de la fecha límite, se genera una multa automáticamente.

### Multa
- Se genera automáticamente al detectar retraso en la devolución.
- Modelo de cálculo: escala Fibonacci acumulativa por semanas de retraso.
- El retraso empieza a contarse desde el día siguiente a la fecha límite.
- Agrupación: cada 7 días calendario completos equivalen a una semana de mora.
- Secuencia Fibonacci aplicada: 1, 1, 2, 3, 5, 8... (una porción por semana vencida).
- La deuda es acumulativa: cada nueva semana suma su porción a la deuda anterior.
- La biblioteca define el valor monetario base por unidad Fibonacci.
- El pago debe ser total; no se aceptan pagos parciales (fuera de alcance del MVP).
- El pago total elimina la deuda y rehabilita al lector para nuevos préstamos.

## Reglas de negocio críticas
1. No se puede prestar un libro que no está disponible.
2. No se puede prestar un libro a un lector con deuda pendiente.
3. La validación de deuda es obligatoria antes de registrar cualquier préstamo.
4. Un préstamo cerrado (devuelto) permanece en el historial del libro.
5. El mensaje correcto ante un libro sin historial es "no registra historial de préstamo",
   nunca "el libro no existe en la biblioteca".

## Alcance del MVP

### Incluido
- Registrar préstamo de un libro disponible a un lector habilitado
- Elegir plazo de devolución (7, 14 o 21 días)
- Calcular automáticamente la fecha de devolución
- Registrar devolución de un libro
- Generar multa automáticamente ante retraso
- Aumentar multa semanalmente siguiendo escala Fibonacci
- Registrar pago total de multa y rehabilitar lector
- Consultar préstamos vencidos con datos del lector responsable
- Consultar lectores con deuda pendiente
- Bloquear préstamos a lectores con deuda pendiente
- Mantener historial de préstamos por libro

### Excluido
- Portal o vista para lectores
- Prórroga de préstamos
- Alta y baja de libros (catálogo maestro)
- Reservas de libros
- Administración de usuarios y membresías
- Pagos parciales de multas
- Notificaciones automáticas
- Exportación de datos
- Filtros por lector o rango de fechas en consulta de vencidos
- Ordenamiento configurable
- Paginación

---

# Requerimientos UX/UI

## Identidad de marca

### Nombre
Biblioteca

### Personalidad y tono
- **Adjetivos**: Cálida, acogedora, confiable, ordenada, clara.
- **NO**: Fría, corporativa, tecnológica, minimalista extrema, lúdica.
- **Voz en la UI**: Directa y respetuosa. Mensajes en segunda persona hacia el bibliotecario.
  Ejemplo: "El lector tiene una deuda pendiente. Registra el pago antes de continuar."
  Nunca: "Error 403. Operación no permitida."

### Cultura y valores
- El sistema es una herramienta de trabajo diario para el bibliotecario; debe sentirse
  familiar y predecible desde el primer día de uso.
- La claridad operativa es prioritaria sobre la estética. Si algo se ve bonito pero
  genera dudas sobre qué hacer, el diseño está mal.
- Los mensajes del sistema deben hablar el idioma del dominio: libros, lectores,
  préstamos, multas. Nunca términos técnicos.

## Paleta de colores

### Filosofía
Paleta cálida inspirada en el ambiente físico de una biblioteca: papel envejecido,
madera, cuero, tinta. Acogedora pero con suficiente contraste para uso profesional.

### Tokens requeridos

| Token | Descripción | Referencia visual |
|-------|-------------|-------------------|
| `--color-brand-primary` | Ocre dorado — color principal de acción | #C17F24 |
| `--color-brand-secondary` | Terracota suave — acento secundario | #A0522D |
| `--color-surface-base` | Crema cálida — fondo de página | #FAF7F2 |
| `--color-surface-raised` | Blanco marfil — tarjetas y modales | #FFFDF9 |
| `--color-surface-overlay` | Beige medio — sidebar y paneles laterales | #F2EDE4 |
| `--color-text-primary` | Marrón oscuro — texto principal | #2C1A0E |
| `--color-text-secondary` | Marrón medio — texto secundario y etiquetas | #7A5C3E |
| `--color-text-disabled` | Beige grisáceo — texto deshabilitado | #C4B49A |
| `--color-text-inverse` | Crema clara — texto sobre fondos oscuros | #FAF7F2 |
| `--color-border-default` | Arena — bordes por defecto | #DDD0BC |
| `--color-border-focus` | Ocre intenso — anillo de foco | #C17F24 |
| `--color-feedback-success` | Verde oliva — estados exitosos | #5A7A3A |
| `--color-feedback-warning` | Ámbar — estados de advertencia | #D4860B |
| `--color-feedback-error` | Rojo ladrillo — errores y acciones destructivas | #B83C2B |
| `--color-feedback-info` | Azul pizarra — información neutral | #4A6FA5 |

### Reglas semánticas de color
- `--color-brand-primary` se usa exclusivamente en: botón primario, estado activo de
  navegación, anillo de foco, enlaces accionables.
- `--color-feedback-error` se usa exclusivamente en: estados de error, mensajes de
  validación, botón de acción destructiva (eliminar, rechazar).
- `--color-feedback-warning` se usa para: lector con deuda pendiente (badge de estado),
  préstamos próximos a vencer.
- `--color-feedback-success` se usa para: devolución registrada, pago confirmado,
  lector habilitado (badge de estado).
- Nunca usar colores de feedback con propósito decorativo.
- Nunca usar hex directamente en el código de componentes; siempre referenciar tokens.

## Tipografía

### Fuentes
- **Principal**: Lora (Google Fonts) — serif humanista, cálida y legible. Usar en
  headings y títulos de página.
- **Funcional**: Inter (Google Fonts) — sans-serif, para etiquetas, datos, formularios,
  tablas y cualquier texto operativo.
- **Monoespaciada**: JetBrains Mono — solo para valores numéricos de deuda y fechas
  en contextos de tabla densa.
- Nunca mezclar más de estas tres fuentes.

### Escala tipográfica (base: 16px, ratio: Major Second 1.125)

| Token | Fuente | Tamaño | Peso | Uso |
|-------|--------|--------|------|-----|
| `--text-display-lg` | Lora | 30px | 600 | Título de página principal |
| `--text-heading-md` | Lora | 24px | 600 | Encabezados de sección |
| `--text-heading-sm` | Lora | 20px | 600 | Títulos de tarjeta y panel |
| `--text-body-md` | Inter | 16px | 400 | Texto de cuerpo por defecto |
| `--text-body-sm` | Inter | 14px | 400 | Texto secundario, captions |
| `--text-label-md` | Inter | 14px | 500 | Etiquetas de formulario, botones |
| `--text-label-sm` | Inter | 12px | 500 | Badges, tags, timestamps |
| `--text-code` | JetBrains Mono | 14px | 400 | Valores numéricos en tablas densas |

## Espaciado y layout

### Grilla base
- Unidad base: 4px.
- Sistema de navegación: sidebar izquierdo fijo + área de contenido principal.
- Ancho del sidebar: 240px expandido / 64px colapsado.
- Alto del navbar: no aplica (sin navbar global; el sidebar contiene toda la navegación).
- Ancho máximo del contenido: 1280px.

### Estructura de página (desktop)
```
[Sidebar 240px fijo] [Contenido principal — flex-grow, max 1280px]
```

### Comportamiento responsive
- El sistema es de uso exclusivo en escritorio (el personal administrativo trabaja
  en computadoras de escritorio o laptops).
- Breakpoint mínimo soportado: 1024px.
- No se requiere diseño mobile ni tablet; no implementar media queries por debajo de 1024px.
- Entre 1024px y 1280px: el sidebar puede colapsarse automáticamente para ganar espacio.

## Patrones de interacción

### Feedback de acciones
- Toda acción asíncrona debe mostrar estado de carga en menos de 100ms.
- Confirmaciones de éxito: toast en esquina inferior derecha, 4 segundos, auto-dismiss.
- Errores de validación de negocio (lector bloqueado, libro no disponible): mensaje
  inline dentro del formulario, nunca como toast. El mensaje debe explicar la causa
  y la acción correctiva.
- Errores de sistema (fallo de red, error de servidor): toast de error.

### Confirmaciones destructivas
- Registrar pago de multa requiere confirmación modal: es una acción irreversible.
- El botón de confirmar en el modal debe replicar exactamente la acción:
  "Confirmar pago" (no "Aceptar", no "Sí").
- El botón cancelar siempre dice "Cancelar".

### Estados de lector — señalización visual
- Lector habilitado: badge verde (`--color-feedback-success`) con texto "Habilitado".
- Lector con deuda pendiente: badge ámbar (`--color-feedback-warning`) con texto
  "Deuda pendiente".
- Nunca usar solo color para transmitir el estado — siempre acompañar con texto.

### Estados de préstamo — señalización visual
- Préstamo activo dentro del plazo: badge neutro con texto "En préstamo".
- Préstamo vencido sin devolución: badge rojo (`--color-feedback-error`) con texto "Vencido".
- Préstamo devuelto a tiempo: badge verde con texto "Devuelto".
- Préstamo devuelto con retraso: badge ámbar con texto "Devuelto con retraso".

### Formularios
- Plazo de devolución: selector visual de tres opciones (7 / 14 / 21 días) presentado
  como grupo de botones de selección única (radio group visual), nunca como dropdown.
- Documento del lector (cédula/DNI): campo de texto con validación en tiempo real
  tras perder el foco.
- Nunca mostrar error de validación en un campo que el usuario aún no ha tocado.

### Estados vacíos
- Toda tabla o lista debe tener un estado vacío diseñado.
- Anatomía: icono representativo del dominio (libro, lector) + título + texto de apoyo.
- Ejemplo para historial de préstamos vacío:
  Icono libro + "Sin préstamos registrados" + "Este libro aún no registra historial
  de préstamo."
- Nunca dejar un espacio en blanco sin diseño ante ausencia de datos.

### Carga de datos
- Usar skeleton screens para todas las cargas de contenido.
- Los skeletons replican la forma exacta del contenido que reemplazan.
- Nunca usar spinner como único indicador de carga para contenido con layout conocido.

## Patrones prohibidos
1. No usar terminología técnica en mensajes de UI (sin "Error 500", sin "null", sin "undefined").
2. No usar dropdowns para la selección de plazo de préstamo — usar radio group visual.
3. No usar `window.alert()` ni `window.confirm()` — siempre usar el componente Modal.
4. No dejar estados vacíos sin diseño.
5. No usar color como único diferenciador de estado — siempre acompañar con texto.
6. No implementar vistas mobile o tablet (fuera de alcance).
7. No usar más de tres fuentes tipográficas.
8. No usar valores hex directamente en componentes.

## Accesibilidad
- WCAG 2.1 AA es el estándar mínimo.
- Contraste mínimo: 4.5:1 para texto normal, 3:1 para texto grande.
- Todos los elementos interactivos deben tener estado de foco visible usando
  `--color-border-focus`.
- Todos los inputs deben tener `<label>` asociado vía `htmlFor` / `id`.
- Los mensajes de error deben referenciar el campo con `aria-describedby`.
- Los badges de estado deben ser legibles por lectores de pantalla (no solo por color).
- Tamaño mínimo de área táctil / clic: 40×40px.

## Iconografía
- Biblioteca: `lucide-react`, estilo outline.
- Tamaño por defecto: 20px. En contexto inline con texto: 16px.
- Color: hereda del texto contenedor, nunca hardcodeado.
- Iconos sugeridos por entidad:
  - Libro: `BookOpen`
  - Lector: `User`
  - Préstamo: `BookMarked`
  - Multa / Deuda: `AlertCircle`
  - Pago: `CheckCircle`
  - Historial: `Clock`
  - Vencido: `AlertTriangle`

## Animaciones y movimiento
El sistema es de uso intensivo y diario. Las animaciones deben ser mínimas y funcionales.

| Elemento | Animación | Duración | Reducción de movimiento |
|----------|-----------|----------|------------------------|
| Modal abrir/cerrar | fade + scale sutil | 150ms ease-out | ninguna |
| Toast entrada/salida | slide desde abajo + fade | 200ms ease-out | ninguna |
| Dropdown abrir | fade | 100ms ease-out | ninguna |
| Skeleton pulse | opacidad 0.5→1.0 | 1200ms ease-in-out ∞ | estático |
| Transición de página | ninguna (instantánea) | — | — |

Todas las animaciones deben estar dentro de `@media (prefers-reduced-motion: no-preference)`.

---

# Requerimientos Técnicos y de Arquitectura

## Stack tecnológico definido

| Capa | Tecnología | Versión |
|------|-----------|---------|
| Framework frontend | React con Vite | React 18+ / Vite 5+ |
| Lenguaje | JavaScript (JSX) | ES2022+ |
| Estilos | CSS Modules | — |
| Componentes UI base | Sin librería externa (design system propio) | — |
| Cliente HTTP | axios | 1.6+ |
| Routing | react-router-dom | 6+ |
| Validación recomendada | zod | 3+ |
| Iconos | lucide-react | latest |
| Fechas | date-fns | 3+ |
| Notificaciones | Componente Toast propio | — |
| Animaciones | CSS nativo | — |
| Testing | Vitest + Testing Library | Vitest 1+ |

## Restricciones técnicas

- No usar librerías de componentes UI externas (Material UI, Chakra, shadcn, Ant Design).
  Todo el sistema de componentes se construye desde cero siguiendo el design system definido.
- Los estilos se gestionan con CSS Modules (archivos `.module.css` co-ubicados con el componente).
  Los valores personalizados de diseño se definen como CSS custom properties (tokens) en un
  archivo central de variables y se referencian desde los módulos CSS.
- No usar `window.alert()`, `window.confirm()`, ni `<dialog>` nativo.
- Estrategia de dark mode: ninguna en el MVP. No implementar variables de dark mode.
- La URL del backend se consume desde `VITE_API_URL` (variable de entorno de Vite);
  nunca hardcodear URLs de API.
- El sistema corre exclusivamente en navegadores de escritorio modernos.
  No se requiere soporte para IE ni navegadores legacy.

## Estructura de carpetas esperada

```
src/
├── main.jsx                    # Punto de entrada React + ReactDOM
├── App.jsx                     # Root component — define rutas con react-router-dom
├── App.module.css              # Estilos del root component
├── index.css                   # Estilos globales y tokens CSS del design system
├── components/                 # Componentes reutilizables de UI
│   ├── [Component].jsx         # Componente React (PascalCase)
│   └── [Component].module.css  # Estilos CSS Module co-ubicados
├── hooks/                      # Custom hooks compartidos
│   └── use[Feature].js         # Un hook por dominio (useLoan, useDebt, etc.)
├── pages/                      # Páginas completas — una por ruta
│   ├── [Feature]Page.jsx       # Componente de página (PascalCase + sufijo Page)
│   └── [Feature]Page.module.css
├── services/                   # Capa de comunicación HTTP con el backend (axios)
│   └── [feature]Service.js     # Un servicio por dominio (loanService, debtService)
└── __tests__/                  # Tests unitarios (Vitest + Testing Library)
    ├── setup.js                # Setup global de tests
    ├── components/             # Tests de componentes
    │   └── [Component].test.jsx
    ├── hooks/                  # Tests de hooks
    │   └── use[Feature].[caso].test.js
    └── pages/                  # Tests de páginas
        └── [Feature]Page.test.jsx
```

## Convenciones de código

| Elemento | Convención |
|----------|-----------|
| Componentes | PascalCase `.jsx` con CSS Module co-ubicado `.module.css` |
| Hooks | camelCase con prefijo `use`, `.js` |
| Servicios | camelCase `.js` (un archivo por dominio) |
| Páginas | PascalCase con sufijo `Page` `.jsx` |
| Tests | mismo nombre + `.test.jsx` (o `.test.js` para hooks) |
| Estilos | CSS Modules — clases importadas como `styles` |

## Convenciones de dominio en el código

Los nombres de entidades y estados en el código deben seguir el vocabulario del PRD:

| Dominio | Nombre en código |
|---------|-----------------|
| Libro | `Book` |
| Lector | `Reader` |
| Préstamo | `Loan` |
| Multa | `Fine` |
| Estado préstamo activo | `ON_LOAN` |
| Estado préstamo devuelto | `RETURNED` |
| Plazo de préstamo | `loanPeriod` (valores: 7, 14, 21) |

## Tokens CSS — archivo y estrategia

- **Archivo**: `src/index.css` (bloque `:root {}` con todas las custom properties del design system)
- **Formato**: CSS custom properties (variables CSS nativas)
- **Importación**: `index.css` se importa en `main.jsx`. Los componentes acceden a los
  tokens a través de `var(--token-name)` en sus respectivos archivos `.module.css`.
- **Dark mode**: no implementar. Solo un bloque `:root {}` sin bloque `.dark {}`.
- Los tokens de color definidos en la sección UX/UI deben declararse exactamente
  con los nombres especificados en la tabla de paleta.