---
title: "Az Azure virtuálisgép-skálázási készletekben – gyakori kérdések |} Microsoft Docs"
description: "Virtuálisgép-méretezési csoportok gyakran feltett kérdésekre adott válaszok."
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
ms.openlocfilehash: f320dd5d1f8c99317792f4ae9e09bc5adaf79e25
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a><span data-ttu-id="a76df-103">Az Azure virtuálisgép-skálázási készletekben – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a76df-103">Azure virtual machine scale sets FAQs</span></span>

<span data-ttu-id="a76df-104">Válaszok virtuálisgép-méretezési csoportok kapcsolatos gyakori kérdések az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a76df-104">Get answers to frequently asked questions about virtual machine scale sets in Azure.</span></span>

## <a name="autoscale"></a><span data-ttu-id="a76df-105">Automatikus méretezés</span><span class="sxs-lookup"><span data-stu-id="a76df-105">Autoscale</span></span>

### <a name="what-are-best-practices-for-azure-autoscale"></a><span data-ttu-id="a76df-106">Mik az Azure automatikus skálázás ajánlott eljárásai?</span><span class="sxs-lookup"><span data-stu-id="a76df-106">What are best practices for Azure Autoscale?</span></span>

<span data-ttu-id="a76df-107">Ajánlott eljárások az automatikus skálázás, lásd: [ajánlott eljárások a virtuális gépek automatikus skálázás](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span><span class="sxs-lookup"><span data-stu-id="a76df-107">For best practices for Autoscale, see [Best practices for autoscaling virtual machines](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span></span>

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a><span data-ttu-id="a76df-108">Hol található a következő automatikus skálázás állomásalapú metrikák használó metrika neve?</span><span class="sxs-lookup"><span data-stu-id="a76df-108">Where do I find metric names for autoscaling that uses host-based metrics?</span></span>

<span data-ttu-id="a76df-109">Automatikus skálázás állomásalapú metrikák használó mérték nevét, lásd: [támogatott Azure-figyelő metrikák](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span><span class="sxs-lookup"><span data-stu-id="a76df-109">For metric names for autoscaling that uses host-based metrics, see [Supported metrics with Azure Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span></span>

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a><span data-ttu-id="a76df-110">Vannak-e az Azure Service Bus témakör és a várólista hossza alapján automatikus skálázás bármely példái?</span><span class="sxs-lookup"><span data-stu-id="a76df-110">Are there any examples of autoscaling based on an Azure Service Bus topic and queue length?</span></span>

<span data-ttu-id="a76df-111">Igen.</span><span class="sxs-lookup"><span data-stu-id="a76df-111">Yes.</span></span> <span data-ttu-id="a76df-112">Az Azure Service Bus témakör és a várólista hossza alapján automatikus skálázás példákért lásd [Azure figyelő automatikus skálázás közös metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span><span class="sxs-lookup"><span data-stu-id="a76df-112">For examples of autoscaling based on an Azure Service Bus topic and queue length, see [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span></span>

<span data-ttu-id="a76df-113">A Service Bus-üzenetsorba használja a következő JSON:</span><span class="sxs-lookup"><span data-stu-id="a76df-113">For a Service Bus queue, use the following JSON:</span></span>

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

<span data-ttu-id="a76df-114">Tároló várólista használja a következő JSON:</span><span class="sxs-lookup"><span data-stu-id="a76df-114">For a storage queue, use the following JSON:</span></span>

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

<span data-ttu-id="a76df-115">Cserélje a erőforrás egységes erőforrás-azonosítók (URI).</span><span class="sxs-lookup"><span data-stu-id="a76df-115">Replace example values with your resource Uniform Resource Identifiers (URIs).</span></span>


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a><span data-ttu-id="a76df-116">Kell I Automatikus méretezéssel állomásalapú metrikák vagy a diagnosztika bővítmény használatával?</span><span class="sxs-lookup"><span data-stu-id="a76df-116">Should I autoscale by using host-based metrics or a diagnostics extension?</span></span>

<span data-ttu-id="a76df-117">Az automatikus skálázási beállítás hozhat létre a virtuális gép a gazdagép szintekhez tartozó metrikákat vagy a vendég operációs rendszer-alapú metrikákat.</span><span class="sxs-lookup"><span data-stu-id="a76df-117">You can create an autoscale setting on a VM to use host-level metrics or guest OS-based metrics.</span></span>

<span data-ttu-id="a76df-118">A támogatott mérőszámok listájáért lásd: [Azure figyelő automatikus skálázás közös metrikák](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span><span class="sxs-lookup"><span data-stu-id="a76df-118">For a list of supported metrics, see [Azure Monitor autoscaling common metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span></span> 

<span data-ttu-id="a76df-119">A teljes minta a virtuálisgép-méretezési csoportok, lásd: [speciális automatikus skálázás konfigurációs Resource Manager-sablonok használatával virtuálisgép-méretezési csoportok](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span><span class="sxs-lookup"><span data-stu-id="a76df-119">For a full sample for virtual machine scale sets, see [Advanced autoscale configuration by using Resource Manager templates for virtual machine scale sets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span></span> 

<span data-ttu-id="a76df-120">A minta a gazdaszintű CPU metrika és üzenet mérőszámot használja.</span><span class="sxs-lookup"><span data-stu-id="a76df-120">The sample uses the host-level CPU metric and a message count metric.</span></span>



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a><span data-ttu-id="a76df-121">Hogyan állíthatom riasztási szabályok a virtuálisgép-méretezési csoportot?</span><span class="sxs-lookup"><span data-stu-id="a76df-121">How do I set alert rules on a virtual machine scale set?</span></span>

<span data-ttu-id="a76df-122">A PowerShell vagy az Azure parancssori felület használatával virtuálisgép-méretezési csoportok metrikáját riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="a76df-122">You can create alerts on metrics for virtual machine scale sets via PowerShell or Azure CLI.</span></span> <span data-ttu-id="a76df-123">További információkért lásd: [Azure figyelő PowerShell quick start minták](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) és [figyelő Azure platformfüggetlen parancssori felület gyors start minták](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span><span class="sxs-lookup"><span data-stu-id="a76df-123">For more information, see [Azure Monitor PowerShell quick start samples](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) and [Azure Monitor cross-platform CLI quick start samples](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span></span>

<span data-ttu-id="a76df-124">A virtuálisgép-méretezési csoport targetresourceid azonosítója így néz ki:</span><span class="sxs-lookup"><span data-stu-id="a76df-124">The TargetResourceId of the virtual machine scale set looks like this:</span></span> 

<span data-ttu-id="a76df-125">/Subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/Providers/Microsoft.COMPUTE/virtualMachineScaleSets/yourvmssname</span><span class="sxs-lookup"><span data-stu-id="a76df-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span></span>

<span data-ttu-id="a76df-126">A metrika a riasztások, dönthet úgy a virtuális gép teljesítményszámláló.</span><span class="sxs-lookup"><span data-stu-id="a76df-126">You can choose any VM performance counter as the metric to set an alert for.</span></span> <span data-ttu-id="a76df-127">További információkért lásd: [Resource Manager-alapú Windows virtuális gépek vendég operációs rendszer metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) és [Linux virtuális gépek vendég operációs rendszer metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) a a [Azure figyelő automatikus skálázás közös metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) cikk.</span><span class="sxs-lookup"><span data-stu-id="a76df-127">For more information, see [Guest OS metrics for Resource Manager-based Windows VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) and [Guest OS metrics for Linux VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) in the [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) article.</span></span>

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a><span data-ttu-id="a76df-128">Hogyan állíthatom be automatikus skálázás a PowerShell segítségével állítsa be a virtuálisgép-méretezési?</span><span class="sxs-lookup"><span data-stu-id="a76df-128">How do I set up autoscale on a virtual machine scale set by using PowerShell?</span></span>

<span data-ttu-id="a76df-129">A virtuálisgép-méretezési PowerShell segítségével állítsa be automatikus skálázás beállításához tekintse meg a következő blogbejegyzésben található [automatikus skálázás felvétele az Azure virtuálisgép-méretezési csoport](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="a76df-129">To set up autoscale on a virtual machine scale set by using PowerShell, see the blog post [How to add autoscale to an Azure virtual machine scale set](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span></span>




## <a name="certificates"></a><span data-ttu-id="a76df-130">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="a76df-130">Certificates</span></span>

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm-how-do-i-provision-a-virtual-machine-scale-set-to-run-a-website-where-the-ssl-for-the-website-is-shipped-securely-from-a-certificate-configuration-the-common-certificate-rotation-operation-would-be-almost-the-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-to-do-this"></a><span data-ttu-id="a76df-131">Hogyan tegye szeretnék biztonságosan küldje el a virtuális gép tanúsítványt?</span><span class="sxs-lookup"><span data-stu-id="a76df-131">How do I securely ship a certificate to the VM?</span></span> <span data-ttu-id="a76df-132">Hogyan kiépíteni a futtatásához egy webhelyet, ahol a webhely SSL nyújtják biztonságosan tanúsítvány konfigurációról beállítása virtuálisgép-méretezési?</span><span class="sxs-lookup"><span data-stu-id="a76df-132">How do I provision a virtual machine scale set to run a website where the SSL for the website is shipped securely from a certificate configuration?</span></span> <span data-ttu-id="a76df-133">(A közös tanúsítvány forgatási művelet lenne majdnem megegyezik a konfiguráció-frissítési művelet.) Példa bemutatja, hogyan ehhez van?</span><span class="sxs-lookup"><span data-stu-id="a76df-133">(The common certificate rotation operation would be almost the same as a configuration update operation.) Do you have an example of how to do this?</span></span> 

<span data-ttu-id="a76df-134">Biztonságos módon küldje el a virtuális gép egy tanúsítványt, a felhasználói tanúsítvány is telepítheti közvetlenül tanúsítványtárolóba a Windows a vásárlói kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="a76df-134">To securely ship a certificate to the VM, you can install a customer certificate directly into a Windows certificate store from the customer's key vault.</span></span>

<span data-ttu-id="a76df-135">Használja a következő JSON:</span><span class="sxs-lookup"><span data-stu-id="a76df-135">Use the following JSON:</span></span>

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

<span data-ttu-id="a76df-136">A kód a Windows és Linux támogatja.</span><span class="sxs-lookup"><span data-stu-id="a76df-136">The code supports Windows and Linux.</span></span>

<span data-ttu-id="a76df-137">További információkért lásd: [létrehozás vagy frissítés egy virtuálisgép-méretezési készlet](https://msdn.microsoft.com/library/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="a76df-137">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/mt589035.aspx).</span></span>


### <a name="example-of-self-signed-certificate"></a><span data-ttu-id="a76df-138">Önaláírt tanúsítvány – példa</span><span class="sxs-lookup"><span data-stu-id="a76df-138">Example of Self-signed certificate</span></span>

1.  <span data-ttu-id="a76df-139">Hozzon létre egy önaláírt tanúsítványt kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="a76df-139">Create a self-signed certificate in a key vault.</span></span>

    <span data-ttu-id="a76df-140">A következő PowerShell-parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="a76df-140">Use the following PowerShell commands:</span></span>

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    <span data-ttu-id="a76df-141">Ez a parancs lehetővé teszi a bemeneti az Azure Resource Manager sablonhoz.</span><span class="sxs-lookup"><span data-stu-id="a76df-141">This command gives you the input for the Azure Resource Manager template.</span></span>

    <span data-ttu-id="a76df-142">Például egy önaláírt tanúsítvány létrehozása a kulcstároló, lásd: [Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span><span class="sxs-lookup"><span data-stu-id="a76df-142">For an example of how to create a self-signed certificate in a key vault, see [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span></span>

2.  <span data-ttu-id="a76df-143">A Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="a76df-143">Change the Resource Manager template.</span></span>

    <span data-ttu-id="a76df-144">Adja hozzá ezt a tulajdonságot **virtualMachineProfile**, részeként a virtuálisgép-méretezési készlet erőforrás:</span><span class="sxs-lookup"><span data-stu-id="a76df-144">Add this property to **virtualMachineProfile**, as part of the virtual machine scale set resource:</span></span>

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
  

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a><span data-ttu-id="a76df-145">Megadhatja egy SSH-kulcspárral állítson be úgy a Resource Manager-sablon a Linux virtuális gép skálával SSH hitelesítés használatához?</span><span class="sxs-lookup"><span data-stu-id="a76df-145">Can I specify an SSH key pair to use for SSH authentication with a Linux virtual machine scale set from a Resource Manager template?</span></span>  

<span data-ttu-id="a76df-146">Igen.</span><span class="sxs-lookup"><span data-stu-id="a76df-146">Yes.</span></span> <span data-ttu-id="a76df-147">A REST API-t a **osProfile** hasonlít a szabványos VM REST API-t.</span><span class="sxs-lookup"><span data-stu-id="a76df-147">The REST API for **osProfile** is similar to the standard VM REST API.</span></span> 

<span data-ttu-id="a76df-148">Tartalmaznak **osProfile** a sablonban:</span><span class="sxs-lookup"><span data-stu-id="a76df-148">Include **osProfile** in your template:</span></span>

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
 
<span data-ttu-id="a76df-149">Ez a JSON-tömb használatos [a 101-vm-ssh-kulcsfájl GitHub gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="a76df-149">This JSON block is used in [the 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>
 
<span data-ttu-id="a76df-150">Az operációsrendszer-profil is van használatban [a grelayhost.json GitHub quick start sablon](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span><span class="sxs-lookup"><span data-stu-id="a76df-150">The OS profile also is used in [the grelayhost.json GitHub quick start template](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span></span>

<span data-ttu-id="a76df-151">További információkért lásd: [létrehozás vagy frissítés egy virtuálisgép-méretezési készlet](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span><span class="sxs-lookup"><span data-stu-id="a76df-151">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span></span>
  

### <a name="how-do-i-remove-deprecated-certificates"></a><span data-ttu-id="a76df-152">Hogyan távolíthatom elavult tanúsítványok?</span><span class="sxs-lookup"><span data-stu-id="a76df-152">How do I remove deprecated certificates?</span></span> 

<span data-ttu-id="a76df-153">Távolítja el az elavult, távolítsa el a régi tanúsítvány a tárolóban tanúsítványok listájából.</span><span class="sxs-lookup"><span data-stu-id="a76df-153">To remove deprecated certificates, remove the old certificate from the vault certificates list.</span></span> <span data-ttu-id="a76df-154">Hagyja meg szeretné őrizni a listában a számítógép összes tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a76df-154">Leave all the certificates that you want to remain on your computer in the list.</span></span> <span data-ttu-id="a76df-155">Ez a virtuális gépek nem távolítja el a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a76df-155">This does not remove the certificate from all your VMs.</span></span> <span data-ttu-id="a76df-156">Azt is nem adja hozzá a tanúsítványt a virtuálisgép-méretezési csoportban lévő létrehozott új virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a76df-156">It also does not add the certificate to new VMs that are created in the virtual machine scale set.</span></span> 

<span data-ttu-id="a76df-157">A tanúsítvány eltávolítása a meglévő virtuális gépek, írni egy egyéni parancsprogramok futtatására szolgáló bővítmény távolíthatja el kézzel a tanúsítványokat a tanúsítványtárolóból.</span><span class="sxs-lookup"><span data-stu-id="a76df-157">To remove the certificate from existing VMs, write a custom script extension to manually remove the certificates from your certificate store.</span></span>
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-to-store-the-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a><span data-ttu-id="a76df-158">Hogyan do I behelyezése egy meglévő nyilvános SSH-kulcsot a virtuális gép méretezési készlet SSH réteg kiépítése során?</span><span class="sxs-lookup"><span data-stu-id="a76df-158">How do I inject an existing SSH public key into the virtual machine scale set SSH layer during provisioning?</span></span> <span data-ttu-id="a76df-159">Az SSH nyilvános kulcs értékei tárolása az Azure Key Vault, és ezeket a Resource Manager-sablon majd használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="a76df-159">I want to store the SSH public key values in Azure Key Vault, and then use them in my Resource Manager template.</span></span>

<span data-ttu-id="a76df-160">Ha meg van adva a virtuális gépek csak a nyilvános SSH-kulcsot, nem kell a nyilvános kulcsok be kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="a76df-160">If you are providing the VMs only with a public SSH key, you don't need to put the public keys in Key Vault.</span></span> <span data-ttu-id="a76df-161">Nyilvános kulcsai nem titkos.</span><span class="sxs-lookup"><span data-stu-id="a76df-161">Public keys are not secret.</span></span>
 
<span data-ttu-id="a76df-162">Megadhat egyszerű szöveges nyilvános SSH-kulcsok Linux virtuális gép létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="a76df-162">You can provide SSH public keys in plain text when you create a Linux VM:</span></span>

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
 
<span data-ttu-id="a76df-163">linuxConfiguration elem neve</span><span class="sxs-lookup"><span data-stu-id="a76df-163">linuxConfiguration element name</span></span> | <span data-ttu-id="a76df-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a76df-164">Required</span></span> | <span data-ttu-id="a76df-165">Típus</span><span class="sxs-lookup"><span data-stu-id="a76df-165">Type</span></span> | <span data-ttu-id="a76df-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="a76df-166">Description</span></span>
--- | --- | --- | --- |  ---
<span data-ttu-id="a76df-167">ssh</span><span class="sxs-lookup"><span data-stu-id="a76df-167">ssh</span></span> | <span data-ttu-id="a76df-168">Nem</span><span class="sxs-lookup"><span data-stu-id="a76df-168">No</span></span> | <span data-ttu-id="a76df-169">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="a76df-169">Collection</span></span> | <span data-ttu-id="a76df-170">Adja meg a Linux operációs rendszert futtató SSH-kulcs konfigurációja</span><span class="sxs-lookup"><span data-stu-id="a76df-170">Specifies the SSH key configuration for a Linux OS</span></span>
<span data-ttu-id="a76df-171">Elérési út</span><span class="sxs-lookup"><span data-stu-id="a76df-171">path</span></span> | <span data-ttu-id="a76df-172">Igen</span><span class="sxs-lookup"><span data-stu-id="a76df-172">Yes</span></span> | <span data-ttu-id="a76df-173">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a76df-173">String</span></span> | <span data-ttu-id="a76df-174">Ha az SSH-kulcsok vagy tanúsítványt kell elhelyezkedniük Linux elérési</span><span class="sxs-lookup"><span data-stu-id="a76df-174">Specifies the Linux file path where the SSH keys or certificate should be located</span></span>
<span data-ttu-id="a76df-175">keyData</span><span class="sxs-lookup"><span data-stu-id="a76df-175">keyData</span></span> | <span data-ttu-id="a76df-176">Igen</span><span class="sxs-lookup"><span data-stu-id="a76df-176">Yes</span></span> | <span data-ttu-id="a76df-177">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a76df-177">String</span></span> | <span data-ttu-id="a76df-178">Megadja a base64-kódolású nyilvános SSH-kulcs</span><span class="sxs-lookup"><span data-stu-id="a76df-178">Specifies a base64-encoded SSH public key</span></span>

<span data-ttu-id="a76df-179">Egy vonatkozó példáért lásd: [a 101-vm-ssh-kulcsfájl GitHub gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="a76df-179">For an example, see [the 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a><span data-ttu-id="a76df-180">Futtatásakor `Update-AzureRmVmss` egynél több tanúsítvány hozzáadása az azonos kulcsú tárolóból, után látható a következő üzenetet:</span><span class="sxs-lookup"><span data-stu-id="a76df-180">When I run `Update-AzureRmVmss` after adding more than one certificate from the same key vault, I see the following message:</span></span>
 
><span data-ttu-id="a76df-181">Frissítés-AzureRmVmss: Lista titkos kulcsot tartalmaz többszöri /subscriptions/ < saját előfizetés-azonosító > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, ami nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="a76df-181">Update-AzureRmVmss: List secret contains repeated instances of /subscriptions/<my-subscription-id>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, which is disallowed.</span></span>
 
<span data-ttu-id="a76df-182">Ez akkor fordulhat elő, ha kísérli meg újra hozzá az új tároló tanúsítvány helyett a meglévő forrás-tároló azonos tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a76df-182">This can happen if you try to re-add the same vault instead of using a new vault certificate for the existing source vault.</span></span> <span data-ttu-id="a76df-183">A `Add-AzureRmVmssSecret` parancs nem működik megfelelően, ha további titkok hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="a76df-183">The `Add-AzureRmVmssSecret` command does not work correctly if you are adding additional secrets.</span></span>
 
<span data-ttu-id="a76df-184">Az azonos kulcstároló további titkok hozzáadásához a $vmss.properties.osProfile.secrets[0].vaultCertificates lista frissítése.</span><span class="sxs-lookup"><span data-stu-id="a76df-184">To add more secrets from the same key vault, update the $vmss.properties.osProfile.secrets[0].vaultCertificates list.</span></span>
 
<span data-ttu-id="a76df-185">Tekintse meg a várt bemeneti szerkezetből [létrehozás vagy frissítés egy virtuálisgép-csoport](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="a76df-185">For the expected input structure, see [Create or update a virtual machine set](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span></span>
 
<span data-ttu-id="a76df-186">A titkos kulcs található a virtuális gép méretezési készlet objektum, amely a kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="a76df-186">Find the secret in the virtual machine scale set object that is in the key vault.</span></span> <span data-ttu-id="a76df-187">Adja hozzá a tárolóhoz rendelt lista a tanúsítvány hivatkozás (az URL-cím és a titkos nevét).</span><span class="sxs-lookup"><span data-stu-id="a76df-187">Then, add your certificate reference (the URL and the secret store name) to the list associated with the vault.</span></span>

> [!NOTE] 
> <span data-ttu-id="a76df-188">Jelenleg nem távolítható el tanúsítványok virtuális gépek a virtuális gép méretezési készlet API használatával.</span><span class="sxs-lookup"><span data-stu-id="a76df-188">Currently, you cannot remove certificates from VMs by using the virtual machine scale set API.</span></span>
>

<span data-ttu-id="a76df-189">Új virtuális gép nem fog a régi tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="a76df-189">New VMs will not have the old certificate.</span></span> <span data-ttu-id="a76df-190">A tanúsítvány van, és a már telepített virtuális gépek azonban a régi tanúsítvány rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a76df-190">However, VMs that have the certificate and which are already deployed will have the old certificate.</span></span>
 
### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a><span data-ttu-id="a76df-191">Képes tanúsítványokat is leküldéses a virtuális gép méretezése nélkül a jelszó megadása, ha a tanúsítvány titkos tárolójában?</span><span class="sxs-lookup"><span data-stu-id="a76df-191">Can I push certificates to the virtual machine scale set without providing the password, when the certificate is in the secret store?</span></span>

<span data-ttu-id="a76df-192">Nem kell kódolnia jelszavak parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="a76df-192">You do not need to hard-code passwords in scripts.</span></span> <span data-ttu-id="a76df-193">A telepítési parancsfájl futtatásához használt engedélyeivel jelszavak dinamikusan kérheti le.</span><span class="sxs-lookup"><span data-stu-id="a76df-193">You can dynamically retrieve passwords with the permissions you use to run the deployment script.</span></span> <span data-ttu-id="a76df-194">Ha egy parancsfájlt, mely az egy tanúsítványt a titkos a tároló kulcsa használó, a titkos tároló `get certificate` parancs is kiírja a jelszót a .pfx fájl.</span><span class="sxs-lookup"><span data-stu-id="a76df-194">If you have a script that moves a certificate from the secret store key vault, the secret store `get certificate` command also outputs the password of the .pfx file.</span></span>
 
### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a><span data-ttu-id="a76df-195">Hogyan nem a titkos kulcsok a virtuálisgép-méretezési virtualMachineProfile.osProfile tulajdonsága munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="a76df-195">How does the Secrets property of virtualMachineProfile.osProfile for a virtual machine scale set work?</span></span> <span data-ttu-id="a76df-196">Miért kell meg sourceVault értékét, ha adja meg a tanúsítvány abszolút URI-JÁNAK certificateUrl tulajdonságának használatával kell?</span><span class="sxs-lookup"><span data-stu-id="a76df-196">Why do I need the sourceVault value when I have to specify the absolute URI for a certificate by using the certificateUrl property?</span></span> 

<span data-ttu-id="a76df-197">A Rendszerfelügyeleti (webszolgáltatások WinRM) tanúsítványhivatkozást az operációsrendszer-profilt a titkos kulcsok tulajdonságában jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a76df-197">A Windows Remote Management (WinRM) certificate reference must be present in the Secrets property of the OS profile.</span></span> 

<span data-ttu-id="a76df-198">A forrás tároló jelző célja hozzáférés-vezérlési lista (ACL) házirendeket, amely szerepel a felhasználó Azure Cloud Service modell érvényesítését.</span><span class="sxs-lookup"><span data-stu-id="a76df-198">The purpose of indicating the source vault is to enforce access control list (ACL) policies that exist in a user's Azure Cloud Service model.</span></span> <span data-ttu-id="a76df-199">Ha nincs megadva a forrás-tárolóba, telepítéséhez, vagy titkos kulcsok számára kulcstároló hozzáférési jogosultságokkal nem rendelkező felhasználók lenne képes keresztül egy számítási erőforrás szolgáltató (CRP).</span><span class="sxs-lookup"><span data-stu-id="a76df-199">If the source vault isn't specified, users who do not have permissions to deploy or access secrets to a key vault would be able to through a Compute Resource Provider (CRP).</span></span> <span data-ttu-id="a76df-200">Hozzáférés-vezérlési listák létezik még a erőforrásokat, amelyek nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="a76df-200">ACLs exist even for resources that do not exist.</span></span>

<span data-ttu-id="a76df-201">Ha egy nem megfelelő tároló Forrásazonosítóval azonban egy érvényes kulcstároló URL-címet ad meg, hibaüzenet jelenik meg kérdezze le a művelet során.</span><span class="sxs-lookup"><span data-stu-id="a76df-201">If you provide an incorrect source vault ID but a valid key vault URL, an error is reported when you poll the operation.</span></span>
 
### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a><span data-ttu-id="a76df-202">Ha a titkos kulcsok hozzáadni egy meglévő virtuálisgép-méretezési beállítása, a titkos kulcsok beszúrta a meglévő virtuális gépekhez, vagy csak újakat?</span><span class="sxs-lookup"><span data-stu-id="a76df-202">If I add secrets to an existing virtual machine scale set, are the secrets injected into existing VMs, or only into new ones?</span></span> 

<span data-ttu-id="a76df-203">Az összes a virtuális gépen, akkor már létező azokat, hozzáadja a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="a76df-203">Certificates are added to all your VMs, even preexisting ones.</span></span> <span data-ttu-id="a76df-204">Ha a virtuálisgép-méretezési készlet upgradePolicy tulajdonság értéke **manuális**, a virtuális gép hozzáadta a tanúsítványt, amikor egy kézi frissítés hajt végre a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a76df-204">If your virtual machine scale set upgradePolicy property is set to **manual**, the certificate is added to the VM when you perform a manual update on the VM.</span></span>
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a><span data-ttu-id="a76df-205">Ha put tanúsítványok Linux virtuális gépekhez?</span><span class="sxs-lookup"><span data-stu-id="a76df-205">Where do I put certificates for Linux VMs?</span></span>

<span data-ttu-id="a76df-206">Tanúsítványok telepítése a Linux virtuális gépekhez, lásd: [tanúsítványok telepítése a virtuális gépek az ügyfél által felügyelt kulcstároló](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="a76df-206">To learn how to deploy certificates for Linux VMs, see [Deploy certificates to VMs from a customer-managed key vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span></span>
  
### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a><span data-ttu-id="a76df-207">Hogyan új tároló tanúsítvány hozzáadása egy új tanúsítvány objektumot?</span><span class="sxs-lookup"><span data-stu-id="a76df-207">How do I add a new vault certificate to a new certificate object?</span></span>

<span data-ttu-id="a76df-208">Tároló tanúsítvány hozzáadása egy meglévő titkos kulcsot, tekintse meg a következő PowerShell-példa.</span><span class="sxs-lookup"><span data-stu-id="a76df-208">To add a vault certificate to an existing secret, see the following PowerShell example.</span></span> <span data-ttu-id="a76df-209">Csak egy titkos objektum használja.</span><span class="sxs-lookup"><span data-stu-id="a76df-209">Use only one secret object.</span></span>
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a><span data-ttu-id="a76df-210">Mi történik a tanúsítványok, ha a virtuális gépek újból lemezképet létrehozni?</span><span class="sxs-lookup"><span data-stu-id="a76df-210">What happens to certificates if you reimage a VM?</span></span>

<span data-ttu-id="a76df-211">Ha Ön újból lemezképet létrehozni egy virtuális Gépet, a tanúsítványok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="a76df-211">If you reimage a VM, certificates are deleted.</span></span> <span data-ttu-id="a76df-212">Törli a teljes operációs rendszer lemez szerepkörpéldány rendszerképét.</span><span class="sxs-lookup"><span data-stu-id="a76df-212">Reimaging deletes the entire OS disk.</span></span> 
 
### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a><span data-ttu-id="a76df-213">Mi történik, ha a tanúsítvány törlése a key vault?</span><span class="sxs-lookup"><span data-stu-id="a76df-213">What happens if you delete a certificate from the key vault?</span></span>

<span data-ttu-id="a76df-214">Ha a titkos kulcsot a key vault törlődik, és ezt követően futtatni `stop deallocate` a virtuális géphez, majd indítsa el újra azokat hiba fog történni.</span><span class="sxs-lookup"><span data-stu-id="a76df-214">If the secret is deleted from the key vault, and then you run `stop deallocate` for all your VMs and then start them again, you will encounter a failure.</span></span> <span data-ttu-id="a76df-215">A hiba oka, hogy a KSZT kell a titkos kulcsok lekérése a kulcstároló, de ez nem lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a76df-215">The failure occurs because the CRP needs to retrieve the secrets from the key vault, but it cannot.</span></span> <span data-ttu-id="a76df-216">Ebben a forgatókönyvben a virtuális gép méretezési készlet modellből törölheti a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="a76df-216">In this scenario, you can delete the certificates from the virtual machine scale set model.</span></span> 

<span data-ttu-id="a76df-217">A CRP-összetevő nem maradnak ügyfél titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="a76df-217">The CRP component does not persist customer secrets.</span></span> <span data-ttu-id="a76df-218">Ha `stop deallocate` a virtuálisgép-méretezési csoportban lévő összes virtuális gép esetében a gyorsítótár törlődik.</span><span class="sxs-lookup"><span data-stu-id="a76df-218">If you run `stop deallocate` for all VMs in the virtual machine scale set, the cache is deleted.</span></span> <span data-ttu-id="a76df-219">Ebben a forgatókönyvben kulcsait a rendszer beolvassa az a key vault.</span><span class="sxs-lookup"><span data-stu-id="a76df-219">In this scenario, secrets are retrieved from the key vault.</span></span>

<span data-ttu-id="a76df-220">Ez a probléma nem tapasztal, amikor, mert a titkos kulcsot az Azure Service Fabric (a bérlő egyetlen-háló modell) gyorsítótárazott másolatának kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="a76df-220">You don't encounter this problem when scaling out because there is a cached copy of the secret in Azure Service Fabric (in the single-fabric tenant model).</span></span>
 
### <a name="why-do-i-have-to-specify-the-exact-location-for-the-certificate-url-httpsname-of-the-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a><span data-ttu-id="a76df-221">Miért kell segítségével megadhatja a pontos helyet a tanúsítvány URL-címhez (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>), a [Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span><span class="sxs-lookup"><span data-stu-id="a76df-221">Why do I have to specify the exact location for the certificate URL (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>), as indicated in [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span></span>
 
<span data-ttu-id="a76df-222">Az Azure Key Vault dokumentáció szerint, hogy az beszerzése titkos REST API-t a legújabb verzióját a titkos kulcsot kell visszaadnia, ha nincs megadva a verziója.</span><span class="sxs-lookup"><span data-stu-id="a76df-222">The Azure Key Vault documentation states that the Get Secret REST API should return the latest version of the secret if the version is not specified.</span></span>
 
<span data-ttu-id="a76df-223">Módszer</span><span class="sxs-lookup"><span data-stu-id="a76df-223">Method</span></span> | <span data-ttu-id="a76df-224">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="a76df-224">URL</span></span>
--- | ---
<span data-ttu-id="a76df-225">GET</span><span class="sxs-lookup"><span data-stu-id="a76df-225">GET</span></span> | <span data-ttu-id="a76df-226">https://mykeyvault.vault.Azure.NET/secrets/ {titkos kulcs neve} / {titkos-version}? api-version = {api-version}</span><span class="sxs-lookup"><span data-stu-id="a76df-226">https://mykeyvault.vault.azure.net/secrets/{secret-name}/{secret-version}?api-version={api-version}</span></span>

<span data-ttu-id="a76df-227">Cserélje le {*titkos-name*} nevű, és cserélje le {*titkos verziójú*} a titkos kulcsot szeretné beolvasni a verzióját.</span><span class="sxs-lookup"><span data-stu-id="a76df-227">Replace {*secret-name*} with the name, and replace {*secret-version*} with the version of the secret you want to retrieve.</span></span> <span data-ttu-id="a76df-228">Előfordulhat, hogy ki kell zárni a titkos kulcs verzióját.</span><span class="sxs-lookup"><span data-stu-id="a76df-228">The secret version might be excluded.</span></span> <span data-ttu-id="a76df-229">Ebben az esetben az aktuális verzió lekérése.</span><span class="sxs-lookup"><span data-stu-id="a76df-229">In that case, the current version is retrieved.</span></span>
  
### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a><span data-ttu-id="a76df-230">Miért kell megadni a tanúsítvány verzióját, ha a Key Vault használni?</span><span class="sxs-lookup"><span data-stu-id="a76df-230">Why do I have to specify the certificate version when I use Key Vault?</span></span>

<span data-ttu-id="a76df-231">A Key Vault vonatkozó követelményt a adja meg a tanúsítvány verziója célja abba, hogy törli a jelölést, a felhasználó milyen tanúsítvány van telepítve. a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="a76df-231">The purpose of the Key Vault requirement to specify the certificate version is to make it clear to the user what certificate is deployed on their VMs.</span></span>

<span data-ttu-id="a76df-232">Hozzon létre egy virtuális Gépet, és majd frissítése a kulcstartót a titkos kulcsot, ha az új tanúsítvány letöltése nem történik meg a virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="a76df-232">If you create a VM and then update your secret in the key vault, the new certificate is not downloaded to your VMs.</span></span> <span data-ttu-id="a76df-233">De a virtuális gépek a jelek szerint hivatkozni rá, és új virtuális gépeket az új titkos kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="a76df-233">But your VMs appear to reference it, and new VMs get the new secret.</span></span> <span data-ttu-id="a76df-234">Ennek elkerülése érdekében meg kell egy titkos verzióra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a76df-234">To avoid this, you are required to reference a secret version.</span></span>

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a><span data-ttu-id="a76df-235">A csoport több olyan tanúsítvány, a számunkra, .cer nyilvános kulcsok elosztott működik.</span><span class="sxs-lookup"><span data-stu-id="a76df-235">My team works with several certificates that are distributed to us as .cer public keys.</span></span> <span data-ttu-id="a76df-236">Mi az ajánlott módszer az ezek a tanúsítványok központi telepítéséhez a virtuálisgép-méretezési beállítása?</span><span class="sxs-lookup"><span data-stu-id="a76df-236">What is the recommended approach for deploying these certificates to a virtual machine scale set?</span></span>

<span data-ttu-id="a76df-237">.Cer telepítéséhez állítsa be a nyilvános kulcsokat a virtuálisgép-méretezési, létrehozhat egy .pfx-fájlt, amely csak a .cer fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a76df-237">To deploy .cer public keys to a virtual machine scale set, you can generate a .pfx file that contains only .cer files.</span></span> <span data-ttu-id="a76df-238">Ehhez használja `X509ContentType = Pfx`.</span><span class="sxs-lookup"><span data-stu-id="a76df-238">To do this, use `X509ContentType = Pfx`.</span></span> <span data-ttu-id="a76df-239">Például töltse be a .cer fájlt C# vagy PowerShell x509Certificate2 objektumot, és majd hívja meg a metódust.</span><span class="sxs-lookup"><span data-stu-id="a76df-239">For example, load the .cer file as an x509Certificate2 object in C# or PowerShell, and then call the method.</span></span> 

<span data-ttu-id="a76df-240">További információkért lásd: [X509Certificate.Export metódus (X509ContentType, karakterlánc)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a76df-240">For more information, see [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span></span>

### <a name="i-do-not-see-an-option-for-users-to-pass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a><span data-ttu-id="a76df-241">A beállítás a felhasználók számára, hogy a tanúsítványok base64 karakterláncként nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a76df-241">I do not see an option for users to pass in certificates as base64 strings.</span></span> <span data-ttu-id="a76df-242">A legtöbb más erőforrás-szolgáltatók kell ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="a76df-242">Most other resource providers have this option.</span></span>

<span data-ttu-id="a76df-243">Sikeres tanúsítvány Base64 kódolású karakterlánc emulációjához, kibonthatja a legújabb verzióval ellátott URL-címet a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="a76df-243">To emulate passing in a certificate as a base64 string, you can extract the latest versioned URL in a Resource Manager template.</span></span> <span data-ttu-id="a76df-244">A következő JSON-tulajdonság az erőforrás-kezelő sablonban a következők:</span><span class="sxs-lookup"><span data-stu-id="a76df-244">Include the following JSON property in your Resource Manager template:</span></span>

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a><span data-ttu-id="a76df-245">Rendelkezik a JSON-objektumok a kulcstárolók tanúsítványok burkolása?</span><span class="sxs-lookup"><span data-stu-id="a76df-245">Do I have to wrap certificates in JSON objects in key vaults?</span></span>

<span data-ttu-id="a76df-246">A virtuálisgép-méretezési csoportok és a virtuális gépek tanúsítványokat kell csomagolni, a JSON-objektumok.</span><span class="sxs-lookup"><span data-stu-id="a76df-246">In virtual machine scale sets and VMs, certificates must be wrapped in JSON objects.</span></span> 

<span data-ttu-id="a76df-247">Alkalmazás/x-pkcs12 tartalomtípus is támogatja.</span><span class="sxs-lookup"><span data-stu-id="a76df-247">We also support the content type application/x-pkcs12.</span></span> <span data-ttu-id="a76df-248">Alkalmazás/x-pkcs12 használatával, lásd: [PFX-tanúsítványokat az Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="a76df-248">For instructions on using application/x-pkcs12, see [PFX certificates in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span></span>
 
<span data-ttu-id="a76df-249">Jelenleg nem támogatjuk .cer kiterjesztésű fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a76df-249">We currently do not support .cer files.</span></span> <span data-ttu-id="a76df-250">A .cer fájl használatához exportálása .pfx tárolók őket.</span><span class="sxs-lookup"><span data-stu-id="a76df-250">To use .cer files, export them into .pfx containers.</span></span>



## <a name="compliance"></a><span data-ttu-id="a76df-251">Megfelelőség</span><span class="sxs-lookup"><span data-stu-id="a76df-251">Compliance</span></span>

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a><span data-ttu-id="a76df-252">Azok a virtuálisgép-skálázási készletekben PCI-kompatibilis?</span><span class="sxs-lookup"><span data-stu-id="a76df-252">Are virtual machine scale sets PCI-compliant?</span></span>

<span data-ttu-id="a76df-253">A virtuálisgép-méretezési csoportok egy vékony API-réteget alkotnak a KSZT tetején.</span><span class="sxs-lookup"><span data-stu-id="a76df-253">Virtual machine scale sets are a thin API layer on top of the CRP.</span></span> <span data-ttu-id="a76df-254">Mindkét összetevő a számítási platform részét képezi az Azure-szolgáltatások rendszerében.</span><span class="sxs-lookup"><span data-stu-id="a76df-254">Both components are part of the compute platform in the Azure service tree.</span></span>

<span data-ttu-id="a76df-255">A megfelelőség szempontjából nézve a virtuálisgép-méretezési csoportok az Azure számítási platformjának alapvető részét képezik.</span><span class="sxs-lookup"><span data-stu-id="a76df-255">From a compliance perspective, virtual machine scale sets are a fundamental part of the Azure compute platform.</span></span> <span data-ttu-id="a76df-256">Ezek a csoportok ugyanazt csapatot, eszközöket, folyamatokat, üzembe helyezési módszereket, biztonsági vezérlőket, igény szerinti (JIT) fordításokat, felügyeletet, riasztásokat stb. használják, mint maga a KSZT.</span><span class="sxs-lookup"><span data-stu-id="a76df-256">They share a team, tools, processes, deployment methodology, security controls, just-in-time (JIT) compilation, monitoring, alerting, and so on, with the CRP itself.</span></span> <span data-ttu-id="a76df-257">A virtuálisgép-méretezési csoportok megfelelnek a PCI-szabványnak, mert a KSZT része a jelenlegi PCI Data Security Standard (DSS) igazolásnak.</span><span class="sxs-lookup"><span data-stu-id="a76df-257">Virtual machine scale sets are Payment Card Industry (PCI)-compliant because the CRP is part of the current PCI Data Security Standard (DSS) attestation.</span></span>

<span data-ttu-id="a76df-258">További információkért lásd: [Microsoft Adatvédelmi központ](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span><span class="sxs-lookup"><span data-stu-id="a76df-258">For more information, see [the Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span></span>






## <a name="extensions"></a><span data-ttu-id="a76df-259">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="a76df-259">Extensions</span></span>

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a><span data-ttu-id="a76df-260">A virtuálisgép-méretezési készlet bővítmény törlése</span><span class="sxs-lookup"><span data-stu-id="a76df-260">How do I delete a virtual machine scale set extension?</span></span>

<span data-ttu-id="a76df-261">A virtuálisgép-méretezési készlet bővítmény törléséhez használja a következő PowerShell-példa:</span><span class="sxs-lookup"><span data-stu-id="a76df-261">To delete a virtual machine scale set extension, use the following PowerShell example:</span></span>

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
<span data-ttu-id="a76df-262">A bővítménynév érték található `$vmss`.</span><span class="sxs-lookup"><span data-stu-id="a76df-262">You can find the extensionName value in `$vmss`.</span></span>
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a><span data-ttu-id="a76df-263">Egy virtuális gép méretezési sablon példa, amely az Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="a76df-263">Is there a virtual machine scale set template example that integrates with Operations Management Suite?</span></span>

<span data-ttu-id="a76df-264">A virtuális gép méretezési sablon példa, amely az Operations Management Suite, lásd: a második példáját [Azure Service Fabric-fürt üzembe helyezése, és engedélyezze a megfigyelést Naplóelemzési használatával](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span><span class="sxs-lookup"><span data-stu-id="a76df-264">For a virtual machine scale set template example that integrates with Operations Management Suite, see the second example in [Deploy an Azure Service Fabric cluster and enable monitoring by using Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span></span>
   
### <a name="extensions-seem-to-run-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-to-fail-what-can-i-do-to-fix-this"></a><span data-ttu-id="a76df-265">Bővítmények úgy tűnik, hogy a virtuálisgép-méretezési csoportok párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="a76df-265">Extensions seem to run in parallel on virtual machine scale sets.</span></span> <span data-ttu-id="a76df-266">Ennek hatására a saját egyéni parancsprogramok futtatására szolgáló bővítmény sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="a76df-266">This causes my custom script extension to fail.</span></span> <span data-ttu-id="a76df-267">Mit tehetek a javítás?</span><span class="sxs-lookup"><span data-stu-id="a76df-267">What can I do to fix this?</span></span>

<span data-ttu-id="a76df-268">A virtuálisgép-méretezési csoportok bővítmény alkalmazás-előkészítés kapcsolatos további tudnivalókért lásd: [bővítmény alkalmazás-előkészítés az Azure virtuálisgép-méretezési csoportok](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="a76df-268">To learn about extension sequencing in virtual machine scale sets, see [Extension sequencing in Azure virtual machine scale sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span></span>
 
 
### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a76df-269">Hogyan tegye alaphelyzetbe állítani a jelszót a virtuális gépek a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="a76df-269">How do I reset the password for VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="a76df-270">A virtuálisgép-méretezési csoportban lévő virtuális gépek a jelszó alaphelyzetbe állítása, használja a virtuális gép eléréséhez kapcsolódó kiegészítő mezőket.</span><span class="sxs-lookup"><span data-stu-id="a76df-270">To reset the password for VMs in your virtual machine scale set, use VM access extensions.</span></span> 

<span data-ttu-id="a76df-271">Használja a következő PowerShell-példát:</span><span class="sxs-lookup"><span data-stu-id="a76df-271">Use the following PowerShell example:</span></span>

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
 
 
### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a76df-272">Hogyan adni a bővítmény virtuális gépeinek a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="a76df-272">How do I add an extension to all VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="a76df-273">Ha a frissítés-házirend **automatikus**, minden virtuális gép ismételt üzembe helyezéssel a sablont az új bővítmény tulajdonságai a frissíti.</span><span class="sxs-lookup"><span data-stu-id="a76df-273">If update policy is set to **automatic**, redeploying the template with the new extension properties updates all VMs.</span></span>

<span data-ttu-id="a76df-274">Ha a frissítés-házirend **manuális**, először frissítse a bővítményt, és frissítse manuálisan a virtuális gépek összes példányán.</span><span class="sxs-lookup"><span data-stu-id="a76df-274">If update policy is set to **manual**, first update the extension, and then manually update all instances in your VMs.</span></span>

  
### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-the-vms-not-match-the-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-the-scripts-that-are-currently-configured-on-the-virtual-machine-scale-set-executed-or-are-the-scripts-that-were-configured-when-the-vm-was-first-created-used"></a><span data-ttu-id="a76df-275">Ha egy meglévő virtuálisgép-méretezési csoport társított bővítmények frissítése, a meglévő virtuális gépek hatással?</span><span class="sxs-lookup"><span data-stu-id="a76df-275">If the extensions associated with an existing virtual machine scale set are updated, are existing VMs affected?</span></span> <span data-ttu-id="a76df-276">(Ez azt jelenti, hogy a rendszer a virtuális gépek *nem* felel meg a virtuális gép méretezési modellel?) Vagy azok figyelmen kívül hagy?</span><span class="sxs-lookup"><span data-stu-id="a76df-276">(That is, will the VMs *not* match the virtual machine scale set model?) Or are they ignored?</span></span> <span data-ttu-id="a76df-277">Ha egy meglévő számítógép szolgáltatás-beforrott, vagy lemezképet, azok a parancsprogramok, jelenleg be van állítva a végre virtuálisgép-méretezési csoport, vagy használja a parancsprogramok, amely be lett állítva, a virtuális gép létrehozásakor?</span><span class="sxs-lookup"><span data-stu-id="a76df-277">When an existing machine is service-healed or reimaged, are the scripts that are currently configured on the virtual machine scale set executed, or are the scripts that were configured when the VM was first created used?</span></span>

<span data-ttu-id="a76df-278">A bővítmény-definíció a virtuálisgép-méretezési, ha a modell frissül, és a upgradePolicy tulajdonság értéke **automatikus**, hogy frissíti a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a76df-278">If the extension definition in the virtual machine scale set model is updated and the upgradePolicy property is set to **automatic**, it updates the VMs.</span></span> <span data-ttu-id="a76df-279">Ha a upgradePolicy tulajdonsága **manuális**, bővítmények megjelölt a modell nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="a76df-279">If the upgradePolicy property is set to **manual**, extensions are flagged as not matching the model.</span></span> 

<span data-ttu-id="a76df-280">Ha egy meglévő virtuális gép szolgáltatás beforrott, újraindítás jelenik meg, és vannak a bővítmények nem futtatják újra.</span><span class="sxs-lookup"><span data-stu-id="a76df-280">If an existing VM is service-healed, it appears as a reboot, and the extensions are not rerun.</span></span> <span data-ttu-id="a76df-281">Ha rendszerképének, például a forrás lemezkép az operációs rendszer meghajtó lecserélését is.</span><span class="sxs-lookup"><span data-stu-id="a76df-281">If it is reimaged, it's like replacing the OS drive with the source image.</span></span> <span data-ttu-id="a76df-282">Bármely futtató és specializációt bővítmények, például a legújabb modellből futnak.</span><span class="sxs-lookup"><span data-stu-id="a76df-282">Any specialization from the latest model, such as extensions, are run.</span></span>
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-azure-ad-domain"></a><span data-ttu-id="a76df-283">Hogyan csatlakozzon a beállítása az Azure AD-tartomány virtuálisgép-méretezési?</span><span class="sxs-lookup"><span data-stu-id="a76df-283">How do I join a virtual machine scale set to an Azure AD domain?</span></span>

<span data-ttu-id="a76df-284">A virtuálisgép-méretezési beállítása az Azure Active Directory (Azure AD) tartományhoz csatlakozik egy bővítmény adhat meg.</span><span class="sxs-lookup"><span data-stu-id="a76df-284">To join a virtual machine scale set to an Azure Active Directory (Azure AD) domain, you can define an extension.</span></span> 

<span data-ttu-id="a76df-285">Egy bővítmény megadásához használja a JsonADDomainExtension tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="a76df-285">To define an extension, use the JsonADDomainExtension property:</span></span>

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
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a><span data-ttu-id="a76df-286">A virtuálisgép-méretezési készlet bővítmény próbál telepíteni, amelyet újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="a76df-286">My virtual machine scale set extension is trying to install something that requires a reboot.</span></span> <span data-ttu-id="a76df-287">Például: "commandToExecute": "powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature-Name FS-Resource-Manager – IncludeManagementTools"</span><span class="sxs-lookup"><span data-stu-id="a76df-287">For example, "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools"</span></span>

<span data-ttu-id="a76df-288">Ha a virtuálisgép-méretezési készlet bővítmény próbál telepíteni, amelyet újra kell indítani, használhatja az Azure Automation célállapot-konfiguráció (Automation DSC) bővítmény.</span><span class="sxs-lookup"><span data-stu-id="a76df-288">If your virtual machine scale set extension is trying to install something that requires a reboot, you can use the Azure Automation Desired State Configuration (Automation DSC) extension.</span></span> <span data-ttu-id="a76df-289">Ha az operációs rendszer Windows Server 2012 R2, a Azure lekéri a Windows Management Framework (WMF) 5.0 telepítő újraindul, a, és a Folytatás a konfigurációjával.</span><span class="sxs-lookup"><span data-stu-id="a76df-289">If the operating system is Windows Server 2012 R2, Azure pulls in the Windows Management Framework (WMF) 5.0 setup, reboots, and then continues with the configuration.</span></span> 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a76df-290">Hogyan bekapcsolni kártevőirtó a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="a76df-290">How do I turn on antimalware in my virtual machine scale set?</span></span>

<span data-ttu-id="a76df-291">A virtuálisgép-méretezési csoport a kártevőirtó bekapcsolásához használja a következő PowerShell-példa:</span><span class="sxs-lookup"><span data-stu-id="a76df-291">To turn on antimalware on your virtual machine scale set, use the following PowerShell example:</span></span>

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve the most recent version number of the extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-to-execute-a-custom-script-thats-hosted-in-a-private-storage-account-the-script-runs-successfully-when-the-storage-is-public-but-when-i-try-to-use-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a><span data-ttu-id="a76df-292">Üzemeltetett egyéni parancsfájl végrehajtása olyan titkos tárfiókban kell.</span><span class="sxs-lookup"><span data-stu-id="a76df-292">I need to execute a custom script that's hosted in a private storage account.</span></span> <span data-ttu-id="a76df-293">A parancsfájl sikeresen fut, ha a tároló nem nyilvános, de egy közös hozzáférésű Jogosultságkód (SAS) használata közben, az nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="a76df-293">The script runs successfully when the storage is public, but when I try to use a Shared Access Signature (SAS), it fails.</span></span> <span data-ttu-id="a76df-294">Ez az üzenet jelenik meg: "Kötelező paraméter hiányzik az érvényes a közös hozzáférésű Jogosultságkód".</span><span class="sxs-lookup"><span data-stu-id="a76df-294">This message is displayed: “Missing mandatory parameters for valid Shared Access Signature”.</span></span> <span data-ttu-id="a76df-295">Hivatkozás + SAS jól működik a helyi böngészőből.</span><span class="sxs-lookup"><span data-stu-id="a76df-295">Link+SAS works fine from my local browser.</span></span>

<span data-ttu-id="a76df-296">Személyes tárfiókokban üzemeltetett egyéni parancsfájl végrehajtása védett beállítások megadása a tárfiók hívóbetűjét, a név.</span><span class="sxs-lookup"><span data-stu-id="a76df-296">To execute a custom script that's hosted in a private storage account, set up protected settings with the storage account key and name.</span></span> <span data-ttu-id="a76df-297">További információkért lásd: [egyéni parancsfájl kiterjesztése a Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span><span class="sxs-lookup"><span data-stu-id="a76df-297">For more information, see [Custom Script Extension for Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span></span>







## <a name="networking"></a><span data-ttu-id="a76df-298">Hálózat</span><span class="sxs-lookup"><span data-stu-id="a76df-298">Networking</span></span>
 
### <a name="is-it-possible-to-assign-a-network-security-group-nsg-to-a-scale-set-so-that-it-will-apply-to-all-the-vm-nics-in-the-set"></a><span data-ttu-id="a76df-299">Ennyi az egész lehet hozzárendelni egy méretezési, a hálózati biztonsági csoport (NSG), hogy az összes virtuális gép hálózati adapterei a készlet fog alkalmazni?</span><span class="sxs-lookup"><span data-stu-id="a76df-299">Is it possible to assign a Network Security Group (NSG) to a scale set, so that it will apply to all the VM NICs in the set?</span></span>

<span data-ttu-id="a76df-300">Igen.</span><span class="sxs-lookup"><span data-stu-id="a76df-300">Yes.</span></span> <span data-ttu-id="a76df-301">Hálózati biztonsági csoport közvetlenül egy méretezési állítja be a profil networkinterfaceconfigurations tulajdonságok szakaszának hivatkoznak rá kell alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="a76df-301">A Network Security Group can be applied directly to a scale set by referencing it in the networkInterfaceConfigurations section of the network profile.</span></span> <span data-ttu-id="a76df-302">Példa:</span><span class="sxs-lookup"><span data-stu-id="a76df-302">Example:</span></span>

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

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a><span data-ttu-id="a76df-303">Hogyan egy virtuális IP-címcsere a virtuálisgép-méretezési csoportok a azonos előfizetésben és ugyanabban a régióban?</span><span class="sxs-lookup"><span data-stu-id="a76df-303">How do I do a VIP swap for virtual machine scale sets in the same subscription and same region?</span></span>

<span data-ttu-id="a76df-304">Ha két virtuálisgép-méretezési csoportok az Azure Load Balancer első-végpontok, és ugyanazt az előfizetést és régió vannak, akkor képes mindegyiknél a nyilvános IP-címek felszabadítása, és rendelje hozzá a másikra.</span><span class="sxs-lookup"><span data-stu-id="a76df-304">If you have two virtual machine scale sets with Azure Load Balancer front-ends, and they are in the same subscription and region, you could deallocate the public IP addresses from each one, and assign to the other.</span></span> <span data-ttu-id="a76df-305">Lásd: [virtuális IP-Címcsere: kék-zöld telepítése Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) például.</span><span class="sxs-lookup"><span data-stu-id="a76df-305">See [VIP Swap: Blue-green deployment in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) for example.</span></span> <span data-ttu-id="a76df-306">Ez a késleltetés utalnak, ha az erőforrások vannak, a hálózati felszabadítása/lefoglalt szinten.</span><span class="sxs-lookup"><span data-stu-id="a76df-306">This does imply a delay though as the resources are deallocated/allocated at the network level.</span></span> <span data-ttu-id="a76df-307">A gyorsabb lehetőség egy Azure Application Gateway két háttérkészlet és útválasztási szabályainak használhat.</span><span class="sxs-lookup"><span data-stu-id="a76df-307">A faster option is to use Azure Application Gateway with two backend pools, and a routing rule.</span></span> <span data-ttu-id="a76df-308">Alternatív megoldásként az alkalmazásba sikerült működteti [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) amelyek támogatást nyújt az gyors váltás átmeneti és üzemi tárhelyek között.</span><span class="sxs-lookup"><span data-stu-id="a76df-308">Alternatively, you could host your application with [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) which provides support for fast switching between staging and production slots.</span></span>
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a><span data-ttu-id="a76df-309">Hogyan adjon meg egy tartományt, magánhálózati IP-címek statikus magánhálózati IP-címek hozzárendelését használandó?</span><span class="sxs-lookup"><span data-stu-id="a76df-309">How do I specify a range of private IP addresses to use for static private IP address allocation?</span></span>

<span data-ttu-id="a76df-310">Megadott alhálózatból származó IP-címek van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="a76df-310">IP addresses are selected from a subnet that you specify.</span></span> 

<span data-ttu-id="a76df-311">A kiosztási módszer a virtuális gép méretezési készlet IP-címek mindig "dinamikus", de, hogy nem jelenti azt, hogy az IP-címeket módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a76df-311">The allocation method of virtual machine scale set IP addresses is always “dynamic,” but that doesn't mean that these IP addresses can change.</span></span> <span data-ttu-id="a76df-312">Ebben az esetben "dinamikus" csak azt jelenti, hogy nincs megadva az IP-cím a PUT kérelmek.</span><span class="sxs-lookup"><span data-stu-id="a76df-312">In this case, "dynamic" only means that you do not specify the IP address in a PUT request.</span></span> <span data-ttu-id="a76df-313">Adja meg a statikus az alhálózat-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a76df-313">Specify the static set by using the subnet.</span></span> 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a><span data-ttu-id="a76df-314">Hogyan telepíthető egy meglévő Azure virtuális hálózat beállítása virtuálisgép-méretezési?</span><span class="sxs-lookup"><span data-stu-id="a76df-314">How do I deploy a virtual machine scale set to an existing Azure virtual network?</span></span> 

<span data-ttu-id="a76df-315">Egy meglévő Azure virtuális hálózat beállítása virtuálisgép-méretezési központi telepítéséhez lásd: [központi telepítése egy virtuális gép méretezési meglévő virtuális hálózat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="a76df-315">To deploy a virtual machine scale set to an existing Azure virtual network, see [Deploy a virtual machine scale set to an existing virtual network](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span></span> 

### <a name="how-do-i-add-the-ip-address-of-the-first-vm-in-a-virtual-machine-scale-set-to-the-output-of-a-template"></a><span data-ttu-id="a76df-316">Hogyan adja hozzá az első virtuális gép IP-címét a sablon kimeneti értéke virtuálisgép-méretezési?</span><span class="sxs-lookup"><span data-stu-id="a76df-316">How do I add the IP address of the first VM in a virtual machine scale set to the output of a template?</span></span>

<span data-ttu-id="a76df-317">Adja hozzá az első virtuális gép IP-címét állítsa be a sablon kimeneti virtuálisgép-méretezési, lásd: [ARM: első VMSS privát IP-címek](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span><span class="sxs-lookup"><span data-stu-id="a76df-317">To add the IP address of the first VM in a virtual machine scale set to the output of a template, see [ARM: Get VMSS's private IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span></span>

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a><span data-ttu-id="a76df-318">Méretezési készlet használható a hálózat elérését gyorsítja fel?</span><span class="sxs-lookup"><span data-stu-id="a76df-318">Can I use scale sets with Accelerated Networking?</span></span>

<span data-ttu-id="a76df-319">Igen.</span><span class="sxs-lookup"><span data-stu-id="a76df-319">Yes.</span></span> <span data-ttu-id="a76df-320">Gyorsított hálózatkezelés használatához állítsa enableAcceleratedNetworking igaz a méretezési a networkinterfaceconfigurations tulajdonságok beállításai.</span><span class="sxs-lookup"><span data-stu-id="a76df-320">To use accelerated networking, set enableAcceleratedNetworking to true in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="a76df-321">Például</span><span class="sxs-lookup"><span data-stu-id="a76df-321">E.g.</span></span>
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

### <a name="how-can-i-configure-the-dns-servers-used-by-a-scale-set"></a><span data-ttu-id="a76df-322">Hogyan konfigurálhatja a DNS-kiszolgálókat, a méretezési készlet?</span><span class="sxs-lookup"><span data-stu-id="a76df-322">How can I configure the DNS servers used by a scale set?</span></span>

<span data-ttu-id="a76df-323">Hozzon létre egy Virtuálisgép-méretezési állítható be egy egyéni DNS-konfiguráció, vegye fel a dnsSettings JSON-csomagot a méretezési készletet networkinterfaceconfigurations tulajdonságok szakasz.</span><span class="sxs-lookup"><span data-stu-id="a76df-323">To create a VM scale set with a custom DNS configuration, add a dnsSettings JSON packet to the scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="a76df-324">Példa:</span><span class="sxs-lookup"><span data-stu-id="a76df-324">Example:</span></span>
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-to-assign-a-public-ip-address-to-each-vm"></a><span data-ttu-id="a76df-325">Hogyan konfigurálható a skála állítsa be a nyilvános IP-cím hozzárendelése az egyes virtuális gépek?</span><span class="sxs-lookup"><span data-stu-id="a76df-325">How can I configure a scale set to assign a public IP address to each VM?</span></span>

<span data-ttu-id="a76df-326">Egy Virtuálisgép-méretezési készlet, amely egy nyilvános IP-címet rendel az egyes virtuális gépek létrehozásához tegye a Microsoft.Compute/virtualMAchineScaleSets erőforrás API-verzió 2017-03-30, és adja hozzá a _publicipaddressconfiguration_ JSON-csomagot, hogy a skála beállítva IP-konfigurációk szakasz.</span><span class="sxs-lookup"><span data-stu-id="a76df-326">To create a VM scale set that assigns a public IP address to each VM, make sure the API version of the Microsoft.Compute/virtualMAchineScaleSets resource is 2017-03-30, and add a _publicipaddressconfiguration_ JSON packet to the scale set ipConfigurations section.</span></span> <span data-ttu-id="a76df-327">Példa:</span><span class="sxs-lookup"><span data-stu-id="a76df-327">Example:</span></span>

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-to-work-with-multiple-application-gateways"></a><span data-ttu-id="a76df-328">Konfigurálhatja a méretezési több alkalmazás átjáró használható készletben?</span><span class="sxs-lookup"><span data-stu-id="a76df-328">Can I configure a scale set to work with multiple Application Gateways?</span></span>

<span data-ttu-id="a76df-329">Igen.</span><span class="sxs-lookup"><span data-stu-id="a76df-329">Yes.</span></span> <span data-ttu-id="a76df-330">Az erőforrás-azonosítója a számára a több Alkalmazásátjáró háttércímkészletek is hozzáadhat a _applicationGatewayBackendAddressPools_ listájában a _IP-konfigurációk_ hálózati profil állítja a skála.</span><span class="sxs-lookup"><span data-stu-id="a76df-330">You can add the resource id's for multiple Application Gateway backend address pools to the _applicationGatewayBackendAddressPools_ list in the _ipConfigurations_ section of your scale set network profile.</span></span>

## <a name="scale"></a><span data-ttu-id="a76df-331">Méretezés</span><span class="sxs-lookup"><span data-stu-id="a76df-331">Scale</span></span>

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a><span data-ttu-id="a76df-332">Milyen esetben volna létrehozni egy virtuálisgép-méretezési beállítása kevesebb mint két virtuális gépek?</span><span class="sxs-lookup"><span data-stu-id="a76df-332">In what case would I create a virtual machine scale set with fewer than two VMs?</span></span>

<span data-ttu-id="a76df-333">Egy ok arra, hogy hozzon létre egy virtuálisgép-méretezési kevesebb mint két virtuális gépek beállítása lenne a virtuálisgép-méretezési csoport rugalmas tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a76df-333">One reason to create a virtual machine scale set with fewer than two VMs would be to use the elastic properties of a virtual machine scale set.</span></span> <span data-ttu-id="a76df-334">Például telepíthet egy virtuálisgép-méretezési állítsa nulla virtuális gépek definiálása az infrastruktúra nélkül költségek futtató virtuális fizet.</span><span class="sxs-lookup"><span data-stu-id="a76df-334">For example, you could deploy a virtual machine scale set with zero VMs to define your infrastructure without paying VM running costs.</span></span> <span data-ttu-id="a76df-335">Majd amikor készen áll a virtuális gépek telepítése, növelheti a "", a virtuálisgép-méretezési beállítása a termelési példányszámot.</span><span class="sxs-lookup"><span data-stu-id="a76df-335">Then, when you are ready to deploy VMs, increase the “capacity” of the virtual machine scale set to the production instance count.</span></span>

<span data-ttu-id="a76df-336">Előfordulhat, hogy hozzon létre egy virtuálisgép-méretezési kevesebb mint két virtuális gépek állítsa be a másik OK, ha aggódik, kevesebb, mint a rendelkezésre állási készlet különálló virtuális gépek használatával rendelkezésre állással.</span><span class="sxs-lookup"><span data-stu-id="a76df-336">Another reason you might create a virtual machine scale set with fewer than two VMs is if you're concerned less with availability than in using an availability set with discrete VMs.</span></span> <span data-ttu-id="a76df-337">Virtuálisgép-méretezési csoportok adjon meg tudja dolgozni, amelyek helyettesíthető magánháztartás számítási egység.</span><span class="sxs-lookup"><span data-stu-id="a76df-337">Virtual machine scale sets give you a way to work with undifferentiated compute units that are fungible.</span></span> <span data-ttu-id="a76df-338">Ez egységes a legfontosabb különbséget a virtuálisgép-méretezési csoportok és a rendelkezésre állási készletek.</span><span class="sxs-lookup"><span data-stu-id="a76df-338">This uniformity is a key differentiator for virtual machine scale sets versus availability sets.</span></span> <span data-ttu-id="a76df-339">Sok állapot nélküli munkaterheléseket nem követik nyomon egyedi.</span><span class="sxs-lookup"><span data-stu-id="a76df-339">Many stateless workloads do not track individual units.</span></span> <span data-ttu-id="a76df-340">Ha a terhelés esik, egy számítási egységhez csökkentheti, és majd vertikális felskálázás sok amikor növeli a munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="a76df-340">If the workload drops, you can scale down to one compute unit, and then scale up to many when the workload increases.</span></span>

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="a76df-341">Hogyan változtathatom meg a virtuálisgép-méretezési csoportban lévő virtuális gépek számát?</span><span class="sxs-lookup"><span data-stu-id="a76df-341">How do I change the number of VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="a76df-342">Ha módosítani szeretné a virtuálisgép-méretezési csoportban lévő virtuális gépek számát, lásd: [módosítása a példányok száma a virtuálisgép-méretezési csoport](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="a76df-342">To change the number of VMs in a virtual machine scale set, see [Change the instance count of a virtual machine scale set](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span></span>

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a><span data-ttu-id="a76df-343">Hogyan határozza meg az egyéni riasztások az egyes küszöbértékek elérésekor?</span><span class="sxs-lookup"><span data-stu-id="a76df-343">How do I define custom alerts for when certain thresholds are reached?</span></span>

<span data-ttu-id="a76df-344">Hogyan kezeli a megadott küszöbértékek riasztások néhány beleszólása van.</span><span class="sxs-lookup"><span data-stu-id="a76df-344">You have some flexibility in how you handle alerts for specified thresholds.</span></span> <span data-ttu-id="a76df-345">Például a testreszabott webhookok adhat meg.</span><span class="sxs-lookup"><span data-stu-id="a76df-345">For example, you can define customized webhooks.</span></span> <span data-ttu-id="a76df-346">A következő webhook példája egy Resource Manager-sablon:</span><span class="sxs-lookup"><span data-stu-id="a76df-346">The following webhook example is from a Resource Manager template:</span></span>

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

<span data-ttu-id="a76df-347">Ebben a példában egy riasztás ugrik Pagerduty.com a küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="a76df-347">In this example, an alert goes to Pagerduty.com when a threshold is reached.</span></span>



## <a name="patching-and-operations"></a><span data-ttu-id="a76df-348">Javítás és műveletek</span><span class="sxs-lookup"><span data-stu-id="a76df-348">Patching and operations</span></span>

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a><span data-ttu-id="a76df-349">Hogyan hozható létre egy meglévő erőforráscsoportot beállított terjedő skálán?</span><span class="sxs-lookup"><span data-stu-id="a76df-349">How do I create a scale set in an existing resource group?</span></span>

<span data-ttu-id="a76df-350">Méretezési csoportok létrehozása a meglévő erőforrás csoport még nem lehetséges az Azure-portálon, de megadhat egy létező erőforráscsoportot, amikor egy méretezési telepítése Azure Resource Manager-sablonok állítható be.</span><span class="sxs-lookup"><span data-stu-id="a76df-350">Creating scale sets in an existing resource group is not yet possible from the Azure portal, but you can specify an existing resource group when deploying a scale set from an Azure Resource Manager template.</span></span> <span data-ttu-id="a76df-351">A méretezési készletben, Azure PowerShell vagy a parancssori felület használatával létrehozásakor egy meglévő erőforráscsoportot is megadható.</span><span class="sxs-lookup"><span data-stu-id="a76df-351">You can also specify an existing resource group when creating a scale set using Azure PowerShell or CLI.</span></span>

### <a name="can-we-move-a-scale-set-to-another-resource-group"></a><span data-ttu-id="a76df-352">Azt áthelyezése egy másik erőforráscsoportba beállítása méretezési?</span><span class="sxs-lookup"><span data-stu-id="a76df-352">Can we move a scale set to another resource group?</span></span>

<span data-ttu-id="a76df-353">Igen, áthelyezheti méretezési erőforrások egy új előfizetés vagy az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a76df-353">Yes, you can move scale set resources to a new subscription or resource group.</span></span>

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a><span data-ttu-id="a76df-354">Hogyan való frissíteni a virtuálisgép-méretezési új lemezkép értékre?</span><span class="sxs-lookup"><span data-stu-id="a76df-354">How to I update my virtual machine scale set to a new image?</span></span> <span data-ttu-id="a76df-355">Hogyan kezelhetők a javítás?</span><span class="sxs-lookup"><span data-stu-id="a76df-355">How do I manage patching?</span></span>

<span data-ttu-id="a76df-356">A virtuálisgép-méretezési beállítása egy új lemezkép frissítése, valamint hogyan kezelhetők a javítás, lásd: [frissítéséhez egy virtuálisgép-méretezési csoport](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span><span class="sxs-lookup"><span data-stu-id="a76df-356">To update your virtual machine scale set to a new image, and to manage patching, see [Upgrade a virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span></span>

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a><span data-ttu-id="a76df-357">A lemezkép-visszaállítási művelet használatával egy virtuális gép visszaállítása a kép megváltoztatása nélkül?</span><span class="sxs-lookup"><span data-stu-id="a76df-357">Can I use the reimage operation to reset a VM without changing the image?</span></span> <span data-ttu-id="a76df-358">(Ez azt jelenti, hogy kívánt egy virtuális gép visszaállítása a gyári beállításokra, nem pedig egy új lemezképen.)</span><span class="sxs-lookup"><span data-stu-id="a76df-358">(That is, I want reset a VM to factory settings rather than to a new image.)</span></span>

<span data-ttu-id="a76df-359">Igen, a lemezkép-visszaállítási művelet használatával egy virtuális gép visszaállítása a kép megváltoztatása nélkül.</span><span class="sxs-lookup"><span data-stu-id="a76df-359">Yes, you can use the reimage operation to reset a VM without changing the image.</span></span> <span data-ttu-id="a76df-360">Azonban ha a virtuálisgép-méretezési csoport platform lemezképre hivatkozik a `version = latest`, a virtuális gép frissíthető újabb operációsrendszer-lemezképek hívásakor `reimage`.</span><span class="sxs-lookup"><span data-stu-id="a76df-360">However, if your virtual machine scale set references a platform image with `version = latest`, your VM can update to a later OS image when you call `reimage`.</span></span>

<span data-ttu-id="a76df-361">További információkért lásd: [virtuálisgép-méretezési csoportban lévő összes virtuális gépek kezeléséhez](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span><span class="sxs-lookup"><span data-stu-id="a76df-361">For more information, see [Manage all VMs in a virtual machine scale set](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="a76df-362">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="a76df-362">Troubleshooting</span></span>

### <a name="how-do-i-turn-on-boot-diagnostics"></a><span data-ttu-id="a76df-363">Hogyan rendszerindítási diagnosztika bekapcsolásához?</span><span class="sxs-lookup"><span data-stu-id="a76df-363">How do I turn on boot diagnostics?</span></span>

<span data-ttu-id="a76df-364">A rendszerindítási diagnosztika bekapcsolásához, először hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a76df-364">To turn on boot diagnostics, first, create a storage account.</span></span> <span data-ttu-id="a76df-365">Ez a JSON-tömb, majd be a virtuálisgép-méretezési csoport **virtualMachineProfile**, és a virtuálisgép-méretezési csoport frissítése:</span><span class="sxs-lookup"><span data-stu-id="a76df-365">Then, put this JSON block in your virtual machine scale set **virtualMachineProfile**, and update the virtual machine scale set:</span></span>

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

<span data-ttu-id="a76df-366">Új virtuális gép létrehozásakor a InstanceView tulajdonság a virtuális gép számára a képernyőkép, és így tovább részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a76df-366">When a new VM is created, the InstanceView property of the VM shows the details for the screenshot, and so on.</span></span> <span data-ttu-id="a76df-367">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="a76df-367">Here's an example:</span></span>
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a><span data-ttu-id="a76df-368">Virtuális gép tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="a76df-368">Virtual machine properties</span></span>

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a76df-369">Hogyan szerezhetek tulajdonság adatait az egyes virtuális gépek több hívás nélkül?</span><span class="sxs-lookup"><span data-stu-id="a76df-369">How do I get property information for each VM without making multiple calls?</span></span> <span data-ttu-id="a76df-370">Például hogyan kellene jelenik meg a tartalék tartomány 100 virtuális gépek mindegyikének a virtuálisgép-méretezési csoportban lévő?</span><span class="sxs-lookup"><span data-stu-id="a76df-370">For example, how would I get the fault domain for each of the 100 VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="a76df-371">Ahhoz, hogy az egyes virtuális gépek a tulajdonság adatait anélkül, hogy több hívást, hívása `ListVMInstanceViews` egy REST API végrehajtásával `GET` a következő alaposztály:</span><span class="sxs-lookup"><span data-stu-id="a76df-371">To get property information for each VM without making multiple calls, you can call `ListVMInstanceViews` by doing a REST API `GET` on the following resource URI:</span></span>

<span data-ttu-id="a76df-372">/Subscriptions/ < ELŐFIZETÉS_AZONOSÍTÓJA > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtuális gépek vannak? $expand argumentum = instanceView & $select = instanceView</span><span class="sxs-lookup"><span data-stu-id="a76df-372">/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView</span></span>

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="a76df-373">I argumentumok adhatók át másik bővítményt egy virtuálisgép-méretezési csoportban lévő különböző virtuális gépek?</span><span class="sxs-lookup"><span data-stu-id="a76df-373">Can I pass different extension arguments to different VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="a76df-374">Nem, a virtuálisgép-méretezési csoportban lévő különböző virtuális gépek különböző argumentumok nem adható át.</span><span class="sxs-lookup"><span data-stu-id="a76df-374">No, you cannot pass different extension arguments to different VMs in a virtual machine scale set.</span></span> <span data-ttu-id="a76df-375">Azonban bővítmények működhet-e azok futnak, például a gép nevét a virtuális gép egyedi tulajdonságai alapján.</span><span class="sxs-lookup"><span data-stu-id="a76df-375">However, extensions can act based on the unique properties of the VM they are running on, such as on the machine name.</span></span> <span data-ttu-id="a76df-376">Bővítmények is lekérdezheti a példány metaadatainak az http://169.254.169.254 a virtuális gép kapcsolatban további információkat.</span><span class="sxs-lookup"><span data-stu-id="a76df-376">Extensions also can query instance metadata on http://169.254.169.254 to get more information about the VM.</span></span>

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a><span data-ttu-id="a76df-377">Miért van a virtuális gép méretezési VM számítógép nevének és a virtuális gép azonosítók között lévő hézagokat?</span><span class="sxs-lookup"><span data-stu-id="a76df-377">Why are there gaps between my virtual machine scale set VM machine names and VM IDs?</span></span> <span data-ttu-id="a76df-378">Például: 0, 1, 3...</span><span class="sxs-lookup"><span data-stu-id="a76df-378">For example: 0, 1, 3...</span></span>

<span data-ttu-id="a76df-379">Nincsenek a virtuális gép méretezési VM számítógép nevének és a virtuális gép azonosítók között lévő hézagokat, mert a virtuálisgép-méretezési csoport **szükségesnél több erőforrás** tulajdonság értéke az alapértelmezett érték **igaz**.</span><span class="sxs-lookup"><span data-stu-id="a76df-379">There are gaps between your virtual machine scale set VM machine names and VM IDs because your virtual machine scale set **overprovision** property is set to the default value of **true**.</span></span> <span data-ttu-id="a76df-380">Ha elhelyezésétől **igaz**, mint a kért jönnek létre további virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="a76df-380">If overprovisioning is set to **true**, more VMs than requested are created.</span></span> <span data-ttu-id="a76df-381">Extra virtuális Gépekért törlődnek.</span><span class="sxs-lookup"><span data-stu-id="a76df-381">Extra VMs are then deleted.</span></span> <span data-ttu-id="a76df-382">Ebben az esetben azt szerezhet telepítés megbízhatóságát, azonban rovására összefüggő elnevezési és összefüggő hálózati címfordítás (NAT) szabályok.</span><span class="sxs-lookup"><span data-stu-id="a76df-382">In this case, you gain increased deployment reliability, but at the expense of contiguous naming and contiguous Network Address Translation (NAT) rules.</span></span> 

<span data-ttu-id="a76df-383">Állíthatja ezt a tulajdonságot **hamis**.</span><span class="sxs-lookup"><span data-stu-id="a76df-383">You can set this property to **false**.</span></span> <span data-ttu-id="a76df-384">Kis virtuálisgép-méretezési csoportok, a központi telepítés megbízhatóság jelentősen ez nincs hatással.</span><span class="sxs-lookup"><span data-stu-id="a76df-384">For small virtual machine scale sets, this doesn't significantly affect deployment reliability.</span></span>

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a><span data-ttu-id="a76df-385">Mi az a különbség egy virtuális Gépet egy virtuálisgép-méretezési csoportban lévő törlése és a virtuális gép felszabadítása?</span><span class="sxs-lookup"><span data-stu-id="a76df-385">What is the difference between deleting a VM in a virtual machine scale set and deallocating the VM?</span></span> <span data-ttu-id="a76df-386">Ha válasszam közül történő?</span><span class="sxs-lookup"><span data-stu-id="a76df-386">When should I choose one over the other?</span></span>

<span data-ttu-id="a76df-387">A virtuálisgép-méretezési csoportban lévő virtuális gép törlése, és a virtuális gép felszabadítása közötti fő különbség, hogy `deallocate` nem törli a virtuális merevlemezek (VHD-k).</span><span class="sxs-lookup"><span data-stu-id="a76df-387">The main difference between deleting a VM in a virtual machine scale set and deallocating the VM is that `deallocate` doesn’t delete the virtual hard disks (VHDs).</span></span> <span data-ttu-id="a76df-388">Nincsenek futó társított tárolási költségek `stop deallocate`.</span><span class="sxs-lookup"><span data-stu-id="a76df-388">There are storage costs associated with running `stop deallocate`.</span></span> <span data-ttu-id="a76df-389">Az egyiket a következő okok valamelyike használhatja:</span><span class="sxs-lookup"><span data-stu-id="a76df-389">You might use one or the other for one of the following reasons:</span></span>

- <span data-ttu-id="a76df-390">Le kívánja állítani a számítási költségek fizet, de megtartja a virtuális gépek állapotot.</span><span class="sxs-lookup"><span data-stu-id="a76df-390">You want to stop paying compute costs, but you want to keep the disk state of the VMs.</span></span>
- <span data-ttu-id="a76df-391">Indítsa el a virtuális gépek halmaza gyorsabban volt a virtuálisgép-méretezési csoport horizontális szeretné.</span><span class="sxs-lookup"><span data-stu-id="a76df-391">You want to start a set of VMs more quickly than you could scale out a virtual machine scale set.</span></span>
  - <span data-ttu-id="a76df-392">A forgatókönyvhöz kapcsolódó, előfordulhat, hogy létrehozta a saját automatikus skálázás-motor és a kívánt gyorsabb végpontok közötti skálán.</span><span class="sxs-lookup"><span data-stu-id="a76df-392">Related to this scenario, you might have created your own autoscale engine and want a faster end-to-end scale.</span></span>
- <span data-ttu-id="a76df-393">A virtuálisgép-méretezési csoport, amely nem egyenlően elosztva tartalék tartományok vagy a frissítési tartományok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a76df-393">You have a virtual machine scale set that is unevenly distributed across fault domains or update domains.</span></span> <span data-ttu-id="a76df-394">Ez lehet, mert szelektív módon törli a virtuális gépek vagy virtuális gépek elhelyezésétől után törölték.</span><span class="sxs-lookup"><span data-stu-id="a76df-394">This might be because you selectively deleted VMs, or because VMs were deleted after overprovisioning.</span></span> <span data-ttu-id="a76df-395">Futó `stop deallocate` követ `start` a virtuális gépen méretezési készletben egyenlően osztja el a virtuális gépek között tartalék tartományok vagy a frissítési tartományok.</span><span class="sxs-lookup"><span data-stu-id="a76df-395">Running `stop deallocate` followed by `start` on the virtual machine scale set evenly distributes the VMs across fault domains or update domains.</span></span>

