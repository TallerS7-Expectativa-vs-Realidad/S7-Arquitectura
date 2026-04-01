# Normativas
## Naming Branchs
```
main      -> producción
develop   -> integración
feature/* -> nuevas funcionalidades
release/* -> Preparar nueva versión
hotfix/*  -> Corregir bug urgente en producción
```

## Branch Protection
### main
#### Reglas activas:
**Require a pull request before merging** \
Nadie puede hacer push directo a la rama main, todo pasa por PR.

**Require approvals** \
Se define cuantas personas deben aprobar el PR. En este caso 1 ya que el equipo es de 2 personas.

**Dismiss stale pull request approvals when new commits are pushed** \
Si cambia el código, se borran las aprobaciones previas para que no se aprueven pull request con cambios realizados luego de ser aprovado

**Require status checks to pass before merging** \
El código debe pasar los checks de automatización para asegurar que el código funciona antes de mergear (por ejemplo, tests, builds, linters, etc)

**Require branches to be up to date before merging** \
El PR debe estar actualizado con la rama base. Si se realizó un merge a main y el PR no tiene estos cambios, se obliga a realizar un merge a la rama antes de realizar el pull request a main.

**Require conversation resolution before merging** \
Todos los comentarios del PR deben resolverse antes de poder haceptar el pull request

#### Reglas inactivas:
**Allow force pushes** \
Desactivado para impedir reescribir el historial

**Allow deletions** \
Desactivado para impedir borrar la rama

### develop
**Require a pull request before merging** \
Nadie puede hacer push directo a la rama develop, todo pasa por PR.

**Require approvals** \
Se define cuantas personas deben aprobar el PR. En este caso 1 ya que el equipo es de 2 personas.

**Dismiss stale pull request approvals when new commits are pushed** \
Si cambia el código, se borran las aprobaciones previas para que no se aprueven pull request con cambios realizados luego de ser aprovado

**Require status checks to pass before merging** \
El código debe pasar los checks de automatización para asegurar que el código funciona antes de mergear (por ejemplo, tests, builds, linters, etc)

**Require branches to be up to date before merging** \
El PR debe estar actualizado con la rama base. Si se realizó un merge a main y el PR no tiene estos cambios, se obliga a realizar un merge a la rama antes de realizar el pull request a main.

## Conventional Commits
> **feat:** \
Para nuevas funcionalidades

>**fix:** \
Para corrección de bugs

>**docs:** \
Para la documentación

>**style:** \
Para trabajo de formato y estilos visuales

>**refactor:** \
Para tareas de refactorización

>**test:** \
Para el trabajo de agregar o modificar tests

>**chore:** \
Para tareas de mantenimiento en configuraciones, estructuras y dependencias

>**ci:** \
Para tareas relacionadas a CI/CD. Como por ejemplo pipelines CI

>**build:** \
Para tareas de construcción del proyecto. Configuración de docker, maven, gradle, etc.

Si el cambio realizado es en un único archivo, entonces el commit tendrá la siguiente estructura:
```
type(scope): description
```
Por ejemplo:
```
feat(auth): add login validation
fix(api): handle null response
docs(readme): update installation guide
```

En caso de que afecte a varios documentos, entonces se utiliza el scope genérico:
```
docs: update installation guide in readme and architecture docs
```

### Plantillas de commits
- feat(HU-XX): implementación básica necesaria para la HU-XX
- feat(HU-XX): implementación de test unitarios de HU-XX
- feat(HU-XX): implementación de test de integración de componentes de HU-XX


## Reglas de Pull Requests
### Conventional Pull Request
Se sigue una estructura similar a los Conventional commites:

En el caso de un **feature/\*** -> **develop** \
`feat(*feature*): *descripción del feature realizado*` \
\*feature\*: se reemplaza por el feature realizado  \
Se debe escribir una descripción brebe del feature implementado. Descripción detallada va en la descripción del PR.

En el caso de un **develop** -> **main** \
`release: v1.2.0`

En el caso de un **hotfix** -> **main** \
`fix: *descripción del fix realizado*` \
Se debe escribir una descripción brebe del fix realizado. Descripción detallada va en la descripción del PR.

El PR debe responder a la pregunta de ¿Qué cambio introduce en el sistema?. No debe responder a la pregunta de ¿Qué se hizo?


# Flujo de trabajo
## Git Workflow
Se deben utilizar las ramas en función de la utilidad que cumplen.
Siempre que se cree una rama nueva, se debe hacer desde la rama develop

Ejemplo de flujo:
```
git checkout develop
git pull
git checkout -b feature/login-form
```

Se debe intentar que los temas trabajados en las ramas sea atómico, se usa la rama para el tema de la rama.

### Reglas importantes
- No hacer push directo a main
- No hacer push directo a develop
- Todo cambio debe pasar por Pull Request
- Todo PR debe pasar tests y linters
- Todo PR debe ser aprobado por el QA

## Cambiar commit name en caso de error
### Cambiar el último commit
```bash
git commit --amend -m "docs: update installation guide in readme and architecture docs"
```
y se vuelve a hacer un push:
```bash
git push --force-with-lease
```
>_⚠️ Se usa `--force-with-lease` porque el commit cambió de hash._

### Cambiar un commit previo
Si el commit incorrecto **no es el último**, entonces se usa:
```bash
git rebase -i HEAD~n
```
Ejemplo:
```bash
git rebase -i HEAD~3
```

Se abre una lista como:
```
pick a1b2c3 fix bug in login
pick d4e5f6 update docs
pick g7h8i9 add tests
```

Cambias `pick` por `reword` o `r` en el commit que quieres editar:
```
pick a1b2c3 fix bug in login
reword d4e5f6 update docs
pick g7h8i9 add tests
```

Git abrirá el editor para cambiar el mensaje.
```bash
git push --force-with-lease
```
#### ejemplo
1. Commit incorrecto:
```bash
git commit -m "update docs"
```

2. CI falla ❌

3. Se corrige
```bash
git commit --amend -m "docs: update installation guide"
```

4. se vuelve a subir
```bash
git push --force-with-lease
```

5. CI corre de nuevo ✅

## Trabajo con Issues
- Toda Issue debe ser asignada por el desarrollador asignado antes de ser trabajada.
- Toda Issue debe tener vinculada la rama en la que se trabajó
- Toda Issue debe tener el commit final que dió por finalizado el Issue

## Uso de ASDD
### construcción de los specs
***descripción:*** genera la implementación de las especificaciones descriptas en el spec indicado con `<feature-spec>` en el servicio backend 
```
/implement-backend <feature-spec>
```

### implementación de funcionalidades
***descripción:*** genera la implementación de las especificaciones descriptas en el spec indicado con `<feature-spec>` en el servicio backend 
```
/implement-backend <feature-spec>
```

***descripción:*** genera la implementación de las especificaciones descriptas en el spec indicado con `<feature-spec>` en el servicio frontend 
```
/implement-frontend <feature-spec>
```

### implementación de tests
***Descripción:*** implementa los test unitarios en backend y frontend; e implementa los test de integración de componentes en backend, utilizando los documentos generados por el QA (`TEST_PLAN` y `TEST_CASES`) como base de información para conocer los datos de ejemplo a testear.
Se le pasa como parámetro el spec a trabajar `<feature-spec>` y se indica ambos para trabajar tanto con frontend como con backend
```
/unit-testing <feature-spec> ambos implementa los test en función de la documentación de test registrada por el QA en los documentos TEST_CASES y TEST_PLAN
```

# Github Actions
## commitlint
se desarrolló un commitlint para validar los mensajes en los commits y respectar las reglas de Conventional Commits

### Estructura de validación
todos los commits deben respetar la estructura de Conventional Commits:

```
type(scope): description
type: description
```

type: feat|fix|docs|style|refactor|test|chore|ci|build
scope: opcional, pero siempre un nombre de archivo o tarea. Si se escriben varios dará error. Si son multiples tareas y/o archivos, se utiliza sin scope.
description: intentar respetar Conventional commits con descripciones correctas

En caso de no estructurar correctamente un commit, Github action no permitirá el pull request y mostrará todos los commits con naming incorrecto:
```
❌ Invalid commit message
Commit ID: $hash
Message: $commit

```

También mostrará los que sean correctos:
```
Checking commit: $hash
$commit
✅ Valid
```
Si surge un error en alguna parte, mostrará el mensaje final:
```
❌ Found $errors invalid commit(s)
```

Si no hay errores, mostrará el mensaje final:
```
✅ All commit messages are valid
```