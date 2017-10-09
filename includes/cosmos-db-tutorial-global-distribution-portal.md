
Azure Cosmos DB globális terjesztési az Azure-ban videó péntek Scott Hanselman és egyszerű mérnöki Manager Karthik Raman olvashat.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Hogyan globális adatbázis-replikációval kapcsolatos további információk a Cosmos DB működik, a következő témakörben: [adatok globálisan Cosmos DB terjesztése](../articles/documentdb/documentdb-distribute-data-globally.md).

## <a id="addregion"></a>Vegye fel a globális adatbázis területek hello Azure portál használatával
Az Azure Cosmos DB érhető el az összes [Azure-régiók] [ azureregions] világszerte. Miután kijelölte hello alapértelmezett konzisztenciaszint adatbázis fiókja, (attól függően, hogy a választott alapértelmezett konzisztencia szintre, valamint globális terjesztési igényeinek) egy vagy több régióban lehet társítani.

1. A hello [Azure-portálon](https://portal.azure.com/), a hello bal oldali sávon, kattintson a **Azure Cosmos DB**.
2. A hello **Azure Cosmos DB** panelen, jelölje be hello adatbázis fiók toomodify.
3. Hello-fiók panelen kattintson a **replikálja az adatokat globális** hello menüből.
4. A hello **replikálja az adatokat globális** panelen válassza ki a hello régiók tooadd vagy hello ábrázolási területek kattintva távolítsa el, és kattintson **mentése**. Hiba egy költség tooadding régiók, lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/documentdb/) vagy hello [globálisan a DocumentDB adatok terjesztése](../articles/documentdb/documentdb-distribute-data-globally.md) cikkében találja.
   
    ![Kattintson a hello térkép tooadd hello régiók, vagy távolítsa el őket][1]
    
Miután hozzáadta a második régió, hello **kézi feladatátvételt** beállítás engedélyezve van a hello **replikálja az adatokat globális** hello portál panel. Ez a beállítás tootest hello feladatátvételi folyamat használja, vagy hello elsődleges írási régió módosítása. Miután hozzáadta a harmadik régió, hello **feladatátvételi prioritások** beállítás engedélyezve van a hello azonos panelen, így módosíthatja a hello feladatátvételi sorrendnek olvasása.  

### <a name="selecting-global-database-regions"></a>Globális adatbázis területek kiválasztása
Két vagy több régió konfigurálása két gyakori forgatókönyvei van:

1. Alacsony késésű hozzáférést toodata tooend felhasználók függetlenül attól, hol találhatók körül hello földgömb továbbítása
2. Üzleti folytonossági és vészhelyreállítási (BCDR) regionális rugalmassági hozzáadása

A szállító kis késleltetésű tooend-felhasználók számára ajánlott toodeploy mindkét hello alkalmazás, és adja hozzá az Azure Cosmos DB hello régiókban toowhere hello alkalmazás felhasználóinak megegyező találhatók.

A BCDR, ajánlott a hello régió párok hello leírtak alapján tooadd régiók [üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure-régiókat párosítva] [ bcdr] cikk.

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
