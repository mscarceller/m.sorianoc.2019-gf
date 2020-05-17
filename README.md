#### Máster Cloud Apps Módulo IV - DevOps, integración y despliegue continuo

# Práctica: Repositorios y modelos de desarrollo 

## 1. GitFlow

* Se inicia el repositorio y se sicroniza con el remoto:


    ```console
    git flow init -d 
    git remote add origin git@github.com:mscarceller/m.sorianoc.2019-gf.git
    git push -u origin develop
    ```

* Se inicia la funcionalidad f1:

    ```console
    git flow feature start feature-f1
    ```

* Se añaden los 3 ficheros haciendo commit de cada uno, y empujandolos al repositorio remoto:

    ```console
    echo "This is the 'a' file" > file-a.txt
    git add file-a.txt
    git commit -m "Add file 'a' in feature-f1"
    git push -u origin feature/feature-f1

    echo "This is the 'b' file" > file-b.txt
    git add file-b.txt
    git commit -m "Add file 'b' in feature-f1"
    git push -u origin feature/feature-f1

    echo "This is the 'c' file" > file-c.txt
    git add file-c.txt
    git commit -m "Add file 'c' in feature-f1"
    git push -u origin feature/feature-f1
    ```

* Se finaliza la funcionalidad f1

    ```console
    git flow feature finish feature-f1 -k
    ```

* Se empuja la rama develop a remoto:

    ```console
    git push -u origin develop
    ```

* Se realiza (inicia y finaliza) la release 0.1.0 y se sube a remoto:

    ```console
    git flow release start 0.1.0
    git push -u origin release/0.1.0
    git flow release finish 0.1.0
    ```

* Se publica en remoto la rama master y se sube la tag 0.1.0:

    ```console
    git push -u origin develop
    git push -u origin master 
    git push --tags
    ```

## **YA TENEMOS LA RELEASE 0.1.0** 


* Se inician las dos funcionalidades f2 y f3 para desarrollarlas en paralelo:

    ```console
    git flow feature start feature-f2
    git flow feature start feature-f3
    ```

**Primero se trabaja sobre la funcionalidad f2:**

* Se añade el nuevo fichero en la funcionalidad f2, se hace commit y se sube al repositorio remoto:

    ```console
    git checkout feature/feature-f2
    echo "This is the 'd' file" > file-d.txt
    git add file-d.txt
    git commit -m "Add file 'd' in feature-f2"
    git push -u origin feature/feature-f2
    ```

* Se modifica el fichero "a" en la funcionalidad f2 y se hace commit y se sube al repositorio remoto:

    ```console
    echo -e "f2 - $(cat file-a.txt)" > file-a.txt 
    git add file-a.txt
    git commit -m "Modify file 'a' with feature-f2 annotation"
    git push -u origin feature/feature-f2
    ```

* Una vez terminada la funcionalidad f2 se finaliza para integrarla con el resto de la aplicación:

    ```console
    git flow feature finish feature-f2 -k
    ```

* Se empuja de nuevo la rama develop a remoto, ahora con la f2:

    ```console
    git push -u origin develop
    ```

* Se realiza (inicia y finaliza) la release 0.2.0 y se sube a remoto:

    ```console
    git flow release start 0.2.0
    git push -u origin release/0.2.0
    git flow release finish 0.2.0
    ```

* Se publica en remoto la rama master y se sube la tag 0.2.0:

    ```console
    git push -u origin develop
    git push -u origin master 
    git push --tags
    ```

## **YA TENEMOS LA RELEASE 0.2.0** 

En este punto se notifica una incidencia (bug) en producción, y se desarrolla un hotfix consistente en añadir el fichero f al repositorio, para ello:

* Se inicia una rama de hotfix:

    ```console
    git flow hotfix start 0.2.1
    ```

* Se añade el fichero "f" que soluciona el bug notificado:

    ```console
    echo "This is the 'f' file to fix the bug" > file-f.txt
    git add file-f.txt
    git commit -m "Add file 'f' in hotfix 0.2.1"
    git push -u origin hotfix/0.2.1
    ```

* Se finaliza el hotfix:

    ```console
    git flow hotfix finish 0.2.1
    ```

* Se publica en remoto la rama master y se sube la tag 0.2.1:

    ```console
    git push -u origin develop
    git push -u origin master 
    git push --tags
    ```

## **YA TENEMOS LA RELEASE 0.2.1** 

**Trabajos sobre la funcionalidad f3:**

Mientras se realiza la funcionalidad f2 tambien se lleva a cabo la f3:

* Se añade el fichero "e":

    ```console
    git checkout feature/feature-f3
    echo "This is the 'e' file" > file-e.txt
    git add file-e.txt
    git commit -m "Add file 'e' in feature-f3"
    git push -u origin feature/feature-f3
    ```

* Se modifica el fichero "a" en la funcionalidad f3 y se hace commit y se sube al repositorio remoto:

    ```console
    echo -e "$(cat file-a.txt) - f3" > file-a.txt 
    git add file-a.txt
    git commit -m "Modify file 'a' with feature-f3 annotation"
    git push -u origin feature/feature-f3
    ```

* Una vez terminada la funcionalidad f3 se finaliza para integrarla con el resto de la aplicación:

    ```console
    git flow feature finish feature-f3 -k
    ```

En este punto tenemos la f2 y la f3 con los ficheros d y e respectivamente pero el fichero a con contenido diferente, y surge un conflicto:

    ```
    <<<<<<< HEAD
    f2 - This is the 'a' file
    =======
    This is the 'a' file - f3
    >>>>>>> feature/feature-f3
    ```

* Resolvemos este conflicto dejando solo la linea:

    ```
    "This is the 'a' file with f2 anf f3 conflict solved"
    ```
* Continuamos la finalización de la feature:

    ```console
    git add . file-a.txt
    git commit -m "Modify file 'a' with feature-f3 annotation and conflict f2 solved"
    git flow feature finish feature-f3 -k
    ```

* Se empuja de nuevo la rama develop a remoto, ahora con la f3:

    ```console
    git push -u origin develop
    ```

* Se realiza la release con la funcionalidad f3:

    ```console
    git flow release start 0.3.0
    git push -u origin release/0.3.0
    git flow release finish 0.3.0
    ```

* Se publica en remoto la rama master y se suba la tag 0.3.0:

    ```console
    git push -u origin develop 
    git push -u origin master  
    git push --tags
    ```

## **YA TENEMOS LA RELEASE 0.3.0**
