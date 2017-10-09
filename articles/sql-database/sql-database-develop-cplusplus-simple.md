---
title: "aaaConnect tooSQL adatbázis-C és C++ |} Microsoft Docs"
description: "Használja a hello minta kódot a gyors üzembe helyezési toobuild a modern alkalmazásnak a C++, és egy hatékony relációs adatbázis hello felhőben az Azure SQL Database által támogatott."
services: sql-database
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: 
ms.assetid: 07d9e0b1-3234-4f17-a252-a7559160a9db
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 03/06/2017
ms.author: edmacauley
ms.openlocfilehash: 9b581424c91bfdd93deb6914212629519a011d8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-database-using-c-and-c"></a>TooSQL Kapcsolódás adatbázis-C és C++ használatával
A feladás egy vagy több fejlesztésére C és C++ fejlesztők tooconnect tooAzure SQL DB közben. Az bontani szakaszok, legjobban rögzíti az érdeklődését toohello szakasz is ugorhat. 

## <a name="prerequisites-for-hello-cc-tutorial"></a>Hello C/C++-oktatóanyag előfeltételei
Győződjön meg arról, hogy a következő elemek hello:

* Aktív Azure-fiók. Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/). Hello C++ nyelvi összetevők toobuild telepítse, és a minta futtatásához.
* [Visual Studio Linux fejlesztői](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Ha Linux rendszeren fejleszt, telepítenie kell a hello Visual Studio Linux bővítmény. 

## <a id="AzureSQL"></a>Az Azure SQL Database és SQL Server virtuális gépeken
Az Azure SQL épül Microsoft SQL Server és tervezett tooprovide egy magas rendelkezésre állású, performant és méretezhető szolgáltatás. Nincsenek számos előnyt toousing SQL Azure a saját adatbázis helyi futó keresztül. Az SQL Azure-ral, nem rendelkezik tooinstall, állítsa be, karbantartása vagy kezeli az adatbázis, de csak hello tartalom és az adatbázis szerkezetének hello. Azt az adatbázisok, például a hibatűrés és a redundancia összes beépített foglalkoznia tipikus tényezőket. 

Azure SQL server-munkaterhelések két lehetősége van: Azure SQL-adatbázis, a szolgáltatás és a virtuális gépek (VM) SQL server adatbázis. Nem Microsoft kap részletes információkat hello különbségei e két, azzal a különbséggel, hogy az Azure SQL-adatbázis a legjobb megoldás az új felhőalapú alkalmazásokat tootake előnyeit költségmegtakarításhoz hello és a teljesítmény optimalizálása, hogy a szolgáltatások nyújt. Ha tervezi az áttelepítésével vagy kiterjeszti a helyszíni alkalmazások toohello felhő, SQL server Azure virtuális gépen működhetnek jobban meg. tookeep dolgot egyszerű ebben a cikkben hozzuk létre az Azure SQL-adatbázis. 

## <a id="ODBC"></a>Adat-hozzáférési technológiák: ODBC és OLE DB
Csatlakozás tooAzure SQL DB ugyanolyan helyzetet teremt, és jelenleg két módon tooconnect toodatabases: ODBC (Nyissa meg az adatbázis-kapcsolat) és az OLE-DB (objektum csatolása és beágyazása adatbázis). Az elmúlt években Microsoft összhangban van [a natív relációs adatok elérése ODBC](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/). ODBC viszonylag egyszerű, de még sokkal gyorsabb, mint az OLE DB. hello Itt csak szerint, az ODBC egy régi C-stílusú API-t használja. 

## <a id="Create"></a>1. lépés: Az Azure SQL-adatbázis létrehozása
Lásd: hello [első lépések lap](sql-database-get-started-portal.md) toolearn hogyan toocreate egy adatbázist.  Azt is megteheti, amelyeket követve ez [rövid kétperces videó](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) egy Azure SQL adatbázis használatával toocreate hello Azure-portálon.

## <a id="ConnectionString"></a>2. lépés: A kapcsolati karakterlánc beolvasása
Miután az Azure SQL-adatbázis van megadva, ki a következő lépéseket toodetermine kapcsolatadatok hello toocarry kell, és tűzfalhozzáférés az ügyfél IP-cím hozzáadása. 

A [Azure-portálon](https://portal.azure.com/), nyissa meg a hello tooyour Azure SQL adatbázis ODBC kapcsolati karakterlánc **adatbázis-kapcsolati karakterláncok megjelenítése** felsorolt hello áttekintés szakaszban az adatbázis részeként: 

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

Hello hello tartalom másolása **(tartalmazza a Node.js) [SQL-hitelesítés] ODBC** karakterlánc. Ez a karakterlánc használjuk a C++ ODBC parancssori parancsértelmező a későbbi tooconnect. Ez a karakterlánc például hello illesztőprogram, a kiszolgáló és a más adatbázis paramétereit részletes adatokat biztosít. 

## <a id="Firewall"></a>3. lépés: Az IP-toohello tűzfal hozzáadása
Nyissa meg az adatbázis-kiszolgáló tűzfal szakasz toohello, és adja hozzá a [ügyfél IP-toohello tűzfal az alábbi lépések végrehajtásával](sql-database-configure-firewall-settings.md) toomake meg arról, hogy azt a sikeres kapcsolatot is: 

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Ezen a ponton konfigurálta-e az Azure SQL-Adatbázisba, és készen áll a tooconnect a C++ kódból. 

## <a id="Windows"></a>4. lépés: Csatlakozás Windows C/C++-alkalmazás
Segítségével könnyedén csatlakoztathatók a tooyour [ODBC használatával Ez a minta használata Windows Azure SQL DB](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) , amely a Visual Studio létrehozza. hello mintát, amely használt tooconnect tooour Azure SQL Database parancssori ODBC-értelmező valósítja meg. Ez a minta egy adatbázis adatforrás neve (DSN) fájl parancssori argumentumként vagy hello részletes kapcsolati karakterláncot, azt a hello Azure-portálon a korábban kimásolt vesz igénybe. Ebben a projektben hello tulajdonság lapot, és illessze be a hello kapcsolati karakterlánc egy parancs argumentumként, ahogy az itt látható: 

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Győződjön meg arról, hogy részletesen hello megfelelő hitelesítési az adatbázis az adott adatbázis-kapcsolati karakterlánc részeként. 

Indítsa el a hello alkalmazás toobuild azt. A sikeres kapcsolat ellenőrzése ablakban a következő hello kell megjelennie. Néhány alapvető SQL-parancsok például is futtathat **tábla létrehozása** toovalidate az adatbázis-kapcsolat:

![SQL-parancsok](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

Azt is megteheti létrehozhat egy DSN fájlt, hello varázsló, amely elindul, amikor nincs parancs argumentum van megadva. Azt javasoljuk, hogy ez a beállítás meg. A DSN-fájl használható automatizálási védelem, valamint a hitelesítési beállításokat: 

![DSN-fájl létrehozása](./media/sql-database-develop-cplusplus-simple/datasource.png)

Gratulálunk! Most már sikeresen csatlakozott tooAzure SQL C++ és ODBC használata Windows rendszeren. Olvasási továbbra is toodo hello azonos Linux platformon is. 

## <a id="Linux"></a>5. lépés: Csatlakozás egy Linux-C/C++-alkalmazás
Ha még nem kérték egyes hello híreket, miközben a Visual Studio lehetővé teszi az olyan toodevelop C++ Linux alkalmazás is. Az új forgatókönyv hello olvashat [Linux fejlesztési Visual C++](https://blogs.msdn.microsoft.com/vcblog/2016/03/30/visual-c-for-linux-development/) blog. toobuild Linux, a Linux distro futtató távoli számítógép szükséges. Ha még nem rendelkezik érhető el, beállíthat egy be gyorsan segítségével [Linux Azure virtuális gépek](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Ebben az oktatóanyagban Tételezzük fel, hogy rendelkezik-e az Ubuntu 16.04 Linux terjesztési beállítása. Itt hello lépéseket tooUbuntu 15.10, Red Hat 6 és Red Hat 7 kell alkalmazni. 

hello lépések telepítése szükséges az SQL és ODBC a distro hello szalagtárak:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Indítsa el a Visual Studio. Az eszközök -> Beállítások -> Cross Platform -> Csatlakozáskezelő, vegye fel a kapcsolat tooyour Linux be: 

![Eszközök beállításai](./media/sql-database-develop-cplusplus-simple/tools.png)

SSH-n keresztül kapcsolat létrejötte után hozzon létre egy üres projektsablon (Linux): 

![Új projekt sablon](./media/sql-database-develop-cplusplus-simple/template.png)

Ezután hozzáadhat egy [új C forrásfájl, és cserélje le ezt a tartalmat](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). Hello ODBC API-k SQLAllocHandle SQLSetConnectAttr és SQLDriverConnect használ, képes tooinitialize legyen és kapcsolat tooyour adatbázis létrehozása. Például hello Windows ODBC mintával kell tooreplace hello SQLDriverConnect hívásainak hello adatokkal az adatbázis-kapcsolati karakterlánc paraméterek hello Azure-portálon a korábban kimásolt. 

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

utolsó lépésként toodo hello előtt fordítása tooadd **odbc** könyvtár függőségei: 

![ODBC egy bemeneti könyvtár hozzáadása](./media/sql-database-develop-cplusplus-simple/lib.png)

toolaunch az alkalmazás hello Linux konzol elindítani a hello **Debug** menüben: 

![Linux-konzol](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Ha a kapcsolat sikeres volt, meg kell jelennie az hello hello Linux konzol nyomtatva aktuális adatbázis neve: 

![Linux konzol kimenetét](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Gratulálunk! Sikeresen lefuttatta a hello oktatóanyag, és most kapcsolódhatnak tooyour Azure SQL Database C++ Windows és Linux platformokon.

## <a id="GetSolution"></a>Hello teljes C/C++ oktatóanyag megoldás beszerzése
Hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben a github webhelyen található:

* [ODBC C++ Windows minta](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), hello Windows C++ ODBC minta tooconnect tooAzure SQL letöltése
* [ODBC C++ Linux minta](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), hello Linux C++ ODBC minta tooconnect tooAzure SQL letöltése

## <a name="next-steps"></a>Következő lépések
* Felülvizsgálati hello [SQL adatbázis-fejlesztői áttekintés](sql-database-develop-overview.md)
* További információ a hello [ODBC API-referencia](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>További források
* [Tervezési minták az Azure SQL Database-t használó több-bérlős SaaS-alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Fedezze fel az összes hello [képességek SQL-adatbázis](https://azure.microsoft.com/services/sql-database/)

