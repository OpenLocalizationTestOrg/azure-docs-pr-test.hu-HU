---
title: "Azure Event Hubs API – áttekintés |} Microsoft Docs"
description: "Elérhető Azure Event Hubs API-kat áttekintése"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 40cd76e1aacb68d6051cae4a3c90a8970f5449f0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="available-event-hubs-apis"></a><span data-ttu-id="9acb1-103">Rendelkezésre álló Event Hubs API-k</span><span class="sxs-lookup"><span data-stu-id="9acb1-103">Available Event Hubs APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="9acb1-104">Futásidejű API-k</span><span class="sxs-lookup"><span data-stu-id="9acb1-104">Runtime APIs</span></span>

<span data-ttu-id="9acb1-105">Minden jelenleg elérhető Azure Event Hubs futásidejű ügyfelek leírása a következő:</span><span class="sxs-lookup"><span data-stu-id="9acb1-105">The following is a description of all currently available Azure Event Hubs runtime clients.</span></span> <span data-ttu-id="9acb1-106">Ezek a könyvtárak is közé korlátozott felügyeleti funkciók, amíg vannak is [adott könyvtárakat](#management-apis) dedikált felügyeleti műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="9acb1-106">While some of these libraries also include limited management functionality, there are also [specific libraries](#management-apis) dedicated to management operations.</span></span> <span data-ttu-id="9acb1-107">A core ezeket a könyvtárakat célja üzeneteket küldjön és fogadjon az eseményközpontban lévő.</span><span class="sxs-lookup"><span data-stu-id="9acb1-107">The core focus of these libraries is to send and receive messages from an event hub.</span></span>

<span data-ttu-id="9acb1-108">Lásd: [további információt](#additional-information) kapcsolatos további információkért az egyes futásidejű kódtár aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="9acb1-108">See [additional information](#additional-information) for more details on the current status of each runtime library.</span></span>

| <span data-ttu-id="9acb1-109">Nyelvi/Platform</span><span class="sxs-lookup"><span data-stu-id="9acb1-109">Language/Platform</span></span> | <span data-ttu-id="9acb1-110">Ügyfélcsomag</span><span class="sxs-lookup"><span data-stu-id="9acb1-110">Client package</span></span> | <span data-ttu-id="9acb1-111">EventProcessorHost csomag</span><span class="sxs-lookup"><span data-stu-id="9acb1-111">EventProcessorHost package</span></span> | <span data-ttu-id="9acb1-112">Tárház</span><span class="sxs-lookup"><span data-stu-id="9acb1-112">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9acb1-113">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="9acb1-113">.NET Standard</span></span> | [<span data-ttu-id="9acb1-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="9acb1-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [<span data-ttu-id="9acb1-115">NuGet</span><span class="sxs-lookup"><span data-stu-id="9acb1-115">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [<span data-ttu-id="9acb1-116">GitHub</span><span class="sxs-lookup"><span data-stu-id="9acb1-116">GitHub</span></span>](https://github.com/azure/azure-event-hubs-dotnet) |
| <span data-ttu-id="9acb1-117">.NET-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="9acb1-117">.NET Framework</span></span> | [<span data-ttu-id="9acb1-118">NuGet</span><span class="sxs-lookup"><span data-stu-id="9acb1-118">NuGet</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [<span data-ttu-id="9acb1-119">NuGet</span><span class="sxs-lookup"><span data-stu-id="9acb1-119">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | <span data-ttu-id="9acb1-120">N/A</span><span class="sxs-lookup"><span data-stu-id="9acb1-120">N/A</span></span> |
| <span data-ttu-id="9acb1-121">Java</span><span class="sxs-lookup"><span data-stu-id="9acb1-121">Java</span></span> | [<span data-ttu-id="9acb1-122">Maven 3</span><span class="sxs-lookup"><span data-stu-id="9acb1-122">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [<span data-ttu-id="9acb1-123">Maven 3</span><span class="sxs-lookup"><span data-stu-id="9acb1-123">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [<span data-ttu-id="9acb1-124">GitHub</span><span class="sxs-lookup"><span data-stu-id="9acb1-124">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-java) |
| <span data-ttu-id="9acb1-125">Csomópont</span><span class="sxs-lookup"><span data-stu-id="9acb1-125">Node</span></span> | [<span data-ttu-id="9acb1-126">NPM</span><span class="sxs-lookup"><span data-stu-id="9acb1-126">NPM</span></span>](https://www.npmjs.com/package/azure-event-hubs) | <span data-ttu-id="9acb1-127">N/A</span><span class="sxs-lookup"><span data-stu-id="9acb1-127">N/A</span></span> | [<span data-ttu-id="9acb1-128">GitHub</span><span class="sxs-lookup"><span data-stu-id="9acb1-128">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-node) |
| <span data-ttu-id="9acb1-129">C#</span><span class="sxs-lookup"><span data-stu-id="9acb1-129">C</span></span> | <span data-ttu-id="9acb1-130">N/A</span><span class="sxs-lookup"><span data-stu-id="9acb1-130">N/A</span></span> | <span data-ttu-id="9acb1-131">N/A</span><span class="sxs-lookup"><span data-stu-id="9acb1-131">N/A</span></span> | [<span data-ttu-id="9acb1-132">GitHub</span><span class="sxs-lookup"><span data-stu-id="9acb1-132">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a><span data-ttu-id="9acb1-133">További információ</span><span class="sxs-lookup"><span data-stu-id="9acb1-133">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="9acb1-134">.NET</span><span class="sxs-lookup"><span data-stu-id="9acb1-134">.NET</span></span>
<span data-ttu-id="9acb1-135">A .NET-ökoszisztéma több futtatókörnyezetek rendelkezik, ezért van több .NET-kódtárakra az Event Hubs számára.</span><span class="sxs-lookup"><span data-stu-id="9acb1-135">The .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="9acb1-136">A .NET-szabvány könyvtár a .NET Core vagy a .NET-keretrendszer használatával, amíg a .NET Framework könyvtár csak a .NET-keretrendszer környezetben futtatható futtatható.</span><span class="sxs-lookup"><span data-stu-id="9acb1-136">The .NET Standard library can be run using either .NET Core or the .NET Framework, while the .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="9acb1-137">További információ a .NET-keretrendszer: [keretrendszer-verziók](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="9acb1-137">For more information on .NET Frameworks, see [framework versions](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span></span>

#### <a name="node"></a><span data-ttu-id="9acb1-138">Csomópont</span><span class="sxs-lookup"><span data-stu-id="9acb1-138">Node</span></span>

<span data-ttu-id="9acb1-139">A Node.js kódtár jelenleg előzetes verzióban érhetők, és a Microsoft alkalmazottai és külső közreműködő szerepkörrel rendelkező személyek által kezelt oldal projektként.</span><span class="sxs-lookup"><span data-stu-id="9acb1-139">The Node.js library is currently in preview and is maintained as a side project by Microsoft employees and external contributors.</span></span> <span data-ttu-id="9acb1-140">Beleértve az adatforrás összes hozzájárulást üdvözlő és felül kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="9acb1-140">All contributions including source code are welcome and will be reviewed.</span></span>

## <a name="management-apis"></a><span data-ttu-id="9acb1-141">API-val</span><span class="sxs-lookup"><span data-stu-id="9acb1-141">Management APIs</span></span>

<span data-ttu-id="9acb1-142">Minden jelenleg elérhető felügyeleti adott könyvtárakat listáját a következő:</span><span class="sxs-lookup"><span data-stu-id="9acb1-142">The following is a listing of all currently available management specific libraries.</span></span> <span data-ttu-id="9acb1-143">Nincs ezeket a könyvtárakat futásidejű műveletek tartalmazhat, és az azzal a céllal, az Event Hubs entitások kezelése.</span><span class="sxs-lookup"><span data-stu-id="9acb1-143">None of these libraries contain runtime operations, and are for the sole purpose of managing Event Hubs entities.</span></span>

| <span data-ttu-id="9acb1-144">Nyelvi/Platform</span><span class="sxs-lookup"><span data-stu-id="9acb1-144">Language/Platform</span></span> | <span data-ttu-id="9acb1-145">Felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="9acb1-145">Management package</span></span> | <span data-ttu-id="9acb1-146">Tárház</span><span class="sxs-lookup"><span data-stu-id="9acb1-146">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9acb1-147">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="9acb1-147">.NET Standard</span></span> | [<span data-ttu-id="9acb1-148">NuGet</span><span class="sxs-lookup"><span data-stu-id="9acb1-148">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [<span data-ttu-id="9acb1-149">GitHub</span><span class="sxs-lookup"><span data-stu-id="9acb1-149">GitHub</span></span>](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a><span data-ttu-id="9acb1-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9acb1-150">Next steps</span></span>
<span data-ttu-id="9acb1-151">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="9acb1-151">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="9acb1-152">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="9acb1-152">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="9acb1-153">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="9acb1-153">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="9acb1-154">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="9acb1-154">Event Hubs FAQ</span></span>](event-hubs-faq.md)