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
# <a name="connect-tooa-secure-service-with-hello-reverse-proxy"></a><span data-ttu-id="74dad-103">Csatlakozás tooa biztonságos szolgáltatás hello fordított proxy</span><span class="sxs-lookup"><span data-stu-id="74dad-103">Connect tooa secure service with hello reverse proxy</span></span>

<span data-ttu-id="74dad-104">Ez a cikk azt ismerteti, hogyan tooestablish hello fordított proxy és a szolgáltatások, így egy záró tooend biztonságos csatorna közötti kapcsolat biztonságossá tétele érdekében.</span><span class="sxs-lookup"><span data-stu-id="74dad-104">This article explains how tooestablish secure connection between hello reverse proxy and services, thus enabling an end tooend secure channel.</span></span>

<span data-ttu-id="74dad-105">Csatlakozás toosecure szolgáltatások támogatott csak a HTTPS konfigurált toolisten fordított proxy esetén.</span><span class="sxs-lookup"><span data-stu-id="74dad-105">Connecting toosecure services is supported only when reverse proxy is configured toolisten on HTTPS.</span></span> <span data-ttu-id="74dad-106">Hello dokumentum többi részén azt feltételezi, hogy hello eset.</span><span class="sxs-lookup"><span data-stu-id="74dad-106">Rest of hello document assumes this is hello case.</span></span>
<span data-ttu-id="74dad-107">Tekintse meg a túl[fordított proxy az Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello fordított proxy a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="74dad-107">Refer too[Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) tooconfigure hello reverse proxy in Service Fabric.</span></span>

## <a name="secure-connection-establishment-between-hello-reverse-proxy-and-services"></a><span data-ttu-id="74dad-108">Hello fordított proxy és a szolgáltatások közötti biztonságos kapcsolat felépítése</span><span class="sxs-lookup"><span data-stu-id="74dad-108">Secure connection establishment between hello reverse proxy and services</span></span> 

### <a name="reverse-proxy-authenticating-tooservices"></a><span data-ttu-id="74dad-109">Fordított proxy hitelesítése tooservices:</span><span class="sxs-lookup"><span data-stu-id="74dad-109">Reverse proxy authenticating tooservices:</span></span>
<span data-ttu-id="74dad-110">hello fordított proxy azonosítja magát a tanúsítványt használni, a megadott tooservices ***reverseProxyCertificate*** hello tulajdonság **fürt** [erőforrás típushoz című rész](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="74dad-110">hello reverse proxy identifies itself tooservices using its certificate, specified with ***reverseProxyCertificate*** property in hello **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="74dad-111">Szolgáltatások hello logika tooverify hello által bemutatott tanúsítványt hello fordított proxy is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="74dad-111">Services can implement hello logic tooverify hello certificate presented by hello reverse proxy.</span></span> <span data-ttu-id="74dad-112">hello szolgáltatás konfigurációs beállításai a hello konfigurációs csomag megadható hello elfogadható ügyfél-tanúsítvány részletei.</span><span class="sxs-lookup"><span data-stu-id="74dad-112">hello services can specify hello accepted client certificate details as configuration settings in hello configuration package.</span></span> <span data-ttu-id="74dad-113">Futásidőben olvasható, és a fordított proxy hello toovalidate hello tanúsítványt használja.</span><span class="sxs-lookup"><span data-stu-id="74dad-113">This can be read at runtime and used toovalidate hello certificate presented by hello reverse proxy.</span></span> <span data-ttu-id="74dad-114">Tekintse meg a túl[alkalmazás paraméterek kezelése](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="74dad-114">Refer too[Manage application parameters](service-fabric-manage-multiple-environment-app-configuration.md) tooadd hello configuration settings.</span></span> 

### <a name="reverse-proxy-verifying-hello-services-identity-via-hello-certificate-presented-by-hello-service"></a><span data-ttu-id="74dad-115">Fordított proxy keresztül hello szolgáltatás hello tanúsítvány hello szolgáltatás identitás ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="74dad-115">Reverse proxy verifying hello service's identity via hello certificate presented by hello service:</span></span>
<span data-ttu-id="74dad-116">tooperform kiszolgálói tanúsítvány hitelesítése a hello szolgáltatások által bemutatott hello tanúsítványok, fordított proxy támogatja hello alábbi beállítások egyikét: None, ServiceCommonNameAndIssuer és ServiceCertificateThumbprints.</span><span class="sxs-lookup"><span data-stu-id="74dad-116">tooperform server certificate validation of hello certificates presented by hello services, reverse proxy supports one of hello following options: None, ServiceCommonNameAndIssuer, and ServiceCertificateThumbprints.</span></span>
<span data-ttu-id="74dad-117">ezen három lehetőség közül tooselect adja meg a hello **ApplicationCertificateValidationPolicy** hello paraméterek szakaszban alapú/Http elem alatt [fabricSettings](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="74dad-117">tooselect one of these three options, specify hello **ApplicationCertificateValidationPolicy** in hello parameters section of ApplicationGateway/Http element under [fabricSettings](service-fabric-cluster-fabric-settings.md).</span></span>

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

<span data-ttu-id="74dad-118">Minden beállítás további konfigurációs vonatkozó további információért tekintse meg a toohello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="74dad-118">Refer toohello next section for details about additional configuration for each of these options.</span></span>

### <a name="service-certificate-validation-options"></a><span data-ttu-id="74dad-119">Szolgáltatásbeállítások tanúsítvány érvényesítése</span><span class="sxs-lookup"><span data-stu-id="74dad-119">Service certificate validation options</span></span> 

- <span data-ttu-id="74dad-120">**Nincs**: fordított proxy kihagyja az hello küldése a proxyn keresztül szolgáltatástanúsítvány ellenőrzése és hello biztonságos kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="74dad-120">**None**: Reverse proxy skips verification of hello proxied service certificate and establishes hello secure connection.</span></span> <span data-ttu-id="74dad-121">Ez az alapértelmezett viselkedés hello.</span><span class="sxs-lookup"><span data-stu-id="74dad-121">This is hello default behavior.</span></span>
<span data-ttu-id="74dad-122">Adja meg a hello **ApplicationCertificateValidationPolicy** értékű **nincs** alapú/Http elem hello paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="74dad-122">Specify hello **ApplicationCertificateValidationPolicy** with value **None** in hello parameters section of ApplicationGateway/Http element.</span></span>

- <span data-ttu-id="74dad-123">**ServiceCommonNameAndIssuer**: fordított proxy ellenőrzi, hogy a tanúsítvány köznapi nevének és azonnali kibocsátójának ujjlenyomata alapján hello szolgáltatás által bemutatott hello tanúsítvány: Adja meg a hello **ApplicationCertificateValidationPolicy**  értékű **ServiceCommonNameAndIssuer** alapú/Http elem hello paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="74dad-123">**ServiceCommonNameAndIssuer**: Reverse proxy verifies hello certificate presented by hello service based on certificate's common name and immediate issuer's thumbprint: Specify hello **ApplicationCertificateValidationPolicy** with value **ServiceCommonNameAndIssuer** in hello parameters section of ApplicationGateway/Http element.</span></span>

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

<span data-ttu-id="74dad-124">toospecify hello szolgáltatás köznapi név és listája kibocsátó ujjlenyomatok hozzáadása egy **alapú/Http/ServiceCommonNameAndIssuer** elem alatt fabricSettings, az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="74dad-124">toospecify hello list of service common name and issuer thumbprints, add a **ApplicationGateway/Http/ServiceCommonNameAndIssuer** element under fabricSettings, as shown below.</span></span> <span data-ttu-id="74dad-125">Több tanúsítvány köznapi név és kiállítójának ujjlenyomata párok hello paraméterek tömbelem lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="74dad-125">Multiple certificate common name and issuer thumbprint pairs can be added in hello parameters array element.</span></span> 

<span data-ttu-id="74dad-126">Ha hello végpont fordított proxy kapcsolódik egy tanúsítványt, aki közös toopresents nevét és kiállítójának ujjlenyomata megegyezik-e az itt megadott hello értékek, akkor SSL-csatorna jön létre.</span><span class="sxs-lookup"><span data-stu-id="74dad-126">If hello endpoint reverse proxy is connecting toopresents a certificate who's common name and  issuer thumbprint matches any of hello values specified here, SSL channel is established.</span></span> <span data-ttu-id="74dad-127">Hiba toomatch hello tanúsítvány részletei alapján fordított proxy hello ügyfélkérések 502 (Hibás átjáró)-állapotkód: sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="74dad-127">Upon failure toomatch hello certificate details, reverse proxy fails hello client's request with a 502 (Bad Gateway) status code.</span></span> <span data-ttu-id="74dad-128">hello HTTP állapotsora is tartalmazni fog hello kifejezés "Érvénytelen SSL-tanúsítvány."</span><span class="sxs-lookup"><span data-stu-id="74dad-128">hello HTTP status line will also contain hello phrase "Invalid SSL Certificate."</span></span> 

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


- <span data-ttu-id="74dad-129">**ServiceCertificateThumbprints**: fordított proxy hello küldése a proxyn keresztül szolgáltatástanúsítvány annak ujjlenyomata alapján ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="74dad-129">**ServiceCertificateThumbprints**: Reverse proxy will verify hello proxied service certificate based on its thumbprint.</span></span> <span data-ttu-id="74dad-130">Választhat toogo Ez az útvonal Ha önaláírt tanúsítványokat hello szolgáltatások vannak beállítva: Adja meg a hello **ApplicationCertificateValidationPolicy** értékű **ServiceCertificateThumbprints**alapú/Http elem hello paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="74dad-130">You can choose toogo this route when hello services are configured with self signed certificates: Specify hello **ApplicationCertificateValidationPolicy** with value **ServiceCertificateThumbprints** in hello parameters section of ApplicationGateway/Http element.</span></span>

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

<span data-ttu-id="74dad-131">A hello ujjlenyomatok is megadhat egy **ServiceCertificateThumbprints** paraméterek bemutató alapú/Http elem.</span><span class="sxs-lookup"><span data-stu-id="74dad-131">Also specify hello thumbprints with a **ServiceCertificateThumbprints** entry under parameters section of ApplicationGateway/Http element.</span></span> <span data-ttu-id="74dad-132">Több ujjlenyomatok hello érték mezőben, vesszővel tagolt lista formájában adható meg alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="74dad-132">Multiple thumbprints can be specified as a comma-separated list in hello value field, as shown below:</span></span>

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

<span data-ttu-id="74dad-133">Ha hello hello kiszolgálói tanúsítvány ujjlenyomata a konfigurációs bejegyzés is szerepel, a fordított proxy sikeres hello SSL-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="74dad-133">If hello thumbprint of hello server certificate is listed in this config entry, reverse proxy succeeds hello SSL connection.</span></span> <span data-ttu-id="74dad-134">Ellenkező esetben Ez megszakítja hello kapcsolatot, és sikertelen hello 502-es (Hibás átjáró) az ügyfél által kért.</span><span class="sxs-lookup"><span data-stu-id="74dad-134">Otherwise, it terminates hello connection and fails hello client's request with a 502 (Bad Gateway).</span></span> <span data-ttu-id="74dad-135">hello HTTP állapotsora is tartalmazni fog hello kifejezés "Érvénytelen SSL-tanúsítvány."</span><span class="sxs-lookup"><span data-stu-id="74dad-135">hello HTTP status line will also contain hello phrase "Invalid SSL Certificate."</span></span>

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a><span data-ttu-id="74dad-136">Ha a szolgáltatások elérhetővé tenni a biztonságos, valamint az olyan nem biztonságos végpontok végpont lemezválasztási logika</span><span class="sxs-lookup"><span data-stu-id="74dad-136">Endpoint selection logic when services expose secure as well as unsecured endpoints</span></span>
<span data-ttu-id="74dad-137">A Service fabric szolgáltatás több végpontot konfigurálását támogatja.</span><span class="sxs-lookup"><span data-stu-id="74dad-137">Service fabric supports configuring  multiple endpoints for a service.</span></span> <span data-ttu-id="74dad-138">Lásd: [Specify resources in a service manifest](service-fabric-service-manifest-resources.md) (Erőforrások megadása a szolgáltatásjegyzékben).</span><span class="sxs-lookup"><span data-stu-id="74dad-138">See [Specify resources in a service manifest](service-fabric-service-manifest-resources.md).</span></span>

<span data-ttu-id="74dad-139">Fordított proxy kiválaszt egy hello végpontok tooforward hello alapuló kérelem hello **ListenerName** lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="74dad-139">Reverse proxy selects one of hello endpoints tooforward hello request based on hello  **ListenerName** query parameter.</span></span> <span data-ttu-id="74dad-140">Ha nincs megadva, kiválaszthatja a tetszőleges végpontot hello végpontok listából.</span><span class="sxs-lookup"><span data-stu-id="74dad-140">If this is not specified, it can pick any endpoint from hello endpoints list.</span></span> <span data-ttu-id="74dad-141">Ez lehet most egy HTTP vagy HTTPS-végponton.</span><span class="sxs-lookup"><span data-stu-id="74dad-141">Now this can be an HTTP or HTTPS endpoint.</span></span> <span data-ttu-id="74dad-142">Előfordulhat, forgatókönyvek/követelmények hello fordított proxy toooperate módban egy"biztonságos", ahová Egytényezős</span><span class="sxs-lookup"><span data-stu-id="74dad-142">There might be scenarios/requirements where you want hello reverse proxy toooperate in a "secure only mode", i.e</span></span> <span data-ttu-id="74dad-143">nem szeretné, hogy hello biztonságos fordított proxy tooforward kérelmek toounsecured végpontok.</span><span class="sxs-lookup"><span data-stu-id="74dad-143">you don't want hello secure reverse proxy tooforward requests toounsecured endpoints.</span></span> <span data-ttu-id="74dad-144">Ez elérhető hello megadásával **SecureOnlyMode** konfigurációs bejegyzés értékű **igaz** alapú/Http elem hello paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="74dad-144">This can be achieved by specifying hello **SecureOnlyMode** configuration entry with value **true** in hello parameters section of ApplicationGateway/Http element.</span></span>   

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
> <span data-ttu-id="74dad-145">Ha a **SecureOnlyMode**, ha az ügyfél megadott egy **ListenerName** megfelelő tooan HTTP(unsecured) végpont, fordított proxy hello kérelem a 404-es (nem található) HTTP-állapotkód: sikertelen.</span><span class="sxs-lookup"><span data-stu-id="74dad-145">When operating in **SecureOnlyMode**, if client has specified a **ListenerName** corresponding tooan HTTP(unsecured) endpoint, reverse proxy fails hello request with a 404 (Not Found) HTTP status code.</span></span>

## <a name="setting-up-client-certificate-authentication-through-hello-reverse-proxy"></a><span data-ttu-id="74dad-146">Ügyféltanúsítvány-alapú hitelesítés hello fordított proxyn keresztül történő beállítása</span><span class="sxs-lookup"><span data-stu-id="74dad-146">Setting up client certificate authentication through hello reverse proxy</span></span>
<span data-ttu-id="74dad-147">SSL-lezárást történik hello fordított proxy, és minden hello ügyfél tanúsítvány adatok elvesznek.</span><span class="sxs-lookup"><span data-stu-id="74dad-147">SSL termination happens at hello reverse proxy and all hello client certificate data is lost.</span></span> <span data-ttu-id="74dad-148">Hello szolgáltatások tooperform ügyféltanúsítvány-alapú hitelesítés, állítsa a hello **ForwardClientCertificate** hello paraméterek szakaszban alapú/Http elem beállítása.</span><span class="sxs-lookup"><span data-stu-id="74dad-148">For hello services tooperform client certificate authentication, set hello **ForwardClientCertificate** setting in hello parameters section of ApplicationGateway/Http element.</span></span>

1. <span data-ttu-id="74dad-149">Ha **ForwardClientCertificate** értéke túl**hamis**, fordított proxy nem fog igényelni hello ügyféltanúsítványt az SSL-kézfogás során hello ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="74dad-149">When **ForwardClientCertificate** is set too**false**, reverse proxy will not request for hello client certificate during its SSL handshake with hello client.</span></span>
<span data-ttu-id="74dad-150">Ez az alapértelmezett viselkedés hello.</span><span class="sxs-lookup"><span data-stu-id="74dad-150">This is hello default behavior.</span></span>

2. <span data-ttu-id="74dad-151">Ha **ForwardClientCertificate** értéke túl**igaz**, fordított proxy kérelmek hello ügyféltanúsítvány hello ügyféllel az SSL-kézfogás során.</span><span class="sxs-lookup"><span data-stu-id="74dad-151">When **ForwardClientCertificate** is set too**true**, reverse proxy requests for hello client's certificate during its SSL handshake with hello client.</span></span>
<span data-ttu-id="74dad-152">Majd továbbítja hello ügyfél nevű egyéni HTTP-fejléc Tanúsítványadatok **X ügyféltanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="74dad-152">It will then forward hello client certificate data in a custom HTTP header named **X-Client-Certificate**.</span></span> <span data-ttu-id="74dad-153">hello fejléc értéke hello base64-kódolású PEM-formázási karakterlánca hello ügyféltanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="74dad-153">hello header value is hello base64 encoded PEM format string of hello client's certificate.</span></span> <span data-ttu-id="74dad-154">hello szolgáltatást is sikeres/sikertelen hello kérelem megfelelő állapotkód: hello Tanúsítványadatok ellenőrzése után.</span><span class="sxs-lookup"><span data-stu-id="74dad-154">hello service can succeed/fail hello request with appropriate status code after inspecting hello certificate data.</span></span>
<span data-ttu-id="74dad-155">Hello ügyfél nincs jelen tanúsítvány, ha a fordított proxy egy üres fejléc továbbítja, és lehetővé teszik a hello szolgáltatás leíró hello eset.</span><span class="sxs-lookup"><span data-stu-id="74dad-155">If hello client does not present a certificate, reverse proxy forwards an empty header and let hello service handle hello case.</span></span>

> <span data-ttu-id="74dad-156">Fordított proxy puszta továbbító.</span><span class="sxs-lookup"><span data-stu-id="74dad-156">Reverse proxy is a mere forwarder.</span></span> <span data-ttu-id="74dad-157">Bármely hello ügyfél-tanúsítvány érvényesítése nem hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="74dad-157">It will not perform any validation of hello client's certificate.</span></span>


## <a name="next-steps"></a><span data-ttu-id="74dad-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74dad-158">Next steps</span></span>
* <span data-ttu-id="74dad-159">Tekintse meg a túl[fordított proxy tooconnect toosecure szolgáltatások konfigurálása](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) az Azure Resource Manager-sablon minták tooconfigure biztonságos fordított proxy hello eltérő tanúsítvány érvényesítése beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="74dad-159">Refer too[Configure reverse proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples tooconfigure secure reverse proxy with hello different service certificate validation options.</span></span>
* <span data-ttu-id="74dad-160">Példa a szolgáltatások közötti HTTP-kommunikációt egy [mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="74dad-160">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="74dad-161">Távoli eljáráshívások a Reliable Services távoli eljáráshívás</span><span class="sxs-lookup"><span data-stu-id="74dad-161">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="74dad-162">Webes API-t használó OWIN Reliable Services</span><span class="sxs-lookup"><span data-stu-id="74dad-162">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="74dad-163">Fürttanúsítványok kezelése</span><span class="sxs-lookup"><span data-stu-id="74dad-163">Manage cluster certificates</span></span>](service-fabric-cluster-security-update-certs-azure.md)
