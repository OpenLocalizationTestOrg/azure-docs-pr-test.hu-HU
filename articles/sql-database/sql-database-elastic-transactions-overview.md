---
title: "felhő adatbázisok aaaDistributed tranzakcióiból"
description: "Az Azure SQL Database rugalmas adatbázis-tranzakciók áttekintése"
services: sql-database
documentationcenter: 
author: torsteng
manager: jhubbard
editor: torsteng
ms.assetid: e14df7a3-7788-4cfb-bcd1-7ad6433ef1f9
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: sql-database
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 9a18f2749108d337bf115b3dc21dd0c7828dd9c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="distributed-transactions-across-cloud-databases"></a>Elosztott tranzakciók több felhőalapú adatbázisban
Az Azure SQL Database (SQL-adatbázis a) rugalmas adatbázis-tranzakciók engedélyezése toorun tranzakciók, amelyek több adatbázisok SQL DB több. Az SQL Database rugalmas adatbázis tranzakciók érhetők el a .NET-alkalmazásokban ADO .NET használatával, és hello megszokott programozási környezetet hello segítségével integrálja [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) osztályok. tooget hello könyvtár, lásd: [.NET-keretrendszer 4.6.1 (webes telepítő)](https://www.microsoft.com/download/details.aspx?id=49981).

A helyszíni ilyen esetben általában szükséges fut a Microsoft elosztott tranzakciók koordinátora (MSDTC). Platform,--szolgáltatás alkalmazás az Azure-ban MSDTC nem érhető el, mert hello képességét toocoordinate elosztott tranzakciók mostantól közvetlenül integrálva lett SQL-adatbázis. Alkalmazások csatlakozhat tooany SQL-adatbázis toolaunch elosztott tranzakciók, valamint hello adatbázisok közül az egyik transzparens módon összehangolják hello elosztott tranzakció, ahogy az ábra a következő hello. 

  ![Az elosztott tranzakciók az Azure SQL Database rugalmas adatbázis-tranzakció használatával ][1]

## <a name="common-scenarios"></a>Gyakori forgatókönyvek
Az SQL-adatbázis a rugalmas adatbázis-tranzakciók engedélyezése alkalmazások toomake atomi módosítások toodata számos különböző SQL-adatbázisban tárolja. hello preview ügyféloldali fejlesztés élmény a C# és .NET összpontosít. A T-SQL használatával kiszolgálóoldali élmény tervezünk-e egy későbbi időpontra.  
A rugalmas adatbázis tranzakciók célok hello a következő esetekben:

* Több adatbázis-alkalmazások az Azure-ban: Ebben a forgatókönyvben az adatok függőleges particionálása SQL DB több adatbázisok között úgy, hogy a különböző típusú adatok legyen elhelyezve a különböző adatbázisokhoz. Egyes műveletek módosítások toodata, amelyet a két vagy több adatbázis szükséges. hello alkalmazás rugalmas adatbázis-tranzakciók toocoordinate hello változások használja az adatbázisok közötti és atomicity biztosítása.
* Horizontálisan skálázott adatbázis-alkalmazások az Azure-ban: Ebben a forgatókönyvben a hello adatrétegbeli használja hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md) vagy önkiszolgáló-horizontális toohorizontally partíció hello adatokat az Adatbázisba az SQL több adatbázis közötti. Egy jól láthatóan elhelyezett használati eset hello kell tooperform atomi módosítások szilánkos több-bérlős alkalmazások esetén módosítások span bérlők. Egy átvitelét egy bérlő tooanother, mind a különböző adatbázisokhoz elhelyezkedhet gondol példányhoz. A második esetben minden részletre kiterjedő horizontális tooaccommodate kapacitási igényeihez a nagy bérlőhöz, amely viszont általában azt jelenti, hogy néhány atomi műveletek igények toostretch hello használt több adatbázisok között ugyanazt a bérlői. Egy harmadik esetben az adatbázisok közötti replikált tooreference adatokat atomi frissíti. Ezek a sorok mentén atomi, a tranzakciós, műveletek most is koordinált hello preview több adatbázis közötti.
  A rugalmas adatbázis-tranzakció kétfázisú végrehajtási tooensure tranzakció atomicity használja az adatbázisok. Remekül beválik, ha egy tranzakción belül egyszerre legfeljebb 100 adatbázisok tranzakciók is. Ezek nem lesz korlátozva, de egy kell látnia teljesítmény és a rugalmas adatbázis-tranzakciók toosuffer sikerességéről működés felső korlátjának túllépésekor.

## <a name="installation-and-migration"></a>Telepítés és áttelepítés
SQL Database rugalmas adatbázis-tranzakciókhoz hello képességek szolgáltatáson keresztül frissítések toohello .NET-kódtárakra System.Data.dll és System.Transactions.dll. hello DLL-ek győződjön meg arról, hogy kétfázisú végrehajtási használatos, ahol szükséges tooensure atomicity. a rugalmas adatbázis-tranzakció használó toostart fejlesztését alkalmazások telepítése [.NET-keretrendszer 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) vagy újabb verziója. Amikor hello .NET-keretrendszer egy korábbi verzióját futtatja, a tranzakciók toopromote tooa elosztott tranzakció sikertelen lesz, és kivételt generál.

A telepítés után használható hello elosztott tranzakció API-k a System.Transactions kapcsolatok tooSQL DB. Ha ezen API-k használata meglévő MSDTC alkalmazásai, egyszerűen építse újra a meglévő alkalmazások .NET 4.6 hello 4.6.1 telepítése után keretrendszer. A projektek .NET 4.6 célként, ha azok automatikusan használja frissítése hello DLL-eket hello új keretrendszer és az elosztott tranzakciók API-hívások, valamint a kapcsolatok tooSQL DB most már sikeres lesz.

Ne feledje, hogy a rugalmas adatbázis-tranzakció szükséges MSDTC telepítése. Rugalmas adatbázis-tranzakció ehelyett közvetlenül kezelt által és az SQL DB belül. Ez jelentősen leegyszerűsíti az felhős rendszerekben mivel MSDTC telepítése nem szükséges toouse elosztott tranzakciók az SQL-Adatbázist. Szakasz 4 részletesebben ismerteti, hogyan toodeploy rugalmas adatbázis-tranzakció és hello kötelező .NET-keretrendszer a felhőalapú alkalmazások tooAzure együtt.

## <a name="development-experience"></a>Fejlesztési felület
### <a name="multi-database-applications"></a>Több adatbázis-alkalmazások
hello alábbi mintakód használ hello megszokott programozási környezetet .NET System.Transactions. hello TransactionScope osztály a .NET környezeti tranzakció hoz létre. ("Környezeti tranzakció" egyike, hogy él, az aktuális szál hello.) Az összes kapcsolat megnyitni a TransactionScope-objektum hello belül hello tranzakció részt. Ha a különböző adatbázisokhoz, hello tranzakció automatikusan emelt szintű tooa elosztott tranzakciót. hello tranzakció eredményét hello beállítása hello hatókör toocomplete tooindicate egy véglegesítési szabályozza.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Horizontálisan skálázott adatbázis-alkalmazások
Az SQL Database rugalmas adatbázis tranzakciók is támogatja az elosztott tranzakciók, ahol a módszert hello OpenConnectionForKey hello rugalmas adatbázis könyvtár tooopen az ügyfélkapcsolatok méretezett kimenő adatok koordinációs réteg. Vegye figyelembe a esetben, amikor meg kell tooguarantee egységessége módosítások több különböző horizontális értékek között. Kapcsolatok toohello szilánkok hello különböző horizontális kulcsértékei futtató rendszer közvetítőalapú OpenConnectionForKey használatával. Hello általános esetben hello kapcsolatok lehet toodifferent szilánkok úgy, hogy tranzakciós garanciák biztosítása elosztott tranzakciót igényel. a következő példakód hello szemlélteti ezt a módszert használja. Azt feltételezi, hogy a shardmap nevű változóra a shard képezze le az hello elastic database ügyféloldali kódtárának használt toorepresent:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>A .NET telepítése Azure Cloud Services csomag
Azure biztosít több ajánlatok toohost .NET-alkalmazások. Hello különböző ajánlatok összehasonlítása érhető el [Azure App Service, a Cloud Services és a virtuális gépek összevetése](../app-service-web/choose-web-site-cloud-service-vm.md). Ha hello Vendég-Operációsrendszert az hello ajánlat kisebb, mint a .NET 4.6.1 rugalmas tranzakciók szükséges, akkor tooupgrade hello vendég operációs rendszer too4.6.1. 

Az Azure App Service szolgáltatások frissíti a toohello vendég operációs rendszer jelenleg nem támogatottak. Az Azure Virtual Machines, egyszerűen jelentkezzen be a virtuális gép hello, és futtassa a telepítőt hello hello legújabb .NET-keretrendszer. Azure Cloud Services van szüksége a hello indítási feladatok a központi telepítés egy újabb .NET tooinclude hello telepítését. hello fogalmakat és lépéseket ismertetett [felhőalapú szolgáltatás szerepkör telepítése .NET](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Figyelje meg, hogy a telepítő hello a .NET 4.6.1 szükség lehet a további ideiglenes tároló rendszerindítása folyamat Azure felhőszolgáltatások, mint .NET 4.6 telepítőjéhez hello hello során. a sikeres telepítés tooensure, szüksége tooincrease ideiglenes tárolóra az Azure-felhőszolgáltatásban hello LocalResources szakaszban és hello környezeti beállítások az indítási feladat a ServiceDefinition.csdef fájlban hello alábbi ábrán Példa:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Tranzakciók több kiszolgáló között
A rugalmas adatbázis-tranzakció támogatottak az Azure SQL Database különböző logikai kiszolgáló között. Ha tranzakciók logikai kiszolgáló határokat, hello részt vevő kiszolgálók először kell toobe kölcsönös kommunikációs kapcsolat létrejött. Hello kommunikációs kapcsolat létrehozása után bármely adatbázis két kiszolgáló részt vehetnek-adatbázisok rugalmas tranzakciók hello valamelyikében hello tároló más kiszolgálón. Több mint két logikai kiszolgáló átfedés tranzakciókkal kommunikációs kapcsolatot kell toobe helyen bármely két logikai kiszolgálók között.

A következő PowerShell parancsmagok toomanage a kiszolgálók közötti kommunikációhoz kapcsolatok rugalmas adatbázis-tranzakciókhoz hello használata:

* **Új AzureRmSqlServerCommunicationLink**: Ez a parancsmag toocreate kommunikációs új Azure SQL Database logikai két kiszolgálója közötti kapcsolatot használjon. hello egy kapcsolat szimmetrikus ami azt jelenti, hogy mindkét kiszolgálón is kezdeményezhető a tranzakciót hello és más kiszolgálón.
* **Get-AzureRmSqlServerCommunicationLink**: Ez a parancsmag meglévő tooretrieve-kommunikációt használnak a kapcsolatok és azok tulajdonságait.
* **Remove-AzureRmSqlServerCommunicationLink**: Ez a parancsmag tooremove egy már meglévő kommunikációs kapcsolat használata. 

## <a name="monitoring-transaction-status"></a>Tranzakció állapotának figyelése
SQL-adatbázis a toomonitor status and progress a folyamatban lévő rugalmas adatbázis-tranzakció és dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) használja. Az összes dinamikus felügyeleti nézetek kapcsolódó tootransactions szükségesek az SQL-adatbázis az elosztott tranzakciók. Hello megfelelő listája dinamikus felügyeleti nézetek itt találja: [tranzakció kapcsolódó dinamikus felügyeleti nézetek és függvények (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

A dinamikus felügyeleti nézetek különösen hasznosak:

* **sys.DM\_tran\_aktív\_tranzakciók**: a jelenleg aktív tranzakciók és azok állapotát tartalmazza. hello UOW-Értékkel (munkaegység) oszlop azonosíthatja hello különböző alárendelt tranzakciók toohello tartozó azonos elosztott tranzakció. Minden tranzakció belül ugyanaz az elosztott tranzakciók lebonyolítására hello hello azonos UOW-értékkel. Lásd: hello [DMV dokumentáció](https://msdn.microsoft.com/library/ms174302.aspx) további részleteket.
* **sys.DM\_tran\_adatbázis\_tranzakciók**: tranzakciók, például hello tranzakció hello naplóban elhelyezésének további információkkal szolgál. Lásd: hello [DMV dokumentáció](https://msdn.microsoft.com/library/ms186957.aspx) további részleteket.
* **sys.DM\_tran\_zárolások**: hello megtalálható zárolt jelenleg folyamatban lévő tranzakciók által információkat nyújt. Lásd: hello [DMV dokumentáció](https://msdn.microsoft.com/library/ms190345.aspx) további részleteket.

## <a name="limitations"></a>Korlátozások
hello jelenleg a következő korlátozások vonatkoznak az SQL-adatbázis adatbázis-tranzakció tooelastic vonatkoznak:

* Csak az SQL-adatbázis az adatbázisok közötti tranzakciók támogatottak. Más [X / nyissa meg a XA](https://en.wikipedia.org/wiki/X/Open_XA) rugalmas adatbázis-tranzakció erőforrás-szolgáltatók és az adatbázisok SQL DB kívül nem szerepelhet. Ez azt jelenti, hogy a rugalmas adatbázis-tranzakció nem a a helyszíni SQL Server és az Azure SQL-adatbázisok között stretch. Az elosztott tranzakciók a helyszíni továbbra is toouse MSDTC. 
* Csak ügyfél egyeztetett .NET-alkalmazás tranzakciók használata támogatott. T-SQL, például a BEGIN TRANSACTION ELOSZTOTT kiszolgálóoldali támogatása, tervezett, de még nem érhető el. 
* WCF-szolgáltatások között nem támogatottak. Például hogy egy WCF-szolgáltatás metódus, amely végrehajtja a tranzakciót. A tranzakció hatókörén belül hello hívás befoglaló meghiúsul, mint egy [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Következő lépések
A kérdésekhez, lépjen kapcsolatba a hello toous [SQL-adatbázis fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a funkciókra vonatkozó kérések, adjon hozzá toohello [SQL adatbázis-visszajelzési fórumon](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



