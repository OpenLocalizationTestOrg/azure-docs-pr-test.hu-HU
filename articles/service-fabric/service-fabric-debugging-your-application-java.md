---
title: "aaaDebug az Azure Service Fabric-alkalmazás az eclipse-ben |} Microsoft Docs"
description: "Tovább fejlesztheti hello megbízhatóságának és teljesítményének a szolgáltatások fejlesztéséhez és a helyi fejlesztési fürtök Hibakeresés az eclipse-ben őket."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: ab86254a5c312db40fd631746c89aab0bbb9d1a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a><span data-ttu-id="51b7d-103">Az Eclipse használata Java Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="51b7d-103">Debug your Java Service Fabric application using Eclipse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="51b7d-104">A Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="51b7d-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="51b7d-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="51b7d-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
> 

1. <span data-ttu-id="51b7d-106">Indítsa el a helyi fejlesztési fürtök hello utasításait követve [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="51b7d-106">Start a local development cluster by following hello steps in [Setting up your Service Fabric development environment](service-fabric-get-started-linux.md).</span></span>

2. <span data-ttu-id="51b7d-107">Frissítse a entryPoint.sh hello szolgáltatást szeretné toodebug, így távoli hibakeresési paraméterek hello java folyamat kezdődik.</span><span class="sxs-lookup"><span data-stu-id="51b7d-107">Update entryPoint.sh of hello service you wish toodebug, so that it starts hello java process with remote debug parameters.</span></span> <span data-ttu-id="51b7d-108">Ez a fájl hello helye a következő helyen találhatók: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span><span class="sxs-lookup"><span data-stu-id="51b7d-108">This file can be found at hello following location: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span></span> <span data-ttu-id="51b7d-109">Ebben a példában a hibakeresés port 8001 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="51b7d-109">Port 8001 is set for debugging in this example.</span></span>

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. <span data-ttu-id="51b7d-110">Application Manifest hello frissítse úgy, hogy hello példányszám vagy hello replikaszám hello szolgáltatás, amely alatt indítja too1.</span><span class="sxs-lookup"><span data-stu-id="51b7d-110">Update hello Application Manifest by setting hello instance count or hello replica count for hello service that is being debugged too1.</span></span> <span data-ttu-id="51b7d-111">Ez a beállítás elkerülhető ütközéssel rendelkezik hello hibakereséshez használt port.</span><span class="sxs-lookup"><span data-stu-id="51b7d-111">This setting avoids conflicts for hello port that is used for debugging.</span></span> <span data-ttu-id="51b7d-112">Például állapotmentes szolgáltatások esetén állítsa ``InstanceCount="1"`` és állapotalapú szolgáltatások set hello cél- és min replika méretét too1 az alábbiak szerint állíthatja: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span><span class="sxs-lookup"><span data-stu-id="51b7d-112">For example, for stateless services, set ``InstanceCount="1"`` and for stateful services set hello target and min replica set sizes too1 as follows: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span></span>

4. <span data-ttu-id="51b7d-113">Hello alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="51b7d-113">Deploy hello application.</span></span>

5. <span data-ttu-id="51b7d-114">Hello Eclipse IDE, válassza ki **Futtatás Debug konfigurációk -> -> Java-alkalmazások és kapcsolat tulajdonságai bemeneti** és az alábbiak szerint állíthatja hello tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="51b7d-114">In hello Eclipse IDE, select **Run -> Debug Configurations -> Remote Java Application and input connection properties** and set hello properties as follows:</span></span>

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  <span data-ttu-id="51b7d-115">Állítson be töréspontokat a kívánt pontokon és hello alkalmazás hibakeresése.</span><span class="sxs-lookup"><span data-stu-id="51b7d-115">Set breakpoints at desired points and debug hello application.</span></span>

<span data-ttu-id="51b7d-116">Ha hello alkalmazás összeomló, tooenable coredumps is érdemes lehet.</span><span class="sxs-lookup"><span data-stu-id="51b7d-116">If hello application is crashing, you may also want tooenable coredumps.</span></span> <span data-ttu-id="51b7d-117">Végrehajtás ``ulimit -c`` a rendszerhéj, és ha 0 értéket adja vissza, majd coredumps nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="51b7d-117">Execute ``ulimit -c`` in a shell and if it returns 0, then coredumps are not enabled.</span></span> <span data-ttu-id="51b7d-118">tooenable korlátlan coredumps hajtható végre a következő parancs hello: ``ulimit -c unlimited``.</span><span class="sxs-lookup"><span data-stu-id="51b7d-118">tooenable unlimited coredumps, execute hello following command: ``ulimit -c unlimited``.</span></span> <span data-ttu-id="51b7d-119">Azt is ellenőrizheti hello paranccsal hello állapot ``ulimit -a``.</span><span class="sxs-lookup"><span data-stu-id="51b7d-119">You can also verify hello status using hello command ``ulimit -a``.</span></span>  <span data-ttu-id="51b7d-120">Ha tooupdate hello coredump generációs elérési útja, végrehajtás ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span><span class="sxs-lookup"><span data-stu-id="51b7d-120">If you wanted tooupdate hello coredump generation path, execute ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span></span> 

### <a name="next-steps"></a><span data-ttu-id="51b7d-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51b7d-121">Next steps</span></span>

* <span data-ttu-id="51b7d-122">[Linux Azure Diagnostics használatával naplógyűjtéshez](service-fabric-diagnostics-how-to-setup-lad.md).</span><span class="sxs-lookup"><span data-stu-id="51b7d-122">[Collect logs using Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span></span>
* <span data-ttu-id="51b7d-123">[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span><span class="sxs-lookup"><span data-stu-id="51b7d-123">[Monitor and diagnose services locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span></span>
