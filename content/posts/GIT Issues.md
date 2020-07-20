---
title : "Your branch is ahead of Origin/Master by X Commits"
date : "2020-07-12"
author : "Mauro"
tags : [ "git", ]
---

Imagina el siguiente escenario. Al momento de aplicar los cambios en el servidor de producción verificas el estado actual del repositorio local antes de traer los cambios hechos en tu repositorio en remoto y te das cuenta de que tu rama local se encuentra por delante de la rama ‘origin/master’ tal como se muestra a continuación.

```bash
git status
# On branch master
# Your branch is ahead of 'origin/master' by X commits.
# (use "git push" to publish your local commits)
nothing to commit, working directory clean
```

Esto suele suceder debido a que hay cambios hechos en el repositorio del servidor de producción y no los haz enviado a tu repositorio remoto (GitHUB O BitBucket).

Si deseas mantener esos cambios pero actualizar tu repositorio local utiliza el siguiente comando:

```bash
git fetch
```

Recupera(fetch) objetos y referencias descargándolas desde un repositorio remoto

Nota: Al intentar hacer de nuevo `push` si no envías los cambios a tu repositorio remoto persistirá el problema.

Si lo que deseas es eliminar esos cambios y solo dejar los que vienen de tu repositorio remoto utiliza:

```bash
git fetch -p 
```

Descripción:
Antes de recuperar, remueve cualquier referencia de seguimiento que no se exista en el repositorio remoto

Así al hacer de nuevo “git pull origin” master no tendremos de nuevo el mismo inconveniente incremental “Your branch is ahead of `origin/master` by X commits”.

