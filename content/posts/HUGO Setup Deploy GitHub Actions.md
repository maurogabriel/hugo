---
title: "Hugo: Setup and Deploy Site using GitHub Actions"
date: "2020-07-13"
author: "Mauro"
---

Si estas usando HUGO para generar tu blog, proyecto o sitio web seguro estás familiarizado con la línea de comando para construir y testear tu sitio en de forma local para luego subirlo con a tu repositorio de preferencia.

Para muchos es un desafio mantener un proyecto de esta manera, en especial al no contar con una interfaz gráfica al estilo Wordpress y menos si tenes que hacer cambios continuos a un artículo o subir varios artículos al día. Esto implicaría utilizar una gran cantidad de comandos y acciones que terminarían de cansarte.

Felizmente ahora es posible automatizar gran parte de ese desarrollo con GitHub Actions. Veamos que es esta nueva herramienta y cómo podemos configurar un workflow para mantener nuestro sitio (en el ejmplo de este artículo vamos a usar GitHub Pages para alojar nuestro sitio web).

GitHub Actions
Esta herramienta es un CI/CD para automatizar, testear, construir y desplegar varios procesos en GitHub, definidos en un solo archivo `.yml`. Es el equivalente y la respuesta de GitHub al CI/CD de GitLab. Esto te permite ahorrar una enorme cantidad de tiempo y comandos que te permitirán obtener resultados casi instantáneos en tu sitio. 

Ahora veamos paso por paso como configurar nuestro sitio y definir el workflow en Actions.

Paso 1: Vamos a crear un repositorio con nuestro nombre de usuario en la siguiente forma ***mauro/mauro.github.io***, esto nos permitirá utilizar GitHub Pages para alojar nuestro sitio y definir mas adelante un dominio propio que apunte a esta dirección. Este repositorio servirá de host de nuestro sitio estático. 

Paso 2: Crear un repostorio donde subiremos el sitio que desarrollaremos con HUGO localmente. Puede ser algo asi: ***mauro/hugo.github.io***

Paso 3: Ahora vamos a clonar con git el repositorio que creamos en ***mauro/hugo.github.io*** a una carpeta local. No te desesperes si te advierte la consola que no hay nada para clonar (por el momento). 

```bash
git clone https://github.com/mauro/hugo && cd /Users/mauro/Documents/ #En este caso se clonara el repositorio Hugo creando una carpeta local en /Users/mauro/Documents/Hugo  
```
Paso 4: En este paso hice lo siguiente. Como ya tenia creado un sitio con Hugo en otra carpeta, solo copie los archivos necesarios y los pegue aqui. De tal modo que esos serán los archivos que se clonarán al repositorio.

Paso 5: Ahora con el tenido en la carpeta vamos a subir son git el contenido a nuestro repositorio que será nuestro espacio de desarrollo para nuestro sitio. 

```bash
git init
git remote add origin git@github.com:mauro/hugo
git add --all
git commit -m "Commit Message"
git push origin master
```
Paso 6: Creamos un Token en GitHub. Esto nos permitirá dar las credenciales al workflow que vamos a cargar en Actions para automatizar nuestras tareas. Vamos a Settings>Developer settings>Personal access tokens y generamos un nuevo Token. Copiamos el código y lo pegamos en algún lado seguro pues lo necesitarás para este y otros trabajos más adelante.

Paso 7: Ahora vamos al repositorio https://github.com/mauro/hugo a Settings>Secrets y agregamos un nuevo Secret. Aqui pegamos el token que copiamos.

Paso 8: Ahora si creamos nuestro workflos en GiuHub Actions. En el repositorio https://github.com/mauro/hugo vamos a Actions y creamos un nuevo workflow. Vamos a cargar un nuevo workflow que se alojará en la dirección `hugo/.github/workflows/main.yml`. 

Aqui vamos a pegar el sigueinte código:
```bash
name: CI
on: push
jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - name: Git checkout
        uses: actions/checkout@v2

      - name: Update theme
        # (Optional)If you have the theme added as submodule, you can pull it and use the most updated version
        run: git submodule update --init --recursive

      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.64.0"  

      - name: Build
        # remove --minify tag if you do not need it
        # docs: https://gohugo.io/hugo-pipes/minification/
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.TOKEN }}
          external_repository: mauro/mauro.github.io # En este ejemplo sería así
          publish_dir: ./public
          #   keep_files: true
          user_name: <username> #Solo tu usuario de GitHub
          user_email: <username@email.com> #Tu correo electrónico
          publish_branch: master
          cname: maurogabriel.com.ar #Si no tenes un dominio podes comentar esta línea para deshabilitar la opcion. 
``` 
Grabamos este workflow y listo. GitHub ya sabrá que hacer cada vez que hagas un push de tu sitio en la consola.

De manera breve, cada vez que hagas una modificación en tu sitio local (/Users/mauro/Documents/Hugo) y luego al hacer push al repositorio Actions ejecutará el workflow main.yml, procesará los cambios (cargando el sitio entero de nuevo) y cargará los archivos renderizados .html a la carpeta Public que se alojara en ***mauro/mauro.github.io***.

Podés probrar cargando un nuevo post en tu sitio local así

```bash
cd /Users/mauro/Documents/Hugo
hugo new posts/hello.md
#Podes modificar el archivo hello.md y luego continuar
git init
git add .
git commit -m "Nuevo Post"
git push origin master
```
Y eso es todo, ahora dejás que el workflow renderice todo y ya podés entrar a tu sitio, por ejemplo mauro.github.io para ver los resultados.

[Fuente](https://ruddra.com/hugo-deploy-static-page-using-github-actions/#step-3-push-hugo-site-code-in-github)