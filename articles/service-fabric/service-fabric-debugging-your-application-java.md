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
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Az Eclipse használata Java Service Fabric-alkalmazás hibakeresése
> [!div class="op_single_selector"]
> * [A Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Indítsa el a helyi fejlesztési fürtök hello utasításait követve [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started-linux.md).

2. Frissítse a entryPoint.sh hello szolgáltatást szeretné toodebug, így távoli hibakeresési paraméterek hello java folyamat kezdődik. Ez a fájl hello helye a következő helyen találhatók: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Ebben a példában a hibakeresés port 8001 van beállítva.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Application Manifest hello frissítse úgy, hogy hello példányszám vagy hello replikaszám hello szolgáltatás, amely alatt indítja too1. Ez a beállítás elkerülhető ütközéssel rendelkezik hello hibakereséshez használt port. Például állapotmentes szolgáltatások esetén állítsa ``InstanceCount="1"`` és állapotalapú szolgáltatások set hello cél- és min replika méretét too1 az alábbiak szerint állíthatja: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Hello alkalmazás központi telepítése.

5. Hello Eclipse IDE, válassza ki **Futtatás Debug konfigurációk -> -> Java-alkalmazások és kapcsolat tulajdonságai bemeneti** és az alábbiak szerint állíthatja hello tulajdonságok:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Állítson be töréspontokat a kívánt pontokon és hello alkalmazás hibakeresése.

Ha hello alkalmazás összeomló, tooenable coredumps is érdemes lehet. Végrehajtás ``ulimit -c`` a rendszerhéj, és ha 0 értéket adja vissza, majd coredumps nem engedélyezettek. tooenable korlátlan coredumps hajtható végre a következő parancs hello: ``ulimit -c unlimited``. Azt is ellenőrizheti hello paranccsal hello állapot ``ulimit -a``.  Ha tooupdate hello coredump generációs elérési útja, végrehajtás ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Következő lépések

* [Linux Azure Diagnostics használatával naplógyűjtéshez](service-fabric-diagnostics-how-to-setup-lad.md).
* [Figyelése és diagnosztizálása helyileg szolgáltatások](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
