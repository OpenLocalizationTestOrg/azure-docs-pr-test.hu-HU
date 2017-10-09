
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, hello following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
<span data-ttu-id="7671f-101">toosave költségek, is szüneteltetése és folytatása számítási erőforrások igény.</span><span class="sxs-lookup"><span data-stu-id="7671f-101">toosave costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="7671f-102">Például ha nem használ hello adatbázis hello éjszakai során, és a hétvégekre, után ilyen alkalmakkor szünetelteti, is fogják folytatni azt hello nap alatt.</span><span class="sxs-lookup"><span data-stu-id="7671f-102">For example, if you won't be using hello database during hello night and on weekends, you can pause it during those times, and resume it during hello day.</span></span> <span data-ttu-id="7671f-103">Ön nem számlázni dwu-k közben hello adatbázis fel van függesztve.</span><span class="sxs-lookup"><span data-stu-id="7671f-103">You won't be charged for DWUs while hello database is paused.</span></span>

<span data-ttu-id="7671f-104">Ha az egérmutatót egy adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="7671f-104">When you pause a database:</span></span>

* <span data-ttu-id="7671f-105">Számítási és memória-erőforrások toohello készlet rendelkezésre álló erőforrások visszaadott hello adatközpont</span><span class="sxs-lookup"><span data-stu-id="7671f-105">Compute and memory resources are returned toohello pool of available resources in hello data center</span></span>
* <span data-ttu-id="7671f-106">DWU költségek olyan hello szünet időtartama hello nulla.</span><span class="sxs-lookup"><span data-stu-id="7671f-106">DWU costs are zero for hello duration of hello pause.</span></span>
* <span data-ttu-id="7671f-107">Ez nem befolyásolja az adatok tárolási és az adatok érintetlen marad.</span><span class="sxs-lookup"><span data-stu-id="7671f-107">Data storage is not affected and your data stays intact.</span></span> 
* <span data-ttu-id="7671f-108">Az SQL Data Warehouse visszavonja az összes futó vagy aszinkron művelet.</span><span class="sxs-lookup"><span data-stu-id="7671f-108">SQL Data Warehouse cancels all running or queued operations.</span></span>

