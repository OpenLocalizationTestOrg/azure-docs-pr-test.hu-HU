---
title: "aaaConnect biztonságosan tooan Azure Service Fabric-fürt |} Microsoft Docs"
description: "Ismerteti, hogyan tooauthenticate ügyfél tooa Service Fabric-fürt eléréséhez, és hogyan ügyfelek és a fürt toosecure kommunikációját."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 1b6a87a1fefaddce2043c604ca53751157232170
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-secure-cluster"></a>Csatlakoztassa tooa biztonságos fürtöt

Amikor egy ügyfél csatlakozik tooa Service Fabric-fürt csomópontja, hello ügyfél lehet biztonságos és hitelesített kommunikáció jött létre a biztonsági tanúsítvány, vagy az Azure Active Directory (AAD). A hitelesítés biztosítja, hogy csak a jogosult felhasználók férhetnek hozzá hello fürt és alkalmazások telepítését, és felügyeleti feladatok elvégzésére.  Tanúsítvány vagy AAD biztonsági kell korábban engedélyezett a hello fürt hello fürt létrehozásakor.  A fürt biztonsági forgatókönyvek további információkért lásd: [fürt biztonsági](service-fabric-cluster-security.md). Ha kapcsolódik tanúsítványokkal, védett tooa fürt [hello ügyféltanúsítvány beállítása](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) toohello fürt csatlakozó hello számítógépen. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-tooa-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Csatlakozás a biztonságos fürtjéhez tooa Azure Service Fabric CLI (sfctl)

Nincsenek néhány különböző módokon tooconnect tooa biztonságos fürt hello Service Fabric CLI (sfctl) használatával. Ha egy ügyfél-tanúsítványt a hitelesítési, részletek egyeznie kell a tanúsítványt hello tanúsítvány telepítve. toohello fürtcsomópontok. Ha a tanúsítvány tanúsítványszolgáltatók (CA), tooadditionally kell adni hello megbízható hitelesítésszolgáltatók.

Tooa fürt hello használatával csatlakoztathatja `sfctl cluster select` parancsot.

Ügyféltanúsítványokat a tanúsítvány és kulcspár, vagy a egyetlen pem-fájl két különböző módokon értelmezik a adható meg. A jelszóval védett `pem` lesz fájlok, automatikusan tooenter hello jelszót kéri.

a pem-fájl toospecify hello ügyfél tanúsítványát hello fájl elérési útvonalát adja meg hello `--pem` argumentum. Példa:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Jelszóval védett pem-fájlok felszólítja jelszó előzetes toorunning bármely parancsot.

toospecify tanúsítványt, a kulcspár használja hello `--cert` és `--key` toospecify hello fájl elérési utak tooeach megfelelő fájl argumentumokat.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Néha tanúsítványokat használ, toosecure teszt vagy fejlesztési fürtök tanúsítvány érvényesítése sikertelen. toobypass tanúsítvány ellenőrzési, adja meg a hello `--no-verify` lehetőséget. Példa:

> [!WARNING]
> Ne használjon hello `no-verify` tooproduction Service Fabric-fürtök kapcsolódáskor lehetőséget.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Emellett a megbízható CA-tanúsítványok, vagy egyedi Tanúsítványos elérési utak toodirectories is megadhat. toospecify elérési utak, használja a hello `--ca` argumentum. Példa:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

A kapcsolódás után hozzá kell férnie túl[további parancsok futtatásának jogára sfctl](service-fabric-cli.md) toointeract hello fürttel.

<a id="connectsecurecluster"></a>

## <a name="connect-tooa-cluster-using-powershell"></a>Csatlakozás tooa fürt PowerShell-lel
Mielőtt elvégezné a műveleteket a PowerShell segítségével egy fürtön, először létesítsen kapcsolatot toohello fürt. a PowerShell-munkamenetben megadott hello minden további parancs hello fürtkapcsolat használható.

### <a name="connect-tooan-unsecure-cluster"></a>Csatlakoztassa tooan nem biztonságos fürtöt

nem biztonságos tooconnect tooan fürt esetén adja meg a hello fürt végpont címe toohello **Connect-ServiceFabricCluster** parancs:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a>Csatlakozás az Azure Active Directoryval tooa biztonságos fürt

tooconnect tooa biztonságos fürt által használt Azure Active Directory tooauthorize rendszergazdai hozzáférés a fürthöz, adja meg a hello fürt tanúsítvány ujjlenyomatát, és használja a hello *AzureActiveDirectory* jelzőt.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Csatlakoztassa tooa biztonságos fürtöt használ tanúsítványt
A következő biztonságos tooconnect tooa fürt, amely PowerShell-parancs futtatása hello ügyfél tanúsítványok tooauthorize rendszergazdai hozzáféréssel használja. Adja meg a hello fürt tanúsítvány ujjlenyomata és hello rendelkezik engedéllyel a fürtkezeléshez hello ügyféltanúsítvány ujjlenyomata. hello Tanúsítványadatok hello fürtcsomópontokon tanúsítványt meg kell egyeznie.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* hello tanúsítvány ujjlenyomata látható hello server hello fürtcsomóponton telepítve van. *Findvalue –* hello ujjlenyomat hello rendszergazdai ügyféltanúsítvány.
Hello paraméterek kitöltése hello parancs néz ki a következő példa hello: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-tooa-secure-cluster-using-windows-active-directory"></a>Csatlakoztassa tooa biztonságos fürtöt Windows Active Directory használatával
Ha a különálló fürt AD biztonsági használatával lett telepítve, csatlakoztassa a toohello fürtöt hello kapcsoló "WindowsCredential" hozzáfűzésével.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-tooa-cluster-using-hello-fabricclient-apis"></a>Csatlakozás tooa fürt hello FabricClient API-k használatával
Service Fabric SDK hello biztosít hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) a fürtkezeléshez osztály. toouse hello FabricClient API-k, hello Microsoft.ServiceFabric NuGet-csomag beolvasása.

### <a name="connect-tooan-unsecure-cluster"></a>Csatlakoztassa tooan nem biztonságos fürtöt

tooconnect tooa távoli nem biztonságos fürt, hozzon létre egy FabricClient példányt, és adja meg a hello fürt címe:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

A fürtön belül futó kód, például egy megbízható szolgáltatásban hozzon létre egy FabricClient *nélkül* hello fürt címének megadása. FabricClient csatlakozik a helyi felügyeleti toohello átjáró hello csomópont hello kódja fut rajta, szükségtelenné téve az extra hálózati Ugrás.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Csatlakoztassa tooa biztonságos fürtöt használ tanúsítványt

hello fürt hello csomópontjának rendelkeznie kell érvényes tanúsítványok közös névvel, vagy a SAN DNS-neve megtalálható hello [RemoteCommonNames tulajdonság](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) be [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Ezt az eljárást lehetővé teszi, hogy a hello ügyfél és a hello fürtcsomópontok közötti kölcsönös hitelesítés.

```csharp
using System.Fabric;
using System.Security.Cryptography.X509Certificates;

string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
var fc = new FabricClient(xc, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "My";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = clientCertThumb;
    xc.RemoteCommonNames.Add(name);
    xc.RemoteCertThumbprints.Add(serverCertThumb);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```

### <a name="connect-tooa-secure-cluster-interactively-using-azure-active-directory"></a>Csatlakoztassa a fürtöt biztonságos tooa interaktív módon az Azure Active Directory használatával

a következő példában Azure Active Directory identitás- és a kiszolgáló ügyféltanúsítványt a kiszolgáló identitásának hello.

A párbeszédpanelen automatikusan ugrik fel a interaktív bejelentkezéshez toohello fürt csatlakozáskor.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

### <a name="connect-tooa-secure-cluster-non-interactively-using-azure-active-directory"></a>Csatlakoztassa a fürtöt biztonságos tooa nem interaktív az Azure Active Directory használatával

hello alábbi példa támaszkodik Microsoft.IdentityModel.Clients.ActiveDirectory, verzió: 2.19.208020213.

Az AAD-jogkivonat megszerzése további információkért lásd: [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

```csharp
string tenantId = "C15CFCEA-02C1-40DC-8466-FBD0EE0B05D2";
string clientApplicationId = "118473C2-7619-46E3-A8E4-6DA8D5F56E12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob");

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    var authContext = new AuthenticationContext(authority);

    var authResult = authContext.AcquireToken(
        resource,
        clientId,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

### <a name="connect-tooa-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Csatlakoztassa a tooa biztonságos fürtöt az Azure Active Directoryval előzetes metaadatok tudta nélkül

hello alábbi példában nem interaktív token beszerzési, de hello ugyanezt a megközelítést lehet használt toobuild a egyéni interaktív jogkivonat beszerzése érdekében. a fürtkonfiguráció hello Azure Active Directory rendszerbe foglalásához szükséges metaadatokat jogkivonat beszerzése az olvasható.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

fc.ClaimsRetrieval += (o, e) =>
{
    return GetAccessToken(e.AzureActiveDirectoryMetadata);
};

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(AzureActiveDirectoryMetadata aad)
{
    var authContext = new AuthenticationContext(aad.Authority);

    var authResult = authContext.AcquireToken(
        aad.ClusterApplication,
        aad.ClientApplication,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-tooa-secure-cluster-using-service-fabric-explorer"></a>Csatlakozás a Service Fabric Explorerrel tooa biztonságos fürt
tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) egy adott fürtben, mutasson a böngészőt, hogy:

`http://<your-cluster-endpoint>:19080/Explorer`

hello teljes URL-címet a hello fürt essentials ablaktábláján hello Azure-portálon is érhető el.

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a>Csatlakozás az Azure Active Directoryval tooa biztonságos fürt

tooconnect tooa fürt aad-ben, védett irányítsa a böngészőt, hogy:

`https://<your-cluster-endpoint>:19080/Explorer`

Automatikusan el vannak rákérdezéses toolog AAD-be.

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Csatlakoztassa tooa biztonságos fürtöt használ tanúsítványt

tooconnect tooa fürt védett tanúsítványokkal, irányítsa a böngészőt, hogy:

`https://<your-cluster-endpoint>:19080/Explorer`

Automatikusan el vannak felszólító tooselect ügyféltanúsítványt.

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-hello-remote-computer"></a>Állítson be egy ügyféltanúsítványt hello távoli számítógépen
Legalább két tanúsítványt használjon hello fürt, hello fürt és a kiszolgálói tanúsítvány egyet, majd egy másikat, az ügyfél hozzáférésének biztonságossá tételéhez.  Javasoljuk, hogy is használjon további másodlagos és az ügyfél-hozzáférési tanúsítványokat.  toosecure hello kommunikációját ügyfél és a fürt csomópont használt biztonsági tanúsítvány, akkor először tooobtain kell, és telepítheti hello ügyféltanúsítványt. hello tanúsítványt a tárolóba hello személyes (a) hello helyi számítógépen vagy a jelenlegi felhasználó hello is telepíthető.  Hello kiszolgálói tanúsítvány ujjlenyomata hello is, hogy hello ügyfél hitelesíthető hello fürt kell.

Futtassa a következő PowerShell-parancsmag tooset hello ügyfél-tanúsítvány van hello fürt hozzáférési hello számítógépen hello.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Ha önaláírt tanúsítványt, hogy tooyour számítógép "megbízható személyeknek" tárolja a tanúsítvány tooconnect tooa biztonságos fürt használatba vétele előtt tooimport kell.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a>Következő lépések

* [Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak](service-fabric-cluster-upgrade.md)
* [A Visual Studio a Service Fabric-alkalmazások kezelése](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric állapotfigyelő modell bemutatása](service-fabric-health-introduction.md)
* [Alkalmazás biztonságának és RunAs](service-fabric-application-runas-security.md)
* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)
