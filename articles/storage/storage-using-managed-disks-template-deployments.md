---
title: "aaaUsing által kezelt lemezeken Azure Resource Manager sablonokban |} Microsoft Docs"
description: "Hogyan toouse kezelése az Azure Resource Manager sablonokban misks részletek"
services: storage
documentationcenter: 
author: jboeshart
manager: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/01/2017
ms.author: jaboes
ms.openlocfilehash: ea83f4ed11acfd8f642dbc8331fa8cf077ef577c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-managed-disks-in-azure-resource-manager-templates"></a><span data-ttu-id="6ad4d-103">Az Azure Resource Manager-sablonok lemezek felügyelt</span><span class="sxs-lookup"><span data-stu-id="6ad4d-103">Using Managed Disks in Azure Resource Manager Templates</span></span>

<span data-ttu-id="6ad4d-104">Ez a dokumentum végigvezeti a kezelt és nem kezelt lemezeken Azure Resource Manager sablonok tooprovision virtuális gépek használatakor hello különbségei.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-104">This document walks through hello differences between managed and unmanaged disks when using Azure Resource Manager templates tooprovision virtual machines.</span></span> <span data-ttu-id="6ad4d-105">Ez segítséget nyújt tooupdate, meglévő sablonok nem felügyelt lemezek toomanaged lemezt használ.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-105">This will help you tooupdate existing templates that are using unmanaged Disks toomanaged disks.</span></span> <span data-ttu-id="6ad4d-106">Referenciaként használunk hello [101-vm-egyszerű-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) útmutatóként sablont.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-106">For reference, we are using hello [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) template as a guide.</span></span> <span data-ttu-id="6ad4d-107">Láthatja, hogy mindkét hello sablon [által kezelt lemezeken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) egy korábbi verzióját használja, és [lemezek nem felügyelt](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) Ha azt szeretné toodirectly hasonlítsa össze azokat.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-107">You can see hello template using both [managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) and a prior version using [unmanaged disks](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) if you'd like toodirectly compare them.</span></span>

## <a name="unmanaged-disks-template-formatting"></a><span data-ttu-id="6ad4d-108">Nem felügyelt lemezek sablon formázás</span><span class="sxs-lookup"><span data-stu-id="6ad4d-108">Unmanaged Disks template formatting</span></span>

<span data-ttu-id="6ad4d-109">toobegin, hogy tekintse meg, hogyan nem felügyelt lemezek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-109">toobegin, we take a look at how unmanaged disks are deployed.</span></span> <span data-ttu-id="6ad4d-110">Nem felügyelt lemezek létrehozásakor a tárolási fiók toohold hello VHD-fájlokat kell.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-110">When creating unmanaged disks, you need a storage account toohold hello VHD files.</span></span> <span data-ttu-id="6ad4d-111">Hozzon létre egy új tárfiókot, vagy használjon már meglévő.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-111">You can create a new storage account or use one that already exists.</span></span> <span data-ttu-id="6ad4d-112">Ez a cikk bemutatja, hogyan toocreate egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-112">This article will show you how toocreate a new storage account.</span></span> <span data-ttu-id="6ad4d-113">tooaccomplish, a tárolási fiók erőforrás hello erőforrások blokkban kell alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-113">tooaccomplish this, you need a storage account resource in hello resources block as shown below.</span></span>

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="6ad4d-114">Hello virtuális gép objektumban kell hello tárolási fiók tooensure hello virtual machine létrehozva függ.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-114">Within hello virtual machine object, we need a dependency on hello storage account tooensure that it's created before hello virtual machine.</span></span> <span data-ttu-id="6ad4d-115">Hello belül `storageProfile` szakaszban, majd meg hello hello VHD helyre, amely hello tárfiók hivatkozik, és az operációs rendszer hello lemez és bármely adatlemezek szükséges teljes URI-címe.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-115">Within hello `storageProfile` section, we then specify hello full URI of hello VHD location, which references hello storage account and is needed for hello OS disk and any data disks.</span></span> 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a><span data-ttu-id="6ad4d-116">Felügyelt lemezek sablon formázás</span><span class="sxs-lookup"><span data-stu-id="6ad4d-116">Managed disks template formatting</span></span>

<span data-ttu-id="6ad4d-117">A Azure felügyelt lemezek esetében a hello lemez egy legfelső szintű erőforrás lesz, és többé nem kell a tárolási fiók toobe hello felhasználó által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-117">With Azure Managed Disks, hello disk becomes a top-level resource and no longer requires a storage account toobe created by hello user.</span></span> <span data-ttu-id="6ad4d-118">Felügyelt lemezek először volt felfedett hello `2016-04-30-preview` API-verzió, akkor az összes azt követő API-verziók és elérhetőek most hello alapértelmezett lemeztípus.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-118">Managed disks were first exposed in hello `2016-04-30-preview` API version, they are available in all subsequent API versions and are now hello default disk type.</span></span> <span data-ttu-id="6ad4d-119">hello alábbiakban hello alapértelmezett beállításait ismerteti és részletesen, hogyan toofurther testre a lemezek.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-119">hello following sections walk through hello default settings and detail how toofurther customize your disks.</span></span>

> [!NOTE]
> <span data-ttu-id="6ad4d-120">Ajánlott toouse API verziója újabb, mint `2016-04-30-preview` mert jelentős változásokat közötti `2016-04-30-preview` és `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-120">It is recommended toouse an API version later than `2016-04-30-preview` as there were breaking changes between `2016-04-30-preview` and `2017-03-30`.</span></span>
>
>

### <a name="default-managed-disk-settings"></a><span data-ttu-id="6ad4d-121">Alapértelmezett felügyelt lemezes beállításai</span><span class="sxs-lookup"><span data-stu-id="6ad4d-121">Default managed disk settings</span></span>

<span data-ttu-id="6ad4d-122">toocreate felügyelt lemezeket, hogy már nem rendelkező virtuális toocreate hello tárolási fiók erőforrás kell, és a következőképpen frissítheti a virtuálisgép-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-122">toocreate a VM with managed disks, you no longer need toocreate hello storage account resource and can update your virtual machine resource as follows.</span></span> <span data-ttu-id="6ad4d-123">Kifejezetten vegye figyelembe, hogy hello `apiVersion` tükrözi `2017-03-30` és hello `osDisk` és `dataDisks` már nem hivatkozik az tooa hello virtuális merevlemez megadott URI-Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-123">Specifically note that hello `apiVersion` reflects `2017-03-30` and hello `osDisk` and `dataDisks` no longer refer tooa specific URI for hello VHD.</span></span> <span data-ttu-id="6ad4d-124">Ha telepítése További tulajdonságok megadása nélkül, használja a hello lemez [Standard-LRS tárolási](storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="6ad4d-124">When deploying without specifying additional properties, hello disk will use [Standard LRS storage](storage-redundancy.md).</span></span> <span data-ttu-id="6ad4d-125">Ha nem ad meg nevet, tart hello formátuma `<VMName>_OsDisk_1_<randomstring>` hello operációsrendszer-lemez és `<VMName>_disk<#>_<randomstring>` adatok lemezek.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-125">If no name is specified, it takes hello format of `<VMName>_OsDisk_1_<randomstring>` for hello OS disk and `<VMName>_disk<#>_<randomstring>` for each data disk.</span></span> <span data-ttu-id="6ad4d-126">Alapértelmezés szerint az Azure disk encryption le van tiltva; Olvasási/írási gyorsítótárazást az operációs rendszer hello lemez és adatlemezek nincs.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-126">By default, Azure disk encryption is disabled; caching is Read/Write for hello OS disk and None for data disks.</span></span> <span data-ttu-id="6ad4d-127">Az alábbi példa hello Észreveheti, továbbra is fennáll a tárolási fiók függőségi, bár ez csak a tárolási diagnosztikai és a lemezes tárolás esetén nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-127">You may notice in hello example below there is still a storage account dependency, though this is only for storage of diagnostics and is not needed for disk storage.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a><span data-ttu-id="6ad4d-128">Egy legfelső szintű felügyelt lemezes erőforrást használja</span><span class="sxs-lookup"><span data-stu-id="6ad4d-128">Using a top-level managed disk resource</span></span>

<span data-ttu-id="6ad4d-129">Egy alternatív toospecifying hello lemezkonfigurációt hello virtuálisgép-objektumot, mint legfelső szintű lemez erőforrás létrehozása, és mellékelje hello virtuális gép létrehozásának részeként.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-129">As an alternative toospecifying hello disk configuration in hello virtual machine object, you can create a top-level disk resource and attach it as part of hello virtual machine creation.</span></span> <span data-ttu-id="6ad4d-130">Például azt a lemezerőforrás toouse adatlemez, az alábbi módon hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-130">For example, we can create a disk resource as follows toouse as a data disk.</span></span>

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

<span data-ttu-id="6ad4d-131">Belül hello Virtuálisgép-objektum a csatlakoztatott lemez objektum toobe majd azt is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-131">Within hello VM object, we can then reference this disk object toobe attached.</span></span> <span data-ttu-id="6ad4d-132">A hello létrehozott lemez hello hello erőforrás-Azonosítóját megadó felügyelt `managedDisk` tulajdonság lehetővé teszi, hogy a hello melléklet hello lemez, a virtuális gép kerül létrehozásra hello.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-132">Specifying hello resource ID of hello managed disk we created in hello `managedDisk` property allows hello attachment of hello disk as hello VM is created.</span></span> <span data-ttu-id="6ad4d-133">Vegye figyelembe, hogy hello `apiVersion` a virtuális gép erőforrásához hello értéke túl`2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-133">Note that hello `apiVersion` for hello VM resource is set too`2017-03-30`.</span></span> <span data-ttu-id="6ad4d-134">Ne feledje, hogy létrehoztunk Önnek egy függőséget meg hello lemez erőforrás tooensure sikeres létrehozása előtt a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-134">Also note that we've created a dependency on hello disk resource tooensure it's successfully created before VM creation.</span></span> 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a><span data-ttu-id="6ad4d-135">Felügyelt rendelkezésre állási csoportok felügyelt lemezek használata virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ad4d-135">Create managed availability sets with VMs using managed disks</span></span>

<span data-ttu-id="6ad4d-136">toocreate felügyelt rendelkezésre állási készletek kezelt lemezeken, virtuális gépek hozzáadása hello `sku` objektum toohello rendelkezésre állási erőforrás és hello `name` tulajdonság túl`Aligned`.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-136">toocreate managed availability sets with VMs using managed disks, add hello `sku` object toohello availability set resource and set hello `name` property too`Aligned`.</span></span> <span data-ttu-id="6ad4d-137">Ez biztosítja, hogy az egyes virtuális gépek hello lemezek elég elkülönül egymástól tooavoid egyetlen ponton felmerülő hibákat.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-137">This ensures that hello disks for each VM are sufficiently isolated from each other tooavoid single points of failure.</span></span> <span data-ttu-id="6ad4d-138">Ne feledje, hogy hello `apiVersion` hello rendelkezésre állási csoport erőforrás van állítani túl`2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-138">Also note that hello `apiVersion` for hello availability set resource is set too`2017-03-30`.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a><span data-ttu-id="6ad4d-139">További forgatókönyvek és testreszabása</span><span class="sxs-lookup"><span data-stu-id="6ad4d-139">Additional scenarios and customizations</span></span>

<span data-ttu-id="6ad4d-140">teljes körű információt toofind hello REST API specifikációk, tekintse át a hello [felügyelt lemezes REST API-dokumentáció](/rest/api/manageddisks/disks/disks-create-or-update).</span><span class="sxs-lookup"><span data-stu-id="6ad4d-140">toofind full information on hello REST API specifications, please review hello [create a managed disk REST API documentation](/rest/api/manageddisks/disks/disks-create-or-update).</span></span> <span data-ttu-id="6ad4d-141">További helyzeteket is, valamint az alapértelmezett és az elfogadható értékek, amelyek lehetnek sablon központi telepítések keresztül elküldött toohello API találja.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-141">You will find additional scenarios, as well as default and acceptable values that can be submitted toohello API through template deployments.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6ad4d-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ad4d-142">Next steps</span></span>

* <span data-ttu-id="6ad4d-143">Teljes kezelt lemezek használó sablonokat Microsoft hello Azure gyors üzembe helyezés tárház hivatkozásokat követve.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-143">For full templates that use managed disks visit hello following Azure Quickstart Repo links.</span></span>
    * [<span data-ttu-id="6ad4d-144">A felügyelt lemezes Windows VM</span><span class="sxs-lookup"><span data-stu-id="6ad4d-144">Windows VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [<span data-ttu-id="6ad4d-145">A felügyelt lemezes Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="6ad4d-145">Linux VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [<span data-ttu-id="6ad4d-146">Felügyelt lemezes sablonok teljes listája</span><span class="sxs-lookup"><span data-stu-id="6ad4d-146">Full list of managed disk templates</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="6ad4d-147">A Microsoft hello [Azure felügyelt lemezekhez – áttekintés](storage-managed-disks-overview.md) további információt a dokumentum toolearn által kezelt lemezeken.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-147">Visit hello [Azure Managed Disks Overview](storage-managed-disks-overview.md) document toolearn more about managed disks.</span></span>
* <span data-ttu-id="6ad4d-148">Tekintse át a virtuális gép erőforrásai hello sablon referenciadokumentációt tartalmaz hello felkeresésével [Microsoft.Compute/virtualMachines sablonra való hivatkozást](/templates/microsoft.compute/virtualmachines) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-148">Review hello template reference documentation for virtual machine resources by visiting hello [Microsoft.Compute/virtualMachines template reference](/templates/microsoft.compute/virtualmachines) document.</span></span>
* <span data-ttu-id="6ad4d-149">Tekintse át a hello sablon referenciadokumentációt tartalmaz lemezerőforrásokat hello felkeresésével [Microsoft.Compute/disks sablonra való hivatkozást](/templates/microsoft.compute/disks) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6ad4d-149">Review hello template reference documentation for disk resources by visiting hello [Microsoft.Compute/disks template reference](/templates/microsoft.compute/disks) document.</span></span>
 
