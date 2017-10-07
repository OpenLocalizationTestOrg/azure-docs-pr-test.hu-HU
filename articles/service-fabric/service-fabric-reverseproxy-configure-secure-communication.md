---
title: "a Service Fabric aaaAzure fordított proxy biztonságos kommunikációs |} Microsoft Docs"
description: "Fordított proxy tooenable biztonságos végpontok közötti kommunikáció konfigurálását."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: e1248dffe2c324373ad0d09d3f5f094db74480d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-secure-service-with-hello-reverse-proxy"></a>Csatlakozás tooa biztonságos szolgáltatás hello fordított proxy

Ez a cikk azt ismerteti, hogyan tooestablish hello fordított proxy és a szolgáltatások, így egy záró tooend biztonságos csatorna közötti kapcsolat biztonságossá tétele érdekében.

Csatlakozás toosecure szolgáltatások támogatott csak a HTTPS konfigurált toolisten fordított proxy esetén. Hello dokumentum többi részén azt feltételezi, hogy hello eset.
Tekintse meg a túl[fordított proxy az Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello fordított proxy a Service Fabric.

## <a name="secure-connection-establishment-between-hello-reverse-proxy-and-services"></a>Hello fordított proxy és a szolgáltatások közötti biztonságos kapcsolat felépítése 

### <a name="reverse-proxy-authenticating-tooservices"></a>Fordított proxy hitelesítése tooservices:
hello fordított proxy azonosítja magát a tanúsítványt használni, a megadott tooservices ***reverseProxyCertificate*** hello tulajdonság **fürt** [erőforrás típushoz című rész](../azure-resource-manager/resource-group-authoring-templates.md). Szolgáltatások hello logika tooverify hello által bemutatott tanúsítványt hello fordított proxy is létrehozható. hello szolgáltatás konfigurációs beállításai a hello konfigurációs csomag megadható hello elfogadható ügyfél-tanúsítvány részletei. Futásidőben olvasható, és a fordított proxy hello toovalidate hello tanúsítványt használja. Tekintse meg a túl[alkalmazás paraméterek kezelése](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello konfigurációs beállításokat. 

### <a name="reverse-proxy-verifying-hello-services-identity-via-hello-certificate-presented-by-hello-service"></a>Fordított proxy keresztül hello szolgáltatás hello tanúsítvány hello szolgáltatás identitás ellenőrzése:
tooperform kiszolgálói tanúsítvány hitelesítése a hello szolgáltatások által bemutatott hello tanúsítványok, fordított proxy támogatja hello alábbi beállítások egyikét: None, ServiceCommonNameAndIssuer és ServiceCertificateThumbprints.
ezen három lehetőség közül tooselect adja meg a hello **ApplicationCertificateValidationPolicy** hello paraméterek szakaszban alapú/Http elem alatt [fabricSettings](service-fabric-cluster-fabric-settings.md).

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "None"
              }
            ]
          }
        ],
        ...
}
```

Minden beállítás további konfigurációs vonatkozó további információért tekintse meg a toohello a következő szakaszban.

### <a name="service-certificate-validation-options"></a>Szolgáltatásbeállítások tanúsítvány érvényesítése 

- **Nincs**: fordított proxy kihagyja az hello küldése a proxyn keresztül szolgáltatástanúsítvány ellenőrzése és hello biztonságos kapcsolatot. Ez az alapértelmezett viselkedés hello.
Adja meg a hello **ApplicationCertificateValidationPolicy** értékű **nincs** alapú/Http elem hello paraméterek szakaszban.

- **ServiceCommonNameAndIssuer**: fordított proxy ellenőrzi, hogy a tanúsítvány köznapi nevének és azonnali kibocsátójának ujjlenyomata alapján hello szolgáltatás által bemutatott hello tanúsítvány: Adja meg a hello **ApplicationCertificateValidationPolicy**  értékű **ServiceCommonNameAndIssuer** alapú/Http elem hello paraméterek szakaszban.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCommonNameAndIssuer"
              }
            ]
          }
        ],
        ...
}
```

toospecify hello szolgáltatás köznapi név és listája kibocsátó ujjlenyomatok hozzáadása egy **alapú/Http/ServiceCommonNameAndIssuer** elem alatt fabricSettings, az alább látható módon. Több tanúsítvány köznapi név és kiállítójának ujjlenyomata párok hello paraméterek tömbelem lehet hozzáadni. 

Ha hello végpont fordított proxy kapcsolódik egy tanúsítványt, aki közös toopresents nevét és kiállítójának ujjlenyomata megegyezik-e az itt megadott hello értékek, akkor SSL-csatorna jön létre. Hiba toomatch hello tanúsítvány részletei alapján fordított proxy hello ügyfélkérések 502 (Hibás átjáró)-állapotkód: sikertelen lesz. hello HTTP állapotsora is tartalmazni fog hello kifejezés "Érvénytelen SSL-tanúsítvány." 

```json
{
"fabricSettings": [
          ...
          {
             "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
            "parameters": [
              {
                "name": "WinFabric-Test-Certificate-CN1",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
              },
              {
                "name": "WinFabric-Test-Certificate-CN2",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
              }
            ]
          }
        ],
        ...
}
```


- **ServiceCertificateThumbprints**: fordított proxy hello küldése a proxyn keresztül szolgáltatástanúsítvány annak ujjlenyomata alapján ellenőrzi. Választhat toogo Ez az útvonal Ha önaláírt tanúsítványokat hello szolgáltatások vannak beállítva: Adja meg a hello **ApplicationCertificateValidationPolicy** értékű **ServiceCertificateThumbprints**alapú/Http elem hello paraméterek szakaszban.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCertificateThumbprints"
              }
            ]
          }
        ],
        ...
}
```

A hello ujjlenyomatok is megadhat egy **ServiceCertificateThumbprints** paraméterek bemutató alapú/Http elem. Több ujjlenyomatok hello érték mezőben, vesszővel tagolt lista formájában adható meg alább látható módon:

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "ServiceCertificateThumbprints",
                "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
              }
            ]
          }
        ],
        ...
}
```

Ha hello hello kiszolgálói tanúsítvány ujjlenyomata a konfigurációs bejegyzés is szerepel, a fordított proxy sikeres hello SSL-kapcsolatot. Ellenkező esetben Ez megszakítja hello kapcsolatot, és sikertelen hello 502-es (Hibás átjáró) az ügyfél által kért. hello HTTP állapotsora is tartalmazni fog hello kifejezés "Érvénytelen SSL-tanúsítvány."

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Ha a szolgáltatások elérhetővé tenni a biztonságos, valamint az olyan nem biztonságos végpontok végpont lemezválasztási logika
A Service fabric szolgáltatás több végpontot konfigurálását támogatja. Lásd: [Specify resources in a service manifest](service-fabric-service-manifest-resources.md) (Erőforrások megadása a szolgáltatásjegyzékben).

Fordított proxy kiválaszt egy hello végpontok tooforward hello alapuló kérelem hello **ListenerName** lekérdezési paraméter. Ha nincs megadva, kiválaszthatja a tetszőleges végpontot hello végpontok listából. Ez lehet most egy HTTP vagy HTTPS-végponton. Előfordulhat, forgatókönyvek/követelmények hello fordított proxy toooperate módban egy"biztonságos", ahová Egytényezős nem szeretné, hogy hello biztonságos fordított proxy tooforward kérelmek toounsecured végpontok. Ez elérhető hello megadásával **SecureOnlyMode** konfigurációs bejegyzés értékű **igaz** alapú/Http elem hello paraméterek szakaszban.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> 
> Ha a **SecureOnlyMode**, ha az ügyfél megadott egy **ListenerName** megfelelő tooan HTTP(unsecured) végpont, fordított proxy hello kérelem a 404-es (nem található) HTTP-állapotkód: sikertelen.

## <a name="setting-up-client-certificate-authentication-through-hello-reverse-proxy"></a>Ügyféltanúsítvány-alapú hitelesítés hello fordított proxyn keresztül történő beállítása
SSL-lezárást történik hello fordított proxy, és minden hello ügyfél tanúsítvány adatok elvesznek. Hello szolgáltatások tooperform ügyféltanúsítvány-alapú hitelesítés, állítsa a hello **ForwardClientCertificate** hello paraméterek szakaszban alapú/Http elem beállítása.

1. Ha **ForwardClientCertificate** értéke túl**hamis**, fordított proxy nem fog igényelni hello ügyféltanúsítványt az SSL-kézfogás során hello ügyféllel.
Ez az alapértelmezett viselkedés hello.

2. Ha **ForwardClientCertificate** értéke túl**igaz**, fordított proxy kérelmek hello ügyféltanúsítvány hello ügyféllel az SSL-kézfogás során.
Majd továbbítja hello ügyfél nevű egyéni HTTP-fejléc Tanúsítványadatok **X ügyféltanúsítvány**. hello fejléc értéke hello base64-kódolású PEM-formázási karakterlánca hello ügyféltanúsítvány. hello szolgáltatást is sikeres/sikertelen hello kérelem megfelelő állapotkód: hello Tanúsítványadatok ellenőrzése után.
Hello ügyfél nincs jelen tanúsítvány, ha a fordított proxy egy üres fejléc továbbítja, és lehetővé teszik a hello szolgáltatás leíró hello eset.

> Fordított proxy puszta továbbító. Bármely hello ügyfél-tanúsítvány érvényesítése nem hajtja végre.


## <a name="next-steps"></a>Következő lépések
* Tekintse meg a túl[fordított proxy tooconnect toosecure szolgáltatások konfigurálása](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) az Azure Resource Manager-sablon minták tooconfigure biztonságos fordított proxy hello eltérő tanúsítvány érvényesítése beállításokkal.
* Példa a szolgáltatások közötti HTTP-kommunikációt egy [mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Távoli eljáráshívások a Reliable Services távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)
* [Webes API-t használó OWIN Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Fürttanúsítványok kezelése](service-fabric-cluster-security-update-certs-azure.md)
