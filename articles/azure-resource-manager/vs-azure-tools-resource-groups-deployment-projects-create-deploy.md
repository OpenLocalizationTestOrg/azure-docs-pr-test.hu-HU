---
title: aaaVisual Studio Azure resource csoport projektek |} Microsoft Docs
description: "Visual Studio toocreate egy Azure erőforráscsoport-projekt használja, és hello erőforrások tooAzure telepítése."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 4bd084c8-0842-4a10-8460-080c6a085bec
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: tomfitz
ms.openlocfilehash: 672c1e71fb809b3b547f0fad30240d45de1ba923
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Azure erőforráscsoport-sablonok létrehozása és telepítése a Visual Studio alkalmazással
A Visual Studio és hello [Azure SDK](https://azure.microsoft.com/downloads/), létrehozhat egy projekt, amely az infrastruktúra és kód tooAzure telepít. Például hello webállomását, webhelyét és adatbázis meghatározása az alkalmazáshoz, és telepítheti az infrastruktúrát hello kóddal együtt. Azt is megteheti, hogy meghatározza a virtuális gépet, a virtuális hálózatot és a tárfiókot, majd telepíti az infrastruktúrát a virtuális gépen végrehajtott parancsfájllal együtt. Hello **Azure erőforráscsoport** telepítési projektje lehetővé teszi, toodeploy összes hello szükséges erőforrást egyetlen, megismételhető műveletben. Az erőforrások telepítésével és kezelésével kapcsolatos további információkért lásd: [Az Azure Resource Manager áttekintése](resource-group-overview.md).

Azure erőforráscsoport-projektek az Azure Resource Manager JSON sablonok, központi telepítését tooAzure hello erőforrások meghatározó tartalmaz. toolearn hello Resource Manager-sablon hello elemeinek kapcsolatban lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md). Visual Studio lehetővé teszi, hogy a hogy tooedit ezeket a sablonokat és eszközöket, amelyek egyszerűbbé teszik a sablonok használatának biztosít.

Ebben a cikkben egy webapp és egy SQL Database üzembe helyezésének módját ismerheti meg. Hello lépéseket rendszer azonban még majdnem hello ugyanazt a különféle típusú erőforrások. Ugyanilyen könnyen telepíthet virtuális gépeket és azok kapcsolódó erőforrásait. A Visual Studio számos különböző kezdősablont kínál a gyakori forgatókönyvek telepítéséhez.

Ez a cikk a Visual Studio 2017 szoftvert mutatja be. Ha a Visual Studio 2015 Update 2 és a Microsoft Azure SDK for .NET 2.9 használja, vagy a Visual Studio 2013 Azure SDK 2.9, a felhasználói élmény nagymértékben hello azonos. Használhatja a hello Azure SDK 2.6 vagy későbbi verziói azonban a felhasználói élmény hello felhasználói felület hello felhasználói felülete látható az ebben a cikkben eltérő lehet. Határozottan javasoljuk, hogy telepítse a legújabb verziójának hello hello [Azure SDK](https://azure.microsoft.com/downloads/) hello lépések megkezdése előtt. 

## <a name="create-azure-resource-group-project"></a>Azure erőforráscsoport-projekt létrehozása
Ebben az eljárásban egy Azure erőforráscsoport-projektet hoz létre egy **Webes alkalmazás + SQL** sablonból.

1. A Visual Studio programban válassza a **Fájl**, **Új projekt**, majd a **C#** vagy a **Visual Basic** lehetőséget. Ezután válassza a **Felhő** lehetőséget, majd az **Azure erőforráscsoport** projektet.
   
    ![Felhőtelepítési projekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)
2. Válassza ki a hello sablont, amelyet az erőforrás-kezelő toodeploy tooAzure. Nincsenek a hello alapján számos különböző lehetőség írja be a projekt, értesítés toodeploy kívánja. Ez a cikk válassza hello **webes alkalmazás + SQL** sablont.
   
    ![Sablon kiválasztása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)
   
    hello sablon csak egy kiindulási pont; Adja hozzá, és távolítsa el a forgatókönyv az erőforrások toofulfill.
   
   > [!NOTE]
   > A Visual Studio lekéri az elérhető online sablonok listáját. hello lista változhat.
   > 
   > 
   
    Visual Studio létrehoz egy erőforráscsoport-telepítési projektet hello web app és az SQL-adatbázis számára.
3. toosee mit hozott létre, keresse meg hello hello telepítési projekt csomópontjának.
   
    ![csomópontok megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)
   
    Hello webes alkalmazás + SQL sablont ehhez a példához választottuk, mert a következő fájlok hello jelenik meg: 
   
   | Fájlnév | Leírás |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Egy PowerShell-parancsfájlt, amely PowerShell-parancsok toodeploy tooAzure erőforrás-kezelő hívja.<br />**Megjegyzés:** Visual Studio a PowerShell parancsfájl toodeploy a sablont használja. A változtatások toothis parancsfájl hatással vannak a Visual Studióban üzemelő, úgyhogy legyen óvatos. |
   | WebSiteSQLDatabase.json |hello infrastruktúra tooAzure telepíteni kívánt, és a telepítés során megadható hello paramétereket meghatározó Resource Manager sablon hello. Hello erőforrások, az erőforrás-kezelő telepíti a megfelelő sorrendben hello hello erőforrások közötti hello függőségeket is meghatározza. |
   | WebSiteSQLDatabase.parameters.json |Paraméterfájl, amely hello sablonhoz szükséges értékeket tartalmazza. Adja meg a paramétert értékek toocustomize egyes központi telepítések. |
   
    Mindegyik erőforráscsoport-telepítési projekt tartalmazza ezeket az alapvető fájlokat. Más projektek további fájlokat toosupport más funkciókat tartalmazhat.

## <a name="customize-hello-resource-manager-template"></a>Hello Resource Manager sablon testreszabása
A telepítési projekteket hello toodeploy kívánt hello erőforrásokat leíró JSON sablonok módosításával szabhatja. JSON a JavaScript Object Notation rövidítése, és könnyen toowork a szerializált adatok formátuma. hello JSON-fájlok az egyes fájlok hello felső hivatkozó séma használja. Ha azt szeretné, hogy toounderstand hello séma, töltse le, és elemezze. hello séma definiálja az elemeket érvényesek, hello típusát és formátumát, a mezők hello felsorolt értékek lehetséges értékeit, és így tovább. toolearn hello Resource Manager-sablon hello elemeinek kapcsolatban lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

a sablon toowork nyissa meg a **WebSiteSQLDatabase.json**.

hello szerkesztő Visual Studio eszközök tooassist meg szerkeszteni hello Resource Manager-sablon. Hello **JSON-vázlat** ablak segítségével könnyen toosee hello elemek a sablonban meghatározott.

![JSON-vázlat megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Hello vázlat hello elemek kiválasztásával viszi hello sablon toothat része, és kiemeli a megfelelő JSON hello.

![navigálás a JSON-fájlban](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Egy erőforrás vagy kiválasztásával hello adhat **erőforrás hozzáadása** gomb hello felső hello JSON-vázlat ablak, vagy kattintson a jobb gombbal **erőforrások** , és kiválasztja **új erőforrás hozzáadása**.

![erőforrás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Ebben az oktatóanyagban válassza a **Tárfiók** lehetőséget, és adjon meg egy nevet. Olyan nevet adjon meg, amely nem hosszabb 11 karakternél, és csak számokat és kisbetűket tartalmaz.

![tároló hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Figyelje meg, hogy nem csak hello erőforrás lett hozzáadva, de is hello paramétere írja be a tárfiók, és egy változót hello hello tárfiókja nevére.

![vázlat megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Hello **storageType** paraméter előre meg határozva az engedélyezett típusokkal és az alapértelmezett típussal. Megtarthatja ezeket az értékeket, vagy módosíthatja őket az adott forgatókönyvnek megfelelően. Ha nem szeretné, hogy bárki toodeploy egy **Premium_LRS** tárfiók a sablonon keresztül, eltávolításában hello engedélyezett típusok. 

```json
"storageType": {
  "type": "string",
  "defaultValue": "Standard_LRS",
  "allowedValues": [
    "Standard_LRS",
    "Standard_ZRS",
    "Standard_GRS",
    "Standard_RAGRS"
  ]
}
```

A Visual Studio is biztosít, hogy tudomásul veszi, hogy mely tulajdonságok intellisense toohelp érhetők el, ha hello sablon szerkesztése. Például nyissa meg a tooedit hello tulajdonságai között az App Service-csomag toohello **HostingPlan** erőforrás, és adjon értéket az hello **tulajdonságok**. Figyelje meg, hogy az intellisense mutatja be a hello rendelkezésre álló értékeket, az adott értékek leírását.

![intellisense megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Beállíthat **numberOfWorkers** too1.

```json
"properties": {
  "name": "[parameters('hostingPlanName')]",
  "numberOfWorkers": 1
}
```

## <a name="deploy-hello-resource-group-project-tooazure"></a>Hello erőforráscsoport-projekt tooAzure telepítése
Meg vannak toodeploy most már készen áll a projekthez. Amikor telepít egy Azure erőforráscsoport-projekt, telepítheti azt tooan Azure erőforráscsoport. hello erőforráscsoportban, amelyek egy közös életciklussal erőforrások logikai csoportosítása.

1. Hello telepítési projekt csomópontjának hello helyi menüjében válassza **telepítés** > **új**.
   
    ![Telepítés, Új üzemelő példány menüelem](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)
   
    Hello **tooResource csoport telepítése** párbeszédpanel jelenik meg.
   
    ![Központi telepítése tooResource csoport párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)
2. A hello **erőforráscsoport** legördülő listából válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat. toocreate egy erőforráscsoport, nyissa meg hello **erőforráscsoport** legördülő listát, és válassza **hozzon létre új**.
   
    ![Központi telepítése tooResource csoport párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)
   
    Hello **erőforráscsoport létrehozása** párbeszédpanel jelenik meg. Megadhatja a csoportnak, nevét és helyét, és válassza ki a hello **létrehozása** gombra.
   
    ![Erőforráscsoport létrehozása párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
3. Hello telepítéshez hello paraméterek szerkesztése hello kiválasztásával **paraméterek szerkesztése** gombra.
   
    ![Paraméterek szerkesztése gomb](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)
4. Hello üres paraméterek értékének megadására, és válassza ki a hello **mentése** gombra. üres paraméterei hello **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**, és **databaseName**.
   
    **hostingPlanName** hello nevét határozza meg [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) toocreate. 
   
    **administratorLogin** hello SQL-kiszolgáló rendszergazdájához tartozó hello felhasználónevét határozza meg. Ne használjon olyan gyakori rendszergazdai neveket, mint az **sa** vagy az **admin**. 
   
    Hello **administratorLoginPassword** megad egy jelszót az SQL Server rendszergazdájához. Hello **jelszavak mentése egyszerű szövegként hello paraméterfájl** , nincs lehetőség biztonságos; ezért csak akkor válassza ezt a beállítást. Mivel hello jelszót a rendszer nem menti egyszerű szövegként, kell tooprovide ezt a jelszót újra üzembe helyezése során. 
   
    **databaseName** hello adatbázis toocreate nevét határozza meg. 
   
    ![Paraméterek szerkesztése párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
5. Válassza ki a hello **telepítés** gomb toodeploy hello projekt tooAzure. A PowerShell-konzolban kívül hello Visual Studio példány megnyitása. Hello PowerShell konzolon, ha a rendszer kéri, adja meg hello SQL Server rendszergazdai jelszót. **A PowerShell-konzolon más elemek se takarja vagy kis méretű hello tálcán.** Keresse meg a konzolon, és válassza ki azt a tooprovide hello jelszót.
   
   > [!NOTE]
   > A Visual Studio kérheti tooinstall hello Azure PowerShell-parancsmagokat. Meg kell hello Azure PowerShell parancsmagok toosuccessfully erőforráscsoportok telepítéséhez. Ha a program kéri, telepítse őket.
   > 
   > 
6. hello telepítés néhány percet is igénybe vehet. A hello **kimeneti** , a windows hello hello központi telepítés állapota látható. Amikor hello telepítés véget ért, hello utolsó üzenet azt jelzi, a sikeres telepítés, az alábbihoz hasonlót:
   
        ... 
        18:00:58 - Successfully deployed template 'websitesqldatabase.json' tooresource group 'DemoSiteGroup'.
7. Egy böngészőben nyissa meg a hello [Azure-portálon](https://portal.azure.com/) és jelentkezzen be tooyour fiókjával. toosee hello erőforráscsoport, jelölje be **erőforráscsoportok** és telepített hello erőforráscsoportot.
   
    ![csoport kijelölése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)
8. Minden hello telepített erőforrás megjelenik. Figyelje meg, hogy hello neve hello a tárfiók nincs pontosan milyen meg, hogy az erőforrás hozzáadása során megadott. hello tárfiók egyedinek kell lennie. hello sablon automatikusan hozzáadja a karakterlánc karakterek toohello nevű megadott tooprovide egy egyedi nevet. 
   
    ![erőforrások megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)
9. Ha a módosításokat, és szeretné, hogy a projekt tooredeploy, választhat hello Azure erőforráscsoport-projekt helyi menüjének hello meglévő erőforráscsoportot. Hello helyi menüjében válassza **telepítés**, majd válassza az üzembe helyezett hello erőforráscsoport.
   
    ![Azure erőforráscsoport üzembe helyezve](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Kód telepítése az infrastruktúrával
Ezen a ponton az alkalmazás hello infrastruktúra telepített, de nincs telepített hello projekt tényleges kód. Ez a cikk bemutatja, hogyan toodeploy egy web app és az SQL-adatbázis táblák üzembe helyezése során. A webalkalmazás helyett virtuális gépet telepít, ha azt szeretné, toorun néhány kódot hello gépen a telepítés részeként. a webalkalmazáshoz tartozó kód telepítésének folyamata hello, vagy a virtuális gép beállítását szinte hello azonos.

1. Adja hozzá a projekt tooyour Visual Studio megoldás. Kattintson a jobb gombbal a hello megoldás, és válassza ki **Hozzáadás** > **új projekt**.
   
    ![projekt hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)
2. Adjon hozzá egy **ASP.NET webalkalmazást**. 
   
    ![webalkalmazás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
3. **MVC** kiválasztása
   
    ![MVC kiválasztása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
4. Visual Studio létrehozza a webalkalmazást, miután mindkét projekt hello megoldásban láthatja.
   
    ![projektek megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)
5. Most toomake meg arról, hogy az erőforráscsoport-projekt hello új projekt tisztában van szüksége. Lépjen vissza a tooyour erőforráscsoport-projekt (AzureResourceGroup1). Kattintson jobb gombbal a **References** (Hivatkozások) elemre és válassza az **Add Reference** (Hivatkozás hozzáadása) lehetőséget.
   
    ![hivatkozás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)
6. Válassza ki a hello webalkalmazás-projekthez létrehozott.
   
    ![hivatkozás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
   
    A hivatkozás hozzáadásával hello web app projektet toohello erőforráscsoport-projekt hivatkozásra, és automatikusan beállít három kulcsfontosságú tulajdonságot. Ezeket a tulajdonságokat a hello látja **tulajdonságok** hello referenciaként ablak.
   
      ![hivatkozás megtekintése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
   
    hello tulajdonságai a következők:
   
   * Hello **további tulajdonságok** hello webes telepítési Web csomag előkészítési helyét, amely az Azure Storage toohello fejlesztőre tartalmazza. Megjegyzés: a hello mappák (ExampleApp) és (package.zip). Szüksége tooknow ezeket az értékeket mert megadnia őket, paraméterek telepítésekor hello alkalmazást. 
   * Hello **fájl elérési útjának belefoglalása** hello csomag létrehozási helyének hello elérési utat tartalmaz. Hello **célok belefoglalása** tartalmazza, amely végrehajtja a központi telepítés hello parancsot. 
   * az alapértelmezett érték hello **Build; Csomag** lehetővé teszi a központi telepítés toobuild hello, és hozzon létre egy webes telepítési csomag (package.zip).  
     
     A közzétételi profil hello telepítési folyamat hello szükséges információkat a hello tulajdonságok toocreate hello csomagból, nem kell.
7. Lépjen vissza tooWebSiteSQLDatabase.json, és vegyen fel egy erőforrás toohello sablont.
   
    ![erőforrás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)
8. Ez alkalommal válassza a **Web Deploy for Web Apps** (Webalkalmazások webes üzembe helyezése) lehetőséget. 
   
    ![webes telepítés hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
9. Telepítse újra az erőforrás csoport projekt toohello erőforráscsoport. Ez alkalommal van néhány új paraméter. Nem kell tooprovide értékeinek **_artifactsLocation** vagy **_artifactsLocationSasToken** mert Visual Studio automatikusan hozza létre ezeket az értékeket. Lehetősége van azonban tooset hello mappa és fájl toohello elérési útja, amely tartalmazza a hello központi telepítési csomagot (megjelennek az helyeként **ExampleAppPackageFolder** és **ExampleAppPackageFileName** a kép a következő hello ). Adja meg a korábban látott hello értékek hello hivatkozás tulajdonságai (**ExampleApp** és **package.zip**).
   
    ![webes telepítés hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
   
    A hello **összetevő tárfiókja**, válasszon egy erőforráscsoportot telepített hello.
10. Hello központi telepítés befejeződését követően válassza ki a webalkalmazás hello portálon. Válassza ki a hello URL-cím toobrowse toohello helyet.
    
     ![hely tallózása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)
11. Figyelje meg, hogy a hello alapértelmezett ASP.NET alkalmazás sikeres legyen-e telepítve.
    
     ![telepített alkalmazás megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Következő lépések
* az erőforrások hello portálon keresztül kezelésével kapcsolatos toolearn lásd: [Using hello Azure portál toomanage az Azure-erőforrások](resource-group-portal.md).
* További információ a sablonok, toolearn lásd [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

