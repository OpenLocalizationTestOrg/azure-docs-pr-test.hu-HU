
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
<span data-ttu-id="fad9b-101">Költségek csökkentése érdekében szüneteltetése, és folytassa a számítási erőforrások igény.</span><span class="sxs-lookup"><span data-stu-id="fad9b-101">To save costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="fad9b-102">Például nem fogja használni az adatbázis éjszaka és hétvégén, akkor is ilyen alkalmakkor szünetelteti, és folytatásához azt a nap folyamán.</span><span class="sxs-lookup"><span data-stu-id="fad9b-102">For example, if you won't be using the database during the night and on weekends, you can pause it during those times, and resume it during the day.</span></span> <span data-ttu-id="fad9b-103">Ön nem számlázni dwu-k, amíg az adatbázis fel van függesztve.</span><span class="sxs-lookup"><span data-stu-id="fad9b-103">You won't be charged for DWUs while the database is paused.</span></span>

<span data-ttu-id="fad9b-104">Ha az egérmutatót egy adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="fad9b-104">When you pause a database:</span></span>

* <span data-ttu-id="fad9b-105">Számítási és memória-erőforrások a rendszer visszairányítja az adatközpontban lévő rendelkezésre álló erőforrások készlete</span><span class="sxs-lookup"><span data-stu-id="fad9b-105">Compute and memory resources are returned to the pool of available resources in the data center</span></span>
* <span data-ttu-id="fad9b-106">DWU költségek a szünet időtartama nulla.</span><span class="sxs-lookup"><span data-stu-id="fad9b-106">DWU costs are zero for the duration of the pause.</span></span>
* <span data-ttu-id="fad9b-107">Ez nem befolyásolja az adatok tárolási és az adatok érintetlen marad.</span><span class="sxs-lookup"><span data-stu-id="fad9b-107">Data storage is not affected and your data stays intact.</span></span> 
* <span data-ttu-id="fad9b-108">Az SQL Data Warehouse visszavonja az összes futó vagy aszinkron művelet.</span><span class="sxs-lookup"><span data-stu-id="fad9b-108">SQL Data Warehouse cancels all running or queued operations.</span></span>

