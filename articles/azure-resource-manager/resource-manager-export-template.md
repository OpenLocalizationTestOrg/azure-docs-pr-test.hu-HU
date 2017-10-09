---
title: Azure Resource Manager-sablon aaaExport |} Microsoft Docs
description: "Használja az Azure Resource Manager tooexport egy sablon, egy meglévő erőforráscsoportot."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: 94daa4812da2fec705044ca31c8e74e6d59bd53f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Azure Resource Manager-sablonok exportálása létező erőforrásokból
Ebből a cikkből megismerheti, hogyan tooexport a Resource Manager-sablon a meglévő erőforrást az előfizetésében. A létrehozott sablon toogain sablon szintaxisáról jobb megértése is használhatja.

Nincsenek két módon tooexport sablont:

* Exportálhatja a hello **tényleges központi telepítéshez használt sablon**. hello exportált sablon tartalmaz minden hello paraméterek és változók pontosan úgy, mint ahogy az eredeti sablon hello. Ez a módszer akkor hasznos, ha az erőforrások hello portálon keresztül telepített, és szeretné toosee hello sablon toocreate ezeket az erőforrásokat. Ez a sablon azonnal használható. 
* Exportálhatja egy **létrehozott sablont, amely hello hello erőforráscsoport aktuális állapotát jeleníti meg**. hello exportált sablon nem alapul, amely a központi telepítéshez használt sablonokat. Ehelyett az alkalmazás létrehozza a sablont, amely hello erőforráscsoportban található egy pillanatfelvétel. hello exportált sablonjának sok változtatható értékek és valószínűleg nem annyi megadott paraméterek általában határozzák meg. Ezt a módszert akkor hasznos, ha a telepítés utáni hello erőforráscsoport módosította. Ez a sablon általában módosításokat igényel, mielőtt használható lenne.

Ez a témakör bemutatja a mindkét megközelítés hello portálon keresztül.

## <a name="deploy-resources"></a>Erőforrások üzembe helyezése
Kezdjük használható sablonként exportálási erőforrások tooAzure telepítésével. Ha már van erőforráscsoport, amelyet az tooexport tooa sablon előfizetését, ez a szakasz kihagyhatja. hello a cikk hátralévő része azt feltételezi, hogy telepítette hello web app és az SQL-adatbázis megoldás ebben a szakaszban látható. Ha egy másik megoldás használja, a felhasználói élmény kissé eltérőek lehetnek, de tooexport sablon vannak hello lépéseket hello azonos. 

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **új**.
   
      ![új kiválasztása](./media/resource-manager-export-template/new.png)
2. Keresse meg **webes alkalmazás + SQL** , és jelölje ki a hello rendelkezésre álló lehetőségek közül.
   
      ![webalkalmazás és SQL keresése](./media/resource-manager-export-template/webapp-sql.png)

3. Kattintson a **Létrehozás** gombra.

      ![létrehozás kiválasztása](./media/resource-manager-export-template/create.png)

4. Adja meg szükség hello hello web app és az SQL-adatbázis. Kattintson a **Létrehozás** gombra.

      ![web és SQL érték megadása](./media/resource-manager-export-template/provide-web-values.png)

hello telepítési egy percet is igénybe vehet. Hello központi telepítés befejezése után az előfizetése tartalmazza majd hello megoldás.

## <a name="view-template-from-deployment-history"></a>Sablon megtekintése az üzembehelyezési előzményekből
1. Nyissa meg toohello erőforráscsoport panel az új erőforráscsoport számára. Figyelje meg, hogy hello hello utolsó telepítési hello eredménye megjelenik. Kattintson erre a hivatkozásra.
   
      ![erőforráscsoport panel](./media/resource-manager-export-template/select-deployment.png)
2. Megjelenik egy hello csoport telepítéseinek előzményei. Az Ön hello panel valószínűleg csak egy központi telepítési sorolja fel. Válassza ki ezt a telepítést.
   
     ![legutóbbi telepítés](./media/resource-manager-export-template/select-history.png)
3. hello panel hello telepítés összegzését jeleníti meg. összefoglaló hello hello hello központi telepítés állapotát, valamint műveleteket és hello paramétereinek megadott értékeket tartalmazza. hello központi telepítését, jelölje be a használt toosee hello sablon **sablon megtekintése**.
   
     ![telepítés összegzésének megtekintése](./media/resource-manager-export-template/view-template.png)
4. Erőforrás-kezelő kéri le a következő hét fájlok meg hello:
   
   1. **Sablon** -hello sablont, amely meghatározza a megoldás hello infrastruktúra. Hello portálon keresztül hello storage-fiók létrehozásakor a Resource Manager egy sablon toodeploy használt, és elmentette ezt a sablont későbbi felhasználás céljából.
   2. **Paraméterek** -egy paraméterfájl használható toopass értékek a telepítés során. Hello első telepítés során megadott hello értékeket tartalmaz. Módosíthatja a következő értékek hello sablon újratelepítésekor.
   3. **Parancssori felület** -Azure parancssori felület (CLI) parancsfájl toodeploy hello sablon használható.
   3. **Parancssori felület 2.0** -Azure parancssori felület (CLI) parancsfájl toodeploy hello sablon használható.
   4. **PowerShell** -toodeploy hello sablon használható az Azure PowerShell-parancsfájlt.
   5. **.NET** -toodeploy hello sablon használhatja A .NET-osztályt.
   6. **Ruby** – használható toodeploy hello sablon, A Ruby osztály.
      
      hello fájlok található hivatkozásokon keresztül érhetők hello panel között. Alapértelmezés szerint hello panel hello sablon jeleníti meg.
      
       ![sablon megtekintése](./media/resource-manager-export-template/see-template.png)
      
Ez a sablon hello tényleges sablont toocreate használni, a web app és az SQL-adatbázis. Figyelje meg, amelyek lehetővé teszik a telepítés során eltérő értékeket tooprovide paramétert tartalmaz. toolearn hello a sablonok struktúrájával kapcsolatos, kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).

## <a name="export-hello-template-from-resource-group"></a>Erőforráscsoport hello-sablonok exportálása
Ha Ön rendelkezik manuálisan az erőforrások új és módosított erőforrások több telepítések esetén, egy sablon lekérése hello üzembe helyezési előzményeket nem tükrözi hello hello erőforráscsoport aktuális állapotát. Ez a szakasz bemutatja, hogyan tooexport a sablont, amely tükrözi hello hello erőforráscsoport aktuális állapotát. 

> [!NOTE]
> Több mint 200 erőforrással rendelkező erőforráscsoport esetében nem exportálhat sablont.
> 
> 

1. az egy erőforráscsoport, jelölje be a tooview hello sablon **automatizálási parancsfájl**.
   
      ![erőforráscsoportok exportálása](./media/resource-manager-export-template/select-automation.png)
   
     Erőforrás-kezelő hello erőforrások hello erőforráscsoportban kiértékeli, és létrehoz egy sablont az erőforrásokhoz. Nem minden erőforrástípusok hello sablon függvényt. Arról, hogy nincs-e probléma hello exportálási hiba jelenhet meg. Megtudhatja, hogyan toohandle azokat problémáinak hello [javítás exportálási problémák](#fix-export-issues) szakaszban.
2. Ismét megjelenik hello hat fájlok használható tooredeploy hello megoldás. Azonban az idő hello sablon kicsit más. Figyelje meg, hogy a létrehozott sablon hello paramétert tartalmaz, kevesebb mint hello sablon az előző szakaszban. Hello értékeket (például a hely és SKU értékek) számos is változtatható egy paraméterérték elfogadása helyett ezt a sablont. Mielőtt Ez a sablon újbóli felhasználása, érdemes lehet tooedit hello sablon toomake jobb paraméterek használatával. 
   
3. Ezzel a sablonnal toowork folyamatos vonatkozó beállítások közül több lehetősége van. Hello sablon letöltése, és dolgozhat rajta egy JSON-szerkesztővel helyileg. Vagy hello tooyour Sablonkönyvtár mentheti, és dolgozhat rajta hello portálon keresztül.
   
     Ha részeként például egy JSON-szerkesztő segítségével [Visual STUDIO Code](https://code.visualstudio.com/) vagy [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), előfordulhat, hogy inkább hello sablont helyben letöltését és a szerkesztő használatát. toowork helyileg, jelölje be **letöltése**.
   
      ![sablon letöltése](./media/resource-manager-export-template/download-template.png)
   
     Ha beállította a JSON-szerkesztővel, előfordulhat, hogy inkább hello portálon keresztül hello sablon szerkesztése. hello a témakör további részei azt feltételezi, hogy a mentett hello tooyour Sablonkönyvtár hello portálon. Azonban ügyeljen a hello azonos szintaxis toohello sablon vált, hogy helyileg dolgozik, egy JSON-szerkesztő vagy hello portálon keresztül. hello portálon keresztül toowork válasszon **toolibrary hozzáadása**.
   
      ![toolibrary hozzáadása](./media/resource-manager-export-template/add-to-library.png)
   
     Egy Sablonkönyvtár toohello hozzáadásakor adjon hello sablon nevét és leírását. Ezt követően válassza a **Mentés** lehetőséget.
   
     ![sablon értékeinek megadása](./media/resource-manager-export-template/save-library-template.png)
4. Válasszon egy sablont a könyvtárban menteni tooview **további szolgáltatások**, típus **sablonok** toofilter eredményeket, jelölje be **sablonok**.
   
      ![sablonok keresése](./media/resource-manager-export-template/find-templates.png)
5. Válassza ki a mentett hello nevű hello sablont.
   
      ![sablon kiválasztása](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-hello-template"></a>Hello sablon testreszabása
elfogadható, ha azt szeretné, hogy toocreate hello azonos web app és minden üzembe helyezés SQL-adatbázis hello exportált sablon működik. A Resource Manager által nyújtott lehetőségek azonban lehetővé teszik, hogy a sablonokat rugalmasabb módon helyezze üzembe. Ez a cikk bemutatja, hogyan hello tooadd paramétereinek adatbázis-rendszergazdai nevet és jelszót. Használhatja a azonos megközelítés tooadd nagyobb rugalmasságot hello sablon más értékekre.

1. toocustomize hello sablon, jelölje be **szerkesztése**.
   
     ![sablon megjelenítése](./media/resource-manager-export-template/select-edit.png)
2. Válassza ki a hello sablont.
   
     ![sablon szerkesztése](./media/resource-manager-export-template/select-added-template.png)
3. toobe képes toopass hello értékeket, hogy a telepítés során toospecify érdemes adja hozzá a következő két paraméterek toohello hello **paraméterek** hello sablon szakaszában:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. toouse hello új paramétereket, cserélje le a hello hello SQL server-definíció **erőforrások** szakasz. Figyelje meg, hogy az **administratorLogin** és az **administratorLoginPassword** most paraméterértékeket használ.

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. Válassza ki **OK** Ha elkészült a szerkesztési hello sablont.
7. Válassza ki **mentése** toosave hello módosítások toohello sablont.
   
     ![sablon mentése](./media/resource-manager-export-template/save-template.png)
8. tooredeploy frissítése hello sablon, jelölje be **telepítés**.
   
     ![sablon üzembe helyezése](./media/resource-manager-export-template/redeploy-template.png)
9. Adja meg a paraméter értékét, és válasszon ki egy erőforrás csoport toodeploy hello erőforrást.


## <a name="fix-export-issues"></a>Az exportálással kapcsolatos problémák megoldása
Nem minden erőforrástípusok hello sablon függvényt. a probléma manuálisan tooresolve adja hozzá a hiányzó erőforrást hello újra üzembe a sablont. hello hibaüzenet hello típusok nem exportálhatók tartalmazza. Ezt az erőforrástípust a [Sablonreferenciában](/azure/templates/) találja. Például toomanually virtuális hálózati átjáró hozzáadása című [Microsoft.Network/virtualNetworkGateways sablonra való hivatkozást](/azure/templates/microsoft.network/virtualnetworkgateways).

> [!NOTE]
> Az exportálási hibák csak akkor lépnek fel, ha az erőforráscsoportból, és nem az üzembe helyezési előzmények közül végez exportálást. Ha a legutóbbi telepítés pontosan hello hello erőforráscsoport aktuális állapotát jeleníti meg, exportálnia kell hello sablon hello üzembe helyezési előzményeket és nem hello erőforráscsoportból. Ha hajtott végre módosításokat toohello erőforráscsoport egyetlen sablonban nem meghatározott csak exportálja az erőforráscsoport.
> 
> 

## <a name="next-steps"></a>Következő lépések
Megtanulta, hogyan tooexport hello portálon létrehozott erőforrásokból sablont.

* Sablont a [PowerShell](resource-group-template-deploy.md), az [Azure parancssori felülete](resource-group-template-deploy-cli.md) vagy a [REST API](resource-group-template-deploy-rest.md) használatával helyezhet üzembe.
* Hogyan tooexport egy sablon PowerShell használatával: toosee [az Azure PowerShell használata Azure Resource Managerrel](powershell-azure-resource-manager.md).
* Hogyan tooexport egy sablont az Azure parancssori felületen keresztül: toosee [használata hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](xplat-cli-azure-resource-manager.md).

