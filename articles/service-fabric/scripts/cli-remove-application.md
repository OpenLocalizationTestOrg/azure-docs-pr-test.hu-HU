---
title: "Service Fabric CLI parancsfájl eltávolítása minta aaaAzure"
description: "Alkalmazások eltávolítása egy Azure Service Fabric-fürt hello Azure Service Fabric parancssori felület használatával"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 3ccefd4a04c5b7af71a2f959e11da6e402f25881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Alkalmazások eltávolítása a Service Fabric-fürt

Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány törli, az alkalmazástípus és -verzió hello fürtből regisztrációjának törlése.  Törlése hello alkalmazáspéldány fut az adott alkalmazáshoz kapcsolódó szolgáltatáspéldány összes hello is törli. A következő hello alkalmazásfájlok hello lemezképtárolóhoz lesznek törölve. 

Szükség esetén telepítse hello [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Mintaparancsfájl

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Következő lépések

További információkért lásd: hello [Service Fabric CLI dokumentáció](../service-fabric-cli.md).

Azure Service Fabric további Service Fabric CLI-példák találhatók hello [Service Fabric CLI minták](../samples-cli.md).
