---
title: "aaaMove adatok tooSQL Server Azure virtuális géphez |} Microsoft Docs"
description: "Adatok áthelyezése egybesimított fájlokba, illetve egy helyszíni SQL Server tooSQL Server Azure virtuális gépen."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 2c9ef1d3-4f5c-4b1f-bf06-223646c8af06
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 63e02158f9c99f4478c4eb1cde62c877983dcf27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-toosql-server-on-an-azure-virtual-machine"></a>Helyezze át az adatokat tooSQL Server Azure virtuális géphez
Ez a témakör bemutatja az adatok áthelyezése vagy strukturálatlan fájlból (CSV vagy TSV formátumban), vagy egy helyi SQL Server tooSQL Server Azure virtuális géphez hello-beállítások. Ezek a feladatok áthelyezése adatok toohello felhő hello Team adatok tudományos folyamat részét képezik.

Ez a témakör bemutatja a Machine Learning hello áthelyezése adatok tooan Azure SQL Database beállítások, lásd: [adatok tooan Azure SQL-adatbázis áthelyezése az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

Hello **menü** hivatkozások tootopics, amelyek ismertetik, hogyan tooingest adatok más cél környezetekben, ahol hello adatok is tárolhatók és feldolgozhatók, során hello Team adatok tudományos folyamat (TDSP) alatt.

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

hello következő táblázat összefoglalja hello beállítások áthelyezése adatok tooSQL Server Azure virtuális géphez.

| <b>FORRÁS</b> | <b>CÉL: SQL Server Azure virtuális gépen</b> |
| --- | --- |
| <b>Egybesimított fájl</b> |1. <a href="#insert-tables-bcp">Parancssori tömeges másolási segédprogram (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">A tömeges beszúrás SQL-lekérdezés</a><br> 3. <a href="#sql-builtin-utilities">Az SQL Server grafikus beépített segédprogramok</a> |
| <b>A helyszíni SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló</a><br> 2. <a href="#export-flat-file">Exportálás tooa lapos fájl</a><br> 3. <a href="#sql-migration">SQL-adatbázis áttelepítése varázsló</a> <br> 4. <a href="#sql-backup">Adatbázis biztonsági mentése és visszaállítása</a><br> |

Figyelje meg, hogy a jelen dokumentum céljából feltételezzük, hogy az SQL-parancsok végrehajtott SQL Server Management Studio vagy Visual Studio adatbázis-kezelő.

> [!TIP]
> Alternatív megoldásként használható [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate és ütemezés szerinti egy folyamatot, amely áthelyezi adatok tooa SQL Server virtuális gép az Azure-on. További információkért lásd: [Azure Data Factory (másolási tevékenység) adatok másolása](../data-factory/data-factory-data-movement-activities.md).
>
>

## <a name="prereqs"></a>Előfeltételek
Ez az oktatóanyag feltételezi, hogy rendelkezik:

* Egy **Azure-előfizetés**. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Egy **Azure storage-fiók**. Szüksége lesz egy Azure storage-fiók ebben az oktatóanyagban hello adatainak tárolásához. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk. Hello storage-fiók létrehozása után kell tooobtain hello fiók kulcs tooaccess hello tárterületet használja. Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Kiépített **egy Azure virtuális Gépen található SQL-kiszolgáló**. Útmutatásért lásd: [állítsa be az Azure SQL Server virtuális gép speciális elemzésekre IPython Notebook kiszolgálóként](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Telepített és konfigurált **Azure PowerShell** helyileg. Útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a>Adatok áthelyezése egy egybesimított fájl forrás tooSQL kiszolgálót egy Azure virtuális gépen
Ha az adatok egy egyszerű fájlban (rendezett sorhoz/oszlophoz formátumban), azok áthelyezett tooSQL Server Azure virtuális gép a következő módszerek hello keresztül:

1. [Parancssori tömeges másolási segédprogram (BCP)](#insert-tables-bcp)
2. [A tömeges beszúrás SQL-lekérdezés](#insert-tables-bulkquery)
3. [Az SQL Server (importálási/exportálási, SSIS) grafikus beépített segédprogramok](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Parancssori tömeges másolási segédprogram (BCP)
BCP parancssori segédprogram SQL Server telepített, és egyik hello leggyorsabb módon toomove adatokat. Minden három SQL Server változat (a helyszíni SQL Server, SQL Azure és SQL Server Azure virtuális gép) is használható.

> [!NOTE]
> **Az adatok hol kell lennie a BCP?**  
> Bár ez nem szükséges, hogy hello hello célként SQL Server egyazon számítógépen lehetővé teszi, hogy gyorsabb átvitelt (hálózati sebesség és a helyi lemez IO sebesség) található forrás adatokat tartalmazó fájlok. Hello egybesimított fájlok tartalmazó adatok toohello gépet áthelyezheti adott SQL Server használatával van telepítve, mint eszközök különböző fájlok másolása [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Tártallózó](http://storageexplorer.com/) vagy a windows másolja és illessze be távoli keresztül Asztal protokoll (RDP).
>
>

1. Győződjön meg arról, hogy a hello tároló SQL Server-adatbázis hello adatbázis és hello táblák jöttek létre. Íme egy példa bemutatja, hogyan toodo adott használatával hello `Create Database` és `Create Table` parancsokat:

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. Hello meghatározó formátumfájl séma hello hello tábla parancs követően – hello gép parancssori hello hello kiállításával bcp futtató készítése.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. Hello adatok beszúrása hello adatbázis hello bcp parancs használatával az alábbiak szerint. Feltételezve, hogy hello SQL Server telepítve van ugyanezen a gépen ezt kell működnie a parancssori hello:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimalizálja a BCP beszúrása** tekintse meg a következő cikket hello ["Tömeges importálással optimalizálása útmutatás"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) ilyen toooptimize szúrja be.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Gyorsabb adatátvitelt jelölik a Beszúrás parallelizing
Ha nagy hello adatokat helyez át, felgyorsíthatja dolgot egy PowerShell-parancsfájlt párhuzamosan több BCP parancsok egyidejűleg végrehajtásával.

> [!NOTE]
> **Big Data típusú adatok adatfeldolgozást** nagy és nagyon nagy adatkészletek betöltése toooptimize adatok particionálása a logikai és fizikai adatbázis táblák fájlcsoportok és a partíció táblákat. Létrehozás és adattáblák toopartition betöltés kapcsolatos további információkért lásd: [párhuzamos terhelés SQL partíciós táblákba](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
>
>

hello PowerShell-parancsfájlpélda az alábbi bemutatása párhuzamos Beszúrás a bcp segítségével:

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for hello script tooexecute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
      }


    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }


    # Wait for it all toocomplete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting hello information back from hello jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset hello execution policy


### <a name="insert-tables-bulkquery"></a>A tömeges beszúrás SQL-lekérdezés
[Tömeges beszúrás SQL-lekérdezés](https://msdn.microsoft.com/library/ms188365) lehet a sorhoz/oszlophoz alapú fájlokban hello adatbázisba használt tooimport adatok (hello támogatott típusok ismertetnek a[tömeges exportálása és importálása (SQL Server) előkészítése adatait](https://msdn.microsoft.com/library/ms188609)) témakör.

Az alábbiakban néhány Példaparancsok tömeges Beszúrás az alábbiak szerint vannak:  

1. Az adatok elemzéséhez, és állítsa be az egyéni beállításokat arról, hogy hello az SQL Server adatbázis azt feltételezi, hogy a dátumok például semmilyen különleges mezők formátuma azonos hello toomake importálása előtt. Íme egy példa hogyan tooset hello dátum formátuma év, hónap-nap (ha az adatok hello dátum év, hónap-nap formátumban):

        SET DATEFORMAT ymd;    
2. Importálja az adatokat, és tömeges importálási utasítást:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be hello row separator in your data
        )

### <a name="sql-builtin-utilities"></a>Az SQL Server beépített segédprogramok
SQL Server integrációja Services (SSIS) tooimport adatok SQL Server egy egybesimított fájlból Azure virtuális gép is használhatja.
SSIS két studio környezetekben érhető el. További információkért lásd: [Integration Services (SSIS) és a Studio környezetek](https://technet.microsoft.com/library/ms140028.aspx):

* További részletek az SQL Server Data Tools: [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
* További részletek az Import/Export varázsló hello: [SQL Server importálása és exportálása varázsló](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Adatok áthelyezése a helyszíni SQL Server tooSQL kiszolgálót egy Azure virtuális gépen
A következő áttelepítési stratégiák hello is használhatja:

1. [Központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [TooFlat fájl exportálása](#export-flat-file)
3. [SQL-adatbázis áttelepítése varázsló](#sql-migration)
4. [Adatbázis biztonsági mentése és visszaállítása](#sql-backup)

Azt mutatják be ezen az alábbi:

### <a name="deploy-a-sql-server-database-tooa-microsoft-azure-vm-wizard"></a>Központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló
Hello **központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló** egy helyi SQL Server példány tooSQL kiszolgálót egy Azure virtuális gépen egy egyszerű és ajánlott módja toomove adatokat jelzi. Részletes lépéseit, valamint az egyéb alternatívák döntéseken, lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>TooFlat fájl exportálása
Különböző módszereket lehet használt toobulk adatok exportálása egy helyszíni SQL Server kiszolgáló hello leírtak [tömeges adatok importálása és exportálása a (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) témakör. Ez a dokumentum példaként hello tömeges másolási Program (BCP) szakaszában. Adatok exportálása egy egyszerű fájlba, után azok importált tooanother SQL server tömeges importálással.

1. A helyszíni SQL Server tooa fájl hello adatok exportálása az alábbiak szerint hello bcp segédprogram használatával

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Hello adatbázis és hello tábla létrehozása az SQL Server virtuális gép Azure-ban hello `create database` és `create table` az 1. lépésben exportált hello tábla sémáját.
3. Hozzon létre egy formátumfájlt leíró hello táblaséma hello adatok exportálása/importálása folyamatban. Hello formátumfájl részleteit ismerteti a [hozzon létre egy Formátumfájlt (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Ha SQL Server-számítógépen fut a BCP hello formátumú fájl létrehozása

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Formázza a fájl létrehozása a jelentés futtatásakor BCP távoli SQL-kiszolgálón

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. A szakaszban leírt hello módszerekkel [áthelyezése adatforrásból származó fájl](#filesource_to_sqlonazurevm) toomove hello adatok egybesimított fájlokba tooa SQL Server.

### <a name="sql-migration"></a>SQL-adatbázis áttelepítése varázsló
[SQL Server adatbázis áttelepítése varázsló](http://sqlazuremw.codeplex.com/) adja meg egy felhasználóbarát módon toomove között két SQL server-példányok adatait. Lehetővé teszi a hello felhasználói toomap hello Adatséma források és a cél táblák között, típusú oszlopokat és a különböző egyéb funkciók kiválasztása. Hello színfalak tömeges másolási (BCP) használja. A képernyőfelvétel a hello hello SQL-adatbázis áttelepítése varázsló alább az üdvözlőképernyő.  

![SQL Server varázsló][2]

### <a name="sql-backup"></a>Adatbázis biztonsági mentése és visszaállítása
SQL Server támogatja:

1. [Adatbázis biztonsági mentése és visszaállítása funkció](https://msdn.microsoft.com/library/ms187048.aspx) (mindkét tooa helyi fájl vagy bacpac exportálás tooblob) és [adatok rétegből álló alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac használatával).
2. Képes toodirectly SQL Server virtuális gépek létrehozása az Azure másolt adatbázis vagy példány tooan meglévő SQL Azure-adatbázishoz. További részletekért lásd: [használata hello másolása adatbázis varázsló](https://msdn.microsoft.com/library/ms188664.aspx).

Fel/helyreállítani a egy képernyőfelvétel a hello adatbázis biztonsági beállításokat az SQL Server Management Studio alább láthatók.

![SQL Server Import eszközt][1]

## <a name="resources"></a>Erőforrások
[Egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Az SQL Server használata Azure virtuális gépeken – áttekintés](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
