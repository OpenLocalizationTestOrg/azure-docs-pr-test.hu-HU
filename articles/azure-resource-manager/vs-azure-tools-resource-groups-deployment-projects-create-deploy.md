---
title: "Azure erőforráscsoportokkal kapcsolatos Visual Studio-projektek | Microsoft Docs"
description: "A Visual Studio használatával Azure erőforráscsoport-projekteket hozhat létre, és telepítheti az erőforrásokat az Azure rendszerbe."
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
ms.openlocfilehash: d647206b882059e0651223dc84f2ad2a314f8a87
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/18/2017
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Azure erőforráscsoport-sablonok létrehozása és telepítése a Visual Studio alkalmazással
A Visual Studio és az [Azure SDK](https://azure.microsoft.com/downloads/) alkalmazással olyan projekteket hozhat létre, amelyekkel telepíthető az infrastruktúra és kód az Azure rendszerbe. Meghatározhatja például az alkalmazás webállomását, webhelyét és adatbázisát, továbbá telepítheti az infrastruktúrát a kóddal együtt. Azt is megteheti, hogy meghatározza a virtuális gépet, a virtuális hálózatot és a tárfiókot, majd telepíti az infrastruktúrát a virtuális gépen végrehajtott parancsfájllal együtt. Az **Azure erőforráscsoport** telepítési projektje lehetővé teszi, hogy az összes szükséges erőforrást egyetlen, megismételhető műveletben telepítse. Az erőforrások telepítésével és kezelésével kapcsolatos további információkért lásd: [Az Azure Resource Manager áttekintése](resource-group-overview.md).

Az Azure-erőforráscsoport-projektek az Azure Resource Managerből származó JSON-sablonokat tartalmaznak, amelyek az Azure-ba telepítendő erőforrásokat határozzák meg. A Resource Manager-sablon elemeivel kapcsolatos információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md). A Visual Studio lehetővé teszi a sablonok szerkesztését, valamint eszközeivel egyszerűbbé teszi a sablonokkal való munkát.

Ebben a cikkben egy webapp és egy SQL Database üzembe helyezésének módját ismerheti meg. A lépések azonban majdnem teljesen azonosak az eltérő típusú erőforrások esetében is. Ugyanilyen könnyen telepíthet virtuális gépeket és azok kapcsolódó erőforrásait. A Visual Studio számos különböző kezdősablont kínál a gyakori forgatókönyvek telepítéséhez.

Ez a cikk a Visual Studio 2017 szoftvert mutatja be. Amennyiben a Visual Studio 2015 2. frissítését és a Microsoft Azure SDK for .NET 2.9-et használja vagy a Visual Studio 2013-as verzióját Azure SDK 2.9-cel, a tapasztalt működés nagyjából azonos lesz. Használhatja az Azure SDK 2.6-os vagy újabb verzióját is, azonban ebben az esetben a felhasználói felület eltérhet a cikkben leírtaktól. Az [Azure SDK](https://azure.microsoft.com/downloads/) legújabb verziójának telepítése erősen ajánlott a lépések megkezdése előtt. 

## <a name="create-azure-resource-group-project"></a>Azure erőforráscsoport-projekt létrehozása
Ebben az eljárásban egy Azure erőforráscsoport-projektet hoz létre egy **Webes alkalmazás + SQL** sablonból.

1. A Visual Studio programban válassza a **Fájl**, **Új projekt**, majd a **C#** vagy a **Visual Basic** lehetőséget (az, hogy melyik nyelvet választja, nincs hatással a későbbi szakaszokra, mert ezek a projektek csak JSON- és PowerShell-tartalmat tartalmaznak). Ezután válassza a **Felhő** lehetőséget, majd az **Azure erőforráscsoport** projektet.
   
    ![Felhőtelepítési projekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)
2. Válassza ki az Azure Resource Managerbe telepíteni kívánt sablont. Figyelje meg, hogy a telepíteni kívánt projekt típusától függően számos különböző lehetőség áll rendelkezésre. Ehhez a cikkhez válassza a **Webapp + SQL** sablont.
   
    ![Sablon kiválasztása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)
   
    A kiválasztott sablon csak egy kiindulási pont. A forgatókönyvnek való megfelelés érdekében hozzáadhat és eltávolíthat erőforrásokat.
   
   > [!NOTE]
   > A Visual Studio lekéri az elérhető online sablonok listáját. A lista tartalma változhat.
   > 
   > 
   
    A Visual Studio létrehoz egy erőforráscsoport-telepítési projektet a webalkalmazás és az SQL-adatbázis számára.
3. A létrehozott elemek megtekintéséhez tekintse meg az üzembe helyezési projektben található csomópontot.
   
    ![csomópontok megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)
   
    Mivel a Webalkalmazás + SQL sablont választottuk ehhez a példához, az alábbi fájlok láthatóak: 
   
   | Fájlnév | Leírás |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |PowerShell-parancsfájl, amely PowerShell-parancsokat hív meg az Azure Resource Manager telepítéséhez.<br />**Megjegyzés** A Visual Studio ezt a PowerShell-parancsfájlt használja a sablon telepítéséhez. A parancsfájlon végrehajtott módosítások hatással vannak a Visual Studióban végzett telepítésre is, ezért legyen óvatos. |
   | WebSiteSQLDatabase.json |Az Azure szolgáltatásban telepíteni kívánt infrastruktúrát, valamint a telepítés során megadható paramétereket meghatározó Resource Manager-sablon. A telepített erőforrások közti függőségeket is meghatározza, így a Resource Manager megfelelő sorrendben telepíti azokat. |
   | WebSiteSQLDatabase.parameters.json |Paraméterfájl, amely a sablonhoz szükséges értékeket tartalmazza. Megadhat paraméterértékeket, amelyekkel testre szabhatóak az egyes telepítések. |
   
    Mindegyik erőforráscsoport-telepítési projekt tartalmazza ezeket az alapvető fájlokat. Más projektek további fájlokat is tartalmazhatnak, egyéb funkciók támogatásához.

## <a name="customize-the-resource-manager-template"></a>A Resource Manager-sablon testreszabása
A telepítési projekteket a telepíteni kívánt erőforrásokat leíró JSON sablonok módosításával szabhatja testre. A JSON a JavaScript Object Notation rövidítése, és egy könnyen kezelhető szerializált adatformátum. A JSON-fájlok az egyes fájlok tetején hivatkozott sémát használják. Amennyiben szeretné megismerni a sémát, töltse le, és elemezze. A séma meghatározza az érvényes elemeket, a mezők típusát és formátumát, a felsorolt értékek lehetséges értékeit stb. A Resource Manager-sablon elemeivel kapcsolatos információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

A munkához nyissa meg a **WebSiteSQLDatabase.json** sablont.

A Visual Studio szerkesztő eszközöket biztosít a Resource Manager-sablon szerkesztéséhez. A **JSON-vázlat** ablak segítségével könnyen áttekinthetőek a sablonban meghatározott elemek.

![JSON-vázlat megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

A vázlatban lévő egyes elemek kiválasztásával a rendszer a sablon adott részére ugrik, és kiemeli a megfelelő JSON-struktúrát.

![navigálás a JSON-fájlban](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Hozzáadhat egy új erőforrást a JSON-vázlat ablak tetején található **Erőforrás hozzáadása** gomb kiválasztásával, vagy kattintson a jobb gombbal az **erőforrások** elemre, és válassza az **Új erőforrás hozzáadása** lehetőséget.

![erőforrás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Ebben az oktatóanyagban válassza a **Tárfiók** lehetőséget, és adjon meg egy nevet. Olyan nevet adjon meg, amely nem hosszabb 11 karakternél, és csak számokat és kisbetűket tartalmaz.

![tároló hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Figyelje meg, hogy nem csupán az erőforrás lett hozzáadva, hanem a tárfiók típusának paramétere is, valamint egy változó a tárfiók nevével.

![vázlat megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

A **storageType** paraméter előre meg van határozva az engedélyezett típusokkal és az alapértelmezett típussal egyetemben. Megtarthatja ezeket az értékeket, vagy módosíthatja őket az adott forgatókönyvnek megfelelően. Ha nem szeretné, hogy bárki **Premium_LRS** tárfiókot hozzon létre a sablonon keresztül, egyszerűen törölje azt az engedélyezett típusok közül. 

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

A Visual Studio intellisense szolgáltatása segítségével megtudhatja, milyen tulajdonságok érhetőek el a sablon szerkesztése során. Például az App Service-csomag tulajdonságainak szerkesztéséhez lépjen a **HostingPlan** erőforrásra, és adjon meg egy értéket a **properties** elemnél. Figyelje meg, hogy az intellisense megjeleníti az elérhető értékeket, valamint az adott értékek leírását.

![intellisense megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Állítsa a **numberOfWorkers** paraméter értékét 1-re.

```json
"properties": {
  "name": "[parameters('hostingPlanName')]",
  "numberOfWorkers": 1
}
```

## <a name="deploy-the-resource-group-project-to-azure"></a>Az Erőforráscsoport-projekt telepítése az Azure szolgáltatásban
Készen áll a projekt telepítésére. Az Azure Erőforráscsoport-projekt telepítésekor egy Azure-erőforráscsoporton helyezi üzembe azt. Az erőforráscsoport közös életciklussal rendelkező erőforrások logikai csoportja.

1. Az üzembe helyezési projekt csomópontjának helyi menüjén válassza a **Telepítés** > **Új** lehetőséget.
   
    ![Telepítés, Új üzemelő példány menüelem](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)
   
    Megjelenik a **Telepítés erőforráscsoportra** párbeszédpanel.
   
    ![Telepítés erőforráscsoportra párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)
2. Az **Erőforráscsoport** legördülő listából válasszon ki egy létező erőforráscsoportot, vagy hozzon létre egy újat. Erőforráscsoport létrehozásához nyissa le az **Erőforráscsoport** legördülő listát, és válassza az **Új létrehozása** elemet.
   
    ![Telepítés erőforráscsoportra párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)
   
    Megjelenik az **Erőforráscsoport létrehozása** párbeszédpanel. Adjon meg egy nevet és egy helyet a csoport számára, és kattintson a **Létrehoz** gombra.
   
    ![Erőforráscsoport létrehozása párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
3. Az üzemelő példány paramétereit a **Paraméterek szerkesztése** gombra kattintva adhatja meg.
   
    ![Paraméterek szerkesztése gomb](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)
4. Adja meg az üres paraméterek értékeit, majd kattintson a **Mentés** gombra. Az üres paraméterek a következők: **hostingPlanName**, **administratorLogin**, **administratorLoginPassword** és **databaseName**.
   
    A **hostingPlanName** a létrehozandó új [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nevét adja meg. 
   
    Az **administratorLogin** az SQL Server rendszergazdájának felhasználónevét adja meg. Ne használjon olyan gyakori rendszergazdai neveket, mint az **sa** vagy az **admin**. 
   
    Az **administratorLoginPassword** az SQL Server rendszergazdájának jelszavát adja meg. A **Jelszavak mentése egyszerű szövegként a paraméterfájlban** lehetőség nem biztonságos, így ezt ne használja. Mivel a jelszót nem egyszerű szövegként menti, az üzembe helyezés során újra meg kell adnia ezt a jelszót. 
   
    A **databasePlanName** a létrehozandó új adatbázis nevét adja meg. 
   
    ![Paraméterek szerkesztése párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
5. Kattintson a **Telepítés** gombra a projekt telepítéséhez az Azure szolgáltatásban. Megnyílik egy PowerShell-konzol a Visual Studio-példányon kívül. Amikor a rendszer kéri, adja meg az SQL Server rendszergazdai jelszavát a PowerShell-konzolon. **Lehetséges, hogy a PowerShell-konzolt egyéb elemek eltakarják, vagy kis méretben fut a tálcán.** Keresse meg és nyissa meg a konzolt a jelszó megadásához.
   
   > [!NOTE]
   > Előfordulhat, hogy a Visual Studio megkéri, hogy telepítse az Azure PowerShell-parancsmagokat. Az erőforráscsoportok sikeres üzembe helyezéséhez szükség van az Azure PowerShell-parancsmagokra. Ha a program kéri, telepítse őket.
   > 
   > 
6. Az üzembe helyezés eltarthat néhány percig. A **Kimenet** ablakban követhető az üzembe helyezés állapota. Az üzembe helyezés befejeztével az utolsó üzenet jelzi az üzembe helyezés sikerességét, a következőhöz hasonló módon:
   
        ... 
        18:00:58 - Successfully deployed template 'websitesqldatabase.json' to resource group 'DemoSiteGroup'.
7. Egy böngészőben nyissa meg az [Azure Portalt](https://portal.azure.com/), és jelentkezzen be a fiókjával. Az erőforráscsoport megtekintéséhez válassza az **Erőforráscsoportok** lehetőséget, valamint az erőforráscsoportot, amelyiken a telepítést végezte.
   
    ![csoport kijelölése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)
8. Az összes telepített erőforrás megjelenik. Figyeljen arra, hogy a tárfiók neve nem pontosan az, amit az erőforrás hozzáadásakor megadott. A tárfiók nevének egyedinek kell lennie. Ahhoz, hogy a név egyedi legyen, a sablon automatikusan hozzáad egy karakterláncot a megadott névhez. 
   
    ![erőforrások megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)
9. Amennyiben módosításokat végez, és újra el kívánja végezni a projekt telepítését, a meglévő erőforráscsoportot kiválaszthatja az Azure erőforráscsoport-projekt helyi menüjéből. A helyi menüben válassza a **Telepítés** lehetőséget, majd válassza ki a telepített erőforráscsoportot.
   
    ![Azure erőforráscsoport üzembe helyezve](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Kód telepítése az infrastruktúrával
Ezen a ponton az alkalmazás infrastruktúrája már telepítve van, tényleges kód azonban még nincs telepítve a projekttel. Ez a cikk a webappok és az SQL Database-táblák üzembe helyezésének módját ismerteti az üzembe helyezés során. Amennyibe webalkalmazás helyett virtuális gépet telepít, a telepítés keretében valamennyi kódot is érdemes futtatni a gépen. A kód telepítésének folyamata a webalkalmazások és a virtuális gépek telepítésénél szinte teljesen megegyezik.

1. Adjon hozzá egy projektet a Visual Studio megoldásához. Kattintson a jobb gombbal a megoldásra, és válassza az **Add** > **New Project** (Hozzáadás – Új projekt) parancsot.
   
    ![projekt hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)
2. Adjon hozzá egy **ASP.NET webalkalmazást**. 
   
    ![webalkalmazás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
3. **MVC** kiválasztása
   
    ![MVC kiválasztása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
4. Miután a Visual Studio létrehozta a webalkalmazást, a megoldásban mindkét projekt megjelenik.
   
    ![projektek megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)
5. Most győződjön meg arról, hogy az erőforráscsoport észleli az új projektet. Lépjen vissza az erőforráscsoport-projekthez (AzureResourceGroup1). Kattintson jobb gombbal a **References** (Hivatkozások) elemre és válassza az **Add Reference** (Hivatkozás hozzáadása) lehetőséget.
   
    ![hivatkozás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)
6. Válassza ki a webalkalmazás-projektet, amelyet létrehozott.
   
    ![hivatkozás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
   
    A hivatkozás hozzáadásával a webalkalmazás-projektet összekapcsolja az erőforráscsoport-projekttel, és automatikusan beállít három kulcsfontosságú tulajdonságot. Ezek a tulajdonságok a hivatkozáshoz tartozó **Properties** (Tulajdonságok) ablakban láthatók.
   
      ![hivatkozás megtekintése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
   
    A tulajdonságok a következők:
   
   * A **További tulajdonságok** a webes telepítési web csomag előkészítési helyét tartalmazza, amely át lett helyezve az Azure Storage-ba. Jegyezze meg a mappa (ExampleApp) és a fájl (package.zip) nevét. Ismernie kell ezeket az értékeket, mert az alkalmazás üzembe helyezésekor meg kell adni őket paraméterekként. 
   * A **Fájl elérési útjának belefoglalása** azt az útvonalat tartalmazza, ahol a csomag létrejött. A **Célok belefoglalása** a telepítés során végrehajtott parancsot tartalmazza. 
   * Az alapértelmezett **Build;Csomag** érték egy webes telepítési csomag (package.zip) felépítését és létrehozását teszi lehetővé.  
     
     Közzétételi profil nem szükséges, mivel a telepítési folyamat a szükséges információkat a csomag létrehozásához használt tulajdonságokból meríti.
7. Lépjen vissza a WebSiteSQLDatabase.json fájlhoz, és adjon hozzá egy erőforrást a sablonhoz.
   
    ![erőforrás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)
8. Ez alkalommal válassza a **Web Deploy for Web Apps** (Webalkalmazások webes üzembe helyezése) lehetőséget. 
   
    ![webes telepítés hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
9. Végezze el ismét az erőforráscsoport-projekt telepítését az erőforráscsoporton. Ez alkalommal van néhány új paraméter. Az **_artifactsLocation** vagy **_artifactsLocationSasToken** paraméterek értékét nem kell megadnia, mivel a Visual Studio ezeket automatikusan osztja ki. Azonban a mappa és a fájl nevét az üzembe helyezési csomag elérési útvonalának megfelelően kell megadni (ahogy az a következő képen a **ExampleAppPackageFolder** és **ExampleAppPackageFileName** paramétereknél látható). Adja meg a korábban a hivatkozás tulajdonságai közt látott értékeket (**ExampleApp** és **package.zip**).
   
    ![webes telepítés hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
   
    Az **Artifact storage account** (Összetevő tárfiókja) paraméternél válassza az adott erőforráscsoporttal üzembe helyezett tárfiókot.
10. Az üzembe helyezés befejeztével válassza ki a webalkalmazást a portálon. Válassza ki az URL-t a hely böngészéséhez.
    
     ![hely tallózása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)
11. Láthatja, hogy az alapértelmezett ASP.NET-alkalmazást sikeresen üzembe helyezte.
    
     ![telepített alkalmazás megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Következő lépések
* Az erőforrásoknak a portálon keresztül történő kezelésével kapcsolatos információkért lásd: [Az Azure Portal használata az Azure erőforrások kezeléséhez](resource-group-portal.md).
* A sablonokkal kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

