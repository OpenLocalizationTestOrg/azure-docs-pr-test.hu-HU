---
title: SQL Server be Azure SQL Data Warehouse (SSIS) aaaLoad adatait |} Microsoft Docs
description: "Bemutatja, hogyan toocreate egy SQL Server Integration Services (SSIS) csomag toomove adatokat az adatok különböző forrásokból tooSQL Data warehouse-bA."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: bb28a08807a5b07832b85f2f074c2acf912c1dc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Adatok betöltése az SQL Server be Azure SQL Data Warehouse (SSIS)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Hozzon létre egy SQL Server Integration Services (SSIS) csomag tooload adatok SQL Server az Azure SQL Data Warehouse között. Opcionálisan átalakításának, átalakítási és nagybetűhasználatának hello adatokat, ahogy keresztülhalad hello SSIS-adatfolyam.

Az oktatóanyag során az alábbi lépéseket fogja végrehajtani:

* Az integrációs szolgáltatások új projekt létrehozása a Visual Studióban.
* Csatlakozás toodata móddal, többek között az SQL Server (mint a forrás-) és az SQL Data Warehouse (célként).
* Tervezze meg az SSIS-csomagot, amely hello forrásból származó adatokat tölt be hello cél.
* Futtassa a hello SSIS tooload hello adatait.

Ez az oktatóanyag az SQL Server hello adatforrásként. SQL Server sikerült fut, a helyszíni vagy egy Azure virtuális gépen.

## <a name="basic-concepts"></a>Alapfogalmak
hello csomag hello munkaegysége SSIS. Kapcsolódó csomagok projektek vannak csoportosítva. Projektek és a Tervező csomagok létrehozása a Visual Studio és az SQL Server Data Tools összetevővel. egy visual folyamat, amelyben Ön áthúzása összetevők hello eszközkészlet toohello tervezési felületéhez hello tervezési csatlakoztassa őket, és állítsa be tulajdonságaikat. Ha befejezte a csomag, igény szerint telepítheti azt tooSQL kiszolgáló átfogó kezelési, figyelési és biztonsági.

## <a name="options-for-loading-data-with-ssis"></a>Beállítások SSIS az adatok betöltése
SQL Server Integration Services (SSIS), amely számos lehetőséget biztosít csatlakozik, és az adatok betöltése az SQL Data Warehouse rugalmas eszközkészlet.

1. Használjon egy ADO NET cél tooconnect tooSQL Data warehouse-bA. Ez az oktatóanyag egy ADO NET cél használja, mert a hello legkevesebb konfigurációs beállításokat.
2. Az OLE DB cél tooconnect tooSQL adatraktár használja. Ez a beállítás által biztosított hello ADO NET cél némileg jobb teljesítményt.
3. Hello Azure Blob feltöltése feladat toostage hello adatokat használja az Azure Blob Storage tárolóban. Ezután használja az hello SSIS SQL végrehajtása a feladat toolaunch hello adatok betöltése az SQL Data Warehouse Polybase parancsfájlból. Ez a beállítás hello három lehetőség az itt felsorolt hello legjobb teljesítményt biztosít. tooget hello Azure Blob feltöltése feladat letöltése hello [Microsoft SQL Server 2016 Integration Services funkciócsomag Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]. toolearn Polybase kapcsolatos további információkért lásd: [PolyBase-útmutatóban][PolyBase Guide].

## <a name="before-you-start"></a>Előkészületek
az oktatóanyag teljesítéséhez toostep, lesz szüksége:

1. **Az SQL Server Integration Services (SSIS)**. SSIS az SQL Server, és próbaverziójáról vagy az SQL Server licenccel rendelkező verzió szükséges. SQL Server 2016 Preview próbaverzióját tooget lásd [SQL Server értékelések][SQL Server Evaluations].
2. **Visual Studio**. tooget ingyenes Visual Studio Community Edition hello című [Visual Studio Community][Visual Studio Community].
3. **SQL Server Data Tools for Visual Studio (SSDT)**. SQL Server Data Tools for Visual Studio tooget lásd [letöltése SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. **Az adatokat**. Ez az oktatóanyag tárolva az SQL Server hello AdventureWorks mintaadatbázishoz source adatok toobe SQL Data Warehouse-bA betöltött hello mintaadatokat használja. tooget hello AdventureWorks mintaadatbázishoz, lásd: [AdventureWorks 2014 mintaadatbázisokat][AdventureWorks 2014 Sample Databases].
5. **Egy SQL Data Warehouse-adatbázis és az engedélyek**. Ez az oktatóanyag tooa SQL Data Warehouse-példányhoz csatlakozik, és adatokat tölt be. Toohave engedélyek toocreate egy tábla és tooload adatokkal rendelkeznek.
6. **Egy tűzfalszabály**. Van toocreate tűzfalszabály SQL Data Warehouse hello IP-cím a helyi számítógép adatok toohello SQL Data Warehouse feltöltéséhez.

## <a name="step-1-create-a-new-integration-services-project"></a>1. lépés: Új Integration Services-projekt létrehozása
1. Indítsa el a Visual Studio.
2. A hello **fájl** menü **új |} Projekt**.
3. Keresse meg a toohello **telepített |} Sablonok |} Üzleti intelligencia |} Az integrációs szolgáltatások** projekt típusok.
4. Válassza ki **Integration Services-projekt**. Adjon meg értékeket a **neve** és **hely**, majd válassza ki **OK**.

A Visual Studio megnyílik, és létrehoz egy új Integration Services (SSIS) projektet. Majd megnyílik a Visual Studio hello designer hello egyetlen új SSIS-csomag (Package.dtsx) hello projektben. A következő képernyő területek hello jelenik meg:

* Hello bal oldali hello **eszközkészlet** SSIS-összetevők.
* Hello középső hello több lappal a tervezési felülethez. Általában használja legalább hello **vezérlő Flow** és hello **adatfolyam** lapokon.
* A jobb oldali hello hello **Megoldáskezelőben** és hello **tulajdonságok** ablaktábla.
  
    ![][01]

## <a name="step-2-create-hello-basic-data-flow"></a>2. lépés: Hello alapszintű adatfolyam létrehozása
1. Az egérrel húzzon át egy Data Flow feladat hello eszközkészlet toohello közepére hello a tervezési felülethez (a hello **vezérlő Flow** lap).
   
    ![][02]
2. Kattintson duplán a hello Data Flow feladat tooswitch toohello adatfolyam-lapon.
3. Hello eszközkészlet hello más adatforrások listájában húzza egy ADO.NET forrás toohello a tervezési felülethez. Hello forrás adapter kijelölését, megváltoztathatja a neve túl**SQL Server-adatforrás** a hello **tulajdonságok** ablaktáblán.
4. Hello eszközkészlet hello más célhelyre listájából húzza az ADO.NET cél toohello tervezési felülethez hello ADO.NET forrás alatt. Hello cél adapter kijelölését, megváltoztathatja a neve túl**SQL DW cél** a hello **tulajdonságok** ablaktáblán.
   
    ![][09]

## <a name="step-3-configure-hello-source-adapter"></a>3. lépés: Hello forrás adapter konfigurálásához.
1. Kattintson duplán a hello forrás adapter tooopen hello **ADO.NET forrás szerkesztő**.
   
    ![][03]
2. A hello **Csatlakozáskezelő** hello lapján **ADO.NET forrás szerkesztő**, hello kattintson **új** gomb következő toohello **ADO.NET Csatlakozáskezelő**lista tooopen hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel és hozza létre, amelyből ez az oktatóanyag adatokat tölt hello SQL Server adatbázis kapcsolati beállításait.
   
    ![][04]
3. A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel hello kattintson **új** gomb tooopen hello **Csatlakozáskezelő** párbeszédpanel és hozza létre az új kapcsolat.
   
    ![][05]
4. A hello **Csatlakozáskezelő** párbeszédpanel dolgok következő hello mezőbe.
   
   1. A **szolgáltató**, válassza ki a hello SqlClient adatszolgáltatója.
   2. A **kiszolgálónév**, hello SQL Server nevét adja meg.
   3. A hello **toohello server bejelentkezés** szakaszban válassza ki vagy adja meg a hitelesítési adatokat.
   4. A hello **Connect tooa adatbázis** területen válassza ki az AdventureWorks mintaadatbázishoz hello.
   5. Kattintson a **tesztkapcsolat**.
      
       ![][06]
   6. A jelentések hello kapcsolat vizsgálati eredményeket hello hello párbeszédpanel, kattintson **OK** tooreturn toohello **Csatlakozáskezelő** párbeszédpanel megnyitásához.
   7. A hello **Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel.
5. A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **ADO.NET forrás szerkesztő**.
6. A hello **ADO.NET forrás szerkesztő**, a hello **hello tábla vagy nézet hello neve** listában, jelölje be hello **Sales.SalesOrderDetail** tábla.
   
    ![][07]
7. Kattintson a **előzetes** toosee hello hello forrástáblában hello az első 200 adatsorokat **lekérdezés eredményének villámnézete** párbeszédpanel megnyitásához.
   
    ![][08]
8. A hello **lekérdezés eredményének villámnézete** párbeszédpanel, kattintson a **Bezárás** tooreturn toohello **ADO.NET forrás szerkesztő**.
9. A hello **ADO.NET forrás szerkesztő**, kattintson a **OK** toofinish hello-adatforrás konfigurálásakor.

## <a name="step-4-connect-hello-source-adapter-toohello-destination-adapter"></a>4. lépés: Csatlakozás hello forrás adapter toohello céladapter
1. Válassza ki a forrás adapterének hello hello a tervezési felülethez.
2. Válassza ki, amely kiterjeszti hello forrás adapteréről hello kék nyíl, és húzza toohello cél szerkesztő amíg helyen igazodik.
   
    ![][10]
   
    Egy tipikus SSIS-csomag más hello SSIS-eszközkészlet Between hello forrás és cél toorestructure hello, átalakító-összetevőt számos, és az adatok nagybetűhasználatának, ahogy keresztülhalad hello SSIS-adatfolyam. tookeep ebben a példában lehető legegyszerűbb azt kapcsolat hello forrás közvetlenül toohello cél.

## <a name="step-5-configure-hello-destination-adapter"></a>5. lépés: Hello céladapter konfigurálása
1. Kattintson duplán a hello cél adapter tooopen hello **ADO.NET cél szerkesztő**.
   
    ![][11]
2. A hello **Csatlakozáskezelő** hello lapján **ADO.NET cél szerkesztő**, hello kattintson **új** gomb következő toohello **Csatlakozáskezelő**lista tooopen hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel és hozza létre, amelybe ez az oktatóanyag adatokat tölt hello Azure SQL Data Warehouse-adatbázis biztonságoskapcsolat-beállításainak.
3. A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel hello kattintson **új** gomb tooopen hello **Csatlakozáskezelő** párbeszédpanel és hozza létre az új kapcsolat.
4. A hello **Csatlakozáskezelő** párbeszédpanel dolgok következő hello mezőbe.
   1. A **szolgáltató**, válassza ki a hello SqlClient adatszolgáltatója.
   2. A **kiszolgálónév**, adja meg a hello SQL Data Warehouse neve.
   3. A hello **toohello server bejelentkezés** szakaszban jelölje be **használja az SQL Server-hitelesítési** , és írja be a hitelesítési adatokat.
   4. A hello **Connect tooa adatbázis** területen válasszon ki egy létező SQL Data Warehouse-adatbázist.
   5. Kattintson a **tesztkapcsolat**.
   6. A jelentések hello kapcsolat vizsgálati eredményeket hello hello párbeszédpanel, kattintson **OK** tooreturn toohello **Csatlakozáskezelő** párbeszédpanel megnyitásához.
   7. A hello **Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel.
5. A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **ADO.NET cél szerkesztő**.
6. A hello **ADO.NET cél szerkesztő**, kattintson a **új** következő toohello **használni kívánt táblát vagy nézetet** lista tooopen hello **Create Table** párbeszédpanel toocreate utasítást egy oszloplistával hello forrástábla megfelelő új táblát.
   
    ![][12a]
7. A hello **Create Table** párbeszédpanel dolgok következő hello mezőbe.
   
   1. Hello céltábla hello nevének módosítása túl**értékesítési rendelési adatot**.
   2. Távolítsa el a hello **rowguid** oszlop. Hello **uniqueidentifier** adattípusa nem támogatott az SQL Data Warehouse.
   3. Hello hello adattípusának módosítása **LineTotal** oszlop túl**pénz**. Hello **decimális** adattípusa nem támogatott az SQL Data Warehouse. További információt a támogatott adattípusok, lásd: [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].
      
       ![][12b]
   4. Kattintson a **OK** toocreate hello tábla, és térjen vissza toohello **ADO.NET cél szerkesztő**.
8. A hello **ADO.NET cél szerkesztő**, jelölje be hello **hozzárendelések** toosee lapon hogyan hello forrás oszlopai leképezve toocolumns hello cél.
   
    ![][13]
9. Kattintson a **OK** toofinish hello-adatforrás konfigurálásakor.

## <a name="step-6-run-hello-package-tooload-hello-data"></a>6. lépés: Hello tooload hello csomagadatok futtatása
Futtatási hello csomag hello kattintva **Start** gombra, vagy egy hello hello eszköztár **futtatása** hello beállítások **Debug** menü.

Hello csomag toorun kezdődik, mivel látni sárga forgó kerekek tooindicate tevékenység, valamint a hello eddig feldolgozott sorok száma.

![][14]

Hello csomag futása befejeződött, amikor, tekintse meg a zöld jelölését tooindicate sikeres, valamint hello adatok betöltődnek a hello forrás toohello cél sorok maximális számát.

![][15]

Gratulálunk! Az Azure SQL Data Warehouse sikeresen használt SQL Server Integration Services tooload adatokat.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello SSIS-adatfolyam. Kezdje itt: [adatfolyam][Data Flow].
* Megtudhatja, hogyan toodebug és hibaelhárítása a csomagok jobb hello tervezési környezetben. Kezdje itt: [hibaelhárítási eszközök csomag fejlesztési][Troubleshooting Tools for Package Development].
* Megtudhatja, hogyan toodeploy a SSIS projektek és csomagok tooIntegration Services-kiszolgáló vagy tooanother tárolási helyét. Kezdje itt: [telepítési a projekt és csomagok][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
