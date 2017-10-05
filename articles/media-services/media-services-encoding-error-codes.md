---
title: "Az Azure Media Services kódolási hibakódok |} Microsoft Docs"
description: "Ez a témakör felsorolja a abban az esetben, ha hiba történt a kódolási feladat a végrehajtás során visszatérő hibakódok..."
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
ms.openlocfilehash: f4fd2212d19f89148dde08c75c5a48cdd322d029
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="encoding-error-codes"></a><span data-ttu-id="eaeb3-103">Kódolási hibakódok</span><span class="sxs-lookup"><span data-stu-id="eaeb3-103">Encoding error codes</span></span>

<span data-ttu-id="eaeb3-104">A következő táblázat sorolja fel, ha hiba történt a kódolási feladat a végrehajtás során visszatérő hibakódok.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-104">The following table lists error codes that could be returned in case an error was encountered during the encoding task execution.</span></span>  <span data-ttu-id="eaeb3-105">Hiba legutolsó részletes adatai a .NET-kódban, amelyet a [hiba részletei](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) osztály.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-105">To get error details in your .NET code, use the [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) class.</span></span> <span data-ttu-id="eaeb3-106">Hiba legutolsó részletes adatai a REST-kódban, amelyet a [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API-t.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-106">To get error details in your REST code, use the [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span></span>

| <span data-ttu-id="eaeb3-107">ErrorDetail.Code</span><span class="sxs-lookup"><span data-stu-id="eaeb3-107">ErrorDetail.Code</span></span> | <span data-ttu-id="eaeb3-108">Hiba lehetséges okai</span><span class="sxs-lookup"><span data-stu-id="eaeb3-108">Possible causes for error</span></span> |
| --- | --- |
| <span data-ttu-id="eaeb3-109">Ismeretlen</span><span class="sxs-lookup"><span data-stu-id="eaeb3-109">Unknown</span></span> |<span data-ttu-id="eaeb3-110">Ismeretlen hiba történt a feladat végrehajtása</span><span class="sxs-lookup"><span data-stu-id="eaeb3-110">Unknown error while executing the task</span></span> |
| <span data-ttu-id="eaeb3-111">ErrorDownloadingInputAssetMalformedContent</span><span class="sxs-lookup"><span data-stu-id="eaeb3-111">ErrorDownloadingInputAssetMalformedContent</span></span> |<span data-ttu-id="eaeb3-112">Kategória hibák, amely lefedi a hibák például rossz fájlneveket, nulla hosszúságú fájlok, a hibás bemeneti eszköz letöltése a formázza és így tovább.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-112">Category of errors that covers errors in downloading input asset such as bad file names, zero length files, incorrect formats and so on.</span></span> |
| <span data-ttu-id="eaeb3-113">ErrorDownloadingInputAssetServiceFailure</span><span class="sxs-lookup"><span data-stu-id="eaeb3-113">ErrorDownloadingInputAssetServiceFailure</span></span> |<span data-ttu-id="eaeb3-114">Kategória hibák szolgáltatás oldalán - letöltése közben hálózati vagy tárolási hibák példa problémákat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-114">Category of errors that covers problems on the service side - for example network or storage errors while downloading.</span></span> |
| <span data-ttu-id="eaeb3-115">ErrorParsingConfiguration</span><span class="sxs-lookup"><span data-stu-id="eaeb3-115">ErrorParsingConfiguration</span></span> |<span data-ttu-id="eaeb3-116">A hibák kategória ahol feladat <see cref="MediaTask.PrivateData"/> (konfiguráció) érvénytelen, például a konfiguráció nem érvényes rendszerrel előre, vagy érvénytelen XML-kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-116">Category of errors where task <see cref="MediaTask.PrivateData"/> (configuration) is not valid, for example the configuration is not a valid system preset or it contains invalid XML.</span></span> |
| <span data-ttu-id="eaeb3-117">ErrorExecutingTaskMalformedContent</span><span class="sxs-lookup"><span data-stu-id="eaeb3-117">ErrorExecutingTaskMalformedContent</span></span> |<span data-ttu-id="eaeb3-118">Hol található a bemeneti médiafájljait okozza hiba a tevékenység végrehajtása közben hibákat kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-118">Category of errors during the execution of the task where issues inside the input media files cause failure.</span></span> |
| <span data-ttu-id="eaeb3-119">ErrorExecutingTaskUnsupportedFormat</span><span class="sxs-lookup"><span data-stu-id="eaeb3-119">ErrorExecutingTaskUnsupportedFormat</span></span> |<span data-ttu-id="eaeb3-120">Kategória hibák, ahol a media feldolgozója nem képes feldolgozni a megadott fájlok - adathordozó formázása nem támogatott, vagy nem felel meg a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-120">Category of errors where the media processor cannot process the files provided - media format not supported, or does not match the Configuration.</span></span> <span data-ttu-id="eaeb3-121">Egy eszköz, amelynek csak videó csak kimenete létrehozásához például közben</span><span class="sxs-lookup"><span data-stu-id="eaeb3-121">For example, trying to produce an audio-only output from an asset that has only video</span></span> |
| <span data-ttu-id="eaeb3-122">ErrorProcessingTask</span><span class="sxs-lookup"><span data-stu-id="eaeb3-122">ErrorProcessingTask</span></span> |<span data-ttu-id="eaeb3-123">Más hibák, amelyek a tevékenység feldolgozása során találkozik az adathordozó processzor tartalom nem kapcsolt kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-123">Category of other errors that the media processor encounters during the processing of the task that are unrelated to content.</span></span> |
| <span data-ttu-id="eaeb3-124">ErrorUploadingOutputAsset</span><span class="sxs-lookup"><span data-stu-id="eaeb3-124">ErrorUploadingOutputAsset</span></span> |<span data-ttu-id="eaeb3-125">A hibák feltöltésekor a rendszer a kimeneti adategységen kategória</span><span class="sxs-lookup"><span data-stu-id="eaeb3-125">Category of errors when uploading the output asset</span></span> |
| <span data-ttu-id="eaeb3-126">ErrorCancelingTask</span><span class="sxs-lookup"><span data-stu-id="eaeb3-126">ErrorCancelingTask</span></span> |<span data-ttu-id="eaeb3-127">Kategória fedik le a hibák, a feladat megszakítására tett próbálkozás során hibák</span><span class="sxs-lookup"><span data-stu-id="eaeb3-127">Category of errors to cover failures when attempting to cancel the Task</span></span> |
| <span data-ttu-id="eaeb3-128">TransientError</span><span class="sxs-lookup"><span data-stu-id="eaeb3-128">TransientError</span></span> |<span data-ttu-id="eaeb3-129">Kategória hibák, amelyek átmeneti probléma (például)</span><span class="sxs-lookup"><span data-stu-id="eaeb3-129">Category of errors to cover transient issues (eg.</span></span> <span data-ttu-id="eaeb3-130">ideiglenes hálózati probléma az Azure Storage szolgáltatással)</span><span class="sxs-lookup"><span data-stu-id="eaeb3-130">temporary networking issues with Azure Storage)</span></span> |

<span data-ttu-id="eaeb3-131">A Súgó a **Media Services** team, nyissa meg a [támogatja a jegy](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="eaeb3-131">To get help from the **Media Services** team, open a [support ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="eaeb3-132">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="eaeb3-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="eaeb3-133">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="eaeb3-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="eaeb3-134">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="eaeb3-134">Related articles</span></span>
* [<span data-ttu-id="eaeb3-135">Speciális feladatokra kódolási Media Encoder Standard készletek testreszabásával.</span><span class="sxs-lookup"><span data-stu-id="eaeb3-135">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="eaeb3-136">Kvóták és korlátozások</span><span class="sxs-lookup"><span data-stu-id="eaeb3-136">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
