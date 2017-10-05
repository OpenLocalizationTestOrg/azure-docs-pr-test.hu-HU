---
title: "Az Azure Service Fabric önálló fürt konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az önálló vagy titkos Service Fabric-fürt."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dekapur
ms.openlocfilehash: 9885dce18dabac4a945dafd219e3ae190e34a83b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Önálló Windows-fürt konfigurációs beállításai
Ez a cikk ismerteti, hogyan konfigurálhatja egy különálló Service Fabric fürt használatával a ***művelet*** fájlt. Ez a fájl határozza meg a Service Fabric-csomópont és az IP-címek, a csomópontok különböző típusú vonatkozó információkat a fürt, a biztonsági beállításokkal, valamint a hálózati topológia hiba/frissítési tartományok tekintetében az önálló fürthöz használható.

Ha Ön [a különálló Service Fabric-csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a művelet fájl néhány minták töltődnek le a munkahelyi számítógép. A minták *DevCluster* útra segít hozzon létre egy fürtöt ugyanazon a számítógépen, például a logikai csomópontok három csomópontjaihoz. Ezeken kívül legalább egy csomópont jelölésűnek kell lennie egy elsődleges csomóponton. A fürt akkor hasznos, ha egy fejlesztési vagy tesztelési környezetben, és egy éles fürt nem támogatott. A minták *MultiMachine* a név segítségével hozhat létre egy éles minőségi fürt minden csomópont egy külön számítógépen.

1. *ClusterConfig.Unsecure.DevCluster.JSON* és *ClusterConfig.Unsecure.MultiMachine.JSON* egy nem biztonságos teszt- vagy éles fürt rendre létrehozását mutatják be. 
2. *ClusterConfig.Windows.DevCluster.JSON* és *ClusterConfig.Windows.MultiMachine.JSON* használatával biztonságossá teszt- vagy éles fürt létrehozását mutatják be [Windows biztonsági](service-fabric-windows-cluster-windows-security.md).
3. *ClusterConfig.X509.DevCluster.JSON* és *ClusterConfig.X509.MultiMachine.JSON* használatával biztonságossá teszt- vagy éles fürt létrehozását mutatják be [X509-alapú biztonsági](service-fabric-windows-cluster-x509-security.md). 

Most azt megvizsgálja, hogy a különböző részeit egy ***művelet*** alábbi fájlt.

## <a name="general-cluster-configurations"></a>Általános fürtkonfigurációk
Ez hozzá van rendelve a széles körű adott fürtkonfigurációk, ahogy az az alábbi JSON-részlet.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

A Service Fabric-fürt bármely rövid nevet a való hozzárendelésével biztosíthat a **neve** változó. A **clusterConfigurationVersion** a fürt; a verziószám növelése kell azt minden alkalommal, amikor a Service Fabric-fürt frissítése. Azonban hagyja meg az **apiVersion** az alapértelmezett értékre.

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a>A fürt csomópontjai
Konfigurálhatja a csomópontok a Service Fabric-fürt használatával a **csomópontok** területen az alábbi kódrészletben látható módon.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

A Service Fabric-fürt tartalmaznia kell legalább 3 csomópontok. A telepítő szerint több csomópont is hozzáadható ehhez a szakaszhoz. Az alábbi táblázat ismerteti az egyes csomópontok konfigurációs beállításait.

| **A csomópont-konfiguráció** | **Leírás** |
| --- | --- |
| Csomópontnév |Bármilyen rövid nevet adhat a csomópontra. |
| IP-cím |Nyisson meg egy parancsablakot, és írja be az IP-cím, a csomópont található `ipconfig`. Vegye figyelembe az IPV4-címet, majd rendelje hozzá a **IP-cím** változó. |
| nodeTypeRef |Minden csomópont rendelhetők hozzá másik csomóponttípus. A [csomóponttípusok](#nodetypes) határozzák meg a következő szakaszban. |
| faultDomain |Tartalék tartományok lehetővé teszik a rendszergazdák fürtön határozza meg a fizikai csomópontok, amelyek egyszerre megosztott fizikai függőségek miatt meghiúsulhat. |
| upgradeDomain |Frissítési tartományok jellemezhető a csomópontokra, amelyeket nagyjából egy időben, a Service Fabric-frissítések állnak le. Melyik frissítési tartományok hozzárendelése csomópontok dönthet úgy, mint bármely fizikai követelmények nem korlátozza. |

## <a name="cluster-properties"></a>Fürt tulajdonságai
A **tulajdonságok** a művelet a szakasz a fürt az alábbiak szerint konfigurálására szolgál.

<a id="reliability"></a>

### <a name="reliability"></a>Megbízhatóság
A fogalom **reliabilityLevel** replikák száma vagy szolgáltatáspéldánynak a Service Fabric rendszer futtatható a fürt elsődleges csomópontjait határoz meg. Meghatározza, hogy ezek a szolgáltatások megbízhatóságát, így a fürt. A rendszer által számított értéke fürt létrehozása és frissítése során.

### <a name="diagnostics"></a>Diagnosztika
A **diagnosticsStore** szakasz lehetővé teszi a diagnosztika és a hibaelhárítási csomópont vagy a fürt hibák engedélyezése paramétereinek a konfigurálása, ahogy az az alábbi kódrészletet. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

A **metaadatok** a fürt diagnosztika leírása és a telepítő szerint állítható be. Ezek a változók megkönnyíti a ETW nyomkövetési napló- és összeomlási memóriaképeket, valamint teljesítményszámlálók gyűjtése. Olvasási [a követési naplóban](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) és [ETW-nyomkövetés](https://msdn.microsoft.com/library/ms751538.aspx) ETW-vel további információ a nyomkövetési naplóit. Beleértve az összes napló [összeomlási memóriaképek](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) és [teljesítményszámlálók](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) irányítható a **connectionString** mappát a számítógépén. Is *AzureStorage* diagnosztika tárolásához. Lentebb egy minta részlet.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Biztonság
A **biztonsági** szakasz esetén szükség a biztonságos különálló Service Fabric-fürt. Az alábbi kódrészletben láthatja az ebben a szakaszban egy részét.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

A **metaadatok** a biztonságos fürt leírása és a telepítő szerint állítható be. A **ClusterCredentialType** és **ServerCredentialType** határozzák meg, hogy a fürt és a csomópontok megvalósítandó biztonsági típusú. Akkor lehet megadni *X509* egy tanúsítványalapú biztonsági vagy *Windows* egy Azure Active Directory-alapú biztonság. A többi a **biztonsági** szakasz a biztonsági típusú alapul. Olvasási [tanúsítványok-alapú biztonsági önálló fürtben](service-fabric-windows-cluster-x509-security.md) vagy [Windows biztonsági önálló fürtben](service-fabric-windows-cluster-windows-security.md) adja meg a többi olvashat a **biztonsági** a szakasz.

<a id="nodetypes"></a>

### <a name="node-types"></a>Csomóponttípusok
A **NodeType tulajdonságok értéke** szakasz ismerteti, amely rendelkezik a fürt a csomópontok típusú. Fürt esetén kell adni legalább egy csomópont típusát, ahogy az alábbi részlet. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

A **neve** az adott csomópont ilyen rövid neve. A csomópont típusú csomópont létrehozása, hozzárendelése rövid nevét, hogy a **nodeTypeRef** változó az adott csomópont, ahogy azt korábban említettük [fent](#clusternodes). Az egyes csomópont adja meg a kapcsolati végpontok használható. Minden kapcsolat végpontokkal portszáma dönthet úgy, mindaddig, amíg azok nem ütköznek-e a fürt bármely más végpontja. Több csomópontos fürtben, lesz egy vagy több elsődleges csomóponton (pl. **isPrimary** beállítása *igaz*) attól függően, hogy a [ **reliabilityLevel** ](#reliability). Olvasási [Service Fabric fürt kapacitástervezésének szempontjai](service-fabric-cluster-capacity.md) információk **NodeType tulajdonságok értéke** és **reliabilityLevel**, és hogy tudják, mit elsődleges és a nem elsődleges csomóponttípusok. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>A csomóponttípusok konfigurálásához használt végpontok
* *clientConnectionEndpointPort* az kapcsolódik a fürthöz, az ügyfél API-k használata esetén az ügyfél által használt port. 
* *clusterConnectionEndpointPort* a portot, amelyen a csomópontok kommunikálnak egymással.
* *leaseDriverEndpointPort* a portot, ha szeretné tudni, ha a csomópontok még mindig aktív a fürt bérleti illesztőprogram használják. 
* *serviceConnectionEndpointPort* a csomóponton való kommunikációhoz, hogy adott csomóponton a Service Fabric-ügyféllel telepített szolgáltatások és az alkalmazások által használt port.
* *httpGatewayEndpointPort* a portot használják a Service Fabric Explorerben csatlakozzon a fürthöz.
* *ephemeralPorts* bírálja felül a [az operációs rendszer által használt dinamikus portok](https://support.microsoft.com/kb/929851). A Service Fabric ezek alkalmazás portok részét fogja használni, és a fennmaradó lesznek elérhetők az operációs rendszer számára. Azt is felelteti meg ezt a tartományt a meglévő tartomány szerepel az operációs rendszer, így minden célra használhatja a minta JSON-fájlokat a megadott tartományokon. Győződjön meg arról, hogy az a kezdő és záró portokat közötti különbség legalább 255 kell. Ha ezt a különbséget túl alacsony, mivel ez a tartomány meg van osztva az operációs rendszer futtathatja az ütközéseket. Tekintse meg a beállított dinamikus porttartományt futtatásával `netsh int ipv4 show dynamicport tcp`.
* *applicationPorts* legyenek a, amely a Service Fabric-alkalmazások által használt portok. Az alkalmazás porttartományát elég nagynak kell lennie, amelyek a végpont követelmény az alkalmazások. Ebben a tartományban kell lennie azon a számítógépen, a dinamikus port tartományból kizárólagos azaz a *ephemeralPorts* tartomány, ahogyan az a konfiguráció.  A Service Fabric fog használni, ezek új portok szükségesek, valamint gondoskodunk megnyitni ezeket a portokat a tűzfalon. 
* *reverseProxyEndpointPort* a választható fordított proxy végpontja. Lásd: [Service Fabric fordított Proxy](service-fabric-reverseproxy.md) további részleteket. 

### <a name="log-settings"></a>Naplózási beállítások
A **fabricSettings** szakasz lehetővé teszi, hogy meg kell adnia a gyökérkönyvtárak a Service Fabric-adatokat és a naplókat. Testre szabhatja ezek csak a kezdeti fürt létrehozása során. Ez a szakasz egy minta szövegrészletet lentebb.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Azt javasoljuk, hogy a FabricDataRoot és a fabriclogroot mappában az operációs rendszer nem meghajtót használ, szemben az operációs rendszer további megbízhatóság összeomlik biztosít. Vegye figyelembe, hogy csak a adatgyökerében testre, majd a napló legfelső szintű kerülnek adatok gyökere alatt egy szinttel.

### <a name="stateful-reliable-service-settings"></a>Állapot-nyilvántartó megbízható szolgáltatás beállításai
A **KtlLogger** szakasz lehetővé teszi, hogy meg kell adnia a Reliable Services globális konfigurációs beállításait. Ezek a beállítások a további részletekért olvassa el a [állapot-nyilvántartó megbízható szolgáltatások konfigurálása](service-fabric-reliable-services-configuration.md).
Az alábbi példa bemutatja, hogyan módosíthatja a biztonsági másolatot a megbízható gyűjtemények állapotalapú szolgáltatások jön létre a megosztott tranzakciónapló.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Bővítmény szolgáltatások
Bővítmény funkciók konfigurálására, a apiVersion konfigurált mint "04-2017' vagy újabb legyen, és addonFeatures kell megadni:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Tároló-támogatás
Ahhoz, hogy a windows server tároló és a hyper-v tároló önálló fürtök tároló támogatása, a "DnsService" bővítmény szolgáltatás engedélyezni kell.


## <a name="next-steps"></a>Következő lépések
Miután a különálló fürt beállítása szerint konfigurált teljes művelet fájlt, a cikk követve a fürtök telepítése [hozzon létre egy különálló Service Fabric-fürt](service-fabric-cluster-creation-for-windows-server.md) majd folytassa a [ a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

