---
title: "a Service Fabric aaaAzure az API Management – első lépések |} Microsoft Docs"
description: "Ez az útmutató bemutatja, hogyan tooquickly el az Azure API Management és a Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: f76f3f39a92f89892d6a02ecaab1ec3d343fe2a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>A Service Fabric az Azure API Management – első lépések

Ez az útmutató bemutatja, hogyan tooset Azure API Management a Service Fabric össze, és az első API művelet toosend forgalom tooback-szolgáltatások konfigurálása a Service Fabric. További információ az Azure API Management-forgatókönyvet az Service Fabric toolearn lásd: hello [áttekintése](service-fabric-api-management-overview.md) cikk. 

## <a name="deploy-api-management-and-service-fabric-tooazure"></a>Az API Management és a Service Fabric tooAzure telepítése

első lépés hello toodeploy API Management és a Service Fabric-fürt tooAzure egy megosztott virtuális hálózaton. Ez lehetővé teszi az API Management toocommunicate közvetlenül a Service Fabric, így műveleteket hajthat végre a szolgáltatásészlelés, a szolgáltatás partíció megoldása és a forgalom közvetlen tooany háttér a Service Fabric szolgáltatás.

### <a name="topology"></a>topológia

Ez az útmutató hello következő telepíti, amelyben az API Management és a Service Fabric is alhálózatain topológia tooAzure hello ugyanazon a virtuális hálózaton:

 ![Képfelirat][sf-apim-topology-overview]

tooget gyorsan lépéseket, Resource Manager-sablonok áll rendelkezésre egyes telepítési lépéseket:

 - Hálózati topológia:
    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]
 - Service Fabric-fürt:
    - [cluster.JSON][cluster-arm]
    - [cluster.Parameters.JSON][cluster-parameters-arm]
 - API-kezelés:
    - [APIM.JSON][apim-arm]
    - [APIM.Parameters.JSON][apim-parameters-arm]

### <a name="sign-in-tooazure-and-select-your-subscription"></a>Jelentkezzen be tooAzure, és jelölje ki az előfizetését

Ez az útmutató használ [Azure PowerShell][azure-powershell]. Amikor egy új PowerShell-munkamenetet indít el, jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését, Azure parancsok végrehajtása előtt.
 
Bejelentkezés tooyour Azure-fiók:

```powershell
PS > Login-AzureRmAccount
```

Jelölje ki az előfizetését:

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy új erőforráscsoportot az üzembe helyezéshez. Adjon neki egy nevet és egy helyet.

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-hello-network-topology"></a>Hello hálózati topológia központi telepítéséhez

hello első lépéseként tooset mentése hello hálózati topológia toowhich API Management és hello Service Fabric-fürt lesz telepítve. Hello [network.json] [ network-arm] Resource Manager-sablon olyan virtuális hálózatot (VNET), két alhálózattal és két hálózati biztonsági csoportok (NSG) konfigurált toocreate. 

Hello [network.parameters.json] [ network-parameters-arm] paraméterfájl hello alhálózatok és a telepítendő API Management és a Service Fabric az NSG-ket hello nevét tartalmazza. Ez az útmutató hello paraméterértékeket nem szükséges módosítani toobe. hello API Management és a Service Fabric Resource Manager sablonok használja ezeket az értékeket, így ha itt módosításuk, módosítania kell azokat hello más Resource Manager-sablonok ennek megfelelően. 

 1. Töltse le a következő Resource Manager sablon és a paraméterek fájl hello:

    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]

 2. A következő PowerShell toodeploy hello Resource Manager sablonnal és paraméterfájlokkal parancsfájlok hello hálózati telepítő hello használata:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-hello-service-fabric-cluster"></a>Hello Service Fabric-fürt központi telepítése

Hello hálózati erőforrások végzett telepítését, miután hello következő lépésre toodeploy a Service Fabric fürt toohello VNET hello alhálózati és NSG hello Service Fabric-fürt jelöltek ki. Ebben az oktatóanyagban hello Service Fabric Resource Manager-sablon előre konfigurált toouse hello nevek hello virtuális Hálózatot, alhálózatot és hello előző lépésben beállított NSG. 

Service Fabric fürt Resource Manager-sablon hello konfigurált toocreate tanúsítvány biztonsági használó biztonságos fürtök. hello tanúsítványok is használt toosecure csomópontok kommunikáció a fürt és a toomanage felhasználói hozzáférés tooyour Service Fabric-fürt. Az API Management szolgáltatás felderítése a tanúsítvány tooaccess hello Service Fabric-szolgáltatás használ.

Ez a lépés szükséges tanúsítvánnyal rendelkező Key Vault a fürt biztonsági. Key Vault használó biztonságos fürtök beállításával kapcsolatos további információkért lásd: [Ez az útmutató a fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> A hozzáférés a fürthöz használt hozzáadása toohello tanúsítvány hozzáadhatja Azure Active Directory-hitelesítés. Az Azure Active Directory hello ajánlott módja toomanage felhasználói hozzáférés tooyour Service Fabric-fürt, de nem szükséges toocomplete Ez az oktatóanyag. Egy tanúsítványra szükség mindkét módszer fürt a csomópontok biztonság és az Azure API Management hitelesítést, amely jelenleg nem támogatja az Azure Active Directoryban a Service Fabric háttérkiszolgáló hitelesítéséhez.

 1. Töltse le a következő Resource Manager sablon és a paraméterek fájl hello:
 
    - [cluster.JSON][cluster-arm]
    - [cluster.Parameters.JSON][cluster-parameters-arm]

 2. Töltse ki a hello üres paraméterek között hello `cluster.parameters.json` fájl a telepítéshez, beleértve a hello [Key Vault információk](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) a fürt tanúsítvány.

 3. A következő PowerShell parancs toodeploy hello Resource Manager sablonnal és paraméterfájlokkal fájlok toocreate hello Service Fabric-fürt hello használata:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Az API Management telepítése

Végezetül telepítése az API Management toohello VNET hello az alhálózaton, és az API Management kijelölt NSG. Toowait hello Service Fabric fürt telepítési toofinish az API Management telepítése előtt nem kell. 

Ebben az oktatóanyagban hello API Management Resource Manager-sablon előre konfigurált toouse hello nevek hello virtuális Hálózatot, alhálózatot és hello előző lépésben beállított NSG. 

 1. Töltse le a következő Resource Manager sablon és a paraméterek fájl hello:
 
    - [APIM.JSON][apim-arm]
    - [APIM.Parameters.JSON][apim-parameters-arm]

 2. Töltse ki a hello üres paraméterek között hello `apim.parameters.json` az üzembe helyezéshez.

 3. Használja a következő PowerShell parancs toodeploy hello Resource Manager sablonnal és paraméterfájlokkal fájlok az API Management hello:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>API-kezelés konfigurálása

Után az API Management és a Service Fabric-fürt vannak telepítve, a Service Fabric háttér állíthatja be az API Management. Ez lehetővé teszi egy háttér szolgáltatás házirend által küldött forgalmat tooyour Service Fabric-fürt toocreate.

### <a name="api-management-security"></a>API Management biztonsági

tooconfigure hello Service Fabric háttér, először tooconfigure API Management biztonsági beállításait. tooconfigure biztonsági beállításait, lépjen a tooyour API Management szolgáltatásba a hello Azure-portálon.

#### <a name="enable-hello-api-management-rest-api"></a>Hello API Management REST API engedélyezése

hello API Management REST API jelenleg hello csak úgy tooconfigure egy háttérszolgáltatáshoz. első lépés hello tooenable hello API Management REST API-t, és biztosíthatja.

 1. Hello API-kezelés szolgáltatás, válassza ki **felügyeleti API - előzetes** alatt **biztonsági**.
 2. Ellenőrizze a hello **engedélyezése API Management REST API** jelölőnégyzetet.
 3. Vegye figyelembe a hello felügyeleti API URL-CÍMÉT – ez használjuk fel hello Service Fabric háttér újabb tooset hello URL-címe
 4. Generate egy **hozzáférési tokent** kiválasztásával lejárati dátummal és a kulcsot, majd kattintson az hello **Generate** gomb hello lap alján hello felé.
 5. Másolás hello **hozzáférési jogkivonat** és mentheti – Ez a következő lépéseket hello fogjuk használni. Vegye figyelembe, hogy ez nem azonos a hello elsődleges és másodlagos kulcsot.

#### <a name="upload-a-service-fabric-client-certificate"></a>A Service Fabric ügyfél-tanúsítvány feltöltése

A Service Fabric-fürtöt használ, amely rendelkezik hozzáférési tooyour fürt tanúsítványt szolgáltatásészlelés az API Management kell hitelesíteni. Az egyszerűség kedvéért a oktatóanyag használ a fürt hello hello Service Fabric-fürt, amely alapértelmezés szerint használt tooaccess lehet létrehozásakor megadott ugyanazt a tanúsítványt.

 1. Hello API-kezelés szolgáltatás, válassza ki **ügyféltanúsítványok - előzetes** alatt **biztonsági**.
 2. Kattintson a hello **+ Hozzáadás** gomb
 2. Hello titkos kulcs fájlja (.pfx) a Service Fabric-fürt létrehozásakor megadott hello fürt tanúsítvány kiválasztása, adjon neki egy nevet, és adja meg a hello titkos kulcs jelszava.

> [!NOTE]
> Ez az oktatóanyag hello ugyanaz a tanúsítvány az ügyfél-hitelesítés és a fürt-csomópontok biztonsági használja. Ha a Service Fabric-fürt rendelkezik egy konfigurált tooaccess, hogy külön ügyfél-tanúsítványt használhatja.

### <a name="configure-hello-backend"></a>Hello háttér konfigurálása

Most, hogy az API Management a biztonsági beállítások konfigurálása hello Service Fabric háttér konfigurálhatja. A Service Fabric háttérkiszolgálókon hello Service Fabric-fürt esetén hello háttér ahelyett, hogy egy adott Service Fabric-szolgáltatás. Ez lehetővé teszi, hogy a szabályzatban tooroute toomore, mint egy szolgáltatás hello fürtben.

Ez a lépés hello hozzáférési jogkivonat korábban létrehozott igényel, és hello tooAPI felügyeleti hello előző lépésben töltött fel a fürt-tanúsítványának ujjlenyomata.

> [!NOTE]
> Az API Management hello előző lépésben használt külön ügyfél-tanúsítványt, ha szüksége hello ujjlenyomat hello ügyféltanúsítvánnyal hozzáadása toohello fürt tanúsítvány ujjlenyomata ebben a lépésben.

A következő HTTP PUT kérés toohello API Management API URL korábban feljegyzett hello API Management REST API tooconfigure hello Service Fabric háttérszolgáltatást engedélyezésekor hello küldése. Megjelenik egy `HTTP 201 Created` választ, ha a hello parancs sikeres. Minden mező további információkért lásd: hello API Management [háttér-API referenciadokumentációt](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

HTTP-parancs és az URL-címe:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Kérelem fejlécei:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

A kérelem törzse:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

Hello **URL-cím** itt paraméter egy teljesen minősített nevét a fürt összes kérelmet, a szolgáltatás tooby alapértelmezett irányítva, ha a háttérkiszolgáló házirendben megadott szolgáltatásnév nem. Használhat egy hamis nevét, például a "fabric: / hamis/szolgáltatás" Ha nem kívánja toohave tartalék szolgáltatás.

Tekintse meg az API Management toohello [háttér-API referenciadokumentációt](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) kapcsolatban további részleteket a minden mező.

#### <a name="example"></a>Példa

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>A Service Fabric háttér-szolgáltatás telepítése

Most, hogy a Service Fabric konfigurálva, egy háttér tooAPI felügyeleti hello, az API-k esetében, amelyek forgalmat küldeni a Service Fabric-szolgáltatások tooyour hozhat létre a háttér-házirendeket. De először meg kell a Service Fabric tooaccept kérelmek futó szolgáltatásban.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>A Service Fabric-szolgáltatás egy HTTP-végpont létrehozása

Ebben az oktatóanyagban létrehozunk egy egyszerű állapot nélküli ASP.NET Core megbízható szolgáltatás hello alapértelmezett webes API projektsablon használatával. Ezzel létrehoz egy HTTP-végpont a szolgáltatás, amely akkor lesz Azure API Management elérhetővé:

```
/api/values
```

Első lépésként [állítja be a fejlesztési környezetet az ASP.NET Core fejlesztési](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Miután beállította a fejlesztőkörnyezetet, indítsa el a Visual Studio rendszergazdaként, és az ASP.NET Core szolgáltatás létrehozása:

 1. A Visual Studio alkalmazásban válassza ki a fájl -> Új projekt.
 2. Válassza ki a felhő hello Service Fabric-alkalmazás sablont, és nevezze el **"ApiApplication"**.
 3. Válassza ki az ASP.NET Core Szolgáltatássablon hello és name hello projekt **"WebApiService"**.
 4. Válassza ki a webes API-t az ASP.NET Core 1.1 hello projekt sablont.
 5. Hello projekt létrehozása után nyissa meg a `PackageRoot\ServiceManifest.xml` , és távolítsa el a hello `Port` attribútumot hello végpont erőforrás-konfiguráció:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Ez lehetővé teszi a Service Fabric toospecify egy portjával dinamikusan hello alkalmazás porttartományát, amely azt nyitják meg a hálózati biztonsági csoport hello hello fürt Resource Manager-sablon, a forgalom tooflow tooit engedélyezi az API Management.
 
 6. Nyomja le az F5 a Visual Studio tooverify hello webes API-t helyileg nem érhető el. 

    Nyissa meg a Service Fabric Explorerben talál, és részletekbe menően tárhatják tooa ASP.NET Core toosee hello alapcímnek hello szolgáltatást figyel a következőn: hello adott példánya. Adja hozzá `/api/values` toohello cím kiinduló, majd nyissa meg a böngészőben. Ez elindítja a hello hello ValuesController hello webes API-sablonban a Get metódust. Hello alapértelmezett válasz hello sablon, egy JSON-tömb két karakterláncot tartalmazó által biztosított adja vissza:

    ```json
    ["value1", "value2"]`
    ```

    Ez a hello végpontot, amely akkor lesz az Azure API Management elérhetővé.

 7. Végezetül telepíteni hello alkalmazás tooyour fürtöket az Azure-ban. [Visual Studio használatával](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), kattintson a jobb gombbal a hello projektet, és válassza ki **közzététel**. Adja meg a fürt végpontja (például `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello alkalmazás tooyour Service Fabric-fürt az Azure-ban.

Az ASP.NET Core állapotmentes szolgáltatások nevű `fabric:/ApiApplication/WebApiService` most futnia kell a Service Fabric-fürt az Azure-ban.

## <a name="create-an-api-operation"></a>Hozzon létre egy API-művelet

Most már készen áll a toocreate az API Management egy műveletet a rendszer az adott külső ügyfelek használata toocommunicate hello hello Service Fabric-fürt futtatja az ASP.NET Core állapotmentes szolgáltatások.

 1. Jelentkezzen be Azure-portálon toohello, és keresse meg a tooyour API-kezelés szolgáltatás telepítését.
 2. Hello API-kezelés szolgáltatás panelen válassza ki a **API-k – előzetes**
 3. Adja hozzá egy új API hello kattintva **üres API** mezőbe, majd kitöltésével hello párbeszédpanel:

     - **Webszolgáltatás URL-címe**: A Service Fabric háttérkiszolgálókon, ez az URL-cím érték nem használható. Bármely érték itt helyezhet el. A jelen oktatóanyag esetében használja: `http://servicefabric`.
     - **Név**: bármely nevezze el az API-t. A jelen oktatóanyag esetében használja `Service Fabric App`.
     - **URL-séma**: jelölje be a HTTP, HTTPS vagy mindkettőt. A jelen oktatóanyag esetében használja `both`.
     - **API URL-cím utótag**: Adjon meg egy utótagot az API felületen. A jelen oktatóanyag esetében használja `myapp`.
 
 4. Hello API létrehozása után kattintson **+ Hozzáadás művelet** tooadd egy előtér-API műveletet. Töltse ki hello értékeket:
    
     - **URL-cím**: válasszon `GET` , és adjon meg egy URL-címe hello API. A jelen oktatóanyag esetében használja `/api/values`.
     
       Alapértelmezés szerint a hello URL-címe itt megadott toohello Service Fabric háttérszolgáltatást küldött hello URL-címe. Ha hello azonos URL-címe Itt a szolgáltatás által használt, ebben az esetben `/api/values`, majd a hello művelet további módosítás nélkül működik. Megadhat egy URL-címe itt háttéralkalmazásának Service Fabric-szolgáltatás által használt hello URL-címe eltérő ebben az esetben a program elérési újraírási kell toospecify a művelet házirend később.
     - **Megjelenített név**: Adjon meg egy tetszőleges nevet hello API. A jelen oktatóanyag esetében használja `Values`.

## <a name="configure-a-backend-policy"></a>Háttér-házirend konfigurálása

hello háttér házirend kötelékek mindent együtt. Ez a szolgáltatás toowhich rendszer kérést átirányítja a Service Fabric hello háttér konfigurálására szolgáló. A házirend tooany API művelet is alkalmazhatja. Hello [háttérkonfiguráció a Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) hello következő kérelem útválasztási vezérlők biztosítja: 
 - Példány kiválasztása szolgáltatás a Service Fabric szolgáltatás példány neve, vagy szoftveresen kötött megadásával (például `"fabric:/myapp/myservice"`), és létre hello HTTP-kérelem (például `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - A Service Fabric particionálási sémát használó partíciós kulcs létrehozása általi megoldása partíció.
 - Állapotalapú szolgáltatások replika kiválasztása.
 - Megoldási ismételje meg a feltételeket, amelyek lehetővé teszik toospecify hello feltételek újra feloldani a helyét, és küldje el újra a kérelmet.

A jelen oktatóanyag esetében győződjön meg arról, hogy útvonalak közvetlenül toohello ASP.NET Core állapotmentes szolgáltatások korábban telepített kérelmek háttér szabályzat létrehozása:

 1. Válassza ki, és szerkesztheti a hello **házirendek bejövő** a hello `Values` hello Szerkesztés ikonra kattint, és jelölje be a művelet **kódnézetben**.
 2. A kód hello házirendszerkesztő, adjon hozzá egy `set-backend-service` házirend a bejövő házirendek, ahogy az itt látható, és kattintson a hello **mentése** gombra:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

A Service Fabric háttér-házirend attribútumainak teljes készletét, tekintse meg a toohello [API Management háttér-dokumentáció](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-hello-api-tooa-product"></a>Hello API tooa termék hozzáadása. 

Hello API hívása előtt, ahol biztosíthat hozzáférést toousers tooa termék hozzá kell adni. 

 1. Hello API-kezelés szolgáltatás, válassza ki **termékek - előzetes**.
 2. Alapértelmezés szerint az API Management szolgáltatók két termékek: alapszintű és a korlátlan. Válassza ki a hello korlátlan terméket.
 3. Válassza ki az API-k.
 4. Kattintson a hello **+ Hozzáadás** gombra.
 5. Jelölje be hello `Service Fabric App` API hello előző lépésekben létrehozott, és kattintson a hello **válasszon** gombra.

### <a name="test-it"></a>Tesztelheti

Most megpróbálhatja üzenetet küld egy kérelem tooyour háttér-szolgáltatás a Service Fabric API Management keresztül közvetlenül hello Azure-portálon.

 1. Hello API-kezelés szolgáltatás, válassza ki **API - előzetes**.
 2. A hello `Service Fabric App` hello előző lépésekben létrehozott API kiválasztása hello **teszt** fülre.
 3. Kattintson a hello **küldése** gomb toosend tesztelési kérelem toohello háttérszolgáltatás.

## <a name="next-steps"></a>Következő lépések

Ezen a ponton rendelkeznie kell egy egyszerű telepítése a Service Fabric- és API-kezelés.

Ez az oktatóanyag a Service Fabric-fürt beállítása gyorsan tooget alapvető tanúsítvány alapú hitelesítést használ. A Service Fabric-fürt speciális felhasználóhitelesítést célszerű a [Azure Active Directory hitelesítési](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Ezt követően [létrehozása és speciális termékbeállítások konfigurálása az Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare az alkalmazás a valós életben-forgalmat.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
