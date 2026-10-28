---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información adicional, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices para generar código.

En este ejercicio:

- explorarás cómo el contexto del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos aplicables a rutas concretas;
- generarás la primera parte de los datos del filtrado (una función auxiliar para editoriales) con las instrucciones actuales;
- añadirás un estándar general al archivo `.github/copilot-instructions.md`;
- volverás a enviar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar;
- confirmarás las instrucciones y el trabajo inicial del filtrado en `main` para que Copilot pueda utilizar la orientación actualizada en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunas de las normas establecidas, porque Copilot no es determinista. El objetivo es observar la *tendencia* de los cambios en el comportamiento después de actualizar las instrucciones, no obtener un resultado idéntico carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Los archivos de instrucciones te permitirán asegurarte de que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo y tareas concretas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando y las prácticas recomendadas.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretos, como TypeScript, la creación de componentes de interfaz de usuario o las pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen metadatos `applyTo`.
>
> Copilot también admite archivos de instrucciones que siguen otros estándares, como AGENTS.md y CLAUDE.md, tal y como se explica en la [documentación sobre compatibilidad con archivos de instrucciones][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque posible. En general:

- Mantén `copilot-instructions.md` centrado en la orientación general del proyecto, como la estructura y los estándares globales de programación.
- Usa `*.instructions.md` para proporcionar directrices específicas para tipos de archivo o tareas.
- Escribe en lenguaje natural, presenta las directrices con claridad e incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No existe una única forma correcta de crear archivos de instrucciones o utilizar IA; la experimentación te ayudará a encontrar el enfoque adecuado.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto hay instrucciones para muchos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI).
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Lee los archivos de instrucciones incluidos en el repositorio: hay un archivo principal `copilot-instructions.md` y archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Observa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las indicaciones anidadas sobre **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora `.github/instructions`; encontrarás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`. Define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones; en este caso, coincide con archivos de pruebas TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa sus enlaces a otros archivos de instrucciones y a archivos existentes. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para comentarios TSDoc y encabezados de comentarios a nivel de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea relevante.

Primero envía un prompt con las instrucciones actuales. Más adelante actualizarás el archivo y volverás a enviar el mismo prompt para comparar los resultados.

1. Cierra las pestañas de editor de los ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar la función auxiliar.
3. Selecciona **Agent** en la lista de agentes de Chat para permitir que Copilot aplique cambios.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto al archivo de la función auxiliar como a sus pruebas.
6. Observa que la función auxiliar propuesta tiene tipos, recibe el cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Estas características proceden de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, aplicable a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot ofrece resultados probabilísticos, es posible que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un nuevo estándar al repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añadamos un estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Aún no incluye reglas para la documentación dentro del código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade estas líneas de Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones generales en `.github/copilot-instructions.md` o instrucciones para lenguajes, tipos de archivo y tareas concretos en archivos `*.instructions.md`. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información sobre editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan como cualquier otro archivo del repositorio. Vamos a confirmar los cambios e insertar la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están los cambios de instrucciones y del asistente:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Inserta la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de cambios

Una vez insertada la rama, crea una solicitud de cambios y vincúlala a la incidencia sobre los estándares de documentación de la lista de tareas. Copilot puede buscar la incidencia, crear la solicitud para cerrarla y, después, combinarla mediante las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realizará acciones de GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub. La primera vez que Copilot utilice una, es posible que VS Code te pida iniciar sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y cree una solicitud desde la rama actual. Indícale que describa tanto las actualizaciones como el asistente de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la solicitud de cambios.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la solicitud, pídele que la combine en `main` y vuelva a actualizar la rama local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot solicita permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general al archivo `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el nuevo estándar;
- confirmar las instrucciones y el trabajo inicial de la función auxiliar en `main` para que el siguiente ejercicio pueda continuar.

A continuación, aplicarás las instrucciones en el [modo agente][next-lesson] mientras Copilot añade una característica en distintos componentes.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
