---
title: a webhook az Azure Automation-runbook aaaStarting |} Microsoft Docs
description: "A webhook, amely lehetővé teszi, hogy egy ügyfél toostart egy runbook az Azure Automationben egy HTTP-hívás.  Ez a cikk ismerteti, hogyan toocreate egy webhook és hogyan toocall egy toostart egy runbookot."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: ca6cde66b3784ceb5d0bc5921cee87aea74cb150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Egy Azure Automation-runbook kezdődő, és olyan webhook
A *webhook* lehetővé teszi egy adott runbookhoz toostart az Azure Automationben egyetlen HTTP-kérelem keresztül. Ez lehetővé teszi külső szolgáltatások, például a Visual Studio Team Services, GitHub, a Microsoft Operations Management Suite Naplóelemzési vagy egyéni alkalmazások toostart runbookokat hello Azure Automation API használatával teljes megoldások megvalósításának nélkül.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Összehasonlíthatja a runbook indítása webhookok tooother módszerek [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>A webhook részleteit
hello következő táblázat ismerteti, hogy konfigurálnia kell egy webhook hello tulajdonságok.

| Tulajdonság | Leírás |
|:--- |:--- |
| Név |Megadhat egy nevet, egy webhook mivel ez nincs felfedve toohello ügyfél.  Csak akkor tooidentify hello runbook az Azure Automationben szolgál. <br>  Ajánlott eljárásként adjon hello webhook neve kapcsolódó toohello ügyfél, amely fogja használni. |
| URL-CÍME |hello hello webhook URL-címe, a kapcsolt toohello webhook hello egyedi címet, hogy egy ügyfél egy HTTP POST toostart hello runbook hívja.  Hello webhook létrehozásakor automatikusan történik.  Egy egyéni URL-címe nem adható meg. <br> <br>  hello URL-címe olyan biztonsági jogkivonatot, amely lehetővé teszi a hitelesítés nélküli további külső rendszer által meghívott hello runbook toobe tartalmaz. Ezért azt kell kezelni, például a jelszó.  Biztonsági okokból is csak nézet hello hello Azure portálon, a hello idő hello webhook URL-címet jön létre. Vegye figyelembe a jövőbeli használatra biztonságos helyen hello URL-címet. |
| Lejárat dátuma |Például egy tanúsítványt egyes webhook van ekkor már nem használható lejárati dátuma.  A lejárati dátum hello webhook létrehozása után módosítható. |
| Engedélyezve |A webhook alapértelmezés szerint engedélyezve van, ha létrehozták.  Ha be állítani tooDisabled, akkor nem lesz képes toouse azt.  Beállíthatja a hello **engedélyezve** tulajdonság hello webhook, vagy bármikor egyszer létrehozásakor jön létre. |

### <a name="parameters"></a>Paraméterek
A webhook runbook paramétereket, illetve ha hello runbook indítja el, hogy a webhook értékeket határozhat meg. hello webhook értékeket hello runbook minden kötelező paraméterhez, és nem kötelező paraméter értékét is járhatott. A paraméter beállított értéke tooa webhook hello webhoook létrehozása után is módosíthatja. Egyetlen runbook több kapcsolódó webhookok tooa egyes eltérő értékek használhatók.

Amikor egy ügyfél elindítja a runbookot egy webhook, metódus nem bírálhatja felül a hello webhook definiált hello paraméterértékek.  hello ügyfél tooreceive adatait, hello runbook nevű egyetlen paramétert fogadhat **$WebhookData** típusú [objektum] adatokat tartalmazó hello ügyfél hello POST kérelemben tartalmazza.

![Webhookdata tulajdonságai](media/automation-webhooks/webhook-data-properties.png)

Hello **$WebhookData** objektum lesz hello következő tulajdonságai:

| Tulajdonság | Leírás |
|:--- |:--- |
| WebhookName |hello webhook hello neve. |
| RequestHeader |A kivonattábla tartalmazó hello fejléceket a bejövő hello POST-kérelmet. |
| requestBody |bejövő hello POST-kérelmet hello törzsét.  Ez megőrzi a formázási karakterlánc, JSON, XML, például vagy űrlap kódolású adatot. hello runbook hello adatok formátumú várt toowork úgy kell megírni. |

Nincs a hello webhook szükséges toosupport hello konfiguráció **$WebhookData** paramétert, és hello runbook nincs szükség tooaccept azt.  Hello runbook hello paraméter nem határozza meg, ha minden adatát hello ügyfélről küldött hello kérelem figyelmen kívül hagyja.

Ha megad egy értéket a $WebhookData hello webhook létrehozásakor, ezt az értéket fogja bírálható felül indításakor hello webhook hello runbook hello adataival hello ügyfél POST-kérelmet, még akkor is, ha hello ügyfél nem vesz be semmilyen adatot hello kérés törzsében.  Ha elindít egy forgatókönyvet, amely eltérő a webhook módszerrel $WebhookData rendelkezik, a értéket adja meg, amely hello runbook felismer $Webhookdata.  Ez az érték legyen hello objektum ugyanazon [tulajdonságok](#details-of-a-webhook) , így hello runbook megfelelően dolgozhat, mintha volt az olyan webhook által átadott tényleges WebhookData használata $Webhookdata.

Például ha meg és indítja el a runbook követően – hello Azure Portal hello WebhookData néhány példa a tesztelésre, mivel WebhookData objektum toopass, azt kell átadni JSON-ként hello felhasználói felület.

![A felhasználói Felületről WebhookData paraméter](media/automation-webhooks/WebhookData-parameter-from-UI.png)

A fenti runbookot, ha a következő tulajdonságok hello WebhookData paraméter hello hello:

1. WebhookName: *MyWebhook*
2. RequestHeader: *= teszt felhasználó*
3. RequestBody: *["VM1", "Vm2 virtuális gépnek"]*

Majd át hello JSON-érték a felhasználói felület hello hello WebhookData paraméter a következő lenne:  

* {"WebhookName": "MyWebhook", "RequestHeader": {"Feladó": "Felhasználói teszt"}, "RequestBody": "[\"VM1\",\"vm2 virtuális gépnek\"]"}

![Kezdő WebhookData paraméter a felhasználói Felületről](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> az összes bemeneti paraméterek értékét hello hello runbook-feladatok naplózása.  Ez azt jelenti, hogy bármely hello webhook kérelem hello ügyfél által megadott hozzáférési toohello automation-feladat a naplózás és a rendelkezésre álló tooanyone lesz.  Emiatt a webhook hívások a bizalmas adatokat is beleértve elővigyázatosnak kell lennie.
>

## <a name="security"></a>Biztonság
a webhook hello biztonságát hello adatvédelme érdekében az URL-CÍMÉT, amely egy biztonsági jogkivonatot tartalmaz, amely lehetővé teszi, hogy meghívott toobe támaszkodik. Azure Automation szolgáltatásbeli nem végzi el a hitelesítési hello kérelem mindaddig, amíg az azt alkotó toohello helyes URL-címet. Emiatt webhookok nem használható a runbookok, amelyek a szigorúan bizalmas valamilyen alternatív, hello kérelem ellenőrzése használata nélkül.

Megadhatja, hogy meghívták volna a webhook által hello ellenőrzésével hello runbook toodetermine belüli logika **WebhookName** hello $WebhookData paraméter tulajdonság. hello runbook volt az ellenőrzéshez további hello az adott információkat keres **RequestHeader** vagy **RequestBody** tulajdonságait.

Egy másik olyan stratégia toohave hello runbook egy külső állapot néhány ellenőrzés végrehajtása, ha webhook kérelmet kapott.  Tegyük fel, ha egy új véglegesítési tooa GitHub-tárházban GitHub hívja runbook.  hello runbook kapcsolódását tooGitHub toovalidate, amely egy új véglegesítési valójában történt volna a folytatás előtt.

## <a name="creating-a-webhook"></a>A webhook létrehozása
A következő eljárás toocreate hello Azure portál egy új kapcsolódó webhook tooa runbook hello használata.

1. A hello **Runbookok panel** hello Azure-portálon, kattintson hello webhook hello runbook elindul tooview a részletek panelen.
2. Kattintson a **Webhook** hello panel tooopen hello hello tetején **hozzáadása Webhook** panelen. <br>
   ![Webhook gomb](media/automation-webhooks/webhooks-button.png)
3. Kattintson a **hozzon létre új webhook** tooopen hello **létrehozás webhook panel**.
4. Adjon meg egy **neve**, **lejárati dátum** hello webhook, és hogy azt engedélyezni kell. Lásd: [olyan webhook részleteit](#details-of-a-webhook) további információt ezeket a tulajdonságokat.
5. Hello másolás ikonra, majd nyomja meg a Ctrl + C toocopy hello URL-címe hello webhook.  Biztonságos helyen, majd rögzítse azt.  **Hello webhook létrehozása után nem lehet lekérdezni a hello URL-cím újra.** <br>
   ![Webhook URL-CÍMÉT](media/automation-webhooks/copy-webhook-url.png)
6. Kattintson a **paraméterek** tooprovide hello gyermekrunbook paramétereinek értékeit.  Ha hello runbook kötelező paraméterekkel rendelkezik, majd nem fogja tudni toocreate hello webhook kivéve, ha az értékek.
7. Kattintson a **létrehozása** toocreate hello webhook.

## <a name="using-a-webhook"></a>A webhook használatával
toouse a webhook létrehozása után, az ügyfélalkalmazás hello URL-címmel hello webhook HTTP POST kell kiadnia.  hello webhook hello szintaxisa a következő formátumban hello lesz.

    http://<Webhook Server>/token?=<Token Value>

hello ügyfél hello POST-kérelmet a visszatérési kódok következő hello egyikét fogja megkapni.  

| Kód | Szöveg | Leírás |
|:--- |:--- |:--- |
| 202 |Elfogadva |hello kérést elfogadták, és hello runbook sikeresen várólistára került. |
| 400 |Helytelen kérelem |hello kérelem hello a következő okok valamelyike miatt elutasítva. <ul> <li>hello webhook érvényessége lejárt.</li> <li>hello webhook le van tiltva.</li> <li>hello token hello URL-cím érvénytelen.</li>  </ul> |
| 404 |Nem található |hello kérelem hello a következő okok valamelyike miatt elutasítva. <ul> <li>hello webhook nem található.</li> <li>hello runbook nem található.</li> <li>hello fiók nem található.</li>  </ul> |
| 500 |Belső kiszolgálóhiba. |hello URL-cím érvénytelen volt, de hiba történt.  Küldje el hello kérelem. |

Feltéve, hogy hello kérés sikeres-e, hello webhook válaszban hello feladatazonosító JSON formátumban az alábbiak szerint. Egyetlen feladatazonosító fogja tartalmazni, de lehetséges jövőbeli fejlesztések hello JSON formátumban teszi lehetővé.

    {"JobIds":["<JobId>"]}  

hello ügyfél nem tudja megállapítani, hello runbook-feladat befejezését vagy hello webhook a befejezési állapotát.  Ezt az információt hello feladatazonosító egy másik módszerrel például meg tudja határozni [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) vagy hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).

### <a name="example"></a>Példa
a következő példa hello Windows PowerShell toostart egy runbook egy webhook használ.  Vegye figyelembe, hogy bármilyen nyelvet használhat, amelyek HTTP-kérelem használhatja a webhook; Windows PowerShell csak használt példa.

hello runbook által várt paraméterekkel hello törzsében hello kérelem JSON-formázott virtuális gépek listáját. Azt is vannak adatai, például ki indul hello runbook és hello dátum és idő hello kérelem hello fejlécében van indítását.      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


hello következő kép bemutatja hello fejléc-információ (használatával egy [Fiddler](http://www.telerik.com/fiddler) nyomkövetési) a kérelemből. Ez tartalmaz szabványos HTTP-kérelem-fejlécekkel hozzáadása toohello egyéni dátum és a hozzáadott-fejlécek.  Ezek az értékek egy elérhető toohello runbookot a hello **RequestHeaders** tulajdonsága **WebhookData**.

![Webhook gomb](media/automation-webhooks/webhook-request-headers.png)

hello következő kép bemutatja hello kérelem törzse hello (használatával egy [Fiddler](http://www.telerik.com/fiddler) nyomkövetési), amely elérhető toohello runbook hello **RequestBody** tulajdonsága **WebhookData**. Ez formázott JSON-ként mert, amely hello hello kérelem törzsében szereplő hello formátumú volt.     

![Webhook gomb](media/automation-webhooks/webhook-request-body.png)

hello következő kép bemutatja a Windows PowerShell és az eredményül kapott válasz hello küldi hello kérelem.  hello feladatazonosító kinyerésének hello válasz és konvertált tooa karakterlánc.

![Webhook gomb](media/automation-webhooks/webhook-request-response.png)

hello következő minta runbook hello előző példa kérést fogad és hello virtuális gépek hello kérés törzsében megadott kezdődik.

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate tooAzure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean toobe started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-tooazure-alerts"></a>Runbookok elindítása a válasz tooAzure riasztások
Runbookok Webhook-kompatibilis túl lehet használt tooreact[Azure riasztások](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Az Azure-erőforrások hello statisztika, például a teljesítmény, rendelkezésre állását és használati Azure riasztások hello segítségével-készítés révén figyelhető. Figyelési metrikákat alapuló riasztást kaphat, vagy eseményeket az Azure-erőforrások, jelenleg az Automation-fiókok csak metrikák támogatja. Ha adott mérőszám hello értéke meghaladja-e a hozzárendelt hello küszöbértéket vagy hello konfigurált az esemény akkor váltódik ki, majd egy értesítést küld toohello rendszergazda vagy a társadminisztrátoroknak tooresolve hello szolgáltatásriasztás, további információt a metrikákat, és események tekintse meg a túl[ Az Azure riasztások](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Használata Azure riasztások értesítési rendszert, mellett is is indítsa el a válasz tooalerts runbookok. Azure Automation szolgáltatásbeli hello funkció toorun webhook engedélyezve van a runbookok a riasztások az Azure biztosít. Metrika meghaladja hello konfigurálva küszöbérték akkor hello riasztási szabály válik aktívvá, és eseményindítók hello pedig végrehajtja a hello runbook automation-webhook.

![webhook](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a>Riasztás környezete
Érdemes lehet például egy virtuális gépet egy Azure-erőforrás, a CPU-felhasználás gép hello legfontosabb teljesítményi metrika egyikét. Ha hello CPU-felhasználás 100 %-os vagy több mint egy hosszú időn keresztül, érdemes lehet toorestart hello virtuális gép toofix hello probléma. Ez megoldható egy riasztási szabály toohello virtuális gép konfigurálása, és ez a szabály tart, mint a metrika processzorszázaléka. Processzorszázaléka Itt csupán példaként lesz végrehajtva, de nincsenek konfigurálhat a tooyour Azure számos más metrikákkal erőforrások és hello virtuális gép újraindítása egy művelet végrehajtott toofix probléma konfigurálhatja hello runbook tootake más műveletek.

Ha a hello riasztási szabály válik aktívvá, és eseményindítók hello webhook engedélyezve van a runbook, hello riasztás környezete toohello runbook küld. [Riasztás környezete](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) tartalmaz információt talál többek között **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** és **időbélyeg** hello runbook tooidentify hello erőforrás amely művelet azt véve szükséges. Riasztási környezetben beágyazott hello törzs részét hello **WebhookData** elküldött toohello runbook objektumra, és elérhető legyen a **Webhook.RequestBody** tulajdonság

### <a name="example"></a>Példa
Hozzon létre egy Azure virtuális gép előfizetését és társítható egy [toomonitor CPU százalékos metrika riasztási](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Hello riasztás létrehozása során mindenképpen feltölti hello webhook mezővel hello hello webhook létrehozása során létrehozó hello webhook URL-CÍMÉT.

hello következő minta runbook lesz kiváltva, ha hello riasztási szabály válik aktívvá, és összegyűjti az hello riasztás környezete paraméterek, amelyek szükségesek a hello runbook tooidentify hello erőforrás amely művelet azt véve.

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on hello webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain hello WebhookBody containing hello AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain hello AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on hello AlertContext data, in our case restarting hello VM.
            # Authenticate tooyour Azure subscription using Organization ID toobe able toorestart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check hello status property of hello VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart hello VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant tooonly be started from a webhook."  
        }  
    }



## <a name="next-steps"></a>Következő lépések
* A különböző módokon toostart egy runbookot, lásd: [Runbook indítása](automation-starting-a-runbook.md).
* A Runbook-feladatok állapotának megtekintése hello információkért tekintse meg túl[a Runbook végrehajtása az Azure Automationben](automation-runbook-execution.md).
* Hogyan toouse Azure Automation tootake művelet Azure riasztások: toolearn [javítása Azure virtuális gép riasztások Automation-Runbook](automation-azure-vm-alert-integration.md).
