---
title: "a Service Fabric aaaAzure fordított proxy |} Microsoft Docs"
description: "A Service Fabric fordított proxy használata a belső és külső hello fürt kommunikációs toomicroservices."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 0e7835a64ccd74293c7bdd8b41deae414c83dffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Az Azure Service Fabric fordított proxy
fordított proxy hello Azure Service Fabric beépített címek hello Service Fabric-fürt, amely elérhetővé teszi a HTTP-végpontokról mikroszolgáltatások létrehozására.

## <a name="microservices-communication-model"></a>Mikroszolgáltatások kommunikációs modellt
A Service Fabric Mikroszolgáltatások általában futtassa a hello fürtön lévő virtuális gépek egy részét, és áthelyezheti egy virtuális gép tooanother különböző okokból. Igen dinamikusan módosítható hello végpontjainak mikroszolgáltatások létrehozására. a következő hello jellegzetes toocommunicate toohello mikroszolgáltatási hello következő oldja meg a hurok:

1. Hárítsa el a hello szolgáltatás helyét kezdetben hello elnevezési szolgáltatáson keresztül.
2. Connect toohello szolgáltatás.
3. Kapcsolódási hibák hello okának megállapításához, és újra feloldása hello szolgáltatáskeresésre, ha szükséges.

Ez a folyamat általában ügyféloldali kommunikációs szalagtárak újrapróbálkozási hurokba került, amely megvalósítja az hello szolgáltatás megoldás, és próbálkozzon újra a házirendek alkalmazásburkoló foglal magában.
További információkért lásd: [Connect és szolgáltatásokkal kommunikálni](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-hello-reverse-proxy"></a>Hello fordított proxy segítségével történő kommunikációhoz
fordított proxy hello a Service Fabric hello fürt összes csomópontjának hello futtatja. Hello teljes szolgáltatási névfeloldási folyamat az ügyfél nevében végez, és ezután továbbítja a hello ügyfélkérés. Igen hello fürtön futó bármely ügyféloldali HTTP-kommunikáció szalagtárak tootalk toohello célként megadott szolgáltatás mezővel használhat hello fordított proxy, hogy fut a helyi hello ugyanahhoz a csomóponthoz.

![Belső kommunikációs][1]

## <a name="reaching-microservices-from-outside-hello-cluster"></a>Külső hello fürtből mikroszolgáltatások elérése
hello alapértelmezett külső kommunikáció modell mikroszolgáltatások egy egy választható modell, ahol minden szolgáltatás nem érhető el közvetlenül a külső ügyfeleknek. [Az Azure Load Balancer](../load-balancer/load-balancer-overview.md), ez egy olyan hálózathatárhoz mikroszolgáltatások létrehozására és a külső ügyfelek közötti hálózati címfordítás végez, és továbbítja külső kérelmek toointernal IP:port végpontok. toomake egy mikroszolgáltatási végpont közvetlenül elérhető tooexternal ügyfelek, először konfigurálnia kell tooforward forgalom tooeach portot, amelyet hello szolgáltatás használ a Load Balancer hello fürtben. Továbbá a legtöbb mikroszolgáltatások, különösen akkor állapot-nyilvántartó mikroszolgáltatások nem élő hello fürt összes csomópontján. hello mikroszolgáltatások áthelyezheti a feladatátvételi csomópontjai között. Ebben az esetben, Load Balancer nem hatékony hello helyének meghatározásához hello cél csomópontjának hello replikák toowhich továbbítsa a forgalmat.

### <a name="reaching-microservices-via-hello-reverse-proxy-from-outside-hello-cluster"></a>Mikroszolgáltatások külső hello fürtből a hello fordított proxyn keresztül történő elérése
Egy szolgáltatás portja hello konfigurálására a terheléselosztóhoz, port is beállítható csak hello hello fordított proxy a terheléselosztó hasonló adataival. Ez a konfiguráció lehetővé teszi az ügyfelek számára hello fürtön kívüli hello fordított proxy további konfiguráció nélkül használatával hello fürtben található szolgáltatások eléréséhez.

![Külső kommunikáció][0]

> [!WARNING]
> Hello fordított proxy portja a Load Balancer konfigurálásakor hello fürt összes HTTP-végponttal visszaállítását mikroszolgáltatások hello fürtön kívüli megcímezhető.
>
>


## <a name="uri-format-for-addressing-services-by-using-hello-reverse-proxy"></a>Szolgáltatások címzéshez hello fordított proxy használatával az URI-formátum
fordított proxy használ a megadott egységes erőforrás-azonosító (URI) formátumban tooidentify hello partíció toowhich hello bejövő szolgáltatáskérés továbbítani hello:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http (s):** hello fordított proxy lehet konfigurált tooaccept HTTP vagy HTTPS-forgalmat. HTTPS-továbbítást, tekintse meg túl[tooa biztonságos szolgáltatás kapcsolattartásnak hello fordított proxy](service-fabric-reverseproxy-configure-secure-communication.md) után fordított proxy telepítő toolisten HTTPS.
* **Fürt teljesen minősített tartománynevét (FQDN) |} belső IP:** külső ügyfelek esetében konfigurálhatja hello fordított proxy úgy, hogy hello fürt tartományban, például mycluster.eastus.cloudapp.azure.com keresztül érhető el. Alapértelmezés szerint a hello fordított proxy minden csomóponton fut. A belső forgalom hello fordított proxy elérhető localhost vagy minden csomópont belső IP-, például 10.0.0.1.
* **Port:** hello portja, például a 19081, amely hello fordított proxy lett meghatározva.
* **ServiceInstanceName:** tooreach hello nélkül próbálja telepíteni hello szolgáltatáspéldány hello teljesen minősített neve "fabric: /" sémával. Például tooreach hello *fabric: / myapp/myservice/* szolgáltatást szeretné használni *myapp/myservice*.

    hello szolgáltatás példány neve megkülönbözteti a kis-és nagybetűket. Egy másik kis-és nagybetűk használata hello szolgáltatás neve hello URL-címben azt eredményezi, hello kérelmek toofail a 404-es (nem található).
* **Utótag elérési út:** Ez az hello tényleges URL-címe, mint például az *myapi/értékek/hozzáadása/3*, tooconnect a kívánt hello szolgáltatás.
* **PartitionKey:** egy particionált szolgáltatást, ez pedig hello számított partíciós kulcs, amelyet az tooreach hello partíció. Vegye figyelembe, hogy ez *nem* hello partíció GUID azonosítója. Ez a paraméter nincs szükség a hello egypéldányos partícióséma használó szolgáltatások.
* **PartitionKind:** hello szolgáltatás partícióséma azt. Ez lehet "Int64Range" vagy "Nevű". Ez a paraméter nincs szükség a hello egypéldányos partícióséma használó szolgáltatások.
* **ListenerName** hello szolgáltatás hello végpontok hello formában vannak {"Végpont": {"Listener1": "Endpoint1", "Listener2": "Endpoint2" …}}. Amikor hello szolgáltatás elérhetővé teszi a több végpontot, hello végpont azonosítja, hogy hello ügyfélkérés továbbítani kell. Ez csak egy figyelő hello szolgáltatás-e elhagyható.
* **TargetReplicaSelector** azt határozza meg, hogyan hello cél replika- vagy kell kiválasztani.
  * Állapot-nyilvántartó hello célszolgáltatás esetén hello TargetReplicaSelector hello következő lehet: "PrimaryReplica", "RandomSecondaryReplica" vagy "RandomReplica". Ha ez a paraméter nincs megadva, a hello alapértelmezett érték "PrimaryReplica".
  * Állapot nélküli hello célként megadott szolgáltatás esetén a fordított proxy szerzi hello szolgáltatás partíció tooforward hello kérelem véletlenszerű példányát.
* **Időtúllépés:** azt határozza meg, hogy hello időtúllépés hello HTTP-kérelem hello fordított proxy toohello szolgáltatás nevében hello ügyfél kérelmet létrehozni. hello alapértelmezett értéke 60 másodperc. Ez nem kötelező paraméter.

### <a name="example-usage"></a>Példa használati
Példaként vegyünk hello *fabric: / MyApp/MyService* , amely megnyitja a hello URL-cím a következő HTTP-figyelő szolgáltatás:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Az alábbiakban hello erőforrásait hello szolgáltatáshoz:

* `/index.html`
* `/api/users/<userId>`

Hello szolgáltatás hello egypéldányos particionálási sémát használja, ha hello *PartitionKey* és *PartitionKind* lekérdezési karakterlánc paraméterek esetén nincs szükség, és hello szolgáltatás elérhetőségének hello átjáró mint használatával:

* Külsőleg:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Belső:`http://localhost:19081/MyApp/MyService`

Hello szolgáltatás hello egységes Int64 particionálási sémát használja, ha hello *PartitionKey* és *PartitionKind* lekérdezési karakterlánc-paraméterrel használt tooreach hello szolgáltatás partíciójának kell lennie:

* Külsőleg:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Belső:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

tooreach hello erőforrások elérhetővé tévő hello szolgáltatást, egyszerűen után hello szolgáltatás neve hello URL-címben helyezze el hello erőforrás elérési útja:

* Külsőleg:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Belső:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

hello átjáró ezután továbbítja a kérelmek toohello szolgáltatás URL-címe:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>A port megosztása különleges kezelést szolgáltatások
Azure Application Gateway megpróbál tooresolve szolgáltatás cím újra, majd próbálja megismételni a hello kérelem, ha a szolgáltatás nem érhető el. Ennek oka egy fő előnye, hogy az Alkalmazásátjáró Ügyfélkód kell tooimplement saját service megoldás, és nem oldja meg a hurok.

Általában a szolgáltatás nem érhető el, amikor hello szolgáltatáspéldány vagy replika áthelyezte tooa másik csomópont, a normál életciklusának. Ez akkor fordul elő, amikor Alkalmazásátjáró fordulhat elő, egy hálózati kapcsolat hiba arról, hogy a végpont nem hosszabb nyissa meg a hello eredetileg feloldva cím.

Azonban replikák és a szolgáltatáspéldány egy gazdafolyamaton megoszthatnak, és előfordulhat, hogy is megoszthat egy portot, ha azt egy http.sys alapú webkiszolgálóhoz, beleértve:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [Az ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Ebben az esetben valószínű hello webkiszolgálóra hello gazdafolyamat és érhető toorequests, de hello feloldása szolgáltatáspéldány, és a replika már nem érhető el a gazdagépen hello válaszol. Ebben az esetben hello átjáró kap egy HTTP 404-es választ hello webkiszolgáló. HTTP 404-es így két különböző jelentése van:

- #1. eset: hello szolgáltatás címe helyes, de, amely a kért felhasználót hello hello erőforrás nem létezik.
- #2. eset: hello szolgáltatás címe nem megfelelő, és, hogy a felhasználó hello hello erőforrás előfordulhat, hogy létezik egy másik csomópontjára.

hello első esetben egy normál HTTP 404-es, amely felhasználói hiba történt. Hello második esetben azonban hello felhasználói kérte a erőforrása, amely létezik. Alkalmazásátjáró nem toolocate, mert hello szolgáltatás maga átkerült volt. Alkalmazás igényeinek tooresolve hello átjárócímet újra, és ismételje meg a hello kérést.

Alkalmazásátjáró így kell egy módon toodistinguish két esetben között. toomake, hogy megkülönböztetés, egy hello kiszolgálóról megadása szükséges.

* Alapértelmezés szerint az Application Gateway azt feltételezi, hogy a #2. eset, és újra megpróbál tooresolve és probléma hello kérelem.
* tooindicate #1. eset tooApplication átjáró hello szolgáltatást a következő HTTP-válaszfejléc hello kell visszaadnia:

  `X-ServiceFabric : ResourceNotFound`

A HTTP-válaszfejléc jelzi egy normál HTTP 404-es helyzetben melyik hello a kért erőforrás nem létezik, és Alkalmazásátjáró nem kísérli meg tooresolve hello szolgáltatás címe újra.

## <a name="setup-and-configuration"></a>Beállítás és konfiguráció

### <a name="enable-reverse-proxy-via-azure-portal"></a>Azure-portálon fordított proxy engedélyezése

Azure portál egy beállítás tooenable fordított proxy biztosít egy új Service Fabric-fürt létrehozása során.
A **létrehozása a Service Fabric-fürt**, 2. lépés: fürtkonfiguráció esetén a csomópont-konfiguráció típusát, jelölje be a hello négyzetet túl "Enable fordított proxy".
Biztonságos fordított proxy konfigurálása az SSL-tanúsítvány adható meg a 3. lépés: biztonsági, fürt biztonsági beállításainak konfigurálása, jelölje be hello jelölőnégyzet túl "Közé tartozik egy SSL-tanúsítvány a fordított proxyhoz", és írja be a hello tanúsítvány adatait.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Fordított proxyn keresztül Azure Resource Manager-sablonok engedélyezése

Használhatja a hello [Azure Resource Manager sablon](service-fabric-cluster-creation-via-arm.md) tooenable hello fordított proxy a Service Fabric hello fürthöz.

Tekintse meg a túl[beállítása HTTPS fordított Proxy biztonságos fürtben](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) az Azure Resource Manager-sablon minták tooconfigure biztonságos fordított proxy tanúsítvány és kezelési tanúsítványok leváltása.

Első lépésként elérhetővé hello sablon hello fürt, amelyet az toodeploy. Hello minta sablonok, vagy hozzon létre egy egyéni erőforrás-kezelő sablont. Ezt követően engedélyezheti hello fordított proxy hello lépések segítségével:

1. Adjon meg egy hello fordított proxy portja hello [paraméterek szakaszban](../azure-resource-manager/resource-group-authoring-templates.md) hello sablon.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Adja meg a hello portot az egyes hello nodetype objektumok hello **fürt** [erőforrás típushoz című rész](../azure-resource-manager/resource-group-authoring-templates.md).

    hello port hello paraméter neve, reverseProxyEndpointPort azonosít.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. tooaddress hello fordított proxy a külső hello Azure-fürttel, az 1. lépésben megadott hello port hello Azure Load Balancer szabályokat.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. SSL-tanúsítványok tooconfigure hello porton hello fordított proxy, vegye fel a hello tanúsítvány toohello ***reverseProxyCertificate*** hello tulajdonság **fürt** [erőforrás típushoz című rész](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-hello-cluster-certificate"></a>A fordított proxy tanúsítvány, amely eltér a fürt tanúsítvány hello támogatása
 Ha hello fordított proxy tanúsítvány nem azonos a hello tanúsítvány, amely biztosítja a hello fürt, majd hello korábban megadott tanúsítvány hello virtuális gépen kell telepíteni, és hozzá toohello hozzáférés-vezérlési lista (ACL), hogy a Service Fabric is -e férni. Ezt megteheti a hello **virtualMachineScaleSets** [erőforrás típushoz című rész](../resource-group-authoring-templates.md). A telepítéshez adja hozzá az adott tanúsítvány toohello osProfile. hello kiterjesztésben megadottaknak hello sablon frissítheti hello hello ACL-tanúsítványt.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Tanúsítványokat, amelyek eltérnek a hello fürt tanúsítvány tooenable hello fordított proxy egy meglévő fürt használatakor hello fordított proxy tanúsítvány telepítéséhez, és frissítse a hello ACL hello fürtön, hello fordított proxy engedélyezése előtt. Teljes hello [Azure Resource Manager sablon](service-fabric-cluster-creation-via-arm.md) említett hello beállításokkal telepítési korábban egy központi telepítési tooenable hello fordított proxy megkezdése előtt a lépések 1-4.

## <a name="next-steps"></a>Következő lépések
* Példa a szolgáltatások közötti HTTP-kommunikációt egy [mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [A fordított proxy hello toosecure HTTP-szolgáltatás továbbítás](service-fabric-reverseproxy-configure-secure-communication.md)
* [Távoli eljáráshívások a Reliable Services távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)
* [Webes API-t használó OWIN Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-kommunikáció Reliable Services használatával](service-fabric-reliable-services-communication-wcf.md)
* A további fordított proxy konfigurációs lehetőségeket, tekintse meg a alapú/Http című [testre szabhatja a Service Fabric-fürt beállítások](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
