---
title: "az Azure virtuális hálózat (klasszikus) - hálózati konfigurációs fájl aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate, és módosítsa a virtuális hálózatok (klasszikus), módosítása, importálása és exportálása révén a hálózati konfigurációs fájlt."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 009108d4315b4b7146d3f1cf2436ee211d2d89ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a><span data-ttu-id="c9811-103">A hálózati konfigurációs fájl segítségével virtuális hálózat (klasszikus) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9811-103">Configure a virtual network (classic) using a network configuration file</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c9811-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9811-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="c9811-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="c9811-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="c9811-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello erőforrás-kezelő telepítési modellt használja.</span><span class="sxs-lookup"><span data-stu-id="c9811-106">Microsoft recommends that most new deployments use hello Resource Manager deployment model.</span></span>

<span data-ttu-id="c9811-107">Hozzon létre, és konfigurálja a virtuális hálózat (klasszikus) a hálózati konfigurációs fájllal hello Azure parancssori felület (CLI) 1.0-s vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c9811-107">You can create and configure a virtual network (classic) with a network configuration file using hello Azure command-line interface (CLI) 1.0 or Azure PowerShell.</span></span> <span data-ttu-id="c9811-108">Nem lehet létrehozni vagy módosítani a virtuális hálózaton hello Azure Resource Manager telepítési modell hálózati konfigurációs fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="c9811-108">You cannot create or modify a virtual network through hello Azure Resource Manager deployment model using a network configuration file.</span></span> <span data-ttu-id="c9811-109">Ön nem lehet nem hello Azure portál toocreate vagy módosítsa a virtuális hálózat (klasszikus) konfigurációs fájlt, azonban használhat hello Azure portál toocreate egy virtuális hálózat (klasszikus), a hálózati konfigurációs fájl használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="c9811-109">You cannot use hello Azure portal toocreate or modify a virtual network (classic) using a network configuration file, however you can use hello Azure portal toocreate a virtual network (classic), without using a network configuration file.</span></span>

<span data-ttu-id="c9811-110">Létrehozása és konfigurálása a virtuális hálózat (klasszikus) a hálózati konfigurációs fájllal szükséges exportálása, módosítása és hello fájl importálásakor.</span><span class="sxs-lookup"><span data-stu-id="c9811-110">Creating and configuring a virtual network (classic) with a network configuration file requires exporting, changing, and importing hello file.</span></span>

## <span data-ttu-id="c9811-111"><a name="export"></a>A hálózati konfigurációs fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="c9811-111"><a name="export"></a>Export a network configuration file</span></span>

<span data-ttu-id="c9811-112">Használhatja a PowerShell vagy a hello Azure CLI tooexport hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9811-112">You can use PowerShell or hello Azure CLI tooexport a network configuration file.</span></span> <span data-ttu-id="c9811-113">PowerShell exportálása az XML-fájl, hello Azure parancssori felület egy json-fájl exportálása során.</span><span class="sxs-lookup"><span data-stu-id="c9811-113">PowerShell exports an XML file, while hello Azure CLI exports a json file.</span></span>

### <a name="powershell"></a><span data-ttu-id="c9811-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9811-114">PowerShell</span></span>
 
1. <span data-ttu-id="c9811-115">[Azure PowerShell telepítése, és jelentkezzen be tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9811-115">[Install Azure PowerShell and sign in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="c9811-116">Módosítsa a könyvtárat hello (és győződjön meg arról, hogy létezik-e) és a következő parancs megfelelő, majd futtassa hello parancs tooexport hello hálózati konfigurációs fájl nevét a hello:</span><span class="sxs-lookup"><span data-stu-id="c9811-116">Change hello directory (and ensure it exists) and filename in hello following command as desired, then run hello command tooexport hello network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a><span data-ttu-id="c9811-117">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c9811-117">Azure CLI 1.0</span></span>

1. <span data-ttu-id="c9811-118">[Telepítse az Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9811-118">[Install hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="c9811-119">Végezze el a hátralévő lépéseket az Azure CLI 1.0 parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="c9811-119">Complete hello remaining steps from an Azure CLI 1.0 command prompt.</span></span>
2. <span data-ttu-id="c9811-120">Jelentkezzen be tooAzure hello megadásával `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c9811-120">Log in tooAzure by entering hello `azure login` command.</span></span>
3. <span data-ttu-id="c9811-121">Nyissa meg a címterület-kezelési mód hello megadásával `azure config mode asm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c9811-121">Ensure you're in asm mode by entering hello `azure config mode asm` command.</span></span>
4. <span data-ttu-id="c9811-122">Módosítsa a könyvtárat hello (és győződjön meg arról, hogy létezik-e) és a következő parancs megfelelő, majd futtassa hello parancs tooexport hello hálózati konfigurációs fájl nevét a hello:</span><span class="sxs-lookup"><span data-stu-id="c9811-122">Change hello directory (and ensure it exists) and filename in hello following command as desired, then run hello command tooexport hello network configuration file:</span></span>
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a><span data-ttu-id="c9811-123">Hozzon létre vagy módosítsa a hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9811-123">Create or modify a network configuration file</span></span>

<span data-ttu-id="c9811-124">A hálózati konfigurációs fájl XML-fájl (amikor a PowerShell használatával) vagy egy json-fájl (használata esetén hello Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="c9811-124">A network configuration file is an XML file (when using PowerShell) or a json file (when using hello Azure CLI).</span></span> <span data-ttu-id="c9811-125">Hello fájl bármely szöveg vagy XML/json-szerkesztőben szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="c9811-125">You can edit hello file in any text, or XML/json editor.</span></span> <span data-ttu-id="c9811-126">Hello [hálózati konfigurációs fájl séma beállításai](https://msdn.microsoft.com/library/azure/jj157100.aspx) cikk tartalmazza az összes beállítást.</span><span class="sxs-lookup"><span data-stu-id="c9811-126">hello [Network configuration file schema settings](https://msdn.microsoft.com/library/azure/jj157100.aspx) article includes details for all settings.</span></span> <span data-ttu-id="c9811-127">Hello beállítások további ismertetése [virtuális hálózatok és beállítások megtekintéséhez](virtual-network-manage-network.md#view-vnet).</span><span class="sxs-lookup"><span data-stu-id="c9811-127">For additional explanation of hello settings, see [View virtual networks and settings](virtual-network-manage-network.md#view-vnet).</span></span> <span data-ttu-id="c9811-128">hello módosításokat toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="c9811-128">hello changes you make toohello file:</span></span>

- <span data-ttu-id="c9811-129">Meg kell felelnie a hello séma vagy importáló hello hálózati konfigurációs fájl sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c9811-129">Must comply with hello schema, or importing hello network configuration file will fail.</span></span>
- <span data-ttu-id="c9811-130">Felülírja a meglévő hálózati beállításait az előfizetés, ezért rendkívül körültekintően járjon el módosításakor.</span><span class="sxs-lookup"><span data-stu-id="c9811-130">Overwrite any existing network settings for your subscription, so use extreme caution when making modifications.</span></span> <span data-ttu-id="c9811-131">Például hivatkozás hello példa hálózati konfigurációs fájlok kövesse.</span><span class="sxs-lookup"><span data-stu-id="c9811-131">For example, reference hello example network configuration files that follow.</span></span> <span data-ttu-id="c9811-132">Tegyük fel például, hello eredeti fájlban szereplő két **VirtualNetworkSite** példányok, és megváltozott, hello példákban látható módon.</span><span class="sxs-lookup"><span data-stu-id="c9811-132">Say hello original file contained two **VirtualNetworkSite** instances, and you changed it, as shown in hello examples.</span></span> <span data-ttu-id="c9811-133">Hello fájl importálásakor Azure törli a virtuális hálózat hello hello **VirtualNetworkSite** példány eltávolította a hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="c9811-133">When you import hello file, Azure deletes hello virtual network for hello **VirtualNetworkSite** instance you removed in hello file.</span></span> <span data-ttu-id="c9811-134">Egyszerűsített ebben a forgatókönyvben azt feltételezi, hogy egyetlen erőforrás volt hello virtuális hálózatban, mintha, hello virtuális hálózat nem törölhető, és hello importálása sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c9811-134">This simplified scenario assumes no resources were in hello virtual network, as if there were, hello virtual network could not be deleted, and hello import would fail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c9811-135">Azure úgy ítéli meg, olyan alhálózatot, amely rendelkezik egy telepített, tooit **használatban**.</span><span class="sxs-lookup"><span data-stu-id="c9811-135">Azure considers a subnet that has something deployed tooit as **in use**.</span></span> <span data-ttu-id="c9811-136">Ha egy alhálózat használatban van, nem lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="c9811-136">When a subnet is in use, it cannot be modified.</span></span> <span data-ttu-id="c9811-137">Módosítása az alhálózati adatokat a hálózati konfigurációs fájlban, előtt helyezze át, amelyeket központilag telepített toohello alhálózati tooa másik alhálózatot, amely nem módosított.</span><span class="sxs-lookup"><span data-stu-id="c9811-137">Before modifying subnet information in a network configuration file, move anything that you have deployed toohello subnet tooa different subnet that isn't being modified.</span></span> <span data-ttu-id="c9811-138">Lásd: [áthelyezése egy másik alhálózat virtuális gép vagy Szerepkörpéldány tooa](virtual-networks-move-vm-role-to-subnet.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="c9811-138">See [Move a VM or Role Instance tooa Different Subnet](virtual-networks-move-vm-role-to-subnet.md) for details.</span></span>

### <a name="example-xml-for-use-with-powershell"></a><span data-ttu-id="c9811-139">Példa XML PowerShell való használatra</span><span class="sxs-lookup"><span data-stu-id="c9811-139">Example XML for use with PowerShell</span></span>

<span data-ttu-id="c9811-140">hello következő hálózati konfigurációs szakasz létrehoz egy virtuális hálózatot nevű *myVirtualNetwork* a cím rendelkező *10.0.0.0/16* a hello *USA keleti régiója* Azure a régióban.</span><span class="sxs-lookup"><span data-stu-id="c9811-140">hello following example network configuration file creates a virtual network named *myVirtualNetwork* with an address space of *10.0.0.0/16* in hello *East US* Azure region.</span></span> <span data-ttu-id="c9811-141">hello virtuális hálózat nevű egy alhálózatot tartalmaz *mySubnet* rendelkező egy címelőtagot *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="c9811-141">hello virtual network contains one subnet named *mySubnet* with an address prefix of *10.0.0.0/24*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

<span data-ttu-id="c9811-142">Exportált hello hálózati konfigurációs fájlt nem tartalma tartalmaz, az előző példában hello hello XML másolja, és illessze be egy új fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9811-142">If hello network configuration file you exported contains no contents, you can copy hello XML in hello previous example, and paste it into a new file.</span></span>

### <a name="example-json-for-use-with-hello-azure-cli-10"></a><span data-ttu-id="c9811-143">Példa JSON hello Azure CLI 1.0 való használatra</span><span class="sxs-lookup"><span data-stu-id="c9811-143">Example JSON for use with hello Azure CLI 1.0</span></span>

<span data-ttu-id="c9811-144">hello következő hálózati konfigurációs szakasz létrehoz egy virtuális hálózatot nevű *myVirtualNetwork* a cím rendelkező *10.0.0.0/16* a hello *USA keleti régiója* Azure a régióban.</span><span class="sxs-lookup"><span data-stu-id="c9811-144">hello following example network configuration file creates a virtual network named *myVirtualNetwork* with an address space of *10.0.0.0/16* in hello *East US* Azure region.</span></span> <span data-ttu-id="c9811-145">hello virtuális hálózat nevű egy alhálózatot tartalmaz *mySubnet* rendelkező egy címelőtagot *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="c9811-145">hello virtual network contains one subnet named *mySubnet* with an address prefix of *10.0.0.0/24*.</span></span>

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

<span data-ttu-id="c9811-146">Exportált hello hálózati konfigurációs fájlt nem tartalma tartalmaz, az előző példában hello hello json másolja, és illessze be egy új fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9811-146">If hello network configuration file you exported contains no contents, you can copy hello json in hello previous example, and paste it into a new file.</span></span>

## <span data-ttu-id="c9811-147"><a name="import"></a>A hálózati konfigurációs fájl importálása</span><span class="sxs-lookup"><span data-stu-id="c9811-147"><a name="import"></a>Import a network configuration file</span></span>

<span data-ttu-id="c9811-148">Használhatja a PowerShell vagy a hello Azure CLI tooimport hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9811-148">You can use PowerShell or hello Azure CLI tooimport a network configuration file.</span></span> <span data-ttu-id="c9811-149">PowerShell importálja az XML-fájlba, amíg hello Azure CLI importálja a json-fájl.</span><span class="sxs-lookup"><span data-stu-id="c9811-149">PowerShell imports an XML file, while hello Azure CLI imports a json file.</span></span> <span data-ttu-id="c9811-150">Ha hello importálás sikertelen lesz, győződjön meg arról, hello fájl megfelel a hello [hálózati konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9811-150">If hello import fails, confirm that hello file complies with hello [network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span> 

### <a name="powershell"></a><span data-ttu-id="c9811-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9811-151">PowerShell</span></span>
 
1. <span data-ttu-id="c9811-152">[Azure PowerShell telepítése, és jelentkezzen be tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9811-152">[Install Azure PowerShell and sign in tooAzure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="c9811-153">Hello directory és a fájlnév szükség szerint parancs a következő hello módosítsa, majd futtassa hello parancs tooimport hello hálózati konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="c9811-153">Change hello directory and filename in hello following command as necessary, then run hello command tooimport hello network configuration file:</span></span>
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a><span data-ttu-id="c9811-154">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c9811-154">Azure CLI 1.0</span></span>

1. <span data-ttu-id="c9811-155">[Telepítse az Azure CLI 1.0 hello](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9811-155">[Install hello Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> <span data-ttu-id="c9811-156">Végezze el a hátralévő lépéseket az Azure CLI 1.0 parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="c9811-156">Complete hello remaining steps from an Azure CLI 1.0 command prompt.</span></span>
2. <span data-ttu-id="c9811-157">Jelentkezzen be tooAzure hello megadásával `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c9811-157">Log in tooAzure by entering hello `azure login` command.</span></span>
3. <span data-ttu-id="c9811-158">Nyissa meg a címterület-kezelési mód hello megadásával `azure config mode asm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c9811-158">Ensure you're in asm mode by entering hello `azure config mode asm` command.</span></span>
4. <span data-ttu-id="c9811-159">Hello directory és a fájlnév szükség szerint parancs a következő hello módosítsa, majd futtassa hello parancs tooimport hello hálózati konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="c9811-159">Change hello directory and filename in hello following command as necessary, then run hello command tooimport hello network configuration file:</span></span>

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
