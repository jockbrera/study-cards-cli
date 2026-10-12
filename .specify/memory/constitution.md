<!--
Sync Impact Report
- Version change: unversioned template -> 1.0.0 (initial ratification)
- Modified principles: none (initial fill from template placeholders)
- Added sections:
  - Core Principles: I. Simplicidad Primero, II. Sin Sobreingeniería (YAGNI),
    III. UX de Terminal Clara, IV. Testing Fácil, V. Mantenibilidad
  - Restricciones Técnicas
  - Flujo de Desarrollo y Calidad
  - Governance (reglas de enmienda, versionado y revisión de cumplimiento)
- Removed sections: none
- Follow-up TODOs: none (all placeholders resolved; RATIFICATION_DATE set to the
  initial adoption date 2026-10-12)
- Notes: this report is scratch material for review and MUST be removed before
  the amended constitution is committed.
-->

# Study Cards CLI Constitution

## Core Principles

### I. Simplicidad Primero (NON-NEGOTIABLE)

- Cada feature MUST resolver el problema actual con la solución más simple que
  funcione; la complejidad MUST poder justificarse en una línea.
- Se MUST preferir las APIs nativas de Node.js y TypeScript antes de introducir
  una dependencia nueva; toda dependencia externa MUST justificarse en el PR.
- El código MUST caber en la cabeza de un desarrollador nuevo: módulos pequeños,
  funciones puras cuando sea posible, sin capas de abstracción que no aporten.

**Rationale**: una CLI de flashcards es un dominio pequeño; la simplicidad es lo
que mantiene el proyecto mantenible y testable a largo plazo.

### II. Sin Sobreingeniería (YAGNI)

- NO se MUST construir funcionalidad, abstracciones, configuraciones ni puntos de
  extensión hasta que exista un requisito real y demostrable.
- NO se MUST introducir frameworks de inyección de dependencias, sistemas de
  plugins, arquitecturas en capas múltiples ni patrones diseñados "por si acaso".
- Preferir código directo y específico hoy; refactorizar cuando el cambio real lo
  exija, no antes.

**Rationale**: evitar sobreingeniería es un objetivo explícito del proyecto; cada
abstracción prematura cuesta mantenimiento y testing sin beneficiario.

### III. Experiencia de Usuario de Terminal Clara

- Toda salida MUST ser legible y concisa: mensajes cortos, sin ruido, con formato
  consistente para éxitos, advertencias y errores.
- Los errores MUST ir a stderr, MUST decir qué falló y qué hacer a continuación,
  y MUST usar códigos de salida (exit codes) consistentes y documentados.
- La CLI MUST ofrecer `--help` completo con ejemplos; los nombres de comandos y
  opciones MUST ser predecibles y estables (no romper sin avisar).
- El output MUST funcionar sin color (respetar `NO_COLOR` y la no-TTY); el color
  es decoración opcional, nunca el único canal de información.

**Rationale**: la terminal es toda la interfaz del usuario; un mensaje ambiguo o
un exit code inconsistente es un bug de funcionalidad, no de estética.

### IV. Testing Fácil (NON-NEGOTIABLE)

- El núcleo de la lógica (parseo de Markdown, lógica de repaso/SM-2 si existe,
  selección de cartas) MUST ser puro y estar separado de la I/O para poder
  testearlo sin tocar el filesystem ni la terminal.
- Se MUST cubrir con tests: el parser de flashcards en Markdown, los comandos de
  CLI a nivel de integración (invocar el binario y verificar stdout/stderr/exit
  code) y cualquier regla de negocio nueva.
- Los tests MUST ser rápidos, deterministas y sin dependencia de red; `npm test`
  MUST pasar de forma local y en CI antes de cualquier merge.

**Rationale**: la facilidad de testing es un requisito del usuario; si una función
no se puede testear sin mocks pesados, su diseño está mal y debe reestructurarse.

### V. Mantenibilidad

- TypeScript MUST usarse en modo `strict` sin `any` implícito; el typecheck MUST
  pasar sin errores en CI.
- El código MUST seguir una estructura de módulos simple y una convención única
  de estilo (formatter/linter automáticos, sin debate manual de formato).
- Los datos de estudio MUST vivir en archivos Markdown legibles por humanos: el
  Markdown es la fuente única de verdad, no un formato interno opaco.
- El README MUST mantenerse sincronizado con los comandos y el formato de
  flashcards realmente soportados.

**Rationale**: el proyecto lo mantendrán una o pocas personas; mantener baja la
carga cognitiva es lo que permite evolucionarlo sin miedo.

## Restricciones Técnicas

- Runtime: Node.js LTS; lenguaje: TypeScript con `strict` habilitado.
- Persistencia: únicamente archivos Markdown en el filesystem; NO se MUST
  introducir bases de datos, servicios externos ni formateos binarios propietarios.
- Dependencias: mínimo necesario; cada dependencia nueva MUST justificarse en su
  PR (costo de mantenimiento > beneficio ⇒ se rechaza).
- Sin requisitos de red en tiempo de ejecución; la aplicación MUST funcionar 100%
  offline.
- El formato de flashcard en Markdown MUST ser simple, documentado y estable; los
  cambios en el formato MUST incluir migración o compatibilidad declarada.

## Flujo de Desarrollo y Calidad

- Todo cambio MUST pasar, antes de merge: `typecheck` → `lint/format` → `tests`.
- Los tests se escriben junto con la feature en el mismo PR; los tests nuevos o
  cambiados MUST reflejar primero el comportamiento deseado.
- Cada PR MUST incluir la actualización de documentación afectada (README,
  `--help`, ejemplos).
- Las revisiones MUST verificar el cumplimiento de esta constitución; si un PR
  necesita violar un principio, primero se enmienda la constitución o se descarta
  el PR.
- Se MUST mantener los comandos CLI pequeños: la capa CLI solo orquesta I/O y
  delega la lógica en módulos testeables.

## Governance

1. La constitución tiene prioridad sobre cualquier práctica, convención o
   preferencia individual del proyecto.
2. **Procedimiento de enmienda**: toda modificación se realiza por PR sobre
   `.specify/memory/constitution.md`, describiendo el motivo, el impacto y el
   plan de migración si afecta a código existente; requiere aprobación del
   mantenedor antes de merge.
3. **Política de versionado**: se aplica semver a la constitución — MAJOR para
   eliminaciones o redefiniciones incompatibles de principios, MINOR para
   principios o secciones nuevas o expansión material de guía, PATCH para
   clarificaciones y correcciones de wording. La línea de versión al pie MUST
   actualizarse en la misma PR.
4. **Revisión de cumplimiento**: cada PR y cada revisión MUST comprobar
   explícitamente el cumplimiento de los principios; la complejidad añadida MUST
   justificarse. Los incumplimientos se corrigen en el mismo PR o se registran
   como TODO con responsable.
5. Los placeholders `TODO(...)` pendientes se rastrean en el Sync Impact Report
   y MUST resolverse antes de dar una sección por terminada.

**Version**: 1.0.0 | **Ratified**: 2026-10-12 | **Last Amended**: 2026-10-12
