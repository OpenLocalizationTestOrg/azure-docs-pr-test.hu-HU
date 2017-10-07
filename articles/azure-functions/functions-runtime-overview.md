---
title: "aaaAzure funkciók futásidejű áttekintése |} Microsoft Docs"
description: "Hello Azure Functions futásidejű Preview áttekintése"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 8ce3e5037731d499c330b395c89c90109d18d65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-runtime-overview"></a><span data-ttu-id="fe9c0-103">Az Azure Functions futásidejű áttekintése</span><span class="sxs-lookup"><span data-stu-id="fe9c0-103">Azure Functions Runtime Overview</span></span>

<span data-ttu-id="fe9c0-104">hello Azure Functions Futtatókörnyezettel lehetőséget biztosít az új, tootake kihasználják az hello egyszerűség és rugalmasság hello az Azure Functions programozási modell a helyszínen.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-104">hello Azure Functions Runtime provides a new way for you tootake advantage of hello simplicity and flexibility of hello Azure Functions programming model on-premises.</span></span> <span data-ttu-id="fe9c0-105">Hello épülő azonos nyissa meg a forrás gyökerek, az Azure Functions, az Azure Functions Futtatókörnyezettel majdnem azonos fejlesztői élmény hello felhőalapú szolgáltatásként telepített helyszíni tooprovide.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-105">Built on hello same open source roots as Azure Functions, Azure Functions Runtime is deployed on-premises tooprovide a nearly identical development experience as hello cloud service.</span></span>

![Az Azure Functions futásidejű a betekintő portálon][1]

<span data-ttu-id="fe9c0-107">hello Azure Functions Futtatókörnyezettel lehetőséget biztosít az Ön tooexperience Azure Functions előtt véglegesítése toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-107">hello Azure Functions Runtime provides a way for you tooexperience Azure Functions before committing toohello cloud.</span></span> <span data-ttu-id="fe9c0-108">Ezzel a módszerrel hello kód eszközök készít majd átvihető toohello felhőhöz való áttelepítésekor.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-108">In this way, hello code assets you build can then be taken with you toohello cloud when you migrate.</span></span>  <span data-ttu-id="fe9c0-109">hello futásidejű új beállítások, például éjszaka használatával a helyszíni számítógépek toorun kötegelt folyamatok hello tartalék számítási teljesítményt is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-109">hello runtime also opens up new options for you, such as using hello spare compute power of your on-premises computers toorun batch processes overnight.</span></span> <span data-ttu-id="fe9c0-110">A szervezet tooconditionally send data tooother rendszerek, mind a helyszíni és felhőben hello eszközök is használható.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-110">You can also use devices within your organization tooconditionally send data tooother systems, both on-premises and in hello cloud.</span></span>

<span data-ttu-id="fe9c0-111">hello Azure Functions Futtatókörnyezettel két részből áll:</span><span class="sxs-lookup"><span data-stu-id="fe9c0-111">hello Azure Functions Runtime consists of two pieces:</span></span>
* <span data-ttu-id="fe9c0-112">Az Azure Functions futásidejű kezelés szerepköre</span><span class="sxs-lookup"><span data-stu-id="fe9c0-112">Azure Functions Runtime Management Role</span></span>
* <span data-ttu-id="fe9c0-113">Az Azure Functions futásidejű feldolgozói szerepkör</span><span class="sxs-lookup"><span data-stu-id="fe9c0-113">Azure Functions Runtime Worker Role</span></span>

## <a name="azure-functions-management-role"></a><span data-ttu-id="fe9c0-114">Az Azure Functions kezelés szerepköre</span><span class="sxs-lookup"><span data-stu-id="fe9c0-114">Azure Functions Management Role</span></span>

<span data-ttu-id="fe9c0-115">hello Azure Functions szerepkör állomás biztosít a funkciók a helyszíni hello kezelését.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-115">hello Azure Functions Management Role provides a host for hello management of your Functions on-premises.</span></span> <span data-ttu-id="fe9c0-116">Ez a szerepkör hello a következő feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="fe9c0-116">This role performs hello following tasks:</span></span>

* <span data-ttu-id="fe9c0-117">Hello Azure funkciók felügyeleti portálján, amely hello tárolása hello megegyezik a hello látni [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fe9c0-117">Hosting of hello Azure Functions Management Portal, which is hello hello same one you see in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="fe9c0-118">Ez lehetővé teszi a funkciók fejleszt hello azonos módon, mint az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-118">This lets you develop your functions in hello same way as you would in hello Azure portal.</span></span>
* <span data-ttu-id="fe9c0-119">Funkciók osztja több funkciók dolgozó között.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-119">Distributing functions across multiple Functions workers.</span></span>
* <span data-ttu-id="fe9c0-120">A közzétételi végpont biztosítása, így közzéteheti a függvényeket közvetlenül a Microsoft Visual Studio eszközből.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-120">Providing a publishing endpoint so that you can publish your functions direct from Microsoft Visual Studio.</span></span>

## <a name="azure-functions-worker-role"></a><span data-ttu-id="fe9c0-121">Az Azure Functions feldolgozói szerepkör</span><span class="sxs-lookup"><span data-stu-id="fe9c0-121">Azure Functions Worker Role</span></span>

<span data-ttu-id="fe9c0-122">hello Azure Functions feldolgozói szerepkörök telepítve van a Windows-tárolókban, és azt, ahol a funkciókódot hajt végre.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-122">hello Azure Functions Worker Roles are deployed in Windows Containers and this is where your function code executes.</span></span>  <span data-ttu-id="fe9c0-123">Több feldolgozói szerepköröket telepítheti a szervezetben, és ahol tehetik kulcs úgy használjon tartalék számítási teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-123">You can deploy multiple Worker Roles throughout your organization and is a key way in which customers can make use of spare compute power.</span></span>

## <a name="minimum-requirements"></a><span data-ttu-id="fe9c0-124">Minimumkövetelmények</span><span class="sxs-lookup"><span data-stu-id="fe9c0-124">Minimum Requirements</span></span>

<span data-ttu-id="fe9c0-125">tooget használatába hello Azure Functions Futtatókörnyezettel kell rendelkeznie a virtuális gép **Windows Server 2016 vagy a Windows 10 Creators Update** való hozzáférés tooa **SQL Server** példány.</span><span class="sxs-lookup"><span data-stu-id="fe9c0-125">tooget started with hello Azure Functions Runtime you must have a machine with **Windows Server 2016 or Windows 10 Creators Update** with access tooa **SQL Server** instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe9c0-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe9c0-126">Next Steps</span></span>

<span data-ttu-id="fe9c0-127">Telepítse a hello [Azure Functions Futtatókörnyezettel előzetes verzió](https://aka.ms/azafr)</span><span class="sxs-lookup"><span data-stu-id="fe9c0-127">Install hello [Azure Functions Runtime preview](https://aka.ms/azafr)</span></span>

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
