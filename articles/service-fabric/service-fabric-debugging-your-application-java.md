---
title: "Az eclipse-ben az Azure Service Fabric-alkalmazás hibakeresése |} Microsoft Docs"
description: "Tovább fejlesztheti megbízhatóságának és teljesítményének a szolgáltatások fejlesztéséhez és a helyi fejlesztési fürtök Hibakeresés az eclipse-ben őket."
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
ms.openlocfilehash: f3bcee3794de35005bd387ecfae7e6707f3cb5ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a><span data-ttu-id="12aaa-103">Az Eclipse használata Java Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="12aaa-103">Debug your Java Service Fabric application using Eclipse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="12aaa-104">A Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="12aaa-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="12aaa-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="12aaa-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
> 

1. <span data-ttu-id="12aaa-106">Indítsa el a helyi fejlesztési fürtök lépéseit követve [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="12aaa-106">Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started-linux.md).</span></span>

2. <span data-ttu-id="12aaa-107">A szolgáltatás kívánt debug, úgy, hogy ez a java-folyamat távoli hibakeresési paraméterekkel entryPoint.sh frissítése.</span><span class="sxs-lookup"><span data-stu-id="12aaa-107">Update entryPoint.sh of the service you wish to debug, so that it starts the java process with remote debug parameters.</span></span> <span data-ttu-id="12aaa-108">Ez a fájl a következő helyen található: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span><span class="sxs-lookup"><span data-stu-id="12aaa-108">This file can be found at the following location: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span></span> <span data-ttu-id="12aaa-109">Ebben a példában a hibakeresés port 8001 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="12aaa-109">Port 8001 is set for debugging in this example.</span></span>

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. <span data-ttu-id="12aaa-110">Az Application Manifest frissítése a példányainak számát vagy a szolgáltatást, amely az írásával a replika száma 1 értékre állításával.</span><span class="sxs-lookup"><span data-stu-id="12aaa-110">Update the Application Manifest by setting the instance count or the replica count for the service that is being debugged to 1.</span></span> <span data-ttu-id="12aaa-111">Ez a beállítás elkerülhető ütközések a hibakereséshez használt port.</span><span class="sxs-lookup"><span data-stu-id="12aaa-111">This setting avoids conflicts for the port that is used for debugging.</span></span> <span data-ttu-id="12aaa-112">Például állapotmentes szolgáltatások esetén állítsa ``InstanceCount="1"`` és állapotalapú szolgáltatások állítja be a cél és a replikakészlet minimális mérete 1 az alábbiak szerint állíthatja: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span><span class="sxs-lookup"><span data-stu-id="12aaa-112">For example, for stateless services, set ``InstanceCount="1"`` and for stateful services set the target and min replica set sizes to 1 as follows: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span></span>

4. <span data-ttu-id="12aaa-113">Az alkalmazás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="12aaa-113">Deploy the application.</span></span>

5. <span data-ttu-id="12aaa-114">Válassza ki az Eclipse IDE **Futtatás Debug konfigurációk -> -> Java-alkalmazások és a kapcsolat tulajdonságai bemeneti** és a tulajdonságok az alábbiak szerint állíthatja:</span><span class="sxs-lookup"><span data-stu-id="12aaa-114">In the Eclipse IDE, select **Run -> Debug Configurations -> Remote Java Application and input connection properties** and set the properties as follows:</span></span>

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  <span data-ttu-id="12aaa-115">Állítson be töréspontokat a kívánt pontokon, és az alkalmazás hibakeresése.</span><span class="sxs-lookup"><span data-stu-id="12aaa-115">Set breakpoints at desired points and debug the application.</span></span>

<span data-ttu-id="12aaa-116">Ha az alkalmazás összeomló, is érdemes lehet coredumps engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="12aaa-116">If the application is crashing, you may also want to enable coredumps.</span></span> <span data-ttu-id="12aaa-117">Végrehajtás ``ulimit -c`` a rendszerhéj, és ha 0 értéket adja vissza, majd coredumps nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="12aaa-117">Execute ``ulimit -c`` in a shell and if it returns 0, then coredumps are not enabled.</span></span> <span data-ttu-id="12aaa-118">Ahhoz, hogy a korlátlan coredumps, hajtsa végre a következő parancsot: ``ulimit -c unlimited``.</span><span class="sxs-lookup"><span data-stu-id="12aaa-118">To enable unlimited coredumps, execute the following command: ``ulimit -c unlimited``.</span></span> <span data-ttu-id="12aaa-119">Azt is ellenőrizheti a folyamat állapotát a parancs ``ulimit -a``.</span><span class="sxs-lookup"><span data-stu-id="12aaa-119">You can also verify the status using the command ``ulimit -a``.</span></span>  <span data-ttu-id="12aaa-120">Ha szeretne frissíteni a coredump generációs elérési útja, a végrehajtást ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span><span class="sxs-lookup"><span data-stu-id="12aaa-120">If you wanted to update the coredump generation path, execute ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span></span> 

### <a name="next-steps"></a><span data-ttu-id="12aaa-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12aaa-121">Next steps</span></span>

* <span data-ttu-id="12aaa-122">[Linux Azure Diagnostics használatával naplógyűjtéshez](service-fabric-diagnostics-how-to-setup-lad.md).</span><span class="sxs-lookup"><span data-stu-id="12aaa-122">[Collect logs using Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span></span>
* <span data-ttu-id="12aaa-123">[Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span><span class="sxs-lookup"><span data-stu-id="12aaa-123">[Monitor and diagnose services locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span></span>
