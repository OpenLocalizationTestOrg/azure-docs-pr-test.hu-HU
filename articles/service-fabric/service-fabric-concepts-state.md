---
title: "Definine és az Azure mikroszolgáltatások állapot kezelése |} Microsoft Docs"
description: "Hogyan határozhatja meg és kezelheti a Service Fabric szolgáltatás állapota"
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
ms.openlocfilehash: 2f7835fab175bdb7ef7ce3894cab9e09a39457f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="service-state"></a><span data-ttu-id="61702-103">Szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="61702-103">Service state</span></span>
<span data-ttu-id="61702-104">**Szolgáltatás állapota** a memóriában vagy lemez-adatokat a szolgáltatás működéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="61702-104">**Service state** refers to the in-memory or on disk data that a service requires to function.</span></span> <span data-ttu-id="61702-105">Ez magában foglalja, például adatstruktúrák és tagváltozók, amely a szolgáltatás olvas és ír működnek.</span><span class="sxs-lookup"><span data-stu-id="61702-105">It includes, for example, the data structures and member variables that the service reads and writes to do work.</span></span> <span data-ttu-id="61702-106">Attól függően, hogy a szolgáltatás tervezett azt is tartalmazhatnak fájlokat vagy más erőforrásokat, amelyek tárolása a lemezen.</span><span class="sxs-lookup"><span data-stu-id="61702-106">Depending on how the service is architected, it could also include files or other resources that are stored on disk.</span></span> <span data-ttu-id="61702-107">Például a fájlok egy adatbázis használna adatok és a tranzakciós naplók tárolásához.</span><span class="sxs-lookup"><span data-stu-id="61702-107">For example, the files a database would use to store data and transaction logs.</span></span>

<span data-ttu-id="61702-108">Példa szolgáltatásként Mérlegeljük, egy Számológép.</span><span class="sxs-lookup"><span data-stu-id="61702-108">As an example service, let's consider a calculator.</span></span> <span data-ttu-id="61702-109">Egy egyszerű számológép szolgáltatás időt vesz igénybe a két szám, és az összegét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="61702-109">A basic calculator service takes two numbers and returns their sum.</span></span> <span data-ttu-id="61702-110">Magában foglalja a számítást, nem tagváltozók és egyéb adatait.</span><span class="sxs-lookup"><span data-stu-id="61702-110">Performing this calculation involves no member variables or other information.</span></span>

<span data-ttu-id="61702-111">Most pedig nézzük meg az azonos Számológép, de egy további módszer tárolásához és az utolsó összeg visszaadó kiszámította.</span><span class="sxs-lookup"><span data-stu-id="61702-111">Now consider the same calculator, but with an additional method for storing and returning the last sum it has computed.</span></span> <span data-ttu-id="61702-112">Ez a szolgáltatás jelenleg állapot-nyilvántartó.</span><span class="sxs-lookup"><span data-stu-id="61702-112">This service is now stateful.</span></span> <span data-ttu-id="61702-113">Állapotalapú alkalmazások és szolgáltatások azt jelenti, hogy néhány állapotba, ha kiszámítja új összege, és amikor kérje meg, hogy térjen vissza a legutóbbi számított összege olvas ír tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="61702-113">Stateful means it contains some state that it writes to when it computes a new sum and reads from when you ask it to return the last computed sum.</span></span>

<span data-ttu-id="61702-114">Az Azure Service Fabric az első szolgáltatás állapotmentes szolgáltatások neve.</span><span class="sxs-lookup"><span data-stu-id="61702-114">In Azure Service Fabric, the first service is called a stateless service.</span></span> <span data-ttu-id="61702-115">A második szolgáltatás egy állapotalapú szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="61702-115">The second service is called a stateful service.</span></span>

## <a name="storing-service-state"></a><span data-ttu-id="61702-116">Tárolja a szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="61702-116">Storing service state</span></span>
<span data-ttu-id="61702-117">Állam vagy externalized vagy közösen elhelyezett a kódot, amely az az állapot kezelésére.</span><span class="sxs-lookup"><span data-stu-id="61702-117">State can be either externalized or co-located with the code that is manipulating the state.</span></span> <span data-ttu-id="61702-118">Állapot externalization általában egy külső adatbázis vagy más adattárolóbeli tárolására, a hálózaton keresztül, vagy ugyanazon a számítógépen a folyamaton kívüli különböző gépeken futó használatával hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="61702-118">Externalization of state is typically done by using an external database or other data store that runs on different machines over the network or out of process on the same machine.</span></span> <span data-ttu-id="61702-119">A Számológép példánkban az adattár lehet egy SQL database vagy az Azure Table-tároló példányát.</span><span class="sxs-lookup"><span data-stu-id="61702-119">In our calculator example, the data store could be a SQL database or instance of Azure Table Store.</span></span> <span data-ttu-id="61702-120">Az összeg kiszámításához kérelmek frissítést végez ezeken az adatokon, és a szolgáltatás arra kéri, hogy az érték eredményt az áruházból lehívott éppen aktuális értéke.</span><span class="sxs-lookup"><span data-stu-id="61702-120">Every request to compute the sum performs an update on this data, and requests to the service to return the value result in the current value being fetched from the store.</span></span> 

<span data-ttu-id="61702-121">Lehet, hogy állapota is együtt a kódot, amely kezeli a állapotát.</span><span class="sxs-lookup"><span data-stu-id="61702-121">State can also be co-located with the code that manipulates the state.</span></span> <span data-ttu-id="61702-122">Ez a modell használatával a Service Fabric állapotalapú szolgáltatások általában készített.</span><span class="sxs-lookup"><span data-stu-id="61702-122">Stateful services in Service Fabric are typically built using this model.</span></span> <span data-ttu-id="61702-123">Győződjön meg arról, hogy a magas rendelkezésre áll, következetes és tartós ebben az állapotban, és, hogy a szolgáltatások beépített ily módon egyszerűen méretezhető infrastruktúrát biztosít a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="61702-123">Service Fabric provides the infrastructure to ensure that this state is highly available, consistent, and durable, and that the services built this way can easily scale.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61702-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61702-124">Next steps</span></span>
<span data-ttu-id="61702-125">A Service Fabric fogalmakat további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="61702-125">For more information on Service Fabric concepts, see the following articles:</span></span>

* [<span data-ttu-id="61702-126">A Service Fabric-szolgáltatások rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="61702-126">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="61702-127">Méretezhetőséget biztosít a Service Fabric-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="61702-127">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="61702-128">A Service Fabric szolgáltatások particionálás</span><span class="sxs-lookup"><span data-stu-id="61702-128">Partitioning Service Fabric services</span></span>](service-fabric-concepts-partitioning.md)
* [<span data-ttu-id="61702-129">Service Fabric megbízható szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="61702-129">Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-introduction.md)
