---
title: "az Azure App Service aaaAgile szoftverfejlesztői"
description: "Megtudhatja, hogyan toocreate jelentősen méretezhető összetett alkalmazások az Azure App Service oly módon, amely támogatja a gyors szoftverfejlesztői."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: c0fdb676-36a6-4738-925f-65b4835d187f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: a1c1c78cfff711774943b0235ed762f03f48fc6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="agile-software-development-with-azure-app-service"></a>Agilis szoftverfejlesztői az Azure App Service
Ebből az oktatóanyagból megtudhatja, hogyan toocreate jelentősen méretezhető összetett alkalmazások [Azure App Service](/azure/app-service/) oly módon, amely támogatja a [gyors szoftverfejlesztői](https://en.wikipedia.org/wiki/Agile_software_development). Azt feltételezi, hogy már tudja, hogyan túl[kiszámítható módon tudja az Azure-ban összetett alkalmazások központi telepítése](app-service-deploy-complex-application-predictably.md).

Korlátozások a műszaki folyamatokban gyakran állhatnak gyors módszereit, amelyek sikeres végrehajtásának hello módon. Az Azure App Service funkcióinak [folyamatos közzététel](app-service-continuous-deployment.md), [átmeneti környezet](web-sites-staged-publishing.md) (tárolóhelyek), és [figyelési](web-sites-monitor.md), ha a georeplikációt alapján kialakulhat hello vezénylési és a központi telepítés kezelése [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), fejlesztők számára bevezetni a gyors szoftverfejlesztői kiválóan része lehet.

hello következő táblát kell gyors fejlesztési társított követelményeknek rövid listáját, és hogyan Azure-szolgáltatások engedélyezése hozzájuk.

| Követelmény | Hogyan Azure lehetővé teszi, hogy |
| --- | --- |
| -Build a minden egyes véglegesítés után<br>-Hozza létre automatikusan és gyors |Ha be van állítva a folyamatos üzembe helyezés, Azure App Service fejlesztői ág alapján élő futó buildek tudnak funkcionálni. Minden alkalommal, amikor a kód fejlesztőre toohello fiókirodai, esetén automatikusan beépített és futó működés közben az Azure-ban. |
| – Ellenőrizze hoz létre önálló tesztelése |Betölteni a teszteket, webes tesztjeinek használatát, stb., hello Azure Resource Manager-sablon üzembe helyezhetők. |
| -Tesztek kerülnek végrehajtásra az éles környezet másolat |Az Azure Resource Manager-sablonok hello Azure éles környezet (beleértve az alkalmazásbeállítások, a kapcsolati karakterlánc sablonok, a méretezés, a stb), gyors és kiszámítható módon tudja a teszteléshez használt toocreate klónok lehet. |
| -Megtekintése legújabb buildjével eredményét könnyen |Folyamatos üzembe helyezés tooAzure a tárházból azt jelenti, hogy tesztelheti új kód egy élő alkalmazásban a véglegesítés után azonnal. |
| -Véglegesítése toohello főágba minden nap<br>-A központi telepítés automatizálása |A tárház főágba az éles alkalmazások folyamatos integrációt minden véglegesítési/egyesítés toohello főágba tooproduction automatikusan telepíti. |

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Mit fog
Rendelés toopublish új módosítások toohello tipikus fejlesztői teszt-szakasz-éles munkafolyamata keresztül végighaladhat [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) mintaalkalmazás, amely két [webalkalmazások](/services/app-service/web/), egyik időtúllépést (FE) és hello másik egy webes API háttéralkalmazás (BE), és egy [SQL-adatbázis](/services/sql-database/). A következő üzembe helyezési architektúrája hello fog dolgozni:

![](./media/app-service-agile-software-development/what-1-architecture.png)

tooput hello kép szavak be:

* hello üzembe helyezési architektúrája van osztva a három különböző környezetben (vagy [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md) az Azure-ban), saját [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [skálázás](web-sites-scale.md) beállítások, és az SQL-adatbázis. 
* Minden környezet külön-külön lehet kezelni. Azok még léteznek, a másik előfizetést.
* Átmeneti és üzemi használják, mint a hello két üzembe helyezési ponti ugyanahhoz az App Service alkalmazáshoz. hello főághoz van beállítva a folyamatos integrációt hello átmeneti tárolóhely.
* A véglegesítési toomaster fiókirodai a tárolóhely (termelési adatokkal) átmeneti hello ellenőrzését követően hello ellenőrzése átmeneti alkalmazást az éles tárolóhelyre hello van cserélve [állásidő nélkül](web-sites-staged-publishing.md).

hello üzemi és átmeneti környezet induló hello sablon [  *&lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

fejlesztői hello és hello sablon határozzák meg a tesztkörnyezetek [  *&lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Hello tipikus elágaztatási stratégia, kóddal tér át hello fejlesztői fiókirodai toohello teszt fiókirodai be, majd toohello főághoz (áthelyezése a minőség, így toospeak) is hasznos.

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-need"></a>Mi szükséges
* Az Azure-fiók
* A [GitHub](https://github.com/) fiók
* Git rendszerhéj (telepített [GitHub for Windows](https://windows.github.com/))-lehetővé teszi, hogy toorun mindkét hello Git és a PowerShell-parancsok a hello azonos munkamenet 
* Legújabb [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) bits
* A következő eszközök hello alapvető ismeretekkel:
  * [Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sablon-üzembehelyezés (lásd még: [kiszámítható módon tudja az Azure-ban összetett alkalmazást központilag](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Ez az oktatóanyag kell egy Azure-fiók toocomplete:
> 
> * Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokkal, és még azok lejárta után hálózati adaptere esetében megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például a Web Apps.
> * Is [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -a Visual Studio-előfizetéssel biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.
> 
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="set-up-your-production-environment"></a>Az éles környezet beállítása
> [!NOTE]
> Ebben az oktatóanyagban használt automatikusan hello parancsfájl a GitHub-tárház folyamatos közzététel konfigurálása. Ehhez az szükséges, hogy a Githubon tartozó hitelesítő adatok már szerepelnek Azure, hello egyébként parancsprogrammal létrehozva a központi telepítés sikertelen lesz a tooconfigure verziókövetési beállítások hello web Apps megkísérlése során. 
> 
> a GitHub-felhasználó hitelesítő adatait az Azure toostore egy webalkalmazás létrehozása az hello [Azure-portálon](https://portal.azure.com/) és [GitHub-KözpontiTelepítés konfigurálása](app-service-continuous-deployment.md). Csak akkor kell toodo ezen egyszer. 
> 
> 

Egy tipikus DevOps forgatókönyv valamely alkalmazás az élő Azure-ban futó, és azt szeretné, hogy toomake módosítások tooit folyamatos közzétételen keresztül;. Ebben a forgatókönyvben rendelkezzen, hogy Ön fejlett, tesztelése és használt toodeploy hello éles környezetben. Beállításához nyújt útmutatást, ebben a szakaszban.

1. Hozzon létre saját elágazás a hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárházba. Létrehozásával kapcsolatos információkat az elágazáshoz, tekintse meg a [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/). Ha létrejött az elágazáshoz, tekintheti meg a böngészőben.
   
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Nyisson meg egy Git rendszerhéj-munkamenetet. Ha a Git rendszerhéj még nincs, telepítse [GitHub for Windows](https://windows.github.com/) most.
3. Az elágazáshoz, helyi másolat létrehozása a következő hello a következő parancs futtatásával:

        git clone https://github.com/<your_fork>/ToDoApp.git 
4. Ha elvégezte a helyi klónozás, keresse meg túl*&lt;repository_root >*\ARMTemplates és futtatási hello deploy.ps1 parancsfájlt az alábbiak szerint:
   
        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git
5. Amikor a rendszer kéri, írja be hello szükséges felhasználónevet és jelszót az adatbázis eléréséhez.
   
   Meg kell jelennie a jogosultságkiosztás folyamata különböző Azure-erőforrások hello. Központi telepítés befejezése után hello parancsfájl elindítja hello böngészőben hello alkalmazást, és adjon meg egy rövid hangjelzés.
   
    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
   
   > [!TIP]
   > Vessen egy pillantást  *&lt;repository_root >*\ARMTemplates\Deploy.ps1, toosee hogyan hoz létre az egyedi azonosítóval rendelkező erőforrásokat. Használhatja ugyanazon megközelítés toocreate klónokat a hello hello azonos környezetben anélkül, hogy ütköző erőforrás nevét.
   > 
   > 
6. Vissza a Git rendszerhéj-munkamenetben futtassa:
   
        .\swap –Name ToDoApp<unique_string>master
   
    ![](./media/app-service-agile-software-development/production-4-swap.png)
7. Hello parancsfájl befejezése után térjen vissza toobrowse toohello előtér-cím (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) toosee hello alkalmazást éles környezetben.
8. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és vessen egy pillantást mi jön létre.
   
   Meg kell tudni toosee két webes alkalmazások hello ugyanabban az erőforráscsoportban, amelyen a hello `Api` utótag hello nevében. Hello erőforráscsoport nézet tekinti meg, ha is látni hello SQL-adatbázis és a kiszolgáló, a hello App Service-csomag és a hello átmeneti üzembe helyezési ponti hello webes alkalmazásokhoz. Tallózzon a hello különböző erőforrások, és hasonlítsa össze azokat a  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee konfigurációjuktól hello sablonban.
   
    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Most már készen hello éles környezetben. A következő lesz indítsa el egy új frissítési toohello alkalmazást.

## <a name="create-dev-and-test-branches"></a>Fejlesztői és az ágakkal rendelkező
Most, hogy egy éles Azure-ban futó összetett alkalmazások, egy frissítés tooyour alkalmazás megfelelően gyors módszert fog készíteni. Ebben a szakaszban hello fejlesztési és tesztelési elágazásokat, hogy szüksége lesz a toomake hello szükséges frissítések hoz létre.

1. Először hozza létre a hello tesztkörnyezetben. A Git rendszerhéj-munkamenetben futtassa hello következő parancsok toocreate hello környezetet egy új fiókirodai nevű **NewUpdate**. 
   
        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate
2. Amikor a rendszer kéri, írja be hello szükséges felhasználónevet és jelszót az adatbázis eléréséhez. 
   
   Központi telepítés befejezése után hello parancsfájl elindítja hello böngészőben hello alkalmazást, és adjon meg egy rövid hangjelzés. Most már rendelkezik egy új fiókirodai saját tesztkörnyezetben. Egy rövid ideig tooreview kapcsolatos a tesztkörnyezet néhány dolgot érvénybe:
   
   * Létrehozhat bármely Azure-előfizetésben. Ez azt jelenti, hogy hello éles környezetben is kezelhető külön-külön a tesztkörnyezetben.
   * A tesztkörnyezetben fut. élő Azure-ban.
   * A tesztkörnyezet azonos toohello éles környezetben, kivéve az átmeneti üzembe helyezési ponti hello és beállítások skálázás hello. Akkor tudja mert ProdandStage.json és Dev.json csak különbségei hello.
   * A tesztkörnyezet felügyelheti a saját App Service-csomagot, és egy másik ár réteg (például **szabad**).
   * A tesztkörnyezet törlése nem egyszerűen hello erőforráscsoport törlése. Hogyan található toodo ez [később](#delete).
3. Nyissa meg a toocreate futtatásával fejlesztői ág hello a következő parancsokat:
   
        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev
4. Amikor a rendszer kéri, írja be hello szükséges felhasználónevet és jelszót az adatbázis eléréséhez. 
   
   Egy rövid ideig tooreview igénybe vehet néhány dolgot a Fejlesztői környezetről: 
   
   * A fejlesztői környezet rendelkezik egy konfigurációs azonos toohello tesztkörnyezetben, mert hello segítségével telepítve van ugyanazt a sablont.
   * Minden egyes fejlesztői környezet hello fejlesztői saját Azure-előfizetéssel, hagyja hello tesztelési környezetben toobe elkülönítve is létrehozható.
   * A fejlesztői környezetében fut élő Azure-ban.
   * Törlés hello fejlesztői környezete egyszerűen hello erőforráscsoport törlése. Hogyan található toodo ez [később](#delete).

> [!NOTE]
> Ha egyszerre több fejlesztők hello új frissítési, azok könnyen létrehozhat egy fiókiroda és dedikált fejlesztői környezet az alábbi lépésekkel hello:
> 
> 1. A saját elágazás hello tárház létrehozása a Githubon (lásd: [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/)).
> 2. A helyi számítógép klónozott hello elágazás
> 3. Hello futtatása azonos parancsokat toocreate saját fejlesztői ág és környezet.
> 
> 

Amikor elkészült, a GitHub elágazás három ágak kell rendelkezniük:

![](./media/app-service-agile-software-development/test-1-github-view.png)

És a három különálló erőforráscsoportok rendelkeznie kell hat web apps (két-három készlet):

![](./media/app-service-agile-software-development/test-2-all-webapps.png)

> [!NOTE]
> ProdandStage.json megadja hello éles környezetben toouse hello **szabványos** IP-címek, amelyek megfelelő méretezhetőséget biztosít a hello éles alkalmazásokban.
> 
> 

## <a name="build-and-test-every-commit"></a>Összeállításához és teszteléséhez minden egyes véglegesítés után
hello sablonfájlokat ProdAndStage.json és Dev.json már hello forrás vezérlő paraméterek megadása, amely alapértelmezés szerint állítja be hello webalkalmazás folyamatos közzététel. Ezért minden véglegesítés toohello GitHub fiókirodai elindítja egy automatikus központi telepítési tooAzure a a fiókirodában. Nézzük meg, hogy a telepítő most már működik.

1. Győződjön meg arról, hogy Ön hello fejlesztői ágában hello helyi tárház. toodo a, futtassa a következő parancsot a Git rendszerhéj hello:
   
        git checkout Dev
2. Ellenőrizze a módosítás toohello alkalmazások felhasználói felületi réteg hello kód toouse módosításával [rendszerindítási](http://getbootstrap.com/components/) sorolja fel. Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml és később hello kiemelt módosítása a következő:
   
    ![](./media/app-service-agile-software-development/commit-1-changes.png)
   
    > [!NOTE]
    > Ha nem tudja olvasni a fenti kép hello: 
    > 
    > * 18 sorban módosítsa `check-list` túl`list-group`.
    > * 19 sorban módosítsa `class="check-list-item"` túl`class="list-group-item"`.
    > 
    > 
3. Hello módosítás mentéséhez. Vissza a Git Rendszerhéjában futtassa a következő parancsok hello:
   
        cd <repository_root>
        git add .
        git commit -m "changed toobootstrap style"
        git push origin Dev
   
   A git-parancsok hasonlóak túl "ellenőrzése a kódban" egy másik vezérlő forrásrendszerben TFS hasonlóan. Amikor futtatja `git push`, hello új véglegesítési elindítja az automatikus kód leküldéses tooAzure, mely majd újraépíti hello alkalmazás tooreflect hello módosítás hello fejlesztői környezetben.
4. a kód leküldéses tooyour fejlesztői környezet történt, tooverify app weblap tooyour fejlesztői környezetben nyissa meg, és nézze meg hello **telepítési** része. A legújabb véglegesítési üzenetet nem tud toosee kell lennie.
   
    ![](./media/app-service-agile-software-development/commit-2-deployed.png)
5. Itt kattintson **Tallózás** toosee hello módosítást az Azure-ban hello élő alkalmazásban.
   
    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)
   
   A kismérvű változás toohello alkalmazások is. Azonban számos új módosítások tooa összetett webalkalmazás időpontokat kell nem szándékos és nem kívánatos hatásai. Képes tooeasily vizsgálat folyamatban minden egyes véglegesítés után élő buildekben lehetővé teszi, hogy Ön toocatch ezeket a problémákat, mielőtt a felhasználók lássák.

Mostanra, tisztában kell lennie a hello megvalósítása, amely a hello fejlesztőként **NewUpdate** projekt, a fejlesztési környezet létrehozása a szolgáltatást, majd minden egyes véglegesítés után létrehozhatja és minden build tesztelése.

## <a name="merge-code-into-test-environment"></a>Kód egyesítése tesztkörnyezetben
Amikor készen áll a kód a fejlesztői fiókirodai mentése tooNewUpdate fiókirodai toopush, hello szabványos git folyamat:

1. Bármely új véglegesítések tooNewUpdate egyesítése hello fejlesztői fiókirodai a Githubon, például más fejlesztők által létrehozott véglegesíti. A Githubon bármely új véglegesítési elindítja a kód leküldéses és -buildek hello fejlesztői környezetben. Majd biztosíthatja a kód fejlesztői ágában továbbra is működik hello legújabb bitként a NewUpdate ág.
2. Az új véglegesíti a fejlesztői fiókirodai egyesítése NewUpdate fiókirodai a Githubon. Ez a művelet egy kódot leküldéses és -buildek hello tesztkörnyezetben váltja ki. 

Ne feledje, hogy a git ággal rendelkező folyamatos üzembe helyezés már be van állítva, mert nem kell más műveletek futtatása integrációs azonos buildek tootake. Tooperform szabványos forrás vezérlő eljárások git használatával egyszerűen, és az Azure-ban minden hello összeállítási folyamat kidolgozásához.

Most tegyük küldje be a kódot túl**NewUpdate** ág. A Git Rendszerhéjában futtassa a következő parancsok hello:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Készen is van. 

Lépjen toohello weblap alkalmazás esetében a tesztelési környezet toosee (egyesített NewUpdate ág) az új véglegesítési most leküldött toohello tesztkörnyezetben. Kattintson a **Tallózás** , stílusának módosítása hello toosee most már fut az élő Azure-ban.

## <a name="deploy-update-tooproduction"></a>Frissítés tooproduction telepítése
Kód toohello küldését átmeneti és üzemi környezetben kell érzi, hogy mi már megtette kód toohello tesztkörnyezetben leküldött, amikor nem egyezik. Fontos, hogy valóban egyszerű. 

A Git Rendszerhéjában futtassa a következő parancsok hello:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Ne feledje, hogy átmeneti és üzemi környezetben hello beállítása ProdandStage.json hello módja alapján, az új kódot fejlesztőre toohello **átmeneti** tárolóhely, és nem működik. Ha toohello előkészítési pont URL-cím keresse meg, akkor látni hello új kódot futtató van. toodo a, a következő parancsmag Git rendszerhéj futtatási hello.

    Start-Process -FilePath "http://ToDoApp<unique_string>master-Staging.azurewebsites.net"

Most, miután ellenőrizte, hogy a tárhely átmeneti hello hello frissítés, hello csak dolog maradt toodo tooswap és az éles környezetben. A Git Rendszerhéjában futtassa a következő parancsok hello:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Gratulálunk! Sikeres közzététel frissítés tooyour éles új webalkalmazást. Ennél az, hogy adott azt könnyen fejlesztési és tesztkörnyezetek létrehozása és felépítése, és minden egyes véglegesítés után tesztelése. Ezek a gyors szoftverfejlesztői kritikus fontosságú építőelemeit.

<a name="delete"></a>

## <a name="delete-dev-and-test-environments"></a>Törlése fejlesztési és tesztelési környezetben
Mivel szándékosan rendelkezik tervezett a fejlesztői és teszt környezetek toobe önálló erőforrás csoport, akkor egyszerűen toodelete őket. Ebben az oktatóanyagban hello GitHub elágazásokat, mind az Azure összetevők létrehozott ők toodelete hello egyszerűen csak futtatnia kell a következő parancsokat a Git rendszerhéj hello:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Összefoglalás
Agilis szoftverfejlesztői egy számos vállalat számára, akik az alkalmazás platformként Azure tooadopt must-van. Ebben az oktatóprogramban megtanulhatta, hogyan toocreate és pontos replikák le vagy replikáinak könnycsepp hello éles környezetben az egyszerű, még a összetett alkalmazások. Rendelkezik is megtanulta, hogyan tooleverage a képes toocreate fejlesztői feldolgozni, felépítéséhez, és minden egyetlen véglegesítési tesztelése az Azure-ban. Ez az oktatóanyag azt remélhetőleg mutatja meg leginkább használatát Azure App Service és az Azure Resource Manager együttes toocreate tooagile módszereit, amelyek caters DevOps megoldást. A következő hozhat létre ebben a forgatókönyvben a DevOps technikák például speciális elvégzésével [tesztelése éles környezetben](app-service-web-test-in-production-get-start.md). Közös tesztelése éles telepítési forgatókönyvhöz, lásd: [(bétatesztelés) az Azure App Service Flighting telepítési](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>További erőforrások
* [Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése](app-service-deploy-complex-application-predictably.md)
* [A gyakorlatban gyors fejlesztési: tippek és trükkök korszerűsített fejlesztési ciklus](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
* [Speciális központi telepítésének stratégiák Azure Web Apps Resource Manager-sablonok segítségével](http://channel9.msdn.com/Events/Build/2015/2-620)
* [Az Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - hello JSON-érvényesítő](http://jsonlint.com/)
* [ARMClient – GitHub közzétételi toosite beállítása](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
* [Git ugorhat – alapvető ugorhat és egyesítése](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [David Ebbo Blog](http://blog.davidebbo.com/)
* [Azure PowerShell](/powershell/azure/overview)
* [Az Azure platformfüggetlen parancssori eszközök](../cli-install-nodejs.md)
* [Felhasználók létrehozása vagy szerkesztése az Azure ad-ben](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
* [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)

