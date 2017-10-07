---
title: "egy függvény hello Azure portál alkalmazásából aaaCreate |} Microsoft Docs"
description: "Létrehoz egy új funkció alkalmazást az Azure App Service hello portálról."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c531fc71c798edf22e25a5f4b79c15413809dc86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-from-hello-azure-portal"></a><span data-ttu-id="8f738-103">Az Azure-portálon hello függvény-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f738-103">Create a function app from hello Azure portal</span></span>

<span data-ttu-id="8f738-104">Az Azure függvény Apps hello Azure App Service-infrastruktúrát használja.</span><span class="sxs-lookup"><span data-stu-id="8f738-104">Azure Function Apps uses hello Azure App Service infrastructure.</span></span> <span data-ttu-id="8f738-105">Ez a témakör bemutatja, hogyan toocreate egy függvény alkalmazás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="8f738-105">This topic shows you how toocreate a function app in hello Azure portal.</span></span> <span data-ttu-id="8f738-106">Egy függvény app egyéni függvényei hello végrehajtásának hello tárolót.</span><span class="sxs-lookup"><span data-stu-id="8f738-106">A function app is hello container that hosts hello execution of individual functions.</span></span> <span data-ttu-id="8f738-107">Amikor létrehoz egy függvény alkalmazást az App Service-csomag üzemeltetési hello, a függvény app funkciók is használhatók összes hello az App Service.</span><span class="sxs-lookup"><span data-stu-id="8f738-107">When you create a function app in hello App Service hosting plan, your function app can use all hello features of App Service.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="8f738-108">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f738-108">Create a function app</span></span>

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="8f738-109">Ha létrehoz egy függvény alkalmazást, adja meg egy érvényes **alkalmazásnév**, amely csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="8f738-109">When you create a function app, supply a valid **App name**, which can contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="8f738-110">Az aláhúzás (**_**) nem engedélyezett karakter.</span><span class="sxs-lookup"><span data-stu-id="8f738-110">Underscore (**_**) is not an allowed character.</span></span>

<span data-ttu-id="8f738-111">A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.</span><span class="sxs-lookup"><span data-stu-id="8f738-111">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="8f738-112">A tárfiók nevének egyedinek kell lennie az Azure rendszerben.</span><span class="sxs-lookup"><span data-stu-id="8f738-112">Your storage account name must be unique within Azure.</span></span> 

<span data-ttu-id="8f738-113">Hello függvény alkalmazás létrehozása után létrehozhat egyéni függvényei egy vagy több különböző nyelveken.</span><span class="sxs-lookup"><span data-stu-id="8f738-113">After hello function app is created, you can create individual functions in one or more different languages.</span></span> <span data-ttu-id="8f738-114">Hozzon létre funkciók [hello portál használatával](functions-create-first-azure-function.md#create-function), [folyamatos üzembe helyezés](functions-continuous-deployment.md), vagy a [FTP-vel feltöltése](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span><span class="sxs-lookup"><span data-stu-id="8f738-114">Create functions [by using hello portal](functions-create-first-azure-function.md#create-function), [continuous deployment](functions-continuous-deployment.md), or by [uploading with FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span></span>

## <a name="service-plans"></a><span data-ttu-id="8f738-115">Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="8f738-115">Service plans</span></span>

<span data-ttu-id="8f738-116">Az Azure Functions van két különböző service-csomagokról: fogyasztás terv és az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="8f738-116">Azure Functions has two different service plans: Consumption plan and App Service plan.</span></span> <span data-ttu-id="8f738-117">hello fogyasztás terv automatikusan lefoglalja számára a számítási teljesítményt, ha a kódja fut., és méretezik kibővített szükséges toohandle terhelés, majd méretezik-a kód nem futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="8f738-117">hello Consumption plan automatically allocates compute power when your code is running, scales-out as necessary toohandle load, and then scales-in when code is not running.</span></span> <span data-ttu-id="8f738-118">App Service-csomag hello biztosít a függvény app hozzáférés tooall hello létesítményekben az App Service.</span><span class="sxs-lookup"><span data-stu-id="8f738-118">hello App Service plan gives your function app access tooall hello facilities of App Service.</span></span> <span data-ttu-id="8f738-119">Ki kell választania a service-csomag, a függvény alkalmazás létrehozása, és azt jelenleg nem módosul.</span><span class="sxs-lookup"><span data-stu-id="8f738-119">You must choose your service plan when your function app is created, and it cannot currently be changed.</span></span> <span data-ttu-id="8f738-120">További információkért lásd: [válassza ki az Azure Functions üzemeltetési terv](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="8f738-120">For more information, see [Choose an Azure Functions hosting plan](functions-scale.md).</span></span>

<span data-ttu-id="8f738-121">Ha azt tervezi, toorun JavaScript-funkcióként az App Service-csomagot, válasszon egy tervet a kevesebb magok.</span><span class="sxs-lookup"><span data-stu-id="8f738-121">If you are planning toorun JavaScript functions on an App Service plan, you should choose a plan with fewer cores.</span></span> <span data-ttu-id="8f738-122">További információkért lásd: hello [függvények JavaScript hivatkozás](functions-reference-node.md#choose-single-core-app-service-plans).</span><span class="sxs-lookup"><span data-stu-id="8f738-122">For more information, see hello [JavaScript reference for Functions](functions-reference-node.md#choose-single-core-app-service-plans).</span></span>

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a><span data-ttu-id="8f738-123">Tárolási fiókra vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="8f738-123">Storage account requirements</span></span>

<span data-ttu-id="8f738-124">Egy függvény alkalmazást az App Service létrehozásakor létre kell hoznia vagy tooa általános célú Azure Storage-fiók, amely támogatja a Blob, Queue és Table storage hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="8f738-124">When creating a function app in App Service, you must create or link tooa general-purpose Azure Storage account that supports Blob, Queue, and Table storage.</span></span> <span data-ttu-id="8f738-125">Belsőleg funkciók tárolást használ műveletek, például eseményindítók kezelése és naplózási funkciót végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="8f738-125">Internally, Functions uses Storage for operations such as managing triggers and logging function executions.</span></span> <span data-ttu-id="8f738-126">Néhány tárfiókok nem támogatják az üzenetsorok és táblák, például csak a blob storage-fiókok, a prémium szintű Azure Storage és a ZRS replikáció általános célú tárfiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="8f738-126">Some storage accounts do not support queues and tables, such as blob-only storage accounts, Azure Premium Storage, and general-purpose storage accounts with ZRS replication.</span></span> <span data-ttu-id="8f738-127">Ezek a fiókok kiszűri kívüli hello Storage-fiók panelen egy függvény alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8f738-127">These accounts are filtered out of from hello Storage Account blade when creating a function app.</span></span>

>[!NOTE]
><span data-ttu-id="8f738-128">Hello fogyasztás üzemeltetési terv használata esetén a függvény kód és a kötés a konfigurációs fájlok Azure File storage hello fő tárfiókban lévő vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="8f738-128">When using hello Consumption hosting plan, your function code and binding configuration files are stored in Azure File storage in hello main storage account.</span></span> <span data-ttu-id="8f738-129">Hello fő storage-fiók törlésekor a tartalom törlődik, és nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="8f738-129">When you delete hello main storage account, this content is deleted and cannot be recovered.</span></span>

<span data-ttu-id="8f738-130">toolearn tárfióktípusokat, kapcsolatos további információkért lásd: [hello Azure Storage szolgáltatásainak bemutatása](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span><span class="sxs-lookup"><span data-stu-id="8f738-130">toolearn more about storage account types, see [Introducing hello Azure Storage Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8f738-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f738-131">Next steps</span></span>

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



