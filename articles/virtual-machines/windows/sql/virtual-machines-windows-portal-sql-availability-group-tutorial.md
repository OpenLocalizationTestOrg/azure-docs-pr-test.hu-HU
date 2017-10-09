---
title: "Kiszolgáló rendelkezésre állási csoportok – Azure virtuális gépek - oktatóanyag aaaSQL |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate egy SQL Server mindig a rendelkezésre állási csoport Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 65b4210b0f851828a32a02053b03e4b8d469ba4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a>Konfigurálás mindig a rendelkezésre állási csoport az Azure virtuális gép manuálisan

Ez az oktatóanyag bemutatja, hogyan toocreate egy SQL Server mindig a rendelkezésre állási csoport Azure virtuális gépeken. hello teljes oktatóanyag egy rendelkezésre állási csoportban két SQL-kiszolgálón az adatbázis-replikát hoz létre.

**Becsült idő**: toocomplete körülbelül 30 percet vesz igénybe, ha hello előfeltételek teljesülnek.

hello ábra szemlélteti a hello oktatóanyag felépítéséhez.

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Előfeltételek

hello oktatóanyag feltételezi, hogy rendelkezik-e az SQL Server Always On rendelkezésre állási csoportokkal kapcsolatos alapvető ismeretekkel. Ha további tájékoztatásra van szüksége, tekintse meg [az mindig a rendelkezésre állási csoportok áttekintése (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).

hello következő táblázat sorolja fel, hogy az oktatóanyag elindítása előtt kell toocomplete hello Előfeltételek:

|  |Követelmény |Leírás |
|----- |----- |----- |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | Két SQL-kiszolgáló | -Az Azure rendelkezésre állási csoportok <br/> -Egyetlen tartományban <br/> -A Feladatátvételi fürtszolgáltatás telepítése |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | A fürt tanúsító fájlmegosztás |  
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server-szolgáltatásfiók | Tartományi fiók |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server Agent szolgáltatásfiók használatával | Tartományi fiók |  
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Tűzfal portjainak megnyitása | -SQL kiszolgáló: **1433** az alapértelmezett példány esetén <br/> -Adatbázis-tükrözési végpontját: **5022** vagy minden elérhető port <br/> -Azure terheléselosztói mintavétel: **59999** vagy minden elérhető port |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Adja hozzá a Feladatátvételi fürtszolgáltatás | Mindkét SQL Server szükség erre a szolgáltatásra |
|![Négyzetes](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Telepítési tartományi fiók | -Minden egyes SQL Server helyi rendszergazdája <br/> – SQL Server SysAdmin (rendszergazda) rögzített kiszolgálói szerepkör az SQL Server minden példányának tagja  |


Hello oktatóanyag elkezdéséhez kell túl[végezze el az Azure virtuális gépek létrehozását a Always On rendelkezésre állási csoportok használatának előfeltételei](virtual-machines-windows-portal-sql-availability-group-prereq.md). Ha az Előfeltételek már megadta, de is ugorhat túl[fürt létrehozása](#CreateCluster).


<!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Hello fürt létrehozása

Hello előfeltételeinek teljesítésekor, miután hello első lépése toocreate tartalmazó Windows Server feladatátvevő fürt két SQL-kiszolgálója és a tanúsító kiszolgálói.  

1. RDP toohello első SQL Server SQL Server-kiszolgálók és a hello tanúsító kiszolgáló egyik rendszergazdájának tartományi fiókkal.

   >[!TIP]
   >Ha követte hello [Előfeltételek dokumentumát](virtual-machines-windows-portal-sql-availability-group-prereq.md), nevű létrehozott **CORP\Install**. Használja ezt a fiókot.

2. A hello **Kiszolgálókezelő** jelölje be az irányítópult **eszközök**, és kattintson a **Feladatátvevőfürt-kezelő**.
3. Hello bal oldali ablaktáblában kattintson a jobb gombbal **Feladatátvevőfürt-kezelő**, és kattintson a **hozzon létre egy fürtöt**.
   ![Fürt létrehozása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. A fürt létrehozása varázsló hello, hozzon létre egy egy csomópontos fürtre lépéseit a következő táblázat hello hello beállításokkal hello lapok végrehajtani:

   | Lap | Beállítások |
   | --- | --- |
   | Előkészületek |Alapértelmezések használata |
   | Kiszolgálók kiválasztása |Első SQL-kiszolgáló neve a típus hello **kiszolgáló nevének megadása** kattintson **Hozzáadás**. |
   | Érvényesítési figyelmeztetés |Válassza ki **szám I nem igényel Microsoft-támogatást ehhez a fürthöz, és ezért nem kívánja toorun hello Érvényesítési tesztek. Ha a Tovább gombra kattintva folytatni annak létrehozását hello fürt**. |
   | Administering hello fürt hozzáférési pont |Írja be a fürt nevét, például **SQLAGCluster1** a **fürtnév**.|
   | Megerősítés |Használhatja az alapértelmezett értékeket, kivéve, ha a tárolóhelyek használata. Lásd a táblázat utáni megjegyzést hello. |

### <a name="set-hello-cluster-ip-address"></a>Hello fürt IP-cím beállítása

1. A **Feladatátvevőfürt-kezelő**, görgessen lefelé, túl**fürt alapvető erőforrásai** , és bontsa ki a hello fürt adatait. Mindkét hello kell megjelennie **neve** és hello **IP-cím** hello erőforrások **sikertelen** állapotát. hello IP-cím erőforrás nem kell online állapotba, mert hello fürt hozzá van rendelve a hello azonos IP-maga hello gépként cím, ezért a ismétlődő címet.

2. Nem sikerült, kattintson a jobb gombbal hello **IP-cím** erőforrás, és kattintson **tulajdonságok**.

   ![Fürt tulajdonságai](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Válassza ki **statikus IP-cím** , és adja meg egy címet alhálózatból hello SQL Server esetén hello cím szövegmezőbe. Kattintson a **OK**.
4. A hello **fürt alapvető erőforrásai** szakaszt, kattintson a jobb gombbal a fürt nevét, és kattintson **online állapotba hozás**. Ezután Várjon, amíg az mindkét erőforrás online. Hello fürt hálózatnév-erőforrás online állapotba kerül, amikor új AD-számítógépfiók frissíti hello tartományvezérlő kiszolgálóhoz. Az AD fiók toorun hello később a rendelkezésre állási csoport fürtözött szolgáltatást használja.

### <a name="addNode"></a>Adja hozzá más SQL Server toocluster hello

Adja hozzá más SQL Server toohello fürt hello.

1. A hello böngésző konzolfáján kattintson a jobb gombbal a hello fürt, majd kattintson **csomópont hozzáadása**.

    ![Fürt csomópont toohello hozzáadása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. A hello **csomópont hozzáadása varázsló**, kattintson a **következő**. A hello **kiszolgálók kiválasztása** lapon hello második SQL Server. Hello server típusnévnek **kiszolgáló nevének megadása** majd **Hozzáadás**. Amikor elkészült, kattintson a **következő**.

1. A hello **érvényesítési figyelmeztetés** kattintson **nem** (éles forgatókönyv esetében végre kell hajtania hello ellenőrző teszteket). Ezután kattintson a **Tovább** gombra.

8. A hello **megerősítő** lapon törölje a jelet hello jelölőnégyzetet a tárolóhelyek használata **adja hozzá az összes megfelelő tárolót toohello fürtöt.**

   ![Vegye fel a csomópont megerősítése](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Ha a tárolóhelyek használ, és nem, törölje a jelet **adja hozzá az összes megfelelő tárolót toohello fürtöt**, a Windows hello virtuális lemezek szervezőről hello fürtszolgáltatás folyamat során. Ennek eredményeképpen csak akkor jelennek meg a Lemezkezelés vagy Explorer hello tárolóhelyek hello fürt el lesznek távolítva, és objektumkörnyezetben a PowerShell használatával. A tárolóhelyek több lemez toostorage készletek felsorolását tartalmazza. További információkért lásd: [tárolóhelyek](https://technet.microsoft.com/library/hh831739).

1. Kattintson a **Tovább** gombra.

1. Kattintson a **Befejezés** gombra.

   Feladatátvevőfürt-kezelő jeleníti meg, hogy a fürt egy új csomóponttal és sorolja fel azokat hello **csomópontok** tároló.

10. Jelentkezzen ki a távoli asztali munkamenetgazda hello.

### <a name="add-a-cluster-quorum-file-share"></a>A fürt kvórum fájlmegosztás hozzáadása

Ebben a példában hello Windows fürt használja, a fájl megosztási toocreate a fürt kvórumát. Ez az oktatóanyag egy csomópont- és fájlmegosztástöbbség kvórum használja. További információkért lásd: [a feladatátvevő fürtök kvórumkonfigurációinak ismertetése](http://technet.microsoft.com/library/cc731739.aspx).

1. Csatlakozás toohello fájl megosztási tanúsító tagkiszolgáló egy távoli asztali munkamenetet.

1. A **Kiszolgálókezelő**, kattintson a **eszközök**. Nyissa meg **számítógép-kezelés**.

1. Kattintson a **megosztott mappák**.

1. Kattintson a jobb gombbal **megosztások**, és kattintson a **új megosztás...** .

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Használjon **megosztott mappa létrehozása varázsló** toocreate egy megosztást.

1. A **mappa elérési útja**, kattintson a **Tallózás** , és keresse meg, vagy hozzon létre egy hello megosztott mappa elérési útját. Kattintson a **Tovább** gombra.

1. A **nevét, leírását és beállítások** hello nevével és elérési ellenőrzése. Kattintson a **Tovább** gombra.

1. A **megosztott mappákra vonatkozó engedélyek** beállítása **engedélyek testreszabását**. Kattintson a **egyéni...** .

1. A **engedélyek testreszabása**, kattintson a **hozzáadása...** .

1. Győződjön meg arról, hogy hello használt fiók toocreate hello fürt teljes körű vezérléssel rendelkezik.

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. Kattintson az **OK** gombra.

1. A **megosztott mappákra vonatkozó engedélyek**, kattintson a **Befejezés**. Kattintson a **Befejezés** újra.  

1. Jelentkezzen ki hello kiszolgáló

### <a name="configure-cluster-quorum"></a>A fürtkvórum konfigurálása

Következő lépésként állítsa hello fürt kvóruma.

1. Csatlakozzon a távoli asztal toohello első fürtcsomópontra.

1. A **Feladatátvevőfürt-kezelő**, jobb gombbal az hello fürt túl**további műveletek**, és kattintson a **fürt kvórumbeállításainak megadása...** .

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. A **fürtkvórum beállítása varázslót**, kattintson a **következő**.

1. A **kvórumbeállítások kijelölése**, válassza a **hello kvórum tanúsítójának kijelölése**, és kattintson a **következő**.

1. A **kvórum Tanúsítójának kijelölése**, kattintson a **konfigurálása egy tanúsító fájlmegosztást**.

   >[!TIP]
   >Windows Server 2016 felhő tanúsító támogatja. Ha úgy dönt, hogy az ilyen típusú tanúsító, nem kell egy fájl tanúsító fájlmegosztás. További információkért lásd: [központi telepítése a feladatátvevő fürt tanúsító felhő](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). Ez az oktatóanyag egy tanúsító fájlmegosztást, amely korábbi operációs rendszerek által támogatott használja.

1. A **tanúsító fájlmegosztás**, létrehozott hello megosztás típus hello elérési útja. Kattintson a **Tovább** gombra.

1. Ellenőrizze, hogy hello beállítások **megerősítő**. Kattintson a **Tovább** gombra.

1. Kattintson a **Befejezés** gombra.

hello fürt alapvető erőforrásai vannak konfigurálva a tanúsító fájlmegosztást.

## <a name="enable-availability-groups"></a>Rendelkezésre állási csoportok engedélyezése

Következő lépésként engedélyezze az hello **AlwaysOn rendelkezésre állási csoportok** szolgáltatás. Hajtsa végre ezeket a lépéseket, a két SQL-kiszolgálón.

1. A hello **Start** indítsa el a jobb **SQL Server Configuration Manager**.
2. Hello böngésző konzolfáján kattintson **SQL Server Services**, majd kattintson a jobb gombbal a hello **SQL Server (MSSQLSERVER)** szolgáltatást, és kattintson a **tulajdonságok**.
3. Kattintson a hello **AlwaysOn magas rendelkezésre állású** lapra, majd válasszon **engedélyezze az AlwaysOn rendelkezésre állási csoportokat**, az alábbiak szerint:

    ![Engedélyezze az AlwaysOn rendelkezésre állási csoportok](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. Kattintson az **Alkalmaz** gombra. Kattintson a **OK** hello előugró párbeszédpanelen.

5. Hello SQL Server-szolgáltatás újraindításához.

Ismételje meg ezeket a lépéseket a hello más SQL-kiszolgáló.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for hello database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for hello instance of SQL Server that is used toosynchronize hello database replicas in hello Availability Groups on that instance.

On both SQL Servers, open hello firewall for hello TCP port for hello database mirroring endpoint.

1. On hello first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In hello left pane, select **Inbound Rules**. On hello right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For hello port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In hello **Action** page, keep **Allow hello connection** selected and click **Next**.
6. In hello **Profile** page, accept hello default settings and click **Next**.
7. In hello **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in hello **Name** text box, then click **Finish**.

Repeat these steps on hello second SQL Server.
-------------------------->

## <a name="create-a-database-on-hello-first-sql-server"></a>Adatbázis létrehozása a hello első SQL-kiszolgáló

1. Indítási hello RDP fájl toohello első SQL-kiszolgálót egy tartományi fiók, amely tagja a sysadmin (rendszergazda) rögzített kiszolgálói szerepkör.
1. Nyissa meg az SQL Server Management Studio eszközt, és csatlakozzon a toohello első SQL-kiszolgáló.
7. A **Object Explorer**, kattintson a jobb gombbal **adatbázisok** kattintson **új adatbázis**.
8. A **adatbázisnév**, típus **MyDB1**, majd kattintson a **OK**.

### <a name="backupshare"></a>Hozzon létre egy biztonsági mentési megosztást

1. Az első SQL-kiszolgálót hello **Kiszolgálókezelő**, kattintson a **eszközök**. Nyissa meg **számítógép-kezelés**.

1. Kattintson a **megosztott mappák**.

1. Kattintson a jobb gombbal **megosztások**, és kattintson a **új megosztás...** .

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Használjon **megosztott mappa létrehozása varázsló** toocreate egy megosztást.

1. A **mappa elérési útja**, kattintson a **Tallózás** , és keresse meg, vagy hozzon létre egy hello adatbázis biztonsági mentési megosztott mappa elérési útját. Kattintson a **Tovább** gombra.

1. A **nevét, leírását és beállítások** hello nevével és elérési ellenőrzése. Kattintson a **Tovább** gombra.

1. A **megosztott mappákra vonatkozó engedélyek** beállítása **engedélyek testreszabását**. Kattintson a **egyéni...** .

1. A **engedélyek testreszabása**, kattintson a **hozzáadása...** .

1. Győződjön meg arról, hogy hello SQL Server és SQL Server Agent szolgáltatásfiókokat mindkét kiszolgáló teljes hozzáférést.

   ![Új megosztás](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. Kattintson az **OK** gombra.

1. A **megosztott mappákra vonatkozó engedélyek**, kattintson a **Befejezés**. Kattintson a **Befejezés** újra.  

### <a name="take-a-full-backup-of-hello-database"></a>Teljes mentés hello adatbázis készítése

Hello új adatbázis tooinitialize hello naplólánca mentése tooback van szüksége. Ha nem ad meg hello új adatbázis biztonsági másolatát, akkor egy rendelkezésre állási csoportban nem szerepelhet.

1. A **Object Explorer**, jobb gombbal az hello adatbázis túl**feladatok...** , kattintson a **készítsen biztonsági másolatot**.

1. Kattintson a **OK** tootake a teljes biztonsági mentés toohello alapértelmezett biztonsági másolat helyét.

## <a name="create-hello-availability-group"></a>Hello rendelkezésre állási csoport létrehozása
Most már készen áll a rendelkezésre állási csoport használata a következő hello lépések tooconfigure áll:

* Adatbázis létrehozása a hello első SQL-kiszolgáló.
* Teljes biztonsági mentés és a tranzakciós napló biztonsági mentési hello adatbázis is igénybe vehet
* Visszaállítás hello teljes és a napló biztonsági mentések toohello hello az SQL Server második **NORECOVERY** beállítás
* Hello rendelkezésre állási csoport létrehozása (**AG1**) szinkron véglegesítésre, automatikus feladatátvétel és olvasható másodlagos másodpéldányokra

### <a name="create-hello-availability-group"></a>Hello rendelkezésre állási csoport létrehozása:

1. A távoli asztali munkamenetgazda toohello első SQL-kiszolgáló. A **Object Explorer** szolgáltatáshoz az ssms, kattintson a jobb gombbal **AlwaysOn magas rendelkezésre állású** kattintson **új rendelkezésre állási csoport varázsló**.

    ![Új rendelkezésre állási csoport varázsló indítása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. A hello **bemutatása** kattintson **következő**. A hello **adja meg a rendelkezésre állási csoport nevének** írja be például a rendelkezésre állási csoportban hello nevét **AG1**, a **rendelkezésre állási csoport nevének**. Kattintson a **Tovább** gombra.

    ![Új rendelkezésre állási csoport varázsló, adja meg a rendelkezésre állási csoport nevét](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. A hello **válasszon adatbázisok** lapon válassza ki az adatbázist, majd kattintson **következő**.

   >[!NOTE]
   >hello adatbázis egy rendelkezésre állási csoport hello előfeltételei megfelel, mert legalább egy teljes biztonsági mentést készített hello kívánt elsődleges replikán.

   ![Új rendelkezésre állási csoport varázsló, válassza ki az adatbázisokat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. A hello **replikák megadása** kattintson **adja hozzá a replika**.

   ![Új rendelkezésre állási csoport varázsló, adja meg a replikák](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. Hello **tooServer csatlakozás** párbeszédpanel jelenik meg. Második kiszolgáló hello hello típusnév **kiszolgálónév**. Kattintson a **Connect** (Csatlakozás) gombra.

   Vissza a hello **replikák megadása** lapon meg kell jelennie hello második kiszolgáló felsorolt **rendelkezésre állási másodpéldányok**. Hello replikákat az alábbiak szerint konfigurálhatja.

   ![Új rendelkezésre állási csoport varázsló, adja meg a replikák (kész)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Kattintson a **végpontok** toosee hello adatbázis-tükrözési végpontját a rendelkezésre állási csoport számára. Ugyanaz a hello beállításakor használt port használata hello [adatbázis-tükrözési végpont tűzfalszabályt](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Új rendelkezésre állási csoport varázsló kezdeti adatszinkronizálás kiválasztása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. A hello **kezdeti adatszinkronizálás kiválasztása** lapon, hogy melyik **teljes** , és adja meg egy megosztott hálózati helyre. A hello helyéhez, használja a hello [létrehozott biztonsági másolatokat tároló megosztáson](#backupshare). Hello példa volt, a **\\\\\<első SQL-kiszolgáló\>\Backup\**. Kattintson a **Tovább** gombra.

   >[!NOTE]
   >Teljes szinkronizálás teljes mentést készít hello hello első példányát az SQL Server-adatbázisba, és visszaállítja azt toohello második példányát. Nagy adatbázisok esetén a teljes szinkronizálás nem ajánlott, mert hosszú ideig is eltarthat. Manuálisan hello adatbázis biztonsági másolatát, és állítja vissza a megadott idő csökkentése érdekében `NO RECOVERY`. Ha hello adatbázis már vissza a `NO RECOVERY` hello az SQL Server rendelkezésre állási csoport hello konfigurálása előtt a második, válassza a **csak összekapcsolás**. Ha azt szeretné tootake hello biztonsági mentés hello rendelkezésre állási csoport konfigurálása után, **kezdeti adatszinkronizálás kihagyása**.

    ![Új rendelkezésre állási csoport varázsló kezdeti adatszinkronizálás kiválasztása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. A hello **érvényesítési** kattintson **következő**. Ezen a lapon a következő kép hasonló toohello kell kinéznie:

    ![Új rendelkezésre állási csoport varázsló, érvényesítése](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Mivel nem állított be egy rendelkezésre állási csoport figyelőjének, nincs hello figyelő konfigurációs vonatkozó figyelmeztetés. Mivel az Azure virtuális gépeken hello Azure terheléselosztó létrehozása után létrehozhat hello figyelő, kihagyhatja ezt a figyelmeztetést.

10. A hello **összegzés** kattintson **Befejezés**, akkor várjon, amíg a hello varázsló konfigurálja hello új rendelkezésre állási csoport. A hello **folyamatban** lap, kattintson **további részleteket** tooview hello részletes folyamatban van. Hello varázsló befejezése után vizsgálja meg a hello **eredmények** lap tooverify, amely hello rendelkezésre állási csoport sikeresen létrehozva.

     ![Új rendelkezésre állási csoport varázsló, annak az eredménye](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Kattintson a **Bezárás** tooexit hello varázsló.

### <a name="check-hello-availability-group"></a>Hello rendelkezésre állási csoport ellenőrzése

1. A **Object Explorer**, bontsa ki a **AlwaysOn magas rendelkezésre állású**, majd bontsa ki a **rendelkezésre állási csoportok**. Most látnia kell hello új rendelkezésre állási csoport ebben a tárolóban. Kattintson a jobb gombbal a hello rendelkezésre állási csoportot, és kattintson a **irányítópult megjelenítése**.

   ![Rendelkezésre állási csoport Irányítópult megjelenítése](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   A **AlwaysOn irányítópult** toothis hasonlóan kell kinéznie.

   ![Rendelkezésre állási csoport Irányítópult](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Hello replikák hello feladatátvételi mód az egyes replika- és hello szinkronizálási tekintheti meg.

2. A **Feladatátvevőfürt-kezelő**, kattintson arra a fürtre. Válassza ki **szerepkörök**. hello rendelkezésre állási csoport nevének használt hello fürtön a szerepkör megadása. Rendelkezésre állási csoport nem rendelkezik ügyfél-kommunikációhoz, IP-címet, mivel egy figyelő nincs konfigurálva. Miután létrehozta az Azure terheléselosztó hello figyelő konfigurálja.

   ![A Feladatátvevőfürt-kezelő AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Ne próbáljon toofail hello rendelkezésre állási csoport a Feladatátvevőfürt-kezelő hello keresztül. Az összes feladatátvételi műveleteket kell elvégezni belül **AlwaysOn irányítópult** szolgáltatáshoz az ssms. További információkért lásd: [Using korlátozásai hello Feladatátvevőfürt-kezelő rendelkezésre állási csoportokkal](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Ezen a ponton rendelkezik egy rendelkezésre állási csoporthoz az SQL Server két példánya replikával. Áthelyezheti hello rendelkezésre állási csoport példányai között. Toohello rendelkezésre állási csoport nem csatlakoztatható még, mert nem rendelkezik egy figyelő. Az Azure virtuális gépeken hello figyelő terheléselosztó szükséges. következő lépés hello toocreate hello terheléselosztó az Azure-ban.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Hozzon létre egy Azure terheléselosztó

Azure virtuális gépeken futó SQL Server rendelkezésre állási csoport terheléselosztó szükséges. hello terheléselosztó hello rendelkezésre állási csoport figyelőjének hello IP-címet tartalmazza. Ez a szakasz összefoglalja, hogyan toocreate hello terheléselosztó a hello Azure-portálon.

1. A hello Azure-portálon, válassza a toohello erőforráscsoport, ahol az SQL-kiszolgálók, és kattintson a **+ Hozzáadás**.
2. Keresse meg **terheléselosztó**. Válassza ki a Microsoft által kiadott hello terheléselosztóhoz.

   ![A Feladatátvevőfürt-kezelő AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  Kattintson a **Create** (Létrehozás) gombra.
3. A következő paraméterek hello terheléselosztóhoz hello konfigurálása.

   | Beállítás | Mező |
   | --- | --- |
   | **Name (Név)** |Szöveg esetében használható hello terheléselosztóhoz, például **sqlLB**. |
   | **Típus** |Belső |
   | **Virtuális hálózat** |Azure-beli virtuális hálózat hello hello nevét használja. |
   | **Alhálózat** |Hello név használata, amely a virtuális gép hello hello alhálózat szerepel.  |
   | **IP-cím hozzárendelése** |Statikus |
   | **IP-cím** |Egy alhálózatból címet használja. |
   | **Előfizetés** |Használja az azonos hello előfizetés hello virtuális gépként. |
   | **Hely** |Használjon hello azonos helyen hello virtuális gépként. |

   az Azure portál panel hello kell kinéznie:

   ![Terheléselosztó létrehozása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Kattintson a **létrehozása**, toocreate hello terheléselosztóhoz.

tooconfigure hello terheléselosztó kell toocreate háttérkészlet, a mintavételi és a set hello terheléselosztási szabályok. Hajtsa végre ezeket a hello Azure-portálon.

### <a name="add-backend-pool"></a>Háttérkészlet hozzáadása

1. A hello Azure-portálon válassza a tooyour rendelkezésre állási csoporthoz. Szükség lehet toorefresh hello nézet toosee az újonnan létrehozott hello terheléselosztóhoz.

   ![Terheléselosztó erőforráscsoportban található](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Kattintson a hello terheléselosztóhoz, majd **háttérkészletek**, és kattintson a **+ Hozzáadás**. Az alábbiak szerint állíthatja hello háttérkészlet:

   | Beállítás | Leírás | Példa
   | --- | --- |---
   | **Name (Név)** | Adjon meg egy szöveges nevet | SQLLBBE
   | **Társított** | Válassza ki a listából | Rendelkezésre állási csoport
   | **A rendelkezésre állási csoport** | Használjon, amelyek az SQL Server VMs hello rendelkezésre állási készlet nevét | sqlAvailabilitySet |
   | **Virtuális gépek** |a két hello Azure SQL Server virtuális gép neve | SQL Server-0, SQL Server-1

1. Írja be a hello háttérkészlethez hello nevét.

1. Kattintson a **+ adja hozzá a virtuális gépek**.

1. Hello rendelkezésre állási csoportot válassza a hello rendelkezésre állási csoportban, hogy az SQL-kiszolgálók hello.

1. A virtuális gépek tartalmazzák mind a hello SQL Server-kiszolgálók. Tanúsító fájlmegosztás hello fájlkiszolgáló nem tartalmaznak.

1. Kattintson a **OK** toocreate hello háttérkészlet.

### <a name="set-hello-probe"></a>Set hello mintavétel

1. Kattintson a hello terheléselosztóhoz, majd **állapot-mintavételi csomagjai**, és kattintson a **+ Hozzáadás**.

1. Az alábbiak szerint állíthatja hello állapotmintáihoz:

   | Beállítás | Leírás | Példa
   | --- | --- |---
   | **Name (Név)** | Szöveg | SQLAlwaysOnEndPointProbe |
   | **Protocol (Protokoll)** | Válassza ki a TCP | TCP |
   | **Port** | Minden nem használt portot | 59999 |
   | **Időköz**  | hello időn közötti mintavételi kísérletek másodpercben. |5 |
   | **Sérült küszöbérték** | hello egymást követő sikertelen mintavételek egy virtuális gép toobe megfelelő állapotúnak számít bekövetkező száma  | 2 |

1. Kattintson a **OK** tooset hello állapotmintáihoz.

### <a name="set-hello-load-balancing-rules"></a>Hello terheléselosztási szabályok beállítása

1. Kattintson a hello terheléselosztóhoz, majd **terheléselosztási szabályok**, és kattintson a **+ Hozzáadás**.

1. Hello terheléselosztási szabályok az alábbiak szerint állítsa be.
   | Beállítás | Leírás | Példa
   | --- | --- |---
   | **Name (Név)** | Szöveg | SQLAlwaysOnEndPointListener |
   | **Előtérbeli IP-cím** | Válasszon címet |Hello terheléselosztó létrehozásakor létrehozott hello címet használja. |
   | **Protocol (Protokoll)** | Válassza ki a TCP |TCP |
   | **Port** | Az SQL Server-példány hello hello port használatára | 1433 |
   | **Háttér-Port** | Ha a fix IP-Címek értéke a közvetlen kiszolgálói visszatérési nem használja ezt a mezőt | 1433 |
   | **Hálózatfigyelő** |hello mintavételhez megadott hello neve | SQLAlwaysOnEndPointProbe |
   | **Munkamenet megőrzését** | Legördülő lista | **Egyik sem** |
   | **Üresjárati időtúllépés** | Nyissa meg a percet tookeep a TCP-kapcsolat | 4 |
   | **Lebegőpontos IP (közvetlen kiszolgálói válasz)** | |Engedélyezve |

   > [!WARNING]
   > A közvetlen kiszolgálói válasz érték létrehozása során. Nem módosítható.

1. Kattintson a **OK** tooset hello terheléselosztási szabályok.

## <a name="configure-listener"></a>Hello figyelő konfigurálása

hello következő lépésként toodo tooconfigure egy rendelkezésre állási csoport figyelőjének hello feladatátvevő fürtön.

> [!NOTE]
> Ez az oktatóanyag bemutatja, hogyan toocreate egyetlen figyelő - egy ILB IP-cím. toocreate egy vagy több IP-címeket használ egy vagy több figyelői lásd [figyelő rendelkezésre állási csoport létrehozása és a terheléselosztó |} Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Set figyelő port

Az SQL Server Management Studio hello figyelő port beállítása.

1. Indítsa el az SQL Server Management Studio eszközt, és csatlakozzon a toohello elsődleges másodpéldány.

1. Keresse meg a túl**AlwaysOn magas rendelkezésre állású** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport figyelői**.

1. Most látnia kell a Feladatátvevőfürt-kezelő létrehozta hello figyelő nevét. Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson a **tulajdonságok**.

1. A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (1433 volt hello alapértelmezett), majd kattintson a **OK**.

Most már rendelkezik egy SQL Server rendelkezésre állási csoport erőforrás-kezelő módban futó Azure virtuális gépeken.

## <a name="test-connection-toolistener"></a>Teszt kapcsolat toolistener

tootest hello kapcsolat:

1. RDP tooa hello az SQL Server azonos virtuális hálózat, de nem saját hello replika. Használhat más SQL Server hello fürt hello.

1. Használjon **sqlcmd** segédprogram tootest hello kapcsolat. Például a következő parancsfájl hello hoz létre egy **sqlcmd** kapcsolat toohello elsődleges replika hello figyelő Windows-hitelesítés használatával:

    ```
    sqlcmd -S <listenerName> -E
    ```

    Ha hello figyelő eltérő portot használ hello alapértelmezett portot (1433), adja meg a hello portot hello kapcsolati karakterláncban. Például hello következő sqlcmd paranccsal összekapcsolja az tooa figyelési port 1435:

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

hello SQLCMD kapcsolat automatikusan csatlakozik a toowhichever példány SQL Server-gazdagépek hello elsődleges replika.

> [!TIP]
> Győződjön meg arról, hogy a megadott hello port a hello tűzfalat mindkét SQL Server nyitva-e. Mindkét kiszolgáló egy bejövő forgalomra vonatkozó szabály hello használt TCP-port szükséges. További információkért lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx).
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by hello way” info, an Important is info users need toocomplete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is hello second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note hello format for documenting hello UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult toomaintain. Highlight areas you are referring tooin red.*-->

<!--**No. of steps**: *Make sure hello number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want toogo on.*-->

## <a name="next-steps"></a>Következő lépések

- [Adja hozzá az IP cím tooa terheléselosztó egy második rendelkezésre állási csoport](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
