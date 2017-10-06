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
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a><span data-ttu-id="48f04-103">Azure Automation-forgatókönyv - javítása Azure virtuális gép riasztások</span><span class="sxs-lookup"><span data-stu-id="48f04-103">Azure Automation scenario - remediate Azure VM alerts</span></span>
<span data-ttu-id="48f04-104">Azure Automation és Azure virtuális gépek kiadott tooconfigure virtuális gép (VM) riasztások toorun Automation-forgatókönyveket teszi új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="48f04-104">Azure Automation and Azure Virtual Machines have released a new feature allowing you tooconfigure Virtual Machine (VM) alerts toorun Automation runbooks.</span></span> <span data-ttu-id="48f04-105">Ezen új szolgáltatás lehetővé teszi a tooautomatically válasz tooVM riasztások, például újraindítása vagy leállítása hello virtuális gép a szabványos szervizelés végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="48f04-105">This new capability allows you tooautomatically perform standard remediation in response tooVM alerts, like restarting or stopping hello VM.</span></span>

<span data-ttu-id="48f04-106">Korábban, a Virtuálisgép-riasztási szabály létrehozása során létrehozott túl[adja meg az Automation-webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook rendelés toorun hello runbook amikor hello figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="48f04-106">Previously, during VM alert rule creation you were able too[specify an Automation webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook in order toorun hello runbook whenever hello alert triggered.</span></span> <span data-ttu-id="48f04-107">Azonban ez szükséges akkor toodo hello munka hello runbook létrehozása, hello webhook hello runbook létrehozása és majd másolás és beillesztés hello webhook riasztási szabály létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="48f04-107">However, this required you toodo hello work of creating hello runbook, creating hello webhook for hello runbook, and then copying and pasting hello webhook during alert rule creation.</span></span> <span data-ttu-id="48f04-108">Új ebben a kiadásban hello folyamat oka sokkal könnyebben közvetlenül választhat egy runbook listáját riasztási szabály létrehozása során, és választhat egy Automation-fiók, amelyek hello runbook futtatására, illetve könnyen hozzon létre egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="48f04-108">With this new release, hello process is much easier because you can directly choose a runbook from a list during alert rule creation, and you can choose an Automation account which will run hello runbook or easily create an account.</span></span>

<span data-ttu-id="48f04-109">Ebben a cikkben rendszer megtudhatja, milyen egyszerűen egy Azure virtuális gép riasztást tooset és konfigurálja az Automation-runbook toorun, amikor elindítja a hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="48f04-109">In this article, we will show you how easy it is tooset up an Azure VM alert and configure an Automation runbook toorun whenever hello alert triggers.</span></span> <span data-ttu-id="48f04-110">Példaforgatókönyvek tartalmaznak egy virtuális gép újraindítása, ha hello memóriahasználata bizonyos küszöb hello VM memóriavesztés az alkalmazás tooan miatt, vagy egy virtuális gép leállítása, ha hello Processzor felhasználói idő alatt 1 % az elmúlt egy órában, és nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="48f04-110">Example scenarios include restarting a VM when hello memory usage exceeds some threshold due tooan application on hello VM with a memory leak, or stopping a VM when hello CPU user time has been below 1% for past hour and is not in use.</span></span> <span data-ttu-id="48f04-111">Azt is ismertetjük hogyan hello automatikus létrehozása az egyszerű az Automation szolgáltatás fiók leegyszerűsíti az Azure riasztási szervizelés runbookokat hello használatát.</span><span class="sxs-lookup"><span data-stu-id="48f04-111">We’ll also explain how hello automated creation of a service principal in your Automation account simplifies hello use of runbooks in Azure alert remediation.</span></span>

## <a name="create-an-alert-on-a-vm"></a><span data-ttu-id="48f04-112">Riasztás létrehozása a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="48f04-112">Create an alert on a VM</span></span>
<span data-ttu-id="48f04-113">Hajtsa végre a következő lépéseket tooconfigure egy riasztási toolaunch egy runbook az küszöböt teljesülésekor hello.</span><span class="sxs-lookup"><span data-stu-id="48f04-113">Perform hello following steps tooconfigure an alert toolaunch a runbook when its threshold has been met.</span></span>

> [!NOTE]
> <span data-ttu-id="48f04-114">Ebben a kiadásban csak támogatjuk V2 virtuális gép és a támogatás klasszikus virtuális gépek hamarosan megjelenik.</span><span class="sxs-lookup"><span data-stu-id="48f04-114">With this release, we only support V2 virtual machines and support for classic VMs will be added soon.</span></span>  
> 
> 

1. <span data-ttu-id="48f04-115">Jelentkezzen be Azure-portálon toohello, és kattintson **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="48f04-115">Log in toohello Azure portal and click **Virtual Machines**.</span></span>  
2. <span data-ttu-id="48f04-116">A virtuális gépek közül.</span><span class="sxs-lookup"><span data-stu-id="48f04-116">Select one of your virtual machines.</span></span>  <span data-ttu-id="48f04-117">hello virtuális gépek irányítópult paneljét fog megjelenni, és hello **beállítások** panel tooits jobbra.</span><span class="sxs-lookup"><span data-stu-id="48f04-117">hello virtual machine dashboard blade will appear and hello **Settings** blade tooits right.</span></span>  
3. <span data-ttu-id="48f04-118">A hello **beállítások** panelen hello figyelés részen jelölje be a **riasztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="48f04-118">From hello **Settings** blade, under hello Monitoring section select **Alert rules**.</span></span>
4. <span data-ttu-id="48f04-119">A hello **riasztási szabályok** panelen kattintson a **riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="48f04-119">On hello **Alert rules** blade, click **Add alert**.</span></span>

<span data-ttu-id="48f04-120">Ezzel megnyílik hello **riasztási szabály felvétele** panel, ahol hello riasztás hello feltételeinek konfigurálása, és egyet vagy minden beállítás közül választhat: toosomeone e-mailt küldeni, használjon webhook tooforward hello riasztási tooanother, és/vagy az Automation-forgatókönyv futtatása válasz kísérlet tooremediate hello probléma.</span><span class="sxs-lookup"><span data-stu-id="48f04-120">This opens up hello **Add an alert rule** blade, where you can configure hello conditions for hello alert and choose among one or all of these options: send email toosomeone, use a webhook tooforward hello alert tooanother system, and/or run an Automation runbook in response attempt tooremediate hello issue.</span></span>

## <a name="configure-a-runbook"></a><span data-ttu-id="48f04-121">A runbook konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48f04-121">Configure a runbook</span></span>
<span data-ttu-id="48f04-122">egy runbook toorun hello VM riasztási küszöbérték teljesülésekor tooconfigure válasszon **Automation-Runbook**.</span><span class="sxs-lookup"><span data-stu-id="48f04-122">tooconfigure a runbook toorun when hello VM alert threshold is met, select **Automation Runbook**.</span></span> <span data-ttu-id="48f04-123">A hello **runbook konfigurálása** panelen kiválaszthatja hello runbook toorun és hello automatizálási fiókot toorun hello runbook.</span><span class="sxs-lookup"><span data-stu-id="48f04-123">In hello **Configure runbook** blade, you can select hello runbook toorun and hello Automation account toorun hello runbook in.</span></span>

![Az Automation-runbook konfigurálása és új Automation-fiók létrehozása](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> <span data-ttu-id="48f04-125">Ebben a kiadásban választhat három runbookok biztosító hello szolgáltatás – indítsa újra a virtuális gép, állítsa le a virtuális gép vagy távolítsa el a virtuális gép (törölni).</span><span class="sxs-lookup"><span data-stu-id="48f04-125">For this release you can choose from three runbooks that hello service provides – Restart VM, Stop VM, or Remove VM (delete it).</span></span>  <span data-ttu-id="48f04-126">képes tooselect más runbookokat hello, vagy egy saját runbookok egy későbbi kiadásban elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="48f04-126">hello ability tooselect other runbooks or one of your own runbooks will be available in a future release.</span></span>
> 
> 

![A Runbookok toochoose](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

<span data-ttu-id="48f04-128">Miután kiválasztotta az egyik rendelkezésre álló runbookokat hello három, hello **Automation-fiók** legördülő lista jelenik meg, és kijelölhet egy automatizálási fiókot hello runbook gépként fognak futni.</span><span class="sxs-lookup"><span data-stu-id="48f04-128">After you select one of hello three available runbooks, hello **Automation account** drop-down list appears and you can select an automation account hello runbook will run as.</span></span> <span data-ttu-id="48f04-129">Runbookokat hello környezetében toorun kell egy [Automation-fiók](automation-security-overview.md) szerepel az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="48f04-129">Runbooks need toorun in hello context of an [Automation account](automation-security-overview.md) that is in your Azure subscription.</span></span> <span data-ttu-id="48f04-130">Kiválaszthatja, hogy már létrehozta, vagy beállíthatja, hogy a hozott létre az Ön új Automation-fiók Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="48f04-130">You can select an Automation account that you already created, or you can have a new Automation account created for you.</span></span>

<span data-ttu-id="48f04-131">által biztosított runbookokat hello tooAzure egyszerű szolgáltatás használatával hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="48f04-131">hello runbooks that are provided authenticate tooAzure using a service principal.</span></span> <span data-ttu-id="48f04-132">Ha a meglévő Automation-fiókok valamelyikével toorun hello runbook, automatikusan létrehozunk hello szolgáltatás egyszerű meg.</span><span class="sxs-lookup"><span data-stu-id="48f04-132">If you choose toorun hello runbook in one of your existing Automation accounts, we will automatically create hello service principal for you.</span></span> <span data-ttu-id="48f04-133">Ha úgy dönt, toocreate új Automation-fiók, majd automatikusan létrehozunk hello fiókot és egy egyszerű hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="48f04-133">If you choose toocreate a new Automation account, then we will automatically create hello account and hello service principal.</span></span> <span data-ttu-id="48f04-134">Mindkét esetben két eszközök is létrejön az Automation-fiók – a tanúsítvány eszköz nevű hello **AzureRunAsCertificate** és a kapcsolódási eszköz nevű **AzureRunAsConnection**.</span><span class="sxs-lookup"><span data-stu-id="48f04-134">In both cases, two assets will also be created in hello Automation account – a certificate asset named **AzureRunAsCertificate** and a connection asset named **AzureRunAsConnection**.</span></span> <span data-ttu-id="48f04-135">runbookokat hello használandó **AzureRunAsConnection** tooauthenticate az Azure-ral rendelés tooperform hello felügyeleti művelet hello VM ellen.</span><span class="sxs-lookup"><span data-stu-id="48f04-135">hello runbooks will use **AzureRunAsConnection** tooauthenticate with Azure in order tooperform hello management action against hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="48f04-136">hello szolgáltatás egyszerű hello előfizetési hatókört jön létre, és hello közreműködői szerepkör van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="48f04-136">hello service principal is created in hello subscription scope and is assigned hello Contributor role.</span></span> <span data-ttu-id="48f04-137">Ez a szerepkör szükséges ahhoz, hogy hello fiók toohave engedély toorun automatizálási runbookok toomanage Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="48f04-137">This role is required in order for hello account toohave permission toorun Automation runbooks toomanage Azure VMs.</span></span>  <span data-ttu-id="48f04-138">hello létrehozása egy Automaton fiók és/vagy az egyszerű szolgáltatásnév az egyszeri esemény.</span><span class="sxs-lookup"><span data-stu-id="48f04-138">hello creation of an Automaton account and/or service principal is a one-time event.</span></span> <span data-ttu-id="48f04-139">A létrehozásuk után a fiók toorun runbookok használható egyéb Azure virtuális gép riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="48f04-139">Once they are created, you can use that account toorun runbooks for other Azure VM alerts.</span></span>
> 
> 

<span data-ttu-id="48f04-140">Amikor rákattint **OK** hello riasztás van konfigurálva, és hello beállítás toocreate új Automation-fiók választott ki, ha egyszerű hello szolgáltatást együtt létrehozták.</span><span class="sxs-lookup"><span data-stu-id="48f04-140">When you click **OK** hello alert is configured and if you selected hello option toocreate a new Automation account, it is created along with hello service principal.</span></span>  <span data-ttu-id="48f04-141">Ez eltarthat néhány másodpercig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="48f04-141">This can take a few seconds toocomplete.</span></span>  

![A Runbook konfigurálva](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

<span data-ttu-id="48f04-143">Hello konfiguráció befejezése után megjelenik-e jelennek meg hello hello runbook neve hello **riasztási szabály felvétele** panelen.</span><span class="sxs-lookup"><span data-stu-id="48f04-143">After hello configuration is completed you will see hello name of hello runbook appear in hello **Add an alert rule** blade.</span></span>

![A Runbook konfigurálva](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

<span data-ttu-id="48f04-145">Kattintson a **OK** a hello **riasztási szabály felvétele** panel és hello riasztási szabály jön létre, és ha hello virtuális gép futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="48f04-145">Click **OK** in hello **Add an alert rule** blade and hello alert rule will be created and activate if hello virtual machine is in a running state.</span></span>

### <a name="enable-or-disable-a-runbook"></a><span data-ttu-id="48f04-146">Engedélyezheti vagy tilthatja le a runbookot</span><span class="sxs-lookup"><span data-stu-id="48f04-146">Enable or disable a runbook</span></span>
<span data-ttu-id="48f04-147">Ha egy runbook riasztás konfigurálva van, bármikor letilthatja azt hello runbook konfiguráció eltávolítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="48f04-147">If you have a runbook configured for an alert, you can disable it without removing hello runbook configuration.</span></span> <span data-ttu-id="48f04-148">Ez lehetővé teszi a tookeep hello riasztás fut, és lehet, hogy tesztelése egyes hello riasztási szabályok, és majd később újra engedélyezheti hello runbook.</span><span class="sxs-lookup"><span data-stu-id="48f04-148">This allows you tookeep hello alert running and perhaps test some of hello alert rules and then later re-enable hello runbook.</span></span>

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a><span data-ttu-id="48f04-149">Hozzon létre egy runbookot, amely az Azure riasztás</span><span class="sxs-lookup"><span data-stu-id="48f04-149">Create a runbook that works with an Azure alert</span></span>
<span data-ttu-id="48f04-150">Ha egy runbook Azure riasztási szabály részeként, hello runbook kell azt a toohave logikai toomanage hello riasztási adatokat tooit átadott.</span><span class="sxs-lookup"><span data-stu-id="48f04-150">When you choose a runbook as part of an Azure alert rule, hello runbook needs toohave logic in it toomanage hello alert data that is passed tooit.</span></span>  <span data-ttu-id="48f04-151">Amikor egy runbook a riasztási szabály van beállítva, a webhook jön létre hello runbook; a webhook nem használt toostart hello runbook minden alkalommal hello riasztási eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="48f04-151">When a runbook is configured in an alert rule, a webhook is created for hello runbook; that webhook is then used toostart hello runbook each time hello alert triggers.</span></span>  <span data-ttu-id="48f04-152">hello tényleges hívás toostart hello runbook egy HTTP POST kérelem toohello webhook URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="48f04-152">hello actual call toostart hello runbook is an HTTP POST request toohello webhook URL.</span></span> <span data-ttu-id="48f04-153">hello POST kérelem törzse hello hasznos tulajdonságok kapcsolódó toohello riasztást tartalmazó JSON-formátumú objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="48f04-153">hello body of hello POST request contains a JSON-formated object that contains useful properties related toohello alert.</span></span>  <span data-ttu-id="48f04-154">Mint az alább látható, a hello riasztási adatokat adatait, például az előfizetés-azonosító, erőforráscsoport-név, resourceName és resourceType tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="48f04-154">As you can see below, hello alert data contains details like subscriptionID, resourceGroupName, resourceName, and resourceType.</span></span>

### <a name="example-of-alert-data"></a><span data-ttu-id="48f04-155">Riasztási adatokat – példa</span><span class="sxs-lookup"><span data-stu-id="48f04-155">Example of Alert data</span></span>
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

<span data-ttu-id="48f04-156">Automation-webhook szolgáltatás hello fogadásakor hello HTTP POST kibontja hello riasztási adatokat, és továbbítja azokat hello WebhookData runbook bemeneti paraméter toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="48f04-156">When hello Automation webhook service receives hello HTTP POST it extracts hello alert data and passes it toohello runbook in hello WebhookData runbook input parameter.</span></span>  <span data-ttu-id="48f04-157">Az alábbiakban van a minta-runbookhoz bemutatja, hogyan toouse hello WebhookData paraméter, bontsa ki a riasztási adatokat hello és toomanage hello hello riasztást kiváltó Azure erőforráscsoport használja.</span><span class="sxs-lookup"><span data-stu-id="48f04-157">Below is a sample runbook that shows how toouse hello WebhookData parameter and extract hello alert data and use it toomanage hello Azure resource that triggered hello alert.</span></span>

### <a name="example-runbook"></a><span data-ttu-id="48f04-158">Példa runbook</span><span class="sxs-lookup"><span data-stu-id="48f04-158">Example runbook</span></span>
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

## <a name="summary"></a><span data-ttu-id="48f04-159">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="48f04-159">Summary</span></span>
<span data-ttu-id="48f04-160">Ha olyan riasztást konfigurálhat egy Azure virtuális gépen, most már rendelkezik hello képességét tooeasily konfigurálása egy automatizálási runbook tooautomatically szervizelési művelet végre, ha elindítja a hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="48f04-160">When you configure an alert on an Azure VM, you now have hello ability tooeasily configure an Automation runbook tooautomatically perform remediation action when hello alert triggers.</span></span> <span data-ttu-id="48f04-161">Ebben a kiadásban választhat runbookok toorestart, állítsa le, vagy egy virtuális Gépet, attól függően, hogy a riasztási forgatókönyv törlése.</span><span class="sxs-lookup"><span data-stu-id="48f04-161">For this release, you can choose from runbooks toorestart, stop, or delete a VM depending on your alert scenario.</span></span> <span data-ttu-id="48f04-162">Ez a forgatókönyvek, ahol megadhatja a hello műveletek (értesítés, hibaelhárítási, szervizelés), amely automatikusan amikor elindítja a riasztás engedélyezése csak hello elejére.</span><span class="sxs-lookup"><span data-stu-id="48f04-162">This is just hello beginning of enabling scenarios where you control hello actions (notification, troubleshooting, remediation) that will be taken automatically when an alert triggers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48f04-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48f04-163">Next Steps</span></span>
* <span data-ttu-id="48f04-164">Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="48f04-164">tooget started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span>
* <span data-ttu-id="48f04-165">a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="48f04-165">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="48f04-166">További információ az runbooktípusokkal, azok előnyeit, és a korlátozásokról toolearn lásd [Azure Automation-runbook típusok](automation-runbook-types.md)</span><span class="sxs-lookup"><span data-stu-id="48f04-166">toolearn more about runbook types, their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md)</span></span>

