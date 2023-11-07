### INSTAL·LACIÓ GENERAL


# Configuración entorno de trabajo

El objetivo de tener un entorno de trabajo con ciertos elementos estandarizados es permitir la reproducción de los proyectos.

## Homebrew (opcional)
Homebrew es un gestor de paquetes del sistema, nos permite instalar programas directamente desde la linea de comandos.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Conda / Miniconda / Mamba

Usaremos estas herramientas para gestionar los paquetes que usamos en los proyectos.

- [Conda](https://www.anaconda.com/products/distribution)

- [Miniconda](https://docs.conda.io/en/latest/miniconda.html): Miniconda es una versión menos pesada de conda. Solo con lo ensencial.

Una vez instalado se recominda instalar [Mamba](https://mamba.readthedocs.io/en/latest/user_guide/mamba.html) en el enviroment base. Mamba hace lo mismo que conda, pero muchísimo más rápido. Cuando un env de conda crece, mamba tarda segundos en resolver las dependencias y conda puede tardar minutos.
```bash
conda install mamba -n base -c conda-forge
```

## Pipx

[Pipx](https://github.com/pypa/pipx) Es una herramienta para instalar y ejecutar herramientas de Python en enviroments aislados. La utilizaremos para todas aquellas herramientas que sean agnósticas del proyecto.


```bash
brew install pipx 
pipx ensurepath
```


## Estilo

El estilo del código es una cosa muy subjetiva. Como vamos a trabajar en equipo, la mejor manera de colaborar es que el estilo del código sea una especificación determinista y ejecutable. Para eso utilizaremos 2 herramientas que usan la mayoría de proyectos serios de python como Pandas, Django, etc.

[Black](https://black.readthedocs.io/en/stable/#) formatea el código.

```bash
pipx install black
```

[Isort](https://github.com/PyCQA/isort) ordena los imports.

```bash
pipx install isort
```

## IDE 

[VSCode](https://code.visualstudio.com/download) tiene muy buen soporte para proyectos de DS. Integra git y un cliente de jupyter donde poder ejecutar notebooks. En Mac os tenéis que bajar la versión .zip y moverla a la carpeta aplicaciones.

## Pre-commit

Pre-commit es una herramienta para ejecutar comandos antes de hacer un commit. Nos permite ejecutar los comandos de formateo y ordenación de imports antes de hacer un commit. De esta manera evitamos que el código upstream en el repositorio.

```bash
pipx install pre-commit
```

### Pre-commit en un proyecto nuevo [no fer cas si no es crea res de nou]

Crear el archivo `.pre-commit-config.yaml` en la raíz del proyecto con el siguiente contenido:

```yaml
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
    -   id: check-yaml
    -   id: end-of-file-fixer
    -   id: trailing-whitespace
-   repo: https://github.com/psf/black
    rev: 23.1.0
    hooks:
    -   id: black
-   repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
        name: isort
- repo: https://github.com/nbQA-dev/nbQA
  rev: 1.6.3
  hooks:
    - id: nbqa-black
    - id: nbqa-isort
```



--------

### Instal·lar git

(https://git-scm.com/download) això ens servirà per poder "connectar" amb el repositori de githu

```bash
brew install git
```


### Crear un environment (després ja l'actualitzarem)

Jo ho he fet així, però en teoria es podria fer directament. El que fem, bàsicament, es crear un environment que treballi amb python, per ja tenir la carpeta 'base' feta i després actualitzar-la amb les llibreries i dependencies del repositori.

```bash
mamba create -n nuevoEnv python=3.10
```


### Clonar el repositori de Github

[Opcional] Crec que anirà millor si primer utilitzem un d'aquests dos commands:

```bash
git config --global user.name "Your Username"
```
o
```bash
git config --global user.email your_email@example.com
```


En teoria, amb aquest command a continuació hauria de funcionar (canvia el 'username' pel teu nom d'usuari i ' repository_name' pel nom del repo):

```bash
git clone https://github.com/username/repository_name.git
```

Una vegada, ho hagis executat et demanarà autentificació del compte. Quan ho tinguis fet, ja hauries d'estar connectat al repo. 



### Actualitzem el environment que haviem creat

[IMPORTANT] Primer de tot, crec que s'hauria de canviar el PREFIX ("prefix: /Users/joanguich/anaconda3/envs/grantify") del document "environment_def.yml", i posar el path on s'hagi creat el environment que teniem. 

Una vegada fet el canvi anterior, s'ha d'executar el següent codi:

```bash
conda env update -f environment_def.yml
```

[S'ha de mirar com fer aquest pas sense haver de canviar el prefix. Hi ha un exemple de fitxer .yml ("environment.yml") que pot ser que el fitxer de environment hagi de tenir aquesta estructura]




---------------

### COSES EXTRES QUE POTSER ENS FARAN FALTA MÉS ENDAVANT.



## DVC

[DVC](https://dvc.org/doc) Es una herramienta de gestión de proyectos de ciencia de datos. La usaremos para gestionar y compartir los datos de los proyectos, así como para especificar las pipelines de los mismos. Su [tutorial](https://dvc.org/doc/start) es muy bueno, os lo recomiendo.

```bash
pipx install "dvc[s3]"
```

### Lanzar DVC en un proyecto nuevo

Una vez en un proyecto nuevo (un proyecto ya creado no requerirá de este paso) habrá que añadir la ruta del bucket de S3 de la siguiente forma:

```bash
dvc init
dvc remote add -d s3remote s3://joan.garcia-bucket/{nombre-proyecto}
```

Y de la siguiente forma le indicaremos las claves que usaremos para acceder al bucket:

```bash
dvc remote modify s3remote profile "dvc" 
```

### Configurar claves de AWS

Para poder acceder al S3 (un repositorio de datos de AWS) será necesario configurar las claves de AWS. Para ello hay que ejecutar los siguientes comandos. 

```bash
echo "[profile dvc]" >> ~/.aws/config
echo "[dvc]
aws_access_key_id = <posar_id>
aws_secret_access_key = <posar_key>" >> ~/.aws/credentials
```
Esto creará unas credenciales dentro del perfil "dvc" para acceder al bucket asignado.
