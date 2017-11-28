---
title: "egy belső terheléselosztó - sablon Azure aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó sablon az erőforrás-kezelő használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 64150862-6ced-42de-85dc-89d323257d7c
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3ffa8178b863367cd79e2bc2b7ce4e45b23267e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-a-template"></a><span data-ttu-id="a249b-103">Belső terheléselosztó létrehozása sablon használatával</span><span class="sxs-lookup"><span data-stu-id="a249b-103">Create an internal load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a249b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a249b-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="a249b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a249b-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="a249b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a249b-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="a249b-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="a249b-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="a249b-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a249b-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="a249b-109">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a249b-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="a249b-110">Hello sablon üzembe helyezése a toodeploy kattintson</span><span class="sxs-lookup"><span data-stu-id="a249b-110">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="a249b-111">hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="a249b-111">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="a249b-112">toodeploy toodeploy, a sablon használatával kattintson kövesse [erre a hivatkozásra](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek szükség és hello portal hello utasításait kövesse.</span><span class="sxs-lookup"><span data-stu-id="a249b-112">toodeploy this template using click toodeploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="a249b-113">Hello sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a249b-113">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="a249b-114">toodeploy hello sablon PowerShell használatával hajtsa végre az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a249b-114">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="a249b-115">Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="a249b-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="a249b-116">Töltse le a hello paraméterek tooyour helyi lemezen.</span><span class="sxs-lookup"><span data-stu-id="a249b-116">Download hello parameters file tooyour local disk.</span></span>
3. <span data-ttu-id="a249b-117">Hello fájl szerkesztésével, és mentse.</span><span class="sxs-lookup"><span data-stu-id="a249b-117">Edit hello file and save it.</span></span>
4. <span data-ttu-id="a249b-118">Futtassa a hello **New-AzureRmResourceGroupDeployment** erőforrás csoport használatával parancsmag toocreate hello sablont.</span><span class="sxs-lookup"><span data-stu-id="a249b-118">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toocreate a resource group using hello template.</span></span>

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="a249b-119">Hello sablon üzembe helyezése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a249b-119">Deploy hello template by using hello Azure CLI</span></span>

<span data-ttu-id="a249b-120">toodeploy hello sablon hello Azure parancssori felület használatával kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a249b-120">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="a249b-121">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="a249b-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="a249b-122">Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="a249b-122">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="a249b-123">Itt egy hello parancs fenti hello várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="a249b-123">Here is hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="a249b-124">Nyissa meg a hello paraméterfájl, válassza ki annak tartalmát, és mentse azokat a számítógép tooa fájl.</span><span class="sxs-lookup"><span data-stu-id="a249b-124">Open hello parameter file, select its contents, and save it tooa file in your computer.</span></span> <span data-ttu-id="a249b-125">Ehhez a példához mentettük hello paraméterfájl túl*parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="a249b-125">For this example, we saved hello parameters file too*parameters.json*.</span></span>
4. <span data-ttu-id="a249b-126">Futtassa a hello **azure-csoportok központi telepítés** parancs toodeploy új belső terheléselosztó hello hello sablonnal és paraméterfájlokkal használatával, a fent letöltött és módosított fájlok.</span><span class="sxs-lookup"><span data-stu-id="a249b-126">Run hello **azure group deployment create** command toodeploy hello new internal load balancer by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="a249b-127">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a249b-127">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a><span data-ttu-id="a249b-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a249b-128">Next steps</span></span>

[<span data-ttu-id="a249b-129">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="a249b-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="a249b-130">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a249b-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

