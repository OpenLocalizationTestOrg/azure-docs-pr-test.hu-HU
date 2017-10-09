---
title: "az Azure-telepítés hibák aaaUnderstand |} Microsoft Docs"
description: "Ismerteti, hogyan toolearn Azure telepítési hibái."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "központi telepítési hiba, az azure-telepítés tooazure központi telepítése"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: a335e121e9b908a763374907e34b1f6e823d6e96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-deployment-errors"></a>Az Azure-telepítés hibák megismerése
Ez a témakör ismerteti a telepítési hibákat, és hogyan felderíthetők a hibával kapcsolatos további információk. A megoldások toocommon telepítési hibákat, tekintse meg a [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Két típusú hibák
A hibák fogadhat két típusa van:

* Érvényesítési hiba
* Telepítési hibák

a következő kép hello hello tevékenységnapló-előfizetéshez tartozó jeleníti meg. Azt jelenti, hogy a két üzemelő példány. Egy központi telepítési hello sablon ellenőrzése nem sikerült (**ellenőrzése**), és nem haladt. A hello másik üzemelő példány, hello sablon érvényesítési átadott, de nem sikerült hello erőforrások létrehozásakor (**írási központi telepítések**). 

![hibakód megjelenítése](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Érvényesítési hibák merülnek fel a szolgáltatásokat, amelyek a központi telepítés előtt meg lehet határozni. Szintaktikai hibákat a sablont, vagy túllépné az előfizetés kvóták közben toodeploy erőforrások tartoznak. Telepítési hibák feltételek hello telepítési folyamat során előforduló merülhetnek fel. Ezek tartalmazzák próbált tooaccess párhuzamosan telepített erőforrás.

Mindkét típusú hibákat visszatérési tootroubleshoot hello telepítési használt hibakódot. Mindkét típusú hibák jelennek meg hello [tevékenységnapló](resource-group-audit.md). Azonban érvényesítési hiba jelenik meg a telepítési előzményeket mert hello telepítési soha nem indult el.

## <a name="determine-error-code"></a>Határozza meg a hiba kódja

Megismerheti a hiba bármikor hello hibaüzenet és hello hibakód. Hello [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md) cikkben megoldások hibakóddal leírni. Ez a témakör bemutatja, hogyan toouse hello Azure portál toodiscover hello hibakód.

### <a name="validation-errors"></a>Érvényesítési hiba

Hello portálon keresztül telepítésekor érvényesítési hiba láthatja az értékek elküldése után.

![portál érvényesítési hibaüzenet megjelenítése](./media/resource-manager-troubleshoot-tips/validation-error.png)

Válassza ki a üdvözlőüzenetére további részleteket. A kép a következő hello, tekintse meg az **InvalidTemplateDeployment** hiba, és egy házirend jelző üzenet blokkolva központi telepítés.

![érvényesítési részletek megjelenítése](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Telepítési hibák

Amikor a hello művelet ellenőrzése sikeres, de a telepítés során nem sikerül, hello értesítések hello hiba látható. Válassza ki a hello értesítést.

![értesítési hiba](./media/resource-manager-troubleshoot-tips/notification.png)

Megjelenik a hello fejlesztésével kapcsolatos további részletekért. Válassza ki a hello beállítás toofind hello hibával kapcsolatban további információt.

![központi telepítése nem sikerült](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Hello üzenet és a hiba hibakódok láthatja. Figyelje meg, hogy van két hibakódok. első hibakód hello (**"deploymentfailed"**) egy általános hiba, amely nem biztosít hello adatokat meg kell toosolve hello hiba. második hibakód hello (**StorageAccountNotFound**) részletesen bemutatja a hello van szüksége. 

![Hiba legutolsó részletes adatai](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>A hibakeresési naplózást engedélyező
Néha kell hello kérelem-válasz toodiscover további információt a probléma okát. PowerShell vagy Azure CLI segítségével kérheti, hogy további információkat kerül a központi telepítés során.

- PowerShell

   A PowerShellben, állítsa be a hello **DeploymentDebugLogLevel** paraméter tooAll, ResponseContent vagy RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Vizsgálja meg a hello kérés tartalma a következő parancsmag hello:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Vagy a tartalom válasz hello:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Ezen információk segítségével meghatározhatja, hogy hello sablonban lévő érték helytelen beállítása.

- Azure CLI

   Vizsgálja meg a hello üzembe helyezési műveleteinek a hello a következő parancsot:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Beágyazott sablon

   egy beágyazott sablon használata hello toolog hibakeresési információ **debugSetting** elemet.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Hibaelhárítási-sablon létrehozása
Bizonyos esetekben hello legegyszerűbb módja tootroubleshoot a sablon tootest része. Létrehozhat egy egyszerűsített sablont, amely lehetővé teszi toofocus hello részre, amelyekről hello hiba okozza. Tegyük fel, hogy hiba azért kapta, való hivatkozáskor erőforrás. Ahelyett, hogy egy teljes sablon foglalkozik, hozzon létre egy sablont, amely a problémát okozó hello részét adja vissza. Ez segít meghatározni, hogy átadott hello jobb paraméterek, a sablon függvénnyel megfelelően, és hello erőforrás lekérése várt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Vagy tegyük fel, hogy a központi telepítési hibái, amelyekről tooincorrectly függőségek beállítása kapcsolódó áll kapcsolatban. Tesztelje a sablon ossza egyszerűsített sablonok. Először hozzon létre egy sablont, amely csak egyetlen erőforrás (például egy SQL Server) telepít. Ha meggyőződött arról, hogy helyesen definiálva erőforrást is tartalmaz, amelyektől függ (például egy SQL-adatbázis) erőforrás hozzáadása. Ha két erőforrások helyesen definiálva van, adja hozzá a más függő erőforrások (például naplózási házirendek). Minden egyes próbatelepítés Between hello erőforrás csoport toomake meg arról, hogy Ön megfelelően tesztelési hello függőségek törlése. 

## <a name="check-deployment-sequence"></a>Feladatütemezési központi telepítés ellenőrzése

Sok központi telepítési hiba fordulhat elő, amikor egy váratlan sorozat erőforrások telepítése. Ezeket a hibákat okozhat, ha a függőségek nem megfelelően van beállítva. Hiányzik egy szükséges függőséget, amikor egy erőforrást próbál toouse értékét, egy másik erőforrás, de más hello még nem létezik. Hibaüzenet arról, hogy egy erőforrás nem található. Hiba az ilyen típusú felmerülhet időnként mivel hello központi telepítéskor az egyes erőforrások változhat. Például az első kísérlet toodeploy az erőforrások sikeres lesz, mert egy szükséges erőforrás véletlenszerűen idő alatt fejeződik be. A második próbálkozás azonban sikertelen lesz, mert hello szükséges erőforrás nem fejeződött be időben. 

De azt szeretné, hogy tooavoid beállítás függőségek esetén nem szükséges. Ha szükségtelen függőségek, erőforrások, amelyek nincsenek a párhuzamos telepített egymástól függenek, hogy meghosszabbítása hello telepítési hello időtartama. Ezenkívül létrehozhat, amelyek blokkolják az hello telepítési körkörös függőségek. Hello [hivatkozás](resource-group-template-functions-resource.md#reference) függvény hoz létre egy implicit függőség hello hivatkozott erőforrás hello erőforrás telepítésekor ugyanazt a sablont. Ezért, akkor elképzelhető, hogy több mint hello megadott hello függőségek **dependsOn** tulajdonság. Hello [resourceId](resource-group-template-functions-resource.md#resourceid) függvény létrehozása egy implicit függőség, vagy nem ellenőrzi, hogy létezik-e hello erőforrás.

A függőségi problémákat tapasztal, meg kell toogain betekintést erőforrások telepítése hello sorrendjét. telepítési műveletek tooview hello sorrendje:

1. Válassza ki a hello üzembe helyezési előzményeket az erőforráscsoport számára.

   ![Válassza ki a központi telepítés előzményei](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Jelölje ki a telepítést az hello előzményeket, és válassza ki **események**.

   ![Válassza ki a központi telepítési eseményeket](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Vizsgálja meg az egyes erőforrások események sorozatát hello. Nagy figyelmet toohello állapota minden egyes művelet. Például hello következő kép bemutatja, amelynek a telepítése három storage-fiókok párhuzamosan. Figyelje meg, hogy három tárfiókok hello: hello indulnak el egyszerre.

   ![Párhuzamos üzembe helyezés](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   hello következő kép bemutatja a három tárfiókok nem telepített párhuzamosan. hello második tárfiók hello első tárfiókon, valamint hello harmadik tárfiók hello második tárfiók függ. Ezért hello első tárfiók elkezdődött, elfogadott, és befejeződött hello mellett megkezdése előtt.

   ![szekvenciális központi telepítés](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

Még a bonyolultabb esetek is használhat ugyanazon módszer toodiscover hello, ha a központi telepítés elkezdődött és befejeződött az egyes erőforrások. Nézze át a központi telepítési események toosee, ha hello feladatütemezési különbözik attól a teheti meg. Ha igen, újra kiértékelje hello függőségek ehhez az erőforráshoz.

Erőforrás-kezelő körkörös függőségi viszony azonosítja a sablon érvényesítése során. Egy hibaüzenet arról, hogy kifejezetten körkörös függőség létezik adja vissza. Körkörös függőség toosolve:

1. A sablonban meghatározott hello körkörös függőség hello erőforrás található. 
2. Az adott erőforráshoz, vizsgálja meg a hello **dependsOn** tulajdonság és bármely használatát hello **hivatkozás** erőforrások attól függ, toosee működik. 
3. Vizsgálja meg az ezen erőforrások toosee, mely erőforrásokat függenek. Hajtsa végre a hello függőségek meg, amikor egy erőforrást, amely hello eredeti erőforrás függ.
5. Hello erőforrások hello körkörös függőség részt, gondosan vizsgálja meg minden használatát hello **dependsOn** tulajdonság tooidentify esetén nem szükséges összes függőséget. Távolítsa el ezeket a függőségeket. Ha biztos abban, hogy a függőség van szükség, távolítsa el azt. 
6. Hello sablon újratelepítése.

Értékek eltávolítása hello **dependsOn** tulajdonság hibákat eredményezhet, hello sablon telepítésekor. Ha hibát észlel, vegye fel a hello függőségi vissza hello sablonba. 

Ha ezt a megközelítést nem oldja meg a hello körkörös függőséget, fontolja meg a központi telepítés logikája a gyermekszintű erőforrása (például a bővítmények vagy konfigurációs beállítások). E gyermek erőforrások toodeploy konfigurálása után hello körkörös függőség hello erőforrás. Tegyük fel például, két olyan virtuális gépet telepít, de a tulajdonságok az egyes más toohello hivatkozó meg kell adni. A sorrend hello telepíthetné őket:

1. vm1
2. vm2 virtuális gépnek
3. Vm1 kiterjesztés vm1 és vm2 virtuális gépnek függ. hello bővítmény, amely azt lekérése vm2 virtuális gépnek vm1 értékek beállítása.
4. Bővítmény vm2 virtuális gépnek a vm1 és vm2 virtuális gépnek függ. hello bővítmény értékek beállítása, amely azt lekérése vm1 vm2 virtuális gépnek.

ugyanezt a megközelítést akkor működik, ha az App Service apps hello. Fontolja meg konfigurációs értékeket az alkalmazás-erőforrást hello gyermek erőforrása. Két webes alkalmazásokat telepíthet a sorrend hello:

1. webapp1
2. webapp2
3. config webapp1 webapp1 és webapp2 függ. Alkalmazásbeállítások webapp2 értékeivel tartalmaz.
4. config webapp2 webapp1 és webapp2 függ. Alkalmazásbeállítások webapp1 értékeivel tartalmaz.


## <a name="next-steps"></a>Következő lépések
* A megoldások toocommon telepítési hibákat, tekintse meg a [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).
* toolearn kapcsolatos naplózási műveletek, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).
* toolearn kapcsolatos műveletek toodetermine hello hibák központi telepítése során lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
