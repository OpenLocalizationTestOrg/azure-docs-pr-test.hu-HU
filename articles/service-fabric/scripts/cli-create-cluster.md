---
title: "Az Azure Service Fabric CLI parancsfájl mintaalkalmazás telepítését."
description: "Biztonságos Linux a Service Fabric-fürt létrehozása az Azure-ban az Azure Service Fabric parancssori felület."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: d49383b4f7b3b13beb9ea36ae725938e17ef1456
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Biztonságos Linux a Service Fabric-fürt létrehozása az Azure-ban

Ez a parancs létrehoz egy önaláírt tanúsítványt, felveszi a kulcstároló, és letölti a tanúsítványt a helyi.  A fürt védelmét, amikor telepíti az új tanúsítvány használatos.  Új létrehozása helyett használhatja a meglévő tanúsítvány.  Mindkét módszer esetén a tanúsítvány tulajdonosának nevét meg kell egyeznie a tartományt, amelyikhez a Service Fabric-fürt eléréséhez használt. A megfelelés szükség az SSL protokoll a fürt HTTPS felügyeleti végpontokat és a Service Fabric Explorerben talál. Nem szerezzen be egy SSL-tanúsítványt egy hitelesítésszolgáltatóból a `.cloudapp.azure.com` tartomány. Be kell szereznie egy egyéni tartománynevet a fürt számára. A hitelesítésszolgáltató tanúsítvány kérése, amikor a tanúsítvány tulajdonosának nevét meg kell egyeznie az egyéni tartománynevet, amelyekkel a fürt számára.

Szükség esetén telepítse a [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Mintaparancsfájl

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoport, a fürt és az összes kapcsolódó erőforrások.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Az ú fürt létrehozása](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Egy új Service Fabric-fürtöt hoz létre.  |

## <a name="next-steps"></a>További lépések

Azure Service Fabric további Service Fabric CLI-példák találhatók a [Service Fabric CLI minták](../samples-cli.md).
