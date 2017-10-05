---
title: "Egy Azure Cosmos DB fiók az Azure-portálon kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti az Azure portál Azure Cosmos DB fiókját. Útmutató az Azure portál használatával megtekintése, másolása, törlés és hozzáférési fiókok található."
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
ms.openlocfilehash: a0c6ec8d490e1adacc96758971ab91d8eaeab45c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a><span data-ttu-id="eb3d0-105">Egy Azure Cosmos DB fiók kezelése</span><span class="sxs-lookup"><span data-stu-id="eb3d0-105">How to manage an Azure Cosmos DB account</span></span>
<span data-ttu-id="eb3d0-106">Ismerje meg, globális konzisztenciahiba, kulcsok dolgozni, és az Azure portálon Azure Cosmos DB-fiók törlése.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-106">Learn how to set global consistency, work with keys, and delete an Azure Cosmos DB account in the Azure portal.</span></span>

## <span data-ttu-id="eb3d0-107"><a id="consistency"></a>Azure Cosmos DB konzisztencia beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="eb3d0-107"><a id="consistency"></a>Manage Azure Cosmos DB consistency settings</span></span>
<span data-ttu-id="eb3d0-108">A jobb oldali konzisztenciaszint kiválasztása attól függ, hogy az alkalmazás szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-108">Selecting the right consistency level depends on the semantics of your application.</span></span> <span data-ttu-id="eb3d0-109">Meg kell Ismerkedjen meg a rendelkezésre álló konzisztenciaszintek az Azure Cosmos Adatbázisba olvasásával [konzisztenciaszintek használatával a rendelkezésre állás és teljesítmény az Azure Cosmos DB maximalizálása érdekében][consistency].</span><span class="sxs-lookup"><span data-stu-id="eb3d0-109">You should familiarize yourself with the available consistency levels in Azure Cosmos DB by reading [Using consistency levels to maximize availability and performance in Azure Cosmos DB][consistency].</span></span> <span data-ttu-id="eb3d0-110">Azure Cosmos-adatbázis konzisztencia, a rendelkezésre állási és a teljesítmény garanciák, az adatbázis fiókjához tartozó minden konzisztencia szintet biztosít.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-110">Azure Cosmos DB provides consistency, availability, and performance guarantees, at every consistency level available for your database account.</span></span> <span data-ttu-id="eb3d0-111">Az adatbázis-fiók beállítása az erős konzisztencia szintű kell lennie az adatok egyetlen Azure régióra zárt és globálisan nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-111">Configuring your database account with a consistency level of Strong requires that your data is confined to a single Azure region and not globally available.</span></span> <span data-ttu-id="eb3d0-112">Másrészről, a laza konzisztenciaszintek - a kötött elavulási, munkamenet és végleges engedélyezése, hogy rendelje hozzá Azure-régiók tetszőleges számú adatbázis fiókját.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-112">On the other hand, the relaxed consistency levels - bounded staleness, session or eventual enable you to associate any number of Azure regions with your database account.</span></span> <span data-ttu-id="eb3d0-113">Az alábbi egyszerű lépéseket mutatja be az adatbázis fiókjához tartozó alapértelmezett konzisztencia szintet.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-113">The following simple steps show you how to select the default consistency level for your database account.</span></span> 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a><span data-ttu-id="eb3d0-114">Az alapértelmezett konzisztencia Azure Cosmos DB fiók megadása</span><span class="sxs-lookup"><span data-stu-id="eb3d0-114">To specify the default consistency for an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="eb3d0-115">Az a [Azure-portálon](https://portal.azure.com/), Azure Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-115">In the [Azure portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="eb3d0-116">A fiók paneljén kattintson **alapértelmezett konzisztencia**.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-116">In the account blade, click **Default consistency**.</span></span>
3. <span data-ttu-id="eb3d0-117">Az a **alapértelmezett konzisztencia** panelen válassza ki az új konzisztenciaszint, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-117">In the **Default Consistency** blade, select the new consistency level and click **Save**.</span></span>
    <span data-ttu-id="eb3d0-118">![Alapértelmezett konzisztencia munkamenet][5]</span><span class="sxs-lookup"><span data-stu-id="eb3d0-118">![Default consistency session][5]</span></span>

## <span data-ttu-id="eb3d0-119"><a id="keys"></a>Megtekintése, másolása és tárelérési kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="eb3d0-119"><a id="keys"></a>View, copy, and regenerate access keys</span></span>
<span data-ttu-id="eb3d0-120">Egy Azure Cosmos DB fiók létrehozásakor a szolgáltatás két fő kulcsot, amely az Azure Cosmos DB fiók elérésekor hitelesítéshez használt állít elő.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-120">When you create an Azure Cosmos DB account, the service generates two master access keys that can be used for authentication when the Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="eb3d0-121">Két tárelérési kulcsok megadásával Azure Cosmos DB teszi Azure Cosmos DB fiókjába megszakítás nélkül a kulcsok újragenerálását.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-121">By providing two access keys, Azure Cosmos DB enables you to regenerate the keys with no interruption to your Azure Cosmos DB account.</span></span> 

<span data-ttu-id="eb3d0-122">Az a [Azure-portálon](https://portal.azure.com/), hozzáférés a **kulcsok** erőforrás-menüjéből panelen a **Azure Cosmos DB fiók** megtekintése, másolása és újragenerálása a tárelérési kulcsok, amelyek használt panel a Azure Cosmos DB-fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-122">In the [Azure portal](https://portal.azure.com/), access the **Keys** blade from the resource menu on the **Azure Cosmos DB account** blade to view, copy, and regenerate the access keys that are used to access your Azure Cosmos DB account.</span></span>

![Az Azure portál képernyőképe, a kulcsok panelen](./media/manage-account/keys.png)

> [!NOTE]
> <span data-ttu-id="eb3d0-124">A **kulcsok** panel is tartalmaz, amelyek segítségével a fiókjához elsődleges és másodlagos kapcsolati karakterláncok a [adatáttelepítési eszköz](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="eb3d0-124">The **Keys** blade also includes primary and secondary connection strings that can be used to connect to your account from the [Data Migration Tool](import-data.md).</span></span>
> 
> 

<span data-ttu-id="eb3d0-125">Csak olvasható kulcsokat is elérhetők a panel.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-125">Read-only keys are also available on this blade.</span></span> <span data-ttu-id="eb3d0-126">Olvasás és az olvasási műveletek kicsit hoz létre, törölhet, illetve cserél nem lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-126">Reads and queries are read-only operations, while creates, deletes, and replaces are not.</span></span>

### <a name="copy-an-access-key-in-the-azure-portal"></a><span data-ttu-id="eb3d0-127">Az elérési kulcs másolása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="eb3d0-127">Copy an access key in the Azure Portal</span></span>
<span data-ttu-id="eb3d0-128">Az a **kulcsok** panelen kattintson a **másolási** kíván másolni a kulcs jobbra látható gombra.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-128">On the **Keys** blade, click the **Copy** button to the right of the key you wish to copy.</span></span>

![Megtekintése és másolása egy hozzáférési kulcsot az Azure portálon, a kulcsok panelen](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a><span data-ttu-id="eb3d0-130">Tárelérési kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="eb3d0-130">Regenerate access keys</span></span>
<span data-ttu-id="eb3d0-131">Módosítsa a tárelérési kulcsok rendszeres időközönként a Azure Cosmos DB fiókját biztonságosabbá a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-131">You should change the access keys to your Azure Cosmos DB account periodically to help keep your connections more secure.</span></span> <span data-ttu-id="eb3d0-132">Engedélyezi, hogy az egyik kulcs, amíg a többi hozzáférési kulcs újragenerálása Azure Cosmos DB fiókhoz kapcsolatok karbantartása két tárelérési kulcsok vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-132">Two access keys are assigned to enable you to maintain connections to the Azure Cosmos DB account using one access key while you regenerate the other access key.</span></span>

> [!WARNING]
> <span data-ttu-id="eb3d0-133">A tárelérési kulcsok újragenerálása hatással van az aktuális kulcs függő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-133">Regenerating your access keys affects any applications that are dependent on the current key.</span></span> <span data-ttu-id="eb3d0-134">A Azure Cosmos DB-fiók eléréséhez a hozzáférési kulcsot használó összes ügyfelet frissíteni kell az új kulcs használatához.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-134">All clients that use the access key to access the Azure Cosmos DB account must be updated to use the new key.</span></span>
> 
> 

<span data-ttu-id="eb3d0-135">Ha alkalmazások vagy az Azure Cosmos DB fiókkal felhőszolgáltatások, elvesznek a kapcsolatok Ha újragenerálja a kulcsokat, kivéve, ha rotálja a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-135">If you have applications or cloud services using the Azure Cosmos DB account, you will lose the connections if you regenerate keys, unless you roll your keys.</span></span> <span data-ttu-id="eb3d0-136">A működés közbeni a kulcsok részt vevő folyamat lépései.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-136">The following steps outline the process involved in rolling your keys.</span></span>

1. <span data-ttu-id="eb3d0-137">Frissítse az alkalmazás kódjában hivatkozhasson rá az Azure Cosmos DB fiók másodlagos elérési kulcsát a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-137">Update the access key in your application code to reference the secondary access key of the Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="eb3d0-138">Újragenerálja az elsődleges elérési kulcsot az Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-138">Regenerate the primary access key for your Azure Cosmos DB account.</span></span> <span data-ttu-id="eb3d0-139">Az a [Azure Portal](https://portal.azure.com/), Azure Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-139">In the [Azure Portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
3. <span data-ttu-id="eb3d0-140">Az a **Azure Cosmos DB fiók** panelen kattintson a **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-140">In the **Azure Cosmos DB Account** blade, click **Keys**.</span></span>
4. <span data-ttu-id="eb3d0-141">Az a **kulcsok** panelen kattintson az újbóli létrehozás gombra, majd **Ok** annak ellenőrzéséhez, hogy szeretné-e hozzon létre egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-141">On the **Keys** blade, click the regenerate button, then click **Ok** to confirm that you want to generate a new key.</span></span>
    <span data-ttu-id="eb3d0-142">![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-keys.png)</span><span class="sxs-lookup"><span data-stu-id="eb3d0-142">![Regenerate access keys](./media/manage-account/regenerate-keys.png)</span></span>
5. <span data-ttu-id="eb3d0-143">Miután ellenőrizte, hogy az új kulcs használható (körülbelül 5 perc után újragenerálása), frissítse az alkalmazás kódjában hivatkozhasson rá az új elsődleges elérési kulcsát a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-143">Once you have verified that the new key is available for use (approximately 5 minutes after regeneration), update the access key in your application code to reference the new primary access key.</span></span>
6. <span data-ttu-id="eb3d0-144">Generálja újra a másodlagos elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-144">Regenerate the secondary access key.</span></span>
   
    ![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> <span data-ttu-id="eb3d0-146">Mielőtt egy újonnan létrehozott kulcs segítségével fér hozzá a Azure Cosmos DB fiókjához több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-146">It can take several minutes before a newly generated key can be used to access your Azure Cosmos DB account.</span></span>
> 
> 

## <a name="get-the--connection-string"></a><span data-ttu-id="eb3d0-147">A kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="eb3d0-147">Get the  connection string</span></span>
<span data-ttu-id="eb3d0-148">A kapcsolati karakterlánc lekéréséhez, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eb3d0-148">To retrieve your connection string, do the following:</span></span> 

1. <span data-ttu-id="eb3d0-149">Az a [Azure-portálon](https://portal.azure.com), Azure Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-149">In the [Azure portal](https://portal.azure.com), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="eb3d0-150">Az erőforrás menüjében kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-150">In the resource menu, click **Keys**.</span></span>
3. <span data-ttu-id="eb3d0-151">Kattintson a **másolási** megjelenítő gombra a **elsődleges kapcsolódási karakterlánc** vagy **másodlagos kapcsolati karakterlánc** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-151">Click the **Copy** button next to the **Primary Connection String** or **Secondary Connection String** box.</span></span> 

<span data-ttu-id="eb3d0-152">A kapcsolati karakterláncot a rendszer használata esetén a [Azure Cosmos DB adatbázis áttelepítési eszköz](import-data.md), az adatbázisnév hozzáfűzése a kapcsolati karakterlánc végén.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-152">If you are using the connection string in the [Azure Cosmos DB Database Migration Tool](import-data.md), append the database name to the end of the connection string.</span></span> <span data-ttu-id="eb3d0-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span></span>

## <span data-ttu-id="eb3d0-154"><a id="delete"></a>Azure Cosmos DB-fiók törlése</span><span class="sxs-lookup"><span data-stu-id="eb3d0-154"><a id="delete"></a> Delete an Azure Cosmos DB account</span></span>
<span data-ttu-id="eb3d0-155">Egy Azure Cosmos DB fiók már nem használ az Azure portálról eltávolításához kattintson a jobb gombbal a fiók nevét, majd kattintson **fiók törlése**.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-155">To remove an Azure Cosmos DB account from the Azure Portal that you are no longer using, right-click the account name, and click **Delete account**.</span></span>

![Az Azure portál Azure Cosmos DB fiók törlése](./media/manage-account/deleteaccount.png)

1. <span data-ttu-id="eb3d0-157">Az a [Azure-portálon](https://portal.azure.com/), a törölni kívánt Azure Cosmos DB fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-157">In the [Azure portal](https://portal.azure.com/), access the Azure Cosmos DB account you wish to delete.</span></span>
2. <span data-ttu-id="eb3d0-158">Az a **Azure Cosmos DB fiók** panelen kattintson a jobb gombbal a fiókot, és kattintson a **fiók törlése**.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-158">On the **Azure Cosmos DB account** blade, right-click the account, and then click **Delete Account**.</span></span> 
3. <span data-ttu-id="eb3d0-159">Az eredményül kapott megerősítő panelen írja be a Azure Cosmos DB nevét annak megerősítéséhez, hogy a fiók törlése.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-159">On the resulting confirmation blade, type the Azure Cosmos DB account name to confirm that you want to delete the account.</span></span>
4. <span data-ttu-id="eb3d0-160">Kattintson a **törlése** gombra.</span><span class="sxs-lookup"><span data-stu-id="eb3d0-160">Click the **Delete** button.</span></span>

![Az Azure portál Azure Cosmos DB fiók törlése](./media/manage-account/delete-account-confirm.png)

## <span data-ttu-id="eb3d0-162"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb3d0-162"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="eb3d0-163">Megtudhatja, hogyan [Ismerkedés az Azure Cosmos DB fiókja](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span><span class="sxs-lookup"><span data-stu-id="eb3d0-163">Learn how to [get started with your Azure Cosmos DB account](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span></span>

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
