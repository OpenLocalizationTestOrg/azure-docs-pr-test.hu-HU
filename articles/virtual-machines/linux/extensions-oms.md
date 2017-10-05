---
title: "Virtuálisgép-bővítmény OMS Azure Linux |} Microsoft Docs"
description: "A Linux virtuális gépet egy virtuálisgép-bővítmény használatával az OMS-ügynök telepítése."
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
ms.openlocfilehash: 138fc8c98ea6f409b28407b20851c96ecc618b09
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a><span data-ttu-id="252ae-103">Linux OMS virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="252ae-103">OMS virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="252ae-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="252ae-104">Overview</span></span>

<span data-ttu-id="252ae-105">Az Operations Management Suite (OMS) figyelési riasztási és riasztási szervizelési képességeket biztosít a felhő között és a helyszíni eszközök.</span><span class="sxs-lookup"><span data-stu-id="252ae-105">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="252ae-106">Az OMS-ügynököt Linux virtuálisgép-bővítmény közzétett és a Microsoft támogatja.</span><span class="sxs-lookup"><span data-stu-id="252ae-106">The OMS Agent virtual machine extension for Linux is published and supported by Microsoft.</span></span> <span data-ttu-id="252ae-107">A bővítmény, az OMS-ügynököt telepít Azure virtuális gépeken, és regisztrálja a virtuális gépek be egy meglévő OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="252ae-107">The extension installs the OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="252ae-108">Ez a dokumentum részletesen a támogatott platformok, a konfigurációk és a Linux virtuálisgép-bővítmény OMS vonatkozó telepítési lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="252ae-108">This document details the supported platforms, configurations, and deployment options for the OMS virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="252ae-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="252ae-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="252ae-110">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="252ae-110">Operating system</span></span>

<span data-ttu-id="252ae-111">Az OMS-ügynököt bővítmény futtatható a Linux terjesztéseket ellen.</span><span class="sxs-lookup"><span data-stu-id="252ae-111">The OMS Agent extension can be run against these Linux distributions.</span></span>

| <span data-ttu-id="252ae-112">Disztribúció</span><span class="sxs-lookup"><span data-stu-id="252ae-112">Distribution</span></span> | <span data-ttu-id="252ae-113">Verzió</span><span class="sxs-lookup"><span data-stu-id="252ae-113">Version</span></span> |
|---|---|
| <span data-ttu-id="252ae-114">CentOS Linux</span><span class="sxs-lookup"><span data-stu-id="252ae-114">CentOS Linux</span></span> | <span data-ttu-id="252ae-115">5, 6 és 7</span><span class="sxs-lookup"><span data-stu-id="252ae-115">5, 6, and 7</span></span> |
| <span data-ttu-id="252ae-116">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="252ae-116">Oracle Linux</span></span> | <span data-ttu-id="252ae-117">5, 6 és 7</span><span class="sxs-lookup"><span data-stu-id="252ae-117">5, 6, and 7</span></span> |
| <span data-ttu-id="252ae-118">Red Hat Enterprise Linux Server</span><span class="sxs-lookup"><span data-stu-id="252ae-118">Red Hat Enterprise Linux Server</span></span> | <span data-ttu-id="252ae-119">5, 6, 7</span><span class="sxs-lookup"><span data-stu-id="252ae-119">5, 6 and 7</span></span> |
| <span data-ttu-id="252ae-120">Debian GNU/Linux</span><span class="sxs-lookup"><span data-stu-id="252ae-120">Debian GNU/Linux</span></span> | <span data-ttu-id="252ae-121">8, 6, 7</span><span class="sxs-lookup"><span data-stu-id="252ae-121">6, 7, and 8</span></span> |
| <span data-ttu-id="252ae-122">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="252ae-122">Ubuntu</span></span> | <span data-ttu-id="252ae-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="252ae-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span></span> |
| <span data-ttu-id="252ae-124">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="252ae-124">SUSE Linux Enterprise Server</span></span> | <span data-ttu-id="252ae-125">11 és 12</span><span class="sxs-lookup"><span data-stu-id="252ae-125">11 and 12</span></span> |

### <a name="internet-connectivity"></a><span data-ttu-id="252ae-126">Internetkapcsolat</span><span class="sxs-lookup"><span data-stu-id="252ae-126">Internet connectivity</span></span>

<span data-ttu-id="252ae-127">Az OMS-ügynököt bővítmény Linux megköveteli, hogy a cél virtuális gép csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="252ae-127">The OMS Agent extension for Linux requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="252ae-128">A séma kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="252ae-128">Extension schema</span></span>

<span data-ttu-id="252ae-129">A következő JSON jeleníti meg az OMS-ügynököt bővítmény sémáját.</span><span class="sxs-lookup"><span data-stu-id="252ae-129">The following JSON shows the schema for the OMS Agent extension.</span></span> <span data-ttu-id="252ae-130">A bővítmény szükséges a munkaterület azonosítója és a cél OMS-munkaterület; kulcsát Ezeket az értékeket az OMS-portálon található.</span><span class="sxs-lookup"><span data-stu-id="252ae-130">The extension requires the workspace ID and workspace key from the target OMS workspace; these values can be found in the OMS portal.</span></span> <span data-ttu-id="252ae-131">A munkaterület-kulcs bizalmas adatokat kell kezelni, mert azt egy védett beállítás konfigurációban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="252ae-131">Because the workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="252ae-132">Az Azure Virtuálisgép-bővítmény védett beállítás adatokat titkosít, és csak visszafejti a cél virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="252ae-132">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span> <span data-ttu-id="252ae-133">Vegye figyelembe, hogy **workspaceId** és **workspaceKey** -és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="252ae-133">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="252ae-134">A tulajdonság értékek</span><span class="sxs-lookup"><span data-stu-id="252ae-134">Property values</span></span>

| <span data-ttu-id="252ae-135">Név</span><span class="sxs-lookup"><span data-stu-id="252ae-135">Name</span></span> | <span data-ttu-id="252ae-136">Érték / – példa</span><span class="sxs-lookup"><span data-stu-id="252ae-136">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="252ae-137">apiVersion</span><span class="sxs-lookup"><span data-stu-id="252ae-137">apiVersion</span></span> | <span data-ttu-id="252ae-138">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="252ae-138">2015-06-15</span></span> |
| <span data-ttu-id="252ae-139">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="252ae-139">publisher</span></span> | <span data-ttu-id="252ae-140">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="252ae-140">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="252ae-141">type</span><span class="sxs-lookup"><span data-stu-id="252ae-141">type</span></span> | <span data-ttu-id="252ae-142">OmsAgentForLinux</span><span class="sxs-lookup"><span data-stu-id="252ae-142">OmsAgentForLinux</span></span> |
| <span data-ttu-id="252ae-143">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="252ae-143">typeHandlerVersion</span></span> | <span data-ttu-id="252ae-144">1.4</span><span class="sxs-lookup"><span data-stu-id="252ae-144">1.4</span></span> |
| <span data-ttu-id="252ae-145">workspaceId (például)</span><span class="sxs-lookup"><span data-stu-id="252ae-145">workspaceId (e.g)</span></span> | <span data-ttu-id="252ae-146">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="252ae-146">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="252ae-147">workspaceKey (például)</span><span class="sxs-lookup"><span data-stu-id="252ae-147">workspaceKey (e.g)</span></span> | <span data-ttu-id="252ae-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="252ae-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="252ae-149">Sablonalapú telepítés</span><span class="sxs-lookup"><span data-stu-id="252ae-149">Template deployment</span></span>

<span data-ttu-id="252ae-150">Az Azure Virtuálisgép-bővítmények az Azure Resource Manager-sablonok is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="252ae-150">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="252ae-151">Sablonok bevezetése az OMS-be például a feladás egy vagy több központi telepítési beállításokra van szükség egy vagy több virtuális gépek telepítése során ideális.</span><span class="sxs-lookup"><span data-stu-id="252ae-151">Templates are ideal when deploying one or more virtual machines that require post deployment configuration such as onboarding to OMS.</span></span> <span data-ttu-id="252ae-152">Az OMS-ügynök Virtuálisgép-bővítmény tartalmazó minta Resource Manager sablon megtalálható a [Azure Quick Start gyűjtemény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span><span class="sxs-lookup"><span data-stu-id="252ae-152">A sample Resource Manager template that includes the OMS Agent VM extension can be found on the [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span></span> 

<span data-ttu-id="252ae-153">A JSON-konfiguráció a virtuálisgép-bővítmény a virtuálisgép-erőforrás ágyazhatók egymásba, vagy elhelyezve, a gyökér vagy a legfelső szintű erőforrás-kezelő JSON-sablon.</span><span class="sxs-lookup"><span data-stu-id="252ae-153">The JSON configuration for a virtual machine extension can be nested inside the virtual machine resource, or placed at the root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="252ae-154">A JSON-konfiguráció elhelyezési erőforrás nevét és típusát. értéke befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="252ae-154">The placement of the JSON configuration affects the value of the resource name and type.</span></span> <span data-ttu-id="252ae-155">További információkért lásd: [nevét és típusát gyermekerőforrásait beállítása](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="252ae-155">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="252ae-156">Az alábbi példa azt feltételezi, hogy a virtuálisgép-erőforrást az OMS-bővítmény van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="252ae-156">The following example assumes the OMS extension is nested inside the virtual machine resource.</span></span> <span data-ttu-id="252ae-157">A bővítmény erőforrás beágyazási, amikor bekerül a JSON a `"resources": []` objektum a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="252ae-157">When nesting the extension resource, the JSON is placed in the `"resources": []` object of the virtual machine.</span></span>

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

<span data-ttu-id="252ae-158">A bővítmény JSON elhelyezésekor a sablon gyökerében, az erőforrás nevét a szülő virtuális gép egy hivatkozást tartalmaz, és a típus beágyazott konfigurációját tükrözi.</span><span class="sxs-lookup"><span data-stu-id="252ae-158">When placing the extension JSON at the root of the template, the resource name includes a reference to the parent virtual machine, and the type reflects the nested configuration.</span></span>  

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

## <a name="azure-cli-deployment"></a><span data-ttu-id="252ae-159">Az Azure CLI-telepítés</span><span class="sxs-lookup"><span data-stu-id="252ae-159">Azure CLI deployment</span></span>

<span data-ttu-id="252ae-160">Az Azure CLI segítségével az OMS-ügynök Virtuálisgép-bővítmény telepítése egy meglévő virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="252ae-160">The Azure CLI can be used to deploy the OMS Agent VM extension to an existing virtual machine.</span></span> <span data-ttu-id="252ae-161">Cserélje le az OMS-kulcsot és az OMS-azonosító az OMS-munkaterület szerintiek.</span><span class="sxs-lookup"><span data-stu-id="252ae-161">Replace the OMS key and OMS ID with those from your OMS workspace.</span></span> 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="252ae-162">Hibaelhárítás és támogatás</span><span class="sxs-lookup"><span data-stu-id="252ae-162">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="252ae-163">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="252ae-163">Troubleshoot</span></span>

<span data-ttu-id="252ae-164">Bővítmény központi telepítések állapotára vonatkozó lehet adatokat beolvasni az Azure-portálon, és az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="252ae-164">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure CLI.</span></span> <span data-ttu-id="252ae-165">A megadott virtuális gépek bővítmények központi telepítési állapotának megtekintéséhez a következő parancsot az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="252ae-165">To see the deployment state of extensions for a given VM, run the following command using the Azure CLI.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="252ae-166">A következő fájl kerül a bővítmény végrehajtás kimenetének:</span><span class="sxs-lookup"><span data-stu-id="252ae-166">Extension execution output is logged to the following file:</span></span>

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a><span data-ttu-id="252ae-167">Hibakódok és azok jelentését</span><span class="sxs-lookup"><span data-stu-id="252ae-167">Error codes and their meanings</span></span>

| <span data-ttu-id="252ae-168">Hibakód:</span><span class="sxs-lookup"><span data-stu-id="252ae-168">Error Code</span></span> | <span data-ttu-id="252ae-169">Jelentése</span><span class="sxs-lookup"><span data-stu-id="252ae-169">Meaning</span></span> | <span data-ttu-id="252ae-170">Lehetséges művelet</span><span class="sxs-lookup"><span data-stu-id="252ae-170">Possible Action</span></span> |
| :---: | --- | --- |
| <span data-ttu-id="252ae-171">10</span><span class="sxs-lookup"><span data-stu-id="252ae-171">10</span></span> | <span data-ttu-id="252ae-172">Virtuális gép már csatlakoztatva van egy OMS-munkaterület</span><span class="sxs-lookup"><span data-stu-id="252ae-172">VM is already connected to an OMS workspace</span></span> | <span data-ttu-id="252ae-173">A virtuális gép csatlakozik a bővítmény sémában megadott munkaterület, stopOnMultipleConnections értéke HAMIS, a nyilvános beállításai, vagy távolítsa el ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="252ae-173">To connect the VM to the workspace specified in the extension schema, set stopOnMultipleConnections to false in public settings or remove this property.</span></span> <span data-ttu-id="252ae-174">Ez a virtuális gép lekérdezi számlázva után az egyes munkaterületeken van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="252ae-174">This VM gets billed once for each workspace it is connected to.</span></span> |
| <span data-ttu-id="252ae-175">11</span><span class="sxs-lookup"><span data-stu-id="252ae-175">11</span></span> | <span data-ttu-id="252ae-176">A bővítmény megadott Érvénytelen konfiguráció</span><span class="sxs-lookup"><span data-stu-id="252ae-176">Invalid config provided to the extension</span></span> | <span data-ttu-id="252ae-177">Kövesse a fenti példákban beállítása a telepítéshez szükséges minden tulajdonság értékével.</span><span class="sxs-lookup"><span data-stu-id="252ae-177">Follow the preceding examples to set all property values necessary for deployment.</span></span> |
| <span data-ttu-id="252ae-178">12</span><span class="sxs-lookup"><span data-stu-id="252ae-178">12</span></span> | <span data-ttu-id="252ae-179">A dpkg package manager zárolva van</span><span class="sxs-lookup"><span data-stu-id="252ae-179">The dpkg package manager is locked</span></span> | <span data-ttu-id="252ae-180">Győződjön meg arról, hogy minden dpkg frissítési műveletek a számítógépen végzett, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="252ae-180">Make sure all dpkg update operations on the machine have finished and retry.</span></span> |
| <span data-ttu-id="252ae-181">20</span><span class="sxs-lookup"><span data-stu-id="252ae-181">20</span></span> | <span data-ttu-id="252ae-182">Túl korán nevű engedélyezése</span><span class="sxs-lookup"><span data-stu-id="252ae-182">Enable called prematurely</span></span> | <span data-ttu-id="252ae-183">[Az Azure Linux ügynök frissítése](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) az elérhető legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="252ae-183">[Update the Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) to the latest available version.</span></span> |
| <span data-ttu-id="252ae-184">51</span><span class="sxs-lookup"><span data-stu-id="252ae-184">51</span></span> | <span data-ttu-id="252ae-185">Ezt a bővítményt a virtuális gép operációs rendszer nem támogatott</span><span class="sxs-lookup"><span data-stu-id="252ae-185">This extension is not supported on the VM's operation system</span></span> | |
| <span data-ttu-id="252ae-186">55</span><span class="sxs-lookup"><span data-stu-id="252ae-186">55</span></span> | <span data-ttu-id="252ae-187">Nem lehet kapcsolódni a Microsoft Operations Management Suite szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="252ae-187">Cannot connect to the Microsoft Operations Management Suite service</span></span> | <span data-ttu-id="252ae-188">Ellenőrizze, hogy a rendszernek van-e Internet-hozzáféréssel, vagy adtak meg, hogy érvényes HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="252ae-188">Check that the system either has Internet access, or that a valid HTTP proxy has been provided.</span></span> <span data-ttu-id="252ae-189">Ezenkívül ellenőrizze a helyességét a munkaterület azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="252ae-189">Additionally, check the correctness of the workspace ID.</span></span> |

<span data-ttu-id="252ae-190">További hibaelhárítási információért található meg a [OMS-ügynök-az-Linux hibaelhárítási útmutatója](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span><span class="sxs-lookup"><span data-stu-id="252ae-190">Additional troubleshooting information can be found on the [OMS-Agent-for-Linux Troubleshooting Guide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span></span>

### <a name="support"></a><span data-ttu-id="252ae-191">Támogatás</span><span class="sxs-lookup"><span data-stu-id="252ae-191">Support</span></span>

<span data-ttu-id="252ae-192">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők a a [MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="252ae-192">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="252ae-193">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="252ae-193">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="252ae-194">Lépjen a [az Azure támogatási webhelyén](https://azure.microsoft.com/en-us/support/options/) válassza ki a Get-támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="252ae-194">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="252ae-195">Támogatja az Azure használatával kapcsolatos információkért olvassa el a [Microsoft Azure-támogatás – gyakori kérdések](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="252ae-195">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
