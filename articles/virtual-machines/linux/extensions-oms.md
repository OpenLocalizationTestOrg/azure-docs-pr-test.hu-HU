---
title: "Linux virtuális gép az Azure kiterjesztése aaaOMS |} Microsoft Docs"
description: "A Linux virtuális gépet egy virtuálisgép-bővítmény használatával hello OMS-ügynököt telepíteni."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 0fc8003d1fae6c043eef18ae78d12f9304b70832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a><span data-ttu-id="63f6a-103">Linux OMS virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="63f6a-103">OMS virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="63f6a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="63f6a-104">Overview</span></span>

<span data-ttu-id="63f6a-105">Az Operations Management Suite (OMS) figyelési riasztási és riasztási szervizelési képességeket biztosít a felhő között és a helyszíni eszközök.</span><span class="sxs-lookup"><span data-stu-id="63f6a-105">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="63f6a-106">hello OMS-ügynököt a virtuálisgép-bővítmény Linux közzétett és a Microsoft támogatja.</span><span class="sxs-lookup"><span data-stu-id="63f6a-106">hello OMS Agent virtual machine extension for Linux is published and supported by Microsoft.</span></span> <span data-ttu-id="63f6a-107">hello bővítmény hello OMS-ügynököt telepít Azure virtuális gépeken, és regisztrálja a virtuális gépek be egy meglévő OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="63f6a-107">hello extension installs hello OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="63f6a-108">Ez a dokumentum részletek hello támogatott platformokat, a konfigurációk és a központi telepítési beállítások hello OMS virtuálisgép-bővítmény Linux.</span><span class="sxs-lookup"><span data-stu-id="63f6a-108">This document details hello supported platforms, configurations, and deployment options for hello OMS virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63f6a-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63f6a-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="63f6a-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="63f6a-110">Operating system</span></span>

<span data-ttu-id="63f6a-111">hello OMS-ügynököt bővítmény futtatható a Linux terjesztéseket ellen.</span><span class="sxs-lookup"><span data-stu-id="63f6a-111">hello OMS Agent extension can be run against these Linux distributions.</span></span>

| <span data-ttu-id="63f6a-112">Disztribúció</span><span class="sxs-lookup"><span data-stu-id="63f6a-112">Distribution</span></span> | <span data-ttu-id="63f6a-113">Verzió</span><span class="sxs-lookup"><span data-stu-id="63f6a-113">Version</span></span> |
|---|---|
| <span data-ttu-id="63f6a-114">CentOS Linux</span><span class="sxs-lookup"><span data-stu-id="63f6a-114">CentOS Linux</span></span> | <span data-ttu-id="63f6a-115">5, 6 és 7</span><span class="sxs-lookup"><span data-stu-id="63f6a-115">5, 6, and 7</span></span> |
| <span data-ttu-id="63f6a-116">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="63f6a-116">Oracle Linux</span></span> | <span data-ttu-id="63f6a-117">5, 6 és 7</span><span class="sxs-lookup"><span data-stu-id="63f6a-117">5, 6, and 7</span></span> |
| <span data-ttu-id="63f6a-118">Red Hat Enterprise Linux Server</span><span class="sxs-lookup"><span data-stu-id="63f6a-118">Red Hat Enterprise Linux Server</span></span> | <span data-ttu-id="63f6a-119">5, 6, 7</span><span class="sxs-lookup"><span data-stu-id="63f6a-119">5, 6 and 7</span></span> |
| <span data-ttu-id="63f6a-120">Debian GNU/Linux</span><span class="sxs-lookup"><span data-stu-id="63f6a-120">Debian GNU/Linux</span></span> | <span data-ttu-id="63f6a-121">8, 6, 7</span><span class="sxs-lookup"><span data-stu-id="63f6a-121">6, 7, and 8</span></span> |
| <span data-ttu-id="63f6a-122">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="63f6a-122">Ubuntu</span></span> | <span data-ttu-id="63f6a-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="63f6a-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span></span> |
| <span data-ttu-id="63f6a-124">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="63f6a-124">SUSE Linux Enterprise Server</span></span> | <span data-ttu-id="63f6a-125">11 és 12</span><span class="sxs-lookup"><span data-stu-id="63f6a-125">11 and 12</span></span> |

### <a name="internet-connectivity"></a><span data-ttu-id="63f6a-126">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="63f6a-126">Internet connectivity</span></span>

<span data-ttu-id="63f6a-127">hello Linux kiterjesztése OMS-ügynököt kell lennie, hogy hello a cél virtuális gép csatlakoztatott toohello internet.</span><span class="sxs-lookup"><span data-stu-id="63f6a-127">hello OMS Agent extension for Linux requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="63f6a-128">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="63f6a-128">Extension schema</span></span>

<span data-ttu-id="63f6a-129">hello következő JSON látható hello OMS-ügynököt bővítmény hello sémáját.</span><span class="sxs-lookup"><span data-stu-id="63f6a-129">hello following JSON shows hello schema for hello OMS Agent extension.</span></span> <span data-ttu-id="63f6a-130">hello bővítmény hello munkaterület azonosítója és munkaterület hello cél OMS-munkaterület; a kulcs szükséges. Ezeket az értékeket az hello OMS-portálon található.</span><span class="sxs-lookup"><span data-stu-id="63f6a-130">hello extension requires hello workspace ID and workspace key from hello target OMS workspace; these values can be found in hello OMS portal.</span></span> <span data-ttu-id="63f6a-131">Hello munkaterületkulcsot bizalmas adatokat kell kezelni, mert azt egy védett beállítás konfigurációban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="63f6a-131">Because hello workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="63f6a-132">Azure virtuális gépekre vonatkozó beállításával bővítmény védett adatok titkosítva, és csak visszafejteni hello cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="63f6a-132">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span> <span data-ttu-id="63f6a-133">Vegye figyelembe, hogy **workspaceId** és **workspaceKey** -és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="63f6a-133">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a><span data-ttu-id="63f6a-134">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="63f6a-134">Property values</span></span>

| <span data-ttu-id="63f6a-135">Név</span><span class="sxs-lookup"><span data-stu-id="63f6a-135">Name</span></span> | <span data-ttu-id="63f6a-136">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="63f6a-136">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="63f6a-137">apiVersion</span><span class="sxs-lookup"><span data-stu-id="63f6a-137">apiVersion</span></span> | <span data-ttu-id="63f6a-138">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="63f6a-138">2015-06-15</span></span> |
| <span data-ttu-id="63f6a-139">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="63f6a-139">publisher</span></span> | <span data-ttu-id="63f6a-140">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="63f6a-140">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="63f6a-141">type</span><span class="sxs-lookup"><span data-stu-id="63f6a-141">type</span></span> | <span data-ttu-id="63f6a-142">OmsAgentForLinux</span><span class="sxs-lookup"><span data-stu-id="63f6a-142">OmsAgentForLinux</span></span> |
| <span data-ttu-id="63f6a-143">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="63f6a-143">typeHandlerVersion</span></span> | <span data-ttu-id="63f6a-144">1.4</span><span class="sxs-lookup"><span data-stu-id="63f6a-144">1.4</span></span> |
| <span data-ttu-id="63f6a-145">workspaceId (például)</span><span class="sxs-lookup"><span data-stu-id="63f6a-145">workspaceId (e.g)</span></span> | <span data-ttu-id="63f6a-146">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="63f6a-146">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="63f6a-147">workspaceKey (például)</span><span class="sxs-lookup"><span data-stu-id="63f6a-147">workspaceKey (e.g)</span></span> | <span data-ttu-id="63f6a-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="63f6a-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="63f6a-149">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="63f6a-149">Template deployment</span></span>

<span data-ttu-id="63f6a-150">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="63f6a-150">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="63f6a-151">Sablonok ideálisak, például a bevezetési tooOMS feladás egy vagy több központi telepítési beállításokra van szükség egy vagy több virtuális gépek telepítése során.</span><span class="sxs-lookup"><span data-stu-id="63f6a-151">Templates are ideal when deploying one or more virtual machines that require post deployment configuration such as onboarding tooOMS.</span></span> <span data-ttu-id="63f6a-152">Egy minta Resource Manager-sablon, amely tartalmazza az OMS-ügynök Virtuálisgép-bővítmény hello hello található [Azure Quick Start gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span><span class="sxs-lookup"><span data-stu-id="63f6a-152">A sample Resource Manager template that includes hello OMS Agent VM extension can be found on hello [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span></span> 

<span data-ttu-id="63f6a-153">egy virtuálisgép-bővítmény hello JSON konfigurációja ágyazott hello virtuálisgép-erőforrást, vagy hello gyökér- vagy legfelső szintű erőforrás-kezelő JSON-sablon elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="63f6a-153">hello JSON configuration for a virtual machine extension can be nested inside hello virtual machine resource, or placed at hello root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="63f6a-154">hello JSON konfigurációs hello elhelyezését hello értéket hello erőforrás neve és típusa befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="63f6a-154">hello placement of hello JSON configuration affects hello value of hello resource name and type.</span></span> <span data-ttu-id="63f6a-155">További információkért lásd: [nevét és típusát gyermekerőforrásait beállítása](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="63f6a-155">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="63f6a-156">hello alábbi példa azt feltételezi, hogy hello OMS bővítmény hello virtuálisgép-erőforrás van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="63f6a-156">hello following example assumes hello OMS extension is nested inside hello virtual machine resource.</span></span> <span data-ttu-id="63f6a-157">Ha hello bővítmény erőforrás beágyazási, hello JSON hello kerül `"resources": []` objektum hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="63f6a-157">When nesting hello extension resource, hello JSON is placed in hello `"resources": []` object of hello virtual machine.</span></span>

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

<span data-ttu-id="63f6a-158">Hello bővítmény JSON hello gyökerében hello sablon elhelyezésekor hello erőforrás neve tartalmaz egy hivatkozást toohello szülő virtuális gép, és hello típus hello beágyazott konfigurációs tükrözi.</span><span class="sxs-lookup"><span data-stu-id="63f6a-158">When placing hello extension JSON at hello root of hello template, hello resource name includes a reference toohello parent virtual machine, and hello type reflects hello nested configuration.</span></span>  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a><span data-ttu-id="63f6a-159">Az Azure CLI-telepítés</span><span class="sxs-lookup"><span data-stu-id="63f6a-159">Azure CLI deployment</span></span>

<span data-ttu-id="63f6a-160">hello Azure CLI használt toodeploy hello OMS-ügynök VM bővítmény tooan meglévő virtuális gép is lehet.</span><span class="sxs-lookup"><span data-stu-id="63f6a-160">hello Azure CLI can be used toodeploy hello OMS Agent VM extension tooan existing virtual machine.</span></span> <span data-ttu-id="63f6a-161">Cserélje le a hello OMS kulcs és az OMS-azonosító az OMS-munkaterület együtt.</span><span class="sxs-lookup"><span data-stu-id="63f6a-161">Replace hello OMS key and OMS ID with those from your OMS workspace.</span></span> 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="63f6a-162">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="63f6a-162">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="63f6a-163">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="63f6a-163">Troubleshoot</span></span>

<span data-ttu-id="63f6a-164">A bővítmény központi telepítések hello állapotával kapcsolatos információkat lehet adatokat beolvasni a hello Azure-portálon, és hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="63f6a-164">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure CLI.</span></span> <span data-ttu-id="63f6a-165">egy adott virtuális Gépet, futtassa a következő parancs használatával hello kiterjesztéseinek toosee hello telepítési állapota hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="63f6a-165">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure CLI.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="63f6a-166">Bővítmény végrehajtási kimeneti van naplózott toohello következő fájl:</span><span class="sxs-lookup"><span data-stu-id="63f6a-166">Extension execution output is logged toohello following file:</span></span>

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a><span data-ttu-id="63f6a-167">Hibakódok és azok jelentését</span><span class="sxs-lookup"><span data-stu-id="63f6a-167">Error codes and their meanings</span></span>

| <span data-ttu-id="63f6a-168">Hibakód:</span><span class="sxs-lookup"><span data-stu-id="63f6a-168">Error Code</span></span> | <span data-ttu-id="63f6a-169">Jelentése</span><span class="sxs-lookup"><span data-stu-id="63f6a-169">Meaning</span></span> | <span data-ttu-id="63f6a-170">Lehetséges művelet</span><span class="sxs-lookup"><span data-stu-id="63f6a-170">Possible Action</span></span> |
| :---: | --- | --- |
| <span data-ttu-id="63f6a-171">10</span><span class="sxs-lookup"><span data-stu-id="63f6a-171">10</span></span> | <span data-ttu-id="63f6a-172">Virtuális gép már csatlakoztatott tooan OMS-munkaterület</span><span class="sxs-lookup"><span data-stu-id="63f6a-172">VM is already connected tooan OMS workspace</span></span> | <span data-ttu-id="63f6a-173">tooconnect hello VM toohello munkaterület hello bővítmény sémában megadott stopOnMultipleConnections toofalse állítsa nyilvános beállításai, vagy távolítsa el ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="63f6a-173">tooconnect hello VM toohello workspace specified in hello extension schema, set stopOnMultipleConnections toofalse in public settings or remove this property.</span></span> <span data-ttu-id="63f6a-174">Ez a virtuális gép lekérdezi számlázva után az egyes munkaterületeken van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="63f6a-174">This VM gets billed once for each workspace it is connected to.</span></span> |
| <span data-ttu-id="63f6a-175">11</span><span class="sxs-lookup"><span data-stu-id="63f6a-175">11</span></span> | <span data-ttu-id="63f6a-176">Érvénytelen a megadott konfigurációs toohello bővítmény</span><span class="sxs-lookup"><span data-stu-id="63f6a-176">Invalid config provided toohello extension</span></span> | <span data-ttu-id="63f6a-177">Kövesse az előző példák tooset a telepítéshez szükséges minden tulajdonság értékével hello.</span><span class="sxs-lookup"><span data-stu-id="63f6a-177">Follow hello preceding examples tooset all property values necessary for deployment.</span></span> |
| <span data-ttu-id="63f6a-178">12</span><span class="sxs-lookup"><span data-stu-id="63f6a-178">12</span></span> | <span data-ttu-id="63f6a-179">hello dpkg Csomagkezelő zárolva van</span><span class="sxs-lookup"><span data-stu-id="63f6a-179">hello dpkg package manager is locked</span></span> | <span data-ttu-id="63f6a-180">Győződjön meg arról, hogy minden dpkg frissítési műveletek hello gépen végzett, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="63f6a-180">Make sure all dpkg update operations on hello machine have finished and retry.</span></span> |
| <span data-ttu-id="63f6a-181">20</span><span class="sxs-lookup"><span data-stu-id="63f6a-181">20</span></span> | <span data-ttu-id="63f6a-182">Túl korán nevű engedélyezése</span><span class="sxs-lookup"><span data-stu-id="63f6a-182">Enable called prematurely</span></span> | <span data-ttu-id="63f6a-183">[Frissítés hello Azure Linux ügynök](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello elérhető legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="63f6a-183">[Update hello Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello latest available version.</span></span> |
| <span data-ttu-id="63f6a-184">51</span><span class="sxs-lookup"><span data-stu-id="63f6a-184">51</span></span> | <span data-ttu-id="63f6a-185">A bővítmény nem támogatott a hello virtuális gép operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="63f6a-185">This extension is not supported on hello VM's operation system</span></span> | |
| <span data-ttu-id="63f6a-186">55</span><span class="sxs-lookup"><span data-stu-id="63f6a-186">55</span></span> | <span data-ttu-id="63f6a-187">Nem lehet kapcsolódni a Microsoft Operations Management Suite szolgáltatás toohello</span><span class="sxs-lookup"><span data-stu-id="63f6a-187">Cannot connect toohello Microsoft Operations Management Suite service</span></span> | <span data-ttu-id="63f6a-188">Ellenőrizze, hogy hello rendszernek van-e Internet-hozzáféréssel, vagy adtak meg, hogy érvényes HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="63f6a-188">Check that hello system either has Internet access, or that a valid HTTP proxy has been provided.</span></span> <span data-ttu-id="63f6a-189">Ezenkívül ellenőrizze a helyességét hello hello munkaterület azonosítója.</span><span class="sxs-lookup"><span data-stu-id="63f6a-189">Additionally, check hello correctness of hello workspace ID.</span></span> |

<span data-ttu-id="63f6a-190">További hibaelhárítási információért hello található [OMS-ügynök-az-Linux hibaelhárítási útmutatója](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span><span class="sxs-lookup"><span data-stu-id="63f6a-190">Additional troubleshooting information can be found on hello [OMS-Agent-for-Linux Troubleshooting Guide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span></span>

### <a name="support"></a><span data-ttu-id="63f6a-191">Támogatás</span><span class="sxs-lookup"><span data-stu-id="63f6a-191">Support</span></span>

<span data-ttu-id="63f6a-192">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure szakértői hello hello [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="63f6a-192">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="63f6a-193">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="63f6a-193">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="63f6a-194">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatást.</span><span class="sxs-lookup"><span data-stu-id="63f6a-194">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="63f6a-195">Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="63f6a-195">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
