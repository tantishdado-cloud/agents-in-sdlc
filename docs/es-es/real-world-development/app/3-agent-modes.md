---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Hemos empezado por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios mayores necesitan un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de una organización y ayudar a desarrollar lo adecuado de la manera correcta. En esta lección y en las siguientes seguirás un proceso habitual de desarrollo dirigido por agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, finalmente, la combinarás con el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de la funcionalidad, utilizarás la misma sesión. Normalmente se usarían sesiones o PR diferentes para los distintos tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente nueva a partir de una incidencia de GitHub.
- definirás los requisitos en el modo **Plan**.
- implementarás la funcionalidad con el modo **Autopilot**.
- revisarás el código.
- comprobarás manualmente la funcionalidad en un lienzo del navegador.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás la validación MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys sigue creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles antes de programar, como la combinación de categorías. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso similar a este:

1. Abrir una incidencia registrada que describa el trabajo necesario.
2. Crear un plan con lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinar el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de este enfoque.

Al seguir este proceso estándar, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de evaluación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera a que le indiques cómo continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que ejecute el trabajo.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar instrucciones.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Confirma que se ha combinado la PR de valoraciones por estrellas y que la rama local `main` está actualizada antes de empezar.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, selecciona **Issues** en la navegación superior. Abre **Allow users to filter games by category and publisher**. Es una incidencia inicial ya existente; no tienes que crear otra.
2. Selecciona **New session** y crea un **nuevo árbol de trabajo** basado en `main` actualizada.

   ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha hacia el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está vinculada a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, revisa la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que hayan fallado. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has empezado por la incidencia, Copilot ya tiene la solicitud de funcionalidad en el contexto. Envía este prompt:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento dudoso, como la combinación de varias categorías o qué debe pasar si no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobaciones de tipos mediante las herramientas existentes del proyecto. Limítalo a implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide los cambios necesarios antes de aprobar el plan y guarda el enlace de la incidencia y las aclaraciones acordadas para validarlos más adelante.

## Aprueba explícitamente Autopilot

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente en tu versión. Comprueba el modo activo en lugar de dar por hecho que la aprobación lo cambia. Confirma que el indicador de modo muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Seguirá el plan, generará código y ejecutará pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar de inmediato; revísalo antes de aprobarlo. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones. Detén solo los servidores que hayas iniciado tú.

## Revisa y verifica la implementación

Una vez generado el código, revísalo antes de combinarlo, como harías con cualquier otro código. Revisa los cambios y ejecuta el sitio para comprobar que todo funciona correctamente.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones del repositorio.
3. Revisa el resultado de lint, las pruebas unitarias, las pruebas E2E y las comprobaciones de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; confirma que el servidor probado pertenece a este árbol de trabajo y no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo del navegador.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. En unos momentos, se iniciará la aplicación y aparecerá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que se muestran sus juegos.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando hayas terminado, pide a Copilot que detenga el servidor de desarrollo de la sesión:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Conserva esta sesión para los siguientes ejercicios

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, este árbol de trabajo y esta rama para las Lecciones 4 a 8. En el siguiente ejercicio aplicarás instrucciones personalizadas al código de filtrado y, después, añadirás mejoras de habilidades y QA antes de crear y combinar la PR conjunta en la Lección 8. Puedes dejar la sesión inactiva después de detener su servidor de desarrollo y retomarla en la siguiente lección.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para crear y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente nueva a partir de una incidencia de GitHub.
- has definido los requisitos en el modo **Plan**.
- has implementado la funcionalidad con el modo **Autopilot**.
- has revisado el código.
- has comprobado manualmente la funcionalidad en un lienzo del navegador.

A continuación, profundizarás en la generación de código y en cómo asegurar que sigue las prácticas documentadas mediante el uso de [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
