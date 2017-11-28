---
title: "egy internetre irányuló aaaCreate terheléselosztó - sablon Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre irányuló terheléselosztót a Resource Manager-sablon használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: b24f4729-4559-4458-8527-71009d242647
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 2bce8cb87303838f3bc732d51228ab46d8015552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a><span data-ttu-id="2cb5e-103">Internetkapcsolattal rendelkező terheléselosztó létrehozása sablon használatával</span><span class="sxs-lookup"><span data-stu-id="2cb5e-103">Creating an Internet facing load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2cb5e-104">Portál</span><span class="sxs-lookup"><span data-stu-id="2cb5e-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="2cb5e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2cb5e-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="2cb5e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2cb5e-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="2cb5e-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="2cb5e-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="2cb5e-108">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="2cb5e-109">Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó klasszikus telepítési modell segítségével](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2cb5e-109">You can also [Learn how toocreate an Internet facing load balancer using classic deployment model](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="2cb5e-110">Hello sablon üzembe helyezése a toodeploy kattintson</span><span class="sxs-lookup"><span data-stu-id="2cb5e-110">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="2cb5e-111">hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-111">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="2cb5e-112">toodeploy toodeploy, a sablon használatával kattintson kövesse [erre a hivatkozásra](http://go.microsoft.com/fwlink/?LinkId=544801), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek szükség és hello portal hello utasításait kövesse.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-112">toodeploy this template using click toodeploy, follow [this link](http://go.microsoft.com/fwlink/?LinkId=544801), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="2cb5e-113">Hello sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2cb5e-113">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="2cb5e-114">toodeploy hello sablon PowerShell használatával hajtsa végre az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-114">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="2cb5e-115">Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="2cb5e-116">Futtassa a hello **New-AzureRmResourceGroupDeployment** erőforrás csoport használatával parancsmag toocreate hello sablont.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-116">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toocreate a resource group using hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="2cb5e-117">Hello sablon üzembe helyezése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="2cb5e-117">Deploy hello template by using hello Azure CLI</span></span>

<span data-ttu-id="2cb5e-118">toodeploy hello sablon hello Azure parancssori felület használatával kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-118">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="2cb5e-119">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-119">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="2cb5e-120">Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-120">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="2cb5e-121">Itt egy hello parancs fenti hello várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2cb5e-121">Here is hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="2cb5e-122">A böngészőből keresse túl[gyorsindítási sablonon hello](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), hello hello json-fájlt másolja és illessze be egy új fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-122">From your browser, navigate too[hello Quickstart Template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), copy hello contents of hello json file and paste into a new file in your computer.</span></span> <span data-ttu-id="2cb5e-123">Ebben a forgatókönyvben meg kellene másolni, hello értékek nevű tooa fájl alábbi **c:\lb\azuredeploy.parameters.json**.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-123">For this scenario, you would be copying hello values below tooa file named **c:\lb\azuredeploy.parameters.json**.</span></span>
4. <span data-ttu-id="2cb5e-124">Futtassa a hello **azure-csoportok központi telepítés** parancsmag toodeploy hello új terheléselosztó hello sablonnal és paraméterfájlokkal használatával, a fent letöltött és módosított fájlok.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-124">Run hello **azure group deployment create** cmdlet toodeploy hello new load balancer by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="2cb5e-125">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2cb5e-125">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a><span data-ttu-id="2cb5e-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2cb5e-126">Next steps</span></span>

[<span data-ttu-id="2cb5e-127">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="2cb5e-127">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="2cb5e-128">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2cb5e-128">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="2cb5e-129">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2cb5e-129">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
