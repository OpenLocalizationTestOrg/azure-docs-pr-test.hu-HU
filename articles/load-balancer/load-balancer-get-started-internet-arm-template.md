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
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Internetkapcsolattal rendelkező terheléselosztó létrehozása sablon használatával

> [!div class="op_single_selector"]
> * [Portál](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó klasszikus telepítési modell segítségével](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Hello sablon üzembe helyezése a toodeploy kattintson

hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ. toodeploy toodeploy, a sablon használatával kattintson kövesse [erre a hivatkozásra](http://go.microsoft.com/fwlink/?LinkId=544801), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek szükség és hello portal hello utasításait kövesse.

## <a name="deploy-hello-template-by-using-powershell"></a>Hello sablon üzembe helyezése a PowerShell használatával

toodeploy hello sablon PowerShell használatával hajtsa végre az alábbi hello lépéseket.

1. Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.
2. Futtassa a hello **New-AzureRmResourceGroupDeployment** erőforrás csoport használatával parancsmag toocreate hello sablont.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
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

3. A böngészőből keresse túl[gyorsindítási sablonon hello](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), hello hello json-fájlt másolja és illessze be egy új fájlt a számítógépen. Ebben a forgatókönyvben meg kellene másolni, hello értékek nevű tooa fájl alábbi **c:\lb\azuredeploy.parameters.json**.
4. Futtassa a hello **azure-csoportok központi telepítés** parancsmag toodeploy hello új terheléselosztó hello sablonnal és paraméterfájlokkal használatával, a fent letöltött és módosított fájlok. hello kimenet után látható hello lista hello paramétereket ismerteti.

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
