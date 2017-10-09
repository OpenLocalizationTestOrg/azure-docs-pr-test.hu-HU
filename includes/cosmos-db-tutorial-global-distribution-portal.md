
<span data-ttu-id="03c5b-101">Azure Cosmos DB globális terjesztési az Azure-ban videó péntek Scott Hanselman és egyszerű mérnöki Manager Karthik Raman olvashat.</span><span class="sxs-lookup"><span data-stu-id="03c5b-101">You can learn about Azure Cosmos DB global distribution in this Azure Friday video with Scott Hanselman and Principal Engineering Manager Karthik Raman.</span></span>

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

<span data-ttu-id="03c5b-102">Hogyan globális adatbázis-replikációval kapcsolatos további információk a Cosmos DB működik, a következő témakörben: [adatok globálisan Cosmos DB terjesztése](../articles/documentdb/documentdb-distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="03c5b-102">For more information about how global database replication works in Cosmos DB, see [Distribute data globally with Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span></span>

## <span data-ttu-id="03c5b-103"><a id="addregion"></a>Vegye fel a globális adatbázis területek hello Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="03c5b-103"><a id="addregion"></a>Add global database regions using hello Azure Portal</span></span>
<span data-ttu-id="03c5b-104">Az Azure Cosmos DB érhető el az összes [Azure-régiók] [ azureregions] világszerte.</span><span class="sxs-lookup"><span data-stu-id="03c5b-104">Azure Cosmos DB is available in all [Azure regions][azureregions] world-wide.</span></span> <span data-ttu-id="03c5b-105">Miután kijelölte hello alapértelmezett konzisztenciaszint adatbázis fiókja, (attól függően, hogy a választott alapértelmezett konzisztencia szintre, valamint globális terjesztési igényeinek) egy vagy több régióban lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="03c5b-105">After selecting hello default consistency level for your database account, you can associate one or more regions (depending on your choice of default consistency level and global distribution needs).</span></span>

1. <span data-ttu-id="03c5b-106">A hello [Azure-portálon](https://portal.azure.com/), a hello bal oldali sávon, kattintson a **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="03c5b-106">In hello [Azure portal](https://portal.azure.com/), in hello left bar, click **Azure Cosmos DB**.</span></span>
2. <span data-ttu-id="03c5b-107">A hello **Azure Cosmos DB** panelen, jelölje be hello adatbázis fiók toomodify.</span><span class="sxs-lookup"><span data-stu-id="03c5b-107">In hello **Azure Cosmos DB** blade, select hello database account toomodify.</span></span>
3. <span data-ttu-id="03c5b-108">Hello-fiók panelen kattintson a **replikálja az adatokat globális** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="03c5b-108">In hello account blade, click **Replicate data globally** from hello menu.</span></span>
4. <span data-ttu-id="03c5b-109">A hello **replikálja az adatokat globális** panelen válassza ki a hello régiók tooadd vagy hello ábrázolási területek kattintva távolítsa el, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="03c5b-109">In hello **Replicate data globally** blade, select hello regions tooadd or remove by clicking regions in hello map, and then click **Save**.</span></span> <span data-ttu-id="03c5b-110">Hiba egy költség tooadding régiók, lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/documentdb/) vagy hello [globálisan a DocumentDB adatok terjesztése](../articles/documentdb/documentdb-distribute-data-globally.md) cikkében találja.</span><span class="sxs-lookup"><span data-stu-id="03c5b-110">There is a cost tooadding regions, see hello [pricing page](https://azure.microsoft.com/pricing/details/documentdb/) or hello [Distribute data globally with DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) article for more information.</span></span>
   
    ![Kattintson a hello térkép tooadd hello régiók, vagy távolítsa el őket][1]
    
<span data-ttu-id="03c5b-112">Miután hozzáadta a második régió, hello **kézi feladatátvételt** beállítás engedélyezve van a hello **replikálja az adatokat globális** hello portál panel.</span><span class="sxs-lookup"><span data-stu-id="03c5b-112">Once you add a second region, hello **Manual Failover** option is enabled on hello **Replicate data globally** blade in hello portal.</span></span> <span data-ttu-id="03c5b-113">Ez a beállítás tootest hello feladatátvételi folyamat használja, vagy hello elsődleges írási régió módosítása.</span><span class="sxs-lookup"><span data-stu-id="03c5b-113">You can use this option tootest hello failover process or change hello primary write region.</span></span> <span data-ttu-id="03c5b-114">Miután hozzáadta a harmadik régió, hello **feladatátvételi prioritások** beállítás engedélyezve van a hello azonos panelen, így módosíthatja a hello feladatátvételi sorrendnek olvasása.</span><span class="sxs-lookup"><span data-stu-id="03c5b-114">Once you add a third region, hello **Failover Priorities** option is enabled on hello same blade so that you can change hello failover order for reads.</span></span>  

### <a name="selecting-global-database-regions"></a><span data-ttu-id="03c5b-115">Globális adatbázis területek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="03c5b-115">Selecting global database regions</span></span>
<span data-ttu-id="03c5b-116">Két vagy több régió konfigurálása két gyakori forgatókönyvei van:</span><span class="sxs-lookup"><span data-stu-id="03c5b-116">There are two common scenarios for configuring two or more regions:</span></span>

1. <span data-ttu-id="03c5b-117">Alacsony késésű hozzáférést toodata tooend felhasználók függetlenül attól, hol találhatók körül hello földgömb továbbítása</span><span class="sxs-lookup"><span data-stu-id="03c5b-117">Delivering low-latency access toodata tooend users no matter where they are located around hello globe</span></span>
2. <span data-ttu-id="03c5b-118">Üzleti folytonossági és vészhelyreállítási (BCDR) regionális rugalmassági hozzáadása</span><span class="sxs-lookup"><span data-stu-id="03c5b-118">Adding regional resiliency for business continuity and disaster recovery (BCDR)</span></span>

<span data-ttu-id="03c5b-119">A szállító kis késleltetésű tooend-felhasználók számára ajánlott toodeploy mindkét hello alkalmazás, és adja hozzá az Azure Cosmos DB hello régiókban toowhere hello alkalmazás felhasználóinak megegyező találhatók.</span><span class="sxs-lookup"><span data-stu-id="03c5b-119">For delivering low-latency tooend-users, it is recommended toodeploy both hello application and add Azure Cosmos DB in hello regions thats correspond toowhere hello application's users are located.</span></span>

<span data-ttu-id="03c5b-120">A BCDR, ajánlott a hello régió párok hello leírtak alapján tooadd régiók [üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure-régiókat párosítva] [ bcdr] cikk.</span><span class="sxs-lookup"><span data-stu-id="03c5b-120">For BCDR, it is recommended tooadd regions based on hello region pairs described in hello [Business continuity and disaster recovery (BCDR): Azure Paired Regions][bcdr] article.</span></span>

<!--

## <a id="selectwriteregion"></a>Select hello write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive hello write (insert, upsert, replace, delete) requests. tooset hello active write region, do hello following  


1. In hello **Azure Cosmos DB** blade, select hello database account toomodify.
2. In hello account blade, click **Replicate data globally** from hello menu.
3. In hello **Replicate data globally** blade, click **Manual Failover** from hello top bar.
    ![Change hello write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region toobecome hello new write region, click hello checkbox tooconfirm triggering a failover, and click OK
    ![Change hello write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
