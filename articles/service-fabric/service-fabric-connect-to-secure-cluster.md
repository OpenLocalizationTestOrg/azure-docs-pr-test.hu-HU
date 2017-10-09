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
# <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="ef0d6-103">Csatlakoztassa tooa biztonságos fürtöt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-103">Connect tooa secure cluster</span></span>

<span data-ttu-id="ef0d6-104">Amikor egy ügyfél csatlakozik tooa Service Fabric-fürt csomópontja, hello ügyfél lehet biztonságos és hitelesített kommunikáció jött létre a biztonsági tanúsítvány, vagy az Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="ef0d6-104">When a client connects tooa Service Fabric cluster node, hello client can be authenticated and secure communication established using certificate security or Azure Active Directory (AAD).</span></span> <span data-ttu-id="ef0d6-105">A hitelesítés biztosítja, hogy csak a jogosult felhasználók férhetnek hozzá hello fürt és alkalmazások telepítését, és felügyeleti feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-105">This authentication ensures that only authorized users can access hello cluster and deployed applications and perform management tasks.</span></span>  <span data-ttu-id="ef0d6-106">Tanúsítvány vagy AAD biztonsági kell korábban engedélyezett a hello fürt hello fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-106">Certificate or AAD security must have been previously enabled on hello cluster when hello cluster was created.</span></span>  <span data-ttu-id="ef0d6-107">A fürt biztonsági forgatókönyvek további információkért lásd: [fürt biztonsági](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="ef0d6-107">For more information on cluster security scenarios, see [Cluster security](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="ef0d6-108">Ha kapcsolódik tanúsítványokkal, védett tooa fürt [hello ügyféltanúsítvány beállítása](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) toohello fürt csatlakozó hello számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-108">If you are connecting tooa cluster secured with certificates, [set up hello client certificate](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) on hello computer that connects toohello cluster.</span></span> 

<a id="connectsecureclustercli"></a> 

## <a name="connect-tooa-secure-cluster-using-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="ef0d6-109">Csatlakozás a biztonságos fürtjéhez tooa Azure Service Fabric CLI (sfctl)</span><span class="sxs-lookup"><span data-stu-id="ef0d6-109">Connect tooa secure cluster using Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="ef0d6-110">Nincsenek néhány különböző módokon tooconnect tooa biztonságos fürt hello Service Fabric CLI (sfctl) használatával.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-110">There are a few different ways tooconnect tooa secure cluster using hello Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="ef0d6-111">Ha egy ügyfél-tanúsítványt a hitelesítési, részletek egyeznie kell a tanúsítványt hello tanúsítvány telepítve. toohello fürtcsomópontok.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-111">When using a client certificate for authentication, hello certificate details must match a certificate deployed toohello cluster nodes.</span></span> <span data-ttu-id="ef0d6-112">Ha a tanúsítvány tanúsítványszolgáltatók (CA), tooadditionally kell adni hello megbízható hitelesítésszolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-112">If your certificate has Certificate Authorities (CAs), you need tooadditionally specify hello trusted CAs.</span></span>

<span data-ttu-id="ef0d6-113">Tooa fürt hello használatával csatlakoztathatja `sfctl cluster select` parancsot.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-113">You can connect tooa cluster using hello `sfctl cluster select` command.</span></span>

<span data-ttu-id="ef0d6-114">Ügyféltanúsítványokat a tanúsítvány és kulcspár, vagy a egyetlen pem-fájl két különböző módokon értelmezik a adható meg.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-114">Client certificates can be specified in two different fashions, either as a cert and key pair, or as a single pem file.</span></span> <span data-ttu-id="ef0d6-115">A jelszóval védett `pem` lesz fájlok, automatikusan tooenter hello jelszót kéri.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-115">For password protected `pem` files, you will be prompted automatically tooenter hello password.</span></span>

<span data-ttu-id="ef0d6-116">a pem-fájl toospecify hello ügyfél tanúsítványát hello fájl elérési útvonalát adja meg hello `--pem` argumentum.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-116">toospecify hello client certificate as a pem file, specify hello file path in hello `--pem` argument.</span></span> <span data-ttu-id="ef0d6-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-117">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="ef0d6-118">Jelszóval védett pem-fájlok felszólítja jelszó előzetes toorunning bármely parancsot.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-118">Password protected pem files will prompt for password prior toorunning any command.</span></span>

<span data-ttu-id="ef0d6-119">toospecify tanúsítványt, a kulcspár használja hello `--cert` és `--key` toospecify hello fájl elérési utak tooeach megfelelő fájl argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-119">toospecify a cert, key pair use hello `--cert` and `--key` arguments toospecify hello file paths tooeach respective file.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

<span data-ttu-id="ef0d6-120">Néha tanúsítványokat használ, toosecure teszt vagy fejlesztési fürtök tanúsítvány érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-120">Sometimes certificates used toosecure test or dev clusters fail certificate validation.</span></span> <span data-ttu-id="ef0d6-121">toobypass tanúsítvány ellenőrzési, adja meg a hello `--no-verify` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-121">toobypass certificate verification, specify hello `--no-verify` option.</span></span> <span data-ttu-id="ef0d6-122">Példa:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-122">For example:</span></span>

> [!WARNING]
> <span data-ttu-id="ef0d6-123">Ne használjon hello `no-verify` tooproduction Service Fabric-fürtök kapcsolódáskor lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-123">Do not use hello `no-verify` option when connecting tooproduction Service Fabric clusters.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

<span data-ttu-id="ef0d6-124">Emellett a megbízható CA-tanúsítványok, vagy egyedi Tanúsítványos elérési utak toodirectories is megadhat.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-124">In addition, you can specify paths toodirectories of trusted CA certs, or individual certs.</span></span> <span data-ttu-id="ef0d6-125">toospecify elérési utak, használja a hello `--ca` argumentum.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-125">toospecify these paths, use hello `--ca` argument.</span></span> <span data-ttu-id="ef0d6-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-126">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

<span data-ttu-id="ef0d6-127">A kapcsolódás után hozzá kell férnie túl[további parancsok futtatásának jogára sfctl](service-fabric-cli.md) toointeract hello fürttel.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-127">After you connect, you should be able too[run other sfctl commands](service-fabric-cli.md) toointeract with hello cluster.</span></span>

<a id="connectsecurecluster"></a>

## <a name="connect-tooa-cluster-using-powershell"></a><span data-ttu-id="ef0d6-128">Csatlakozás tooa fürt PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="ef0d6-128">Connect tooa cluster using PowerShell</span></span>
<span data-ttu-id="ef0d6-129">Mielőtt elvégezné a műveleteket a PowerShell segítségével egy fürtön, először létesítsen kapcsolatot toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-129">Before you perform operations on a cluster through PowerShell, first establish a connection toohello cluster.</span></span> <span data-ttu-id="ef0d6-130">a PowerShell-munkamenetben megadott hello minden további parancs hello fürtkapcsolat használható.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-130">hello cluster connection is used for all subsequent commands in hello given PowerShell session.</span></span>

### <a name="connect-tooan-unsecure-cluster"></a><span data-ttu-id="ef0d6-131">Csatlakoztassa tooan nem biztonságos fürtöt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-131">Connect tooan unsecure cluster</span></span>

<span data-ttu-id="ef0d6-132">nem biztonságos tooconnect tooan fürt esetén adja meg a hello fürt végpont címe toohello **Connect-ServiceFabricCluster** parancs:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-132">tooconnect tooan unsecure cluster, provide hello cluster endpoint address toohello **Connect-ServiceFabricCluster** command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="ef0d6-133">Csatlakozás az Azure Active Directoryval tooa biztonságos fürt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-133">Connect tooa secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="ef0d6-134">tooconnect tooa biztonságos fürt által használt Azure Active Directory tooauthorize rendszergazdai hozzáférés a fürthöz, adja meg a hello fürt tanúsítvány ujjlenyomatát, és használja a hello *AzureActiveDirectory* jelzőt.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-134">tooconnect tooa secure cluster that uses Azure Active Directory tooauthorize cluster administrator access, provide hello cluster certificate thumbprint and use hello *AzureActiveDirectory* flag.</span></span>  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="ef0d6-135">Csatlakoztassa tooa biztonságos fürtöt használ tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-135">Connect tooa secure cluster using a client certificate</span></span>
<span data-ttu-id="ef0d6-136">A következő biztonságos tooconnect tooa fürt, amely PowerShell-parancs futtatása hello ügyfél tanúsítványok tooauthorize rendszergazdai hozzáféréssel használja.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-136">Run hello following PowerShell command tooconnect tooa secure cluster that uses client certificates tooauthorize administrator access.</span></span> <span data-ttu-id="ef0d6-137">Adja meg a hello fürt tanúsítvány ujjlenyomata és hello rendelkezik engedéllyel a fürtkezeléshez hello ügyféltanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-137">Provide hello cluster certificate thumbprint and hello thumbprint of hello client certificate that has been granted permissions for cluster management.</span></span> <span data-ttu-id="ef0d6-138">hello Tanúsítványadatok hello fürtcsomópontokon tanúsítványt meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-138">hello certificate details must match a certificate on hello cluster nodes.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="ef0d6-139">*ServerCertThumbprint* hello tanúsítvány ujjlenyomata látható hello server hello fürtcsomóponton telepítve van.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-139">*ServerCertThumbprint* is hello thumbprint of hello server certificate installed on hello cluster nodes.</span></span> <span data-ttu-id="ef0d6-140">*Findvalue –* hello ujjlenyomat hello rendszergazdai ügyféltanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-140">*FindValue* is hello thumbprint of hello admin client certificate.</span></span>
<span data-ttu-id="ef0d6-141">Hello paraméterek kitöltése hello parancs néz ki a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-141">When hello parameters are filled in, hello command looks like hello following example:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-tooa-secure-cluster-using-windows-active-directory"></a><span data-ttu-id="ef0d6-142">Csatlakoztassa tooa biztonságos fürtöt Windows Active Directory használatával</span><span class="sxs-lookup"><span data-stu-id="ef0d6-142">Connect tooa secure cluster using Windows Active Directory</span></span>
<span data-ttu-id="ef0d6-143">Ha a különálló fürt AD biztonsági használatával lett telepítve, csatlakoztassa a toohello fürtöt hello kapcsoló "WindowsCredential" hozzáfűzésével.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-143">If your standalone cluster is deployed using AD security, connect toohello cluster by appending hello switch "WindowsCredential".</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-tooa-cluster-using-hello-fabricclient-apis"></a><span data-ttu-id="ef0d6-144">Csatlakozás tooa fürt hello FabricClient API-k használatával</span><span class="sxs-lookup"><span data-stu-id="ef0d6-144">Connect tooa cluster using hello FabricClient APIs</span></span>
<span data-ttu-id="ef0d6-145">Service Fabric SDK hello biztosít hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) a fürtkezeléshez osztály.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-145">hello Service Fabric SDK provides hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) class for cluster management.</span></span> <span data-ttu-id="ef0d6-146">toouse hello FabricClient API-k, hello Microsoft.ServiceFabric NuGet-csomag beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-146">toouse hello FabricClient APIs, get hello Microsoft.ServiceFabric NuGet package.</span></span>

### <a name="connect-tooan-unsecure-cluster"></a><span data-ttu-id="ef0d6-147">Csatlakoztassa tooan nem biztonságos fürtöt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-147">Connect tooan unsecure cluster</span></span>

<span data-ttu-id="ef0d6-148">tooconnect tooa távoli nem biztonságos fürt, hozzon létre egy FabricClient példányt, és adja meg a hello fürt címe:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-148">tooconnect tooa remote unsecured cluster, create a FabricClient instance and provide hello cluster address:</span></span>

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

<span data-ttu-id="ef0d6-149">A fürtön belül futó kód, például egy megbízható szolgáltatásban hozzon létre egy FabricClient *nélkül* hello fürt címének megadása.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-149">For code that is running from within a cluster, for example, in a Reliable Service, create a FabricClient *without* specifying hello cluster address.</span></span> <span data-ttu-id="ef0d6-150">FabricClient csatlakozik a helyi felügyeleti toohello átjáró hello csomópont hello kódja fut rajta, szükségtelenné téve az extra hálózati Ugrás.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-150">FabricClient connects toohello local management gateway on hello node hello code is currently running on, avoiding an extra network hop.</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="ef0d6-151">Csatlakoztassa tooa biztonságos fürtöt használ tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-151">Connect tooa secure cluster using a client certificate</span></span>

<span data-ttu-id="ef0d6-152">hello fürt hello csomópontjának rendelkeznie kell érvényes tanúsítványok közös névvel, vagy a SAN DNS-neve megtalálható hello [RemoteCommonNames tulajdonság](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) be [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="ef0d6-152">hello nodes in hello cluster must have valid certificates whose common name or DNS name in SAN appears in hello [RemoteCommonNames property](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) set on [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span></span> <span data-ttu-id="ef0d6-153">Ezt az eljárást lehetővé teszi, hogy a hello ügyfél és a hello fürtcsomópontok közötti kölcsönös hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-153">Following this process enables mutual authentication between hello client and hello cluster nodes.</span></span>

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

### <a name="connect-tooa-secure-cluster-interactively-using-azure-active-directory"></a><span data-ttu-id="ef0d6-154">Csatlakoztassa a fürtöt biztonságos tooa interaktív módon az Azure Active Directory használatával</span><span class="sxs-lookup"><span data-stu-id="ef0d6-154">Connect tooa secure cluster interactively using Azure Active Directory</span></span>

<span data-ttu-id="ef0d6-155">a következő példában Azure Active Directory identitás- és a kiszolgáló ügyféltanúsítványt a kiszolgáló identitásának hello.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-155">hello following example uses Azure Active Directory for client identity and server certificate for server identity.</span></span>

<span data-ttu-id="ef0d6-156">A párbeszédpanelen automatikusan ugrik fel a interaktív bejelentkezéshez toohello fürt csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-156">A dialog window automatically pops up for interactive sign-in upon connecting toohello cluster.</span></span>

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

### <a name="connect-tooa-secure-cluster-non-interactively-using-azure-active-directory"></a><span data-ttu-id="ef0d6-157">Csatlakoztassa a fürtöt biztonságos tooa nem interaktív az Azure Active Directory használatával</span><span class="sxs-lookup"><span data-stu-id="ef0d6-157">Connect tooa secure cluster non-interactively using Azure Active Directory</span></span>

<span data-ttu-id="ef0d6-158">hello alábbi példa támaszkodik Microsoft.IdentityModel.Clients.ActiveDirectory, verzió: 2.19.208020213.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-158">hello following example relies on Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span></span>

<span data-ttu-id="ef0d6-159">Az AAD-jogkivonat megszerzése további információkért lásd: [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef0d6-159">For more information on AAD token acquisition, see [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span></span>

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

### <a name="connect-tooa-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a><span data-ttu-id="ef0d6-160">Csatlakoztassa a tooa biztonságos fürtöt az Azure Active Directoryval előzetes metaadatok tudta nélkül</span><span class="sxs-lookup"><span data-stu-id="ef0d6-160">Connect tooa secure cluster without prior metadata knowledge using Azure Active Directory</span></span>

<span data-ttu-id="ef0d6-161">hello alábbi példában nem interaktív token beszerzési, de hello ugyanezt a megközelítést lehet használt toobuild a egyéni interaktív jogkivonat beszerzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-161">hello following example uses non-interactive token acquisition, but hello same approach can be used toobuild a custom interactive token acquisition experience.</span></span> <span data-ttu-id="ef0d6-162">a fürtkonfiguráció hello Azure Active Directory rendszerbe foglalásához szükséges metaadatokat jogkivonat beszerzése az olvasható.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-162">hello Azure Active Directory metadata needed for token acquisition is read from cluster configuration.</span></span>

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

## <a name="connect-tooa-secure-cluster-using-service-fabric-explorer"></a><span data-ttu-id="ef0d6-163">Csatlakozás a Service Fabric Explorerrel tooa biztonságos fürt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-163">Connect tooa secure cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="ef0d6-164">tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) egy adott fürtben, mutasson a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-164">tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) for a given cluster, point your browser to:</span></span>

`http://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="ef0d6-165">hello teljes URL-címet a hello fürt essentials ablaktábláján hello Azure-portálon is érhető el.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-165">hello full URL is also available in hello cluster essentials pane of hello Azure portal.</span></span>

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="ef0d6-166">Csatlakozás az Azure Active Directoryval tooa biztonságos fürt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-166">Connect tooa secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="ef0d6-167">tooconnect tooa fürt aad-ben, védett irányítsa a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-167">tooconnect tooa cluster that is secured with AAD, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="ef0d6-168">Automatikusan el vannak rákérdezéses toolog AAD-be.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-168">You are automatically be prompted toolog in with AAD.</span></span>

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="ef0d6-169">Csatlakoztassa tooa biztonságos fürtöt használ tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="ef0d6-169">Connect tooa secure cluster using a client certificate</span></span>

<span data-ttu-id="ef0d6-170">tooconnect tooa fürt védett tanúsítványokkal, irányítsa a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="ef0d6-170">tooconnect tooa cluster that is secured with certificates, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="ef0d6-171">Automatikusan el vannak felszólító tooselect ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-171">You are automatically be prompted tooselect a client certificate.</span></span>

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-hello-remote-computer"></a><span data-ttu-id="ef0d6-172">Állítson be egy ügyféltanúsítványt hello távoli számítógépen</span><span class="sxs-lookup"><span data-stu-id="ef0d6-172">Set up a client certificate on hello remote computer</span></span>
<span data-ttu-id="ef0d6-173">Legalább két tanúsítványt használjon hello fürt, hello fürt és a kiszolgálói tanúsítvány egyet, majd egy másikat, az ügyfél hozzáférésének biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-173">At least two certificates should be used for securing hello cluster, one for hello cluster and server certificate and another for client access.</span></span>  <span data-ttu-id="ef0d6-174">Javasoljuk, hogy is használjon további másodlagos és az ügyfél-hozzáférési tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-174">We recommend that you also use additional secondary certificates and client access certificates.</span></span>  <span data-ttu-id="ef0d6-175">toosecure hello kommunikációját ügyfél és a fürt csomópont használt biztonsági tanúsítvány, akkor először tooobtain kell, és telepítheti hello ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-175">toosecure hello communication between a client and a cluster node using certificate security, you first need tooobtain and install hello client certificate.</span></span> <span data-ttu-id="ef0d6-176">hello tanúsítványt a tárolóba hello személyes (a) hello helyi számítógépen vagy a jelenlegi felhasználó hello is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-176">hello certificate can be installed into hello Personal (My) store of hello local computer or hello current user.</span></span>  <span data-ttu-id="ef0d6-177">Hello kiszolgálói tanúsítvány ujjlenyomata hello is, hogy hello ügyfél hitelesíthető hello fürt kell.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-177">You also need hello thumbprint of hello server certificate so that hello client can authenticate hello cluster.</span></span>

<span data-ttu-id="ef0d6-178">Futtassa a következő PowerShell-parancsmag tooset hello ügyfél-tanúsítvány van hello fürt hozzáférési hello számítógépen hello.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-178">Run hello following PowerShell cmdlet tooset up hello client certificate on hello computer from which you access hello cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

<span data-ttu-id="ef0d6-179">Ha önaláírt tanúsítványt, hogy tooyour számítógép "megbízható személyeknek" tárolja a tanúsítvány tooconnect tooa biztonságos fürt használatba vétele előtt tooimport kell.</span><span class="sxs-lookup"><span data-stu-id="ef0d6-179">If it is a self-signed certificate, you need tooimport it tooyour machine's "trusted people" store before you can use this certificate tooconnect tooa secure cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a><span data-ttu-id="ef0d6-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef0d6-180">Next steps</span></span>

* [<span data-ttu-id="ef0d6-181">Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak</span><span class="sxs-lookup"><span data-stu-id="ef0d6-181">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="ef0d6-182">A Visual Studio a Service Fabric-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="ef0d6-182">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="ef0d6-183">Service Fabric állapotfigyelő modell bemutatása</span><span class="sxs-lookup"><span data-stu-id="ef0d6-183">Service Fabric Health model introduction</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="ef0d6-184">Alkalmazás biztonságának és RunAs</span><span class="sxs-lookup"><span data-stu-id="ef0d6-184">Application Security and RunAs</span></span>](service-fabric-application-runas-security.md)
* [<span data-ttu-id="ef0d6-185">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="ef0d6-185">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
