---
title: "az Azure Media Services-fiók Azure StorSimple aaaUpload fájlok |} Microsoft Docs"
description: "Ez a cikk rövid áttekintést nyújt az Azure StorSimple Data Managerről. hello cikket is hivatkozásokat tartalmaz, amelyek bemutatják, hogyan tootutorials StorSimple tooextract adatait, és töltse fel eszközök tooan Azure Media Services-fiók."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: juliako
ms.openlocfilehash: 7e9712aa480106bbd5fcc63eaecf0418b24a8bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a><span data-ttu-id="5ad61-104">Fájlok feltöltése Azure Media Services-fiókba az Azure StorSimple-ből</span><span class="sxs-lookup"><span data-stu-id="5ad61-104">Upload files into an Azure Media Services account from Azure StorSimple</span></span>

<span data-ttu-id="5ad61-105">Ez a cikk rövid áttekintést nyújt az Azure StorSimple Data Managerről.</span><span class="sxs-lookup"><span data-stu-id="5ad61-105">This article gives a brief overview of Azure StorSimple Data Manager.</span></span> <span data-ttu-id="5ad61-106">hello cikket is hivatkozásokat tartalmaz, amelyek bemutatják, hogyan tootutorials StorSimple tooextract adatait, majd töltse fel ezeket az adatokat, az eszközök tooan Azure Media Services (AMS) fiók.</span><span class="sxs-lookup"><span data-stu-id="5ad61-106">hello article also links tootutorials that show you how tooextract data from StorSimple and upload this data as assets tooan Azure Media Services (AMS) account.</span></span>

> 
> [!NOTE]
> <span data-ttu-id="5ad61-107">Az Azure StorSimple Data Manager jelenleg privát előzetes verzióban érhető el.</span><span class="sxs-lookup"><span data-stu-id="5ad61-107">Azure StorSimple Data Manager is currently in private preview.</span></span> 
> 

## <a name="overview"></a><span data-ttu-id="5ad61-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5ad61-108">Overview</span></span>

<span data-ttu-id="5ad61-109">A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="5ad61-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="5ad61-110">hello eszköz tartalmazhat videó, hang, képeket, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.) Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="5ad61-110">hello Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.) Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="5ad61-111">[Az Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) által használt felhőbeli tárhelyén hello kiterjesztése a helyszíni megoldás, és automatikusan tiers adatok között hello a helyszíni és felhőalapú tárolására.</span><span class="sxs-lookup"><span data-stu-id="5ad61-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) uses cloud storage as an extension of hello on-premises solution and automatically tiers data across hello on-premises storage and cloud storage.</span></span> <span data-ttu-id="5ad61-112">hello StorSimple eszköz dedupes, és tömöríti az adatokat, így nagy fájlok toohello felhő küldési nagyon hatékony toohello felhő elküldés előtt.</span><span class="sxs-lookup"><span data-stu-id="5ad61-112">hello StorSimple device dedupes and compresses your data before sending it toohello cloud making it very efficient for sending large files toohello cloud.</span></span> <span data-ttu-id="5ad61-113">Hello [StorSimple adatkezelő](../storsimple/storsimple-data-manager-overview.md) szolgáltatást biztosít az API-k, amelyek lehetővé teszik az Ön tooextract adatok a StorSimple és mutatni az AMS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="5ad61-113">hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) service provides APIs that enable you tooextract data from StorSimple and present it as AMS assets.</span></span>

## <a name="get-started"></a><span data-ttu-id="5ad61-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5ad61-114">Get started</span></span>

1. <span data-ttu-id="5ad61-115">[Media Services-fiók létrehozása](media-services-portal-create-account.md) szeretné tootransfer hello eszközök.</span><span class="sxs-lookup"><span data-stu-id="5ad61-115">[Create a Media Services account](media-services-portal-create-account.md) into which you want tootransfer hello assets.</span></span>
2. <span data-ttu-id="5ad61-116">Regisztrálhat adatkezelő előzetes verziójára, a hello [StorSimple adatkezelő](../storsimple/storsimple-data-manager-overview.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="5ad61-116">Sign up for Data Manager preview, as described in hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) article.</span></span>
3. <span data-ttu-id="5ad61-117">Hozzon létre egy StorSimple Data Manager-fiókot.</span><span class="sxs-lookup"><span data-stu-id="5ad61-117">Create a StorSimple Data Manager account.</span></span>
4. <span data-ttu-id="5ad61-118">Hozzon létre egy adatátalakítási feladatot, amely a futtatásakor adatokat gyűjt egy StorSimple-eszközről, és továbbítja azokat objektumokként egy AMS-fiókba.</span><span class="sxs-lookup"><span data-stu-id="5ad61-118">Create a data transformation job that when runs, extracts data from a StorSimple device and transfers it into an AMS account as assets.</span></span> 

    <span data-ttu-id="5ad61-119">Amikor hello feladat elindul, a tároló várólista jön létre.</span><span class="sxs-lookup"><span data-stu-id="5ad61-119">When hello job starts running, a storage queue is created.</span></span> <span data-ttu-id="5ad61-120">A sorba az átalakított blobokkal kapcsolatos üzenetek kerülnek, amint a blobok elkészültek.</span><span class="sxs-lookup"><span data-stu-id="5ad61-120">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="5ad61-121">a várólista nevét hello van hello ugyanaz, mint a hello feladatdefiníció hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5ad61-121">hello name of this queue is hello same as hello name of hello job definition.</span></span> <span data-ttu-id="5ad61-122">A várólista toodetermine használható amikor eszköz, készen áll, és hívja meg a kívánt Media Services művelet toorun rajta.</span><span class="sxs-lookup"><span data-stu-id="5ad61-122">You can use this queue toodetermine when as asset is ready and call your desired Media Services operation toorun on it.</span></span> <span data-ttu-id="5ad61-123">Például a várólista tootrigger egy Azure-függvény, amely rendelkezik a szükséges kód hello a Media Services azt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5ad61-123">For example, you can use this queue tootrigger an Azure Function that has hello necessary Media Services code in it.</span></span>

## <a name="see-also"></a><span data-ttu-id="5ad61-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5ad61-124">See also</span></span>

[<span data-ttu-id="5ad61-125">Használjon .net SDK hello hello adatkezelő tootrigger feladatok</span><span class="sxs-lookup"><span data-stu-id="5ad61-125">Use hello .Net SDK tootrigger jobs in hello Data Manager</span></span>](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="5ad61-126">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="5ad61-126">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5ad61-127">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="5ad61-127">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="5ad61-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ad61-128">Next steps</span></span>

<span data-ttu-id="5ad61-129">Most már kódolhatja a feltöltött adategységeket.</span><span class="sxs-lookup"><span data-stu-id="5ad61-129">You can now encode your uploaded assets.</span></span> <span data-ttu-id="5ad61-130">További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).</span><span class="sxs-lookup"><span data-stu-id="5ad61-130">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>
