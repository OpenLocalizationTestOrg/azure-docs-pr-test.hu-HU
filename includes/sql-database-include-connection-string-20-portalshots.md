
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-hello-connection-string-from-hello-azure-portal"></a>Hello kapcsolati karakterlánc szerzi be hello Azure-portálon
Használjon hello [Azure-portálon](https://portal.azure.com/) tooobtain hello kapcsolati karakterlánc az ügyfél program toointeract az Azure SQL Database szükséges: 

1. Kattintson a **Tallózás** > **SQL-adatbázisok**.
2. Adja meg az adatbázis hello nevét hello szűrő szövegmezőbe közelében hello bal felső sarokban a hello **SQL-adatbázisok** panelen.
3. Kattintson az adatbázis hello sort.
4. Miután hello panelen megjelenik az adatbázis, a hello standard visual kényelmi célokat szolgál, kattintson vezérlők toocollapse hello panelen keresse meg és adatbázis-szűrés használt minimalizálása érdekében. 
   
    ![Szűrés tooisolate az adatbázis][10-FilterDatabase]
5. Az adatbázis hello paneljén kattintson **adatbázis-kapcsolati karakterláncok megjelenítése**.
6. Ha azt tervezi, toouse hello ADO.NET kapcsolattára, másolja feliratú hello karakterláncot **ADO**. 
   
    ![Hello ADO kapcsolati karakterlánc az adatbázis másolása][20-CopyAdoConnectionString]
7. Egy formátumban vagy egy másik a program Ügyfélkód hello kapcsolati karakterlánc adatokat beilleszteni.

További információkért lásd:<br/>[A kapcsolati karakterláncokat és konfigurációs fájlok](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
