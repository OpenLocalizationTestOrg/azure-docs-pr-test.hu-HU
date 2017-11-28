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
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a><span data-ttu-id="0c6d2-103">Alkalmazás tooa Service Fabric-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0c6d2-103">Deploy an application tooa Service Fabric cluster</span></span>

<span data-ttu-id="0c6d2-104">Ez a parancsfájlpélda másolja át az alkalmazás csomag tooa fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása hello fürt, és létrehoz egy alkalmazáspéldányt hello alkalmazás típusa.</span><span class="sxs-lookup"><span data-stu-id="0c6d2-104">This sample script copies an application package tooa cluster image store, registers hello application type in hello cluster, and creates an application instance from hello application type.</span></span> <span data-ttu-id="0c6d2-105">Alapértelmezett szolgáltatások is létrejön, most.</span><span class="sxs-lookup"><span data-stu-id="0c6d2-105">Any default services are also created at this time.</span></span>

<span data-ttu-id="0c6d2-106">Szükség esetén telepítse hello [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0c6d2-106">If needed, install hello [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="0c6d2-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="0c6d2-107">Sample script</span></span>

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0c6d2-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0c6d2-108">Clean up deployment</span></span>

<span data-ttu-id="0c6d2-109">Ha befejezte, hello [eltávolítása](cli-remove-application.md) parancsfájl használt tooremove hello alkalmazás lehet.</span><span class="sxs-lookup"><span data-stu-id="0c6d2-109">When done, hello [remove](cli-remove-application.md) script can be used tooremove hello application.</span></span> <span data-ttu-id="0c6d2-110">hello eltávolítása parancsfájl hello alkalmazáspéldány törli, hello alkalmazástípus regisztrációjának törlése és az image store hello alkalmazáscsomag törlése.</span><span class="sxs-lookup"><span data-stu-id="0c6d2-110">hello remove script deletes hello application instance, unregisters hello application type, and deletes hello application package from the image store.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c6d2-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c6d2-111">Next steps</span></span>

<span data-ttu-id="0c6d2-112">További információkért lásd: hello [Service Fabric CLI dokumentáció](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0c6d2-112">For more information, see hello [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="0c6d2-113">Azure Service Fabric további Service Fabric CLI-példák találhatók hello [Service Fabric CLI minták](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0c6d2-113">Additional Service Fabric CLI samples for Azure Service Fabric can be found in hello [Service Fabric CLI samples](../samples-cli.md).</span></span>
