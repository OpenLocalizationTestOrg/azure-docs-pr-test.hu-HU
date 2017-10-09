---
title: "a bemeneti paraméterek aaaRunbook |} Microsoft Docs"
description: "Runbook bemeneti paraméterek rugalmasabb hello a runbookok lehetővé teszik, hogy toopass adatok tooa runbook indításakor. Ez a cikk ismerteti a különböző alkalmazási helyzetek, amely bemeneti paraméterek runbookok használják."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: f3abaf92382e7d41019616bafb14af23cf98dd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-input-parameters"></a>Runbook bemeneti paraméterei
Runbook bemeneti paraméterek rugalmasabb hello a runbookok lehetővé teszik, hogy toopass adatok tooit indításakor. hello paraméterek segítségével hello runbook műveletek toobe érvényes az adott forgatókönyvekben és környezetekben. Ez a cikk azt haladhat végig különböző alkalmazási helyzetek runbookok helyének bemeneti paraméterek.

## <a name="configure-input-parameters"></a>A bemeneti paraméterek konfigurálása
A bemeneti paramétereket a PowerShell, a PowerShell-munkafolyamatok és a grafikus forgatókönyvek konfigurálható. Egy runbook is rendelkezik különböző adattípusokkal több paramétert, vagy nincsenek paraméterei egyáltalán. Kötelező vagy választható lehet bemeneti paramétereket, és hozzá lehet rendelni egy választható paramétereik alapértelmezett értéke. Megadhat értéket toohello bemeneti paraméterek egy runbook hello rendelkezésre álló módszerek egyikével indításakor. Ezek a metódusok lehetnek egy runbook indítása hello portal vagy webszolgáltatás futhat. Akkor is elindíthatja, mint amit a gyermekrunbook beágyazottan is nevezett egy másik runbook.

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a>Konfigurálhatja a bemeneti paramétereket a PowerShell és a PowerShell-munkafolyamati forgatókönyvek
PowerShell és [PowerShell munkafolyamat-forgatókönyvekről](automation-first-runbook-textual.md) az Azure Automationben támogatja a következő attribútumok hello keresztül meghatározott bemeneti paraméterek.  

| **Tulajdonság** | **Leírás** |
|:--- |:--- |
| Típus |Kötelező. hello adattípus hello paraméter értéket vár. A .NET-típus érvénytelen. |
| Név |Kötelező. hello hello paraméter neve. Ez kell hello runbookon belüli egyedi és is csak betűket, számokat, vagy aláhúzás karaktereket tartalmazhat. Betűvel kell kezdődnie. |
| Kötelező |Választható. Meghatározza, hogy egy érték hello paramétert meg kell adni. Ha ez túl**$true**, akkor meg kell adni egy értéket, hello runbook indításakor. Ha ez túl**$false**, majd egy értéket nem kötelező megadni. |
| Alapértelmezett érték |Választható.  Adja meg egy értéket, amely hello paraméter alkalmazva lesznek, ha az érték nem kerül át a hello runbook indításakor. Alapértelmezett érték bármelyik paraméternél állítható be, és automatikusan teszik hello paraméter függetlenül hello kötelező beállítás nem kötelező. |

Windows PowerShell támogatja a bemeneti paraméterek további attribútumok azokat az itt felsorolt, érvényesítése, például aliasokat, és a paraméter állandóként állítja be. Azure Automation jelenleg csak hello bemeneti paraméterek fent felsorolt támogatja.

A paraméter PowerShell munkafolyamat-forgatókönyvekről van hello következő általános űrlapot, ahol több paraméter vesszővel kell elválasztani.

   ```
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> Ha amely akkor paraméterek, ha nem adja meg a hello **kötelező** attribútum, akkor alapértelmezés szerint hello paraméter megadása nem kötelező. Is, ha a paraméter alapértelmezett értékét a PowerShell munkafolyamat-forgatókönyvekről, azt minősül PowerShell, nem kötelező paraméter, függetlenül attól, hello **kötelező** attribútum értéke.
> 
> 

Tegyük fel pedig konfiguráljuk hello bemeneti paramétereinek egy PowerShell-munkafolyamati forgatókönyv, amely virtuális gépeket, vagy egy virtuális vagy az erőforráscsoporton belül minden virtuális gép adatait. Ez a forgatókönyv két paraméterrel rendelkezik, ahogy az alábbi képernyőfelvétel a hello: hello nevét, valamint a virtuális gép hello hello erőforráscsoport nevét.

![Automatizálás PowerShell-munkafolyamat](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

Ez a paraméter-definícióban hello paraméterek **$VMName** és **$resourceGroupName** egyszerű paraméterek karakterlánc típusú. Azonban PowerShell és a PowerShell-munkafolyamati forgatókönyvek támogatja az egyszerű típusok és a komplex típusok, például a **objektum** vagy **PSCredential** -bemeneti paraméterekhez.

Ha a runbook rendelkezik az objektum típusú bemeneti paraméter, majd a PowerShell kivonattáblaként (név, érték) párokat toopass értékét. Ha például van egy runbook paraméter a következő hello:

     [Parameter (Mandatory = $true)]
     [object] $FullName

Akkor is továbbítja a következő érték toohello paraméter hello:

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a>Grafikus forgatókönyvek bemeneti paramétereinek a konfigurálása
túl[konfigurálása egy grafikus forgatókönyvnek](automation-first-runbook-graphical.md) bemeneti paraméterek, hozzon létre egy grafikus forgatókönyv, amely virtuális gépek adatait vagy egy virtuális vagy virtuális gépeinek erőforráscsoporton belül. Egy runbook konfigurálása áll két fő tevékenység az alább ismertetett.

[**Azure-beli futtató fiókot a Runbookok hitelesítést** ](automation-sec-configure-azure-runas-account.md) tooauthenticate az Azure-ral.

[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello tulajdonságai egy virtuális gépet.

Használhatja a hello [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) tevékenységek toooutput hello nevének a virtuális gépek. tevékenység hello **Get-AzureRmVm** két paramétert fogad, hello **virtuálisgép-nevet** és hello **erőforráscsoport-név**. Mivel ezek a paraméterek hello runbook minden egyes indításakor igényelhet eltérő értékek tartoznak, a bemeneti paraméterek tooyour runbook is hozzáadhat. Az alábbiakban hello lépéseket tooadd bemeneti paraméterek:

1. Válassza ki a hello grafikus forgatókönyvnek hello **Runbookok** panel megnyitásához, és kattintson [ **szerkesztése** ](automation-graphical-authoring-intro.md) azt.
2. Hello forgatókönyv-szerkesztőben kattintson **bemeneti és kimeneti** tooopen hello **bemeneti és kimeneti** panelen.
   
    ![Grafikus Automation-forgatókönyv](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. Hello **bemeneti és kimeneti** panel hello runbook meghatározott bemeneti paraméterek listáját jeleníti meg. A panel adja hozzá egy új bemeneti paramétert, vagy egy meglévő bemeneti paraméter hello konfigurációjának a szerkesztésével. egy új paraméter hello runbook tooadd kattintson **bemenet hozzáadása** tooopen hello **Runbook bemeneti paraméter** panelen. Hiba a következő paraméterek hello is konfigurálhatja:
   
   | **Tulajdonság** | **Leírás** |
   |:--- |:--- |
   | Név |Kötelező.  hello hello paraméter neve. Ez kell hello runbookon belüli egyedi és is csak betűket, számokat, vagy aláhúzás karaktereket tartalmazhat. Betűvel kell kezdődnie. |
   | Leírás |Választható. Bemeneti paraméter hello céljának leírása. |
   | Típus |Választható. hello adattípus, amely hello paraméterérték várt. Támogatott paramétertípusa **karakterlánc**, **Int32**, **Int64**, **decimális**, **logikai**,  **Dátum és idő**, és **objektum**. Ha adattípus nincs bejelölve, az alapértelmezés szerinti érték túl**karakterlánc**. |
   | Kötelező |Választható. Meghatározza, hogy egy érték hello paramétert meg kell adni. Ha úgy dönt, **Igen**, akkor meg kell adni egy értéket, hello runbook indításakor. Ha úgy dönt, **nem**, majd kitöltése nem kötelező hello forgatókönyv indításakor, és egy alapértelmezett érték is beállítható. |
   | Alapértelmezett érték |Választható. Adja meg egy értéket, amely hello paraméter alkalmazva lesznek, ha az érték nem kerül át a hello runbook indításakor. Alapértelmezett érték a nem kötelező paraméter állítható be. Válasszon tooset alapértelmezés **egyéni**. Ezt az értéket használja, ha egy másik értéket biztosított hello runbook indítását. Válasszon **nincs** Ha nem szeretné, hogy tooprovide minden alapértelmezett érték. |
   
    ![Új bemenet hozzáadása](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Hozzon létre két paramétert a következő tulajdonságok hello által használt hello **Get-AzureRmVm** tevékenység:
   
   * **Parameter1:**
     
     * Név – VMName
     * -Karakterlánc típusú
     * Kötelező – nem
   * **2:**
     
     * Név - erőforráscsoport-név
     * -Karakterlánc típusú
     * Kötelező – nem
     * Alapértelmezett érték – egyéni
     * Egyéni alapértelmezett érték – \<hello virtuális gépeket tartalmazó erőforráscsoport hello neve >
5. Ha hello paraméterek hozzáadásához kattintson **OK**.  Most már megtekintheti azokat a hello **bemeneti és kimeneti panel**. Kattintson a **OK** újra, és kattintson a **mentése** és **közzététel** a runbookban.

## <a name="assign-values-tooinput-parameters-in-runbooks"></a>Értékek társításához tooinput paraméterek a runbookokban
Átadhatók értékek tooinput paraméterek a runbookokat hello a következő forgatókönyvek a.

### <a name="start-a-runbook-and-assign-parameters"></a>Elindítja a runbookot, és rendelje hozzá a paraméterek
Egy runbook indítható számos szempont: hello Azure-portálon, és olyan webhook, a PowerShell-parancsmagokkal, hello REST API vagy hello SDK használatával. Az alábbiakban arról lesz szó különböző módszereket a runbook indítása, és rendelje hozzá a paramétereket.

#### <a name="start-a-published-runbook-by-using-hello-azure-portal-and-assign-parameters"></a>Indítsa el a közzétett runbookok hello Azure portál segítségével, és rendelje hozzá a paraméterek
Ha Ön [hello runbook indítása](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **runbook meghívása** panel nyílik meg, és konfigurálhatja az imént létrehozott hello paraméter értékét.

![Indítsa el a hello portál használatával](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Hello címke alatt hello beviteli mezőt, a hello attribútumok hello paraméter beállított látható. Attribútumok kötelező vagy választható, típusa és alapértelmezett értékét tartalmazza. A hello súgó buborékban megjelenő következő toohello paraméter neve megtekintheti az összes hello kulcsadatokat bemeneti paraméterértékek toomake döntéseket kell. Ezen információk közé tartozik, hogy a paraméter kötelező vagy választható. Hello típusa és alapértelmezett értéket (ha van ilyen) és más hasznos megjegyzéseket is tartalmaz.

![A buborékban megjelenő Súgó](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> Karakterlánc típusú paramétereket támogatási **üres** karakterlánc-értékek.  Belépés **[EmptyString]** hello bemeneti paraméterben mezőbe egy üres karakterlánc toohello paraméter fogja továbbítani. Ezenkívül nem támogatják a karakterlánc típusú paraméterrel **Null** átadta-e értékeket. Ha bármely érték toohello karakterlánc-paramétert nem adhatók át, majd PowerShell értelmezi null értékként.
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Indítsa el a közzétett runbookok PowerShell-parancsmagok használatával, és rendelje hozzá a paraméterek
* **Az Azure Resource Manager parancsmagjainak:** elindíthatja egy hozott létre egy erőforráscsoportot az Automation-runbook [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).
  
  **Példa**
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* **Azure szolgáltatásfelügyelet parancsmagok:** elindíthatja az automation-forgatókönyv használatával létrehozott egy alapértelmezett erőforráscsoportban [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).
  
  **Példa**
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> Amikor elindít egy runbookot PowerShell-parancsmagok, egy alapértelmezett paraméter használatával **MicrosoftApplicationManagementStartedBy** jön létre a hello érték **PowerShell**. Ez a paraméter megtekintheti a hello **feladat részletei** panelen.  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Az SDK használatával indítsa el a runbookot, és rendelje hozzá a paraméterek
* **Az Azure Resource Manager-metódus:** megkezdése egy runbook hello SDK egy programozási nyelv használatával. Alatt van egy C# kódrészletet a runbook indítása az Automation-fiókban. Megtekintheti az összes hello kódot a [GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  
  
  ```
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* **Azure szolgáltatásfelügyelet metódus:** megkezdése egy runbook hello SDK egy programozási nyelv használatával. Alatt van egy C# kódrészletet a runbook indítása az Automation-fiókban. Megtekintheti az összes hello kódot a [GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).
  
  ```      
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  toostart ezzel a módszerrel hozzon létre egy szótár toostore hello runbook-paraméterek, **VMName** és **resourceGroupName**, és azok értékeit. Majd indítsa el a hello runbook. Az alábbiakban van hello C# kódrészletet, amely a fent definiált hello metódus hívásakor.
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters toohello dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call hello StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-hello-rest-api-and-assign-parameters"></a>Forgatókönyv indítása hello REST API használatával, és rendelje hozzá a paraméterek
A runbook-feladatok létrehozhatók és hello Azure Automation REST API használatába hello segítségével **PUT** metódus a következő hello kérelmi URI.

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

Hello kérelem URI-azonosítója cserélje le a következő paraméterek hello:

* **előfizetés-azonosító:** az Azure-előfizetés-azonosítójával.  
* **felhő szolgáltatásnév:** hello felhő toowhich hello szolgáltatáskérés hello nevét kell küldeni.  
* **Automation-fiók-name:** megadott felhőszolgáltatás az automation-fiók, amely hello gazdája hello neve.  
* **azonosítójú feladat:** GUID hello hello feladat. A PowerShell GUID hello segítségével hozhatók létre **[GUID]::NewGuid(). ToString()** parancsot.

A sorrend toopass paraméterek toohello runbook-feladat használja a hello kérés törzsében. A következő két tulajdonságait JSON formátumban megadott hello tart:

* **Runbook neve:** szükséges. hello neve hello runbook hello feladat toostart.  
* **Runbook-paraméterek:** nem kötelező. Hello paraméterlista (név, érték) álló dictionary formátumban, ahol neve karakterlánc típusúnak kell lennie, és értéke lehet bármely érvényes JSON-érték.

Ha azt szeretné, hogy toostart hello **Get-AzureVMTextual** hoztak létre korábbi runbook **VMName** és **resourceGroupName** paraméterként, használja a következő JSON formátummal hello a hello kérés törzsében.

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

A 201-es HTTP-állapotkód eredményül, ha a hello feladat sikeresen létrehozva. A válaszfejlécek és hello adott válasz törzsének olvashat bővebben arról, hogyan toohello cikk túl[egy runbook-feladat létrehozása hello REST API használatával.](https://msdn.microsoft.com/library/azure/mt163849.aspx)

### <a name="test-a-runbook-and-assign-parameters"></a>Egy runbook tesztelése, és rendelje hozzá a paraméterek
Ha Ön [teszt hello a runbook vázlatverzióját](automation-testing-runbook.md) hello vizsgálati beállítással hello **tesztelése** panel nyílik meg, és konfigurálhatja az imént létrehozott hello paraméter értékét.

![Tesztelje, és rendelje hozzá a paraméterek](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-tooa-runbook-and-assign-parameters"></a>Egy ütemezés tooa runbook hivatkozásra, és rendelje hozzá a paraméterek
Is [összekapcsolhat egy ütemezést](automation-schedules.md) tooyour runbook úgy, hogy hello runbook egy adott időpontban kezdődik. Hello ütemezés létrehozása, és hello runbook ezeket az értékeket fogja használni, akkor hello ütemezés szerint rendelhet bemeneti paraméterek. Hello ütemezés nem menthető, amíg az összes kötelező paraméter az értékek.

![Az ütemezés és a hozzárendelés paraméterek](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>A webhook egy runbook létrehozása és hozzárendelése paraméterek
Létrehozhat egy [webhook](automation-webhooks.md) a runbook és konfigurálhatja a runbook bemeneti paramétereket. Hello webhook nem menthetők, amíg az összes kötelező paraméter az értékek.

![Webhook létrehozása és hozzárendelése paraméterek](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Amikor egy runbook egy webhook használatával hajtható végre, hello előre meghatározott bemeneti paraméter  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  , együtt küldött hello meghatározott bemeneti paraméterek. Kattinthat a tooexpand hello **WebhookData** paraméter további részleteket.

![WebhookData paraméter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a>Következő lépések
* A forgatókönyv bemeneti és kimeneti további információkért lásd: [Azure Automation: runbook bemeneti, kimeneti és egymásba ágyazott runbookok](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).
* Különböző módokon toostart egy runbookot, lásd: [runbook indítása](automation-starting-a-runbook.md).
* tooedit szöveges forgatókönyvként, tekintse meg a túl[szöveges runbookok szerkesztéséhez](automation-edit-textual-runbook.md).
* egy grafikus forgatókönyvnek tooedit tekintse meg a túl[grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).

