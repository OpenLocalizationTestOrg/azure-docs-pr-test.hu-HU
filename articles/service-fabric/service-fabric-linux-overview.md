---
title: a Service Fabric Linux aaaAzure |} Microsoft Docs
description: "Service Fabric-fürtök támogatja a Linux és a Java, ami azt jelenti, hogy be tudja toodeploy és Linux Java és a C# nyelven írt alkalmazások üzemeltetését a Service Fabric fog."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 459afade-145d-4ee6-b72b-ddf380ccd1bf
ms.service: service-fabric
ms.devlang: Java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: SubramaR
ms.openlocfilehash: ca0e092b00faec560963c0d6cc379593d085f6a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-on-linux"></a><span data-ttu-id="2017e-103">A Service Fabric Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2017e-103">Service Fabric on Linux</span></span>
<span data-ttu-id="2017e-104">hello előzetes verzióját a Service Fabric Linux lehetővé teszi a toobuild, telepítéséhez és felügyeletéhez a magas rendelkezésre állású, nagymértékben méretezhető alkalmazások Linux, ugyanúgy, mint Windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2017e-104">hello preview of Service Fabric on Linux enables you toobuild, deploy, and manage highly available, highly scalable applications on Linux just as you would on Windows.</span></span> <span data-ttu-id="2017e-105">hello Service Fabric keretrendszerek (Reliable Services és Reliable Actors) érhetők el a Linux Java hozzáadása tooC # (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="2017e-105">hello Service Fabric frameworks (Reliable Services and Reliable Actors) are available in Java on Linux in addition tooC# (.NET Core).</span></span>  <span data-ttu-id="2017e-106">Is létrehozható [végrehajtható vendégszolgáltatások](service-fabric-deploy-existing-app.md) rendelkező bármilyen nyelven vagy keretrendszerben.</span><span class="sxs-lookup"><span data-stu-id="2017e-106">You can also build [guest executable services](service-fabric-deploy-existing-app.md) with any language or framework.</span></span> <span data-ttu-id="2017e-107">Emellett a hello előzetes verziója is támogatja a koordinálása Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="2017e-107">In addition, hello preview also supports orchestrating Docker containers.</span></span> <span data-ttu-id="2017e-108">Docker-tároló Vendég végrehajtható fájlok, illetve hello Service Fabric-keretrendszert használó natív Service Fabric szolgáltatások futtathatók.</span><span class="sxs-lookup"><span data-stu-id="2017e-108">Docker containers can run guest executables or native Service Fabric services, which use hello Service Fabric frameworks.</span></span>

<span data-ttu-id="2017e-109">A Service Fabric Linux fogalmilag egyenértékű tooService Windows Fabric (kivéve az operációs rendszer jellemzőit és a támogatott programozási nyelvek).</span><span class="sxs-lookup"><span data-stu-id="2017e-109">Service Fabric on Linux is conceptually equivalent tooService Fabric on Windows (except for OS specifics and programming language support).</span></span> <span data-ttu-id="2017e-110">Így a legtöbb a [meglévő dokumentáció](http://aka.ms/servicefabricdocs) ismerkedhet meg vele hello technológia, segítve a vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2017e-110">Thus, most of our [existing documentation](http://aka.ms/servicefabricdocs) applies in helping you get familiar with hello technology.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a><span data-ttu-id="2017e-111">Támogatott operációs rendszerek és programozási nyelvek</span><span class="sxs-lookup"><span data-stu-id="2017e-111">Supported operating systems and programming languages</span></span>
<span data-ttu-id="2017e-112">korlátozott hello preview támogat hello létrehozása egy beépített fejlesztési fürtök emellett toomulti-gép fürtök Ubuntu Server 16.04 futó Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2017e-112">hello limited preview supports hello creation of one-box development clusters in addition toomulti-machine clusters in Azure running Ubuntu Server 16.04.</span></span> <span data-ttu-id="2017e-113">hello Reliable Actors és megbízható állapotmentes szolgáltatások keretrendszerek Java és C# hozzáadása tooguest végrehajtható fájlok és a Docker-tároló koordinálása hello hello preview támogat.</span><span class="sxs-lookup"><span data-stu-id="2017e-113">hello preview supports hello Reliable Actors and hello Reliable Stateless Services frameworks in Java and C# in addition tooguest executables and orchestrating Docker containers.</span></span>  

> [!NOTE]
> <span data-ttu-id="2017e-114">Megbízható gyűjtemények nem támogatottak a Linux még.</span><span class="sxs-lookup"><span data-stu-id="2017e-114">Reliable Collections are not supported in Linux yet.</span></span> <span data-ttu-id="2017e-115">Készenléti önmagában nem használhatók - vagy csak egy mezőt és az Azure Linux többgépes fürtök hello előzetes verzió támogatott.</span><span class="sxs-lookup"><span data-stu-id="2017e-115">Stand alone clusters aren't supported either - only one box and Azure Linux multi-machine clusters are supported in hello preview.</span></span>
>
>


## <a name="supported-tooling"></a><span data-ttu-id="2017e-116">Támogatott tooling eszköz</span><span class="sxs-lookup"><span data-stu-id="2017e-116">Supported tooling</span></span>
<span data-ttu-id="2017e-117">hello előzetes verziója támogatja a Service Fabric CLI hello fürtön való együttműködéshez.</span><span class="sxs-lookup"><span data-stu-id="2017e-117">hello preview supports interaction with hello cluster through Service Fabric CLI.</span></span> <span data-ttu-id="2017e-118">Java-fejlesztők számára az integrációra az eclipse-ben és a Yeoman által biztosított Eclipse OSX és Linux rendszeren támogatott.</span><span class="sxs-lookup"><span data-stu-id="2017e-118">For Java developers, integration with Eclipse and Yeoman is provided with Eclipse supported on Linux and on OSX.</span></span> <span data-ttu-id="2017e-119">hello OSX integrációs hello technikai vagrant keresztül egy Linux virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="2017e-119">hello OSX integration uses a Linux VM under hello hood via vagrant.</span></span> <span data-ttu-id="2017e-120">C#-fejlesztők számára toogenerate alkalmazássablonok Yeoman integrációja biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2017e-120">For C# developers, integration with Yeoman is provided toogenerate application templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2017e-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2017e-121">Next steps</span></span>

* <span data-ttu-id="2017e-122">Ismerkedjen meg hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) és [Reliable Services](service-fabric-reliable-services-introduction.md) keretrendszerek programozási</span><span class="sxs-lookup"><span data-stu-id="2017e-122">Get familiar with hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) and [Reliable Services](service-fabric-reliable-services-introduction.md) programming frameworks</span></span>
* [<span data-ttu-id="2017e-123">A fejlesztőkörnyezet előkészítése Linuxon</span><span class="sxs-lookup"><span data-stu-id="2017e-123">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="2017e-124">A fejlesztőkörnyezet előkészítése OSX-en</span><span class="sxs-lookup"><span data-stu-id="2017e-124">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="2017e-125">Az első Service Fabric Java-alkalmazás létrehozása Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2017e-125">Create your first Service Fabric Java application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="2017e-126">A telepítő a Service Fabric folyamatos integrációt és Jenkins és a GitHub megadott központi telepítés</span><span class="sxs-lookup"><span data-stu-id="2017e-126">Setup Service Fabric continuous integration and deployment with Jenkins and GitHub</span></span>](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [<span data-ttu-id="2017e-127">Service Fabric – Különbségek Windows és Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2017e-127">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
