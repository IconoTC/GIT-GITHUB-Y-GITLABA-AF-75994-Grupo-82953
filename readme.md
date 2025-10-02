# Curso de Git

- Duración: 25 horas
- Modalidad: On-line
- Fechas: 29, 30 Septiembre, 1, 2 y 3 Octubre 2025
- Horario 9:30 – 14:30 hs.

## Contenidos

- Introducción
  - Instalación y configuración de git
  - Gestión de un repositorio
    - Quick Start
    - Aprendiendo a referenciar revisiones y paths
    - Herramientas para preparar un buen commit en cualquier situación
    - Rescribiendo la historia
  - GUI’s (Integración con otras herramientas y entornos)
    - Github
    - GitLab
- Ramas (Trabajando en paralelo)
  - Ramificación
  - Fusión de Ramas
  - Resolviendo Problemas
  - Tipos de Ramas
  - Tags
- Buenas prácticas
  - Reorganización
  - Regla de Oro
  - Forzar subida
- Colaboración en Github
  - Ramas remotas
  - Publicando
  - Tags
  - Pull Request
  - Liberaciones
  - Github Actions
- Github Actions
  - Introducción
  - Workflow. Partes y sintaxis
  - Configuración y ejecución de un workflow
  - Construir la imagen con Docker
  - Secretos
- GitFlow
  - Introducción
  - Features
  - Releases
  - Hotfix
  - Git hooks
- GitLab
  - Introducción
  - Versiones
  - Funcionalidades
  - Arquitectura
  - Usuarios
  - Repositorios
  - Ramas
  - Merge Request
  - Pipelines

Desarrollo del curso en la carpeta Oficial -> [repo](https://github.com/IconoTC/GIT-GITHUB-Y-GITLABA-AF-75994-Grupo-82953)

## Desarrollo del curso

## Día 1 (Lunes 29 Septiembre 2025)

- Presentación profesor / alumnos
- Introducción: Qué es un SCV y qué un SCV distribuido
- IDE / Editor de código: Visual Studio Code (VSC)
- Terminales
- Instalación de Git
  Configuración inicial
- Primeros pasos con Git
  - Primer repo (init), primer commit
  - Anatomía de un repositorio git: working directory, staging area (index o cache) y repositorio (.git)
  - Estados de un archivo: untracked (U), tracked (modified (M), staged (A), committed)
  - add/commit/reset y status/log/show
  - Mensajes de commit
- Anatomía de comandos típicos, referencias VS paths
  - HEAD, master, HEAD~1 y otras referencias útiles
  - Referencias por mensaje de commit (:/cadena)
- Integración con otras herramientas y entornos
  - Clientes gráficos
  - Entornos de desarrollo
  - Repositorios remotos: GitHub, GitLab, Bitbucket
- Ficheros Markdown
  - Qué son
  - Sintaxis básica
  - Vista previa en VSC
- Aliases
  - Qué son
  - Cómo crearlos: `git config --global alias.co checkout`

## Día 2 (Martes 30 Septiembre 2025)

- Git internals
  - Estructura de un repositorio git: .git
  - Objetos git: blobs, trees, commits (y tags)
  - Creación y lectura de objetos
  - Creación del árbol de objetos en un primer commit
  - Modificación del árbol de objetos en commits sucesivos
  - Referencias: heads, ramas (tags y remotes)
  - Taller: creación de un repositorio git "a mano"
- Herramientas para preparar un buen commit en cualquier situación
  - Comprobar el repositorio. Git log
  - Operaciones en la Staging Area (Index)
    - Añadir ficheros
    - Eliminar de la Staging Area (Index)
  - Eliminar ficheros
    - Problemas con .gitignore
  - Cambiar nombre de ficheros
  - git diff
  - git blame
  - Recapitulando: Git básico
- Reescribiendo la historia
  - Advertencia
  - git command --amend
  - git checkout
  - git reset

## Día 3 (Miércoles 1 Octubre 2025)

- Reescribiendo la historia (2)
  - Evolución de git checkout: Nuevos comandos git switch y git restore
  - git checkout a nivel de archivo (restore)
  - git reset a nivel de archivo
  - rebase interactivo
    - edit: modificando un commit
    - squash y fixup: fusionando commits
    - drop: eliminando un commit
  - Ref logs
  - Otros comandos
    - git stash
    - git clean
    - git revert
    - git bisect
- Trabajando en paralelo
  - Ramas
    - Crear y seleccionar
      - Crear desde referencia
    - Ver ramas
    - Borrar ramas
    - Mover y renombrar ramas
  - Combinación de ramas: Merge y Rebase
    - git merge
      - fast-forward
      - three-way merge
    - git rebase
    - git cherry-pick
    - Resolución de conflictos

  - Repositorios remotos
    - Repositorios "bare"
    - Clonar repositorios: git clone
    - git remote
    - git push
    - git pull
      - git fetch
      - git merge / git rebase
      - Conflictos

<!-- ## Día 4 (Jueves 2 Octubre 2025)

    - Ramas remotas
      - Seguimiento de ramas remotas
      - Eliminar ramas remotas -->
