---
title: "a Service Fabric tároló biztonsági aaaAzure |} Microsoft Docs"
description: "Ismerje meg, most toosecure tárolószolgáltatásainak."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 88faf4e8f949c2f7743756b6272ca672d9710630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-security"></a><span data-ttu-id="6998c-103">Tároló biztonsági</span><span class="sxs-lookup"><span data-stu-id="6998c-103">Container security</span></span>

<span data-ttu-id="6998c-104">A Service Fabric lehetővé teszi a szolgáltatások egy tároló tooaccess belül olyan tanúsítvány, amely a Windows vagy Linux rendszerű fürt (5.7-es vagy újabb verzió) hello csomópontjára telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6998c-104">Service Fabric provides a mechanism for services inside a container tooaccess a certificate that is installed on hello nodes in a Windows or Linux cluster (version 5.7 or higher).</span></span> <span data-ttu-id="6998c-105">Emellett a Service Fabric is támogatja csoportosan felügyelt szolgáltatásfiók (csoportosan felügyelt szolgáltatásfiókok) Windows tárolók.</span><span class="sxs-lookup"><span data-stu-id="6998c-105">In addition, Service Fabric also supports gMSA (group Managed Service Accounts) for Windows containers.</span></span> 

## <a name="certificate-management-for-containers"></a><span data-ttu-id="6998c-106">Az tárolókat Tanúsítványkezelő</span><span class="sxs-lookup"><span data-stu-id="6998c-106">Certificate management for containers</span></span>

<span data-ttu-id="6998c-107">A tároló szolgáltatások tanúsítvány megadásával biztosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6998c-107">You can secure your container services by specifying a certificate.</span></span> <span data-ttu-id="6998c-108">hello tanúsítvány hello fürt hello csomópontjára telepítenie kell.</span><span class="sxs-lookup"><span data-stu-id="6998c-108">hello certificate must be installed on hello nodes of hello cluster.</span></span> <span data-ttu-id="6998c-109">hello tanúsítvány információ alapján hello hello alkalmazásjegyzékben `ContainerHostPolicies` címke, a következő kódrészletet mutat be hello:</span><span class="sxs-lookup"><span data-stu-id="6998c-109">hello certificate information is provided in hello application manifest under hello `ContainerHostPolicies` tag as hello following snippet shows:</span></span>

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

<span data-ttu-id="6998c-110">Hello alkalmazás indításakor hello futásidejű hello tanúsítványok olvas, és a PFX-fájlt, és minden tanúsítvány jelszava.</span><span class="sxs-lookup"><span data-stu-id="6998c-110">When starting hello application, hello runtime reads hello certificates and generates a PFX file and password for each certificate.</span></span> <span data-ttu-id="6998c-111">A PFX-fájlt és a jelszó használata a következő környezeti változók hello hello tároló belül érhetők el:</span><span class="sxs-lookup"><span data-stu-id="6998c-111">This PFX file and password are accessible inside hello container using hello following environment variables:</span></span> 

* <span data-ttu-id="6998c-112">**[CodePackageName] Certificate_ _ [CertName] _PFX**</span><span class="sxs-lookup"><span data-stu-id="6998c-112">**Certificate_[CodePackageName]_[CertName]_PFX**</span></span>
* <span data-ttu-id="6998c-113">**[CodePackageName] Certificate_ _ [CertName] _Password**</span><span class="sxs-lookup"><span data-stu-id="6998c-113">**Certificate_[CodePackageName]_[CertName]_Password**</span></span>

<span data-ttu-id="6998c-114">hello tároló szolgáltatás vagy folyamat felelős a hello PFX-fájl importálása hello tárolóba.</span><span class="sxs-lookup"><span data-stu-id="6998c-114">hello container service or process is responsible for importing hello PFX file into hello container.</span></span> <span data-ttu-id="6998c-115">tooimport hello tanúsítvány, használhat `setupentrypoint.sh` a parancsfájlok vagy egyéni kód hello tároló folyamaton belül végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="6998c-115">tooimport hello certificate, you can use `setupentrypoint.sh` scripts or executed custom code within hello container process.</span></span> <span data-ttu-id="6998c-116">A következő példakód C# hello PFX-fájl importálásához:</span><span class="sxs-lookup"><span data-stu-id="6998c-116">Sample code in C# for importing hello PFX file follows:</span></span>

```c#
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
<span data-ttu-id="6998c-117">A PFX-tanúsítvány hitelesítése hello alkalmazás vagy szolgáltatás-vagy más szolgáltatásokkal biztonságos commmunication használható.</span><span class="sxs-lookup"><span data-stu-id="6998c-117">This PFX certificate can be used for authenticate hello application or service or secure commmunication with other services.</span></span>


## <a name="set-up-gmsa-for-windows-containers"></a><span data-ttu-id="6998c-118">Windows-tárolók csoportosan felügyelt szolgáltatásfiók beállítása</span><span class="sxs-lookup"><span data-stu-id="6998c-118">Set up gMSA for Windows containers</span></span>

<span data-ttu-id="6998c-119">csoportosan felügyelt szolgáltatásfiók (felügyelt szolgáltatásfiókok. csoport), a hitelesítő adatok megadását fájl mentése tooset (`credspec`) hello fürt összes csomópontján el van helyezve.</span><span class="sxs-lookup"><span data-stu-id="6998c-119">tooset up gMSA (group Managed Service Accounts), a credential specification file (`credspec`) is placed on all nodes in hello cluster.</span></span> <span data-ttu-id="6998c-120">hello fájlt is másolhatja az összes olyan csomóponton, a Virtuálisgép-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="6998c-120">hello file can be copied on all nodes using a VM extension.</span></span>  <span data-ttu-id="6998c-121">Hello `credspec` hello csoportosan felügyelt szolgáltatásfiók fiókadatok kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="6998c-121">hello `credspec` file must contain hello gMSA account information.</span></span> <span data-ttu-id="6998c-122">További információ a hello `credspec` fájlba, lásd: [szolgáltatásfiókok](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span><span class="sxs-lookup"><span data-stu-id="6998c-122">For more information on hello `credspec` file, see [Service Accounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span></span> <span data-ttu-id="6998c-123">hello hitelesítő adatok megadását és hello `Hostname` címke hello alkalmazásjegyzék vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="6998c-123">hello credential specification and hello `Hostname` tag are specified in hello application manifest.</span></span> <span data-ttu-id="6998c-124">Hello `Hostname` címke hello csoportosan felügyelt fiók nevét, amely alatt fut, tároló hello egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="6998c-124">hello `Hostname` tag must match hello gMSA account name that hello container runs under.</span></span>  <span data-ttu-id="6998c-125">Hello `Hostname` címke lehetővé teszi, hogy maga hello tároló tooauthenticate tooother szolgáltatások hello tartomány Kerberos-hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="6998c-125">hello `Hostname` tag allows hello container tooauthenticate itself tooother services in hello domain using Kerberos authentication.</span></span>  <span data-ttu-id="6998c-126">Egy minta hello megadásának `Hostname` és hello `credspec` hello az alkalmazásjegyzék megjelenik-e a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="6998c-126">A sample for specifying hello `Hostname` and hello `credspec` in hello application manifest is shown in hello following snippet:</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="6998c-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6998c-127">Next steps</span></span>

* [<span data-ttu-id="6998c-128">Egy Windows-tároló tooService Windows Server 2016 háló telepítése</span><span class="sxs-lookup"><span data-stu-id="6998c-128">Deploy a Windows container tooService Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="6998c-129">Egy Docker-tároló tooService Linux háló telepítése</span><span class="sxs-lookup"><span data-stu-id="6998c-129">Deploy a Docker container tooService Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
