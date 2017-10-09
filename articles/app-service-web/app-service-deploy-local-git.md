---
title: "aaaLocal Git-telepítésének tooAzure App Service"
description: "Megtudhatja, hogyan tooenable helyi Git telepítési tooAzure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1905e0b7acd58d8dd6496a14f6e4f38f9f8c0212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="local-git-deployment-tooazure-app-service"></a>Helyi Git-telepítésének tooAzure App Service
Az oktatóanyag bemutatja, hogyan toodeploy az alkalmazás túl[Azure App Service] a Git-tárházat a helyi számítógépen. App Service támogatja ezt a módszert a hello **helyi Git** hello a rendszerbe állítási beállításának [Azure Portal].  
Ebben a cikkben leírt hello Git-parancsok számos automatikusan megtörténik, egy App Service alkalmazáshoz hello létrehozásakor [Azure parancssori felület] leírtak [Itt](app-service-web-get-started.md).

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban szüksége:

* Git. Letöltheti a bináris hello telepítési [Itt](http://www.git-scm.com/downloads).  
* Git alapszintű ismeretét.
* Egy Microsoft Azure-fiók. Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű alkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.  
> 
> 

## <a name="Step1"></a>1. lépés: A helyi tárház létrehozása
Hajtsa végre a következő feladatok toocreate új Git-tárház hello.

1. Indítsa el a parancssori eszköz, például a **GitBash** (Windows) vagy **Bash** (Unix rendszerhéj). OS X rendszereken érheti el parancssori hello hello **Terminálszolgáltatások** alkalmazás.
2. Keresse meg a toohello könyvtár, amelyben hello tartalom toodeploy volna található.
3. A következő parancs tooinitialize új Git-tárház hello használata:

```bash  
git init
```

## <a name="Step2"></a>2. lépés: A tartalom véglegesítése
App Service számos programozási nyelven készült alkalmazások támogatja. 

1. Ha a tárház már tartalmazza a tartalom kihagyása a pont, és helyezze át az alábbi 2 toopoint. Ha a tárház már tartalmazza a tartalom egyszerűen feltölteni egy statikus .html fájllal az alábbiak szerint: 
   
   * Egy szövegszerkesztő használatával hozzon létre egy új fájlt **index.html** hello Git-tárház hello gyökérkönyvtárában
   * Adja hozzá a hello hello index.html hello tartalmának fájlt, és mentse a következő szöveget: *Hello Git!*
2. A parancssori hello ellenőrizze, hogy a Git-tárház hello gyökere alatt. Kövesse a következő parancs tooadd fájlok tooyour tárház hello:

```bash  
git add -A
```
3. Ezt követően véglegesítse hello módosítások toohello tárház hello a következő parancs használatával:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>3. lépés: Az App Service alkalmazás tárház hello engedélyezése
Hajtsa végre a következő lépéseket tooenable egy App Service-alkalmazás Git-tárházat hello.

1. Jelentkezzen be toohello [Azure Portal].
2. Az App Service-alkalmazás paneljén kattintson **beállítások > központi telepítés forrásának**. Kattintson a **forrás választása**, majd kattintson a **helyi Git-tárház**, és kattintson a **OK**.  
   
    ![Helyi Git-tárház](./media/app-service-deploy-local-git/local_git_selection.png)
3. Ha ez az első idő beállítása a tárházat az Azure-ban, hozzá kell toocreate bejelentkezési hitelesítő adatokat. Szüksége lesz őket az Azure-tárházat hello toolog, és küldje el a módosításokat a helyi Git-tárház a. Az alkalmazás paneljén kattintson **beállítások > üzembe helyezési hitelesítő adatok**, majd konfigurálja a központi telepítés felhasználónevét és jelszavát. Amikor elkészült, kattintson a **mentése**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>4. lépés: A projekt telepítése
A következő lépéseket toopublish hello használata az alkalmazás tooApp helyi Git-szolgáltatást.

1. A hello Azure portálra az alkalmazás paneljén kattintson **beállítások > Tulajdonságok** a hello **Git URL-cím**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **Git URL-cím** hello távoli referencia toodeploy toofrom van a helyi tárházzal. Az alábbi lépésekkel hello URL-címet fogja használni.
2. Parancssori hello használ, győződjön meg arról, hogy a helyi Git-tárház hello gyökerében vannak-e.
3. Használjon `git remote` tooadd hello távoli leírásában felsorolt **Git URL-cím** az 1. lépés. A parancshoz hasonló toohello következő fog kinézni:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > Hello **távoli** parancs hozzáadása egy elnevezett hivatkozás tooa távoli tárházba. Ebben a példában a webalkalmazás-tárház "azure" nevű hivatkozást hoz létre.
   > 
   > 
4. A tartalom tooApp szolgáltatás Push használatával új hello **azure** távoli újonnan létrehozott.

```bash  
git push azure master
```
    You will be prompted for hello password you created earlier when you reset your deployment credentials in hello Azure Portal. Enter hello password (note that Gitbash does not echo asterisks toohello console as you type your password). 
5. Lépjen vissza a tooyour alkalmazás hello Azure portálon. A legutóbbi leküldéses a naplóbejegyzést kell megjelennie hello **központi telepítések** panelen. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Kattintson a hello **Tallózás** hello app panel tooverify hello tartalom hello tetején gomb van telepítve. 

## <a name="Step5"></a>Hibaelhárítás
hello az alábbiakban láthatók a hibák vagy problémák gyakran fordul elő, ha a Git toopublish tooan App Service alkalmazás használatával az Azure-ban:

- - -
**Jelenség**: "[siteURL]" nem tooaccess: nem sikerült túl tooconnect [scmAddress]

**OK**: Ez a hiba akkor fordulhat elő, ha hello alkalmazás nem válik működőképessé.

**Megoldási**: Start hello alkalmazás hello Azure portálon. Git-telepítés nem fog működni, ha fut a hello alkalmazás. 

- - -
**Jelenség**: nem sikerült feloldani a gazdagép "állomásnév"

**OK**: Ez a hiba akkor fordulhat elő, ha hello címadatok létrehozásakor megadott hello "azure" távoli helytelen volt.

**Megoldási**: használata hello `git remote -v` toolist parancsot minden távvezérlő együtt hello tartozó URL-CÍMÉT. Ellenőrizze, hogy helyes-e hello hello "azure" távoli URL-címe. Ha szükséges, távolítsa el és hozza létre újból a távoli hello helyes URL-cím használatával.

- - -
**Jelenség**: nincs közös refs, és nincs megadva; semmi sem történt. Lehet, hogy adja meg például a "master" ág.

**OK**: Ez a hiba akkor fordulhat elő, ha nem adja meg egy ágat, ha a git push művelet végrehajtása, és rendelkezik nem hello push.default érték beállítása Git használják.

**Megoldási**: hello főághoz megadó hello leküldéses megismételni a műveletet, hajtsa végre. Példa:

```bash  
git push azure master
```
- - -
**Jelenség**: src refspec [branchname] nem felel meg egyik sem.

**OK**: Ez a hiba akkor fordulhat elő, ha toopush tooa fiókirodai hello "azure" távoli a főadatbázison kívül.

**Megoldási**: hello főághoz megadó hello leküldéses megismételni a műveletet, hajtsa végre. Példa:

```bash  
git push azure master
```
- - -
**Jelenség**: RPC sikertelen; a eredménye = 22, HTTP-kód = 502-es.

**OK**: A hiba akkor fordulhat elő, ha toopush egy nagy git-tárház HTTPS-KAPCSOLATON keresztül.

**Megoldási**: hello hello helyi számítógép toomake hello postBuffer nagyobb a git konfigurációjának módosítása

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Jelenség**: hiba - módosítások véglegesítése tooremote tárház azonban a webalkalmazás nem frissül.

**OK**: Ez a hiba akkor fordulhat elő, ha telepít egy Node.js-alkalmazás, amely meghatározza a további szükséges modulokat package.json fájlt tartalmazó.

**Megoldási**: további üzeneteket tartalmazó "npm hiba!" a korábbi naplózott toothis hiba legyen, és képes szolgáltatáskészletének hello hiba esetén. hello következő a hiba oka ismert, és megfelelő "npm hiba!" hello üzenet:

* **Nem megfelelően formázott package.json fájl**: npm hiba! Nem lehetett olvasni a függőségek.
* **Natív modul, amely nem rendelkezik Windows bináris terjesztési**:
  
  * npm hiba! \`cmd "/ c" "csomópont-gyp rebuild"\` sikertelen 1
    
      VAGY
  * npm hiba! [modulename@version] preinstall: \`ellenőrizze || gmake\`

## <a name="additional-resources"></a>További források
* [A Git dokumentációja](http://git-scm.com/documentation)
* [Project Kudu dokumentációja](https://github.com/projectkudu/kudu/wiki)
* [Folyamatos telepítés tooAzure App Service](app-service-continuous-deployment.md)
* [Hogyan toouse az Azure PowerShell](/powershell/azure/overview)
* [Hogyan toouse hello Azure parancssori felület](../cli-install-nodejs.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure parancssori felület]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
