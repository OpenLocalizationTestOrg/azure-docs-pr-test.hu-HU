---
title: "hello Azure portálon keresztül Azure Cosmos DB fiók aaaManage |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure Cosmos DB fiók hello Azure portálon keresztül. Útmutató, hello Azure Portal tooview, másolás, törlés és hozzáférési fiókok használatával található."
keywords: "Azure-portálon, a documentdb, az azure, a Microsoft azure"
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: 77ad953cf558a519674be08ad913a12202f69496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-an-azure-cosmos-db-account"></a><span data-ttu-id="0f0e9-105">Hogyan toomanage Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="0f0e9-105">How toomanage an Azure Cosmos DB account</span></span>
<span data-ttu-id="0f0e9-106">Megtudhatja, hogyan tooset globális konzisztenciahiba, kulcsok használata, és az Azure-portálon hello Azure Cosmos DB-fiók törlése.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-106">Learn how tooset global consistency, work with keys, and delete an Azure Cosmos DB account in hello Azure portal.</span></span>

## <span data-ttu-id="0f0e9-107"><a id="consistency"></a>Azure Cosmos DB konzisztencia beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="0f0e9-107"><a id="consistency"></a>Manage Azure Cosmos DB consistency settings</span></span>
<span data-ttu-id="0f0e9-108">Hello jobb konzisztenciaszint kiválasztása attól függ, hogy az alkalmazás hello szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-108">Selecting hello right consistency level depends on hello semantics of your application.</span></span> <span data-ttu-id="0f0e9-109">Tanulmányozza át az hello elérhető konzisztenciaszintek az Azure Cosmos Adatbázisba olvasásával [toomaximize rendelkezésre állásának és teljesítményének a Azure Cosmos DB használatával konzisztencia szintek][consistency].</span><span class="sxs-lookup"><span data-stu-id="0f0e9-109">You should familiarize yourself with hello available consistency levels in Azure Cosmos DB by reading [Using consistency levels toomaximize availability and performance in Azure Cosmos DB][consistency].</span></span> <span data-ttu-id="0f0e9-110">Azure Cosmos-adatbázis konzisztencia, a rendelkezésre állási és a teljesítmény garanciák, az adatbázis fiókjához tartozó minden konzisztencia szintet biztosít.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-110">Azure Cosmos DB provides consistency, availability, and performance guarantees, at every consistency level available for your database account.</span></span> <span data-ttu-id="0f0e9-111">Az adatbázis-fiók beállítása az erős konzisztencia szintű futnia kell az adatok zárt tooa egyetlen Azure-régió, és globális nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-111">Configuring your database account with a consistency level of Strong requires that your data is confined tooa single Azure region and not globally available.</span></span> <span data-ttu-id="0f0e9-112">A ugyanakkor hello, laza konzisztenciaszintek - a kötött elavulási, munkamenet és végül mind engedélyezés hello meg tooassociate tetszőleges számú Azure-régiók adatbázis fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-112">On hello other hand, hello relaxed consistency levels - bounded staleness, session or eventual enable you tooassociate any number of Azure regions with your database account.</span></span> <span data-ttu-id="0f0e9-113">hello egyszerű lépések bemutatják, hogyan tooselect hello konzisztenciaszint alapértelmezett adatbázis-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-113">hello following simple steps show you how tooselect hello default consistency level for your database account.</span></span> 

### <a name="toospecify-hello-default-consistency-for-an-azure-cosmos-db-account"></a><span data-ttu-id="0f0e9-114">toospecify hello alapértelmezett konzisztencia Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="0f0e9-114">toospecify hello default consistency for an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="0f0e9-115">A hello [Azure-portálon](https://portal.azure.com/), Azure Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-115">In hello [Azure portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="0f0e9-116">Hello-fiók panelen kattintson a **alapértelmezett konzisztencia**.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-116">In hello account blade, click **Default consistency**.</span></span>
3. <span data-ttu-id="0f0e9-117">A hello **alapértelmezett konzisztencia** panelen, jelölje be hello új konzisztencia szinten, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-117">In hello **Default Consistency** blade, select hello new consistency level and click **Save**.</span></span>
    <span data-ttu-id="0f0e9-118">![Alapértelmezett konzisztencia munkamenet][5]</span><span class="sxs-lookup"><span data-stu-id="0f0e9-118">![Default consistency session][5]</span></span>

## <span data-ttu-id="0f0e9-119"><a id="keys"></a>Megtekintése, másolása és tárelérési kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="0f0e9-119"><a id="keys"></a>View, copy, and regenerate access keys</span></span>
<span data-ttu-id="0f0e9-120">Egy Azure Cosmos DB fiók létrehozásakor hello szolgáltatás hello Azure Cosmos DB fiók használatakor a hitelesítéshez használt két fő tárelérési kulcsokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-120">When you create an Azure Cosmos DB account, hello service generates two master access keys that can be used for authentication when hello Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="0f0e9-121">Két tárelérési kulcsok megadásával Azure Cosmos DB lehetővé teszi tooregenerate hello kulcsok nem megszakítás tooyour Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-121">By providing two access keys, Azure Cosmos DB enables you tooregenerate hello keys with no interruption tooyour Azure Cosmos DB account.</span></span> 

<span data-ttu-id="0f0e9-122">A hello [Azure-portálon](https://portal.azure.com/), hozzáférési hello **kulcsok** hello erőforrás menüjéből hello panel **Azure Cosmos DB fiók** panel tooview, másolása és újragenerálása hello elérési kulcsainak, olyan használt tooaccess Azure Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-122">In hello [Azure portal](https://portal.azure.com/), access hello **Keys** blade from hello resource menu on hello **Azure Cosmos DB account** blade tooview, copy, and regenerate hello access keys that are used tooaccess your Azure Cosmos DB account.</span></span>

![Az Azure portál képernyőképe, a kulcsok panelen](./media/manage-account/keys.png)

> [!NOTE]
> <span data-ttu-id="0f0e9-124">Hello **kulcsok** panel is tartalmaz elsődleges és másodlagos kapcsolati karakterláncok, amelyek lehetnek használt tooconnect tooyour hello a fiók [adatáttelepítési eszköz](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="0f0e9-124">hello **Keys** blade also includes primary and secondary connection strings that can be used tooconnect tooyour account from hello [Data Migration Tool](import-data.md).</span></span>
> 
> 

<span data-ttu-id="0f0e9-125">Csak olvasható kulcsokat is elérhetők a panel.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-125">Read-only keys are also available on this blade.</span></span> <span data-ttu-id="0f0e9-126">Olvasás és az olvasási műveletek kicsit hoz létre, törölhet, illetve cserél nem lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-126">Reads and queries are read-only operations, while creates, deletes, and replaces are not.</span></span>

### <a name="copy-an-access-key-in-hello-azure-portal"></a><span data-ttu-id="0f0e9-127">Az elérési kulcs másolása hello Azure portál</span><span class="sxs-lookup"><span data-stu-id="0f0e9-127">Copy an access key in hello Azure Portal</span></span>
<span data-ttu-id="0f0e9-128">A hello **kulcsok** panelen hello kattintson **másolási** toocopy gomb toohello sarkában hello kulcs kívánja.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-128">On hello **Keys** blade, click hello **Copy** button toohello right of hello key you wish toocopy.</span></span>

![Megtekintése és másolása hívóbetű hello Azure portálon, a kulcsok panelen](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a><span data-ttu-id="0f0e9-130">Tárelérési kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="0f0e9-130">Regenerate access keys</span></span>
<span data-ttu-id="0f0e9-131">Meg kell változtatni hello hozzáférési kulcsok tooyour Azure Cosmos DB fiók rendszeresen toohelp a kapcsolatok nagyobb biztonságban.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-131">You should change hello access keys tooyour Azure Cosmos DB account periodically toohelp keep your connections more secure.</span></span> <span data-ttu-id="0f0e9-132">Két tárelérési kulcsok rendelt tooenable meg toomaintain kapcsolatok toohello Azure Cosmos DB fiók az egyik kulcs, amíg a újragenerálja hello más hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-132">Two access keys are assigned tooenable you toomaintain connections toohello Azure Cosmos DB account using one access key while you regenerate hello other access key.</span></span>

> [!WARNING]
> <span data-ttu-id="0f0e9-133">A tárelérési kulcsok újragenerálása hatással van az aktuális kulcs hello függő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-133">Regenerating your access keys affects any applications that are dependent on hello current key.</span></span> <span data-ttu-id="0f0e9-134">Hello hozzáférési kulcs tooaccess hello Azure Cosmos DB fiókhoz használó összes ügyfelet frissített toouse hello új kulcsot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-134">All clients that use hello access key tooaccess hello Azure Cosmos DB account must be updated toouse hello new key.</span></span>
> 
> 

<span data-ttu-id="0f0e9-135">Ha alkalmazások vagy szolgáltatások hello Azure Cosmos DB fiók használatával, elvesznek hello kapcsolatok Ha újragenerálja a kulcsokat, kivéve, ha rotálja a kulcsokat felhő.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-135">If you have applications or cloud services using hello Azure Cosmos DB account, you will lose hello connections if you regenerate keys, unless you roll your keys.</span></span> <span data-ttu-id="0f0e9-136">hello lépései hello folyamat működés közbeni a kulcsok részt.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-136">hello following steps outline hello process involved in rolling your keys.</span></span>

1. <span data-ttu-id="0f0e9-137">Frissítés hello hozzáférési kulcs az alkalmazás kódja tooreference hello másodlagos elérési kulcsára hello Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-137">Update hello access key in your application code tooreference hello secondary access key of hello Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="0f0e9-138">Az Azure Cosmos DB fiók hello elsődleges elérési kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-138">Regenerate hello primary access key for your Azure Cosmos DB account.</span></span> <span data-ttu-id="0f0e9-139">A hello [Azure Portal](https://portal.azure.com/), Azure Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-139">In hello [Azure Portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
3. <span data-ttu-id="0f0e9-140">A hello **Azure Cosmos DB fiók** panelen kattintson a **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-140">In hello **Azure Cosmos DB Account** blade, click **Keys**.</span></span>
4. <span data-ttu-id="0f0e9-141">A hello **kulcsok** panelen hello újbóli létrehozás gombra kattintva, majd kattintson a **Ok** , amelyet az új kulcs toogenerate tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-141">On hello **Keys** blade, click hello regenerate button, then click **Ok** tooconfirm that you want toogenerate a new key.</span></span>
    <span data-ttu-id="0f0e9-142">![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-keys.png)</span><span class="sxs-lookup"><span data-stu-id="0f0e9-142">![Regenerate access keys](./media/manage-account/regenerate-keys.png)</span></span>
5. <span data-ttu-id="0f0e9-143">Miután ellenőrizte, hello új kulcs használható (körülbelül 5 perc után újragenerálása), frissíteni az hello elérési kulcsot a az alkalmazás kódja tooreference hello új elsődleges elérési kulcsát.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-143">Once you have verified that hello new key is available for use (approximately 5 minutes after regeneration), update hello access key in your application code tooreference hello new primary access key.</span></span>
6. <span data-ttu-id="0f0e9-144">Hello másodlagos elérési kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-144">Regenerate hello secondary access key.</span></span>
   
    ![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> <span data-ttu-id="0f0e9-146">Is igénybe vehet néhány percet egy újonnan létrehozott kulcs lehet használt tooaccess Azure Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-146">It can take several minutes before a newly generated key can be used tooaccess your Azure Cosmos DB account.</span></span>
> 
> 

## <a name="get-hello--connection-string"></a><span data-ttu-id="0f0e9-147">Hello kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="0f0e9-147">Get hello  connection string</span></span>
<span data-ttu-id="0f0e9-148">tooretrieve a kapcsolati karakterlánc, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="0f0e9-148">tooretrieve your connection string, do hello following:</span></span> 

1. <span data-ttu-id="0f0e9-149">A hello [Azure-portálon](https://portal.azure.com), Azure Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-149">In hello [Azure portal](https://portal.azure.com), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="0f0e9-150">A hello erőforrás kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-150">In hello resource menu, click **Keys**.</span></span>
3. <span data-ttu-id="0f0e9-151">Hello kattintson **másolási** gomb következő toohello **elsődleges kapcsolódási karakterlánc** vagy **másodlagos kapcsolati karakterlánc** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-151">Click hello **Copy** button next toohello **Primary Connection String** or **Secondary Connection String** box.</span></span> 

<span data-ttu-id="0f0e9-152">Hello használatakor hello kapcsolati karakterlánc [Azure Cosmos DB adatbázis áttelepítési eszköz](import-data.md), hozzáfűző hello adatbázis neve toohello vége hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-152">If you are using hello connection string in hello [Azure Cosmos DB Database Migration Tool](import-data.md), append hello database name toohello end of hello connection string.</span></span> <span data-ttu-id="0f0e9-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span></span>

## <span data-ttu-id="0f0e9-154"><a id="delete"></a>Azure Cosmos DB-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="0f0e9-154"><a id="delete"></a> Delete an Azure Cosmos DB account</span></span>
<span data-ttu-id="0f0e9-155">egy Azure Cosmos DB tooremove hello Azure portál, amely már nem használ, kattintson a jobb gombbal hello fiók nevét, a fiókot, és kattintson a **fiók törlése**.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-155">tooremove an Azure Cosmos DB account from hello Azure Portal that you are no longer using, right-click hello account name, and click **Delete account**.</span></span>

![Hogyan toodelete egy Azure Cosmos DB fiókként hello Azure portál](./media/manage-account/deleteaccount.png)

1. <span data-ttu-id="0f0e9-157">A hello [Azure-portálon](https://portal.azure.com/), toodelete kívánja hello Azure Cosmos DB fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-157">In hello [Azure portal](https://portal.azure.com/), access hello Azure Cosmos DB account you wish toodelete.</span></span>
2. <span data-ttu-id="0f0e9-158">A hello **Azure Cosmos DB fiók** panelen kattintson a jobb gombbal a hello fiókot, és kattintson a **fiók törlése**.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-158">On hello **Azure Cosmos DB account** blade, right-click hello account, and then click **Delete Account**.</span></span> 
3. <span data-ttu-id="0f0e9-159">Az eredményül kapott megerősítő panelen hello írja be a hello Azure Cosmos DB fiók neve tooconfirm, amelyet az toodelete hello fiók.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-159">On hello resulting confirmation blade, type hello Azure Cosmos DB account name tooconfirm that you want toodelete hello account.</span></span>
4. <span data-ttu-id="0f0e9-160">Kattintson a hello **törlése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0f0e9-160">Click hello **Delete** button.</span></span>

![Hogyan toodelete egy Azure Cosmos DB fiókként hello Azure portál](./media/manage-account/delete-account-confirm.png)

## <span data-ttu-id="0f0e9-162"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f0e9-162"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="0f0e9-163">Ismerje meg, hogyan túl[Ismerkedés az Azure Cosmos DB fiókja](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span><span class="sxs-lookup"><span data-stu-id="0f0e9-163">Learn how too[get started with your Azure Cosmos DB account](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span></span>

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
