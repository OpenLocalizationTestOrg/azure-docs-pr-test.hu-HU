---
title: "AAA\"javítása Azure virtuális gép riasztások Automation-Runbook |} Microsoft dokumentumok\""
description: "Ez a cikk bemutatja, hogyan toointegrate Azure virtuális gép Azure Automation-runbook riasztásokat, és a problémák automatikus javítása"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2016
ms.author: csand;magoedte
ms.openlocfilehash: c226368a5c4c51fbfb331f4b97f7f2f239e701c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Azure Automation-forgatókönyv - javítása Azure virtuális gép riasztások
Azure Automation és Azure virtuális gépek kiadott tooconfigure virtuális gép (VM) riasztások toorun Automation-forgatókönyveket teszi új szolgáltatása. Ezen új szolgáltatás lehetővé teszi a tooautomatically válasz tooVM riasztások, például újraindítása vagy leállítása hello virtuális gép a szabványos szervizelés végrehajtására.

Korábban, a Virtuálisgép-riasztási szabály létrehozása során létrehozott túl[adja meg az Automation-webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook rendelés toorun hello runbook amikor hello figyelmeztetés. Azonban ez szükséges akkor toodo hello munka hello runbook létrehozása, hello webhook hello runbook létrehozása és majd másolás és beillesztés hello webhook riasztási szabály létrehozása során. Új ebben a kiadásban hello folyamat oka sokkal könnyebben közvetlenül választhat egy runbook listáját riasztási szabály létrehozása során, és választhat egy Automation-fiók, amelyek hello runbook futtatására, illetve könnyen hozzon létre egy fiókot.

Ebben a cikkben rendszer megtudhatja, milyen egyszerűen egy Azure virtuális gép riasztást tooset és konfigurálja az Automation-runbook toorun, amikor elindítja a hello riasztás. Példaforgatókönyvek tartalmaznak egy virtuális gép újraindítása, ha hello memóriahasználata bizonyos küszöb hello VM memóriavesztés az alkalmazás tooan miatt, vagy egy virtuális gép leállítása, ha hello Processzor felhasználói idő alatt 1 % az elmúlt egy órában, és nincs használatban. Azt is ismertetjük hogyan hello automatikus létrehozása az egyszerű az Automation szolgáltatás fiók leegyszerűsíti az Azure riasztási szervizelés runbookokat hello használatát.

## <a name="create-an-alert-on-a-vm"></a>Riasztás létrehozása a virtuális gép
Hajtsa végre a következő lépéseket tooconfigure egy riasztási toolaunch egy runbook az küszöböt teljesülésekor hello.

> [!NOTE]
> Ebben a kiadásban csak támogatjuk V2 virtuális gép és a támogatás klasszikus virtuális gépek hamarosan megjelenik.  
> 
> 

1. Jelentkezzen be Azure-portálon toohello, és kattintson **virtuális gépek**.  
2. A virtuális gépek közül.  hello virtuális gépek irányítópult paneljét fog megjelenni, és hello **beállítások** panel tooits jobbra.  
3. A hello **beállítások** panelen hello figyelés részen jelölje be a **riasztási szabályok**.
4. A hello **riasztási szabályok** panelen kattintson a **riasztás hozzáadása**.

Ezzel megnyílik hello **riasztási szabály felvétele** panel, ahol hello riasztás hello feltételeinek konfigurálása, és egyet vagy minden beállítás közül választhat: toosomeone e-mailt küldeni, használjon webhook tooforward hello riasztási tooanother, és/vagy az Automation-forgatókönyv futtatása válasz kísérlet tooremediate hello probléma.

## <a name="configure-a-runbook"></a>A runbook konfigurálása
egy runbook toorun hello VM riasztási küszöbérték teljesülésekor tooconfigure válasszon **Automation-Runbook**. A hello **runbook konfigurálása** panelen kiválaszthatja hello runbook toorun és hello automatizálási fiókot toorun hello runbook.

![Az Automation-runbook konfigurálása és új Automation-fiók létrehozása](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> Ebben a kiadásban választhat három runbookok biztosító hello szolgáltatás – indítsa újra a virtuális gép, állítsa le a virtuális gép vagy távolítsa el a virtuális gép (törölni).  képes tooselect más runbookokat hello, vagy egy saját runbookok egy későbbi kiadásban elérhető lesz.
> 
> 

![A Runbookok toochoose](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Miután kiválasztotta az egyik rendelkezésre álló runbookokat hello három, hello **Automation-fiók** legördülő lista jelenik meg, és kijelölhet egy automatizálási fiókot hello runbook gépként fognak futni. Runbookokat hello környezetében toorun kell egy [Automation-fiók](automation-security-overview.md) szerepel az Azure-előfizetéshez. Kiválaszthatja, hogy már létrehozta, vagy beállíthatja, hogy a hozott létre az Ön új Automation-fiók Automation-fiók.

által biztosított runbookokat hello tooAzure egyszerű szolgáltatás használatával hitelesíteni. Ha a meglévő Automation-fiókok valamelyikével toorun hello runbook, automatikusan létrehozunk hello szolgáltatás egyszerű meg. Ha úgy dönt, toocreate új Automation-fiók, majd automatikusan létrehozunk hello fiókot és egy egyszerű hello szolgáltatást. Mindkét esetben két eszközök is létrejön az Automation-fiók – a tanúsítvány eszköz nevű hello **AzureRunAsCertificate** és a kapcsolódási eszköz nevű **AzureRunAsConnection**. runbookokat hello használandó **AzureRunAsConnection** tooauthenticate az Azure-ral rendelés tooperform hello felügyeleti művelet hello VM ellen.

> [!NOTE]
> hello szolgáltatás egyszerű hello előfizetési hatókört jön létre, és hello közreműködői szerepkör van hozzárendelve. Ez a szerepkör szükséges ahhoz, hogy hello fiók toohave engedély toorun automatizálási runbookok toomanage Azure virtuális gépeken.  hello létrehozása egy Automaton fiók és/vagy az egyszerű szolgáltatásnév az egyszeri esemény. A létrehozásuk után a fiók toorun runbookok használható egyéb Azure virtuális gép riasztásokat.
> 
> 

Amikor rákattint **OK** hello riasztás van konfigurálva, és hello beállítás toocreate új Automation-fiók választott ki, ha egyszerű hello szolgáltatást együtt létrehozták.  Ez eltarthat néhány másodpercig toocomplete.  

![A Runbook konfigurálva](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Hello konfiguráció befejezése után megjelenik-e jelennek meg hello hello runbook neve hello **riasztási szabály felvétele** panelen.

![A Runbook konfigurálva](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Kattintson a **OK** a hello **riasztási szabály felvétele** panel és hello riasztási szabály jön létre, és ha hello virtuális gép futó állapotban van.

### <a name="enable-or-disable-a-runbook"></a>Engedélyezheti vagy tilthatja le a runbookot
Ha egy runbook riasztás konfigurálva van, bármikor letilthatja azt hello runbook konfiguráció eltávolítása nélkül. Ez lehetővé teszi a tookeep hello riasztás fut, és lehet, hogy tesztelése egyes hello riasztási szabályok, és majd később újra engedélyezheti hello runbook.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Hozzon létre egy runbookot, amely az Azure riasztás
Ha egy runbook Azure riasztási szabály részeként, hello runbook kell azt a toohave logikai toomanage hello riasztási adatokat tooit átadott.  Amikor egy runbook a riasztási szabály van beállítva, a webhook jön létre hello runbook; a webhook nem használt toostart hello runbook minden alkalommal hello riasztási eseményindítók.  hello tényleges hívás toostart hello runbook egy HTTP POST kérelem toohello webhook URL-CÍMÉT. hello POST kérelem törzse hello hasznos tulajdonságok kapcsolódó toohello riasztást tartalmazó JSON-formátumú objektumot tartalmaz.  Mint az alább látható, a hello riasztási adatokat adatait, például az előfizetés-azonosító, erőforráscsoport-név, resourceName és resourceType tartalmazza.

### <a name="example-of-alert-data"></a>Riasztási adatokat – példa
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Automation-webhook szolgáltatás hello fogadásakor hello HTTP POST kibontja hello riasztási adatokat, és továbbítja azokat hello WebhookData runbook bemeneti paraméter toohello runbook.  Az alábbiakban van a minta-runbookhoz bemutatja, hogyan toouse hello WebhookData paraméter, bontsa ki a riasztási adatokat hello és toomanage hello hello riasztást kiváltó Azure erőforráscsoport használja.

### <a name="example-runbook"></a>Példa runbook
```
#  This runbook will restart an ARM (V2) VM in response tooan Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get hello data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that hello alert status is 'Activated' (alert condition went from false tootrue)
    # and not 'Resolved' (alert condition went from true toofalse)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get hello info needed tooidentify hello VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is hello expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate tooAzure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in hello Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart hello VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # hello alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant toobe started from an Azure alert only."
}
```

## <a name="summary"></a>Összefoglalás
Ha olyan riasztást konfigurálhat egy Azure virtuális gépen, most már rendelkezik hello képességét tooeasily konfigurálása egy automatizálási runbook tooautomatically szervizelési művelet végre, ha elindítja a hello riasztás. Ebben a kiadásban választhat runbookok toorestart, állítsa le, vagy egy virtuális Gépet, attól függően, hogy a riasztási forgatókönyv törlése. Ez a forgatókönyvek, ahol megadhatja a hello műveletek (értesítés, hibaelhárítási, szervizelés), amely automatikusan amikor elindítja a riasztás engedélyezése csak hello elejére.

## <a name="next-steps"></a>Következő lépések
* Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)
* További információ az runbooktípusokkal, azok előnyeit, és a korlátozásokról toolearn lásd [Azure Automation-runbook típusok](automation-runbook-types.md)

