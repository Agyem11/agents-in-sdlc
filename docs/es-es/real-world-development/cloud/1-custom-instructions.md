---
title: "Ejercicio 1 - Instrucciones personalizadas (Agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas;
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no ejecutarás un prompt «antes y después». El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Los archivos de instrucciones te permitirán asegurarte de que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Son una herramienta eficaz para orientar a Copilot y obtener sugerencias y fragmentos de código más útiles. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo y tareas concretas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener orientación general sobre el proyecto: información relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando, las prácticas recomendadas y la orientación global.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo concretos. Puedes utilizarlos para establecer directrices para lenguajes específicos, como TypeScript, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen metadatos `applyTo`.
>
> Copilot también admite archivos de instrucciones que siguen otros estándares, como AGENTS.md y CLAUDE.md, tal y como se explica en la [documentación sobre compatibilidad con archivos de instrucciones][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

La creación de archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, el proyecto de ejemplo muestra un enfoque representativo. En general:

- Mantén `.github/copilot-instructions.md` centrado en la orientación general del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas para tipos de archivo (pruebas unitarias, componentes de Astro o la capa de datos), o para tareas concretas.
- Escribe en lenguaje natural y presenta las directrices con claridad. Incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No existe una única forma correcta de crear archivos de instrucciones, igual que no existe una única forma correcta de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá observes que hay instrucciones para muchos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que utilices lo indicará cuando sea relevante.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón global relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas de TypeScript, por ejemplo, uno que coincida con `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos reutilizables más pequeños y señalar ejemplos que Copilot puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios a nivel de archivo.

## Añade un nuevo estándar para el repositorio

Este es el único cambio que realizarás en este ejercicio: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentarios a nivel de archivo. El agente en la nube aplicará esta regla al trabajar en las incidencias que le asignes más adelante.

Antes de editar el archivo, prepara una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero así también podrás combinar los cambios de forma ordenada:

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Vamos a documentar los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero aún no incluye reglas para la documentación en el código; por eso la función auxiliar generada no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentarios a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar orientación global, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, inserta y combina el cambio de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, cuando asignes una incidencia a Copilot, este creará una rama desde `main`. Por tanto, tus cambios de instrucciones deben estar en `main` para que el agente pueda utilizarlos.

1. Prepara los cambios y crea la confirmación:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Inserta la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios desde `custom-instructions-cloud` hacia `main` y combínala. Así, el agente en la nube podrá leer las instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. El flujo con una rama permite practicar el método que utilizarías en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones de este proyecto y has añadido un estándar general al archivo `.github/copilot-instructions.md`. Durante el resto de este itinerario, verás cómo el agente en la nube aplica ese estándar en las solicitudes de cambios que genere.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
