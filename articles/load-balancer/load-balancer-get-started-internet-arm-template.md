---
title: "Internetkapcsolattal rendelkező terheléselosztó létrehozása – Azure-sablon | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó a Resource Managerben sablon használatával"
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
ms.openlocfilehash: d829000e63515814b192f3f8256e3b8637bb3a34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a><span data-ttu-id="0afb8-103">Internetkapcsolattal rendelkező terheléselosztó létrehozása sablon használatával</span><span class="sxs-lookup"><span data-stu-id="0afb8-103">Creating an Internet facing load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0afb8-104">Portál</span><span class="sxs-lookup"><span data-stu-id="0afb8-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="0afb8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0afb8-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="0afb8-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0afb8-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="0afb8-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="0afb8-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="0afb8-108">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0afb8-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="0afb8-109">Emellett [azt is megismerheti, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó a klasszikus üzemi modell használatával](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="0afb8-109">You can also [Learn how to create an Internet facing load balancer using classic deployment model](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="0afb8-110">A sablon üzembe helyezése kattintással végrehajtható üzembe helyezéssel</span><span class="sxs-lookup"><span data-stu-id="0afb8-110">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="0afb8-111">A nyilvános tárházban elérhető mintasablon a fent leírt forgatókönyv létrehozásához használt alapértelmezett értékeket tartalmazó paraméterfájlt használja.</span><span class="sxs-lookup"><span data-stu-id="0afb8-111">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="0afb8-112">Ha a sablon üzembe helyezését kattintással végrehajtható üzembe helyezéssel szeretné elvégezni, kövesse [ezt a hivatkozást](http://go.microsoft.com/fwlink/?LinkId=544801), kattintson az **Üzembe helyezés az Azure-on** lehetőségre, cserélje ki az alapértelmezett paraméterértékeket, ha szükséges, majd kövesse a portálon megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="0afb8-112">To deploy this template using click to deploy, follow [this link](http://go.microsoft.com/fwlink/?LinkId=544801), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="0afb8-113">A sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="0afb8-113">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="0afb8-114">A letöltött sablon PowerShell használatával történő üzembe helyezéséhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0afb8-114">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="0afb8-115">Ha még nem használta az Azure PowerShellt, tekintse meg [How to Install and Configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című részt, majd kövesse az utasításokat egészen az utolsó lépésig az Azure-ba való bejelentkezéshez és az előfizetése kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="0afb8-115">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="0afb8-116">A **New-AzureRmResourceGroupDeployment** parancsmag futtatásával és a sablon használatával hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="0afb8-116">Run the **New-AzureRmResourceGroupDeployment** cmdlet to create a resource group using the template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="0afb8-117">A sablon üzembe helyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="0afb8-117">Deploy the template by using the Azure CLI</span></span>

<span data-ttu-id="0afb8-118">Az alábbi lépéseket követve hozhatja létre a sablont az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="0afb8-118">To deploy the template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="0afb8-119">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="0afb8-119">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="0afb8-120">Az **azure config mode** parancs futtatásával váltson az Erőforrás-kezelő módra, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="0afb8-120">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="0afb8-121">A fenti parancs várható kimenete:</span><span class="sxs-lookup"><span data-stu-id="0afb8-121">Here is the expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="0afb8-122">Lépjen a böngészőből a [Gyorssablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules) lapra, másolja át és illessze be a json-fájl tartalmát egy új fájlba a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="0afb8-122">From your browser, navigate to [the Quickstart Template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), copy the contents of the json file and paste into a new file in your computer.</span></span> <span data-ttu-id="0afb8-123">Ehhez a forgatókönyvhöz az alábbi értékeket át kell másolnia egy **c:\lb\azuredeploy.parameters.json** nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="0afb8-123">For this scenario, you would be copying the values below to a file named **c:\lb\azuredeploy.parameters.json**.</span></span>
4. <span data-ttu-id="0afb8-124">Futtassa az **azure group deployment create** parancsmagot, hogy a fent letöltött és módosított sablonnal és paraméterfájlokkal üzembe helyezhesse az új terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="0afb8-124">Run the **azure group deployment create** cmdlet to deploy the new load balancer by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="0afb8-125">A kimenet után látható lista ismerteti a használt paramétereket.</span><span class="sxs-lookup"><span data-stu-id="0afb8-125">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a><span data-ttu-id="0afb8-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0afb8-126">Next steps</span></span>

[<span data-ttu-id="0afb8-127">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="0afb8-127">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="0afb8-128">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0afb8-128">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="0afb8-129">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0afb8-129">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
