---
title: "Lección 8 - Crea y fusiona el PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea un PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos en conjunto y abrir un pull request. Tú mismo fusionaste el pull request (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la funcionalidad, las actualizaciones de las instrucciones y la habilidad y el agente de QA en varios PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y lo incluirás en un único PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- inspeccionarás todos los cambios de la funcionalidad y las evidencias de verificación.
- crearás el PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que se ha fusionado el PR.

## Escenario

A lo largo del flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes del PR, sin perder el control del desarrollador sobre la autorización de fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo que falta para integrar un pull request. Cuando lo activas, Copilot resuelve los bloqueos del PR —corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta— y, después, activa la fusión automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede hacerse cargo de esa responsabilidad. Revisa el trabajo y decide si está listo antes de activarlo.

## Usa Agent Merge para gestionar el PR

Con todo el código creado, revisémoslo en conjunto, creemos el PR y dejemos que Agent Merge se encargue del resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo termine correctamente, realizará la fusión.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando la configuración del repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora la gestión del pull request. En concreto, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- inspeccionado todos los cambios de la funcionalidad y las evidencias de verificación.
- creado el PR de filtrado.
- activado Agent Merge solo después de la revisión y confirmado que se ha fusionado el PR.

A continuación, [explorarás más comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestión de pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
