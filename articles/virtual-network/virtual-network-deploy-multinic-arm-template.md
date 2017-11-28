---
title: "virtuális gép több hálózati adapter - Azure Resource Manager-sablon és aaaCreate |} Microsoft Docs"
description: "Egy Azure Resource Manager-sablon használatával több hálózati adapterrel rendelkező virtuális gép létrehozása."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5d9ffcbd40c72dc18ae6de38e739eb5e45bf669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a><span data-ttu-id="41120-103">Egy sablon használatával több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="41120-103">Create a VM with multiple NICs using a template</span></span>
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="41120-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="41120-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="41120-105">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="41120-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="41120-106">hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="41120-106">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41120-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="41120-107">Prerequisites</span></span>
<span data-ttu-id="41120-108">Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="41120-108">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="41120-109">toocreate ezeket az erőforrásokat, végezze el hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="41120-109">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="41120-110">Keresse meg a túl[hello sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="41120-110">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="41120-111">Hello sablon lapon jobbra toohello **szülő erőforráscsoport**, kattintson a **tooAzure telepítése**.</span><span class="sxs-lookup"><span data-stu-id="41120-111">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="41120-112">Szükség esetén módosítsa a hello paraméterértékeket, majd hello lépésekkel hello Azure betekintő portál toodeploy hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="41120-112">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41120-113">Győződjön meg arról, hogy a tárfiókok neve egyedi.</span><span class="sxs-lookup"><span data-stu-id="41120-113">Make sure your storage account names are unique.</span></span> <span data-ttu-id="41120-114">Ismétlődő tárfiókok neve nem lehet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="41120-114">You cannot have duplicate storage account names in Azure.</span></span>
> 

## <a name="understand-hello-deployment-template"></a><span data-ttu-id="41120-115">Hello központi telepítési sablont ismertetése</span><span class="sxs-lookup"><span data-stu-id="41120-115">Understand hello deployment template</span></span>
<span data-ttu-id="41120-116">Ebben a dokumentációban megadott hello sablon telepítése előtt győződjön meg arról hogy tudomásul veszi, hogyan kezeli.</span><span class="sxs-lookup"><span data-stu-id="41120-116">Before you deploy hello template provided with this documentation, make sure you understand what it does.</span></span> <span data-ttu-id="41120-117">a lépéseket követve hello hello sablon jó áttekintést biztosítanak:</span><span class="sxs-lookup"><span data-stu-id="41120-117">hello following steps provide a good overview of hello template:</span></span>

1. <span data-ttu-id="41120-118">Keresse meg a túl[hello sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="41120-118">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="41120-119">Kattintson a **azuredeploy.json** tooopen hello sablonfájl.</span><span class="sxs-lookup"><span data-stu-id="41120-119">Click **azuredeploy.json** tooopen hello template file.</span></span>
3. <span data-ttu-id="41120-120">Értesítés hello *osType* alábbi paraméter.</span><span class="sxs-lookup"><span data-stu-id="41120-120">Notice hello *osType* parameter listed below.</span></span> <span data-ttu-id="41120-121">Ez a paraméter akkor használt tooselect milyen VM-lemezkép toouse hello adatbázis-kiszolgáló, valamint több operációs rendszer kapcsolódó beállításokat.</span><span class="sxs-lookup"><span data-stu-id="41120-121">This parameter is used tooselect what VM image toouse for hello database server, along with multiple operating system related settings.</span></span>

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS toouse for VMs: Windows or Ubuntu."
      }
    },
    ```

4. <span data-ttu-id="41120-122">Görgessen le a változók toohello listája, és ellenőrizze a hello hello definíciója **dbVMSetting** alább felsorolt változók.</span><span class="sxs-lookup"><span data-stu-id="41120-122">Scroll down toohello list of variables, and check hello definition for hello **dbVMSetting** variables, listed below.</span></span> <span data-ttu-id="41120-123">Hello tömbelemek hello szereplő egyik kap **dbVMSettings** változó.</span><span class="sxs-lookup"><span data-stu-id="41120-123">It receives one of hello array elements contained in hello **dbVMSettings** variable.</span></span> <span data-ttu-id="41120-124">Ha ismeri a szoftver fejlesztési terminológia, megtekintheti a hello **dbVMSettings** egy kivonattáblát vagy dictionary változó.</span><span class="sxs-lookup"><span data-stu-id="41120-124">If you are familiar with software development terminology, you can view hello **dbVMSettings** variable as a hash table, or a dictionary.</span></span>

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. <span data-ttu-id="41120-125">Tegyük fel, toodeploy Windows virtuális gépeken futó SQL hello háttér-mellett dönt.</span><span class="sxs-lookup"><span data-stu-id="41120-125">Suppose you decide toodeploy Windows VMs running SQL in hello back-end.</span></span> <span data-ttu-id="41120-126">A következő majd hello **osType** lenne *Windows*, és hello **dbVMSetting** változóhoz az alább felsorolt hello elemet, amely jelzi a hello hello első értékét tartalmazná **dbVMSettings** változó.</span><span class="sxs-lookup"><span data-stu-id="41120-126">Then hello value for **osType** would be *Windows*, and hello **dbVMSetting** variable would contain hello element listed below, which represents hello first value in hello **dbVMSettings** variable.</span></span>

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. <span data-ttu-id="41120-127">Értesítés hello **vmSize** hello értéket tartalmaz *Standard_DS3*.</span><span class="sxs-lookup"><span data-stu-id="41120-127">Notice hello **vmSize** contains hello value *Standard_DS3*.</span></span> <span data-ttu-id="41120-128">Csak egyes Virtuálisgép-méretek hello több hálózati adapter használatát teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="41120-128">Only certain VM sizes allow for hello use of multiple NICs.</span></span> <span data-ttu-id="41120-129">Ellenőrizheti, hogy mely Virtuálisgép-méretek támogatja több hálózati adapter hello olvasásával [Windows Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Linux Virtuálisgép-méretek](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="41120-129">You can verify which VM sizes support multiple NICs by reading hello [Windows VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux VM sizes](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) articles.</span></span>

7. <span data-ttu-id="41120-130">Görgessen lefelé, túl**erőforrások** és a hirdetmény hello első eleme.</span><span class="sxs-lookup"><span data-stu-id="41120-130">Scroll down too**resources** and notice hello first element.</span></span> <span data-ttu-id="41120-131">A tárfiók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="41120-131">It describes a storage account.</span></span> <span data-ttu-id="41120-132">Ezt a tárfiókot az egyes adatbázisok virtuális gép által használt használt toomaintain hello adatlemezek lesz.</span><span class="sxs-lookup"><span data-stu-id="41120-132">This storage account will be used toomaintain hello data disks used by each database VM.</span></span> <span data-ttu-id="41120-133">Ebben az esetben az egyes adatbázisok VM van egy rendszeres storage-ban tárolt operációsrendszer-lemez, és két adatlemezek SSD-re (prémium) tárolja.</span><span class="sxs-lookup"><span data-stu-id="41120-133">In this scenario, each database VM has an OS disk stored in regular storage, and two data disks stored in SSD (premium) storage.</span></span>

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. <span data-ttu-id="41120-134">Görgessen le a következő erőforrás toohello, az alább felsorolt.</span><span class="sxs-lookup"><span data-stu-id="41120-134">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="41120-135">Ehhez az erőforráshoz a hálózati adapter által használt minden virtuális gép adatbázis adatbázis-hozzáférési hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="41120-135">This resource represents hello NIC used for database access in each database VM.</span></span> <span data-ttu-id="41120-136">Értesítés hello használata hello **másolási** függvényt ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="41120-136">Notice hello use of hello **copy** function in this resource.</span></span> <span data-ttu-id="41120-137">hello sablon teszi toodeploy, ahányat csak szeretne, a hello alapján sok virtuális gép **dbCount** paraméter.</span><span class="sxs-lookup"><span data-stu-id="41120-137">hello template allows you toodeploy as many VMs as you want, based on hello **dbCount** parameter.</span></span> <span data-ttu-id="41120-138">Ezért toocreate van szüksége az adatbázis eléréséhez, az egyes virtuális gépek egyik hálózati adapter olyan mértékű hello.</span><span class="sxs-lookup"><span data-stu-id="41120-138">Therefore you need toocreate hello same amount of NICs for database access, one for each VM.</span></span>

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. <span data-ttu-id="41120-139">Görgessen le a következő erőforrás toohello, az alább felsorolt.</span><span class="sxs-lookup"><span data-stu-id="41120-139">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="41120-140">Ehhez az erőforráshoz minden adatbázis VM-kezelésre szolgáló hálózati hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="41120-140">This resource represents hello NIC used for management in each database VM.</span></span> <span data-ttu-id="41120-141">Ebben az esetben kell ezek a hálózati adapterek közül az egyes adatbázisok virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="41120-141">Once again, you need one of these NICs for each database VM.</span></span> <span data-ttu-id="41120-142">Értesítés hello **hálózati biztonsági csoporthoz tartozik** elem csatolása, amely lehetővé teszi a hozzáférést tooRDP/SSH toothis NIC csak egy NSG.</span><span class="sxs-lookup"><span data-stu-id="41120-142">Notice hello **networkSecurityGroup** element, linking an NSG that allows access tooRDP/SSH toothis NIC only.</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. <span data-ttu-id="41120-143">Görgessen le a következő erőforrás toohello, az alább felsorolt.</span><span class="sxs-lookup"><span data-stu-id="41120-143">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="41120-144">Ez az erőforrás egy rendelkezésre állási készlet toobe összes adatbázis virtuális gépek által megosztott jelöli.</span><span class="sxs-lookup"><span data-stu-id="41120-144">This resource represents an availability set toobe shared by all database VMs.</span></span> <span data-ttu-id="41120-145">Ily módon azt garantálja, hogy mindig lesz egy virtuális gép a karbantartás során futó hello.</span><span class="sxs-lookup"><span data-stu-id="41120-145">That way, you guarantee that there will always be one VM in hello set running during maintenance.</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. <span data-ttu-id="41120-146">Görgessen le a következő erőforrás toohello.</span><span class="sxs-lookup"><span data-stu-id="41120-146">Scroll down toohello next resource.</span></span> <span data-ttu-id="41120-147">Ehhez az erőforráshoz hello adatbázis virtuális gépeket, jelöli hello látható első néhány sor alábbi.</span><span class="sxs-lookup"><span data-stu-id="41120-147">This resource represents hello database VMs, as seen in hello first few lines listed below.</span></span> <span data-ttu-id="41120-148">Értesítés hello használata hello **másolási** újra működik, győződjön meg arról, hogy több virtuális gépek jönnek létre alapján hello **dbCount** paraméter.</span><span class="sxs-lookup"><span data-stu-id="41120-148">Notice hello use of hello **copy** function again, ensuring that multiple VMs are created based on hello **dbCount** parameter.</span></span> <span data-ttu-id="41120-149">Észrevette hello **dependsOn** gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="41120-149">Also notice hello **dependsOn** collection.</span></span> <span data-ttu-id="41120-150">Felsorolja alatt álló virtuális gép van telepítve, valamint hello rendelkezésre állási csoportot, és az hello tárfiók hello előtt létrehozott szükséges toobe két hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="41120-150">It lists two NICs being necessary toobe created before hello VM is deployed, along with hello availability set, and hello storage account.</span></span>

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. <span data-ttu-id="41120-151">Görgessen le a hello VM erőforrás toohello **networkProfile** elem, az alább felsorolt.</span><span class="sxs-lookup"><span data-stu-id="41120-151">Scroll down in hello VM resource toohello **networkProfile** element, as listed below.</span></span> <span data-ttu-id="41120-152">Figyelje meg, hogy vannak-e két hálózati adaptert, az egyes virtuális gépek hivatkozás alatt.</span><span class="sxs-lookup"><span data-stu-id="41120-152">Notice that there are two NICs being reference for each VM.</span></span> <span data-ttu-id="41120-153">Több hálózati adaptert a virtuális gépek létrehozásakor meg kell adni a hello **elsődleges** hello hálózati adapterek közül az egyik tulajdonság túl*igaz*, és a többi túl hello*hamis*.</span><span class="sxs-lookup"><span data-stu-id="41120-153">When you create multiple NICs for a VM, you must set hello **primary** property of one of hello NICs too*true*, and hello rest too*false*.</span></span>

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a><span data-ttu-id="41120-154">Központi telepítése hello ARM-sablon használatával toodeploy kattintson</span><span class="sxs-lookup"><span data-stu-id="41120-154">Deploy hello ARM template by using click toodeploy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41120-155">Ellenőrizze, hogy hajtsa végre a hello [szükséges előfeltételek](#Pre-requisites) lépéseket hello az alábbi utasítások követése előtt.</span><span class="sxs-lookup"><span data-stu-id="41120-155">Make sure you follow hello [pre-requisites](#Pre-requisites) steps before following hello instructions below.</span></span>
> 

<span data-ttu-id="41120-156">hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="41120-156">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="41120-157">toodeploy toodeploy, a sablon használatával kattintson kövesse [Ez a hivatkozás](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello jobbra **erőforráscsoport háttér (lásd a dokumentációt)** kattintson **tooAzure telepítése**, cserélje le hello alapértelmezett paraméterértékek, ha szükséges, és hello utasítások hello portálon.</span><span class="sxs-lookup"><span data-stu-id="41120-157">toodeploy this template using click toodeploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello right of **Backend resource group (see documentation)** click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

<span data-ttu-id="41120-158">hello az alábbi ábra hello tartalmát hello új erőforráscsoportot, a telepítés utáni.</span><span class="sxs-lookup"><span data-stu-id="41120-158">hello figure below shows hello contents of hello new resource group, after deployment.</span></span>

![Háttér-erőforráscsoport](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="41120-160">Hello sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="41120-160">Deploy hello template by using PowerShell</span></span>
<span data-ttu-id="41120-161">a letöltött PowerShell, toodeploy hello sablon PowerShell telepítése és konfigurálása a hello hello lépések elvégzésével [PowerShell telepítése és konfigurálása](/powershell/azure/overview) a következő cikket, és kövesse az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="41120-161">toodeploy hello template you downloaded by using PowerShell, install and configure PowerShell by completing hello steps in hello [Install and configure PowerShell](/powershell/azure/overview) article and then complete hello following steps:</span></span>

<span data-ttu-id="41120-162">Futtassa a hello  **`New-AzureRmResourceGroup`**  erőforrás csoport használatával parancsmag toocreate hello sablont.</span><span class="sxs-lookup"><span data-stu-id="41120-162">Run hello **`New-AzureRmResourceGroup`** cmdlet toocreate a resource group using hello template.</span></span>

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

<span data-ttu-id="41120-163">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="41120-163">Expected output:</span></span>

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="41120-164">Hello sablon üzembe helyezése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="41120-164">Deploy hello template by using hello Azure CLI</span></span>
<span data-ttu-id="41120-165">toodeploy hello sablon hello Azure parancssori felület használatával kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="41120-165">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="41120-166">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="41120-166">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="41120-167">Futtassa a hello  **`azure config mode`**  tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="41120-167">Run hello **`azure config mode`** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="41120-168">hello várható kimenet követi:</span><span class="sxs-lookup"><span data-stu-id="41120-168">hello expected output follows:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="41120-169">Nyissa meg hello [paraméterfájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), válassza ki annak tartalmát, és mentse azokat a számítógép tooa fájlt.</span><span class="sxs-lookup"><span data-stu-id="41120-169">Open hello [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), select its contents, and save it tooa file in your computer.</span></span> <span data-ttu-id="41120-170">Ehhez a példához mentettük hello paraméterfájl túl*parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="41120-170">For this example, we saved hello parameters file too*parameters.json*.</span></span>
4. <span data-ttu-id="41120-171">Futtassa a hello  **`azure group deployment create`**  parancsmag toodeploy hello hello sablonnal és paraméterfájlokkal segítségével új virtuális hálózat fájlokat fent letöltött és módosított.</span><span class="sxs-lookup"><span data-stu-id="41120-171">Run hello **`azure group deployment create`** cmdlet toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="41120-172">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="41120-172">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    <span data-ttu-id="41120-173">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="41120-173">Expected output:</span></span>
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

