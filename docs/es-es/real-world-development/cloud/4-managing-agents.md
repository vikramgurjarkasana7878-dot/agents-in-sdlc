---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos últimos ejercicios has pedido a Copilot cloud agent que se encargara de tres tareas independientes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque cloud agent está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Hay muchas herramientas para gestionar las tareas que se asignan a cloud agent, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes que tienen pull requests (PR) abiertos. Puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas de cloud agent.
- dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de cambio de modo, parecía lógico incluir esta funcionalidad. Quieres dirigir el trabajo del agente para asegurarte de que añade el modo claro además del modo de alto contraste.

## Revisa las tareas de Copilot cloud agent

Veamos el estado actual de todas las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
    - obtener el código del repositorio.
    - crear el entorno de trabajo de Copilot.
    - configurar servidores MCP.
    - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que se muestren dependerán del estado del trabajo de Copilot y del enfoque que haya adoptado.

4. Fíjate también en el panel del pull request (PR), que aparece a la derecha. Desde ahí puedes ver el PR y los archivos modificados para seguir el trabajo.

## Dirige cloud agent

Ahora que has visto las tareas activas, pidamos a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Ya que estamos trabajando en un modo de alto contraste, vamos a añadir también un modo claro. Debe haber un conmutador para este modo, de forma que los usuarios puedan seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de la tarea de cloud agent en la página de agentes con el diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como ocurría antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas de GitHub Copilot cloud agent. Desde este centro de control puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir tareas en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas de cloud agent.
- redirigir una sesión en curso para solicitar más funcionalidades.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes pasar a [iterar sobre los pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
