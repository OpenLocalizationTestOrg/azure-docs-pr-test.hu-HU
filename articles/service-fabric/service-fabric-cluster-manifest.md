---
title: "aaaConfigure az Azure Service Fabric önálló fürthöz |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure az önálló vagy titkos Service Fabric-fürt."
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
ms.openlocfilehash: ce2ad387162a05668bbd3a271c754776fe471850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Önálló Windows-fürt konfigurációs beállításai
Ez a cikk ismerteti, hogyan egy különálló Service Fabric fürt használt tooconfigure hello ***művelet*** fájlt. A fájl toospecify információt használhatja például hello Service Fabric-csomópont és az IP-címek, a csomópontok különböző típusú hello fürt, hello biztonsági beállításokkal, valamint hello hálózati topológia hiba/frissítési tartományok tekintetében a különálló fürt.

Ha Ön [hello különálló Service Fabric-csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), néhány hello művelet fájl mintákat letöltött tooyour munkahelyi gépet. hello minták *DevCluster* útra segít hozzon létre egy fürtöt azonos számítógépre, például a logikai csomópontok hello három csomópontjaihoz. Ezeken kívül legalább egy csomópont jelölésűnek kell lennie egy elsődleges csomóponton. A fürt akkor hasznos, ha egy fejlesztési vagy tesztelési környezetben, és egy éles fürt nem támogatott. hello minták *MultiMachine* a név segítségével hozhat létre egy éles minőségi fürt minden csomópont egy külön számítógépen.

1. *ClusterConfig.Unsecure.DevCluster.JSON* és *ClusterConfig.Unsecure.MultiMachine.JSON* hogyan toocreate egy nem biztonságos teszt- vagy éles fürt rendre megjelenítése. 
2. *ClusterConfig.Windows.DevCluster.JSON* és *ClusterConfig.Windows.MultiMachine.JSON* hogyan toocreate teszt- vagy éles fürt használatával biztonságossá téve megjelenítése [Windows biztonsági](service-fabric-windows-cluster-windows-security.md).
3. *ClusterConfig.X509.DevCluster.JSON* és *ClusterConfig.X509.MultiMachine.JSON* hogyan toocreate teszt- vagy éles fürt használatával biztonságossá téve megjelenítése [X509-alapú biztonsági](service-fabric-windows-cluster-x509-security.md). 

Most azt megvizsgálja hello különböző részeit egy ***művelet*** alábbi fájlt.

## <a name="general-cluster-configurations"></a>Általános fürtkonfigurációk
Ez magában foglalja a hello széleskörű fürt specifikus konfigurációk, az alábbi hello JSON kódrészletben látható.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

Rövid név tooyour Service Fabric-fürt toohello hozzárendelésével biztosíthat **neve** változó. Hello **clusterConfigurationVersion** a fürt; hello verziószám növelése kell azt minden alkalommal, amikor a Service Fabric-fürt frissítése. Azonban hagyja hello **apiVersion** toohello alapértelmezett értéket.

<a id="clusternodes"></a>

## <a name="nodes-on-hello-cluster"></a>Hello fürtön található csomópontok
Konfigurálható hello csomópont a Service Fabric-fürt hello segítségével **csomópontok** szakaszban, mint a következő kódrészletet mutat be hello.

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

A Service Fabric-fürt tartalmaznia kell legalább 3 csomópontok. A beállítás szerint további csomópontokat toothis szakasz is hozzáadhat. a következő táblázat hello hello konfigurációs beállítások az egyes csomópontok ismerteti.

| **A csomópont-konfiguráció** | **Leírás** |
| --- | --- |
| Csomópontnév |Egy rövid nevet toohello csomópont biztosíthat. |
| IP-cím |Nyisson meg egy parancsablakot, és írja be a csomópont hello IP-címének megállapítása `ipconfig`. Vegye figyelembe a hello IPV4-címet, és rendelje hozzá toohello **IP-cím** változó. |
| nodeTypeRef |Minden csomópont rendelhetők hozzá másik csomóponttípus. Hello [csomóponttípusok](#nodetypes) hello szakaszában az alábbi. |
| faultDomain |Tartalék tartományok engedélyezése rendszergazdák toodefine hello fizikai fürtcsomóponton, amely előfordulhat, hogy sikertelen volt hello azonos időben tooshared fizikai függőségek miatt. |
| upgradeDomain |Frissítési tartományok jellemezhető a csomópontokra, amelyeket állítsa le a Service Fabric frissíti a vonatkozó hello azonos idő. Dönthet úgy, mely csomópontok tooassign toowhich frissítési tartományok, mint bármely fizikai követelmények nem korlátozza. |

## <a name="cluster-properties"></a>Fürt tulajdonságai
Hello **tulajdonságok** hello művelet szakaszában a következőképpen történik használt tooconfigure hello fürt.

<a id="reliability"></a>

### <a name="reliability"></a>Megbízhatóság
hello fogalma **reliabilityLevel** replikák száma hello vagy szolgáltatáspéldányok hello Service Fabric rendszer hello hello fürt elsődleges csomópontjának futtatható határoz meg. Meghatározza, hogy ezek a szolgáltatások hello megbízhatóságát, és ezért a fürt hello. hello értéke hello rendszer által számított fürt létrehozása és frissítése során.

### <a name="diagnostics"></a>Diagnosztika
Hello **diagnosticsStore** szakasz lehetővé teszi tooconfigure paraméterek tooenable diagnosztika és a hibaelhárítási csomópont vagy a fürt hibák, ahogy az alábbi részlet hello. 

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

Hello **metaadatok** a fürt diagnosztika leírása és a telepítő szerint állítható be. Ezek a változók megkönnyíti a ETW nyomkövetési napló- és összeomlási memóriaképeket, valamint teljesítményszámlálók gyűjtése. Olvasási [a követési naplóban](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) és [ETW-nyomkövetés](https://msdn.microsoft.com/library/ms751538.aspx) ETW-vel további információ a nyomkövetési naplóit. Beleértve az összes napló [összeomlási memóriaképek](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) és [teljesítményszámlálók](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) lehet irányított toohello **connectionString** mappát a számítógépén. Is *AzureStorage* diagnosztika tárolásához. Lentebb egy minta részlet.

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Biztonság
Hello **biztonsági** szakasz esetén szükség a biztonságos különálló Service Fabric-fürt. a következő kódrészletet hello ebben a szakaszban egy részét tartalmazza.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

Hello **metaadatok** a biztonságos fürt leírása és a telepítő szerint állítható be. Hello **ClusterCredentialType** és **ServerCredentialType** határozzák meg, amely hello fürtből, valamint hello csomópontot megvalósítandó biztonsági hello típusú. A megadható tooeither *X509* egy tanúsítványalapú biztonsági vagy *Windows* egy Azure Active Directory-alapú biztonság. hello részeinek hello **biztonsági** szakasz hello biztonsági hello típusú alapul. Olvasási [tanúsítványok-alapú biztonsági önálló fürtben](service-fabric-windows-cluster-x509-security.md) vagy [Windows biztonsági önálló fürtben](service-fabric-windows-cluster-windows-security.md) olvashat, hogyan kimenő hello toofill rest-e a hello **biztonsági**szakasz.

<a id="nodetypes"></a>

### <a name="node-types"></a>Csomóponttípusok
Hello **NodeType tulajdonságok értéke** szakasz ismerteti, amely a fürt rendelkezik hello csomópontok hello típusú. Fürt esetén kell adni legalább egy csomópont típusát, ahogy az alábbi hello részlet. 

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

Hello **neve** hello felhasználóbarát név az adott csomóponttípus. a csomópont típusú csomópont toocreate rendelje hozzá a rövid név toohello **nodeTypeRef** változó az adott csomópont, ahogy azt korábban említettük [fent](#clusternodes). Az egyes csomópont adja meg a használandó hello kapcsolati végpontok. Minden kapcsolat végpontokkal portszáma dönthet úgy, mindaddig, amíg azok nem ütköznek-e a fürt bármely más végpontja. Több csomópontos fürtben, lesz egy vagy több elsődleges csomóponton (pl. **isPrimary** túl beállítása*igaz*) hello attól függően, hogy [ **reliabilityLevel** ](#reliability). Olvasási [Service Fabric fürt kapacitástervezésének szempontjai](service-fabric-cluster-capacity.md) információk **NodeType tulajdonságok értéke** és **reliabilityLevel**, és mi elsődleges és hello tooknow nem elsődleges csomóponttípusok. 

#### <a name="endpoints-used-tooconfigure-hello-node-types"></a>Végpontok használt tooconfigure hello csomóponttípusok
* *clientConnectionEndpointPort* hello port által használt hello ügyfél tooconnect toohello hello ügyfél API-k használata esetén. 
* *clusterConnectionEndpointPort* hello port, amelyen hello csomópontok kommunikálnak egymással.
* *leaseDriverEndpointPort* hello port hello fürt bérleti illesztőprogram toofind kimenő használják, ha hello csomópontok még mindig aktív. 
* *serviceConnectionEndpointPort* hello alkalmazások által használt hello port és a csomóponton, hogy adott csomóponton hello Service Fabric ügyféllel toocommunicate telepített szolgáltatások.
* *httpGatewayEndpointPort* hello Service Fabric Explorer tooconnect toohello fürt által használt hello port.
* *ephemeralPorts* hello felülbírálása [hello az operációs rendszer által használt dinamikus portok](https://support.microsoft.com/kb/929851). Service Fabric fogja használni a ezek alkalmazás portok része, és hello fennmaradó lesz elérhető a hello az operációs rendszer. Azt fogja is tartományt képeznek le a tartomány toohello meglévő szerepel hello az operációs rendszer, így minden célra használhatja hello tartományok hello minta JSON-fájlokat a megadott. Szüksége van arra, hogy a hello kezdő és záró portok hello hello különbségének legalább 255 toomake. Ha ezt a különbséget túl alacsony, mivel ez a tartomány megosztott hello operációs rendszerrel való ütközések futtathatnak. Tekintse meg a konfigurált hello dinamikus porttartományt futtatásával `netsh int ipv4 show dynamicport tcp`.
* *applicationPorts* hello Service Fabric-alkalmazások által használt portok hello. hello alkalmazás porttartományát legyen elég nagy toocover hello végpont követelmény az alkalmazások. Ebben a tartományban kell lennie a hello dinamikus porttartományt hello gépen, azaz hello kizárólagos *ephemeralPorts* hello konfigurációjában beállított tartományon.  A Service Fabric fog használni, ezek új portok szükségesek, valamint gondoskodunk hello tűzfal ezen portok megnyitása. 
* *reverseProxyEndpointPort* a választható fordított proxy végpontja. Lásd: [Service Fabric fordított Proxy](service-fabric-reverseproxy.md) további részleteket. 

### <a name="log-settings"></a>Naplózási beállítások
Hello **fabricSettings** szakasz lehetővé teszi, hogy tooset hello gyökérkönyvtárak hello Service Fabric adatokat és a naplókat. Testre szabhatja ezek csak hello kezdeti fürt létrehozása során. Ez a szakasz egy minta szövegrészletet lentebb.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Azt javasoljuk, hogy használatával egy-az operációs rendszer meghajtóján hello FabricDataRoot, és a fabriclogroot mappában, szemben az operációs rendszer összeomlások további megbízhatóságot biztosít. Vegye figyelembe, hogy csak a hello adatgyökerében testre, majd hello napló legfelső szintű kerülnek hello adatgyökerében alatt egy szinttel.

### <a name="stateful-reliable-service-settings"></a>Állapot-nyilvántartó megbízható szolgáltatás beállításai
Hello **KtlLogger** szakasz lehetővé teszi a Reliable Services tooset hello globális konfigurációs beállításait. Ezek a beállítások a további részletekért olvassa el a [állapot-nyilvántartó megbízható szolgáltatások konfigurálása](service-fabric-reliable-services-configuration.md).
hello az alábbi példa bemutatja, hogyan toochange hello hello megosztott tranzakciós napló, amely lekérdezi hozza létre a tooback a megbízható gyűjtemények állapotalapú szolgáltatások.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Bővítmény szolgáltatások
tooconfigure bővítményeire, hello apiVersion legyen konfigurált mint "04-2017' vagy újabb, és addonFeatures toobe konfigurálni kell:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Tároló-támogatás
tooenable tároló támogatása a windows server tároló, mind önálló fürtök a hyper-v tárolója, hello "DnsService" bővítmény szolgáltatás toobe engedélyezni kell.


## <a name="next-steps"></a>Következő lépések
Miután a különálló fürt beállítása szerint konfigurált teljes művelet fájlt, a cikk a következő hello fürtök telepítése [hozzon létre egy különálló Service Fabric-fürt](service-fabric-cluster-creation-for-windows-server.md) majd folytassa a túl[a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

