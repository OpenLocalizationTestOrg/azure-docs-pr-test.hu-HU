---
title: " Automation-Runbook riasztások Azure virtuális gép javítása |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan lehet Azure virtuális gép riasztások integrálása az Azure Automation-forgatókönyv és a problémák automatikus javítása"
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
ms.openlocfilehash: 738959b8e1ee5da989bb996d1ce8148cbf912781
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a><span data-ttu-id="bfe3a-103">Azure Automation-forgatókönyv - javítása Azure virtuális gép riasztások</span><span class="sxs-lookup"><span data-stu-id="bfe3a-103">Azure Automation scenario - remediate Azure VM alerts</span></span>
<span data-ttu-id="bfe3a-104">Azure Automation és Azure virtuális gépek kiadott lehet konfigurálni a virtuális gép (VM) riasztások automatizálási runbookok futtatására új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-104">Azure Automation and Azure Virtual Machines have released a new feature allowing you to configure Virtual Machine (VM) alerts to run Automation runbooks.</span></span> <span data-ttu-id="bfe3a-105">Ezen új szolgáltatás lehetővé teszi, hogy automatikusan szervizelés szabványos VM riasztás, például a virtuális gép leállítaná.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-105">This new capability allows you to automatically perform standard remediation in response to VM alerts, like restarting or stopping the VM.</span></span>

<span data-ttu-id="bfe3a-106">Korábban, a Virtuálisgép-riasztási szabály létrehozása során létrehozott és [adja meg az Automation-webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) egy runbook a runbook futtatásához, amikor a figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-106">Previously, during VM alert rule creation you were able to [specify an Automation webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) to a runbook in order to run the runbook whenever the alert triggered.</span></span> <span data-ttu-id="bfe3a-107">Azonban ez szükséges a runbook létrehozása, a runbook webhook létrehozása másolása és beillesztése a webhook riasztási szabály létrehozása során munkájának elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-107">However, this required you to do the work of creating the runbook, creating the webhook for the runbook, and then copying and pasting the webhook during alert rule creation.</span></span> <span data-ttu-id="bfe3a-108">Új ezzel a kiadással a folyamat nem sokkal könnyebben mert közvetlenül választhat egy runbook listáját riasztási szabály létrehozása során, és választhat egy Automation-fiók, amelyek a runbook futtatásához, illetve könnyen hozzon létre egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-108">With this new release, the process is much easier because you can directly choose a runbook from a list during alert rule creation, and you can choose an Automation account which will run the runbook or easily create an account.</span></span>

<span data-ttu-id="bfe3a-109">Ebben a cikkben láthatja, milyen egyszerűen az Azure virtuális gép értesítés beállítása és konfigurálása egy Automation-runbook elindul, ha a riasztás akkor váltja ki.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-109">In this article, we will show you how easy it is to set up an Azure VM alert and configure an Automation runbook to run whenever the alert triggers.</span></span> <span data-ttu-id="bfe3a-110">Példaforgatókönyvek közé tartozik a virtuális gép újraindítása, ha a memóriahasználat meghaladja a virtuális Géphez a memóriavesztés alkalmazás miatt egyes küszöbértéket, vagy egy virtuális gép leállítása, ha a Processzor felhasználói idő alatt 1 % az elmúlt egy órában, és nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-110">Example scenarios include restarting a VM when the memory usage exceeds some threshold due to an application on the VM with a memory leak, or stopping a VM when the CPU user time has been below 1% for past hour and is not in use.</span></span> <span data-ttu-id="bfe3a-111">Azt is ismertetjük, hogyan egy szolgáltatásnév az Automation-fiók automatikus létrehozása egyszerűbbé teszi a runbookok Azure riasztási szervizelés használatát.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-111">We’ll also explain how the automated creation of a service principal in your Automation account simplifies the use of runbooks in Azure alert remediation.</span></span>

## <a name="create-an-alert-on-a-vm"></a><span data-ttu-id="bfe3a-112">Riasztás létrehozása a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="bfe3a-112">Create an alert on a VM</span></span>
<span data-ttu-id="bfe3a-113">A következő lépésekkel, konfigurálhat egy riasztást az küszöböt teljesülésekor runbook elindítására.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-113">Perform the following steps to configure an alert to launch a runbook when its threshold has been met.</span></span>

> [!NOTE]
> <span data-ttu-id="bfe3a-114">Ebben a kiadásban csak támogatjuk V2 virtuális gép és a támogatás klasszikus virtuális gépek hamarosan megjelenik.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-114">With this release, we only support V2 virtual machines and support for classic VMs will be added soon.</span></span>  
> 
> 

1. <span data-ttu-id="bfe3a-115">Jelentkezzen be az Azure-portálon, majd kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-115">Log in to the Azure portal and click **Virtual Machines**.</span></span>  
2. <span data-ttu-id="bfe3a-116">A virtuális gépek közül.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-116">Select one of your virtual machines.</span></span>  <span data-ttu-id="bfe3a-117">A virtuális gép irányítópult panel jelenik meg és a **beállítások** a jobb oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-117">The virtual machine dashboard blade will appear and the **Settings** blade to its right.</span></span>  
3. <span data-ttu-id="bfe3a-118">Az a **beállítások** panelen, a figyelés szakaszban válassza a **riasztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-118">From the **Settings** blade, under the Monitoring section select **Alert rules**.</span></span>
4. <span data-ttu-id="bfe3a-119">Az a **riasztási szabályok** panelen kattintson a **riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-119">On the **Alert rules** blade, click **Add alert**.</span></span>

<span data-ttu-id="bfe3a-120">Ezzel megnyílik a **riasztási szabály felvétele** panel, ahol konfigurálhatja a riasztás feltételeit, és egy vagy több ezek a lehetőségek közül választhat: valaki e-mailt küldeni, a webhook használatával továbbítja a riasztást egy másik rendszerre, és/vagy futtassa egy Automation-runbook válaszul próbálja meg kijavítani a problémát.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-120">This opens up the **Add an alert rule** blade, where you can configure the conditions for the alert and choose among one or all of these options: send email to someone, use a webhook to forward the alert to another system, and/or run an Automation runbook in response attempt to remediate the issue.</span></span>

## <a name="configure-a-runbook"></a><span data-ttu-id="bfe3a-121">A runbook konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bfe3a-121">Configure a runbook</span></span>
<span data-ttu-id="bfe3a-122">Egy runbook futtatására, ha a virtuális gép riasztási küszöbérték elérése konfigurálásához jelölje ki **Automation-Runbook**.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-122">To configure a runbook to run when the VM alert threshold is met, select **Automation Runbook**.</span></span> <span data-ttu-id="bfe3a-123">Az a **runbook konfigurálása** panelen kiválaszthatja a runbook futtatását és az Automation-fiók a runbook futtatását.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-123">In the **Configure runbook** blade, you can select the runbook to run and the Automation account to run the runbook in.</span></span>

![Az Automation-runbook konfigurálása és új Automation-fiók létrehozása](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> <span data-ttu-id="bfe3a-125">Ebben a kiadásban választhat három runbookokat, a szolgáltatás biztosítja – indítsa újra a virtuális gép, állítsa le a virtuális gép vagy távolítsa el a virtuális gép (törölni).</span><span class="sxs-lookup"><span data-stu-id="bfe3a-125">For this release you can choose from three runbooks that the service provides – Restart VM, Stop VM, or Remove VM (delete it).</span></span>  <span data-ttu-id="bfe3a-126">Választhatók ki az egyéb forgatókönyvek vagy a saját runbookok egy későbbi kiadásban elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-126">The ability to select other runbooks or one of your own runbooks will be available in a future release.</span></span>
> 
> 

![Választhat a Runbookok](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

<span data-ttu-id="bfe3a-128">Miután kiválasztotta a három rendelkezésre álló runbookok egyikét a **Automation-fiók** legördülő lista jelenik meg, és kiválaszthatja, hogy fut a runbook automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-128">After you select one of the three available runbooks, the **Automation account** drop-down list appears and you can select an automation account the runbook will run as.</span></span> <span data-ttu-id="bfe3a-129">Runbookok környezetében futtatnia kell egy [Automation-fiók](automation-security-overview.md) szerepel az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-129">Runbooks need to run in the context of an [Automation account](automation-security-overview.md) that is in your Azure subscription.</span></span> <span data-ttu-id="bfe3a-130">Kiválaszthatja, hogy már létrehozta, vagy beállíthatja, hogy a hozott létre az Ön új Automation-fiók Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-130">You can select an Automation account that you already created, or you can have a new Automation account created for you.</span></span>

<span data-ttu-id="bfe3a-131">A runbookok által biztosított hitelesítésre egyszerű szolgáltatás használatával.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-131">The runbooks that are provided authenticate to Azure using a service principal.</span></span> <span data-ttu-id="bfe3a-132">Ha a runbook futtatásához a meglévő Automation-fiókok egyikével, azt automatikusan a szolgáltatás egyszerű hozza létre.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-132">If you choose to run the runbook in one of your existing Automation accounts, we will automatically create the service principal for you.</span></span> <span data-ttu-id="bfe3a-133">Ha egy új Automation-fiók létrehozása mellett dönt, majd automatikusan létrehozunk a fiók és a szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-133">If you choose to create a new Automation account, then we will automatically create the account and the service principal.</span></span> <span data-ttu-id="bfe3a-134">Mindkét esetben két eszközök is létrejön az Automation-fiók – a tanúsítvány eszköz nevű **AzureRunAsCertificate** és a kapcsolódási eszköz nevű **AzureRunAsConnection**.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-134">In both cases, two assets will also be created in the Automation account – a certificate asset named **AzureRunAsCertificate** and a connection asset named **AzureRunAsConnection**.</span></span> <span data-ttu-id="bfe3a-135">A runbookok használandó **AzureRunAsConnection** hitelesítéséhez az Azure-ral ahhoz, hogy a virtuális gép elleni felügyeleti művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-135">The runbooks will use **AzureRunAsConnection** to authenticate with Azure in order to perform the management action against the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="bfe3a-136">A szolgáltatás egyszerű az előfizetési hatókört jön létre, és hozzá van rendelve a közreműködő szerepkört.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-136">The service principal is created in the subscription scope and is assigned the Contributor role.</span></span> <span data-ttu-id="bfe3a-137">Ez a szerepkör szükséges ahhoz, hogy a fiók automatizálási runbookok futtatását Azure virtuális gépek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-137">This role is required in order for the account to have permission to run Automation runbooks to manage Azure VMs.</span></span>  <span data-ttu-id="bfe3a-138">Egy Automaton fiók és/vagy az egyszerű szolgáltatás létrehozása az egyszeri esemény.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-138">The creation of an Automaton account and/or service principal is a one-time event.</span></span> <span data-ttu-id="bfe3a-139">A létrehozásuk után a fiók segítségével más Azure virtuális gép riasztások runbookot futtatni.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-139">Once they are created, you can use that account to run runbooks for other Azure VM alerts.</span></span>
> 
> 

<span data-ttu-id="bfe3a-140">Amikor rákattint **OK** a riasztás van konfigurálva, és ha az új Automation-fiók létrehozása lehetőséget választotta, létrejön az egyszerű szolgáltatás együtt.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-140">When you click **OK** the alert is configured and if you selected the option to create a new Automation account, it is created along with the service principal.</span></span>  <span data-ttu-id="bfe3a-141">Ez eltarthat néhány másodpercig is.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-141">This can take a few seconds to complete.</span></span>  

![A Runbook konfigurálva](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

<span data-ttu-id="bfe3a-143">A konfiguráció befejezése után megjelenik a runbook neve jelenik meg a **riasztási szabály felvétele** panelen.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-143">After the configuration is completed you will see the name of the runbook appear in the **Add an alert rule** blade.</span></span>

![A Runbook konfigurálva](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

<span data-ttu-id="bfe3a-145">Kattintson a **OK** a a **riasztási szabály felvétele** panel megnyitásához, és a riasztási szabály jön létre, és aktiválja, ha a virtuális gép futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-145">Click **OK** in the **Add an alert rule** blade and the alert rule will be created and activate if the virtual machine is in a running state.</span></span>

### <a name="enable-or-disable-a-runbook"></a><span data-ttu-id="bfe3a-146">Engedélyezheti vagy tilthatja le a runbookot</span><span class="sxs-lookup"><span data-stu-id="bfe3a-146">Enable or disable a runbook</span></span>
<span data-ttu-id="bfe3a-147">Ha riasztást beállított runbookok, bármikor letilthatja azt a runbook-konfiguráció eltávolítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-147">If you have a runbook configured for an alert, you can disable it without removing the runbook configuration.</span></span> <span data-ttu-id="bfe3a-148">Ez lehetővé teszi a riasztás fut, és lehet, hogy egyes a riasztási szabályok tesztelése és majd később újra engedélyezheti a runbook.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-148">This allows you to keep the alert running and perhaps test some of the alert rules and then later re-enable the runbook.</span></span>

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a><span data-ttu-id="bfe3a-149">Hozzon létre egy runbookot, amely az Azure riasztás</span><span class="sxs-lookup"><span data-stu-id="bfe3a-149">Create a runbook that works with an Azure alert</span></span>
<span data-ttu-id="bfe3a-150">Ha egy runbook Azure riasztási szabály részeként, a runbook a hozzá riasztási adatok kezelésére logika rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-150">When you choose a runbook as part of an Azure alert rule, the runbook needs to have logic in it to manage the alert data that is passed to it.</span></span>  <span data-ttu-id="bfe3a-151">Amikor egy runbook a riasztási szabály van beállítva, a webhook jön létre a runbook; a webhook ezután elindítja a runbookot minden alkalommal, amikor elindítja a riasztás segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-151">When a runbook is configured in an alert rule, a webhook is created for the runbook; that webhook is then used to start the runbook each time the alert triggers.</span></span>  <span data-ttu-id="bfe3a-152">A tényleges hívás a runbook futtatására egy HTTP POST-kérelmet a webhook URL-cím.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-152">The actual call to start the runbook is an HTTP POST request to the webhook URL.</span></span> <span data-ttu-id="bfe3a-153">A POST kérelem törzse a riasztással kapcsolatban hasznos tulajdonságokat tartalmazó JSON-formátumú objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-153">The body of the POST request contains a JSON-formated object that contains useful properties related to the alert.</span></span>  <span data-ttu-id="bfe3a-154">Mint az alább látható, a riasztási adatokat tartalmaz adatait, például az előfizetés-azonosító, erőforráscsoport-név, resourceName és resourceType.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-154">As you can see below, the alert data contains details like subscriptionID, resourceGroupName, resourceName, and resourceType.</span></span>

### <a name="example-of-alert-data"></a><span data-ttu-id="bfe3a-155">Riasztási adatokat – példa</span><span class="sxs-lookup"><span data-stu-id="bfe3a-155">Example of Alert data</span></span>
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

<span data-ttu-id="bfe3a-156">Amikor az Automation-webhook szolgáltatás megkapja a HTTP POST bontja ki a riasztási adatokat, és átadja a runbook a runbook WebhookData bemeneti paraméter.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-156">When the Automation webhook service receives the HTTP POST it extracts the alert data and passes it to the runbook in the WebhookData runbook input parameter.</span></span>  <span data-ttu-id="bfe3a-157">Alább a minta-runbookhoz, amely bemutatja, hogyan használja a WebhookData paramétert, és bontsa ki a riasztási adatokat, majd a riasztást kiváltó Azure-erőforrás kezeléséhez van.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-157">Below is a sample runbook that shows how to use the WebhookData parameter and extract the alert data and use it to manage the Azure resource that triggered the alert.</span></span>

### <a name="example-runbook"></a><span data-ttu-id="bfe3a-158">Példa runbook</span><span class="sxs-lookup"><span data-stu-id="bfe3a-158">Example runbook</span></span>
```
#  This runbook will restart an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a><span data-ttu-id="bfe3a-159">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="bfe3a-159">Summary</span></span>
<span data-ttu-id="bfe3a-160">Ha olyan riasztást konfigurálhat egy Azure virtuális gépen, hogy könnyen állítson be egy Automation-runbook automatikusan végrehajtani a javítási művelet, ha a riasztás Elindítja most már rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-160">When you configure an alert on an Azure VM, you now have the ability to easily configure an Automation runbook to automatically perform remediation action when the alert triggers.</span></span> <span data-ttu-id="bfe3a-161">Ebben a kiadásban közül választhat a runbookok, állítsa le, vagy törölje a virtuális gépek, a riasztás forgatókönyvtől függően.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-161">For this release, you can choose from runbooks to restart, stop, or delete a VM depending on your alert scenario.</span></span> <span data-ttu-id="bfe3a-162">Ez az imént elejére forgatókönyvek, ahol megadhatja a műveleteket (értesítés, hibaelhárítási, szervizelés), amely automatikusan amikor elindítja a riasztás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bfe3a-162">This is just the beginning of enabling scenarios where you control the actions (notification, troubleshooting, remediation) that will be taken automatically when an alert triggers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfe3a-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bfe3a-163">Next Steps</span></span>
* <span data-ttu-id="bfe3a-164">A grafikus forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első grafikus forgatókönyvem](automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="bfe3a-164">To get started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span>
* <span data-ttu-id="bfe3a-165">A PowerShell-alapú munkafolyamat-forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első PowerShell-alapú munkafolyamat-forgatókönyvem](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="bfe3a-165">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="bfe3a-166">További információk a forgatókönyvek típusairól, az előnyeikről és a korlátaikról: [Az Azure Automation forgatókönyveinek típusai](automation-runbook-types.md)</span><span class="sxs-lookup"><span data-stu-id="bfe3a-166">To learn more about runbook types, their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md)</span></span>

