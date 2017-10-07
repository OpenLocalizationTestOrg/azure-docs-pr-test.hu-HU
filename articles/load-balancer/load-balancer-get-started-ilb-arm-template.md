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
# <a name="create-an-internal-load-balancer-using-a-template"></a>Belső terheléselosztó létrehozása sablon használatával

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Hello sablon üzembe helyezése a toodeploy kattintson

hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ. toodeploy toodeploy, a sablon használatával kattintson kövesse [erre a hivatkozásra](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek szükség és hello portal hello utasításait kövesse.

## <a name="deploy-hello-template-by-using-powershell"></a>Hello sablon üzembe helyezése a PowerShell használatával

toodeploy hello sablon PowerShell használatával hajtsa végre az alábbi hello lépéseket.

1. Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.
2. Töltse le a hello paraméterek tooyour helyi lemezen.
3. Hello fájl szerkesztésével, és mentse.
4. Futtassa a hello **New-AzureRmResourceGroupDeployment** erőforrás csoport használatával parancsmag toocreate hello sablont.

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Hello sablon üzembe helyezése hello Azure parancssori felület használatával

toodeploy hello sablon hello Azure parancssori felület használatával kövesse hello lépéseket.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.

    ```azurecli
    azure config mode arm
    ```

    Itt egy hello parancs fenti hello várt kimenet:

        info:    New mode is arm

3. Nyissa meg a hello paraméterfájl, válassza ki annak tartalmát, és mentse azokat a számítógép tooa fájl. Ehhez a példához mentettük hello paraméterfájl túl*parameters.json*.
4. Futtassa a hello **azure-csoportok központi telepítés** parancs toodeploy új belső terheléselosztó hello hello sablonnal és paraméterfájlokkal használatával, a fent letöltött és módosított fájlok. hello kimenet után látható hello lista hello paramétereket ismerteti.

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

