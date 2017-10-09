---
title: "aaaAzure Event Hubs API – áttekintés |} Microsoft Docs"
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
ms.openlocfilehash: 46dfcc544ff92642cfd7a967f9ec38a0d8e2bd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="available-event-hubs-apis"></a><span data-ttu-id="bc55b-103">Rendelkezésre álló Event Hubs API-k</span><span class="sxs-lookup"><span data-stu-id="bc55b-103">Available Event Hubs APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="bc55b-104">Futásidejű API-k</span><span class="sxs-lookup"><span data-stu-id="bc55b-104">Runtime APIs</span></span>

<span data-ttu-id="bc55b-105">hello minden jelenleg elérhető Azure Event Hubs futásidejű ügyfelek leírása látható.</span><span class="sxs-lookup"><span data-stu-id="bc55b-105">hello following is a description of all currently available Azure Event Hubs runtime clients.</span></span> <span data-ttu-id="bc55b-106">Ezek a könyvtárak is közé korlátozott felügyeleti funkciók, amíg vannak is [adott könyvtárakat](#management-apis) dedikált toomanagement műveletek.</span><span class="sxs-lookup"><span data-stu-id="bc55b-106">While some of these libraries also include limited management functionality, there are also [specific libraries](#management-apis) dedicated toomanagement operations.</span></span> <span data-ttu-id="bc55b-107">Ezeket a könyvtárakat hello core fókusz toosend és üzenetek fogadása egy eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="bc55b-107">hello core focus of these libraries is toosend and receive messages from an event hub.</span></span>

<span data-ttu-id="bc55b-108">Lásd: [további információt](#additional-information) hello aktuális állapota minden egyes futásidejű kódtár olvashat.</span><span class="sxs-lookup"><span data-stu-id="bc55b-108">See [additional information](#additional-information) for more details on hello current status of each runtime library.</span></span>

| <span data-ttu-id="bc55b-109">Nyelvi/Platform</span><span class="sxs-lookup"><span data-stu-id="bc55b-109">Language/Platform</span></span> | <span data-ttu-id="bc55b-110">Ügyfélcsomag</span><span class="sxs-lookup"><span data-stu-id="bc55b-110">Client package</span></span> | <span data-ttu-id="bc55b-111">EventProcessorHost csomag</span><span class="sxs-lookup"><span data-stu-id="bc55b-111">EventProcessorHost package</span></span> | <span data-ttu-id="bc55b-112">Tárház</span><span class="sxs-lookup"><span data-stu-id="bc55b-112">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bc55b-113">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="bc55b-113">.NET Standard</span></span> | [<span data-ttu-id="bc55b-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="bc55b-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [<span data-ttu-id="bc55b-115">NuGet</span><span class="sxs-lookup"><span data-stu-id="bc55b-115">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [<span data-ttu-id="bc55b-116">GitHub</span><span class="sxs-lookup"><span data-stu-id="bc55b-116">GitHub</span></span>](https://github.com/azure/azure-event-hubs-dotnet) |
| <span data-ttu-id="bc55b-117">.NET-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="bc55b-117">.NET Framework</span></span> | [<span data-ttu-id="bc55b-118">NuGet</span><span class="sxs-lookup"><span data-stu-id="bc55b-118">NuGet</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [<span data-ttu-id="bc55b-119">NuGet</span><span class="sxs-lookup"><span data-stu-id="bc55b-119">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | <span data-ttu-id="bc55b-120">N/A</span><span class="sxs-lookup"><span data-stu-id="bc55b-120">N/A</span></span> |
| <span data-ttu-id="bc55b-121">Java</span><span class="sxs-lookup"><span data-stu-id="bc55b-121">Java</span></span> | [<span data-ttu-id="bc55b-122">Maven 3</span><span class="sxs-lookup"><span data-stu-id="bc55b-122">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [<span data-ttu-id="bc55b-123">Maven 3</span><span class="sxs-lookup"><span data-stu-id="bc55b-123">Maven</span></span>](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [<span data-ttu-id="bc55b-124">GitHub</span><span class="sxs-lookup"><span data-stu-id="bc55b-124">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-java) |
| <span data-ttu-id="bc55b-125">Csomópont</span><span class="sxs-lookup"><span data-stu-id="bc55b-125">Node</span></span> | [<span data-ttu-id="bc55b-126">NPM</span><span class="sxs-lookup"><span data-stu-id="bc55b-126">NPM</span></span>](https://www.npmjs.com/package/azure-event-hubs) | <span data-ttu-id="bc55b-127">N/A</span><span class="sxs-lookup"><span data-stu-id="bc55b-127">N/A</span></span> | [<span data-ttu-id="bc55b-128">GitHub</span><span class="sxs-lookup"><span data-stu-id="bc55b-128">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-node) |
| <span data-ttu-id="bc55b-129">C#</span><span class="sxs-lookup"><span data-stu-id="bc55b-129">C</span></span> | <span data-ttu-id="bc55b-130">N/A</span><span class="sxs-lookup"><span data-stu-id="bc55b-130">N/A</span></span> | <span data-ttu-id="bc55b-131">N/A</span><span class="sxs-lookup"><span data-stu-id="bc55b-131">N/A</span></span> | [<span data-ttu-id="bc55b-132">GitHub</span><span class="sxs-lookup"><span data-stu-id="bc55b-132">GitHub</span></span>](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a><span data-ttu-id="bc55b-133">További információ</span><span class="sxs-lookup"><span data-stu-id="bc55b-133">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="bc55b-134">.NET</span><span class="sxs-lookup"><span data-stu-id="bc55b-134">.NET</span></span>
<span data-ttu-id="bc55b-135">hello .NET ökoszisztéma több futtatókörnyezetek rendelkezik, ezért van több .NET-kódtárakra az Event Hubs számára.</span><span class="sxs-lookup"><span data-stu-id="bc55b-135">hello .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="bc55b-136">hello .NET szabványos függvénytár is futtatható, használja a .NET Core vagy a .NET-keretrendszer hello közben hello .NET Framework könyvtár csak a .NET-keretrendszer környezetben futtatható.</span><span class="sxs-lookup"><span data-stu-id="bc55b-136">hello .NET Standard library can be run using either .NET Core or hello .NET Framework, while hello .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="bc55b-137">További információ a .NET-keretrendszer: [keretrendszer-verziók](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="bc55b-137">For more information on .NET Frameworks, see [framework versions](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).</span></span>

#### <a name="node"></a><span data-ttu-id="bc55b-138">Csomópont</span><span class="sxs-lookup"><span data-stu-id="bc55b-138">Node</span></span>

<span data-ttu-id="bc55b-139">hello Node.js kódtár jelenleg előzetes verzióban érhetők, és a Microsoft alkalmazottai és külső közreműködő szerepkörrel rendelkező személyek által kezelt oldal projektként.</span><span class="sxs-lookup"><span data-stu-id="bc55b-139">hello Node.js library is currently in preview and is maintained as a side project by Microsoft employees and external contributors.</span></span> <span data-ttu-id="bc55b-140">Beleértve az adatforrás összes hozzájárulást üdvözlő és felül kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="bc55b-140">All contributions including source code are welcome and will be reviewed.</span></span>

## <a name="management-apis"></a><span data-ttu-id="bc55b-141">API-val</span><span class="sxs-lookup"><span data-stu-id="bc55b-141">Management APIs</span></span>

<span data-ttu-id="bc55b-142">hello minden jelenleg elérhető felügyeleti adott könyvtárak listája látható.</span><span class="sxs-lookup"><span data-stu-id="bc55b-142">hello following is a listing of all currently available management specific libraries.</span></span> <span data-ttu-id="bc55b-143">Ezeket a könyvtárakat nincs futásidejű műveletek tartalmazhat, és a hello azzal a céllal, az Event Hubs entitások kezelése.</span><span class="sxs-lookup"><span data-stu-id="bc55b-143">None of these libraries contain runtime operations, and are for hello sole purpose of managing Event Hubs entities.</span></span>

| <span data-ttu-id="bc55b-144">Nyelvi/Platform</span><span class="sxs-lookup"><span data-stu-id="bc55b-144">Language/Platform</span></span> | <span data-ttu-id="bc55b-145">Felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="bc55b-145">Management package</span></span> | <span data-ttu-id="bc55b-146">Tárház</span><span class="sxs-lookup"><span data-stu-id="bc55b-146">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bc55b-147">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="bc55b-147">.NET Standard</span></span> | [<span data-ttu-id="bc55b-148">NuGet</span><span class="sxs-lookup"><span data-stu-id="bc55b-148">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [<span data-ttu-id="bc55b-149">GitHub</span><span class="sxs-lookup"><span data-stu-id="bc55b-149">GitHub</span></span>](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a><span data-ttu-id="bc55b-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc55b-150">Next steps</span></span>
<span data-ttu-id="bc55b-151">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="bc55b-151">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="bc55b-152">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="bc55b-152">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="bc55b-153">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc55b-153">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="bc55b-154">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bc55b-154">Event Hubs FAQ</span></span>](event-hubs-faq.md)