---
title: "aaaDeploy tooAzure Jenkins beépülő modul az App Service |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure App Service Jenkins beépülő modul toodeploy Java web app tooAzure a Jenkins"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 080be7277555ce7d688dccdf38eef309e7a7b194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-plugin"></a>App Service tooAzure rendelkező Jenkins beépülő modul telepítése 
a Java web app tooAzure toodeploy, használhatja az Azure CLI [Jenkins csővezeték](/azure/jenkins/execute-cli-jenkins-pipeline) lehetőség hello használata [Azure App Service Jenkins beépülő modul](https://plugins.jenkins.io/azure-app-service). Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Jenkins toodeploy tooAzure keresztül FTP App Service konfigurálása 
> * Linux-rendszeren keresztül Docker Jenkins toodeploy tooAzure App Service konfigurálása 

## <a name="create-and-configure-jenkins-instance"></a>Hozzon létre és Jenkins konfigurálása
Ha még nem rendelkezik egy Jenkins master, kezdje hello [Megoldássablonban](install-jenkins-solution-template.md), mely tartalmazza a JDK8 és hello szükséges beépülő modulok a következő:

* [Jenkins Git ügyfél beépülő modul](https://plugins.jenkins.io/git-client) v.2.4.6 
* [Docker Commons beépülő modul](https://plugins.jenkins.io/docker-commons) v.1.4.0
* [Azure hitelesítő adataival](https://plugins.jenkins.io/azure-credentials) v.1.2
* [Az Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1

Hello App Service beépülő modul toodeploy webalkalmazást az Azure App Service által támogatott összes nyelven (például C#, PHP, Java és node.js stb.) is használhatja. Az oktatóanyag Java hello mintaalkalmazást, használunk [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample). toofork hello tárház tooyour saját GitHub-fiók, kattintson a hello **elágazás** hello jobb felső sarkában található gombra.  

Java JDK és Maven szükségesek hello Java projekt. Ellenőrizze, hogy hello összetevőinek telepítését hello Jenkins fő vagy a Virtuálisgép-ügynök hello egy folyamatos integrációt a használatakor. 

tooinstall, jelentkezzen be toohello Jenkins példány SSH használatával, és futtassa a következő parancsok hello:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

TooApp Linux szolgáltatás telepítéséhez, akkor is kell a Docker tooinstall hello Jenkins master vagy hello build használt Virtuálisgép-ügynök. Tekintse meg a toothis cikk tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.

## <a name="add-azure-service-principal-toojenkins-credential"></a>Az Azure szolgáltatás egyszerű tooJenkins hitelesítő adatok hozzáadása

Egy Azure szolgáltatás egyszerű szükséges toodeploy tooAzure. 

<ol>
<li>Használjon [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) vagy [Azure-portálon](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate egy egyszerű Azure szolgáltatást</li>
<li>Belül hello Jenkins irányítópultján kattintson **hitelesítő adatok -> rendszer ->**. Kattintson a **globális credentials(unrestricted)**.</li>
<li>Kattintson a **adja hozzá a hitelesítő adatok** tooadd egy Microsoft Azure szolgáltatás egyszerű előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont hello kitöltésével. Adja meg az Azonosítót **mySp**, a későbbi lépésekben használatra.</li>
</ol>

## <a name="azure-app-service-plugin"></a>Az Azure App Service beépülő modul

Az Azure App Service beépülő modul 1.0-s verziója támogatja a folyamatos üzembe helyezés tooAzure Web App használatával:

* Git és az FTP
* Webalkalmazás Linux docker

## <a name="configure-jenkins-toodeploy-web-app-through-ftp-using-hello-jenkins-dashboard"></a>Jenkins toodeploy keresztül hello Jenkins irányítópulttal FTP webalkalmazás konfigurálása

toodeploy a projekt tooAzure Web App, feltöltheti a build összetevők (például .war fájl Java) Git vagy FTP segítségével.

Mielőtt beállítaná a Jenkins hello feladat, kell az Azure App Service-csomag és a webes alkalmazás futó hello Java-alkalmazás.


1. Hozzon létre az Azure App Service-csomag hello **szabad** hello segítségével tarifacsomag [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot. hello App Service-csomagot határoz meg hello használt fizikai erőforrások toohost az alkalmazásokat. App Service-csomagot tooan hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, így toosave költség több alkalmazás esetén.
2. Webes alkalmazás létrehozása. Vagy használjon hello is [Azure-portálon](/azure/app-service-web/web-sites-configure) , vagy használjon hello Az parancssori parancsot:
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. Ellenőrizze, hogy a kell hello Java runtime konfigurációs beállítása. a következő parancs az Azure parancssori felület hello hello web app toorun konfigurálja a legújabb Java 8 JDK és [Apache Tomcat](http://tomcat.apache.org/) 8.0.
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-hello-jenkins-job"></a>Hello Jenkins feladat beállítása


1. Hozzon létre egy új **freestyle** projekt Jenkins irányítópult
2. Konfigurálása **forrás kód felügyeleti** toouse az a helyi elágazáshoz [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) hello megadásával **tárház URL-CÍMÉT**. Például: http://github.com/&lt;yourID > / javawebappsample.
3. Adjon hozzá egy összeállítása lépés toobuild hello projektet Maven használatával. Ehhez adja hozzá egy **hajtható végre a rendszerhéj**. Ebben a példában egy további lépést toorename hello *.war fájl tároló mappa tooROOT.war kell.   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. Adja hozzá a létrehozás után végrehajtandó művelet kiválasztásával **közzététele az Azure Web Apps**.
5. Adjon meg, "mySp" hello Azure szolgáltatás egyszerű tárolja az előző lépésben.
6. A **Alkalmazáskonfiguráció** területen válassza ki a hello erőforrás csoport és a webes alkalmazást az előfizetésben. hello beépülő modul automatikusan észleli, hogy-e a Windows hello webalkalmazás vagy Linux-alapú. Egy Windows-alapú webalkalmazás "Közzététele fájlok" hello lehetőség áll rendelkezésre.
7. Töltse ki a hello fájlok kívánt toodeploy (például egy war-csomag használata Java.) Forrás és cél könyvtárat opcionálisak. hello paraméterek lehetővé teszik toospecify forrása és célja mappák fájlok feltöltésekor. Java-webalkalmazás az Azure-on fut egy Tomcat kiszolgálót. Így feltöltött, háború csomagot a webapps mappát. Ebben a példában beállítása **forráskönyvtár** túl "cél" és "webalkalmazás" megadnia **célkönyvtár**.
8. Ha azt szeretné, hogy toodeploy tooa tárolóhely éles eltérő, akkor is megadhatja **tárolóhely** nevét.
9. Mentse hello projektet, és állítsa be úgy. A webalkalmazás üzembe helyezett tooAzure esetén build befejeződött.

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a>Webalkalmazás keresztül Jenkins kimenetátirányításának segítségével FTP telepítése

hello beépülő modul az adatcsatorna használatra kész. Olvassa el a GitHub-tárház hello tooa mintát.

1. Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile_ftp_plugin** fájlt. Kattintson a gombra hello ceruza ikonra tooedit a fájl tooupdate hello erőforráscsoport és 11 és 12 sor a webalkalmazás nevét.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Sor 14 tooupdate hitelesítő adat azonosító Jenkins betűtípusainak módosítható.    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a>Hozzon létre egy Jenkins folyamat

1. Nyissa meg webböngészővel Jenkins, kattintson **új elem**.
2. Adja meg a hello feladatnak a nevét, majd válasszon **csővezeték**. Kattintson az **OK** gombra.
3. Kattintson a hello **csővezeték** következő lap.
4. A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.
5. A **SCM**, jelölje be **Git**. Adja meg a villás tárház a Githubon URL-cím hello: https:&lt;a villás tárház > .git
6. Frissítés **parancsfájl elérési útján** túl "Jenkinsfile_ftp_plugin"
7. Kattintson a **mentése** és futtatási hello feladat.

## <a name="configure-jenkins-toodeploy-web-app-on-linux-through-docker"></a>Linux keresztül Docker Jenkins toodeploy webalkalmazás konfigurálása

Git/FTP, leszámítva a webalkalmazás Linux állításra Docker támogatja. használja a Docker, toodeploy tooprovide egy Dockerfile, amely csomagokat a webalkalmazás szolgáltatás futtatási idő mellett a docker-lemezkép van szüksége. Hello beépülő modul majd hello képet alkot, leküldéses értesítések tooa docker beállításkulcs, és hello kép tooyour webes alkalmazást telepíti.

Webes alkalmazás Linux támogatja a hagyományos módon, mint például a Git és az FTP, de csak beépített nyelveket (.NET Core, Node.js, PHP és Ruby) is. A többi nyelvet kell toopackage az alkalmazás kódja és szolgáltatás futásidejű együtt egy docker-lemezképpel, és docker toodeploy használja.

Mielőtt beállítaná a Jenkins hello feladat, kell Linux az Azure app service. Egy tároló beállításjegyzék is szükséges toostore, és a titkos Docker-tároló lemezképek kezeléséhez. Használhatja a DockerHub; Azure-tároló beállításjegyzék ehhez a példához használjuk.

* A lépésekkel hello [Itt](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate Linux webes alkalmazás 
* Az Azure tároló beállításjegyzék egy felügyelt [Docker beállításjegyzék] alapján (https://docs.docker.com/registry/) szolgáltatás hello nyílt forráskódú Docker beállításjegyzék 2.0. Lépések hello [itt] (/ azure/container-registry/container-registry-get-started-azure-cli) kapcsolatos további útmutatás toodo így. DockerHub is használható.

### <a name="toodeploy-using-docker"></a>toodeploy docker használatával:

1. Hozzon létre új freestyle projekt Jenkins irányítópulton.
2. Konfigurálása **forrás kód felügyeleti** toouse az a helyi elágazáshoz [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) hello megadásával **tárház URL-CÍMÉT**. Például: http://github.com/&lt;yourid > / javawebappsample.
Adjon hozzá egy összeállítása lépés toobuild hello projektet Maven használatával. Ehhez adja hozzá egy **hajtható végre a rendszerhéj** , és adja hozzá a következő sort a hello **parancs**:    
```bash
mvn clean package
```

3. Adja hozzá a létrehozás után végrehajtandó művelet kiválasztásával **közzététele az Azure Web Apps**.
4. Adja meg, **mySp**, hello Azure szolgáltatás egyszerű Azure hitelesítő adatként az előző lépésben tárolja.
5. A **Alkalmazáskonfiguráció** területen válassza ki a hello az erőforráscsoportot és egy Linux-webalkalmazást az előfizetésben.
6. Válassza a Docker keresztül közzétenni.
7. Töltse ki **Dockerfile** elérési útja. Hello alapértelmezett megtarthatja "/ Dockerfile" a **Docker beállításjegyzék URL-cím**, szállítási https:// hello formátumban&lt;myRegistry >. Ha Azure-tároló rendszerleíró azurecr.io. Hagyja üresen a mezőt DockerHub használatakor.
8. A **beállításjegyzék hitelesítő adatok**, vegye fel a hello Azure tároló beállításjegyzék hello hitelesítő adatait. Hello felhasználónév és jelszó lekéréséhez, futtassa a következő Azure parancssori felület parancsai hello. hello első parancs lehetővé teszi, hogy a hello rendszergazdai fiókot.    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. docker nevet és -címke hello **speciális** lapon opcionálisak. Alapértelmezés szerint a lemezkép neve egységekre hello lemezképből konfigurálta az Azure portál (a Docker-tároló beállítás.) hello címke neve $BUILD_NUMBER jön létre. Győződjön meg arról, adja meg a hello kép neve vagy az Azure portálon, vagy adjon meg egy értéket a **Docker kép** a **speciális** fülre. A jelen példában adja meg a "&lt;yourRegistry >.azurecr.io/calculator" a **Docker kép** és hagyja **Docker Képcímke** üres.
10. Megjegyzés: a telepítés sikertelen lesz, ha a beépített Docker lemezkép beállítást használja. Ellenőrizze, hogy a docker config toouse egyéni lemezképként az Azure-portálon a Docker-tároló beállítást módosíthatja. Beépített lemezkép használata a fájl feltöltése megközelítés toodeploy.
11. Hasonló fájlfeltöltés módszert használja toofile, dönthet úgy, nem éles különböző tárhely.
12. Mentse és hello projekt felépítéséhez. Megjelenik a tároló lemezkép fejlesztőre tooyour beállításjegyzék és a webes alkalmazást telepíti.

### <a name="deploy-tooweb-app-on-linux-through-docker-using-jenkins-pipeline"></a>TooWeb Jenkins kimenetátirányításának segítségével Docker keresztül Linux alkalmazás központi telepítése

1. Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile_container_plugin** fájlt. Kattintson a gombra hello ceruza ikonra tooedit a fájl tooupdate hello erőforráscsoport és 11 és 12 sor a webalkalmazás nevét.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Sor 13 tooyour tároló beállításjegyzék kiszolgáló módosítása    
```java
def registryServer = '<registryURL>'
```    

3. Módosítsa a Jenkins példányát sor 16 tooupdate Hitelesítőadat-azonosító    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a>Jenkins folyamat létrehozása    

1. Nyissa meg webböngészővel Jenkins, kattintson **új elem**.
2. Adja meg a hello feladatnak a nevét, majd válasszon **csővezeték**. Kattintson az **OK** gombra.
3. Kattintson a hello **csővezeték** következő lap.
4. A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.
5. A **SCM**, jelölje be **Git**.
6. Adja meg a villás tárház a Githubon URL-cím hello: https:&lt;a villás tárház > .git</li>
7, frissítése **parancsfájl elérési útján** túl "Jenkinsfile_container_plugin"
8. Kattintson a **mentése** és futtatási hello feladat.

## <a name="verify-your-web-app"></a>A webalkalmazás ellenőrzése

1. tooverify hello WAR-fájl sikeresen telepítve lett tooyour webalkalmazás. Nyisson meg egy webböngészőt.
2. Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping látja:    
     Üdvözli a webes alkalmazás tooJava!!! A rendszer frissíti!
   Sun jún 17 16:39:10 UTC 2017
3. Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y        
    ![A Számológép: hozzáadása](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a>Az App service Linux rendszeren

* tooverify, az Azure parancssori felület futtassa:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    A következő eredmény hello elérhetővé:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping. Hello üzenet jelenik meg: 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y
    
## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello Azure App Service beépülő modul toodeploy tooAzure használja.

Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Azure App Service segítségével FTP Jenkins toodeploy konfigurálása 
> * Linux-rendszeren keresztül Docker Jenkins toodeploy tooAzure App Service konfigurálása 
