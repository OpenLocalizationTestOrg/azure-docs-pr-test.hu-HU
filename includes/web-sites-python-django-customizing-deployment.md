**Ha az alábbi két feltétel igaz**, az Azure megállapítja, hogy az alkalmazása a Pythont használja:

* a Requirements.txt fájl hello gyökérmappa
* bármely .py fájl hello gyökérmappában vagy egy runtime.txt, amely a pythont adja meg

Amikor hello esetben egy Python adott központi telepítési parancsfájl, amely hello szabványos szinkronizálását, fájlok, valamint egyéb Python-műveleteket, mint a használja:

* A virtuális környezet automatikus felügyeletét
* A requirements.txt fájlban felsorolt csomagok pippel történő telepítését
* A hello megfelelő Web.config fájl létrehozását hello alapján kiválasztott Python-verzió.
* A statikus fájlok összegyűjtését a Django-alkalmazások számára

Bizonyos elemeinek hello alapértelmezett telepítési lépést anélkül, hogy toocustomize hello parancsfájl szabályozhatja.

Ha tooskip Python adott központi telepítési lépéseket, akkor a üres fájl létrehozásához:

    \.skipPythonDeployment

Ha tooskip statikus fájlok összegyűjtését a Django-alkalmazáshoz használni szeretne:

    \.skipDjango 

Pontosabban a központi telepítés hello következő fájlok létrehozásával felülírhatja hello alapértelmezett telepítési parancsfájl:

    \.deployment
    \deploy.cmd

Használhatja a hello [Azure parancssori felület] [ Azure command-line interface] toocreate hello fájlokat.  Futtassa a következő parancsot a projektmappából:

    azure site deploymentscript --python

Ha ezek a fájlok nem léteznek, az Azure létrehoz, majd futtat egy ideiglenes üzembe helyezési parancsfájlt.  Azonos toohello egy hoz létre a fenti hello paranccsal is.

[Azure command-line interface]: http://azure.microsoft.com/downloads/
