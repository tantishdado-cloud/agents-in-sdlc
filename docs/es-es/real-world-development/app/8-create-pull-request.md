---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, autoriza Agent Merge y confirma que la PR se ha combinado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú mismo combinaste la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la PR y resuelve los impedimentos: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reorganiza la rama cuando es necesario. Después, la combina en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque se reinicie la aplicación y se desactiva cuando la PR se ha combinado.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa responsabilidad, pero sigue necesitando tu autorización explícita para editar código y combinarlo. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Cuando hayas creado y revisado el código, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable por sí solo no lo inicia.

Se inicia el proceso de Agent Merge, que:

- crea la solicitud de incorporación de cambios con un título y una descripción.
- si has iniciado la sesión desde una incidencia, la incluye en el cuerpo de la descripción.
- reorganiza la rama o gestiona posibles conflictos con la rama de destino.
- supervisa la integración continua para asegurar que se superen todas las comprobaciones.
- supervisa la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot, y realiza cambios para resolverlos.
- puede combinar la PR automáticamente cuando todo se haya completado correctamente.

Dejaremos que Agent Merge combine también la PR cuando se superen todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que hay una marca junto a **Merge pull request**.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el proceso y resuelve los impedimentos que se indiquen. Confirma que el estado de la PR es **Merged** antes de continuar; habilitar el permiso para combinar o tener un agente en ejecución no confirma que la combinación haya terminado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos impedimentos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la verificación y, ahora, el proceso de solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has inspeccionado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
