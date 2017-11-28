---
title: "aaaAzure Media Services kódolási hibakódok |} Microsoft Docs"
description: "Ez a témakör felsorolja a abban az esetben, ha hiba történt a feladat a végrehajtás kódolás hello során visszatérő hibakódok..."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: b69b6abee797c40c9b8b8f23bf2398273c170e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encoding-error-codes"></a><span data-ttu-id="0f846-103">Kódolási hibakódok</span><span class="sxs-lookup"><span data-stu-id="0f846-103">Encoding error codes</span></span>

<span data-ttu-id="0f846-104">hello következő táblázat sorolja fel, ha hiba történt a feladat a végrehajtás kódolás hello során visszatérő hibakódok.</span><span class="sxs-lookup"><span data-stu-id="0f846-104">hello following table lists error codes that could be returned in case an error was encountered during hello encoding task execution.</span></span>  <span data-ttu-id="0f846-105">tooget hiba részletes adatait, a .NET-kódot, a hello használata [hiba részletei](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="0f846-105">tooget error details in your .NET code, use hello [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) class.</span></span> <span data-ttu-id="0f846-106">tooget hiba részletes adatait, a többi kódban hello használata [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API-t.</span><span class="sxs-lookup"><span data-stu-id="0f846-106">tooget error details in your REST code, use hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span></span>

| <span data-ttu-id="0f846-107">ErrorDetail.Code</span><span class="sxs-lookup"><span data-stu-id="0f846-107">ErrorDetail.Code</span></span> | <span data-ttu-id="0f846-108">Hiba lehetséges okai</span><span class="sxs-lookup"><span data-stu-id="0f846-108">Possible causes for error</span></span> |
| --- | --- |
| <span data-ttu-id="0f846-109">Ismeretlen</span><span class="sxs-lookup"><span data-stu-id="0f846-109">Unknown</span></span> |<span data-ttu-id="0f846-110">Hello feladat végrehajtása közben ismeretlen hiba</span><span class="sxs-lookup"><span data-stu-id="0f846-110">Unknown error while executing hello task</span></span> |
| <span data-ttu-id="0f846-111">ErrorDownloadingInputAssetMalformedContent</span><span class="sxs-lookup"><span data-stu-id="0f846-111">ErrorDownloadingInputAssetMalformedContent</span></span> |<span data-ttu-id="0f846-112">Kategória hibák, amely lefedi a hibák például rossz fájlneveket, nulla hosszúságú fájlok, a hibás bemeneti eszköz letöltése a formázza és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0f846-112">Category of errors that covers errors in downloading input asset such as bad file names, zero length files, incorrect formats and so on.</span></span> |
| <span data-ttu-id="0f846-113">ErrorDownloadingInputAssetServiceFailure</span><span class="sxs-lookup"><span data-stu-id="0f846-113">ErrorDownloadingInputAssetServiceFailure</span></span> |<span data-ttu-id="0f846-114">Kategória hibák hello szolgáltatás oldalán - letöltése közben hálózati vagy tárolási hibák példa problémákat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0f846-114">Category of errors that covers problems on hello service side - for example network or storage errors while downloading.</span></span> |
| <span data-ttu-id="0f846-115">ErrorParsingConfiguration</span><span class="sxs-lookup"><span data-stu-id="0f846-115">ErrorParsingConfiguration</span></span> |<span data-ttu-id="0f846-116">A hibák kategória ahol feladat <see cref="MediaTask.PrivateData"/> (konfiguráció) érvénytelen, például hello konfigurációja nem érvényes rendszerrel előre, vagy érvénytelen XML-kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0f846-116">Category of errors where task <see cref="MediaTask.PrivateData"/> (configuration) is not valid, for example hello configuration is not a valid system preset or it contains invalid XML.</span></span> |
| <span data-ttu-id="0f846-117">ErrorExecutingTaskMalformedContent</span><span class="sxs-lookup"><span data-stu-id="0f846-117">ErrorExecutingTaskMalformedContent</span></span> |<span data-ttu-id="0f846-118">Ahol problémák belül hello bemeneti médiafájlok hiba következtében hello feladat hello végrehajtása során hibák kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="0f846-118">Category of errors during hello execution of hello task where issues inside hello input media files cause failure.</span></span> |
| <span data-ttu-id="0f846-119">ErrorExecutingTaskUnsupportedFormat</span><span class="sxs-lookup"><span data-stu-id="0f846-119">ErrorExecutingTaskUnsupportedFormat</span></span> |<span data-ttu-id="0f846-120">Kategória hibák, ahol hello media feldolgozója nem képes feldolgozni a megadott hello fájlok - adathordozó formázása nem támogatott, vagy nem felel meg a konfigurációs hello.</span><span class="sxs-lookup"><span data-stu-id="0f846-120">Category of errors where hello media processor cannot process hello files provided - media format not supported, or does not match hello Configuration.</span></span> <span data-ttu-id="0f846-121">Például próbált tooproduce egy eszköz, amelynek csak videó csak kimenete</span><span class="sxs-lookup"><span data-stu-id="0f846-121">For example, trying tooproduce an audio-only output from an asset that has only video</span></span> |
| <span data-ttu-id="0f846-122">ErrorProcessingTask</span><span class="sxs-lookup"><span data-stu-id="0f846-122">ErrorProcessingTask</span></span> |<span data-ttu-id="0f846-123">Más hibák media processzor hello kategória észlel, amelyek nem kapcsolódó toocontent hello feladat hello feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="0f846-123">Category of other errors that hello media processor encounters during hello processing of hello task that are unrelated toocontent.</span></span> |
| <span data-ttu-id="0f846-124">ErrorUploadingOutputAsset</span><span class="sxs-lookup"><span data-stu-id="0f846-124">ErrorUploadingOutputAsset</span></span> |<span data-ttu-id="0f846-125">A hibák hello kimeneti adategységen feltöltésekor kategória</span><span class="sxs-lookup"><span data-stu-id="0f846-125">Category of errors when uploading hello output asset</span></span> |
| <span data-ttu-id="0f846-126">ErrorCancelingTask</span><span class="sxs-lookup"><span data-stu-id="0f846-126">ErrorCancelingTask</span></span> |<span data-ttu-id="0f846-127">A hibák toocover hibákat megkísérlésekor. a feladat toocancel hello kategória</span><span class="sxs-lookup"><span data-stu-id="0f846-127">Category of errors toocover failures when attempting toocancel hello Task</span></span> |
| <span data-ttu-id="0f846-128">TransientError</span><span class="sxs-lookup"><span data-stu-id="0f846-128">TransientError</span></span> |<span data-ttu-id="0f846-129">Kategória hibák toocover átmeneti problémákat (például)</span><span class="sxs-lookup"><span data-stu-id="0f846-129">Category of errors toocover transient issues (eg.</span></span> <span data-ttu-id="0f846-130">ideiglenes hálózati probléma az Azure Storage szolgáltatással)</span><span class="sxs-lookup"><span data-stu-id="0f846-130">temporary networking issues with Azure Storage)</span></span> |

<span data-ttu-id="0f846-131">a hello tooget súgó **Media Services** team, nyissa meg a [támogatja a jegy](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="0f846-131">tooget help from hello **Media Services** team, open a [support ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0f846-132">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0f846-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0f846-133">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0f846-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="0f846-134">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="0f846-134">Related articles</span></span>
* [<span data-ttu-id="0f846-135">Speciális feladatokra kódolási Media Encoder Standard készletek testreszabásával.</span><span class="sxs-lookup"><span data-stu-id="0f846-135">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="0f846-136">Kvóták és korlátozások</span><span class="sxs-lookup"><span data-stu-id="0f846-136">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
