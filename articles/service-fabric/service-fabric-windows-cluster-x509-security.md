---
title: "Az Azure Service Fabric-fürt a tanúsítványok használata a Windows biztonságos |} Microsoft Docs"
description: "Ez a cikk ismerteti az önálló vagy titkos, valamint az ügyfelek előbb és a fürtön belüli kommunikáció biztonságossá tételére."
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
ms.openlocfilehash: 71ece1e43cc3c4ac3350cd59633065de06672420
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a><span data-ttu-id="2de06-103">Önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos</span><span class="sxs-lookup"><span data-stu-id="2de06-103">Secure a standalone cluster on Windows using X.509 certificates</span></span>
<span data-ttu-id="2de06-104">A cikk ismerteti a különálló Windows fürt, a különböző csomópontok közötti kommunikáció védelméhez, valamint hogy használatával hitelesíti az ügyfeleket ehhez a fürthöz csatlakozik használ X.509 tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="2de06-104">This article describes how to secure the communication between the various nodes of your standalone Windows cluster, as well as how to authenticate clients connecting to this cluster, using X.509 certificates.</span></span> <span data-ttu-id="2de06-105">Ez biztosítja, hogy az csak a hitelesített felhasználóknak a fürthöz, a központilag telepített alkalmazások és felügyeleti feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="2de06-105">This ensures that only authorized users can access the cluster, the deployed applications and perform management tasks.</span></span>  <span data-ttu-id="2de06-106">Tanúsítvány biztonsági engedélyezni kell a fürt a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2de06-106">Certificate security should be enabled on the cluster when the cluster is created.</span></span>  

<span data-ttu-id="2de06-107">A fürt biztonsági például-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="2de06-107">For more information on cluster security such as node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

## <a name="which-certificates-will-you-need"></a><span data-ttu-id="2de06-108">Tanúsítványok kell?</span><span class="sxs-lookup"><span data-stu-id="2de06-108">Which certificates will you need?</span></span>
<span data-ttu-id="2de06-109">Kezdő-és [a különálló fürt csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) a fürtben található csomópontok egyikére.</span><span class="sxs-lookup"><span data-stu-id="2de06-109">To start with, [download the standalone cluster package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) to one of the nodes in your cluster.</span></span> <span data-ttu-id="2de06-110">A letöltött csomagban található egy **ClusterConfig.X509.MultiMachine.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2de06-110">In the downloaded package, you will find a **ClusterConfig.X509.MultiMachine.json** file.</span></span> <span data-ttu-id="2de06-111">Nyissa meg a fájlt, és tekintse át a szakasz **biztonsági** alatt a **tulajdonságok** szakasz:</span><span class="sxs-lookup"><span data-stu-id="2de06-111">Open the file and review the section for **security** under the **properties** section:</span></span>

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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

<span data-ttu-id="2de06-112">Ez a szakasz ismerteti a tanúsítványokat, amelyekre szüksége van a különálló Windows-fürt biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="2de06-112">This section describes the certificates that you need for securing your standalone Windows cluster.</span></span> <span data-ttu-id="2de06-113">Megadása esetén a fürt tanúsítvány, állítsa be a **ClusterCredentialType** való  _**X509**_.</span><span class="sxs-lookup"><span data-stu-id="2de06-113">If you are specifying a cluster certificate, set the value of **ClusterCredentialType** to _**X509**_.</span></span> <span data-ttu-id="2de06-114">Külső kapcsolatok kiszolgálói tanúsítványának megadása esetén, állítsa be a **ServerCredentialType** való  _**X509**_.</span><span class="sxs-lookup"><span data-stu-id="2de06-114">If you are specifying server certificate for outside connections, set the **ServerCredentialType** to _**X509**_.</span></span> <span data-ttu-id="2de06-115">Bár nem kötelező, ajánlott mindkét ezek a tanúsítványok megfelelően védett fürt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2de06-115">Although not mandatory, we recommend to have both these certificates for a properly secured cluster.</span></span> <span data-ttu-id="2de06-116">Ha ezeket az értékeket állít *X509* akkor is meg kell adnia a megfelelő tanúsítványok és a Service Fabric kivételt jelez.</span><span class="sxs-lookup"><span data-stu-id="2de06-116">If you set these values to *X509* then you must also specify the corresponding certificates or Service Fabric will throw an exception.</span></span> <span data-ttu-id="2de06-117">Bizonyos esetekben előfordulhat, hogy csak szeretne megadni a _ClientCertificateThumbprints_ vagy _ReverseProxyCertificate_.</span><span class="sxs-lookup"><span data-stu-id="2de06-117">In some scenarios, you may only want to specify the _ClientCertificateThumbprints_ or _ReverseProxyCertificate_.</span></span> <span data-ttu-id="2de06-118">Ezek a forgatókönyvek kell nem állíthatja _ClusterCredentialType_ vagy _ServerCredentialType_ való _X509_.</span><span class="sxs-lookup"><span data-stu-id="2de06-118">In those scenarios, you need not set _ClusterCredentialType_ or _ServerCredentialType_ to _X509_.</span></span>


> [!NOTE]
> <span data-ttu-id="2de06-119">A [ujjlenyomat](https://en.wikipedia.org/wiki/Public_key_fingerprint) tanúsítvány elsődleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2de06-119">A [thumbprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) is the primary identity of a certificate.</span></span> <span data-ttu-id="2de06-120">Olvasási [hogyan lehet lekérni a tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695.aspx) tudja meg a létrehozott tanúsítványok ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="2de06-120">Read [How to retrieve thumbprint of a certificate](https://msdn.microsoft.com/library/ms734695.aspx) to find out the thumbprint of the certificates that you create.</span></span>
> 
> 

<span data-ttu-id="2de06-121">Az alábbi táblázat a tanúsítványok, a fürt beállítása lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="2de06-121">The following table lists the certificates that you will need on your cluster setup:</span></span>

| <span data-ttu-id="2de06-122">**CertificateInformation beállítás**</span><span class="sxs-lookup"><span data-stu-id="2de06-122">**CertificateInformation Setting**</span></span> | <span data-ttu-id="2de06-123">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="2de06-123">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="2de06-124">ClusterCertificate</span><span class="sxs-lookup"><span data-stu-id="2de06-124">ClusterCertificate</span></span> |<span data-ttu-id="2de06-125">Tesztelési környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2de06-125">Recommended for test environment.</span></span> <span data-ttu-id="2de06-126">Ez a tanúsítvány szükséges a fürt a csomópontok közötti kommunikáció biztonságossá tételére.</span><span class="sxs-lookup"><span data-stu-id="2de06-126">This certificate is required to secure the communication between the nodes on a cluster.</span></span> <span data-ttu-id="2de06-127">Frissítés két különböző tanúsítványok, egy elsődleges és másodlagos használható.</span><span class="sxs-lookup"><span data-stu-id="2de06-127">You can use two different certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="2de06-128">Állítsa be az elsődleges tanúsítvány ujjlenyomatát a **ujjlenyomat** szakaszt, és hogy a másodlagos a **ThumbprintSecondary** változók.</span><span class="sxs-lookup"><span data-stu-id="2de06-128">Set the thumbprint of the primary certificate in the **Thumbprint** section and that of the secondary in the **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="2de06-129">ClusterCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="2de06-129">ClusterCertificateCommonNames</span></span> |<span data-ttu-id="2de06-130">Az éles környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2de06-130">Recommended for production environment.</span></span> <span data-ttu-id="2de06-131">Ez a tanúsítvány szükséges a fürt a csomópontok közötti kommunikáció biztonságossá tételére.</span><span class="sxs-lookup"><span data-stu-id="2de06-131">This certificate is required to secure the communication between the nodes on a cluster.</span></span> <span data-ttu-id="2de06-132">Egy vagy két fürt közös kiszolgálótanúsítvány-nevek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2de06-132">You can use one or two cluster certificate common names.</span></span> |
| <span data-ttu-id="2de06-133">ServerCertificate</span><span class="sxs-lookup"><span data-stu-id="2de06-133">ServerCertificate</span></span> |<span data-ttu-id="2de06-134">Tesztelési környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2de06-134">Recommended for test environment.</span></span> <span data-ttu-id="2de06-135">Ezt a tanúsítványt az ügyfél áll rendelkezésre, ha csatlakozik a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2de06-135">This certificate is presented to the client when it tries to connect to this cluster.</span></span> <span data-ttu-id="2de06-136">Kényelmi célokat szolgál, ha szeretné, használja ugyanazt a tanúsítványt a *ClusterCertificate* és *ServerCertificate*.</span><span class="sxs-lookup"><span data-stu-id="2de06-136">For convenience, you can choose to use the same certificate for *ClusterCertificate* and *ServerCertificate*.</span></span> <span data-ttu-id="2de06-137">Frissítés két különböző kiszolgálói tanúsítványok, egy elsődleges és másodlagos használható.</span><span class="sxs-lookup"><span data-stu-id="2de06-137">You can use two different server certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="2de06-138">Állítsa be az elsődleges tanúsítvány ujjlenyomatát a **ujjlenyomat** szakaszt, és hogy a másodlagos a **ThumbprintSecondary** változók.</span><span class="sxs-lookup"><span data-stu-id="2de06-138">Set the thumbprint of the primary certificate in the **Thumbprint** section and that of the secondary in the **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="2de06-139">ServerCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="2de06-139">ServerCertificateCommonNames</span></span> |<span data-ttu-id="2de06-140">Az éles környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2de06-140">Recommended for production environment.</span></span> <span data-ttu-id="2de06-141">Ezt a tanúsítványt az ügyfél áll rendelkezésre, ha csatlakozik a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2de06-141">This certificate is presented to the client when it tries to connect to this cluster.</span></span> <span data-ttu-id="2de06-142">Kényelmi célokat szolgál, ha szeretné, használja ugyanazt a tanúsítványt a *ClusterCertificateCommonNames* és *ServerCertificateCommonNames*.</span><span class="sxs-lookup"><span data-stu-id="2de06-142">For convenience, you can choose to use the same certificate for *ClusterCertificateCommonNames* and *ServerCertificateCommonNames*.</span></span> <span data-ttu-id="2de06-143">Használhat egy vagy két kiszolgálótanúsítványok köznapi neve.</span><span class="sxs-lookup"><span data-stu-id="2de06-143">You can use one or two server certificate common names.</span></span> |
| <span data-ttu-id="2de06-144">ClientCertificateThumbprints</span><span class="sxs-lookup"><span data-stu-id="2de06-144">ClientCertificateThumbprints</span></span> |<span data-ttu-id="2de06-145">Ez olyan tanúsítványokat, amelyek a hitelesített ügyfelek telepíteni szeretné.</span><span class="sxs-lookup"><span data-stu-id="2de06-145">This is a set of certificates that you want to install on the authenticated clients.</span></span> <span data-ttu-id="2de06-146">Akkor segítségével számos különböző ügyfél-tanúsítványok engedélyezi a hozzáférést a fürthöz használni kívánt számítógépeken telepítve van.</span><span class="sxs-lookup"><span data-stu-id="2de06-146">You can have a number of different client certificates installed on the machines that you want to allow access to the cluster.</span></span> <span data-ttu-id="2de06-147">Állítsa be a minden tanúsítvány ujjlenyomatát a **CertificateThumbprint** változó.</span><span class="sxs-lookup"><span data-stu-id="2de06-147">Set the thumbprint of each certificate in the **CertificateThumbprint** variable.</span></span> <span data-ttu-id="2de06-148">Ha a **IsAdmin** való *igaz*, majd az ügyfél és a tanúsítvány telepítve van-e is tegye a rendszergazda a fürt felügyeleti tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="2de06-148">If you set the **IsAdmin** to *true*, then the client with this certificate installed on it can do administrator management activities on the cluster.</span></span> <span data-ttu-id="2de06-149">Ha a **IsAdmin** van *hamis*, az ügyfél és a tanúsítvány csak a felhasználói hozzáférési jogosultságokat, általában csak olvasható engedélyezett műveleteket hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="2de06-149">If the **IsAdmin** is *false*, the client with this certificate can only perform the actions allowed for user access rights, typically read-only.</span></span> <span data-ttu-id="2de06-150">További információk a szerepkörök [szerepköralapú hozzáférés-vezérlést (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span><span class="sxs-lookup"><span data-stu-id="2de06-150">For more information on roles read [Role based access control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span></span> |
| <span data-ttu-id="2de06-151">ClientCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="2de06-151">ClientCertificateCommonNames</span></span> |<span data-ttu-id="2de06-152">Az első ügyféltanúsítványt köznapi nevének beállítása a **CertificateCommonName**.</span><span class="sxs-lookup"><span data-stu-id="2de06-152">Set the common name of the first client certificate for the **CertificateCommonName**.</span></span> <span data-ttu-id="2de06-153">A **CertificateIssuerThumbprint** az ujjlenyomat van, ezt a tanúsítványt kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="2de06-153">The **CertificateIssuerThumbprint** is the thumbprint for the issuer of this certificate.</span></span> <span data-ttu-id="2de06-154">Olvasási [tanúsítványok használata](https://msdn.microsoft.com/library/ms731899.aspx) további információkat a gyakori nevei és a kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="2de06-154">Read [Working with certificates](https://msdn.microsoft.com/library/ms731899.aspx) to know more about common names and the issuer.</span></span> |
| <span data-ttu-id="2de06-155">ReverseProxyCertificate</span><span class="sxs-lookup"><span data-stu-id="2de06-155">ReverseProxyCertificate</span></span> |<span data-ttu-id="2de06-156">Tesztelési környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2de06-156">Recommended for test environment.</span></span> <span data-ttu-id="2de06-157">Ez az egy nem kötelező tanúsítvány, amely meg, hogy szeretné-e biztonságos a [fordított Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="2de06-157">This is an optional certificate that can be specified if you want to secure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="2de06-158">Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="2de06-158">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |
| <span data-ttu-id="2de06-159">ReverseProxyCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="2de06-159">ReverseProxyCertificateCommonNames</span></span> |<span data-ttu-id="2de06-160">Az éles környezetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2de06-160">Recommended for production environment.</span></span> <span data-ttu-id="2de06-161">Ez az egy nem kötelező tanúsítvány, amely meg, hogy szeretné-e biztonságos a [fordított Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="2de06-161">This is an optional certificate that can be specified if you want to secure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="2de06-162">Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="2de06-162">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |

<span data-ttu-id="2de06-163">Íme példa fürtkonfiguráció, ahol a fürt, a kiszolgáló és az ügyfél tanúsítványok vannak-e megadva.</span><span class="sxs-lookup"><span data-stu-id="2de06-163">Here is example cluster configuration where the Cluster, Server, and Client certificates have been provided.</span></span> <span data-ttu-id="2de06-164">Ne feledje, hogy a fürt / server / reverseProxy tanúsítványok, ujjlenyomat és köznapi név nem engedélyezett cert ugyanolyan együtt konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="2de06-164">Please note that for cluster/ server/ reverseProxy certificates, thumbprint and common name are not allowed to be configured together for the same cert type.</span></span>

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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

## <a name="certificate-roll-over"></a><span data-ttu-id="2de06-165">Tanúsítvány váltása</span><span class="sxs-lookup"><span data-stu-id="2de06-165">Certificate roll over</span></span>
<span data-ttu-id="2de06-166">Tanúsítvány egyszerű neve helyett ujjlenyomat használatakor felett tanúsítványt összegző konfigurációs Fürtfrissítés nem igényel.</span><span class="sxs-lookup"><span data-stu-id="2de06-166">When using certificate common name instead of thumbprint, certificate roll over doesn't require cluster configuration upgrade.</span></span>
<span data-ttu-id="2de06-167">Ha felett tanúsítványt összegző szerint kibocsátó váltása, tartsa a régi kibocsátó cert a CERT tárolja az új kibocsátó tanúsítvány telepítése után legalább 2 órát.</span><span class="sxs-lookup"><span data-stu-id="2de06-167">If certificate roll over involves issuer roll over, please keep the old issuer cert in the cert store at least 2 hours after installing the new issuer cert.</span></span>

## <a name="acquire-the-x509-certificates"></a><span data-ttu-id="2de06-168">Szerezzen be X.509-tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="2de06-168">Acquire the X.509 certificates</span></span>
<span data-ttu-id="2de06-169">A fürtön belüli kommunikáció védelméhez, először a fürtcsomópontok esetén, X.509-tanúsítványokat szerezzenek be.</span><span class="sxs-lookup"><span data-stu-id="2de06-169">To secure communication within the cluster, you will first need to obtain X.509 certificates for your cluster nodes.</span></span> <span data-ttu-id="2de06-170">Továbbá a jogosult felhasználók vagy gépek fürthöz kapcsolat korlátozására, szüksége lesz beszerzése és az ügyfél gépek tanúsítványok telepítése.</span><span class="sxs-lookup"><span data-stu-id="2de06-170">Additionally, to limit connection to this cluster to authorized machines/users, you will need to obtain and install certificates for the client machines.</span></span>

<span data-ttu-id="2de06-171">Termelési számítási feladatokhoz futtató fürtök esetén használjon egy [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority) védelméhez a fürt X.509 tanúsítvány aláírására használatos.</span><span class="sxs-lookup"><span data-stu-id="2de06-171">For clusters that are running production workloads, you should use a [Certificate Authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signed X.509 certificate to secure the cluster.</span></span> <span data-ttu-id="2de06-172">Ezek a tanúsítványok beszerzéséről további információkért látogasson el [hogyan: tanúsítvány beszerzése](http://msdn.microsoft.com/library/aa702761.aspx).</span><span class="sxs-lookup"><span data-stu-id="2de06-172">For details on obtaining these certificates, go to [How to: Obtain a Certificate](http://msdn.microsoft.com/library/aa702761.aspx).</span></span>

<span data-ttu-id="2de06-173">Tesztelési célokra használó fürtök esetén dönthet úgy, önaláírt tanúsítvány használatára.</span><span class="sxs-lookup"><span data-stu-id="2de06-173">For clusters that you use for test purposes, you can choose to use a self-signed certificate.</span></span>

## <a name="optional-create-a-self-signed-certificate"></a><span data-ttu-id="2de06-174">Választható lehetőség: Hozzon létre egy önaláírt tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="2de06-174">Optional: Create a self-signed certificate</span></span>
<span data-ttu-id="2de06-175">Egy módja megfelelően védett önaláírt tanúsítványt létrehozni a *CertSetup.ps1* parancsfájl a könyvtárban, a Service Fabric SDK mappában *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\ Biztonságos*.</span><span class="sxs-lookup"><span data-stu-id="2de06-175">One way to create a self-signed cert that can be secured correctly is to use the *CertSetup.ps1* script in the Service Fabric SDK folder in the directory *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*.</span></span> <span data-ttu-id="2de06-176">Szerkessze a fájlt a tanúsítványhoz tartozó alapértelmezett nevének módosításához (kívánt érték *CN = ServiceFabricDevClusterCert*).</span><span class="sxs-lookup"><span data-stu-id="2de06-176">Edit this file to change the default name of the certificate (look for the value *CN=ServiceFabricDevClusterCert*).</span></span> <span data-ttu-id="2de06-177">Futtassa ezt a parancsfájlt, `.\CertSetup.ps1 -Install`.</span><span class="sxs-lookup"><span data-stu-id="2de06-177">Run this script as `.\CertSetup.ps1 -Install`.</span></span>

<span data-ttu-id="2de06-178">Exportálja a tanúsítványt egy PFX-fájl jelszóval védett.</span><span class="sxs-lookup"><span data-stu-id="2de06-178">Now export the certificate to a PFX file with a protected password.</span></span> <span data-ttu-id="2de06-179">Először kapnak a tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="2de06-179">First get the thumbprint of the certificate.</span></span> <span data-ttu-id="2de06-180">Az a *Start* menü, futtassa a *számítógép-tanúsítványok kezelése*.</span><span class="sxs-lookup"><span data-stu-id="2de06-180">From the *Start* menu, run the *Manage computer certificates*.</span></span> <span data-ttu-id="2de06-181">Keresse meg a **helyi számítógép személyes** mappa és a Keresés az imént tanúsítvány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2de06-181">Navigate to the **Local Computer\Personal** folder and find the certificate you just created.</span></span> <span data-ttu-id="2de06-182">Kattintson duplán a tanúsítványra megnyitásához, jelölje be a *részletek* lapra, és görgessen le a *ujjlenyomat* mező.</span><span class="sxs-lookup"><span data-stu-id="2de06-182">Double-click the certificate to open it, select the *Details* tab and scroll down to the *Thumbprint* field.</span></span> <span data-ttu-id="2de06-183">A szóközök eltávolítása után a PowerShell-parancsot, másolja az ujjlenyomat értékét.</span><span class="sxs-lookup"><span data-stu-id="2de06-183">Copy the thumbprint value into the PowerShell command below, after removing the spaces.</span></span>  <span data-ttu-id="2de06-184">Módosítsa a `String` a védelmét, és futtassa a következő PowerShell megfelelő biztonságos jelszó értéket:</span><span class="sxs-lookup"><span data-stu-id="2de06-184">Change the `String` value to a suitable secure password to protect it and run the following in PowerShell:</span></span>

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

<span data-ttu-id="2de06-185">A Részletek területen találja a számítógépen telepített tanúsítvány futtathatja a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="2de06-185">To see the details of a certificate installed on the machine you can run the following PowerShell command:</span></span>

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

<span data-ttu-id="2de06-186">Azt is megteheti, ha Azure-előfizetéssel rendelkezik, kövesse a szakasz [tanúsítványok hozzáadása a Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span><span class="sxs-lookup"><span data-stu-id="2de06-186">Alternatively, if you have an Azure subscription, follow the section [Add certificates to Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span></span>

## <a name="install-the-certificates"></a><span data-ttu-id="2de06-187">A tanúsítványok telepítése</span><span class="sxs-lookup"><span data-stu-id="2de06-187">Install the certificates</span></span>
<span data-ttu-id="2de06-188">Miután (oka) t, telepítheti azokat a fürtcsomópontokon.</span><span class="sxs-lookup"><span data-stu-id="2de06-188">Once you have certificate(s), you can install them on the cluster nodes.</span></span> <span data-ttu-id="2de06-189">A csomópontok kell rendelkeznie a legújabb Windows PowerShell 3.x rajtuk.</span><span class="sxs-lookup"><span data-stu-id="2de06-189">Your nodes need to have the latest Windows PowerShell 3.x installed on them.</span></span> <span data-ttu-id="2de06-190">Ismételje meg ezeket a lépéseket minden csomóponton, a fürt és a kiszolgáló és az összes másodlagos tanúsítványokat kell.</span><span class="sxs-lookup"><span data-stu-id="2de06-190">You will need to repeat these steps on each node, for both Cluster and Server certificates and any secondary certificates.</span></span>

1. <span data-ttu-id="2de06-191">A .pfx fájl átmásolása a csomópont.</span><span class="sxs-lookup"><span data-stu-id="2de06-191">Copy the .pfx file(s) to the node.</span></span>
2. <span data-ttu-id="2de06-192">Nyissa meg rendszergazdaként a PowerShell ablakot, és írja be a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2de06-192">Open a PowerShell window as an administrator and enter the following commands.</span></span> <span data-ttu-id="2de06-193">Cserélje le a *$pswd* a, melyek a tanúsítvány létrehozásához használt jelszó.</span><span class="sxs-lookup"><span data-stu-id="2de06-193">Replace the *$pswd* with the password that you used to create this certificate.</span></span> <span data-ttu-id="2de06-194">Cserélje le a *$PfxFilePath* a teljes elérési útját a .pfx másolja ezt a csomópontot.</span><span class="sxs-lookup"><span data-stu-id="2de06-194">Replace the *$PfxFilePath* with the full path of the .pfx copied to this node.</span></span>
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. <span data-ttu-id="2de06-195">Most beállítása az ezt a tanúsítványt, hogy a Service Fabric folyamat, amely a hálózati szolgáltatás fiók alatt fut, használhatja a következő parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2de06-195">Now set the access control on this certificate so that the Service Fabric process, which runs under the Network Service account, can use it by running the following script.</span></span> <span data-ttu-id="2de06-196">Adja meg az ujjlenyomatot, a tanúsítványt és a "Hálózati szolgáltatás" a szolgáltatás fiók.</span><span class="sxs-lookup"><span data-stu-id="2de06-196">Provide the thumbprint of the certificate and "NETWORK SERVICE" for the service account.</span></span> <span data-ttu-id="2de06-197">Ellenőrizheti, hogy a tanúsítvány az ACL-ek nyissa meg a tanúsítvány helyesen *Start* > *számítógép-tanúsítványok kezelése* és megnézi *feladataival*  >  *Titkos kulcsok kezelése*.</span><span class="sxs-lookup"><span data-stu-id="2de06-197">You can check that the ACLs on the certificate are correct by opening the certificate in *Start* > *Manage computer certificates* and looking at *All Tasks* > *Manage Private Keys*.</span></span>
   
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
   
    # Specify the user, the permissions and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current acl of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ace to the acl of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate.
    get-acl $keyFullPath| fl
    ```
4. <span data-ttu-id="2de06-198">Ismételje meg a fenti lépéseket minden kiszolgálói tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="2de06-198">Repeat the steps above for each server certificate.</span></span> <span data-ttu-id="2de06-199">A lépések segítségével telepítse az ügyféltanúsítványokat az engedélyezi a hozzáférést a fürthöz használni kívánt gépekre.</span><span class="sxs-lookup"><span data-stu-id="2de06-199">You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.</span></span>

## <a name="create-the-secure-cluster"></a><span data-ttu-id="2de06-200">A biztonságos fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2de06-200">Create the secure cluster</span></span>
<span data-ttu-id="2de06-201">Beállítása után a **biztonsági** szakasza a **ClusterConfig.X509.MultiMachine.json** fájl, folytathatja a [a fürt létrehozása](service-fabric-cluster-creation-for-windows-server.md#createcluster) szakasz a csomópontok és a különálló fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2de06-201">After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster.</span></span> <span data-ttu-id="2de06-202">Fontos, hogy a **ClusterConfig.X509.MultiMachine.json** fájl a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="2de06-202">Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster.</span></span> <span data-ttu-id="2de06-203">Például a parancs nézhet ki például a következőket:</span><span class="sxs-lookup"><span data-stu-id="2de06-203">For example, your command might look like the following:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

<span data-ttu-id="2de06-204">Miután a fürt sikeresen fut, és a hitelesített ügyfelek csatlakoznak, hogy a telepítő a biztonságos önálló Windows, kövesse a szakasz [csatlakozás PowerShell használatával biztonságos fürthöz](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) a csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="2de06-204">Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it.</span></span> <span data-ttu-id="2de06-205">Példa:</span><span class="sxs-lookup"><span data-stu-id="2de06-205">For example:</span></span>

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

<span data-ttu-id="2de06-206">Ezt követően futtathatja más PowerShell-parancsok használata ehhez a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2de06-206">You can then run other PowerShell commands to work with this cluster.</span></span> <span data-ttu-id="2de06-207">Például [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) való megjelenítéséhez a biztonságos fürt a csomópontok listáját.</span><span class="sxs-lookup"><span data-stu-id="2de06-207">For example, [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) to show a list of nodes on this secure cluster.</span></span>


<span data-ttu-id="2de06-208">Távolítsa el a fürtöt, a csomópont a fürt, amelybe letöltötte a Service Fabric-csomag csatlakozzon, nyisson meg egy parancssort, és keresse meg a csomag mappát.</span><span class="sxs-lookup"><span data-stu-id="2de06-208">To remove the cluster, connect to the node on the cluster where you downloaded the Service Fabric package, open a command line and navigate to the package folder.</span></span> <span data-ttu-id="2de06-209">Most futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2de06-209">Now run the following command:</span></span>

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> <span data-ttu-id="2de06-210">Tanúsítvány nem megfelelő konfigurációs megakadályozhatja, hogy a fürt üzembe helyezése során várható.</span><span class="sxs-lookup"><span data-stu-id="2de06-210">Incorrect certificate configuration may prevent the cluster from coming up during deployment.</span></span> <span data-ttu-id="2de06-211">Önálló biztonsági problémák elemzéséhez, tekintse meg a megjelenítő eseménycsoportban *alkalmazási és Szolgáltatásnaplójában* > *Microsoft-Service Fabric*.</span><span class="sxs-lookup"><span data-stu-id="2de06-211">To self-diagnose security issues, please look in event viewer group *Applications and Services Logs* > *Microsoft-Service Fabric*.</span></span>
> 
> 

