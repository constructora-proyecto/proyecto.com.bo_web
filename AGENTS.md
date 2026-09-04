# Flujo de edición, previsualización y publicación

Estas instrucciones se aplican a cualquier agente de IA que trabaje en este repositorio.

## Datos del sitio

- Rama de producción: `main`.
- Sitio de producción: `https://proyecto.com.bo`.
- Las ramas distintas de `main` son temporales y se usan para previsualización.
- Una rama temporal puede mantenerse únicamente en el entorno local mientras el usuario revisa los cambios; no es obligatorio publicarla para una preview local.
- Las previsualizaciones se publican mediante Cloudflare Pages. El nombre real del proyecto de Pages debe obtenerse de la configuración o del resultado del despliegue; nunca debe inventarse.

## Regla principal

Nunca fusionar, empujar directamente ni publicar cambios en `main` sin que el usuario haya aprobado explícitamente la previsualización actual. Una petición de editar, corregir, probar, mostrar o publicar una preview no equivale a aprobar producción.

## Preparar cambios

1. Antes de editar, actualizar las referencias remotas y comprobar la rama y el estado del repositorio.
2. No mezclar ni sobrescribir cambios existentes que no pertenezcan a la tarea.
3. Crear o reutilizar una rama temporal distinta de `main`, con un nombre breve y descriptivo. Ejemplo: `prueba20260904`.
4. Realizar los cambios solicitados y validarlos en proporción a su alcance. Como mínimo:
   - revisar el diff;
   - ejecutar `git diff --check`;
   - comprobar visualmente la página en escritorio y móvil cuando cambie su presentación;
   - conservar `CNAME` y cualquier configuración de publicación, salvo que el usuario solicite expresamente modificarlos.
5. Antes de publicar la rama, determinar si el usuario puede revisar los cambios mediante una preview local. Si puede hacerlo, mantener la rama sin publicar hasta que la revisión requiera una URL pública o el usuario apruebe continuar.

## Mostrar una previsualización local sin publicar la rama

Cuando el agente comparte el equipo o una interfaz de navegador accesible para el usuario, ofrecer primero una preview local que no haga `push` ni publique la rama:

1. Mantener todos los cambios en la rama temporal local. No fusionarlos con `main`.
2. Iniciar un servidor estático desde la raíz del repositorio, por ejemplo:

   `python3 -m http.server 8000`

   Si el puerto `8000` está ocupado, escoger otro puerto libre y comunicarlo.
3. Verificar que la página carga desde `http://localhost:<puerto>` y comprobar visualmente los cambios solicitados en escritorio y móvil cuando corresponda.
4. Si el entorno dispone de control de navegador, abrir la dirección local en una pestaña o ventana nueva para el usuario. En caso contrario, entregar la dirección local como enlace y explicar cómo abrirla.
5. Informar claramente que:
   - los cambios solo existen en el equipo y en la rama temporal local;
   - todavía no se publicaron en GitHub, Cloudflare Pages ni `proyecto.com.bo`;
   - la preview dejará de estar disponible cuando se detenga el servidor local;
   - hace falta aprobación explícita para continuar con la publicación.
6. Mantener el servidor activo mientras el usuario revisa la página, salvo que continuar ejecutándolo impida completar otra acción solicitada.

No afirmar que `localhost` es accesible para el usuario cuando el servidor se ejecuta dentro de un agente cloud, contenedor remoto o equipo diferente. En ese caso, usar una URL de preview que el entorno exponga de forma segura o seguir el flujo de Cloudflare Pages descrito a continuación.

## Publicar y mostrar la previsualización

Usar esta modalidad cuando el usuario necesite abrir la preview desde otro equipo, cuando el agente trabaje completamente en la nube o cuando el usuario solicite una URL pública. Crear un commit descriptivo y publicar solamente la rama temporal en el remoto. Después de subirla:

1. Esperar a que el despliegue de Cloudflare Pages termine correctamente. No considerar publicada la preview únicamente porque `git push` haya finalizado.
2. Obtener la URL real desde el check de despliegue, el pull request o Cloudflare Pages.
3. La dirección estable de una rama normalmente sigue este formato:

   `https://<rama-normalizada>.<proyecto-pages>.pages.dev`

   Por ejemplo, si la rama es `prueba20260904` y el proyecto confirmado de Pages es `proyecto-com-bo-web`, la URL sería:

   `https://prueba20260904.proyecto-com-bo-web.pages.dev`

4. No presentar el ejemplo anterior como una URL real hasta haber confirmado el nombre del proyecto y que el despliegue existe.
5. Verificar que la URL pública carga y que muestra específicamente los cambios solicitados; un estado HTTP exitoso por sí solo no es suficiente.
6. Si el entorno dispone de control de navegador, abrir la preview en una pestaña o ventana nueva para el usuario. Si no dispone de él, entregar un enlace Markdown claramente identificado.
7. Explicar al usuario, de forma explícita:
   - que los cambios están únicamente en una rama temporal;
   - que la preview no modifica `proyecto.com.bo`;
   - que hace falta su aprobación explícita para fusionar a `main`.

## Solicitar aprobación

Al presentar una preview pública, indicar el nombre de la rama, el commit probado y la URL verificada. Para una preview local, indicar la rama local, el estado sin publicar y la dirección `localhost`. Pedir al usuario que revise el resultado y que confirme explícitamente si desea aprobarlo y publicarlo en producción.

Si el usuario pide ajustes, hacerlos en la misma rama temporal, volver a desplegar y mostrar la preview actualizada. La aprobación de una versión anterior no autoriza a publicar cambios posteriores que el usuario aún no haya visto.

## Aprobar y publicar en producción

Solo después de una aprobación explícita del usuario:

1. Confirmar que el commit aprobado sigue siendo la cabeza de la rama temporal y que no aparecieron cambios nuevos.
2. Actualizar las referencias remotas y comprobar que `main` no avanzó de una manera que produzca conflictos o invalide la preview.
3. Si la rama solo existía localmente, crear un commit descriptivo, publicarla y crear el pull request necesario. Verificar que el commit publicado es exactamente el que el usuario revisó localmente.
4. Fusionar la rama temporal en `main`, preferiblemente mediante el pull request y los controles configurados en GitHub. No usar force push ni omitir protecciones de rama.
5. Esperar a que el despliegue de producción termine correctamente.
6. Verificar `https://proyecto.com.bo` en el navegador y comprobar que contiene los cambios aprobados.
7. Si el entorno dispone de control de navegador, abrir `https://proyecto.com.bo` en una pestaña o ventana nueva. En caso contrario, entregar el enlace al usuario.
8. Informar el commit fusionado, el resultado del despliegue y la verificación visual realizada.

No declarar completada la publicación si el merge fue exitoso pero la página pública todavía no refleja los cambios. Si el despliegue falla o permanece pendiente, mantener informado al usuario y no afirmar que producción está actualizada.

## Bloqueos

Si Cloudflare Pages no está conectado al repositorio, la rama no genera una preview o el agente no tiene permisos para publicar o fusionar, detenerse después de conservar los cambios de forma segura. Explicar exactamente qué configuración o permiso falta y no sustituir la preview por un cambio directo en producción.
