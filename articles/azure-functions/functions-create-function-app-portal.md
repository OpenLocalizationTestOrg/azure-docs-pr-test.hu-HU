---
title: "Függvény-alkalmazás létrehozása az Azure portálról |} Microsoft Docs"
description: "Létrehoz egy új funkció alkalmazást az Azure App Service-ben a portálról."
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
ms.openlocfilehash: 85a88c537415cd6f2b6bc005cc18e3baaa29e9a4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a><span data-ttu-id="3a87e-103">Függvény-alkalmazás létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3a87e-103">Create a function app from the Azure portal</span></span>

<span data-ttu-id="3a87e-104">Azure függvény alkalmazások az Azure App Service-infrastruktúrát használja.</span><span class="sxs-lookup"><span data-stu-id="3a87e-104">Azure Function Apps uses the Azure App Service infrastructure.</span></span> <span data-ttu-id="3a87e-105">Ez a témakör bemutatja, hogyan függvény alkalmazás létrehozása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3a87e-105">This topic shows you how to create a function app in the Azure portal.</span></span> <span data-ttu-id="3a87e-106">Egy függvény alkalmazást tartalmazó egyéni függvényei végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="3a87e-106">A function app is the container that hosts the execution of individual functions.</span></span> <span data-ttu-id="3a87e-107">Az App Service üzemeltetési terv egy függvény alkalmazást hoz létre, amikor a függvény app használhatja az App Service összes funkcióját.</span><span class="sxs-lookup"><span data-stu-id="3a87e-107">When you create a function app in the App Service hosting plan, your function app can use all the features of App Service.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="3a87e-108">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a87e-108">Create a function app</span></span>

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="3a87e-109">Ha létrehoz egy függvény alkalmazást, adja meg egy érvényes **alkalmazásnév**, amely csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3a87e-109">When you create a function app, supply a valid **App name**, which can contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="3a87e-110">Az aláhúzás (**_**) nem engedélyezett karakter.</span><span class="sxs-lookup"><span data-stu-id="3a87e-110">Underscore (**_**) is not an allowed character.</span></span>

<span data-ttu-id="3a87e-111">A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.</span><span class="sxs-lookup"><span data-stu-id="3a87e-111">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="3a87e-112">A tárfiók nevének egyedinek kell lennie az Azure rendszerben.</span><span class="sxs-lookup"><span data-stu-id="3a87e-112">Your storage account name must be unique within Azure.</span></span> 

<span data-ttu-id="3a87e-113">A függvény alkalmazás létrehozása után létrehozhat egyéni függvényei egy vagy több különböző nyelveken.</span><span class="sxs-lookup"><span data-stu-id="3a87e-113">After the function app is created, you can create individual functions in one or more different languages.</span></span> <span data-ttu-id="3a87e-114">Hozzon létre funkciók [a portál használatával](functions-create-first-azure-function.md#create-function), [folyamatos üzembe helyezés](functions-continuous-deployment.md), vagy a [FTP-vel feltöltése](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span><span class="sxs-lookup"><span data-stu-id="3a87e-114">Create functions [by using the portal](functions-create-first-azure-function.md#create-function), [continuous deployment](functions-continuous-deployment.md), or by [uploading with FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span></span>

## <a name="service-plans"></a><span data-ttu-id="3a87e-115">Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="3a87e-115">Service plans</span></span>

<span data-ttu-id="3a87e-116">Az Azure Functions van két különböző service-csomagokról: fogyasztás terv és az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="3a87e-116">Azure Functions has two different service plans: Consumption plan and App Service plan.</span></span> <span data-ttu-id="3a87e-117">A felhasználási terv automatikusan lefoglalja számára a számítási teljesítményt, ha a kódja fut, a skála kibővített terhelés kezelése érdekében szükség szerint, és majd méretezik-a kód nem futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="3a87e-117">The Consumption plan automatically allocates compute power when your code is running, scales-out as necessary to handle load, and then scales-in when code is not running.</span></span> <span data-ttu-id="3a87e-118">Az App Service-csomag a függvény alkalmazás hozzáférést biztosít az App Service minden lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3a87e-118">The App Service plan gives your function app access to all the facilities of App Service.</span></span> <span data-ttu-id="3a87e-119">Ki kell választania a service-csomag, a függvény alkalmazás létrehozása, és azt jelenleg nem módosul.</span><span class="sxs-lookup"><span data-stu-id="3a87e-119">You must choose your service plan when your function app is created, and it cannot currently be changed.</span></span> <span data-ttu-id="3a87e-120">További információkért lásd: [válassza ki az Azure Functions üzemeltetési terv](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="3a87e-120">For more information, see [Choose an Azure Functions hosting plan](functions-scale.md).</span></span>

<span data-ttu-id="3a87e-121">Ha azt tervezi, JavaScript-funkcióként futhat az App Service-csomagot, válasszon egy tervet a kevesebb magok.</span><span class="sxs-lookup"><span data-stu-id="3a87e-121">If you are planning to run JavaScript functions on an App Service plan, you should choose a plan with fewer cores.</span></span> <span data-ttu-id="3a87e-122">További információkért lásd: a [függvények JavaScript hivatkozás](functions-reference-node.md#choose-single-core-app-service-plans).</span><span class="sxs-lookup"><span data-stu-id="3a87e-122">For more information, see the [JavaScript reference for Functions](functions-reference-node.md#choose-single-core-app-service-plans).</span></span>

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a><span data-ttu-id="3a87e-123">Tárolási fiókra vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="3a87e-123">Storage account requirements</span></span>

<span data-ttu-id="3a87e-124">Egy függvény alkalmazást az App Service létrehozásakor vagy létre kell hoznia egy általános célú Azure Storage-fiók, amely támogatja a Blob, Queue és Table storage kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="3a87e-124">When creating a function app in App Service, you must create or link to a general-purpose Azure Storage account that supports Blob, Queue, and Table storage.</span></span> <span data-ttu-id="3a87e-125">Belsőleg funkciók tárolást használ műveletek, például eseményindítók kezelése és naplózási funkciót végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="3a87e-125">Internally, Functions uses Storage for operations such as managing triggers and logging function executions.</span></span> <span data-ttu-id="3a87e-126">Néhány tárfiókok nem támogatják az üzenetsorok és táblák, például csak a blob storage-fiókok, a prémium szintű Azure Storage és a ZRS replikáció általános célú tárfiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="3a87e-126">Some storage accounts do not support queues and tables, such as blob-only storage accounts, Azure Premium Storage, and general-purpose storage accounts with ZRS replication.</span></span> <span data-ttu-id="3a87e-127">Ezek a fiókok kiszűri kívüli a Storage-fiók panelen egy függvény alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3a87e-127">These accounts are filtered out of from the Storage Account blade when creating a function app.</span></span>

>[!NOTE]
><span data-ttu-id="3a87e-128">A felhasználás üzemeltetési terv használatakor függvény kód és a kötés konfigurációs fájljainak Azure File storage a fő tárfiókban vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="3a87e-128">When using the Consumption hosting plan, your function code and binding configuration files are stored in Azure File storage in the main storage account.</span></span> <span data-ttu-id="3a87e-129">A fő storage-fiók törlésekor a tartalom törlődik, és nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="3a87e-129">When you delete the main storage account, this content is deleted and cannot be recovered.</span></span>

<span data-ttu-id="3a87e-130">Tárfióktípusokat kapcsolatos további információkért lásd: [az Azure Storage szolgáltatásainak bemutatása](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span><span class="sxs-lookup"><span data-stu-id="3a87e-130">To learn more about storage account types, see [Introducing the Azure Storage Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3a87e-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a87e-131">Next steps</span></span>

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



