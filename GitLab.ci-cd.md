# GitLab

## CI / CD

### CI / CD en GitLab

Gitlab intro: Repository graph
mentions => Assign To-Do

#### Introducción a CI/CD

CI/CD significa Integración Continua y Entrega/Despliegue Continuo. Su objetivo es automatizar:

Integración: ejecución automática de pruebas al hacer cambios en el código.

Entrega/Despliegue: envío del código validado a producción u otros entornos sin intervención manual.

GitLab ofrece una solución completa de CI/CD integrada con los repositorios, sin necesidad de herramientas externas.

#### Configuración de un pipeline

Para activar CI/CD en GitLab, se debe crear un archivo llamado `.gitlab-ci.yml` en la raíz del repositorio.

Estructura básica

```yaml
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo "Compilando el proyecto"

test_job:
  stage: test
  script:
    - echo "Ejecutando pruebas"

deploy_job:
  stage: deploy
  script:
    - echo "Desplegando a producción"
  only:
    - main
```

Explicación:

- Stages
  - define las fases del pipeline, agrupando los jobs en etapas o secciones lógicas (test, build...).
  - las sucesivas stages se ejecutan en orden, secuencialmente
- Jobs

  - cada bloque de instrucciones (conjunto de keywords) que se ejecuta en una etapa específica, por parte de un GitLab Runner (agente remoto).
  - los distintos jobs de una misma stage se ejecutan en paralelo.
  - cuando concluyen con éxito todos los jobs de una stage, se pasa a la siguiente.
  - si alguno de los jobs falla, generalmente detiene el pipeline.

- [Keywords de un job](https://docs.gitlab.com/ci/yaml/):

  - image: define la imagen de Docker que se usará para ejecutar el job, por defecto desde DockerHub, pero opcionalmente de otras fuentes. Pueden ser globales o específicas de cada job
  - script: comandos a ejecutar en un job. Pueden indicarse los comandos o una llamada a un script del shell. Es la parte mandatoria del job.
  - service: define servicios adicionales necesarios (ej. una imagen de base de datos).
  - variables: variables de entorno para el job o globales a todos los jobs. Como veremos, pueden venir definidas desde la UI de GitLab
  - artifacts: para conservar archivos entre jobs.
  - cache: para almacenar archivos entre ejecuciones del pipeline, acelerando la ejecución de los jobs.
  - rules: control avanzado de condiciones de ejecución.
  - tags: para ejecutar en runners específicos.
  - only: especifica en qué ramas se ejecuta. Deprecado en favor de rules.
  - when: manual permite ejecutar el despliegue desde la interfaz cuando se desee.

Formas de iniciar el pipeline:

- Push a una rama (ej. main, develop).
- Manualmente desde la interfaz de GitLab.
- Programado para ejecutarse en intervalos regulares.
- Disparado por otro pipeline, incluso de otro proyecto.

#### Artefactos y dependencias entre jobs

Un job puede generar artefactos (archivos) que serán necesarios en jobs posteriores. Para definir artefactos en un job, se utiliza la keyword `artifacts`:

```yaml
build_job:
  stage: build
  script:
  script:
    - echo "Compilando el proyecto"
    - echo "Generando archivos..."
    - mkdir -p dist
    - echo "Archivo generado" > dist/archivo.txt
  artifacts:
    paths:
      - dist/
```

En jobs posteriores, se pueden usar los artefactos generados:

```yaml
test_job:
  stage: test
  script:
    - cat dist/archivo.txt
  dependencies:
    - build_job
```

#### Test values and variables

Se pueden definir variables de entorno globales o específicas de cada job. Ejemplo:

```yaml
variables:
  MY_WORD: "any text"

stages:          # List of stages for jobs, and their order of execution
  - build
  - test
  - deploy

build-job:       # This job runs in the build stage, which runs first.
  stage: build
  script:
    - echo "Compiling the code..."
    - echo "Compile complete."
    - echo "Text sample with the word $MY_WORD"  | tee file.txt
  artifacts:
    paths:
      - "file.txt"

file-test-job:   # This job runs in the test stage.
  stage: test    # It only starts when the job in the build stage completes successfully.
  script:
    - echo "Test file creation in artefact."
    - cat file.txt
  dependencies:
    - "build-job"

unit-test-job:   # This job runs in the test stage.
  stage: test    # It only starts when the job in the build stage completes successfully.
  script:
    - echo "Running unit tests... This will take about 6 seconds."
    - sleep 6
    - echo "Testing for the string gitlab"
    - grep "gitlab" file.txt
  dependencies:
    - "build-job"

lint-test-job:   # This job also runs in the test stage.
  stage: test    # It can run at the same time as unit-test-job (in parallel).
  script:
    - echo "Linting code... This will take about 10 seconds."
    - sleep 10
    - echo "No lint issues found."
```

#### Prepare deployment

Se pueden definir variables de entorno globales o específicas de cada job. Ejemplo:

```yaml
default:
  image: "alpine:latest"

stages:          # List of stages for jobs, and their order of execution
  - build
  - test
  - deploy

render_md-job:       # This job runs in the build stage, which runs first.
  stage: build
  script:
    - apk add markdown
    - markdown README.md | tee index.html
  artifacts:
    paths:
      - "index.html"

lint-test-job:   # This job runs in the test stage.
  stage: test    # It only starts when the job in the build stage completes successfully.
  script:
    - apk add libxml2-utils
    - echo "Test file creation in artefact."
    - xmllint --html index.html
  dependencies:
    - "render_md-job"

deploy-job:   # This job also runs in the test stage.
  stage: deploy    # It can run at the same time as unit-test-job (in parallel).
  environment:
    name: production
    url: https://my-production-site.com
  script:
    - echo "Deploying to production server..."
```

#### Despliegue automático

Se puede configurar un job para hacer deploys automáticos a diferentes entornos (staging, producción, etc.).

Ejemplo con despliegue simulado:

```yaml
deploy_to_staging:
  stage: deploy
  script:
    - ./scripts/deploy.sh staging
  environment:
    name: staging
  only:
    - develop

deploy_to_production:
  stage: deploy
  script:
    - ./scripts/deploy.sh production
  environment:
    name: production
    url: https://mi-app.com
  only:
    - main
  when: manual
```

#### Gestión de variables y secretos

GitLab permite definir variables de entorno desde la interfaz web o el archivo CI para almacenar:

- Tokens de acceso (API keys, secretos).
- Configuración del entorno (host, puertos, contraseñas).
- Variables personalizadas.

Definir en .gitlab-ci.yml (no recomendado para secretos):

```yaml
variables:
  NODE_ENV: production
  API_URL: https://api.miapp.com
```

Definir en la interfaz:

Ir a Settings > CI/CD > Variables.

Añadir variables seguras como API_KEY, DB_PASSWORD, etc.

Activar Protect variable para limitar su uso a ramas protegidas.

Uso en el script:

```yaml
script:
  - curl -H "Authorization: Bearer $API_KEY" $API_URL
```

#### Buenas prácticas en CI/CD

- Usar rules en lugar de only/except para mayor flexibilidad.
- Proteger ramas y runners para evitar ejecuciones no deseadas.
- Usar entornos (environment:) para distinguir producción y staging.
- Usar variables protegidas para tokens sensibles.
- Utilizar caché y artifacts para acelerar pipelines.
