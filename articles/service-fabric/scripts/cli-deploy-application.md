---
title: "Service Fabric CLI parancsfájl központi telepítése minta aaaAzure"
description: "Alkalmazás tooan Azure Service Fabric-fürt üzembe helyezése hello Azure Service Fabric parancssori felület használatával"
services: service-fabric
documentationcenter: 
author: Thraka
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
ms.openlocfilehash: aaec7042a4fd7ed32ad706cde70361f23d18fb48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a>Alkalmazás tooa Service Fabric-fürt üzembe helyezése

Ez a parancsfájlpélda másolja át az alkalmazás csomag tooa fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása hello fürt, és létrehoz egy alkalmazáspéldányt hello alkalmazás típusa. Alapértelmezett szolgáltatások is létrejön, most.

Szükség esetén telepítse hello [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Mintaparancsfájl

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

Ha befejezte, hello [eltávolítása](cli-remove-application.md) parancsfájl használt tooremove hello alkalmazás lehet. hello eltávolítása parancsfájl hello alkalmazáspéldány törli, hello alkalmazástípus regisztrációjának törlése és az image store hello alkalmazáscsomag törlése.

## <a name="next-steps"></a>Következő lépések

További információkért lásd: hello [Service Fabric CLI dokumentáció](../service-fabric-cli.md).

Azure Service Fabric további Service Fabric CLI-példák találhatók hello [Service Fabric CLI minták](../samples-cli.md).
