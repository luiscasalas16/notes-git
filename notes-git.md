# notes-git

---
### Configuración
```
//establecer nombre usuario
git config --global user.name "Luis Carlos Salas Villalobos"
//consultar nombre usuario
git config --global user.name
//establecer correo usuario
git config --global user.email "lusalas16@gmail.com"
//consultar correo usuario
git config --global user.email
//eliminar waring de saltos de linea
git config --global core.autocrlf true

//editar configuración
git config --global -e 

//git s (status)
git config --global alias.s "status --short --branch"
//git l (log)
git config --global alias.l "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"
//git acp "message" (add & commit & push)
git config --global alias.acp "!f() { git add -A && git commit -m \"$@\" && git push; }; f"
//git ac "message" (add & commit)
git config --global alias.ac "!f() { git add -A && git commit -m \"$@\"; }; f"
```
---
### Files
```
//archivo para ignorar archivos o carpetas, se incluye en el repositorio
.gitignote
//archivo para incluir carpetas vacias en el repositorio, se incluye en el repositorio
.gitkeep
```
---
### Repositorio
```
//inicializar repositorio
git init

//obtener estado del repositorio
git status

//incluir un archivo o todos con . en el stage
git add .
git add ARCHIVO
git add ARCHIVO ARCHIVO
git add CARPETA
git add CARPETA CARPETA
git add *.html
git add js/
git add js/*.js

//mueve o renombra X como Y (si es el mismo directorio)
git mv X Y

//elimina X
git rm X
```
---
### Commit
```
//resetear ARCHIVO o . (todos) en el stage
git reset .

//listar archivos previo a commit
git commit --dry-run
//commit con mensaje
git commit -m "MENSAJE"
//incluir y commit con mensaje, sólo para archivos que ya estén tranqueado
git commit -am "MENSAJE"

//descarta cambios tracked locales
git restore .
//descarta cambios untracked locales
git clean -d -f

//reconstruir versión actual de ARCHIVO o . (todos)
git checkout -- .
//obtiene la versión del COMMIT del repositorio
git checkout COMMIT
//obtiene la versión del COMMIT del ARCHIVO
git checkout COMMIT ARCHIVO
//devuelve el repositorio la versión actual
git checkout RAMA
//devuelve el ARCHIVO la versión actual, si está en el staged hay que quitarlo con un reset
git checkout -- ARCHIVO

//obtener nombre de rama
git branch
//cambiar nombre de rama de master a main
git branch -m master main

//obtener detalle de commits
git log

//ver diferencias entre cambios locales
git diff
//ver diferencias entre cambios locales en el staged
git diff --staged

//actualiza comentario de último commit
git commit --amend -m "MENSAJE"
//hace commid de cambios en el último commit
git commit --amend --no-edit

//deshace ~X commits o hasta el COMMIT y establece los cambios en el staged para volver a hacer el commit
git reset --soft HEAD~X

//deshace ~X commits o hasta el COMMIT y desecha cambios para volver a versiones previas del repositorio
git reset --hard COMMIT

//historia completo de referenicia de cambios en repositorio, se pueden utilizar para un reset los IDs
git reflog

//lista cambios realizados en COMMIT
git show COMMIT --stat
//lista archivos cambiados en COMMIT 
git diff-tree COMMIT
```
---
### Tags
```
//lista tags
git tag
//crea tag X, el mensaje es el mismo del commit donde se hace
git tag X
//elimina tag X
git tag -d X
//crea tag anotado con MENSAJE
git tag -a v1.0.0 -m MENSAJE
//crea tag anotado con MENSAJE en COMMIT
git tag -a v1.0.0 COMMIT -m MENSAJE
```
---
### Stashs
```
//lista stashs
git stash list
//crea stash
git stash
//crea stash con un mensaje
git stash save "mensahe"
//obtiene último stash
git stash pop
//elimina stashs
git stash clear
//obtiene un shash particular X
git stash apply stash@{X}
//elimina un shash particular X
git stash drop stash@{X}
//muestra un shash particular X
git shash show stash@{X}
//restaura stash en RAMA
git stash branch RAMA
```
---
### Rebase
```
//actualiza la base de la rama con X, se debe estar en la rama que uno quiere actualizar
git rebase X
//lanza el modo interactivo, se puede usar: squash para unir commits, reword para renombra mensajes, edit para separar commits
git rebase -i HEAD~X
```
---
### Remote
```
//lista origenes remotos
git remote -v
//registra un origen remoto origin con la URL
gir remote add origin URL
//hace push en origen remoto en rama main, con -u la rama se establece por defecto
git push -u origin main
//hace push de tags en origen remoto
git push --tags
//hace push de los cambios
git push 
//hace pull de los cambios
git pull

//especifica estrategia para hacer pull y evitar waring, se puede habilitar ff y si no rebase
git config --global pull.ff only
git config --global pull.rebase true

//clona el repositorio en la URL con el historial localmente
git clone URL
//actualiza la URL del repositorio 
git remote set-url origin URL
```
---
### Ramas
```
//lista ramas
git branch
//crea RAMA
git branch RAMA
//mueve a RAMA
git checkout RAMA
//hace merge de la RAMA en la actual
git merge RAMA
//elimina la RAMA
git branch -d RAMA
//actualiza las ramas desde el origin, si sólo existe localmente
git remote prune origin
//crea y mueve a RAMA
git checkout -b RAMA
//aborta merge
git merge --abort
	
//hacer RAMA
    git branch RAMA
    git checkout RAMA

//hacer merge de RAMA en main
    git checkout main
    git merge RAMA
    git branch -d RAMA

//actualizar RAMA con main
    git checkout RAMA
    git rebase main

//actualizar RAMA con main
    git checkout RAMA
    git merge main

//hacer push
    git pull
    si hay conflictos
        resolver conflictos
        git commit -am "merge"
        git rebase --continue
    git push

//subir RAMA a remote y compartir
    // A sube RAMA
    git push origin RAMA
    // B baja RAMA
    git pull --all
    // B lista las ramas
    git branch --all
    // B cambia a RAMA
    git checkout RAMA
    ... // B hace cambios y commits en RAMA
    // B sube RAMA
    git push origin RAMA
    // A baja RAMA
    git pull origin RAMA
    // se elimina ramas
    git push origin :RAMA
    
//sobreescribir archivos locales desde RAMA
    git fetch --all
    git reset --hard origin/main
    
//unir últimos 2 commits
    git reset --hard HEAD~2
    git merge --squash HEAD@{1}

//obtiene los cambios del repositorio central, pero no mueve el head, hay que hacer un pull
git fetch

//sube una RAMA a repositorio
git push origin RAMA
//descarga RAMAS del repositorio
git pull --all
//elimina las RAMAS eliminadas del repositorio
git remote prune origin
```
