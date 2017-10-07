---
title: "a rugalmas adatbázis-feladatok aaaInstalling |} Microsoft Docs"
description: "Hello rugalmas feladat szolgáltatás telepítési útmutatót."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 0349f66a4428f81d00d43681d7f2177f273ec032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-elastic-database-jobs-overview"></a>Telepítése rugalmas feladatok – áttekintés
[**Rugalmas adatbázis-feladatok** ](sql-database-elastic-jobs-overview.md) PowerShell telepíthető vagy keresztül kaphatnak a klasszikus Azure-Portal.You hello hozzáférési toocreate és hello PowerShell API használata csak akkor, ha hello PowerShell csomag telepítése feladatok kezelése. Emellett hello PowerShell API-k jóval több funkciót kínál mint hello portál ezen a ponton a időben.

Ha már telepítette **rugalmas adatbázis-feladatok** keresztül hello Portal a meglévő **rugalmas készlet**, hello legújabb Powershell preview tartalmaz parancsfájlok tooupgrade a meglévő telepítést. Nagyon fontos ajánlott tooupgrade a telepítési toohello legújabb **rugalmas adatbázis-feladatok** keresztül új funkciók előnyeit rendelés tootake összetevők hello PowerShell API-k.

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Ingyenes próbaverzió, lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. Hello hello segítségével legújabb verziójának telepítése [Webplatform-telepítő](http://go.microsoft.com/fwlink/p/?linkid=320376). Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* [NuGet parancssori segédprogram](https://nuget.org/nuget.exe) használt tooinstall hello rugalmas adatbázis-feladatok csomagja. További információkért lásd: http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-hello-elastic-database-jobs-powershell-package"></a>Töltse le és hello rugalmas feladatok PowerShell csomag importálása
1. Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg a toohello könyvtárat, amelybe letöltötte NuGet parancssori segédprogram (nuget.exe).
2. Töltse le és importálja **rugalmas adatbázis-feladatok** könyvtárba, amely hello aktuális hello a következő parancs a csomag:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    Hello **rugalmas adatbázis-feladatok** fájl hello helyi könyvtárban nevű mappába kerül **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** ahol *x.x.xxxx.x* hello verziószám által adott jelentéseket tükrözik. hello PowerShell-parancsmagok (beleértve a szükséges ügyfél .dll) találhatók hello **tools\ElasticDatabaseJobs** alkönyvtárát és hello PowerShell parancsfájlok tooinstall, frissítése és eltávolítása is találhatók hello  **eszközök** alkönyvtárát.
3. Keresse meg a toohello eszközök alkönyvtárát hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mappa alatt írja be a cd-eszközök, például:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. Végrehajtás hello.\InstallElasticDatabaseJobsCmdlets.ps1 parancsfájl toocopy hello ElasticDatabaseJobs directory $home\Documents\WindowsPowerShell\Modules be. Ez is automatikusan importálni fogja hello modul használható, például:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-hello-elastic-database-jobs-components-using-powershell"></a>PowerShell-lel hello rugalmas adatbázis-feladatok összetevőinek telepítése
1. Indítsa el a Microsoft Azure PowerShell-parancsablakot, és keresse meg a toohello \tools alkönyvtárát hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mappában: írja be a cd \tools
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. Hello.\InstallElasticDatabaseJobs.ps1 PowerShell parancsfájlt, és adja meg a kért változók értékeit. Ezt a parancsfájlt hoz létre hello ismertetett összetevőkön [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing) hello Azure Cloud Service konfigurálása mellett tooappropriately hello függő összetevőket használnak.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

A parancs futtatásakor egy ablak megnyitása megadását kéri a **felhasználónév** és **jelszó**. Ez nem az Azure hitelesítő adatait, hello felhasználónevet és jelszót, amely hello rendszergazdai hitelesítő adatokat szeretne toocreate hello új kiszolgáló lesz.

a minta betöltéshez megadott hello paraméterek módosíthatók a kívánt beállítást. hello következő hello viselkedés minden paraméter nyújt részletesebb információt:

<table style="width:100%">
  <tr>
    <th>Paraméter</th>
    <th>Leírás</th>
  </tr>

<tr>
    <td>erőforráscsoport-név</td>
    <td>Hello Azure erőforráscsoport-név létrehozása az újonnan létrehozott Azure összetevők toocontain hello biztosít. Ez a paraméter alapértelmezett értéke túl "__ElasticDatabaseJob". Nem ajánlott toochange ezt az értéket.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Az újonnan létrehozott Azure összetevők hello használt hello Azure-beli hely toobe biztosít. Ez a paraméter toohello USA középső RÉGIÓJA hely alapértelmezés szerint.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Megadja a szolgáltatás munkavállalók tooinstall hello számát. Ez a paraméter alapértelmezett értéke: too1. Feldolgozók nagyobb számú használt tooscale hello szolgáltatást és tooprovide magas rendelkezésre állású ki lehet. "2" toouse hello szolgáltatás nagy rendelkezésre állást igénylő telepítésekhez javasolt.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>Hello Felhőszolgáltatás-felhasználásról hello Virtuálisgép-méretet biztosít. Ez a paraméter alapértelmezett értéke: tooA0. A0/A1/A2 A3 paraméterek értékeit okozó hello feldolgozói szerepkör toouse egy: ExtraSmall/kis/közepes vagy nagy mérete, illetve elfogadottak. Fő feldolgozói szerepkör méretét, a további tudnivalókat lásd a [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Standard edition hello szolgáltatásiszint-célkitűzés biztosít. Ez a paraméter alapértelmezett értéke: tooS0. S0/S1/S2 S3 paraméter értékének elfogadottak, aminek következtében az Azure SQL Database hello toouse hello megfelelő slo-t. Az SQL-adatbázis segítségével további információkért lásd: [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Itt hello rendszergazda felhasználóneve hello az újonnan létrehozott az Azure SQL Database-kiszolgálóhoz. Ha nincs megadva, PowerShell hitelesítő adatok megnyílik egy tooprompt hello hitelesítő adatokat.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Azure SQL adatbázis-kiszolgáló található, újonnan létrehozott hello hello rendszergazdai jelszó biztosít. Ha nincs megadva, PowerShell hitelesítő adatok megnyílik egy tooprompt hello hitelesítő adatokat.</td>
</tr>
</table>

A megcélzó számos, az adatbázisok párhuzamosan futó feladatok nagy számú rendelkező rendszerek esetében javasolt toospecify paraméterek például: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Egy meglévő rugalmas feladatok összetevők telepítésének PowerShell használatával
**Rugalmas adatbázis-feladatok** belül a méretezés és magas rendelkezésre állású egy példánya lehet frissíteni. Ez a folyamat lehetővé teszi, hogy a szolgáltatás kód későbbi verziófrissítések toodrop nélkül, és hozza létre a feladatvezérlő adatbázishoz hello. Ez a folyamat is hello belül azonos toomodify hello szolgáltatás virtuális gép méretét vagy a hello kiszolgáló munkavégző verziószám.

tooupdate hello Virtuálisgép-méretet egy telepítés, a következő paraméterekkel rendelkező parancsfájl futtatási hello frissítése az Ön által választott toohello értékeket.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Paraméter</th>
  <th>Leírás</th>
</tr>

  <tr>
    <td>erőforráscsoport-név</td>
    <td>Hello Azure erőforráscsoport-név használható, ha kezdetben megtörtént-e hello rugalmas feladat összetevők azonosítja. Ez a paraméter alapértelmezett értéke túl "__ElasticDatabaseJob". Mivel nem ajánlott toochange ezt az értéket kellett volna toospecify ezt a paramétert.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Megadja a szolgáltatás munkavállalók tooinstall hello számát.  Ez a paraméter alapértelmezett értéke: too1.  Feldolgozók nagyobb számú használt tooscale hello szolgáltatást és tooprovide magas rendelkezésre állású ki lehet.  "2" toouse hello szolgáltatás nagy rendelkezésre állást igénylő telepítésekhez javasolt.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Hello Felhőszolgáltatás-felhasználásról hello Virtuálisgép-méretet biztosít. Ez a paraméter alapértelmezett értéke: tooA0. A0/A1/A2 A3 paraméterek értékeit okozó hello feldolgozói szerepkör toouse egy: ExtraSmall/kis/közepes vagy nagy mérete, illetve elfogadottak. Fő feldolgozói szerepkör méretét, a további tudnivalókat lásd a [rugalmas feladatok összetevők és az árképzés terén](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-hello-elastic-database-jobs-components-using-hello-portal"></a>Hello Portal használatával hello rugalmas adatbázis-feladatok összetevőinek telepítése
Ha elvégezte [létrehozott egy rugalmas készlet](sql-database-elastic-pool-manage-portal.md), telepítése **rugalmas adatbázis-feladatok** összetevők tooenable hello rugalmas készlet minden egyes adatbázison felügyeleti feladatok végrehajtása. Ha használatával hello eltérően **rugalmas adatbázis-feladatok** PowerShell API-k, hello portál illesztő egy meglévő készlet végrehajtása jelenleg korlátozott tooonly.

**Becsült idő toocomplete:** 10 perc.

1. Az irányítópult-nézet hello hello rugalmas készlet keresztül hello [Azure Portal](https://portal.azure.com/#) , kattintson a **létrehozási feladat**.
2. Ha hoz létre egy feladatot az hello először, telepítenie kell **rugalmas adatbázis-feladatok** kattintva **PREVIEW feltételek**.
3. Fogadja el a hello feltételek kattintva hello jelölőnégyzetet.
4. A "telepítés szolgáltatások" hello nézetben kattintson **feladat hitelesítő adatai**.
   
    ![Hello szolgáltatások telepítése][1]
5. Írjon be egy felhasználónevet és jelszót egy adatbázis-rendszergazdához. Hello telepítésének részeként egy új Azure SQL Database kiszolgáló akkor jön létre. Az új kiszolgálón belül egy új adatbázist, hello feladatvezérlő adatbázishoz, néven jön létre, és toocontain hello metaadatokban használt rugalmas adatbázis-feladatok. hello felhasználónevet és jelszót itt létrehozott hello célra toohello feladatvezérlő adatbázishoz bejelentkezéshez használhatók. Külön hitelesítő adatokat hello adatbázisokhoz belül hello parancsfájl végrehajtására szolgál.
   
    ![Felhasználónevet és jelszót hozhat létre][2]
6. Hello OK gombra. hello összetevők, a rendszer létrehozza az új néhány perc múlva [erőforráscsoport](../azure-resource-manager/resource-group-overview.md). Új erőforráscsoport hello van rögzítve toohello üzenőfalon, indítható el, mert a lent látható módon. Létrehozás után, a rugalmas adatbázis-feladatok (felhőalapú szolgáltatás, SQL-adatbázis, a Service Bus és tárolás) hello csoport létrejönnek.
   
    ![a start Bizottsága erőforráscsoport][3]
7. Ha toocreate kísérlet vagy kezelése egy feladatot, amikor a rugalmas feladatok telepíti, így **hitelesítő adatok** hello a következő üzenet jelenik meg.
   
    ![A folyamatban lévő telepítés][4]

Ha az Eltávolítás szükség, hello csoport törléséhez. Lásd: [hogyan feladat toouninstall hello rugalmas adatbázis-összetevőinek](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Következő lépések
A parancsfájl végrehajtása az összes adatbázisra hello csoportban, további információ: jön létre, győződjön meg arról, hogy hello megfelelő jogosultsággal rendelkező hitelesítő adatot [SQL-adatbázisok védelme](sql-database-manage-logins.md).
Lásd: [létrehozása és egy rugalmas adatbázis-feladatok kezelése](sql-database-elastic-jobs-create-and-manage.md) tooget elindult.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
