# Organización de componentes React

## Principio general

Cada componente React con una responsabilidad funcional independiente debe desarrollarse en su propio archivo `.tsx`.

La organización debe favorecer la mantenibilidad, reutilización, legibilidad y separación de responsabilidades.

No se deben concentrar múltiples componentes funcionales complejos dentro de un único archivo.

## Reglas obligatorias

1. Cada página o vista principal debe tener su propio archivo.
2. Cada componente reutilizable debe tener su propio archivo.
3. Los formularios, tablas especializadas, paneles, modales y controles funcionales independientes deben tener archivos propios.
4. Los componentes con estado o lógica propia significativa deben separarse.
5. Los componentes que contienen temporizadores, consultas independientes o efectos complejos deben separarse cuando constituyan funcionalidades independientes.
6. Los tipos e interfaces compartidos entre archivos deben declararse en módulos de tipos apropiados.
7. Los hooks personalizados deben extraerse cuando la lógica sea reutilizable o suficientemente compleja para justificar su aislamiento.
8. Los componentes exclusivos de una vista deben permanecer dentro del módulo al que pertenecen.

## Excepciones permitidas

Se pueden declarar varios componentes en un mismo archivo únicamente cuando:

- Sean componentes auxiliares pequeños.
- No tengan responsabilidad funcional independiente.
- No estén destinados a reutilizarse.
- Su separación dificulte la comprensión del código.
- No introduzcan una complejidad considerable en el componente principal.

No deben crearse archivos independientes para fragmentos triviales de JSX sin una justificación de mantenimiento.

## Organización de directorios

Respetar la estructura de carpetas existente.

Agrupar los componentes por módulo o funcionalidad y utilizar los directorios de componentes establecidos por el proyecto.

Evitar directorios globales para componentes utilizados en una sola funcionalidad.

No introducir estructuras de carpetas excesivamente profundas sin necesidad.

## Responsabilidad de los archivos

Cada archivo debe tener una responsabilidad principal claramente identificable.

Se debe evitar mezclar en el mismo archivo:

- Una vista principal con varios formularios independientes.
- Varios componentes funcionales complejos.
- Componentes de presentación con múltiples funcionalidades no relacionadas.
- Hooks reutilizables con componentes que no dependen exclusivamente de ellos.

La separación del código no debe introducir abstracciones artificiales ni duplicar funcionalidades.

## Nuevos desarrollos

Estas reglas deben aplicarse desde el momento en que se crean nuevos componentes.

No se debe esperar a que un archivo crezca demasiado para reorganizar componentes funcionalmente independientes.

## Modificaciones existentes

Cuando se modifique un archivo que contenga varios componentes:

- Evaluar si corresponde separar los componentes afectados.
- Mantener los cambios limitados al alcance solicitado.
- No realizar refactorizaciones masivas ajenas a la tarea sin una instrucción explícita.
- Preservar el comportamiento, las interfaces públicas y la lógica existente.

## Criterios de validación

Antes de finalizar un cambio:

- Verificar la correcta organización de componentes.
- Verificar imports y exports.
- Comprobar que no existan dependencias circulares nuevas.
- Ejecutar las verificaciones disponibles de TypeScript, lint y pruebas.
- Confirmar que la separación no modificó el comportamiento funcional ni visual.

La organización de componentes es un requisito de calidad del código, no una optimización automática de rendimiento.