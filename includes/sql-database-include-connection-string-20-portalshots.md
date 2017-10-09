
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-hello-connection-string-from-hello-azure-portal"></a><span data-ttu-id="cc92d-101">Hello kapcsolati karakterlánc szerzi be hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cc92d-101">Obtain hello connection string from hello Azure portal</span></span>
<span data-ttu-id="cc92d-102">Használjon hello [Azure-portálon](https://portal.azure.com/) tooobtain hello kapcsolati karakterlánc az ügyfél program toointeract az Azure SQL Database szükséges:</span><span class="sxs-lookup"><span data-stu-id="cc92d-102">Use hello [Azure portal](https://portal.azure.com/) tooobtain hello connection string necessary for your client program toointeract with Azure SQL Database:</span></span> 

1. <span data-ttu-id="cc92d-103">Kattintson a **Tallózás** > **SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="cc92d-103">Click **BROWSE** > **SQL databases**.</span></span>
2. <span data-ttu-id="cc92d-104">Adja meg az adatbázis hello nevét hello szűrő szövegmezőbe közelében hello bal felső sarokban a hello **SQL-adatbázisok** panelen.</span><span class="sxs-lookup"><span data-stu-id="cc92d-104">Enter hello name of your database into hello filter text box near hello upper-left of hello **SQL databases** blade.</span></span>
3. <span data-ttu-id="cc92d-105">Kattintson az adatbázis hello sort.</span><span class="sxs-lookup"><span data-stu-id="cc92d-105">Click hello row for your database.</span></span>
4. <span data-ttu-id="cc92d-106">Miután hello panelen megjelenik az adatbázis, a hello standard visual kényelmi célokat szolgál, kattintson vezérlők toocollapse hello panelen keresse meg és adatbázis-szűrés használt minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="cc92d-106">After hello blade appears for your database, for visual convenience you can click hello standard minimize controls toocollapse hello blades  you used for browsing and database filtering.</span></span> 
   
    ![Szűrés tooisolate az adatbázis][10-FilterDatabase]
5. <span data-ttu-id="cc92d-108">Az adatbázis hello paneljén kattintson **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="cc92d-108">On hello blade for your database, click **Show database connection strings**.</span></span>
6. <span data-ttu-id="cc92d-109">Ha azt tervezi, toouse hello ADO.NET kapcsolattára, másolja feliratú hello karakterláncot **ADO**.</span><span class="sxs-lookup"><span data-stu-id="cc92d-109">If you intend toouse hello ADO.NET connection library, copy hello string labeled **ADO**.</span></span> 
   
    ![Hello ADO kapcsolati karakterlánc az adatbázis másolása][20-CopyAdoConnectionString]
7. <span data-ttu-id="cc92d-111">Egy formátumban vagy egy másik a program Ügyfélkód hello kapcsolati karakterlánc adatokat beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="cc92d-111">In one format or another, paste hello connection string information into your client program code.</span></span>

<span data-ttu-id="cc92d-112">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="cc92d-112">For more information, see:</span></span><br/><span data-ttu-id="cc92d-113">[A kapcsolati karakterláncokat és konfigurációs fájlok](http://msdn.microsoft.com/library/ms254494.aspx).</span><span class="sxs-lookup"><span data-stu-id="cc92d-113">[Connection Strings and Configuration Files](http://msdn.microsoft.com/library/ms254494.aspx).</span></span>

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
