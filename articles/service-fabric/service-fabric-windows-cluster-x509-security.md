---
title: "aaaSecure egy Azure Service Fabric fürt Tanúsítványos használata Windows rendszeren |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toosecure kommunikációs belül hello önálló vagy titkos, valamint az ügyfelek és a hello fürt közötti fürt."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dekapur
ms.openlocfilehash: f0d411963615349a84edfc8125dec4ee5908f146
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a><span data-ttu-id="ce7ba-103">Önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos</span><span class="sxs-lookup"><span data-stu-id="ce7ba-103">Secure a standalone cluster on Windows using X.509 certificates</span></span>
<span data-ttu-id="ce7ba-104">Ez a cikk ismerteti, hogyan toosecure hello közötti kommunikáció hello a különálló Windows-fürt különböző csomópontok módjáról, valamint tooauthenticate csatlakozó-ügyfeleket toothis fürt X.509-tanúsítványokat használ.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-104">This article describes how toosecure hello communication between hello various nodes of your standalone Windows cluster, as well as how tooauthenticate clients connecting toothis cluster, using X.509 certificates.</span></span> <span data-ttu-id="ce7ba-105">Ez biztosítja, amelyek csak a jogosult felhasználók férhetnek hello fürt hello központilag telepített alkalmazások, és felügyeleti feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-105">This ensures that only authorized users can access hello cluster, hello deployed applications and perform management tasks.</span></span>  <span data-ttu-id="ce7ba-106">Tanúsítvány biztonsági engedélyezni kell a fürt hello hello fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-106">Certificate security should be enabled on hello cluster when hello cluster is created.</span></span>  

<span data-ttu-id="ce7ba-107">A fürt biztonsági például-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="ce7ba-107">For more information on cluster security such as node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

## <a name="which-certificates-will-you-need"></a><span data-ttu-id="ce7ba-108">Tanúsítványok kell?</span><span class="sxs-lookup"><span data-stu-id="ce7ba-108">Which certificates will you need?</span></span>
<span data-ttu-id="ce7ba-109">toostart, [hello önálló fürt csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) hello fürtben található csomópontok a tooone.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-109">toostart with, [download hello standalone cluster package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone of hello nodes in your cluster.</span></span> <span data-ttu-id="ce7ba-110">Hello a letöltött csomag, megtalálja a **ClusterConfig.X509.MultiMachine.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-110">In hello downloaded package, you will find a **ClusterConfig.X509.MultiMachine.json** file.</span></span> <span data-ttu-id="ce7ba-111">Nyissa meg hello fájlt, és tekintse át az hello területén **biztonsági** alatt hello **tulajdonságok** szakasz:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-111">Open hello file and review hello section for **security** under hello **properties** section:</span></span>

```JSON
"security": {
    "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

<span data-ttu-id="ce7ba-112">Ez a szakasz ismerteti, amelyekre szüksége van a különálló Windows-fürt védelmét biztosító hello tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-112">This section describes hello certificates that you need for securing your standalone Windows cluster.</span></span> <span data-ttu-id="ce7ba-113">Megadása esetén a fürt tanúsítvány, állítsa be hello **ClusterCredentialType** too_**X509**_. Külső kapcsolatok kiszolgálói tanúsítványának megadása esetén, állítsa be a hello **ServerCredentialType** túl_**X509**_. Bár nem kötelező, az ajánlott toohave mindkét ezek a tanúsítványok megfelelően védett fürt. Ha túl állítsa be ezeket az értékeket*X509* , majd a megfelelő tanúsítványok hello, vagy a Service Fabric rendszer kivételt jelez is meg kell adnia. Bizonyos esetekben előfordulhat, hogy csak szeretné toospecify hello _ClientCertificateThumbprints_ vagy _ReverseProxyCertificate_. Ezek a forgatókönyvek kell nem állíthatja _ClusterCredentialType_ vagy _ServerCredentialType_ too_X509_.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-113">If you are specifying a cluster certificate, set hello value of **ClusterCredentialType** too_**X509**_. If you are specifying server certificate for outside connections, set hello **ServerCredentialType** too_**X509**_. Although not mandatory, we recommend toohave both these certificates for a properly secured cluster. If you set these values too*X509* then you must also specify hello corresponding certificates or Service Fabric will throw an exception. In some scenarios, you may only want toospecify hello _ClientCertificateThumbprints_ or _ReverseProxyCertificate_. In those scenarios, you need not set _ClusterCredentialType_ or _ServerCredentialType_ too_X509_.</span></span>


> [!NOTE]
> <span data-ttu-id="ce7ba-114">A [ujjlenyomat](https://en.wikipedia.org/wiki/Public_key_fingerprint) hello elsődleges identitás-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-114">A [thumbprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) is hello primary identity of a certificate.</span></span> <span data-ttu-id="ce7ba-115">Olvasási [hogyan tanúsítvány ujjlenyomata tooretrieve](https://msdn.microsoft.com/library/ms734695.aspx) toofind ki az Ön által létrehozott hello tanúsítványok hello ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-115">Read [How tooretrieve thumbprint of a certificate](https://msdn.microsoft.com/library/ms734695.aspx) toofind out hello thumbprint of hello certificates that you create.</span></span>
> 
> 

<span data-ttu-id="ce7ba-116">hello következő tábla hello-tanúsítványokat sorolja fel a fürt beállítása lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-116">hello following table lists hello certificates that you will need on your cluster setup:</span></span>

| <span data-ttu-id="ce7ba-117">**CertificateInformation beállítás**</span><span class="sxs-lookup"><span data-stu-id="ce7ba-117">**CertificateInformation Setting**</span></span> | <span data-ttu-id="ce7ba-118">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="ce7ba-118">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="ce7ba-119">ClusterCertificate</span><span class="sxs-lookup"><span data-stu-id="ce7ba-119">ClusterCertificate</span></span> |<span data-ttu-id="ce7ba-120">Tesztelési környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-120">Recommended for test environment.</span></span> <span data-ttu-id="ce7ba-121">Ez a tanúsítvány szükséges toosecure hello kommunikációs fürt hello csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-121">This certificate is required toosecure hello communication between hello nodes on a cluster.</span></span> <span data-ttu-id="ce7ba-122">Frissítés két különböző tanúsítványok, egy elsődleges és másodlagos használható.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-122">You can use two different certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="ce7ba-123">Hello elsődleges tanúsítvány ujjlenyomata hello beállított hello **ujjlenyomat** szakasz és az hello a másodlagos hello **ThumbprintSecondary** változók.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-123">Set hello thumbprint of hello primary certificate in hello **Thumbprint** section and that of hello secondary in hello **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="ce7ba-124">ClusterCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="ce7ba-124">ClusterCertificateCommonNames</span></span> |<span data-ttu-id="ce7ba-125">Az éles környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-125">Recommended for production environment.</span></span> <span data-ttu-id="ce7ba-126">Ez a tanúsítvány szükséges toosecure hello kommunikációs fürt hello csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-126">This certificate is required toosecure hello communication between hello nodes on a cluster.</span></span> <span data-ttu-id="ce7ba-127">Egy vagy két fürt közös kiszolgálótanúsítvány-nevek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-127">You can use one or two cluster certificate common names.</span></span> |
| <span data-ttu-id="ce7ba-128">ServerCertificate</span><span class="sxs-lookup"><span data-stu-id="ce7ba-128">ServerCertificate</span></span> |<span data-ttu-id="ce7ba-129">Tesztelési környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-129">Recommended for test environment.</span></span> <span data-ttu-id="ce7ba-130">Ez a tanúsítvány toohello ügyfél áll rendelkezésre, amikor tooconnect toothis fürt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-130">This certificate is presented toohello client when it tries tooconnect toothis cluster.</span></span> <span data-ttu-id="ce7ba-131">Kényelmi célokat szolgál, dönthet úgy, toouse ugyanaz a tanúsítvány hello *ClusterCertificate* és *ServerCertificate*.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-131">For convenience, you can choose toouse hello same certificate for *ClusterCertificate* and *ServerCertificate*.</span></span> <span data-ttu-id="ce7ba-132">Frissítés két különböző kiszolgálói tanúsítványok, egy elsődleges és másodlagos használható.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-132">You can use two different server certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="ce7ba-133">Hello elsődleges tanúsítvány ujjlenyomata hello beállított hello **ujjlenyomat** szakasz és az hello a másodlagos hello **ThumbprintSecondary** változók.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-133">Set hello thumbprint of hello primary certificate in hello **Thumbprint** section and that of hello secondary in hello **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="ce7ba-134">ServerCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="ce7ba-134">ServerCertificateCommonNames</span></span> |<span data-ttu-id="ce7ba-135">Az éles környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-135">Recommended for production environment.</span></span> <span data-ttu-id="ce7ba-136">Ez a tanúsítvány toohello ügyfél áll rendelkezésre, amikor tooconnect toothis fürt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-136">This certificate is presented toohello client when it tries tooconnect toothis cluster.</span></span> <span data-ttu-id="ce7ba-137">Kényelmi célokat szolgál, dönthet úgy, toouse ugyanaz a tanúsítvány hello *ClusterCertificateCommonNames* és *ServerCertificateCommonNames*.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-137">For convenience, you can choose toouse hello same certificate for *ClusterCertificateCommonNames* and *ServerCertificateCommonNames*.</span></span> <span data-ttu-id="ce7ba-138">Használhat egy vagy két kiszolgálótanúsítványok köznapi neve.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-138">You can use one or two server certificate common names.</span></span> |
| <span data-ttu-id="ce7ba-139">ClientCertificateThumbprints</span><span class="sxs-lookup"><span data-stu-id="ce7ba-139">ClientCertificateThumbprints</span></span> |<span data-ttu-id="ce7ba-140">Ez olyan tanúsítványok, amelyet az tooinstall hitelesített hello ügyfeleken.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-140">This is a set of certificates that you want tooinstall on hello authenticated clients.</span></span> <span data-ttu-id="ce7ba-141">Számos különböző ügyfél-tanúsítványok telepítve, amelyet az tooallow hozzáférés toohello fürt hello gépen lehet.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-141">You can have a number of different client certificates installed on hello machines that you want tooallow access toohello cluster.</span></span> <span data-ttu-id="ce7ba-142">Minden tanúsítvány ujjlenyomata hello beállított hello **CertificateThumbprint** változó.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-142">Set hello thumbprint of each certificate in hello **CertificateThumbprint** variable.</span></span> <span data-ttu-id="ce7ba-143">Ha hello **IsAdmin** túl*igaz*, majd hello ügyfél telepítve van-e a tanúsítvánnyal is tegye a rendszergazda hello fürt felügyeleti tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-143">If you set hello **IsAdmin** too*true*, then hello client with this certificate installed on it can do administrator management activities on hello cluster.</span></span> <span data-ttu-id="ce7ba-144">Ha hello **IsAdmin** van *hamis*, ezzel a tanúsítvánnyal hello ügyfél csak az engedélyezett felhasználói hozzáférési jogosultságokat, általában csak olvasható hello műveleteket tudják végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-144">If hello **IsAdmin** is *false*, hello client with this certificate can only perform hello actions allowed for user access rights, typically read-only.</span></span> <span data-ttu-id="ce7ba-145">További információk a szerepkörök [szerepköralapú hozzáférés-vezérlést (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span><span class="sxs-lookup"><span data-stu-id="ce7ba-145">For more information on roles read [Role based access control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span></span> |
| <span data-ttu-id="ce7ba-146">ClientCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="ce7ba-146">ClientCertificateCommonNames</span></span> |<span data-ttu-id="ce7ba-147">Set hello köznapi neve hello első ügyféltanúsítvány hello **CertificateCommonName**.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-147">Set hello common name of hello first client certificate for hello **CertificateCommonName**.</span></span> <span data-ttu-id="ce7ba-148">Hello **CertificateIssuerThumbprint** hello ujjlenyomata a tanúsítvány kiállítója hello van.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-148">hello **CertificateIssuerThumbprint** is hello thumbprint for hello issuer of this certificate.</span></span> <span data-ttu-id="ce7ba-149">Olvasási [tanúsítványok használata](https://msdn.microsoft.com/library/ms731899.aspx) tooknow további információk a gyakori nevei és hello kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-149">Read [Working with certificates](https://msdn.microsoft.com/library/ms731899.aspx) tooknow more about common names and hello issuer.</span></span> |
| <span data-ttu-id="ce7ba-150">ReverseProxyCertificate</span><span class="sxs-lookup"><span data-stu-id="ce7ba-150">ReverseProxyCertificate</span></span> |<span data-ttu-id="ce7ba-151">Tesztelési környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-151">Recommended for test environment.</span></span> <span data-ttu-id="ce7ba-152">Ez az egy nem kötelező tanúsítvány, amely meg, ha azt szeretné, toosecure a [fordított Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="ce7ba-152">This is an optional certificate that can be specified if you want toosecure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="ce7ba-153">Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-153">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |
| <span data-ttu-id="ce7ba-154">ReverseProxyCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="ce7ba-154">ReverseProxyCertificateCommonNames</span></span> |<span data-ttu-id="ce7ba-155">Az éles környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-155">Recommended for production environment.</span></span> <span data-ttu-id="ce7ba-156">Ez az egy nem kötelező tanúsítvány, amely meg, ha azt szeretné, toosecure a [fordított Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="ce7ba-156">This is an optional certificate that can be specified if you want toosecure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="ce7ba-157">Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-157">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |

<span data-ttu-id="ce7ba-158">Itt található példa fürtkonfiguráció ahol hello fürt, a kiszolgáló és az ügyfél tanúsítványok vannak-e megadva.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-158">Here is example cluster configuration where hello Cluster, Server, and Client certificates have been provided.</span></span> <span data-ttu-id="ce7ba-159">Ne feledje, hogy a fürt / server / reverseProxy tanúsítványok, ujjlenyomat és köznapi név nem engedélyezett toobe konfigurálva együtt hello azonos tanúsítványtípus.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-159">Please note that for cluster/ server/ reverseProxy certificates, thumbprint and common name are not allowed toobe configured together for hello same cert type.</span></span>

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace hello localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-roll-over"></a><span data-ttu-id="ce7ba-160">Tanúsítvány váltása</span><span class="sxs-lookup"><span data-stu-id="ce7ba-160">Certificate roll over</span></span>
<span data-ttu-id="ce7ba-161">Tanúsítvány egyszerű neve helyett ujjlenyomat használatakor felett tanúsítványt összegző konfigurációs Fürtfrissítés nem igényel.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-161">When using certificate common name instead of thumbprint, certificate roll over doesn't require cluster configuration upgrade.</span></span>
<span data-ttu-id="ce7ba-162">Ha felett tanúsítványt összegző szerint kibocsátó váltása, ne hello régi kibocsátó cert hello tanúsítványtároló hello új kibocsátó tanúsítvány telepítése után legalább 2 órát.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-162">If certificate roll over involves issuer roll over, please keep hello old issuer cert in hello cert store at least 2 hours after installing hello new issuer cert.</span></span>

## <a name="acquire-hello-x509-certificates"></a><span data-ttu-id="ce7ba-163">Szerezzen be hello X.509-tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="ce7ba-163">Acquire hello X.509 certificates</span></span>
<span data-ttu-id="ce7ba-164">hello fürtön belüli kommunikáció toosecure, akkor először tooobtain X.509-tanúsítványokat a fürtcsomópontok esetén.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-164">toosecure communication within hello cluster, you will first need tooobtain X.509 certificates for your cluster nodes.</span></span> <span data-ttu-id="ce7ba-165">Emellett toolimit kapcsolat toothis fürt tooauthorized gépek/felhasználók esetén szeretne tooobtain kell majd hello ügyfélszámítógépeknél tanúsítványok telepítése.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-165">Additionally, toolimit connection toothis cluster tooauthorized machines/users, you will need tooobtain and install certificates for hello client machines.</span></span>

<span data-ttu-id="ce7ba-166">Termelési számítási feladatokhoz futtató fürtök esetén használjon egy [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority) X.509 tanúsítvány toosecure hello fürt aláírva.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-166">For clusters that are running production workloads, you should use a [Certificate Authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signed X.509 certificate toosecure hello cluster.</span></span> <span data-ttu-id="ce7ba-167">Ezek a tanúsítványok beszerzéséről részletekért lépjen túl[hogyan: tanúsítvány beszerzése](http://msdn.microsoft.com/library/aa702761.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce7ba-167">For details on obtaining these certificates, go too[How to: Obtain a Certificate](http://msdn.microsoft.com/library/aa702761.aspx).</span></span>

<span data-ttu-id="ce7ba-168">Tesztelési célokra használó fürtök esetén dönthet úgy toouse egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-168">For clusters that you use for test purposes, you can choose toouse a self-signed certificate.</span></span>

## <a name="optional-create-a-self-signed-certificate"></a><span data-ttu-id="ce7ba-169">Választható lehetőség: Hozzon létre egy önaláírt tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="ce7ba-169">Optional: Create a self-signed certificate</span></span>
<span data-ttu-id="ce7ba-170">Egyirányú toocreate egy önaláírt tanúsítványt megfelelően védett toouse hello *CertSetup.ps1* hello könyvtárban hello Service Fabric SDK mappában parancsfájl *C:\Program Files\Microsoft SDKs\Service Fabric\ ClusterSetup\Secure*.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-170">One way toocreate a self-signed cert that can be secured correctly is toouse hello *CertSetup.ps1* script in hello Service Fabric SDK folder in hello directory *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*.</span></span> <span data-ttu-id="ce7ba-171">Szerkessze a fájlt toochange hello alapértelmezett hello tanúsítvány neve (hello értéket keresi *CN = ServiceFabricDevClusterCert*).</span><span class="sxs-lookup"><span data-stu-id="ce7ba-171">Edit this file toochange hello default name of hello certificate (look for hello value *CN=ServiceFabricDevClusterCert*).</span></span> <span data-ttu-id="ce7ba-172">Futtassa ezt a parancsfájlt, `.\CertSetup.ps1 -Install`.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-172">Run this script as `.\CertSetup.ps1 -Install`.</span></span>

<span data-ttu-id="ce7ba-173">Mostantól exportálhatja hello tooa PFX tanúsítványfájl jelszóval védett.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-173">Now export hello certificate tooa PFX file with a protected password.</span></span> <span data-ttu-id="ce7ba-174">Először kapnak hello hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-174">First get hello thumbprint of hello certificate.</span></span> <span data-ttu-id="ce7ba-175">A hello *Start* menü, futtassa a hello *számítógép-tanúsítványok kezelése*.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-175">From hello *Start* menu, run hello *Manage computer certificates*.</span></span> <span data-ttu-id="ce7ba-176">Keresse meg a toohello **helyi számítógép személyes** létrehozott mappa és a keresés hello csak tanúsítvány meg.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-176">Navigate toohello **Local Computer\Personal** folder and find hello certificate you just created.</span></span> <span data-ttu-id="ce7ba-177">Kattintson duplán a hello tanúsítvány tooopen azt, jelölje be hello *részletek* lapot, és görgessen lefelé toohello *ujjlenyomat* mező.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-177">Double-click hello certificate tooopen it, select hello *Details* tab and scroll down toohello *Thumbprint* field.</span></span> <span data-ttu-id="ce7ba-178">Hello ujjlenyomat értékét másolja az alábbi PowerShell-paranccsal hello hello szóközök eltávolítása után.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-178">Copy hello thumbprint value into hello PowerShell command below, after removing hello spaces.</span></span>  <span data-ttu-id="ce7ba-179">Változás hello `String` tooa megfelelő biztonságos jelszó tooprotect értékét, és futtassa a következő PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-179">Change hello `String` value tooa suitable secure password tooprotect it and run hello following in PowerShell:</span></span>

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

<span data-ttu-id="ce7ba-180">hello telepített tanúsítványok toosee hello részleteit a gép akkor is futtatható a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-180">toosee hello details of a certificate installed on hello machine you can run hello following PowerShell command:</span></span>

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

<span data-ttu-id="ce7ba-181">Azt is megteheti, ha Azure-előfizetéssel, hajtsa végre a hello szakasz [tanúsítványok tooKey tároló hozzáadása](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span><span class="sxs-lookup"><span data-stu-id="ce7ba-181">Alternatively, if you have an Azure subscription, follow hello section [Add certificates tooKey Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span></span>

## <a name="install-hello-certificates"></a><span data-ttu-id="ce7ba-182">Hello tanúsítványok telepítése</span><span class="sxs-lookup"><span data-stu-id="ce7ba-182">Install hello certificates</span></span>
<span data-ttu-id="ce7ba-183">Miután (oka) t, telepítheti azokat hello fürtcsomópontokon.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-183">Once you have certificate(s), you can install them on hello cluster nodes.</span></span> <span data-ttu-id="ce7ba-184">A csomópontok kell toohave legújabb Windows PowerShell hello 3.x rajtuk.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-184">Your nodes need toohave hello latest Windows PowerShell 3.x installed on them.</span></span> <span data-ttu-id="ce7ba-185">Szüksége lesz toorepeat ezeket a lépéseket minden csomóponton, a fürt és a kiszolgáló és az összes másodlagos tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-185">You will need toorepeat these steps on each node, for both Cluster and Server certificates and any secondary certificates.</span></span>

1. <span data-ttu-id="ce7ba-186">Másolás hello .pfx (oka) t toohello csomópont.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-186">Copy hello .pfx file(s) toohello node.</span></span>
2. <span data-ttu-id="ce7ba-187">Nyissa meg rendszergazdaként a PowerShell ablakot, és írja be a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-187">Open a PowerShell window as an administrator and enter hello following commands.</span></span> <span data-ttu-id="ce7ba-188">Cserélje le a hello *$pswd* hello jelszó, amellyel toocreate ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-188">Replace hello *$pswd* with hello password that you used toocreate this certificate.</span></span> <span data-ttu-id="ce7ba-189">Cserélje le a hello *$PfxFilePath* hello .pfx másolt toothis csomópont hello a teljes elérési úttal.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-189">Replace hello *$PfxFilePath* with hello full path of hello .pfx copied toothis node.</span></span>
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. <span data-ttu-id="ce7ba-190">Most beállítása a hello ezt a tanúsítványt, hogy hello Service Fabric folyamat, amely hello hálózati szolgáltatás fiók alatt fut, használhatja a következő parancsfájl hello futtatásával.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-190">Now set hello access control on this certificate so that hello Service Fabric process, which runs under hello Network Service account, can use it by running hello following script.</span></span> <span data-ttu-id="ce7ba-191">Adja meg a hello tanúsítványt és a "Hálózati szolgáltatás" hello szolgáltatásfiók hello ujjlenyomatot.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-191">Provide hello thumbprint of hello certificate and "NETWORK SERVICE" for hello service account.</span></span> <span data-ttu-id="ce7ba-192">A hozzáférés-vezérlési listák hello hello tanúsítvány helyesek hello tanúsítványt megnyitásával ellenőrizheti *Start* > *számítógép-tanúsítványok kezelése* és megnézi *feladatok*  >  *Titkos kulcsok kezelése*.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-192">You can check that hello ACLs on hello certificate are correct by opening hello certificate in *Start* > *Manage computer certificates* and looking at *All Tasks* > *Manage Private Keys*.</span></span>
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify hello user, hello permissions and hello permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of hello machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get hello current acl of hello private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add hello new ace toohello acl of hello private key
    $acl.SetAccessRule($accessRule)
   
    # Write back hello new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe hello access rights currently assigned toothis certificate.
    get-acl $keyFullPath| fl
    ```
4. <span data-ttu-id="ce7ba-193">Ismételje meg a hello felett minden egyes kiszolgáló-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-193">Repeat hello steps above for each server certificate.</span></span> <span data-ttu-id="ce7ba-194">Használhatja ezen lépések tooinstall hello ügyféltanúsítványok hello gépeken, amelyet az tooallow hozzáférés toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-194">You can also use these steps tooinstall hello client certificates on hello machines that you want tooallow access toohello cluster.</span></span>

## <a name="create-hello-secure-cluster"></a><span data-ttu-id="ce7ba-195">Hello biztonságos fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce7ba-195">Create hello secure cluster</span></span>
<span data-ttu-id="ce7ba-196">Hello konfigurálása után **biztonsági** hello szakasza **ClusterConfig.X509.MultiMachine.json** fájl, a Folytatás túl[a fürt létrehozása](service-fabric-cluster-creation-for-windows-server.md#createcluster) szakasz tooconfigure hello csomópontot, majd hozzon létre hello önálló fürtöt.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-196">After configuring hello **security** section of hello **ClusterConfig.X509.MultiMachine.json** file, you can proceed too[Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section tooconfigure hello nodes and create hello standalone cluster.</span></span> <span data-ttu-id="ce7ba-197">Ne feledje toouse hello **ClusterConfig.X509.MultiMachine.json** fájl hello fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-197">Remember toouse hello **ClusterConfig.X509.MultiMachine.json** file while creating hello cluster.</span></span> <span data-ttu-id="ce7ba-198">A parancs például hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-198">For example, your command might look like hello following:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

<span data-ttu-id="ce7ba-199">Ha biztonságos hello önálló Windows sikeresen fut a fürt és telepítő hitelesített ügyfelek tooconnect tooit hello hajtsa végre az hello szakasz [Connect tooa biztonságos fürt PowerShell-lel](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-199">Once you have hello secure standalone Windows cluster successfully running, and have setup hello authenticated clients tooconnect tooit, follow hello section [Connect tooa secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit.</span></span> <span data-ttu-id="ce7ba-200">Példa:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-200">For example:</span></span>

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

<span data-ttu-id="ce7ba-201">Ezt követően futtathatja más PowerShell-parancsok toowork a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-201">You can then run other PowerShell commands toowork with this cluster.</span></span> <span data-ttu-id="ce7ba-202">Például [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow a biztonságos fürtön található csomópontok listáját.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-202">For example, [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow a list of nodes on this secure cluster.</span></span>


<span data-ttu-id="ce7ba-203">tooremove hello fürt, csatlakozás hello fürt, amelybe letöltötte a Service Fabric-csomag hello toohello csomópont, nyisson meg egy parancssort, és keresse meg a toohello csomagmappáihoz.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-203">tooremove hello cluster, connect toohello node on hello cluster where you downloaded hello Service Fabric package, open a command line and navigate toohello package folder.</span></span> <span data-ttu-id="ce7ba-204">Most futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="ce7ba-204">Now run hello following command:</span></span>

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> <span data-ttu-id="ce7ba-205">Tanúsítvány nem megfelelő konfigurációs megakadályozhatja, hogy a hello fürt üzembe helyezése során várható.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-205">Incorrect certificate configuration may prevent hello cluster from coming up during deployment.</span></span> <span data-ttu-id="ce7ba-206">tooself-biztonsági problémák elemzéséhez, tekintse meg a megjelenítő eseménycsoportban *alkalmazási és Szolgáltatásnaplójában* > *Microsoft-Service Fabric*.</span><span class="sxs-lookup"><span data-stu-id="ce7ba-206">tooself-diagnose security issues, please look in event viewer group *Applications and Services Logs* > *Microsoft-Service Fabric*.</span></span>
> 
> 

