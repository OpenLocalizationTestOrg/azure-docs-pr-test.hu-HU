---
title: "aaaExecute hello Jenkins az Azure parancssori Felülettel |} Microsoft Docs"
description: Ismerje meg, hogyan toouse Azure CLI toodeploy Java web app tooAzure Jenkins folyamat
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 4bd1e12e6de1f010453ff51c835f84e7361962f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-and-hello-azure-cli"></a>TooAzure Jenkins az App Service telepítése és hello Azure parancssori felület
a Java web app tooAzure toodeploy, használhatja az Azure CLI [Jenkins csővezeték](https://jenkins.io/doc/book/pipeline/). Ebben az oktatóanyagban létrehoz egy CI/CD folyamat egy Azure virtuális gépen történő is beleértve:

> [!div class="checklist"]
> * Jenkins virtuális gép létrehozása
> * Jenkins konfigurálása
> * Webalkalmazás létrehozása az Azure-ban
> * A GitHub-tárházban előkészítése
> * Jenkins folyamat létrehozása
> * Hello folyamat fut, és ellenőrizze a hello webalkalmazás

Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb. toofind hello verzió, futtassa `az --version`. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Hozzon létre és Jenkins konfigurálása
Ha még nem rendelkezik egy Jenkins master, kezdje hello [Megoldássablonban](install-jenkins-solution-template.md), mely tartalmazza a szükséges hello [Azure hitelesítő adatok](https://plugins.jenkins.io/azure-credentials) beépülő modul alapértelmezés szerint. 

hello Azure hitelesítő adat beépülő modul lehetővé teszi toostore Microsoft Azure szolgáltatás egyszerű hitelesítő adatait a Jenkins. 1.2-es verziója, az azt támogatása hello úgy, hogy Jenkins csővezeték beszerezheti hello Azure hitelesítő adataival. 

Győződjön meg arról, hogy 1.2-es vagy újabb verziót:
* Belül hello Jenkins irányítópultján kattintson **Jenkins kezelése beépülő modul Manager -> ->** keresse meg a **Azure hitelesítő**. 
* Frissítse a hello beépülő modult, ha hello verziója korábbi, mint 1.2-es.

Hello Jenkins fő Java JDK és Maven is szükséges. tooinstall, jelentkezzen be tooJenkins fő SSH használatával, és futtassa a következő parancsok hello:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-toojenkins-credential"></a>Az Azure szolgáltatás egyszerű tooJenkins hitelesítő adatok hozzáadása

Meg kell szükséges tooexecute Azure parancssori felület egy Azure hitelesítő adatait.

* Belül hello Jenkins irányítópultján kattintson **hitelesítő adatok -> rendszer ->**. Kattintson a **globális credentials(unrestricted)**.
* Kattintson a **adja hozzá a hitelesítő adatok** tooadd egy [Microsoft Azure szolgáltatás egyszerű](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont hello kitöltésével. Adja meg a Azonosítót az ezt követő lépés felhasználásra.

![Adja hozzá a hitelesítő adatok](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-hello-java-web-app"></a>Az Azure App Service üzembe helyezéséhez hello Java-webalkalmazás létrehozása

Hozzon létre az Azure App Service-csomag hello **szabad** hello segítségével tarifacsomag [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) CLI parancsot. hello App Service-csomagot határoz meg hello használt fizikai erőforrások toohost az alkalmazásokat. App Service-csomagot tooan hozzárendelt összes alkalmazás ossza meg ezeket az erőforrásokat, így toosave költség több alkalmazás esetén. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Amikor készen áll a hello terv, hello Azure CLI hasonló látható kimeneti toohello a következő példa:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Azure-webalkalmazás létrehozása

 Használjon hello [az webalkalmazás létrehozása](/cli/azure/appservice/web#create) CLI parancs toocreate egy web app-definíciót a hello `myAppServicePlan` App Service-csomag. hello web app-definíciót tartalmaz egy URL-cím tooaccess az alkalmazásba, és konfigurálja a több beállítások toodeploy a kód tooAzure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Helyettesítő hello `<app_name>` helyőrző a saját egyedi alkalmazás nevével. A egyedi név része hello alapértelmezett tartomány nevét hello webalkalmazás, így hello nevének kell toobe egyedi az Azure-ban minden alkalmazások között. Ki kell alakítani az tooyour felhasználók hozzárendelhető egy egyéni tartomány nevét bejegyzés toohello webalkalmazást.

Amikor készen áll a hello web app-definíciót, hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Java konfigurálása 

Hello Java runtime konfiguráció, amelyet az alkalmazás a hello [az App Service web config frissítés](/cli/azure/appservice/web/config#update) parancsot.

hello következő parancsot konfigurálása hello web app toorun a legújabb Java 8 JDK és [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>A GitHub-tárházban előkészítése
Nyissa meg hello [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) tárházban. toofork hello tárház tooyour saját GitHub-fiók, kattintson a hello **elágazás** hello jobb felső sarkában található gombra.

* Nyissa meg a GitHub webes felhasználói felület, **Jenkinsfile** fájlt. Kattintson a gombra hello ceruza ikonra tooedit a fájl tooupdate hello erőforráscsoport és a sor 20 és 21 a webalkalmazás nevét.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Módosítsa a Jenkins példányát sor 23 tooupdate Hitelesítőadat-azonosító

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Jenkins folyamat létrehozása
Nyissa meg webböngészővel Jenkins, kattintson **új elem**. 

* Adja meg a hello feladatnak a nevét, majd válasszon **csővezeték**. Kattintson az **OK** gombra.
* Kattintson a hello **csővezeték** következő lap. 
* A **Definition**, jelölje be **parancsfájl az SCM-feldolgozási folyamat**.
* A **SCM**, jelölje be **Git**.
* Adja meg a villás tárház a Githubon URL-cím hello: https:\<a villás tárház\>.git
* Kattintson a **mentése**

## <a name="test-your-pipeline"></a>A folyamat tesztelése
* Nyissa meg a létrehozott toohello csővezeték, kattintson a **Build most**
* Néhány másodpercen belül a build sikeres legyen, és toohello build megnyithatja, és kattintson a **konzol kimeneti** toosee hello részletei

## <a name="verify-your-web-app"></a>A webalkalmazás ellenőrzése
tooverify hello WAR-fájl sikeresen telepítve lett tooyour webalkalmazás. Nyisson meg egy webböngészőt:

* Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/ping  
Lásd:

        Welcome tooJava Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Nyissa meg toohttp: / /&lt;alkalmazás_neve >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (helyettesítse &lt;x > és &lt;y > bármely számokkal) tooget hello számának összege, azaz x és y

![A Számológép: hozzáadása](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-tooazure-web-app-on-linux"></a>TooAzure Linux webalkalmazás telepítése
Most, hogy tudja, hogyan toouse a Jenkins az Azure CLI-feldolgozási, hello parancsfájl toodeploy tooan Azure Web Apps Linux módosíthatja.

Webes alkalmazás Linux egy másik módja toodo hello környezetben, amely toouse Docker támogatja. toodeploy, tooprovide egy Dockerfile, amely csomagokat a webalkalmazás szolgáltatás futtatási idő mellett a Docker-lemezkép szükséges. hello beépülő modul fog majd hello lemezkép, tooa Docker beállításjegyzék leküldeni és hello kép tooyour webalkalmazás telepítése.

* Hello lépésekkel [Itt](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate egy Linux rendszeren futó Azure webalkalmazás.
* A Jenkins példányán, ezen hello utasításokat követve telepítse Docker [cikk](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Egy tároló beállításjegyzék hello Azure-portálon a hello lépések használatával hozható létre [Itt](/azure/container-registry/container-registry-get-started-azure-cli).
* Az azonos hello [egyszerű Azure Java-webalkalmazás](https://github.com/azure-devops/javawebappsample) ágazik el, hogy tárház szerkesztése hello **Jenkinsfile2** fájlt:
    * 18-21. sor, illetve frissíteni az erőforráscsoport, a web app és az ACR toohello nevei. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Sor 24, a frissítés \<azsrvprincipal\> tooyour Hitelesítőadat-azonosító
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Hozzon létre egy új Jenkins folyamat tooAzure webes alkalmazás a Windows rendszerben csak megadott idő használata telepítése hasonló módon **Jenkinsfile2** helyette.
* Az új feladat futtatása.
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
Ebben az oktatóanyagban egy Jenkins folyamatot, amely ellenőrzi a kimenő hello forráskód a GitHub-tárház konfigurálva. Maven toobuild futtatja a war-fájl, és ezután használja az Azure CLI toodeploy tooAzure App Service. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Jenkins virtuális gép létrehozása
> * Jenkins konfigurálása
> * Webalkalmazás létrehozása az Azure-ban
> * A GitHub-tárházban előkészítése
> * Jenkins folyamat létrehozása
> * Hello folyamat fut, és ellenőrizze a hello webalkalmazás
