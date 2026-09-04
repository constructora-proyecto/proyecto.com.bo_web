# Flujo de edición, previsualización y publicación

Estas instrucciones se aplican a cualquier agente de IA que trabaje en este repositorio.

## Datos del sitio

- Rama de producción: `main`.
- Sitio de producción: `https://proyecto.com.bo`.
- Las ramas distintas de `main` son temporales y se usan para previsualización.
- Una rama temporal puede mantenerse únicamente en el entorno local mientras el usuario revisa los cambios; no es obligatorio publicarla para una preview local.
- La publicación de producción se realiza exclusivamente mediante GitHub Pages desde `main`.
- Repositorio de previews públicas: `constructora-proyecto/proyecto-com-bo-previews`.
- Sitio base de previews: `https://constructora-proyecto.github.io/proyecto-com-bo-previews/`.
- GitHub Pages no proporciona una URL independiente por rama o commit en este repositorio. Para una preview pública, copiar únicamente los archivos necesarios al repositorio de previews, dentro de un subdirectorio con el nombre normalizado de la rama temporal.

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
5. Mantener la rama sin publicar mientras el usuario revisa la preview local. Publicar la rama en GitHub o crear una preview pública únicamente cuando el usuario lo solicite; subir una rama no genera por sí mismo una preview de GitHub Pages.

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
   - todavía no se publicaron en GitHub Pages ni `proyecto.com.bo`;
   - la preview dejará de estar disponible cuando se detenga el servidor local;
   - hace falta aprobación explícita para continuar con la publicación.
6. Mantener el servidor activo mientras el usuario revisa la página, salvo que continuar ejecutándolo impida completar otra acción solicitada.

No afirmar que `localhost` es accesible para el usuario cuando el servidor se ejecuta dentro de un agente cloud, contenedor remoto o equipo diferente. Si el entorno ofrece una función segura para exponer o abrir su servidor local, usarla y verificarla. Si no la ofrece, explicar la limitación y proporcionar capturas verificadas o las instrucciones para ejecutar la preview local, sin publicar los cambios en producción como sustituto.

## Publicar la rama temporal en GitHub

Publicar la rama temporal únicamente cuando el usuario lo solicite o cuando sea necesario preparar el pull request posterior a su aprobación:

1. Crear un commit descriptivo que contenga únicamente los cambios revisados.
2. Publicar solamente la rama temporal; nunca empujar esos cambios directamente a `main`.
3. Informar el nombre de la rama y el commit publicado.
4. Explicar que la rama en GitHub conserva el código para revisión, pero no tiene una URL de GitHub Pages propia y no modifica `proyecto.com.bo`.
5. No configurar ningún proveedor de despliegue adicional sin una solicitud explícita del usuario.

## Publicar una previsualización pública en GitHub Pages

Usar esta modalidad únicamente cuando el usuario solicite una URL pública:

1. Confirmar que la versión que se copiará corresponde exactamente al commit de la rama temporal que se desea revisar.
2. Clonar o actualizar `constructora-proyecto/proyecto-com-bo-previews` y comprobar que su rama de publicación es `main`.
3. Crear o actualizar un subdirectorio cuyo nombre coincida con el nombre normalizado de la rama temporal. Por ejemplo, `borrador-enlaces-instagram/`.
4. Copiar únicamente los archivos necesarios para ejecutar el sitio. No copiar `CNAME`, `.git`, `AGENTS.md`, secretos ni configuración exclusiva de producción.
5. Actualizar el índice del repositorio de previews cuando corresponda, ejecutar `git diff --check`, revisar el diff y publicar el cambio en la rama `main` del repositorio de previews.
6. Esperar a que el despliegue de GitHub Pages termine correctamente. La URL tendrá la forma `https://constructora-proyecto.github.io/proyecto-com-bo-previews/<subdirectorio>/`.
7. Verificar la URL pública en escritorio y móvil y comprobar específicamente el cambio solicitado; una respuesta HTTP exitosa no basta.
8. Informar la rama y commit del sitio original, el commit del repositorio de previews y la URL verificada. Aclarar que la preview no modifica `proyecto.com.bo` y que todavía requiere aprobación explícita para producción.

Si el nombre o la configuración del repositorio de previews cambia, obtener los datos actuales desde GitHub y actualizar estas instrucciones; no inventar URLs. Cuando una preview deje de ser necesaria, retirarla solo con autorización explícita del usuario.

## Solicitar aprobación

Al presentar una preview, indicar la rama, el commit revisado, si es local o pública y su dirección verificada. Pedir al usuario que revise el resultado y que confirme explícitamente si desea aprobarlo y publicarlo en producción.

Si el usuario pide ajustes, hacerlos en la misma rama temporal y mostrar nuevamente la preview actualizada por el mismo medio. La aprobación de una versión anterior no autoriza a publicar cambios posteriores que el usuario aún no haya visto.

## Aprobar y publicar en producción

Solo después de una aprobación explícita del usuario:

1. Confirmar que el commit aprobado sigue siendo la cabeza de la rama temporal y que no aparecieron cambios nuevos.
2. Actualizar las referencias remotas y comprobar que `main` no avanzó de una manera que produzca conflictos o invalide la preview.
3. Si la rama solo existía localmente, crear un commit descriptivo, publicarla y crear el pull request necesario. Verificar que el commit publicado es exactamente el que el usuario revisó localmente.
4. Fusionar la rama temporal en `main`, preferiblemente mediante el pull request y los controles configurados en GitHub. No usar force push ni omitir protecciones de rama.
5. Esperar a que el despliegue de GitHub Pages termine correctamente.
6. Verificar `https://proyecto.com.bo` en el navegador y comprobar que contiene los cambios aprobados.
7. Si el entorno dispone de control de navegador, abrir `https://proyecto.com.bo` en una pestaña o ventana nueva. En caso contrario, entregar el enlace al usuario.
8. Informar el commit fusionado, el resultado del despliegue y la verificación visual realizada.

No declarar completada la publicación si el merge fue exitoso pero la página pública todavía no refleja los cambios. Si el despliegue falla o permanece pendiente, mantener informado al usuario y no afirmar que producción está actualizada.

## Bloqueos

Si no es posible mostrar `localhost`, GitHub Pages no despliega correctamente o el agente no tiene permisos para publicar o fusionar, detenerse después de conservar los cambios de forma segura. Explicar exactamente qué capacidad, configuración o permiso falta y no sustituir la preview por un cambio directo en producción.
