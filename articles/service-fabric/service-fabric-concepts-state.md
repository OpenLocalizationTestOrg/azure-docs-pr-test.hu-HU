---
title: "aaaDefinine és az Azure mikroszolgáltatások állapot kezelése |} Microsoft Docs"
description: "Hogyan toodefine és kezelheti a Service Fabric szolgáltatás állapota"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4a24696da71753d0f343a86df3556b5b7c964834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-state"></a><span data-ttu-id="ce646-103">Szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="ce646-103">Service state</span></span>
<span data-ttu-id="ce646-104">**Szolgáltatás állapota** toohello a memóriában vagy a lemez adatokkal, hogy a szolgáltatás futtatásához szükséges toofunction hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ce646-104">**Service state** refers toohello in-memory or on disk data that a service requires toofunction.</span></span> <span data-ttu-id="ce646-105">Ez magában foglalja, például hello adatstruktúrák és tagváltozók hello szolgáltatás olvas és ír toodo munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="ce646-105">It includes, for example, hello data structures and member variables that hello service reads and writes toodo work.</span></span> <span data-ttu-id="ce646-106">Attól függően, hogy hogyan hello szolgáltatás tervezett azt is tartalmazhatnak fájlokat vagy más erőforrásokat, amelyek tárolása a lemezen.</span><span class="sxs-lookup"><span data-stu-id="ce646-106">Depending on how hello service is architected, it could also include files or other resources that are stored on disk.</span></span> <span data-ttu-id="ce646-107">Hello fájlok, például egy adatbázis toostore adatok és a tranzakciós naplók használna.</span><span class="sxs-lookup"><span data-stu-id="ce646-107">For example, hello files a database would use toostore data and transaction logs.</span></span>

<span data-ttu-id="ce646-108">Példa szolgáltatásként Mérlegeljük, egy Számológép.</span><span class="sxs-lookup"><span data-stu-id="ce646-108">As an example service, let's consider a calculator.</span></span> <span data-ttu-id="ce646-109">Egy egyszerű számológép szolgáltatás időt vesz igénybe a két szám, és az összegét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ce646-109">A basic calculator service takes two numbers and returns their sum.</span></span> <span data-ttu-id="ce646-110">Magában foglalja a számítást, nem tagváltozók és egyéb adatait.</span><span class="sxs-lookup"><span data-stu-id="ce646-110">Performing this calculation involves no member variables or other information.</span></span>

<span data-ttu-id="ce646-111">Most pedig nézzük meg ugyanazon Számológép hello, de egy további módszer tárolásához és hello utolsó sum visszaadó kiszámította.</span><span class="sxs-lookup"><span data-stu-id="ce646-111">Now consider hello same calculator, but with an additional method for storing and returning hello last sum it has computed.</span></span> <span data-ttu-id="ce646-112">Ez a szolgáltatás jelenleg állapot-nyilvántartó.</span><span class="sxs-lookup"><span data-stu-id="ce646-112">This service is now stateful.</span></span> <span data-ttu-id="ce646-113">Állapotalapú alkalmazások és szolgáltatások azt jelenti, hogy néhány állapotát, írja az toowhen kiszámítja új összege, és ha tegye fel azt tooreturn hello utolsó számított összege olvas tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ce646-113">Stateful means it contains some state that it writes toowhen it computes a new sum and reads from when you ask it tooreturn hello last computed sum.</span></span>

<span data-ttu-id="ce646-114">Az Azure Service Fabric hello első szolgáltatás állapotmentes szolgáltatások neve.</span><span class="sxs-lookup"><span data-stu-id="ce646-114">In Azure Service Fabric, hello first service is called a stateless service.</span></span> <span data-ttu-id="ce646-115">második hello szolgáltatást egy állapotalapú szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="ce646-115">hello second service is called a stateful service.</span></span>

## <a name="storing-service-state"></a><span data-ttu-id="ce646-116">Tárolja a szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="ce646-116">Storing service state</span></span>
<span data-ttu-id="ce646-117">Állam vagy externalized vagy közös elhelyezésű hello kóddal, amely van hello állapot kezelésére.</span><span class="sxs-lookup"><span data-stu-id="ce646-117">State can be either externalized or co-located with hello code that is manipulating hello state.</span></span> <span data-ttu-id="ce646-118">Állapot externalization általában egy külső adatbázis használatával hajtható végre, vagy egyéb adatok tárolására, hogy fut a különböző gépeken hello hálózaton keresztül, vagy a folyamaton kívüli hello egyazon számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ce646-118">Externalization of state is typically done by using an external database or other data store that runs on different machines over hello network or out of process on hello same machine.</span></span> <span data-ttu-id="ce646-119">A Számológép példánkban hello adattár lehet egy SQL database vagy az Azure Table-tároló példányát.</span><span class="sxs-lookup"><span data-stu-id="ce646-119">In our calculator example, hello data store could be a SQL database or instance of Azure Table Store.</span></span> <span data-ttu-id="ce646-120">Minden kérelem toocompute hello sum frissítést végez ezeken az adatokon, és toohello szolgáltatás tooreturn hello értéket eredményez hello aktuális érték hello áruházból lehívott alatt álló kérelmek.</span><span class="sxs-lookup"><span data-stu-id="ce646-120">Every request toocompute hello sum performs an update on this data, and requests toohello service tooreturn hello value result in hello current value being fetched from hello store.</span></span> 

<span data-ttu-id="ce646-121">Állapot is is elhelyezhető hello kóddal, amely kezeli a hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="ce646-121">State can also be co-located with hello code that manipulates hello state.</span></span> <span data-ttu-id="ce646-122">Ez a modell használatával a Service Fabric állapotalapú szolgáltatások általában készített.</span><span class="sxs-lookup"><span data-stu-id="ce646-122">Stateful services in Service Fabric are typically built using this model.</span></span> <span data-ttu-id="ce646-123">A Service Fabric hello infrastruktúra tooensure, hogy magas rendelkezésre áll, következetes és tartós ebben az állapotban, és könnyedén méretezhető, hogy hello szolgáltatások beépített ily módon biztosít.</span><span class="sxs-lookup"><span data-stu-id="ce646-123">Service Fabric provides hello infrastructure tooensure that this state is highly available, consistent, and durable, and that hello services built this way can easily scale.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce646-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ce646-124">Next steps</span></span>
<span data-ttu-id="ce646-125">A Service Fabric fogalmakat további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="ce646-125">For more information on Service Fabric concepts, see hello following articles:</span></span>

* [<span data-ttu-id="ce646-126">A Service Fabric-szolgáltatások rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="ce646-126">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="ce646-127">Méretezhetőséget biztosít a Service Fabric-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="ce646-127">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="ce646-128">A Service Fabric szolgáltatások particionálás</span><span class="sxs-lookup"><span data-stu-id="ce646-128">Partitioning Service Fabric services</span></span>](service-fabric-concepts-partitioning.md)
* [<span data-ttu-id="ce646-129">Service Fabric megbízható szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="ce646-129">Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-introduction.md)
