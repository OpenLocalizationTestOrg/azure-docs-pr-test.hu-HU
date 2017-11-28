---
title: "aaaAzure virtulálisgép-skálázási készletekben – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toofrequently kérdések a virtuálisgép-méretezési csoportok beolvasása."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: 0deb9e2bb79f87f17bbf748397b94dc53070cfbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a><span data-ttu-id="edaf8-103">Az Azure virtuálisgép-skálázási készletekben – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="edaf8-103">Azure virtual machine scale sets FAQs</span></span>

<span data-ttu-id="edaf8-104">A válaszok kérdések a virtuálisgép-méretezési toofrequently beállítja az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="edaf8-104">Get answers toofrequently asked questions about virtual machine scale sets in Azure.</span></span>

## <a name="autoscale"></a><span data-ttu-id="edaf8-105">Automatikus méretezés</span><span class="sxs-lookup"><span data-stu-id="edaf8-105">Autoscale</span></span>

### <a name="what-are-best-practices-for-azure-autoscale"></a><span data-ttu-id="edaf8-106">Mik az Azure automatikus skálázás ajánlott eljárásai?</span><span class="sxs-lookup"><span data-stu-id="edaf8-106">What are best practices for Azure Autoscale?</span></span>

<span data-ttu-id="edaf8-107">Ajánlott eljárások az automatikus skálázás, lásd: [ajánlott eljárások a virtuális gépek automatikus skálázás](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span><span class="sxs-lookup"><span data-stu-id="edaf8-107">For best practices for Autoscale, see [Best practices for autoscaling virtual machines](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span></span>

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a><span data-ttu-id="edaf8-108">Hol található a következő automatikus skálázás állomásalapú metrikák használó metrika neve?</span><span class="sxs-lookup"><span data-stu-id="edaf8-108">Where do I find metric names for autoscaling that uses host-based metrics?</span></span>

<span data-ttu-id="edaf8-109">Automatikus skálázás állomásalapú metrikák használó mérték nevét, lásd: [támogatott Azure-figyelő metrikák](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-109">For metric names for autoscaling that uses host-based metrics, see [Supported metrics with Azure Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span></span>

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a><span data-ttu-id="edaf8-110">Vannak-e az Azure Service Bus témakör és a várólista hossza alapján automatikus skálázás bármely példái?</span><span class="sxs-lookup"><span data-stu-id="edaf8-110">Are there any examples of autoscaling based on an Azure Service Bus topic and queue length?</span></span>

<span data-ttu-id="edaf8-111">Igen.</span><span class="sxs-lookup"><span data-stu-id="edaf8-111">Yes.</span></span> <span data-ttu-id="edaf8-112">Az Azure Service Bus témakör és a várólista hossza alapján automatikus skálázás példákért lásd [Azure figyelő automatikus skálázás közös metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-112">For examples of autoscaling based on an Azure Service Bus topic and queue length, see [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span></span>

<span data-ttu-id="edaf8-113">A Service Bus-üzenetsorba használja a következő JSON hello:</span><span class="sxs-lookup"><span data-stu-id="edaf8-113">For a Service Bus queue, use hello following JSON:</span></span>

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

<span data-ttu-id="edaf8-114">Tároló várólista a következő JSON hello használata:</span><span class="sxs-lookup"><span data-stu-id="edaf8-114">For a storage queue, use hello following JSON:</span></span>

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

<span data-ttu-id="edaf8-115">Cserélje a erőforrás egységes erőforrás-azonosítók (URI).</span><span class="sxs-lookup"><span data-stu-id="edaf8-115">Replace example values with your resource Uniform Resource Identifiers (URIs).</span></span>


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a><span data-ttu-id="edaf8-116">Kell I Automatikus méretezéssel állomásalapú metrikák vagy a diagnosztika bővítmény használatával?</span><span class="sxs-lookup"><span data-stu-id="edaf8-116">Should I autoscale by using host-based metrics or a diagnostics extension?</span></span>

<span data-ttu-id="edaf8-117">Az automatikus skálázási beállítás létrehozhat egy virtuális gép toouse gazdaszintű metrikák vagy Vendég operációsrendszer-alapú metrikákat.</span><span class="sxs-lookup"><span data-stu-id="edaf8-117">You can create an autoscale setting on a VM toouse host-level metrics or guest OS-based metrics.</span></span>

<span data-ttu-id="edaf8-118">A támogatott mérőszámok listájáért lásd: [Azure figyelő automatikus skálázás közös metrikák](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span><span class="sxs-lookup"><span data-stu-id="edaf8-118">For a list of supported metrics, see [Azure Monitor autoscaling common metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span></span> 

<span data-ttu-id="edaf8-119">A teljes minta a virtuálisgép-méretezési csoportok, lásd: [speciális automatikus skálázás konfigurációs Resource Manager-sablonok használatával virtuálisgép-méretezési csoportok](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span><span class="sxs-lookup"><span data-stu-id="edaf8-119">For a full sample for virtual machine scale sets, see [Advanced autoscale configuration by using Resource Manager templates for virtual machine scale sets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span></span> 

<span data-ttu-id="edaf8-120">hello minta hello gazdaszintű CPU metrika és üzenet mérőszámot használja.</span><span class="sxs-lookup"><span data-stu-id="edaf8-120">hello sample uses hello host-level CPU metric and a message count metric.</span></span>



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-121">Hogyan állíthatom riasztási szabályok a virtuálisgép-méretezési csoportot?</span><span class="sxs-lookup"><span data-stu-id="edaf8-121">How do I set alert rules on a virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-122">A PowerShell vagy az Azure parancssori felület használatával virtuálisgép-méretezési csoportok metrikáját riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="edaf8-122">You can create alerts on metrics for virtual machine scale sets via PowerShell or Azure CLI.</span></span> <span data-ttu-id="edaf8-123">További információkért lásd: [Azure figyelő PowerShell quick start minták](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) és [figyelő Azure platformfüggetlen parancssori felület gyors start minták](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span><span class="sxs-lookup"><span data-stu-id="edaf8-123">For more information, see [Azure Monitor PowerShell quick start samples](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) and [Azure Monitor cross-platform CLI quick start samples](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span></span>

<span data-ttu-id="edaf8-124">hello hello virtuálisgép-méretezési csoport targetresourceid azonosítója így néz ki:</span><span class="sxs-lookup"><span data-stu-id="edaf8-124">hello TargetResourceId of hello virtual machine scale set looks like this:</span></span> 

<span data-ttu-id="edaf8-125">/Subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/Providers/Microsoft.COMPUTE/virtualMachineScaleSets/yourvmssname</span><span class="sxs-lookup"><span data-stu-id="edaf8-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span></span>

<span data-ttu-id="edaf8-126">Dönthet úgy, minden virtuális gép teljesítményszámláló hello metrika tooset, egy riasztás.</span><span class="sxs-lookup"><span data-stu-id="edaf8-126">You can choose any VM performance counter as hello metric tooset an alert for.</span></span> <span data-ttu-id="edaf8-127">További információkért lásd: [Resource Manager-alapú Windows virtuális gépek vendég operációs rendszer metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) és [Linux virtuális gépek vendég operációs rendszer metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) a hello [Azure figyelő automatikus skálázás közös metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)cikk.</span><span class="sxs-lookup"><span data-stu-id="edaf8-127">For more information, see [Guest OS metrics for Resource Manager-based Windows VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) and [Guest OS metrics for Linux VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) in hello [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) article.</span></span>

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a><span data-ttu-id="edaf8-128">Hogyan állíthatom be automatikus skálázás a PowerShell segítségével állítsa be a virtuálisgép-méretezési?</span><span class="sxs-lookup"><span data-stu-id="edaf8-128">How do I set up autoscale on a virtual machine scale set by using PowerShell?</span></span>

<span data-ttu-id="edaf8-129">automatikus skálázás a PowerShell segítségével állítsa be a virtuálisgép-méretezési mentése tooset lásd hello blogbejegyzésben [tooadd automatikus skálázás tooan Azure virtuálisgép-méretezési beállításának módját](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-129">tooset up autoscale on a virtual machine scale set by using PowerShell, see hello blog post [How tooadd autoscale tooan Azure virtual machine scale set](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span></span>




## <a name="certificates"></a><span data-ttu-id="edaf8-130">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="edaf8-130">Certificates</span></span>

### <a name="how-do-i-securely-ship-a-certificate-toohello-vm-how-do-i-provision-a-virtual-machine-scale-set-toorun-a-website-where-hello-ssl-for-hello-website-is-shipped-securely-from-a-certificate-configuration-hello-common-certificate-rotation-operation-would-be-almost-hello-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-toodo-this"></a><span data-ttu-id="edaf8-131">Hogyan tegye szeretnék biztonságosan küldje el a tanúsítvány toohello VM?</span><span class="sxs-lookup"><span data-stu-id="edaf8-131">How do I securely ship a certificate toohello VM?</span></span> <span data-ttu-id="edaf8-132">Hogyan kiépíteni a virtuális gép méretezési készlet toorun egy webhelyet, ahol hello hello webhelyének SSL nyújtják biztonságosan tanúsítvány konfigurációról?</span><span class="sxs-lookup"><span data-stu-id="edaf8-132">How do I provision a virtual machine scale set toorun a website where hello SSL for hello website is shipped securely from a certificate configuration?</span></span> <span data-ttu-id="edaf8-133">(hello közös tanúsítvány forgatási művelet kellene lennie szinte hello ugyanaz, mint a konfigurációs-frissítési művelet.) Rendelkezik egy példa bemutatja, hogyan toodo Ez?</span><span class="sxs-lookup"><span data-stu-id="edaf8-133">(hello common certificate rotation operation would be almost hello same as a configuration update operation.) Do you have an example of how toodo this?</span></span> 

<span data-ttu-id="edaf8-134">toosecurely küldje el a tanúsítvány toohello VM, közvetlenül tanúsítványtárolóba a Windows hello ügyfél kulcstároló a is telepíthet a felhasználói tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="edaf8-134">toosecurely ship a certificate toohello VM, you can install a customer certificate directly into a Windows certificate store from hello customer's key vault.</span></span>

<span data-ttu-id="edaf8-135">A következő JSON hello használata:</span><span class="sxs-lookup"><span data-stu-id="edaf8-135">Use hello following JSON:</span></span>

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

<span data-ttu-id="edaf8-136">hello kód támogatja a Windows és Linux.</span><span class="sxs-lookup"><span data-stu-id="edaf8-136">hello code supports Windows and Linux.</span></span>

<span data-ttu-id="edaf8-137">További információkért lásd: [létrehozás vagy frissítés egy virtuálisgép-méretezési készlet](https://msdn.microsoft.com/library/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="edaf8-137">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/mt589035.aspx).</span></span>


### <a name="example-of-self-signed-certificate"></a><span data-ttu-id="edaf8-138">Önaláírt tanúsítvány – példa</span><span class="sxs-lookup"><span data-stu-id="edaf8-138">Example of Self-signed certificate</span></span>

1.  <span data-ttu-id="edaf8-139">Hozzon létre egy önaláírt tanúsítványt kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="edaf8-139">Create a self-signed certificate in a key vault.</span></span>

    <span data-ttu-id="edaf8-140">A következő PowerShell-parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="edaf8-140">Use hello following PowerShell commands:</span></span>

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    <span data-ttu-id="edaf8-141">A parancs által biztosított, hogy hello hello Azure Resource Manager-sablon a megadott.</span><span class="sxs-lookup"><span data-stu-id="edaf8-141">This command gives you hello input for hello Azure Resource Manager template.</span></span>

    <span data-ttu-id="edaf8-142">Hogyan toocreate a kulcstároló, önaláírt tanúsítvány: példa [Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-142">For an example of how toocreate a self-signed certificate in a key vault, see [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span></span>

2.  <span data-ttu-id="edaf8-143">Hello Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="edaf8-143">Change hello Resource Manager template.</span></span>

    <span data-ttu-id="edaf8-144">Ez a tulajdonság túl hozzáadása**virtualMachineProfile**, hello részeként virtuálisgép-méretezési készlet erőforrás:</span><span class="sxs-lookup"><span data-stu-id="edaf8-144">Add this property too**virtualMachineProfile**, as part of hello virtual machine scale set resource:</span></span>

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-toouse-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a><span data-ttu-id="edaf8-145">Állítható be egy SSH-kulcspár toouse SSH hitelesítés állítson be úgy a Resource Manager-sablon a Linux virtuális gép skálával?</span><span class="sxs-lookup"><span data-stu-id="edaf8-145">Can I specify an SSH key pair toouse for SSH authentication with a Linux virtual machine scale set from a Resource Manager template?</span></span>  

<span data-ttu-id="edaf8-146">Igen.</span><span class="sxs-lookup"><span data-stu-id="edaf8-146">Yes.</span></span> <span data-ttu-id="edaf8-147">a REST API hello **osProfile** hasonló toohello szabványos VM REST API.</span><span class="sxs-lookup"><span data-stu-id="edaf8-147">hello REST API for **osProfile** is similar toohello standard VM REST API.</span></span> 

<span data-ttu-id="edaf8-148">Tartalmaznak **osProfile** a sablonban:</span><span class="sxs-lookup"><span data-stu-id="edaf8-148">Include **osProfile** in your template:</span></span>

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
<span data-ttu-id="edaf8-149">Ez a JSON-tömb használatos [hello 101-vm-ssh-kulcsfájl GitHub gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="edaf8-149">This JSON block is used in [hello 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>
 
<span data-ttu-id="edaf8-150">hello operációsrendszer-profil is van használatban [hello grelayhost.json GitHub quick start sablon](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span><span class="sxs-lookup"><span data-stu-id="edaf8-150">hello OS profile also is used in [hello grelayhost.json GitHub quick start template](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span></span>

<span data-ttu-id="edaf8-151">További információkért lásd: [létrehozás vagy frissítés egy virtuálisgép-méretezési készlet](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span><span class="sxs-lookup"><span data-stu-id="edaf8-151">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span></span>
  

### <a name="how-do-i-remove-deprecated-certificates"></a><span data-ttu-id="edaf8-152">Hogyan távolíthatom elavult tanúsítványok?</span><span class="sxs-lookup"><span data-stu-id="edaf8-152">How do I remove deprecated certificates?</span></span> 

<span data-ttu-id="edaf8-153">tooremove tanúsítványok eltávolítása hello régi tanúsítványt hello tároló tanúsítványok listája elavult.</span><span class="sxs-lookup"><span data-stu-id="edaf8-153">tooremove deprecated certificates, remove hello old certificate from hello vault certificates list.</span></span> <span data-ttu-id="edaf8-154">Hagyja meg minden hello tanúsítványt, amelyet az tooremain hello listában a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="edaf8-154">Leave all hello certificates that you want tooremain on your computer in hello list.</span></span> <span data-ttu-id="edaf8-155">Ez a virtuális gépek nem távolítja el hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="edaf8-155">This does not remove hello certificate from all your VMs.</span></span> <span data-ttu-id="edaf8-156">Azt is nem bővíti ki hello tanúsítvány toonew hello virtuálisgép-méretezési csoportban lévő létrehozott virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="edaf8-156">It also does not add hello certificate toonew VMs that are created in hello virtual machine scale set.</span></span> 

<span data-ttu-id="edaf8-157">tooremove hello tanúsítványt a meglévő virtuális gépeken, parancsfájlokat egyéni kiterjesztés toomanually hello tanúsítványok eltávolítása a tanúsítványt a tanúsítványtárolóból.</span><span class="sxs-lookup"><span data-stu-id="edaf8-157">tooremove hello certificate from existing VMs, write a custom script extension toomanually remove hello certificates from your certificate store.</span></span>
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-hello-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-toostore-hello-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a><span data-ttu-id="edaf8-158">Hogyan do I behelyezése egy meglévő nyilvános SSH-kulcs hello virtuális gép méretezési készlet SSH réteg kiépítése során?</span><span class="sxs-lookup"><span data-stu-id="edaf8-158">How do I inject an existing SSH public key into hello virtual machine scale set SSH layer during provisioning?</span></span> <span data-ttu-id="edaf8-159">I toostore hello SSH nyilvános kulcs értékeit az Azure Key Vault szeretne, és használja a Resource Manager sablon.</span><span class="sxs-lookup"><span data-stu-id="edaf8-159">I want toostore hello SSH public key values in Azure Key Vault, and then use them in my Resource Manager template.</span></span>

<span data-ttu-id="edaf8-160">Ha meg van adva hello virtuális gépek csak a nyilvános SSH-kulcsot, nem kell tooput hello nyilvános kulcsokat a Key Vault.</span><span class="sxs-lookup"><span data-stu-id="edaf8-160">If you are providing hello VMs only with a public SSH key, you don't need tooput hello public keys in Key Vault.</span></span> <span data-ttu-id="edaf8-161">Nyilvános kulcsai nem titkos.</span><span class="sxs-lookup"><span data-stu-id="edaf8-161">Public keys are not secret.</span></span>
 
<span data-ttu-id="edaf8-162">Megadhat egyszerű szöveges nyilvános SSH-kulcsok Linux virtuális gép létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="edaf8-162">You can provide SSH public keys in plain text when you create a Linux VM:</span></span>

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
<span data-ttu-id="edaf8-163">linuxConfiguration elem neve</span><span class="sxs-lookup"><span data-stu-id="edaf8-163">linuxConfiguration element name</span></span> | <span data-ttu-id="edaf8-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="edaf8-164">Required</span></span> | <span data-ttu-id="edaf8-165">Típus</span><span class="sxs-lookup"><span data-stu-id="edaf8-165">Type</span></span> | <span data-ttu-id="edaf8-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="edaf8-166">Description</span></span>
--- | --- | --- | --- |  ---
<span data-ttu-id="edaf8-167">ssh</span><span class="sxs-lookup"><span data-stu-id="edaf8-167">ssh</span></span> | <span data-ttu-id="edaf8-168">Nem</span><span class="sxs-lookup"><span data-stu-id="edaf8-168">No</span></span> | <span data-ttu-id="edaf8-169">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="edaf8-169">Collection</span></span> | <span data-ttu-id="edaf8-170">Hello SSH kulcs konfigurációját a Linux operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="edaf8-170">Specifies hello SSH key configuration for a Linux OS</span></span>
<span data-ttu-id="edaf8-171">Elérési út</span><span class="sxs-lookup"><span data-stu-id="edaf8-171">path</span></span> | <span data-ttu-id="edaf8-172">Igen</span><span class="sxs-lookup"><span data-stu-id="edaf8-172">Yes</span></span> | <span data-ttu-id="edaf8-173">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="edaf8-173">String</span></span> | <span data-ttu-id="edaf8-174">Hello Linux elérési útjának megadása ahol hello SSH-kulcsok vagy tanúsítványt kell lennie</span><span class="sxs-lookup"><span data-stu-id="edaf8-174">Specifies hello Linux file path where hello SSH keys or certificate should be located</span></span>
<span data-ttu-id="edaf8-175">keyData</span><span class="sxs-lookup"><span data-stu-id="edaf8-175">keyData</span></span> | <span data-ttu-id="edaf8-176">Igen</span><span class="sxs-lookup"><span data-stu-id="edaf8-176">Yes</span></span> | <span data-ttu-id="edaf8-177">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="edaf8-177">String</span></span> | <span data-ttu-id="edaf8-178">Megadja a base64-kódolású nyilvános SSH-kulcs</span><span class="sxs-lookup"><span data-stu-id="edaf8-178">Specifies a base64-encoded SSH public key</span></span>

<span data-ttu-id="edaf8-179">Egy vonatkozó példáért lásd: [hello 101-vm-ssh-kulcsfájl GitHub gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="edaf8-179">For an example, see [hello 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-hello-same-key-vault-i-see-hello-following-message"></a><span data-ttu-id="edaf8-180">Futtatásakor `Update-AzureRmVmss` egynél több tanúsítvány hozzáadása a hello után ugyanaz a kulcs tárolóban, látom hello a következő üzenetet:</span><span class="sxs-lookup"><span data-stu-id="edaf8-180">When I run `Update-AzureRmVmss` after adding more than one certificate from hello same key vault, I see hello following message:</span></span>
 
><span data-ttu-id="edaf8-181">Frissítés-AzureRmVmss: Lista titkos kulcsot tartalmaz többszöri /subscriptions/ < saját előfizetés-azonosító > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, ami nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="edaf8-181">Update-AzureRmVmss: List secret contains repeated instances of /subscriptions/<my-subscription-id>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, which is disallowed.</span></span>
 
<span data-ttu-id="edaf8-182">Ez akkor fordulhat elő, ha toore-hozzáadása hello azonos tároló hello meglévő forrás tároló egy új tároló tanúsítvány használata helyett.</span><span class="sxs-lookup"><span data-stu-id="edaf8-182">This can happen if you try toore-add hello same vault instead of using a new vault certificate for hello existing source vault.</span></span> <span data-ttu-id="edaf8-183">Hello `Add-AzureRmVmssSecret` parancs nem működik megfelelően, ha további titkok hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="edaf8-183">hello `Add-AzureRmVmssSecret` command does not work correctly if you are adding additional secrets.</span></span>
 
<span data-ttu-id="edaf8-184">tooadd hello a további titkok azonos kulcstároló, a frissítés hello $vmss.properties.osProfile.secrets[0].vaultCertificates lista.</span><span class="sxs-lookup"><span data-stu-id="edaf8-184">tooadd more secrets from hello same key vault, update hello $vmss.properties.osProfile.secrets[0].vaultCertificates list.</span></span>
 
<span data-ttu-id="edaf8-185">Hello várt bemeneti szerkezetből, lásd: [létrehozás vagy frissítés egy virtuálisgép-csoport](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="edaf8-185">For hello expected input structure, see [Create or update a virtual machine set](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span></span>
 
<span data-ttu-id="edaf8-186">Hello virtuális gép méretezési készlet objektum, amely a key vaultban hello hello titkos kulcs található.</span><span class="sxs-lookup"><span data-stu-id="edaf8-186">Find hello secret in hello virtual machine scale set object that is in hello key vault.</span></span> <span data-ttu-id="edaf8-187">Adja hozzá a tanúsítvány (hello URL-cím és hello titkos Tárolónév) toohello hivatkozáslista társított hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="edaf8-187">Then, add your certificate reference (hello URL and hello secret store name) toohello list associated with hello vault.</span></span>

> [!NOTE] 
> <span data-ttu-id="edaf8-188">Jelenleg nem távolítható el tanúsítványok virtuális gépek hello virtuális gép méretezési készlet API használatával.</span><span class="sxs-lookup"><span data-stu-id="edaf8-188">Currently, you cannot remove certificates from VMs by using hello virtual machine scale set API.</span></span>
>

<span data-ttu-id="edaf8-189">Új virtuális gépek nem fognak rendelkezni hello régi tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="edaf8-189">New VMs will not have hello old certificate.</span></span> <span data-ttu-id="edaf8-190">Hello tanúsítvány van, és a már telepített virtuális gépek azonban hello régi tanúsítvánnyal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="edaf8-190">However, VMs that have hello certificate and which are already deployed will have hello old certificate.</span></span>
 
### <a name="can-i-push-certificates-toohello-virtual-machine-scale-set-without-providing-hello-password-when-hello-certificate-is-in-hello-secret-store"></a><span data-ttu-id="edaf8-191">Leküldéses tanúsítványok virtuálisgép-méretezési toohello nélkül hello jelszót, ha hello tanúsítvány titkos tárolójában hello beállítása is?</span><span class="sxs-lookup"><span data-stu-id="edaf8-191">Can I push certificates toohello virtual machine scale set without providing hello password, when hello certificate is in hello secret store?</span></span>

<span data-ttu-id="edaf8-192">Nem kell toohard-kód jelszavak parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="edaf8-192">You do not need toohard-code passwords in scripts.</span></span> <span data-ttu-id="edaf8-193">Jelszavak dinamikusan lekérdezésére hello engedélyekkel toorun hello telepítési parancsfájl használatára.</span><span class="sxs-lookup"><span data-stu-id="edaf8-193">You can dynamically retrieve passwords with hello permissions you use toorun hello deployment script.</span></span> <span data-ttu-id="edaf8-194">Ha egy parancsfájlt, mely egy tanúsítványt az hello titkos tároló kulcstároló, hello titkos tároló `get certificate` parancs is kiírja hello jelszó hello .pfx fájl.</span><span class="sxs-lookup"><span data-stu-id="edaf8-194">If you have a script that moves a certificate from hello secret store key vault, hello secret store `get certificate` command also outputs hello password of hello .pfx file.</span></span>
 
### <a name="how-does-hello-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-hello-sourcevault-value-when-i-have-toospecify-hello-absolute-uri-for-a-certificate-by-using-hello-certificateurl-property"></a><span data-ttu-id="edaf8-195">Hogyan hello titkok tulajdonsága a virtuálisgép-méretezési virtualMachineProfile.osProfile munkahelyi be?</span><span class="sxs-lookup"><span data-stu-id="edaf8-195">How does hello Secrets property of virtualMachineProfile.osProfile for a virtual machine scale set work?</span></span> <span data-ttu-id="edaf8-196">Miért kell meg hello sourceVault értéket, ha abszolút URI-azonosító tanúsítványt hello hello certificateUrl tulajdonság használatával toospecify?</span><span class="sxs-lookup"><span data-stu-id="edaf8-196">Why do I need hello sourceVault value when I have toospecify hello absolute URI for a certificate by using hello certificateUrl property?</span></span> 

<span data-ttu-id="edaf8-197">A Rendszerfelügyeleti (webszolgáltatások WinRM) tanúsítványhivatkozást hello hello operációsrendszer-profilt a titkos kulcsok tulajdonság jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="edaf8-197">A Windows Remote Management (WinRM) certificate reference must be present in hello Secrets property of hello OS profile.</span></span> 

<span data-ttu-id="edaf8-198">hello hello forrás tároló jelző célja tooenforce hozzáférés-vezérlési lista (ACL) házirendeket, amelyek a felhasználó Azure Cloud Service modell szerepel.</span><span class="sxs-lookup"><span data-stu-id="edaf8-198">hello purpose of indicating hello source vault is tooenforce access control list (ACL) policies that exist in a user's Azure Cloud Service model.</span></span> <span data-ttu-id="edaf8-199">Ha hello forrás tárolóban nincs megadva, a felhasználók, akik nem rendelkeznek engedéllyel toodeploy vagy hozzáférési kulcsait tooa kulcstároló lenne képes toothrough számítási erőforrás szolgáltató (CRP).</span><span class="sxs-lookup"><span data-stu-id="edaf8-199">If hello source vault isn't specified, users who do not have permissions toodeploy or access secrets tooa key vault would be able toothrough a Compute Resource Provider (CRP).</span></span> <span data-ttu-id="edaf8-200">Hozzáférés-vezérlési listák létezik még a erőforrásokat, amelyek nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-200">ACLs exist even for resources that do not exist.</span></span>

<span data-ttu-id="edaf8-201">Egy nem megfelelő tároló Forrásazonosítóval azonban egy érvényes kulcstároló URL-címet ad meg, ha rendszer hibát jelez, akkor kérdezze le a hello művelet során.</span><span class="sxs-lookup"><span data-stu-id="edaf8-201">If you provide an incorrect source vault ID but a valid key vault URL, an error is reported when you poll hello operation.</span></span>
 
### <a name="if-i-add-secrets-tooan-existing-virtual-machine-scale-set-are-hello-secrets-injected-into-existing-vms-or-only-into-new-ones"></a><span data-ttu-id="edaf8-202">Ha a titkos kulcsok tooan meglévő virtuálisgép-méretezési csoport hozzáadásához vannak hello titkok beszúrta meglévő virtuális gépekhez, vagy csak újakat?</span><span class="sxs-lookup"><span data-stu-id="edaf8-202">If I add secrets tooan existing virtual machine scale set, are hello secrets injected into existing VMs, or only into new ones?</span></span> 

<span data-ttu-id="edaf8-203">Tanúsítványok tooall ad hozzá a virtuális gépeken, akár már létező azokat.</span><span class="sxs-lookup"><span data-stu-id="edaf8-203">Certificates are added tooall your VMs, even preexisting ones.</span></span> <span data-ttu-id="edaf8-204">Ha a virtuálisgép-méretezési készlet upgradePolicy tulajdonság értéke túl**manuális**, hello tanúsítvány kerül toohello VM hello VM kézi frissítés végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="edaf8-204">If your virtual machine scale set upgradePolicy property is set too**manual**, hello certificate is added toohello VM when you perform a manual update on hello VM.</span></span>
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a><span data-ttu-id="edaf8-205">Ha put tanúsítványok Linux virtuális gépekhez?</span><span class="sxs-lookup"><span data-stu-id="edaf8-205">Where do I put certificates for Linux VMs?</span></span>

<span data-ttu-id="edaf8-206">Hogyan toodeploy tanúsítványok Linux virtuális gép esetében: toolearn [telepíteni az ügyfél által felügyelt kulcstároló tanúsítványok tooVMs](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-206">toolearn how toodeploy certificates for Linux VMs, see [Deploy certificates tooVMs from a customer-managed key vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span></span>
  
### <a name="how-do-i-add-a-new-vault-certificate-tooa-new-certificate-object"></a><span data-ttu-id="edaf8-207">Hogyan hozzá egy új tároló tooa új tanúsítvány tanúsítványobjektum?</span><span class="sxs-lookup"><span data-stu-id="edaf8-207">How do I add a new vault certificate tooa new certificate object?</span></span>

<span data-ttu-id="edaf8-208">tooadd tároló tanúsítvány tooan meglévő titkos kulcs, tekintse meg a következő PowerShell-példa hello.</span><span class="sxs-lookup"><span data-stu-id="edaf8-208">tooadd a vault certificate tooan existing secret, see hello following PowerShell example.</span></span> <span data-ttu-id="edaf8-209">Csak egy titkos objektum használja.</span><span class="sxs-lookup"><span data-stu-id="edaf8-209">Use only one secret object.</span></span>
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-toocertificates-if-you-reimage-a-vm"></a><span data-ttu-id="edaf8-210">Toocertificates mi történik, ha a virtuális gépek újból lemezképet létrehozni?</span><span class="sxs-lookup"><span data-stu-id="edaf8-210">What happens toocertificates if you reimage a VM?</span></span>

<span data-ttu-id="edaf8-211">Ha Ön újból lemezképet létrehozni egy virtuális Gépet, a tanúsítványok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-211">If you reimage a VM, certificates are deleted.</span></span> <span data-ttu-id="edaf8-212">Különösen törlések hello teljes operációsrendszer-lemezt.</span><span class="sxs-lookup"><span data-stu-id="edaf8-212">Reimaging deletes hello entire OS disk.</span></span> 
 
### <a name="what-happens-if-you-delete-a-certificate-from-hello-key-vault"></a><span data-ttu-id="edaf8-213">Mi történik, ha a tanúsítvány törlése hello kulcstároló?</span><span class="sxs-lookup"><span data-stu-id="edaf8-213">What happens if you delete a certificate from hello key vault?</span></span>

<span data-ttu-id="edaf8-214">Ha a hello titkos hello kulcstároló törlődik, és ezt követően futtatni `stop deallocate` a virtuális géphez, majd indítsa el újra azokat hiba fog történni.</span><span class="sxs-lookup"><span data-stu-id="edaf8-214">If hello secret is deleted from hello key vault, and then you run `stop deallocate` for all your VMs and then start them again, you will encounter a failure.</span></span> <span data-ttu-id="edaf8-215">hello hiba fordul elő, mert hello KSZT kell tooretrieve hello titkokat a hello kulcstároló, de ez nem lehetséges.</span><span class="sxs-lookup"><span data-stu-id="edaf8-215">hello failure occurs because hello CRP needs tooretrieve hello secrets from hello key vault, but it cannot.</span></span> <span data-ttu-id="edaf8-216">Ebben a forgatókönyvben a virtuális gép méretezési modellel hello hello tanúsítványok törölheti.</span><span class="sxs-lookup"><span data-stu-id="edaf8-216">In this scenario, you can delete hello certificates from hello virtual machine scale set model.</span></span> 

<span data-ttu-id="edaf8-217">hello KSZT összetevő nem maradnak ügyfél titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="edaf8-217">hello CRP component does not persist customer secrets.</span></span> <span data-ttu-id="edaf8-218">Ha `stop deallocate` hello virtuálisgép-méretezési csoportban lévő összes virtuális gép esetében hello gyorsítótár törlődik.</span><span class="sxs-lookup"><span data-stu-id="edaf8-218">If you run `stop deallocate` for all VMs in hello virtual machine scale set, hello cache is deleted.</span></span> <span data-ttu-id="edaf8-219">Ebben a forgatókönyvben a titkos kulcsok lekért hello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="edaf8-219">In this scenario, secrets are retrieved from hello key vault.</span></span>

<span data-ttu-id="edaf8-220">Ez a probléma nem tapasztal, amikor kiterjesztése, mert nincs a gyorsítótárazott másolatának hello titkos kulcsot az Azure Service Fabric (hello bérlő egyetlen-háló modell).</span><span class="sxs-lookup"><span data-stu-id="edaf8-220">You don't encounter this problem when scaling out because there is a cached copy of hello secret in Azure Service Fabric (in hello single-fabric tenant model).</span></span>
 
### <a name="why-do-i-have-toospecify-hello-exact-location-for-hello-certificate-url-httpsname-of-hello-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a><span data-ttu-id="edaf8-221">Miért kell toospecify hello pontos helyét hello tanúsítvány URL-címe (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), a [Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span><span class="sxs-lookup"><span data-stu-id="edaf8-221">Why do I have toospecify hello exact location for hello certificate URL (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), as indicated in [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span></span>
 
<span data-ttu-id="edaf8-222">hello Azure Key Vault dokumentáció szerint az adott hello beolvasni a titkos kulcs REST API hello hello titkos legújabb verzióját kell visszaadnia, ha nincs megadva hello verziója.</span><span class="sxs-lookup"><span data-stu-id="edaf8-222">hello Azure Key Vault documentation states that hello Get Secret REST API should return hello latest version of hello secret if hello version is not specified.</span></span>
 
<span data-ttu-id="edaf8-223">Módszer</span><span class="sxs-lookup"><span data-stu-id="edaf8-223">Method</span></span> | <span data-ttu-id="edaf8-224">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="edaf8-224">URL</span></span>
--- | ---
<span data-ttu-id="edaf8-225">GET</span><span class="sxs-lookup"><span data-stu-id="edaf8-225">GET</span></span> | <span data-ttu-id="edaf8-226">https://mykeyvault.vault.Azure.NET/secrets/ {titkos kulcs neve} / {titkos-version}? api-version = {api-version}</span><span class="sxs-lookup"><span data-stu-id="edaf8-226">https://mykeyvault.vault.azure.net/secrets/{secret-name}/{secret-version}?api-version={api-version}</span></span>

<span data-ttu-id="edaf8-227">Cserélje le {*titkos-név*} hello névvel, és cserélje le {*titkos verziójú*} hello titkos hello verziójával tooretrieve szeretné.</span><span class="sxs-lookup"><span data-stu-id="edaf8-227">Replace {*secret-name*} with hello name, and replace {*secret-version*} with hello version of hello secret you want tooretrieve.</span></span> <span data-ttu-id="edaf8-228">Előfordulhat, hogy zárva hello titkos kulcs verzióját.</span><span class="sxs-lookup"><span data-stu-id="edaf8-228">hello secret version might be excluded.</span></span> <span data-ttu-id="edaf8-229">Ebben az esetben beolvasott hello jelenlegi verziójával.</span><span class="sxs-lookup"><span data-stu-id="edaf8-229">In that case, hello current version is retrieved.</span></span>
  
### <a name="why-do-i-have-toospecify-hello-certificate-version-when-i-use-key-vault"></a><span data-ttu-id="edaf8-230">Miért van meg toospecify hello tanúsítvány verziója, Key Vault használatakor?</span><span class="sxs-lookup"><span data-stu-id="edaf8-230">Why do I have toospecify hello certificate version when I use Key Vault?</span></span>

<span data-ttu-id="edaf8-231">hello hello Key Vault követelmény toospecify hello tanúsítvány verziója célja toomake, akkor törölje a jelet toohello felhasználó milyen tanúsítvány van telepítve. a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="edaf8-231">hello purpose of hello Key Vault requirement toospecify hello certificate version is toomake it clear toohello user what certificate is deployed on their VMs.</span></span>

<span data-ttu-id="edaf8-232">Ha egy virtuális gép létrehozása, és ezután frissítse a titkos kód hello a key vaultban, hello új tanúsítvány nincs letöltött tooyour virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-232">If you create a VM and then update your secret in hello key vault, hello new certificate is not downloaded tooyour VMs.</span></span> <span data-ttu-id="edaf8-233">De a virtuális gépek és az új virtuális gépek hello új titkos kulcs lekérése tooreference jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="edaf8-233">But your VMs appear tooreference it, and new VMs get hello new secret.</span></span> <span data-ttu-id="edaf8-234">tooavoid, a titkos kulcs verzióját szükséges tooreference áll.</span><span class="sxs-lookup"><span data-stu-id="edaf8-234">tooavoid this, you are required tooreference a secret version.</span></span>

### <a name="my-team-works-with-several-certificates-that-are-distributed-toous-as-cer-public-keys-what-is-hello-recommended-approach-for-deploying-these-certificates-tooa-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-235">A csoport több olyan tanúsítvány, mint .cer nyilvános kulcsok toous elosztott működik.</span><span class="sxs-lookup"><span data-stu-id="edaf8-235">My team works with several certificates that are distributed toous as .cer public keys.</span></span> <span data-ttu-id="edaf8-236">Mi az ajánlott módszer a tanúsítványok tooa virtuálisgép-méretezési csoport telepítéséhez hello?</span><span class="sxs-lookup"><span data-stu-id="edaf8-236">What is hello recommended approach for deploying these certificates tooa virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-237">toodeploy .cer nyilvános kulcsok tooa virtuális gép méretezési, létrehozhat egy .pfx-fájlt, amely csak a .cer fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="edaf8-237">toodeploy .cer public keys tooa virtual machine scale set, you can generate a .pfx file that contains only .cer files.</span></span> <span data-ttu-id="edaf8-238">toodo ehhez, `X509ContentType = Pfx`.</span><span class="sxs-lookup"><span data-stu-id="edaf8-238">toodo this, use `X509ContentType = Pfx`.</span></span> <span data-ttu-id="edaf8-239">Például C# vagy PowerShell x509Certificate2 objektumként hello .cer fájl betöltése, és ezután hívja meg hello metódust.</span><span class="sxs-lookup"><span data-stu-id="edaf8-239">For example, load hello .cer file as an x509Certificate2 object in C# or PowerShell, and then call hello method.</span></span> 

<span data-ttu-id="edaf8-240">További információkért lásd: [X509Certificate.Export metódus (X509ContentType, karakterlánc)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span><span class="sxs-lookup"><span data-stu-id="edaf8-240">For more information, see [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span></span>

### <a name="i-do-not-see-an-option-for-users-toopass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a><span data-ttu-id="edaf8-241">A beállítás a tanúsítványok felhasználók toopass base64 karakterláncként nem látható.</span><span class="sxs-lookup"><span data-stu-id="edaf8-241">I do not see an option for users toopass in certificates as base64 strings.</span></span> <span data-ttu-id="edaf8-242">A legtöbb más erőforrás-szolgáltatók kell ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="edaf8-242">Most other resource providers have this option.</span></span>

<span data-ttu-id="edaf8-243">egy tanúsítványt, mint a Base64 kódolású karakterlánc benyújtása tooemulate kibonthatja hello legújabb verzióval ellátott URL-címet a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="edaf8-243">tooemulate passing in a certificate as a base64 string, you can extract hello latest versioned URL in a Resource Manager template.</span></span> <span data-ttu-id="edaf8-244">A következő JSON-tulajdonság az erőforrás-kezelő sablonban hello a következők:</span><span class="sxs-lookup"><span data-stu-id="edaf8-244">Include hello following JSON property in your Resource Manager template:</span></span>

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-toowrap-certificates-in-json-objects-in-key-vaults"></a><span data-ttu-id="edaf8-245">Van toowrap tanúsítványok a JSON-objektumok a kulcstárolók?</span><span class="sxs-lookup"><span data-stu-id="edaf8-245">Do I have toowrap certificates in JSON objects in key vaults?</span></span>

<span data-ttu-id="edaf8-246">A virtuálisgép-méretezési csoportok és a virtuális gépek tanúsítványokat kell csomagolni, a JSON-objektumok.</span><span class="sxs-lookup"><span data-stu-id="edaf8-246">In virtual machine scale sets and VMs, certificates must be wrapped in JSON objects.</span></span> 

<span data-ttu-id="edaf8-247">Alkalmazás/x-pkcs12 hello tartalomtípus is támogatja.</span><span class="sxs-lookup"><span data-stu-id="edaf8-247">We also support hello content type application/x-pkcs12.</span></span> <span data-ttu-id="edaf8-248">Alkalmazás/x-pkcs12 használatával, lásd: [PFX-tanúsítványokat az Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-248">For instructions on using application/x-pkcs12, see [PFX certificates in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span></span>
 
<span data-ttu-id="edaf8-249">Jelenleg nem támogatjuk .cer kiterjesztésű fájlokat.</span><span class="sxs-lookup"><span data-stu-id="edaf8-249">We currently do not support .cer files.</span></span> <span data-ttu-id="edaf8-250">toouse .cer fájlt, a .pfx-tárolók exportálhatja őket.</span><span class="sxs-lookup"><span data-stu-id="edaf8-250">toouse .cer files, export them into .pfx containers.</span></span>



## <a name="compliance"></a><span data-ttu-id="edaf8-251">Megfelelőség</span><span class="sxs-lookup"><span data-stu-id="edaf8-251">Compliance</span></span>

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a><span data-ttu-id="edaf8-252">Azok a virtuálisgép-skálázási készletekben PCI-kompatibilis?</span><span class="sxs-lookup"><span data-stu-id="edaf8-252">Are virtual machine scale sets PCI-compliant?</span></span>

<span data-ttu-id="edaf8-253">Virtuálisgép-méretezési csoportok vékony API felső szintű rétege hello KSZT.</span><span class="sxs-lookup"><span data-stu-id="edaf8-253">Virtual machine scale sets are a thin API layer on top of hello CRP.</span></span> <span data-ttu-id="edaf8-254">Összetevőket egyaránt hello számítógépes platform hello Azure szolgáltatás fájában a részét képezik.</span><span class="sxs-lookup"><span data-stu-id="edaf8-254">Both components are part of hello compute platform in hello Azure service tree.</span></span>

<span data-ttu-id="edaf8-255">A megfelelőség szempontjából a virtuálisgép-méretezési csoportok hello Azure számítási platform alapvető részét képezik.</span><span class="sxs-lookup"><span data-stu-id="edaf8-255">From a compliance perspective, virtual machine scale sets are a fundamental part of hello Azure compute platform.</span></span> <span data-ttu-id="edaf8-256">Egy csoport, eszközök, folyamatok, telepítési módszer, biztonsági vezérlők, just-in-time (JIT) fordítási, figyelés, riasztás, és így tovább, azok megosztása hello KSZT magát.</span><span class="sxs-lookup"><span data-stu-id="edaf8-256">They share a team, tools, processes, deployment methodology, security controls, just-in-time (JIT) compilation, monitoring, alerting, and so on, with hello CRP itself.</span></span> <span data-ttu-id="edaf8-257">Virtuálisgép-méretezési csoportok a Payment Card Industry (PCI)-kompatibilis, mert a KSZT hello hello aktuális PCI adatok biztonsági szabvány (DSS) igazolási része.</span><span class="sxs-lookup"><span data-stu-id="edaf8-257">Virtual machine scale sets are Payment Card Industry (PCI)-compliant because hello CRP is part of hello current PCI Data Security Standard (DSS) attestation.</span></span>

<span data-ttu-id="edaf8-258">További információkért lásd: [Microsoft Trust Center hello](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span><span class="sxs-lookup"><span data-stu-id="edaf8-258">For more information, see [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span></span>






## <a name="extensions"></a><span data-ttu-id="edaf8-259">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="edaf8-259">Extensions</span></span>

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a><span data-ttu-id="edaf8-260">A virtuálisgép-méretezési készlet bővítmény törlése</span><span class="sxs-lookup"><span data-stu-id="edaf8-260">How do I delete a virtual machine scale set extension?</span></span>

<span data-ttu-id="edaf8-261">egy virtuálisgép-méretezési toodelete virtuáliskapcsoló-kiterjesztés, a következő PowerShell-példa használata hello beállítása:</span><span class="sxs-lookup"><span data-stu-id="edaf8-261">toodelete a virtual machine scale set extension, use hello following PowerShell example:</span></span>

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
<span data-ttu-id="edaf8-262">Hello bővítménynév érték található `$vmss`.</span><span class="sxs-lookup"><span data-stu-id="edaf8-262">You can find hello extensionName value in `$vmss`.</span></span>
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a><span data-ttu-id="edaf8-263">Egy virtuális gép méretezési sablon példa, amely az Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="edaf8-263">Is there a virtual machine scale set template example that integrates with Operations Management Suite?</span></span>

<span data-ttu-id="edaf8-264">A virtuális gép méretezési sablon példa, amely az Operations Management Suite, lásd: hello második példáját [Azure Service Fabric-fürt üzembe helyezése, és engedélyezze a megfigyelést Naplóelemzési használatával](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span><span class="sxs-lookup"><span data-stu-id="edaf8-264">For a virtual machine scale set template example that integrates with Operations Management Suite, see hello second example in [Deploy an Azure Service Fabric cluster and enable monitoring by using Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span></span>
   
### <a name="extensions-seem-toorun-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-toofail-what-can-i-do-toofix-this"></a><span data-ttu-id="edaf8-265">Bővítmények toorun párhuzamos virtuálisgép-méretezési csoportok tűnnek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-265">Extensions seem toorun in parallel on virtual machine scale sets.</span></span> <span data-ttu-id="edaf8-266">Ennek hatására a saját egyéni parancsfájl-kiterjesztés toofail.</span><span class="sxs-lookup"><span data-stu-id="edaf8-266">This causes my custom script extension toofail.</span></span> <span data-ttu-id="edaf8-267">Mi a teendő toofix Ez?</span><span class="sxs-lookup"><span data-stu-id="edaf8-267">What can I do toofix this?</span></span>

<span data-ttu-id="edaf8-268">Virtuálisgép-méretezési csoportok, a bővítmény alkalmazás-előkészítés kapcsolatos toolearn lásd [bővítmény alkalmazás-előkészítés az Azure virtuálisgép-méretezési csoportok](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-268">toolearn about extension sequencing in virtual machine scale sets, see [Extension sequencing in Azure virtual machine scale sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span></span>
 
 
### <a name="how-do-i-reset-hello-password-for-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-269">Hogyan tegye alaphelyzetbe állít egy hello jelszó virtuális gépek a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="edaf8-269">How do I reset hello password for VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-270">a virtuálisgép-méretezési a virtuális gépek tooreset hello jelszó megadásához használja a virtuális gép eléréséhez kapcsolódó kiegészítő mezőket.</span><span class="sxs-lookup"><span data-stu-id="edaf8-270">tooreset hello password for VMs in your virtual machine scale set, use VM access extensions.</span></span> 

<span data-ttu-id="edaf8-271">A következő PowerShell-példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="edaf8-271">Use hello following PowerShell example:</span></span>

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-tooall-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-272">Hogyan hozzá a virtuálisgép-méretezési csoportban lévő egy bővítmény tooall virtuális gépek?</span><span class="sxs-lookup"><span data-stu-id="edaf8-272">How do I add an extension tooall VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-273">Ha a házirend értéke túl**automatikus**, hello sablon ismételt üzembe helyezéssel hello új bővítmény tulajdonságokkal rendelkező összes virtuális gépet frissíti.</span><span class="sxs-lookup"><span data-stu-id="edaf8-273">If update policy is set too**automatic**, redeploying hello template with hello new extension properties updates all VMs.</span></span>

<span data-ttu-id="edaf8-274">Ha a házirend értéke túl**manuális**, előbb módosítsa a hello kiterjesztését, és manuálisan frissítse a virtuális gépek összes példányán.</span><span class="sxs-lookup"><span data-stu-id="edaf8-274">If update policy is set too**manual**, first update hello extension, and then manually update all instances in your VMs.</span></span>

  
### <a name="if-hello-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-hello-vms-not-match-hello-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-hello-scripts-that-are-currently-configured-on-hello-virtual-machine-scale-set-executed-or-are-hello-scripts-that-were-configured-when-hello-vm-was-first-created-used"></a><span data-ttu-id="edaf8-275">Ha egy meglévő virtuálisgép-méretezési csoport társított hello bővítmények frissítése, a meglévő virtuális gépek hatással?</span><span class="sxs-lookup"><span data-stu-id="edaf8-275">If hello extensions associated with an existing virtual machine scale set are updated, are existing VMs affected?</span></span> <span data-ttu-id="edaf8-276">(Ez azt jelenti, hogy a rendszer hello virtuális gépek *nem* egyezés hello virtuális gép méretezési modellel?) Vagy azok figyelmen kívül hagy?</span><span class="sxs-lookup"><span data-stu-id="edaf8-276">(That is, will hello VMs *not* match hello virtual machine scale set model?) Or are they ignored?</span></span> <span data-ttu-id="edaf8-277">Ha egy meglévő számítógép szolgáltatás beforrott vagy lemezképet, hello olyan parancsfájlok, amelyek jelenleg be van állítva a hello virtuálisgép-méretezési csoport hajtotta végre, vagy hello parancsfájlok konfigurált virtuális gép létrehozásának hello használata esetén?</span><span class="sxs-lookup"><span data-stu-id="edaf8-277">When an existing machine is service-healed or reimaged, are hello scripts that are currently configured on hello virtual machine scale set executed, or are hello scripts that were configured when hello VM was first created used?</span></span>

<span data-ttu-id="edaf8-278">Ha a virtuálisgép-méretezési hello hello-bővítmény definíciójának modell frissül, és hello upgradePolicy tulajdonsága túl**automatikus**, hello virtuális gépek frissíti.</span><span class="sxs-lookup"><span data-stu-id="edaf8-278">If hello extension definition in hello virtual machine scale set model is updated and hello upgradePolicy property is set too**automatic**, it updates hello VMs.</span></span> <span data-ttu-id="edaf8-279">Ha hello upgradePolicy tulajdonsága túl**manuális**, bővítmények megjelölt hello modell nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="edaf8-279">If hello upgradePolicy property is set too**manual**, extensions are flagged as not matching hello model.</span></span> 

<span data-ttu-id="edaf8-280">Ha egy meglévő virtuális gép szolgáltatás beforrott, újraindítás jelenik meg, és vannak hello bővítmények nem futtatják újra.</span><span class="sxs-lookup"><span data-stu-id="edaf8-280">If an existing VM is service-healed, it appears as a reboot, and hello extensions are not rerun.</span></span> <span data-ttu-id="edaf8-281">Ha rendszerképének, például az operációs rendszer hello meghajtó lecserélését hello forráslemezkép.</span><span class="sxs-lookup"><span data-stu-id="edaf8-281">If it is reimaged, it's like replacing hello OS drive with hello source image.</span></span> <span data-ttu-id="edaf8-282">Bármely futtató és specializációt hello legújabb modellből, például a bővítmények, futtathatók.</span><span class="sxs-lookup"><span data-stu-id="edaf8-282">Any specialization from hello latest model, such as extensions, are run.</span></span>
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-tooan-azure-ad-domain"></a><span data-ttu-id="edaf8-283">Hogyan virtuális gép méretezési készlet tooan az Azure AD tartományhoz csatlakozni?</span><span class="sxs-lookup"><span data-stu-id="edaf8-283">How do I join a virtual machine scale set tooan Azure AD domain?</span></span>

<span data-ttu-id="edaf8-284">toojoin egy virtuális gép méretezési készlet tooan Azure Active Directory (Azure AD) tartományhoz, definiálhat egy bővítmény.</span><span class="sxs-lookup"><span data-stu-id="edaf8-284">toojoin a virtual machine scale set tooan Azure Active Directory (Azure AD) domain, you can define an extension.</span></span> 

<span data-ttu-id="edaf8-285">egy bővítmény toodefine hello JsonADDomainExtension tulajdonság használja:</span><span class="sxs-lookup"><span data-stu-id="edaf8-285">toodefine an extension, use hello JsonADDomainExtension property:</span></span>

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-tooinstall-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a><span data-ttu-id="edaf8-286">A virtuálisgép-méretezési készlet bővítmény próbál tooinstall, amelyet újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="edaf8-286">My virtual machine scale set extension is trying tooinstall something that requires a reboot.</span></span> <span data-ttu-id="edaf8-287">Például: "commandToExecute": "powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature-Name FS-Resource-Manager – IncludeManagementTools"</span><span class="sxs-lookup"><span data-stu-id="edaf8-287">For example, "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools"</span></span>

<span data-ttu-id="edaf8-288">Ha a virtuálisgép-méretezési készlet bővítmény próbál tooinstall, amelyet újra kell indítani, használhatja a hello Azure Automation célállapot-konfiguráció (Automation DSC) bővítmény.</span><span class="sxs-lookup"><span data-stu-id="edaf8-288">If your virtual machine scale set extension is trying tooinstall something that requires a reboot, you can use hello Azure Automation Desired State Configuration (Automation DSC) extension.</span></span> <span data-ttu-id="edaf8-289">Windows Server 2012 R2 hello operációs rendszer esetén a Azure lekéri a hello a Windows Management Framework (WMF) 5.0 telepítés újraindítások idejére, és a Folytatás hello konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="edaf8-289">If hello operating system is Windows Server 2012 R2, Azure pulls in hello Windows Management Framework (WMF) 5.0 setup, reboots, and then continues with hello configuration.</span></span> 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-290">Hogyan bekapcsolni kártevőirtó a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="edaf8-290">How do I turn on antimalware in my virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-291">a antimalware-t a virtuálisgép-méretezési tooturn megadásához használja a következő PowerShell-példa hello:</span><span class="sxs-lookup"><span data-stu-id="edaf8-291">tooturn on antimalware on your virtual machine scale set, use hello following PowerShell example:</span></span>

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve hello most recent version number of hello extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-tooexecute-a-custom-script-thats-hosted-in-a-private-storage-account-hello-script-runs-successfully-when-hello-storage-is-public-but-when-i-try-toouse-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a><span data-ttu-id="edaf8-292">Egyéni parancsfájl, amely egy privát tárfiókban lévő tooexecute kell.</span><span class="sxs-lookup"><span data-stu-id="edaf8-292">I need tooexecute a custom script that's hosted in a private storage account.</span></span> <span data-ttu-id="edaf8-293">hello parancsprogram sikeresen lefut, nyilvános hello tárolás esetén, de közben toouse egy közös hozzáférésű Jogosultságkód (SAS), az nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="edaf8-293">hello script runs successfully when hello storage is public, but when I try toouse a Shared Access Signature (SAS), it fails.</span></span> <span data-ttu-id="edaf8-294">Ez az üzenet jelenik meg: "Kötelező paraméter hiányzik az érvényes a közös hozzáférésű Jogosultságkód".</span><span class="sxs-lookup"><span data-stu-id="edaf8-294">This message is displayed: “Missing mandatory parameters for valid Shared Access Signature”.</span></span> <span data-ttu-id="edaf8-295">Hivatkozás + SAS jól működik a helyi böngészőből.</span><span class="sxs-lookup"><span data-stu-id="edaf8-295">Link+SAS works fine from my local browser.</span></span>

<span data-ttu-id="edaf8-296">személyes tárfiókokban, üzemeltetett egyéni parancsfájl tooexecute hello tárfiók hívóbetűjét, a név védett beállítások megadása.</span><span class="sxs-lookup"><span data-stu-id="edaf8-296">tooexecute a custom script that's hosted in a private storage account, set up protected settings with hello storage account key and name.</span></span> <span data-ttu-id="edaf8-297">További információkért lásd: [egyéni parancsfájl kiterjesztése a Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span><span class="sxs-lookup"><span data-stu-id="edaf8-297">For more information, see [Custom Script Extension for Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span></span>







## <a name="networking"></a><span data-ttu-id="edaf8-298">Hálózat</span><span class="sxs-lookup"><span data-stu-id="edaf8-298">Networking</span></span>
 
### <a name="is-it-possible-tooassign-a-network-security-group-nsg-tooa-scale-set-so-that-it-will-apply-tooall-hello-vm-nics-in-hello-set"></a><span data-ttu-id="edaf8-299">Azt a hálózati biztonsági csoport (NSG) tooa méretezési, lehetséges tooassign azért, hogy tooall hello virtuális gép hálózati adapterek hello készlet fog alkalmazni?</span><span class="sxs-lookup"><span data-stu-id="edaf8-299">Is it possible tooassign a Network Security Group (NSG) tooa scale set, so that it will apply tooall hello VM NICs in hello set?</span></span>

<span data-ttu-id="edaf8-300">Igen.</span><span class="sxs-lookup"><span data-stu-id="edaf8-300">Yes.</span></span> <span data-ttu-id="edaf8-301">Hálózati biztonsági csoport közvetlen tooa méretezési hello networkinterfaceconfigurations tulajdonságok szakaszának hello hálózati profil Vezérlőpultjának lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="edaf8-301">A Network Security Group can be applied directly tooa scale set by referencing it in hello networkInterfaceConfigurations section of hello network profile.</span></span> <span data-ttu-id="edaf8-302">Példa:</span><span class="sxs-lookup"><span data-stu-id="edaf8-302">Example:</span></span>

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-hello-same-subscription-and-same-region"></a><span data-ttu-id="edaf8-303">Hogyan egy virtuális IP-címcsere a virtuálisgép-skálázási készletekben hello azonos előfizetésben és ugyanabban a régióban?</span><span class="sxs-lookup"><span data-stu-id="edaf8-303">How do I do a VIP swap for virtual machine scale sets in hello same subscription and same region?</span></span>

<span data-ttu-id="edaf8-304">Ha két virtuális virtulálisgép-skálázási készletekben az Azure Load Balancer első-végpontok, és vannak hello azonos előfizetésével és régiójával, képes mindegyiknél hello nyilvános IP-címek felszabadítása, és rendelje hozzá a többi toohello.</span><span class="sxs-lookup"><span data-stu-id="edaf8-304">If you have two virtual machine scale sets with Azure Load Balancer front-ends, and they are in hello same subscription and region, you could deallocate hello public IP addresses from each one, and assign toohello other.</span></span> <span data-ttu-id="edaf8-305">Lásd: [virtuális IP-Címcsere: kék-zöld telepítése Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) például.</span><span class="sxs-lookup"><span data-stu-id="edaf8-305">See [VIP Swap: Blue-green deployment in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) for example.</span></span> <span data-ttu-id="edaf8-306">Ez utalnak késleltetés, bár, hello erőforrások felszabadítása/lefoglalt hello szintjén.</span><span class="sxs-lookup"><span data-stu-id="edaf8-306">This does imply a delay though as hello resources are deallocated/allocated at hello network level.</span></span> <span data-ttu-id="edaf8-307">A gyorsabb elem toouse Azure Application Gateway két háttérkészletek menüpontot, és útválasztási szabályainak.</span><span class="sxs-lookup"><span data-stu-id="edaf8-307">A faster option is toouse Azure Application Gateway with two backend pools, and a routing rule.</span></span> <span data-ttu-id="edaf8-308">Alternatív megoldásként az alkalmazásba sikerült működteti [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) amelyek támogatást nyújt az gyors váltás átmeneti és üzemi tárhelyek között.</span><span class="sxs-lookup"><span data-stu-id="edaf8-308">Alternatively, you could host your application with [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) which provides support for fast switching between staging and production slots.</span></span>
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-toouse-for-static-private-ip-address-allocation"></a><span data-ttu-id="edaf8-309">Hogyan adja meg a privát IP-címek toouse statikus magánhálózati IP-címek hozzárendelését a számos?</span><span class="sxs-lookup"><span data-stu-id="edaf8-309">How do I specify a range of private IP addresses toouse for static private IP address allocation?</span></span>

<span data-ttu-id="edaf8-310">Megadott alhálózatból származó IP-címek van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="edaf8-310">IP addresses are selected from a subnet that you specify.</span></span> 

<span data-ttu-id="edaf8-311">hello elosztási módszert a virtuális gép méretezési készlet IP-címek mindig "dinamikus" azonban, hogy nem jelenti azt, hogy az IP-címeket módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="edaf8-311">hello allocation method of virtual machine scale set IP addresses is always “dynamic,” but that doesn't mean that these IP addresses can change.</span></span> <span data-ttu-id="edaf8-312">Ebben az esetben "dinamikus" csak azt jelenti, hogy nincs megadva hello IP-címet a PUT kérelmek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-312">In this case, "dynamic" only means that you do not specify hello IP address in a PUT request.</span></span> <span data-ttu-id="edaf8-313">Adjon meg statikus hello hello alhálózat-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="edaf8-313">Specify hello static set by using hello subnet.</span></span> 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-tooan-existing-azure-virtual-network"></a><span data-ttu-id="edaf8-314">Hogyan telepíthető egy virtuális gép méretezési készlet tooan meglévő Azure virtuális hálózat?</span><span class="sxs-lookup"><span data-stu-id="edaf8-314">How do I deploy a virtual machine scale set tooan existing Azure virtual network?</span></span> 

<span data-ttu-id="edaf8-315">a virtuálisgép-méretezési toodeploy tooan meglévő Azure virtuális hálózat, olvassa el [központi telepítése egy virtuális gép méretezési készlet tooan meglévő virtuális hálózat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="edaf8-315">toodeploy a virtual machine scale set tooan existing Azure virtual network, see [Deploy a virtual machine scale set tooan existing virtual network](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span></span> 

### <a name="how-do-i-add-hello-ip-address-of-hello-first-vm-in-a-virtual-machine-scale-set-toohello-output-of-a-template"></a><span data-ttu-id="edaf8-316">Hogyan hello IP-cím hozzáadása a hello a virtuálisgép-méretezési első Virtuálisgép-sablon toohello kimeneti csoportban?</span><span class="sxs-lookup"><span data-stu-id="edaf8-316">How do I add hello IP address of hello first VM in a virtual machine scale set toohello output of a template?</span></span>

<span data-ttu-id="edaf8-317">tooadd hello IP-címe hello első virtuális gép egy virtuális gép méretezési készlet toohello kimenet egy sablon, lásd: [ARM: első VMSS privát IP-címek](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span><span class="sxs-lookup"><span data-stu-id="edaf8-317">tooadd hello IP address of hello first VM in a virtual machine scale set toohello output of a template, see [ARM: Get VMSS's private IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span></span>

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a><span data-ttu-id="edaf8-318">Méretezési készlet használható a hálózat elérését gyorsítja fel?</span><span class="sxs-lookup"><span data-stu-id="edaf8-318">Can I use scale sets with Accelerated Networking?</span></span>

<span data-ttu-id="edaf8-319">Igen.</span><span class="sxs-lookup"><span data-stu-id="edaf8-319">Yes.</span></span> <span data-ttu-id="edaf8-320">hálózat, az elérését gyorsítja fel toouse beállítása enableAcceleratedNetworking tootrue a méretezési készlet által networkinterfaceconfigurations tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="edaf8-320">toouse accelerated networking, set enableAcceleratedNetworking tootrue in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="edaf8-321">Például</span><span class="sxs-lookup"><span data-stu-id="edaf8-321">E.g.</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-hello-dns-servers-used-by-a-scale-set"></a><span data-ttu-id="edaf8-322">Hogyan konfigurálható a méretezési készlet használt hello DNS-kiszolgálók?</span><span class="sxs-lookup"><span data-stu-id="edaf8-322">How can I configure hello DNS servers used by a scale set?</span></span>

<span data-ttu-id="edaf8-323">toocreate egy Virtuálisgép-méretezési készlet egy egyéni DNS-konfigurációval, vegye fel a dnsSettings JSON csomag toohello méretezési networkinterfaceconfigurations tulajdonságok szakasz.</span><span class="sxs-lookup"><span data-stu-id="edaf8-323">toocreate a VM scale set with a custom DNS configuration, add a dnsSettings JSON packet toohello scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="edaf8-324">Példa:</span><span class="sxs-lookup"><span data-stu-id="edaf8-324">Example:</span></span>
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-tooassign-a-public-ip-address-tooeach-vm"></a><span data-ttu-id="edaf8-325">Hogyan konfigurálható a méretezési készlet tooassign egy nyilvános IP-cím tooeach VM?</span><span class="sxs-lookup"><span data-stu-id="edaf8-325">How can I configure a scale set tooassign a public IP address tooeach VM?</span></span>

<span data-ttu-id="edaf8-326">a Virtuálisgép-méretezési készlet, amely toocreate hozzárendel egy nyilvános IP-cím tooeach VM, legyen hello Microsoft.Compute/virtualMAchineScaleSets erőforrás hello API verziója 2017-03-30, és adja hozzá a _publicipaddressconfiguration_ JSON-csomag a méretezési toohello IP-konfigurációk szakasz.</span><span class="sxs-lookup"><span data-stu-id="edaf8-326">toocreate a VM scale set that assigns a public IP address tooeach VM, make sure hello API version of hello Microsoft.Compute/virtualMAchineScaleSets resource is 2017-03-30, and add a _publicipaddressconfiguration_ JSON packet toohello scale set ipConfigurations section.</span></span> <span data-ttu-id="edaf8-327">Példa:</span><span class="sxs-lookup"><span data-stu-id="edaf8-327">Example:</span></span>

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-toowork-with-multiple-application-gateways"></a><span data-ttu-id="edaf8-328">Beállítható a méretezési készlet toowork több Alkalmazásátjárót?</span><span class="sxs-lookup"><span data-stu-id="edaf8-328">Can I configure a scale set toowork with multiple Application Gateways?</span></span>

<span data-ttu-id="edaf8-329">Igen.</span><span class="sxs-lookup"><span data-stu-id="edaf8-329">Yes.</span></span> <span data-ttu-id="edaf8-330">Hozzáadhat hello erőforrás azonosítók több Application Gateway háttér cím készletek toohello _applicationGatewayBackendAddressPools_ hello listájában _IP-konfigurációk_ a méretezési szakasz hálózati profilt.</span><span class="sxs-lookup"><span data-stu-id="edaf8-330">You can add hello resource id's for multiple Application Gateway backend address pools toohello _applicationGatewayBackendAddressPools_ list in hello _ipConfigurations_ section of your scale set network profile.</span></span>

## <a name="scale"></a><span data-ttu-id="edaf8-331">Méretezés</span><span class="sxs-lookup"><span data-stu-id="edaf8-331">Scale</span></span>

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a><span data-ttu-id="edaf8-332">Milyen esetben volna létrehozni egy virtuálisgép-méretezési beállítása kevesebb mint két virtuális gépek?</span><span class="sxs-lookup"><span data-stu-id="edaf8-332">In what case would I create a virtual machine scale set with fewer than two VMs?</span></span>

<span data-ttu-id="edaf8-333">Egyik oka toocreate egy virtuális gép méretezési csoportban kevesebb mint két virtuális toouse hello rugalmas tulajdonságait egy virtuálisgép-méretezési van beállítva.</span><span class="sxs-lookup"><span data-stu-id="edaf8-333">One reason toocreate a virtual machine scale set with fewer than two VMs would be toouse hello elastic properties of a virtual machine scale set.</span></span> <span data-ttu-id="edaf8-334">Például telepíthet egy virtuálisgép-méretezési csoport a nulla virtuális gépek toodefine az infrastruktúra nélkül költségek futtató virtuális fizet.</span><span class="sxs-lookup"><span data-stu-id="edaf8-334">For example, you could deploy a virtual machine scale set with zero VMs toodefine your infrastructure without paying VM running costs.</span></span> <span data-ttu-id="edaf8-335">Ezután amikor készen áll a toodeploy virtuális gépeket, megnöveli hello "" hello virtuális gép méretezési készlet toohello éles példányok száma.</span><span class="sxs-lookup"><span data-stu-id="edaf8-335">Then, when you are ready toodeploy VMs, increase hello “capacity” of hello virtual machine scale set toohello production instance count.</span></span>

<span data-ttu-id="edaf8-336">Előfordulhat, hogy hozzon létre egy virtuálisgép-méretezési kevesebb mint két virtuális gépek állítsa be a másik OK, ha aggódik, kevesebb, mint a rendelkezésre állási készlet különálló virtuális gépek használatával rendelkezésre állással.</span><span class="sxs-lookup"><span data-stu-id="edaf8-336">Another reason you might create a virtual machine scale set with fewer than two VMs is if you're concerned less with availability than in using an availability set with discrete VMs.</span></span> <span data-ttu-id="edaf8-337">Virtuálisgép-méretezési csoportok olyan módon toowork magánháztartás számítási egységek használatával, amelyek helyettesíthető biztosítják.</span><span class="sxs-lookup"><span data-stu-id="edaf8-337">Virtual machine scale sets give you a way toowork with undifferentiated compute units that are fungible.</span></span> <span data-ttu-id="edaf8-338">Ez egységes a legfontosabb különbséget a virtuálisgép-méretezési csoportok és a rendelkezésre állási készletek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-338">This uniformity is a key differentiator for virtual machine scale sets versus availability sets.</span></span> <span data-ttu-id="edaf8-339">Sok állapot nélküli munkaterheléseket nem követik nyomon egyedi.</span><span class="sxs-lookup"><span data-stu-id="edaf8-339">Many stateless workloads do not track individual units.</span></span> <span data-ttu-id="edaf8-340">Hello terhelés esik, csökkentheti a tooone számítási egység, és majd vertikális felskálázás toomany, amikor hello munkaterhelés növeli.</span><span class="sxs-lookup"><span data-stu-id="edaf8-340">If hello workload drops, you can scale down tooone compute unit, and then scale up toomany when hello workload increases.</span></span>

### <a name="how-do-i-change-hello-number-of-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-341">Hogyan változtathatom hello virtuálisgép-méretezési csoportban lévő virtuális gépek száma?</span><span class="sxs-lookup"><span data-stu-id="edaf8-341">How do I change hello number of VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-342">Tekintse meg a toochange hello virtuális gépek száma a virtuálisgép-méretezési csoportban lévő [hello példányok száma a virtuálisgép-méretezési csoport módosítása](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="edaf8-342">toochange hello number of VMs in a virtual machine scale set, see [Change hello instance count of a virtual machine scale set](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span></span>

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a><span data-ttu-id="edaf8-343">Hogyan határozza meg az egyéni riasztások az egyes küszöbértékek elérésekor?</span><span class="sxs-lookup"><span data-stu-id="edaf8-343">How do I define custom alerts for when certain thresholds are reached?</span></span>

<span data-ttu-id="edaf8-344">Hogyan kezeli a megadott küszöbértékek riasztások néhány beleszólása van.</span><span class="sxs-lookup"><span data-stu-id="edaf8-344">You have some flexibility in how you handle alerts for specified thresholds.</span></span> <span data-ttu-id="edaf8-345">Például a testreszabott webhookok adhat meg.</span><span class="sxs-lookup"><span data-stu-id="edaf8-345">For example, you can define customized webhooks.</span></span> <span data-ttu-id="edaf8-346">a következő webhook példa hello Resource Manager sablon alapján van:</span><span class="sxs-lookup"><span data-stu-id="edaf8-346">hello following webhook example is from a Resource Manager template:</span></span>

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

<span data-ttu-id="edaf8-347">Ebben a példában egy riasztás tooPagerduty.com kerül, a küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="edaf8-347">In this example, an alert goes tooPagerduty.com when a threshold is reached.</span></span>



## <a name="patching-and-operations"></a><span data-ttu-id="edaf8-348">Javítás és műveletek</span><span class="sxs-lookup"><span data-stu-id="edaf8-348">Patching and operations</span></span>

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a><span data-ttu-id="edaf8-349">Hogyan hozható létre egy meglévő erőforráscsoportot beállított terjedő skálán?</span><span class="sxs-lookup"><span data-stu-id="edaf8-349">How do I create a scale set in an existing resource group?</span></span>

<span data-ttu-id="edaf8-350">Méretezési csoportok létrehozása a meglévő erőforrás csoport még nem lehetséges a hello Azure-portálon, de megadhat egy létező erőforráscsoportot, amikor egy méretezési telepítése Azure Resource Manager-sablonok állítható be.</span><span class="sxs-lookup"><span data-stu-id="edaf8-350">Creating scale sets in an existing resource group is not yet possible from hello Azure portal, but you can specify an existing resource group when deploying a scale set from an Azure Resource Manager template.</span></span> <span data-ttu-id="edaf8-351">A méretezési készletben, Azure PowerShell vagy a parancssori felület használatával létrehozásakor egy meglévő erőforráscsoportot is megadható.</span><span class="sxs-lookup"><span data-stu-id="edaf8-351">You can also specify an existing resource group when creating a scale set using Azure PowerShell or CLI.</span></span>

### <a name="can-we-move-a-scale-set-tooanother-resource-group"></a><span data-ttu-id="edaf8-352">A Microsoft azt áthelyezheti egy méretezési tooanother erőforráscsoport?</span><span class="sxs-lookup"><span data-stu-id="edaf8-352">Can we move a scale set tooanother resource group?</span></span>

<span data-ttu-id="edaf8-353">Igen, áthelyezheti méretezési készlet erőforrások tooa új előfizetéshez vagy erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="edaf8-353">Yes, you can move scale set resources tooa new subscription or resource group.</span></span>

### <a name="how-tooi-update-my-virtual-machine-scale-set-tooa-new-image-how-do-i-manage-patching"></a><span data-ttu-id="edaf8-354">Hogyan tooI frissíteni a virtuális gép méretezési tooa új lemezkép?</span><span class="sxs-lookup"><span data-stu-id="edaf8-354">How tooI update my virtual machine scale set tooa new image?</span></span> <span data-ttu-id="edaf8-355">Hogyan kezelhetők a javítás?</span><span class="sxs-lookup"><span data-stu-id="edaf8-355">How do I manage patching?</span></span>

<span data-ttu-id="edaf8-356">a virtuális gép méretezési tooa új lemezképet, és a javítás toomanage, tooupdate lásd [frissítéséhez egy virtuálisgép-méretezési csoport](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span><span class="sxs-lookup"><span data-stu-id="edaf8-356">tooupdate your virtual machine scale set tooa new image, and toomanage patching, see [Upgrade a virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span></span>

### <a name="can-i-use-hello-reimage-operation-tooreset-a-vm-without-changing-hello-image-that-is-i-want-reset-a-vm-toofactory-settings-rather-than-tooa-new-image"></a><span data-ttu-id="edaf8-357">Hello kép megváltoztatása nélkül is használható hello lemezkép-visszaállítási művelet tooreset egy virtuális Gépet?</span><span class="sxs-lookup"><span data-stu-id="edaf8-357">Can I use hello reimage operation tooreset a VM without changing hello image?</span></span> <span data-ttu-id="edaf8-358">(Ez azt jelenti, hogy kívánt alaphelyzetbe állítása a virtuális gép toofactory beállításait ahelyett, hogy új lemezképet tooa.)</span><span class="sxs-lookup"><span data-stu-id="edaf8-358">(That is, I want reset a VM toofactory settings rather than tooa new image.)</span></span>

<span data-ttu-id="edaf8-359">Igen, hello kép megváltoztatása nélkül hello lemezkép-visszaállítási művelet tooreset egy virtuális Gépet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="edaf8-359">Yes, you can use hello reimage operation tooreset a VM without changing hello image.</span></span> <span data-ttu-id="edaf8-360">Azonban, ha a virtuálisgép-méretezési csoport platform lemezképre hivatkozik a `version = latest`, a virtuális gép tooa frissítheti az operációs rendszer újabb hívásakor kép `reimage`.</span><span class="sxs-lookup"><span data-stu-id="edaf8-360">However, if your virtual machine scale set references a platform image with `version = latest`, your VM can update tooa later OS image when you call `reimage`.</span></span>

<span data-ttu-id="edaf8-361">További információkért lásd: [virtuálisgép-méretezési csoportban lévő összes virtuális gépek kezeléséhez](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span><span class="sxs-lookup"><span data-stu-id="edaf8-361">For more information, see [Manage all VMs in a virtual machine scale set](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="edaf8-362">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="edaf8-362">Troubleshooting</span></span>

### <a name="how-do-i-turn-on-boot-diagnostics"></a><span data-ttu-id="edaf8-363">Hogyan rendszerindítási diagnosztika bekapcsolásához?</span><span class="sxs-lookup"><span data-stu-id="edaf8-363">How do I turn on boot diagnostics?</span></span>

<span data-ttu-id="edaf8-364">a rendszerindítási diagnosztika tooturn először hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="edaf8-364">tooturn on boot diagnostics, first, create a storage account.</span></span> <span data-ttu-id="edaf8-365">Ez a JSON-tömb, majd be a virtuálisgép-méretezési csoport **virtualMachineProfile**, és hello virtuálisgép-méretezési csoport frissítése:</span><span class="sxs-lookup"><span data-stu-id="edaf8-365">Then, put this JSON block in your virtual machine scale set **virtualMachineProfile**, and update hello virtual machine scale set:</span></span>

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

<span data-ttu-id="edaf8-366">Egy új virtuális gép létrehozásakor hello hello VM InstanceView tulajdonsága hello képernyőképe, és így tovább hello részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="edaf8-366">When a new VM is created, hello InstanceView property of hello VM shows hello details for hello screenshot, and so on.</span></span> <span data-ttu-id="edaf8-367">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="edaf8-367">Here's an example:</span></span>
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a><span data-ttu-id="edaf8-368">Virtuális gép tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="edaf8-368">Virtual machine properties</span></span>

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-hello-fault-domain-for-each-of-hello-100-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-369">Hogyan szerezhetek tulajdonság adatait az egyes virtuális gépek több hívás nélkül?</span><span class="sxs-lookup"><span data-stu-id="edaf8-369">How do I get property information for each VM without making multiple calls?</span></span> <span data-ttu-id="edaf8-370">Például hogyan kellene elérni hello tartalék tartomány minden hello 100 virtuális gépek a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="edaf8-370">For example, how would I get hello fault domain for each of hello 100 VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-371">tooget tulajdonság információ az egyes virtuális gépek anélkül, hogy több hívások hívása `ListVMInstanceViews` egy REST API végrehajtásával `GET` az erőforrás URI azonosítója a következő hello:</span><span class="sxs-lookup"><span data-stu-id="edaf8-371">tooget property information for each VM without making multiple calls, you can call `ListVMInstanceViews` by doing a REST API `GET` on hello following resource URI:</span></span>

<span data-ttu-id="edaf8-372">/Subscriptions/ < ELŐFIZETÉS_AZONOSÍTÓJA > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtuális gépek vannak? $expand argumentum = instanceView & $select = instanceView</span><span class="sxs-lookup"><span data-stu-id="edaf8-372">/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView</span></span>

### <a name="can-i-pass-different-extension-arguments-toodifferent-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="edaf8-373">I teljen különböző argumentumok toodifferent virtuális gépek egy virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="edaf8-373">Can I pass different extension arguments toodifferent VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="edaf8-374">Nem, nem adható át másik argumentumok toodifferent virtuális gépek egy virtuálisgép-méretezési csoportban lévő.</span><span class="sxs-lookup"><span data-stu-id="edaf8-374">No, you cannot pass different extension arguments toodifferent VMs in a virtual machine scale set.</span></span> <span data-ttu-id="edaf8-375">Bővítmények azonban egyedi tulajdonságainak hello hello futtatnak hello számítógépnév meg például a virtuális gép alapján működhet.</span><span class="sxs-lookup"><span data-stu-id="edaf8-375">However, extensions can act based on hello unique properties of hello VM they are running on, such as on hello machine name.</span></span> <span data-ttu-id="edaf8-376">Bővítmények is lekérdezheti a példány metaadatainak az http://169.254.169.254 tooget hello VM további információt.</span><span class="sxs-lookup"><span data-stu-id="edaf8-376">Extensions also can query instance metadata on http://169.254.169.254 tooget more information about hello VM.</span></span>

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a><span data-ttu-id="edaf8-377">Miért van a virtuális gép méretezési VM számítógép nevének és a virtuális gép azonosítók között lévő hézagokat?</span><span class="sxs-lookup"><span data-stu-id="edaf8-377">Why are there gaps between my virtual machine scale set VM machine names and VM IDs?</span></span> <span data-ttu-id="edaf8-378">Például: 0, 1, 3...</span><span class="sxs-lookup"><span data-stu-id="edaf8-378">For example: 0, 1, 3...</span></span>

<span data-ttu-id="edaf8-379">Nincsenek a virtuális gép méretezési VM számítógép nevének és a virtuális gép azonosítók között lévő hézagokat, mert a virtuálisgép-méretezési csoport **szükségesnél több erőforrás** tulajdonsága toohello alapértelmezett értékének **igaz**.</span><span class="sxs-lookup"><span data-stu-id="edaf8-379">There are gaps between your virtual machine scale set VM machine names and VM IDs because your virtual machine scale set **overprovision** property is set toohello default value of **true**.</span></span> <span data-ttu-id="edaf8-380">Ha túl elhelyezésétől értéke**igaz**, mint a kért jönnek létre további virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-380">If overprovisioning is set too**true**, more VMs than requested are created.</span></span> <span data-ttu-id="edaf8-381">Extra virtuális Gépekért törlődnek.</span><span class="sxs-lookup"><span data-stu-id="edaf8-381">Extra VMs are then deleted.</span></span> <span data-ttu-id="edaf8-382">Ebben az esetben azt szerezhet telepítés megbízhatóságát, azonban összefüggő elnevezési és összefüggő hálózati címfordítás (NAT) hello költségén szabályok.</span><span class="sxs-lookup"><span data-stu-id="edaf8-382">In this case, you gain increased deployment reliability, but at hello expense of contiguous naming and contiguous Network Address Translation (NAT) rules.</span></span> 

<span data-ttu-id="edaf8-383">Ez a tulajdonság túl állíthatja be**hamis**.</span><span class="sxs-lookup"><span data-stu-id="edaf8-383">You can set this property too**false**.</span></span> <span data-ttu-id="edaf8-384">Kis virtuálisgép-méretezési csoportok, a központi telepítés megbízhatóság jelentősen ez nincs hatással.</span><span class="sxs-lookup"><span data-stu-id="edaf8-384">For small virtual machine scale sets, this doesn't significantly affect deployment reliability.</span></span>

### <a name="what-is-hello-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-hello-vm-when-should-i-choose-one-over-hello-other"></a><span data-ttu-id="edaf8-385">Mi az, hogy a virtuálisgép-méretezési csoportban lévő virtuális gép törlése és hello virtuális gép felszabadítása hello különbségének?</span><span class="sxs-lookup"><span data-stu-id="edaf8-385">What is hello difference between deleting a VM in a virtual machine scale set and deallocating hello VM?</span></span> <span data-ttu-id="edaf8-386">Amikor kell választani egy más hello?</span><span class="sxs-lookup"><span data-stu-id="edaf8-386">When should I choose one over hello other?</span></span>

<span data-ttu-id="edaf8-387">hello virtuálisgép-méretezési csoportban lévő virtuális gép törlése és hello virtuális gép felszabadítása közötti fő különbség, hogy `deallocate` hello virtuális merevlemezeket (VHD) nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="edaf8-387">hello main difference between deleting a VM in a virtual machine scale set and deallocating hello VM is that `deallocate` doesn’t delete hello virtual hard disks (VHDs).</span></span> <span data-ttu-id="edaf8-388">Nincsenek futó társított tárolási költségek `stop deallocate`.</span><span class="sxs-lookup"><span data-stu-id="edaf8-388">There are storage costs associated with running `stop deallocate`.</span></span> <span data-ttu-id="edaf8-389">Előfordulhat, hogy egy alkalmazni vagy hello más hello a következő okok valamelyike miatt:</span><span class="sxs-lookup"><span data-stu-id="edaf8-389">You might use one or hello other for one of hello following reasons:</span></span>

- <span data-ttu-id="edaf8-390">Azt szeretné, hogy a számítási költségek fizető toostop, de azt szeretné, hogy a virtuális gépek hello tookeep hello lemez állapotát.</span><span class="sxs-lookup"><span data-stu-id="edaf8-390">You want toostop paying compute costs, but you want tookeep hello disk state of hello VMs.</span></span>
- <span data-ttu-id="edaf8-391">Virtuális gépek halmaza toostart gyorsabban sikerült horizontális egy virtuálisgép-méretezési csoport szeretné.</span><span class="sxs-lookup"><span data-stu-id="edaf8-391">You want toostart a set of VMs more quickly than you could scale out a virtual machine scale set.</span></span>
  - <span data-ttu-id="edaf8-392">Kapcsolódó toothis esetben előfordulhat, hogy létrehozta a saját automatikus skálázás-motor és a kívánt gyorsabb végpontok közötti skálán.</span><span class="sxs-lookup"><span data-stu-id="edaf8-392">Related toothis scenario, you might have created your own autoscale engine and want a faster end-to-end scale.</span></span>
- <span data-ttu-id="edaf8-393">A virtuálisgép-méretezési csoport, amely nem egyenlően elosztva tartalék tartományok vagy a frissítési tartományok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="edaf8-393">You have a virtual machine scale set that is unevenly distributed across fault domains or update domains.</span></span> <span data-ttu-id="edaf8-394">Ez lehet, mert szelektív módon törli a virtuális gépek vagy virtuális gépek elhelyezésétől után törölték.</span><span class="sxs-lookup"><span data-stu-id="edaf8-394">This might be because you selectively deleted VMs, or because VMs were deleted after overprovisioning.</span></span> <span data-ttu-id="edaf8-395">Futó `stop deallocate` követ `start` hello a virtuális gép méretezési készletben egyenletes elosztása az hello virtuális gépek tartalék tartományok vagy a frissítési tartományok.</span><span class="sxs-lookup"><span data-stu-id="edaf8-395">Running `stop deallocate` followed by `start` on hello virtual machine scale set evenly distributes hello VMs across fault domains or update domains.</span></span>

