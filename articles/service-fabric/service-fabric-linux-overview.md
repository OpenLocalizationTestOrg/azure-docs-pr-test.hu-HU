---
title: Az Azure Service Fabric Linux |} Microsoft Docs
description: "Service Fabric-fürtök Linux és a Java, ami azt jelenti, képes lesz központi telepítéséhez és Linux Java és a C# nyelven írt alkalmazások üzemeltetését a Service Fabric támogatja."
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
ms.openlocfilehash: dddc9f698d9776999d406117b46285a0f90d9620
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="service-fabric-on-linux"></a><span data-ttu-id="2412d-103">A Service Fabric Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2412d-103">Service Fabric on Linux</span></span>
<span data-ttu-id="2412d-104">Az előzetes verzióját a Service Fabric Linux lehetővé teszi, hogy hozhat létre, telepíthetnek és Linux magas rendelkezésre állású, nagymértékben méretezhető alkalmazásokat kezeléséhez, ugyanúgy, mint a Windows.</span><span class="sxs-lookup"><span data-stu-id="2412d-104">The preview of Service Fabric on Linux enables you to build, deploy, and manage highly available, highly scalable applications on Linux just as you would on Windows.</span></span> <span data-ttu-id="2412d-105">A Service Fabric-keretrendszerek (Reliable Services és Reliable Actors) a C# (.NET Core) mellett a Linux Java nyelven érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2412d-105">The Service Fabric frameworks (Reliable Services and Reliable Actors) are available in Java on Linux in addition to C# (.NET Core).</span></span>  <span data-ttu-id="2412d-106">Is létrehozható [végrehajtható vendégszolgáltatások](service-fabric-deploy-existing-app.md) rendelkező bármilyen nyelven vagy keretrendszerben.</span><span class="sxs-lookup"><span data-stu-id="2412d-106">You can also build [guest executable services](service-fabric-deploy-existing-app.md) with any language or framework.</span></span> <span data-ttu-id="2412d-107">Emellett az előzetes verziója is támogatja koordinálása Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="2412d-107">In addition, the preview also supports orchestrating Docker containers.</span></span> <span data-ttu-id="2412d-108">Docker-tároló Vendég végrehajtható fájlok, illetve a Service Fabric-keretrendszert használó natív Service Fabric szolgáltatások futtathatók.</span><span class="sxs-lookup"><span data-stu-id="2412d-108">Docker containers can run guest executables or native Service Fabric services, which use the Service Fabric frameworks.</span></span>

<span data-ttu-id="2412d-109">A Service Fabric Linux fogalmilag Service Fabric Windows (kivéve az operációs rendszer jellemzőit és a támogatott programozási nyelvek).</span><span class="sxs-lookup"><span data-stu-id="2412d-109">Service Fabric on Linux is conceptually equivalent to Service Fabric on Windows (except for OS specifics and programming language support).</span></span> <span data-ttu-id="2412d-110">Így a legtöbb a [meglévő dokumentáció](http://aka.ms/servicefabricdocs) , ismerkedjen meg a technológia segítséget nyújt a vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2412d-110">Thus, most of our [existing documentation](http://aka.ms/servicefabricdocs) applies in helping you get familiar with the technology.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a><span data-ttu-id="2412d-111">Támogatott operációs rendszerek és programozási nyelvek</span><span class="sxs-lookup"><span data-stu-id="2412d-111">Supported operating systems and programming languages</span></span>
<span data-ttu-id="2412d-112">A korlátozott előzetes verziója támogatja a többgépes fürtök mellett egy beépített fejlesztési fürtök létrehozásához az Ubuntu Server 16.04 futó Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2412d-112">The limited preview supports the creation of one-box development clusters in addition to multi-machine clusters in Azure running Ubuntu Server 16.04.</span></span> <span data-ttu-id="2412d-113">Az előzetes verziója támogatja a Reliable Actors és a megbízható állapotmentes szolgáltatások keretrendszerek Java és C# Vendég végrehajtható fájlok és a Docker-tároló koordinálása mellett.</span><span class="sxs-lookup"><span data-stu-id="2412d-113">The preview supports the Reliable Actors and the Reliable Stateless Services frameworks in Java and C# in addition to guest executables and orchestrating Docker containers.</span></span>  

> [!NOTE]
> <span data-ttu-id="2412d-114">Megbízható gyűjtemények nem támogatottak a Linux még.</span><span class="sxs-lookup"><span data-stu-id="2412d-114">Reliable Collections are not supported in Linux yet.</span></span> <span data-ttu-id="2412d-115">Készenléti önmagában nem használhatók vagy - csak egy mezőt és az Azure Linux többgépes fürtök az előzetes verzió támogatott.</span><span class="sxs-lookup"><span data-stu-id="2412d-115">Stand alone clusters aren't supported either - only one box and Azure Linux multi-machine clusters are supported in the preview.</span></span>
>
>


## <a name="supported-tooling"></a><span data-ttu-id="2412d-116">Támogatott tooling eszköz</span><span class="sxs-lookup"><span data-stu-id="2412d-116">Supported tooling</span></span>
<span data-ttu-id="2412d-117">Az előzetes verziója támogatja a fürt Service Fabric parancssori felületen keresztül való együttműködéshez.</span><span class="sxs-lookup"><span data-stu-id="2412d-117">The preview supports interaction with the cluster through Service Fabric CLI.</span></span> <span data-ttu-id="2412d-118">Java-fejlesztők számára az integrációra az eclipse-ben és a Yeoman által biztosított Eclipse OSX és Linux rendszeren támogatott.</span><span class="sxs-lookup"><span data-stu-id="2412d-118">For Java developers, integration with Eclipse and Yeoman is provided with Eclipse supported on Linux and on OSX.</span></span> <span data-ttu-id="2412d-119">Az os x-integráció a technikai részletek vagrant keresztül egy Linux virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="2412d-119">The OSX integration uses a Linux VM under the hood via vagrant.</span></span> <span data-ttu-id="2412d-120">C#-fejlesztők számára Yeoman integrációja biztosítja alkalmazás sablonok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2412d-120">For C# developers, integration with Yeoman is provided to generate application templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2412d-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2412d-121">Next steps</span></span>

* <span data-ttu-id="2412d-122">Ismerkedjen meg a [Reliable Actors](service-fabric-reliable-actors-introduction.md) és [Reliable Services](service-fabric-reliable-services-introduction.md) keretrendszerek programozási</span><span class="sxs-lookup"><span data-stu-id="2412d-122">Get familiar with the [Reliable Actors](service-fabric-reliable-actors-introduction.md) and [Reliable Services](service-fabric-reliable-services-introduction.md) programming frameworks</span></span>
* [<span data-ttu-id="2412d-123">A fejlesztőkörnyezet előkészítése Linuxon</span><span class="sxs-lookup"><span data-stu-id="2412d-123">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="2412d-124">A fejlesztőkörnyezet előkészítése OSX-en</span><span class="sxs-lookup"><span data-stu-id="2412d-124">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="2412d-125">Az első Service Fabric Java-alkalmazás létrehozása Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2412d-125">Create your first Service Fabric Java application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="2412d-126">A telepítő a Service Fabric folyamatos integrációt és Jenkins és a GitHub megadott központi telepítés</span><span class="sxs-lookup"><span data-stu-id="2412d-126">Setup Service Fabric continuous integration and deployment with Jenkins and GitHub</span></span>](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [<span data-ttu-id="2412d-127">Service Fabric – Különbségek Windows és Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2412d-127">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
