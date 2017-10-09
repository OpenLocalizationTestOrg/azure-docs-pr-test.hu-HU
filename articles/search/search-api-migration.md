---
title: "Azure Search szolgáltatás REST API verziója 2016-09-01 aaaUpgrading toohello |} Microsoft Docs"
description: "Azure Search szolgáltatás REST API verziója 2016-09-01 toohello frissítése"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: d0276b9cc52996a59f9aa726c27e62c6082eb908
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-service-rest-api-version-2016-09-01"></a><span data-ttu-id="c7d54-103">Azure Search szolgáltatás REST API verziója 2016-09-01 toohello frissítése</span><span class="sxs-lookup"><span data-stu-id="c7d54-103">Upgrading toohello Azure Search Service REST API version 2016-09-01</span></span>
<span data-ttu-id="c7d54-104">2015-02-28 verzió vagy 2015-02-28-előzetes verziója hello használata [Azure Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), ez a cikk segít frissíteni alkalmazás toouse hello következő általánosan elérhető API-verzióval, 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="c7d54-104">If you're using version 2015-02-28 or 2015-02-28-Preview of hello [Azure Search Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), this article will help you upgrade your application toouse hello next generally available API version, 2016-09-01.</span></span>

<span data-ttu-id="c7d54-105">Hello REST API verziója 2016-09-01 korábbi verzióihoz képest végrehajtott egyes módosításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c7d54-105">Version 2016-09-01 of hello REST API contains some changes from earlier versions.</span></span> <span data-ttu-id="c7d54-106">Ezek a legtöbbször visszamenőlegesen kompatibilis, így a kód módosítása csak csatlakozhatnak, attól függően, hogy milyen verziójú használata előtt érdemes beállítani.</span><span class="sxs-lookup"><span data-stu-id="c7d54-106">These are mostly backward compatible, so changing your code should require only minimal effort, depending on which version you were using before.</span></span> <span data-ttu-id="c7d54-107">Lásd: [lépéseket tooupgrade](#UpgradeSteps) útmutatást toochange kód toouse hello új API-verzióval.</span><span class="sxs-lookup"><span data-stu-id="c7d54-107">See [Steps tooupgrade](#UpgradeSteps) for instructions on how toochange your code toouse hello new API version.</span></span>

> [!NOTE]
> <span data-ttu-id="c7d54-108">Az Azure Search szolgáltatás példányát támogatja több REST API-t, többek között a legújabb hello.</span><span class="sxs-lookup"><span data-stu-id="c7d54-108">Your Azure Search service instance supports several REST API versions, including hello latest one.</span></span> <span data-ttu-id="c7d54-109">Egy verzió toouse tovább már nem hello legújabb, de azt javasoljuk, hogy áttelepítette-e a kód toouse hello legújabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c7d54-109">You can continue toouse a version when it is no longer hello latest one, but we recommend that you migrate your code toouse hello newest version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a><span data-ttu-id="c7d54-110">2016-09-01 verzió újdonságai</span><span class="sxs-lookup"><span data-stu-id="c7d54-110">What's new in version 2016-09-01</span></span>
<span data-ttu-id="c7d54-111">Verzió 2016-09-01 hello második általánosan elérhető kiadása hello Azure Search szolgáltatás REST API.</span><span class="sxs-lookup"><span data-stu-id="c7d54-111">Version 2016-09-01 is hello second generally available release of hello Azure Search Service REST API.</span></span> <span data-ttu-id="c7d54-112">Az API-verzió az új funkciói a következők:</span><span class="sxs-lookup"><span data-stu-id="c7d54-112">New features in this API version include:</span></span>

* <span data-ttu-id="c7d54-113">[Egyéni lekérdezések](https://aka.ms/customanalyzers), amely hello folyamat alakításával előállított szöveges példánycímkének és kereshető jogkivonatokba tootake ellenőrzését lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="c7d54-113">[Custom analyzers](https://aka.ms/customanalyzers), which allow you tootake control over hello process of converting text into indexable and searchable tokens.</span></span>
* <span data-ttu-id="c7d54-114">[Az Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) és [Azure Table Storage](search-howto-indexing-azure-tables.md) indexelők, amelyek lehetővé teszik tooeasily adatok importálása az Azure storage Azure Search szolgáltatásba történő ütemezés vagy igény szerinti.</span><span class="sxs-lookup"><span data-stu-id="c7d54-114">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexers, which allow you tooeasily import data from Azure storage into Azure Search on a schedule or on-demand.</span></span>
* <span data-ttu-id="c7d54-115">[Hozzárendelések mezőben](search-indexer-field-mappings.md), amelyek lehetővé teszik, hogy toocustomize hogyan indexelők adatok importálása az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c7d54-115">[Field mappings](search-indexer-field-mappings.md), which allow you toocustomize how indexers import data into Azure Search.</span></span>
* <span data-ttu-id="c7d54-116">ETag-EK, amelyek lehetővé teszik egy feldolgozási szálbiztos módon tooupdate hello definíciók indexek, az indexelők és az adatforrások.</span><span class="sxs-lookup"><span data-stu-id="c7d54-116">ETags, which allow you tooupdate hello definitions of indexes, indexers, and data sources in a concurrency-safe manner.</span></span> 

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a><span data-ttu-id="c7d54-117">Lépéseket tooupgrade</span><span class="sxs-lookup"><span data-stu-id="c7d54-117">Steps tooupgrade</span></span>
<span data-ttu-id="c7d54-118">Ha 2015-02-28 verzióról frissít, valószínűleg nem kell toomake bármely módosítások tooyour kód toochange hello verziószáma eltérő.</span><span class="sxs-lookup"><span data-stu-id="c7d54-118">If you are upgrading from version 2015-02-28, you probably won't have toomake any changes tooyour code, other than toochange hello version number.</span></span> <span data-ttu-id="c7d54-119">hello csak helyzetekben, ahol szükség lehet a toochange kódot, amikor:</span><span class="sxs-lookup"><span data-stu-id="c7d54-119">hello only situations in which you may need toochange code are when:</span></span>

* <span data-ttu-id="c7d54-120">A kód sikertelen lesz, amikor egy API-válaszból ismeretlen tulajdonságok vissza.</span><span class="sxs-lookup"><span data-stu-id="c7d54-120">Your code fails when unrecognized properties are returned in an API response.</span></span> <span data-ttu-id="c7d54-121">Alapértelmezés szerint az alkalmazás figyelmen kívül hagyja-tulajdonságok nem értelmezi.</span><span class="sxs-lookup"><span data-stu-id="c7d54-121">By default your application should ignore properties that it does not understand.</span></span>
* <span data-ttu-id="c7d54-122">A kód API-kérelmek tartósan fennáll, és megpróbál tooresend őket toohello új API-verzió.</span><span class="sxs-lookup"><span data-stu-id="c7d54-122">Your code persists API requests and tries tooresend them toohello new API version.</span></span> <span data-ttu-id="c7d54-123">Például ez előfordulhat, hogy fordulhat elő, ha az alkalmazás továbbra is fennáll, hello Search API által visszaadott folytatási jogkivonatok (további információkért tekintse meg `@search.nextPageParameters` a hello [keresési API-referencia](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span><span class="sxs-lookup"><span data-stu-id="c7d54-123">For example, this might happen if your application persists continuation tokens returned from hello Search API (for more information, look for `@search.nextPageParameters` in hello [Search API Reference](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).</span></span>

<span data-ttu-id="c7d54-124">Ha bármelyik ilyen helyzetekben tooyou, majd szükség lehet toochange a kód ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="c7d54-124">If either of these situations apply tooyou, then you may need toochange your code accordingly.</span></span> <span data-ttu-id="c7d54-125">Ellenkező esetben nem végez módosítást kell szükséges kivéve, ha azt szeretné, hogy hello segítségével toostart [új szolgáltatások](#WhatsNew) verzió 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="c7d54-125">Otherwise, no changes should be necessary unless you want toostart using hello [new features](#WhatsNew) of version 2016-09-01.</span></span>

<span data-ttu-id="c7d54-126">Ha verzió 2015-02-28-Preview frissít, a fenti hello is vonatkozik, de is figyelembe kell venni, hogy bizonyos előzetes verziójú funkciók nem érhetők el verziójában 2016-09-01:</span><span class="sxs-lookup"><span data-stu-id="c7d54-126">If you are upgrading from version 2015-02-28-Preview, hello above also applies, but you must also be aware that some preview features are not available in version 2016-09-01:</span></span>

* <span data-ttu-id="c7d54-127">Az Azure Blob Storage indexelő támogatása CSV-fájlok és a JSON-tömbök tartalmazó BLOB.</span><span class="sxs-lookup"><span data-stu-id="c7d54-127">Azure Blob Storage indexer support for CSV files and blobs containing JSON arrays.</span></span>
* <span data-ttu-id="c7d54-128">Szinonimák</span><span class="sxs-lookup"><span data-stu-id="c7d54-128">Synonyms</span></span>
* <span data-ttu-id="c7d54-129">"Több a" lekérdezések</span><span class="sxs-lookup"><span data-stu-id="c7d54-129">"More like this" queries</span></span>

<span data-ttu-id="c7d54-130">Ha a kódot használja ezeket a szolgáltatásokat, nem fogja az használatának egyik eltávolítása nélkül too2016-09-01 képes tooupgrade.</span><span class="sxs-lookup"><span data-stu-id="c7d54-130">If your code uses these features, you will not be able tooupgrade too2016-09-01 without removing your usage of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7d54-131">Adjon ne feledje, az előzetes API-k tesztelésében és értékelésében készült, és nem használható üzemi környezetben.</span><span class="sxs-lookup"><span data-stu-id="c7d54-131">Please remember, preview APIs are intended for testing and evaluation, and should not be used in production environments.</span></span>
> 
> 

## <a name="conclusion"></a><span data-ttu-id="c7d54-132">Összegzés</span><span class="sxs-lookup"><span data-stu-id="c7d54-132">Conclusion</span></span>
<span data-ttu-id="c7d54-133">Ha további részleteket a hello Azure Search szolgáltatás REST API használatával van szüksége, tekintse meg a nemrég frissített hello [API-referencia](https://msdn.microsoft.com/library/azure/dn798935.aspx) az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="c7d54-133">If you need more details on using hello Azure Search Service REST API, see hello recently updated [API Reference](https://msdn.microsoft.com/library/azure/dn798935.aspx) on MSDN.</span></span>

<span data-ttu-id="c7d54-134">Azure Search szívesen fogadjuk a visszajelzéseket.</span><span class="sxs-lookup"><span data-stu-id="c7d54-134">We welcome your feedback on Azure Search.</span></span> <span data-ttu-id="c7d54-135">Ha problémákat tapasztal, érzi, hogy szabad tooask nekünk a Súgó a hello [Azure Search MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) vagy [StackOverflow](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c7d54-135">If you encounter problems, feel free tooask us for help on hello [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) or [StackOverflow](http://stackoverflow.com/).</span></span> <span data-ttu-id="c7d54-136">Ha most kérni a kérdés kapcsolatos Azure keresése StackOverflow, győződjön meg arról, hogy tootag együtt `azure-search`.</span><span class="sxs-lookup"><span data-stu-id="c7d54-136">If you're asking a question about Azure Search on StackOverflow, make sure tootag it with `azure-search`.</span></span>

<span data-ttu-id="c7d54-137">Köszönjük, hogy az Azure Search használatával!</span><span class="sxs-lookup"><span data-stu-id="c7d54-137">Thank you for using Azure Search!</span></span>

