---
title: "Biztonságos kapcsolódás az Azure Service Fabric-fürt |} Microsoft Docs"
description: "Ismerteti, hogyan hitelesítheti az ügyfelek hozzáférhessenek a Service Fabric-fürt és az ügyfelek és a fürt közötti kommunikáció biztonságossá tétele."
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
ms.openlocfilehash: d6a13ceb8ccd9207ecacc166247535d496d5dec7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="c26be-103">Csatlakozás biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-103">Connect to a secure cluster</span></span>

<span data-ttu-id="c26be-104">Amikor egy ügyfél csatlakozik egy Service Fabric-fürt csomópontja, akkor az ügyfél jött létre a biztonsági tanúsítvány, vagy az Azure Active Directory (AAD) biztonságos és hitelesített kommunikáció lehet.</span><span class="sxs-lookup"><span data-stu-id="c26be-104">When a client connects to a Service Fabric cluster node, the client can be authenticated and secure communication established using certificate security or Azure Active Directory (AAD).</span></span> <span data-ttu-id="c26be-105">A hitelesítés biztosítja, hogy csak a jogosult felhasználók férhetnek hozzá a fürtöt és alkalmazások telepítését, és felügyeleti feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="c26be-105">This authentication ensures that only authorized users can access the cluster and deployed applications and perform management tasks.</span></span>  <span data-ttu-id="c26be-106">Tanúsítvány vagy AAD biztonsági kell korábban engedélyezve van a fürt a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c26be-106">Certificate or AAD security must have been previously enabled on the cluster when the cluster was created.</span></span>  <span data-ttu-id="c26be-107">A fürt biztonsági forgatókönyvek további információkért lásd: [fürt biztonsági](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="c26be-107">For more information on cluster security scenarios, see [Cluster security](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="c26be-108">Ha a tanúsítványok által védett fürthöz csatlakozik [állítsa be az ügyféltanúsítvány](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) a számítógépen, amely kapcsolódik a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="c26be-108">If you are connecting to a cluster secured with certificates, [set up the client certificate](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) on the computer that connects to the cluster.</span></span> 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="c26be-109">Csatlakozás Azure Service Fabric CLI-vel (sfctl) biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-109">Connect to a secure cluster using Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="c26be-110">Csatlakozás a Service Fabric CLI-vel (sfctl) biztonságos fürthöz néhány különböző módja van.</span><span class="sxs-lookup"><span data-stu-id="c26be-110">There are a few different ways to connect to a secure cluster using the Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="c26be-111">Ha a hitelesítés ügyféltanúsítvány használatával történik, a tanúsítvány adatainak meg kell egyeznie egy, a fürtcsomópontokon telepített tanúsítvány adataival.</span><span class="sxs-lookup"><span data-stu-id="c26be-111">When using a client certificate for authentication, the certificate details must match a certificate deployed to the cluster nodes.</span></span> <span data-ttu-id="c26be-112">Ha a tanúsítvány rendelkezik tanúsítványszolgáltatók (CA), továbbá adja meg a megbízható hitelesítésszolgáltatók szeretné.</span><span class="sxs-lookup"><span data-stu-id="c26be-112">If your certificate has Certificate Authorities (CAs), you need to additionally specify the trusted CAs.</span></span>

<span data-ttu-id="c26be-113">A fürt használatával csatlakozhat a `sfctl cluster select` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c26be-113">You can connect to a cluster using the `sfctl cluster select` command.</span></span>

<span data-ttu-id="c26be-114">Ügyféltanúsítványokat a tanúsítvány és kulcspár, vagy a egyetlen pem-fájl két különböző módokon értelmezik a adható meg.</span><span class="sxs-lookup"><span data-stu-id="c26be-114">Client certificates can be specified in two different fashions, either as a cert and key pair, or as a single pem file.</span></span> <span data-ttu-id="c26be-115">A jelszóval védett `pem` fájlok, a rendszer kérni fogja a automatikusan meg kell adnia a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c26be-115">For password protected `pem` files, you will be prompted automatically to enter the password.</span></span>

<span data-ttu-id="c26be-116">Adja meg az ügyféltanúsítványt a pem-fájl, adja meg az elérési út a `--pem` argumentum.</span><span class="sxs-lookup"><span data-stu-id="c26be-116">To specify the client certificate as a pem file, specify the file path in the `--pem` argument.</span></span> <span data-ttu-id="c26be-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="c26be-117">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="c26be-118">Jelszóval védett pem-fájlok kérni fogja a parancs futtatása előtt jelszót.</span><span class="sxs-lookup"><span data-stu-id="c26be-118">Password protected pem files will prompt for password prior to running any command.</span></span>

<span data-ttu-id="c26be-119">Adja meg a tanúsítványt, a kulcs pár használja a `--cert` és `--key` argumentumok megadása a fájl minden egyes fájlok elérési útjait.</span><span class="sxs-lookup"><span data-stu-id="c26be-119">To specify a cert, key pair use the `--cert` and `--key` arguments to specify the file paths to each respective file.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

<span data-ttu-id="c26be-120">Néha a tanúsítványok használatával teszi biztonságossá a teszt- vagy fejlesztési fürtök tanúsítvány érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="c26be-120">Sometimes certificates used to secure test or dev clusters fail certificate validation.</span></span> <span data-ttu-id="c26be-121">A tanúsítványellenőrzés kihagyása, adja meg a `--no-verify` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c26be-121">To bypass certificate verification, specify the `--no-verify` option.</span></span> <span data-ttu-id="c26be-122">Példa:</span><span class="sxs-lookup"><span data-stu-id="c26be-122">For example:</span></span>

> [!WARNING]
> <span data-ttu-id="c26be-123">Ne használja a `no-verify` beállítást az éles Service Fabric-fürtök történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="c26be-123">Do not use the `no-verify` option when connecting to production Service Fabric clusters.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

<span data-ttu-id="c26be-124">Emellett a megbízható CA-tanúsítványok, vagy egyedi Tanúsítványos könyvtárak elérési útjait is megadhat.</span><span class="sxs-lookup"><span data-stu-id="c26be-124">In addition, you can specify paths to directories of trusted CA certs, or individual certs.</span></span> <span data-ttu-id="c26be-125">Az elérési utak megadásához használja a `--ca` argumentum.</span><span class="sxs-lookup"><span data-stu-id="c26be-125">To specify these paths, use the `--ca` argument.</span></span> <span data-ttu-id="c26be-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="c26be-126">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

<span data-ttu-id="c26be-127">A kapcsolódás után meg kell tudni [további parancsok futtatásának jogára sfctl](service-fabric-cli.md) amellyel kommunikálni tud a fürt.</span><span class="sxs-lookup"><span data-stu-id="c26be-127">After you connect, you should be able to [run other sfctl commands](service-fabric-cli.md) to interact with the cluster.</span></span>

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a><span data-ttu-id="c26be-128">Csatlakozás fürthöz a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c26be-128">Connect to a cluster using PowerShell</span></span>
<span data-ttu-id="c26be-129">Mielőtt elvégezné a műveleteket a PowerShell segítségével egy fürtön, először kapcsolatot létrehozni a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="c26be-129">Before you perform operations on a cluster through PowerShell, first establish a connection to the cluster.</span></span> <span data-ttu-id="c26be-130">A fürtkapcsolat szolgál minden további parancs a megadott PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="c26be-130">The cluster connection is used for all subsequent commands in the given PowerShell session.</span></span>

### <a name="connect-to-an-unsecure-cluster"></a><span data-ttu-id="c26be-131">Csatlakozás nem biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-131">Connect to an unsecure cluster</span></span>

<span data-ttu-id="c26be-132">Egy nem biztonságos fürthöz csatlakozzon, adja meg a fürt végpont címét, hogy a **Connect-ServiceFabricCluster** parancs:</span><span class="sxs-lookup"><span data-stu-id="c26be-132">To connect to an unsecure cluster, provide the cluster endpoint address to the **Connect-ServiceFabricCluster** command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="c26be-133">Csatlakozás az Azure Active Directoryval biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-133">Connect to a secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="c26be-134">Szeretne csatlakozni egy biztonságos fürt, amely a fürt rendszergazdai hozzáférés hitelesítése az Azure Active Directory, adja meg a fürt tanúsítvány ujjlenyomatát, és használja a *AzureActiveDirectory* jelzőt.</span><span class="sxs-lookup"><span data-stu-id="c26be-134">To connect to a secure cluster that uses Azure Active Directory to authorize cluster administrator access, provide the cluster certificate thumbprint and use the *AzureActiveDirectory* flag.</span></span>  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="c26be-135">Csatlakozás egy ügyfél-tanúsítvány használatával biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-135">Connect to a secure cluster using a client certificate</span></span>
<span data-ttu-id="c26be-136">A következő parancsot PowerShell rendszergazdai hozzáférés hitelesítése ügyféltanúsítványok használó biztonságos fürtben való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="c26be-136">Run the following PowerShell command to connect to a secure cluster that uses client certificates to authorize administrator access.</span></span> <span data-ttu-id="c26be-137">Adja meg a fürt tanúsítvány ujjlenyomata és, hogy rendelkezik engedéllyel a kiszolgálófürt-felügyelet az ügyféltanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="c26be-137">Provide the cluster certificate thumbprint and the thumbprint of the client certificate that has been granted permissions for cluster management.</span></span> <span data-ttu-id="c26be-138">A tanúsítvány részleteinél meg kell egyeznie a fürtcsomópontokon egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c26be-138">The certificate details must match a certificate on the cluster nodes.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="c26be-139">*ServerCertThumbprint* az ujjlenyomatot a kiszolgálói tanúsítvány a fürtcsomóponton telepítve van.</span><span class="sxs-lookup"><span data-stu-id="c26be-139">*ServerCertThumbprint* is the thumbprint of the server certificate installed on the cluster nodes.</span></span> <span data-ttu-id="c26be-140">*Findvalue –* van a felügyeleti ügyfél tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="c26be-140">*FindValue* is the thumbprint of the admin client certificate.</span></span>
<span data-ttu-id="c26be-141">Ha a paraméter ki van töltve, a parancs néz ki a következő példa:</span><span class="sxs-lookup"><span data-stu-id="c26be-141">When the parameters are filled in, the command looks like the following example:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a><span data-ttu-id="c26be-142">Csatlakozás a Windows Active Directory használatával biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-142">Connect to a secure cluster using Windows Active Directory</span></span>
<span data-ttu-id="c26be-143">Ha a különálló fürt AD biztonsági használatával lett telepítve, csatlakozzon a fürthöz a következő kapcsolóhoz: "WindowsCredential" hozzáfűzésével.</span><span class="sxs-lookup"><span data-stu-id="c26be-143">If your standalone cluster is deployed using AD security, connect to the cluster by appending the switch "WindowsCredential".</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a><span data-ttu-id="c26be-144">Csatlakozzon a fürthöz, a FabricClient API-k használatával</span><span class="sxs-lookup"><span data-stu-id="c26be-144">Connect to a cluster using the FabricClient APIs</span></span>
<span data-ttu-id="c26be-145">A Service Fabric SDK biztosít a [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) a fürtkezeléshez osztály.</span><span class="sxs-lookup"><span data-stu-id="c26be-145">The Service Fabric SDK provides the [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) class for cluster management.</span></span> <span data-ttu-id="c26be-146">A FabricClient API-k használatához kérje a Microsoft.ServiceFabric NuGet csomag.</span><span class="sxs-lookup"><span data-stu-id="c26be-146">To use the FabricClient APIs, get the Microsoft.ServiceFabric NuGet package.</span></span>

### <a name="connect-to-an-unsecure-cluster"></a><span data-ttu-id="c26be-147">Csatlakozás nem biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-147">Connect to an unsecure cluster</span></span>

<span data-ttu-id="c26be-148">A csatlakozás nem biztonságos távoli fürthöz, FabricClient példány létrehozása, és adja meg a fürt címe:</span><span class="sxs-lookup"><span data-stu-id="c26be-148">To connect to a remote unsecured cluster, create a FabricClient instance and provide the cluster address:</span></span>

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

<span data-ttu-id="c26be-149">A fürtön belül futó kód, például egy megbízható szolgáltatásban hozzon létre egy FabricClient *nélkül* fürt címének megadása.</span><span class="sxs-lookup"><span data-stu-id="c26be-149">For code that is running from within a cluster, for example, in a Reliable Service, create a FabricClient *without* specifying the cluster address.</span></span> <span data-ttu-id="c26be-150">FabricClient csatlakozik a helyi felügyeleti átjáró a csomóponton, a kód jelenleg fut, szükségtelenné téve az extra hálózati ugrások.</span><span class="sxs-lookup"><span data-stu-id="c26be-150">FabricClient connects to the local management gateway on the node the code is currently running on, avoiding an extra network hop.</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="c26be-151">Csatlakozás egy ügyfél-tanúsítvány használatával biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-151">Connect to a secure cluster using a client certificate</span></span>

<span data-ttu-id="c26be-152">A fürt csomópontjaihoz rendelkeznie kell érvényes tanúsítványok közös névvel, vagy a SAN DNS-neve megtalálható a [RemoteCommonNames tulajdonság](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) be [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="c26be-152">The nodes in the cluster must have valid certificates whose common name or DNS name in SAN appears in the [RemoteCommonNames property](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) set on [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span></span> <span data-ttu-id="c26be-153">Ezt az eljárást lehetővé teszi, hogy az ügyfél és a fürt csomópontjai közötti kölcsönös hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="c26be-153">Following this process enables mutual authentication between the client and the cluster nodes.</span></span>

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

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a><span data-ttu-id="c26be-154">Csatlakozás Azure Active Directory használatával interaktívan biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-154">Connect to a secure cluster interactively using Azure Active Directory</span></span>

<span data-ttu-id="c26be-155">A következő példa az Azure Active Directory identitás- és a kiszolgáló ügyféltanúsítványt a kiszolgáló identitását.</span><span class="sxs-lookup"><span data-stu-id="c26be-155">The following example uses Azure Active Directory for client identity and server certificate for server identity.</span></span>

<span data-ttu-id="c26be-156">A párbeszédpanelen automatikusan ugrik fel a interaktív bejelentkezéshez a fürthöz való csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="c26be-156">A dialog window automatically pops up for interactive sign-in upon connecting to the cluster.</span></span>

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

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a><span data-ttu-id="c26be-157">Csatlakozás Azure Active Directory nem interaktív használatával biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-157">Connect to a secure cluster non-interactively using Azure Active Directory</span></span>

<span data-ttu-id="c26be-158">Az alábbi példa támaszkodik Microsoft.IdentityModel.Clients.ActiveDirectory, verzió: 2.19.208020213.</span><span class="sxs-lookup"><span data-stu-id="c26be-158">The following example relies on Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span></span>

<span data-ttu-id="c26be-159">Az AAD-jogkivonat megszerzése további információkért lásd: [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span><span class="sxs-lookup"><span data-stu-id="c26be-159">For more information on AAD token acquisition, see [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span></span>

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

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a><span data-ttu-id="c26be-160">Csatlakozás az Azure Active Directoryval előzetes metaadatok tudomása nélkül biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-160">Connect to a secure cluster without prior metadata knowledge using Azure Active Directory</span></span>

<span data-ttu-id="c26be-161">Az alábbi példában a nem interaktív token beszerzési, de ugyanezt a megközelítést lehet összeállításához kell használni a egyéni interaktív jogkivonat beszerzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c26be-161">The following example uses non-interactive token acquisition, but the same approach can be used to build a custom interactive token acquisition experience.</span></span> <span data-ttu-id="c26be-162">Az Azure Active Directory foglalásához szükséges metaadatokat jelenti a token beszerzése a fürtkonfiguráció olvasható.</span><span class="sxs-lookup"><span data-stu-id="c26be-162">The Azure Active Directory metadata needed for token acquisition is read from cluster configuration.</span></span>

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

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a><span data-ttu-id="c26be-163">Csatlakozás biztonságos fürthöz a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="c26be-163">Connect to a secure cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="c26be-164">Eléréséhez [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) egy adott fürtben, mutasson a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="c26be-164">To reach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) for a given cluster, point your browser to:</span></span>

`http://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="c26be-165">A teljes URL-címet is érhető el a fürt essentials ablaktábláján az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c26be-165">The full URL is also available in the cluster essentials pane of the Azure portal.</span></span>

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="c26be-166">Csatlakozás az Azure Active Directoryval biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-166">Connect to a secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="c26be-167">A fürt AAD-ben biztosított csatlakozhat, mutasson a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="c26be-167">To connect to a cluster that is secured with AAD, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="c26be-168">Automatikusan kell kéri aad-ben bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="c26be-168">You are automatically be prompted to log in with AAD.</span></span>

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="c26be-169">Csatlakozás egy ügyfél-tanúsítvány használatával biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="c26be-169">Connect to a secure cluster using a client certificate</span></span>

<span data-ttu-id="c26be-170">Szeretne csatlakozni egy fürt, amely a tanúsítványok által védett, mutasson a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="c26be-170">To connect to a cluster that is secured with certificates, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="c26be-171">Automatikusan kell kéri egy ügyféltanúsítványt választhat.</span><span class="sxs-lookup"><span data-stu-id="c26be-171">You are automatically be prompted to select a client certificate.</span></span>

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-the-remote-computer"></a><span data-ttu-id="c26be-172">A távoli számítógépen ügyfél tanúsítvány beállítása</span><span class="sxs-lookup"><span data-stu-id="c26be-172">Set up a client certificate on the remote computer</span></span>
<span data-ttu-id="c26be-173">Legalább két tanúsítványt használjon a fürt egy, a fürt és a kiszolgálói tanúsítvány és egy másik, az ügyfél hozzáférésének biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="c26be-173">At least two certificates should be used for securing the cluster, one for the cluster and server certificate and another for client access.</span></span>  <span data-ttu-id="c26be-174">Javasoljuk, hogy is használjon további másodlagos és az ügyfél-hozzáférési tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="c26be-174">We recommend that you also use additional secondary certificates and client access certificates.</span></span>  <span data-ttu-id="c26be-175">Az ügyfél és egy fürt csomópontja, használja a biztonsági tanúsítvány közötti kommunikáció védelme érdekében először beszerzése és telepíti az ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c26be-175">To secure the communication between a client and a cluster node using certificate security, you first need to obtain and install the client certificate.</span></span> <span data-ttu-id="c26be-176">A tanúsítványt a tárolóba személyes (a) a helyi számítógépen vagy az aktuális felhasználó telepítheti.</span><span class="sxs-lookup"><span data-stu-id="c26be-176">The certificate can be installed into the Personal (My) store of the local computer or the current user.</span></span>  <span data-ttu-id="c26be-177">A tanúsítvány ujjlenyomatát is, hogy az ügyfél képes hitelesíteni a fürt kell.</span><span class="sxs-lookup"><span data-stu-id="c26be-177">You also need the thumbprint of the server certificate so that the client can authenticate the cluster.</span></span>

<span data-ttu-id="c26be-178">Futtassa a következő PowerShell-parancsmag segítségével állítsa be az ügyféltanúsítványt a számítógépen, ahol a fürt eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c26be-178">Run the following PowerShell cmdlet to set up the client certificate on the computer from which you access the cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

<span data-ttu-id="c26be-179">Ha önaláírt tanúsítványt, meg kell importálni szeretné a számítógép "megbízható személyeknek" tárolóban, ezt a tanúsítványt a csatlakozni a biztonságos fürt használatba vétele előtt.</span><span class="sxs-lookup"><span data-stu-id="c26be-179">If it is a self-signed certificate, you need to import it to your machine's "trusted people" store before you can use this certificate to connect to a secure cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a><span data-ttu-id="c26be-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c26be-180">Next steps</span></span>

* [<span data-ttu-id="c26be-181">Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak</span><span class="sxs-lookup"><span data-stu-id="c26be-181">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="c26be-182">A Visual Studio a Service Fabric-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="c26be-182">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="c26be-183">Service Fabric állapotfigyelő modell bemutatása</span><span class="sxs-lookup"><span data-stu-id="c26be-183">Service Fabric Health model introduction</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="c26be-184">Alkalmazás biztonságának és RunAs</span><span class="sxs-lookup"><span data-stu-id="c26be-184">Application Security and RunAs</span></span>](service-fabric-application-runas-security.md)
* [<span data-ttu-id="c26be-185">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="c26be-185">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
