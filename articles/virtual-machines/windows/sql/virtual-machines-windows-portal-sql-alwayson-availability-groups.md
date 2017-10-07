---
title: "az Azure Resource Manager virtuális gépek magas rendelkezésre állás telepítése aaaSet |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toocreate Always On rendelkezésre állási csoport az Azure virtuális gépek az Azure Resource Manager módra."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: 6f0a253d3502259a487e66fd62d92e41c379a6b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Always On rendelkezésre állási csoportok Azure virtuális gépek automatikus konfigurálásához: erőforrás-kezelő

Az oktatóanyag bemutatja, hogyan toocreate egy SQL Server rendelkezésre állási csoportot, amelyben az Azure Resource Manager virtuális gépek. hello az oktatóanyag az Azure paneleken tooconfigure sablont használja. Tekintse át a hello alapértelmezett beállításokat, írja be a kívánt beállításokat, és hello paneleken hello portálon frissíti, az oktatóanyag ismerteti.

hello teljes oktatóanyag Azure virtuális gépeken, amelyek tartalmazzák a következő elemek hello SQL Server rendelkezésre állási csoportot hoz létre:

* Virtuális hálózat már több alhálózat működik, beleértve az olyan előtér- és a backend alhálózathoz
* Két tartományvezérlők, amelyeken az Active Directory-tartomány
* Két olyan virtuális gépet, amely SQL Server fut, illetve telepített toohello backend alhálózathoz és Active Directory-tartományhoz csatlakoztatott toohello
* Hello csomóponttöbbség kvórummodell három csomópontos feladatátvevő fürtön
* Két szinkron véglegesítésű másodpéldányt a rendelkezésre állási adatbázis egy rendelkezésre állási csoport

a következő ábra hello hello teljes megoldás jelöli.

![Tesztkörnyezet felépítése az Azure-ban rendelkezésre állási csoportokkal](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

A megoldás összes erőforrások tartoznak tooa egyetlen erőforráscsoportként működnek.

Ez az oktatóanyag megkezdése előtt ellenőrizze a hello következő:

* Már rendelkezik Azure-fiókkal. Ha még nincs fiókja, [regisztrálhat a próbafiókra](http://azure.microsoft.com/pricing/free-trial/).
* Már tudja, hogyan toouse hello grafikus felhasználói Felülettel tooprovision egy SQL Server virtuális gép hello virtuális gép gyűjteményből. További információkért lásd: [SQL Server Azure virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).
* Már van egy rendelkezésre állási csoportok alapos ismerete. További információkért lásd: [Always On rendelkezésre állási csoportok (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Ha érdekli a rendelkezésre állási csoportokat használ a SharePoint, [konfigurálja az SQL Server 2012 Always On rendelkezésre állási csoportok a SharePoint 2013 rendszerhez](http://technet.microsoft.com/library/jj715261.aspx).
>
>

Ebben az oktatóanyagban, használja a hello Azure portál számára:

* Hello mindig a sablonban válasszon hello portálról.
* Tekintse át a hello sablon beállításait, és a környezet néhány konfigurációs beállításainak frissítése.
* Mivel az hozza létre a teljes környezet hello Azure figyelése
* Csatlakoztassa tooa tartományvezérlőt, majd az SQL Server rendszert futtató kiszolgáló tooa.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-hello-cluster-from-hello-gallery"></a>Kiépítés hello fürt hello gyűjteményből
Azure hello teljes megoldás egy gyűjtemény lemezképet biztosít. toolocate hello sablonja esetében:

1. Jelentkezzen be toohello Azure-portál használatával a fiókját.
2. Hello Azure-portálon, kattintson **+ új** tooopen hello **új** panelen.
3. A hello **új** panelen, a Keresés **AlwaysOn**.
   ![AlwaysOn sablon keresése](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. A keresési eredmények hello, keresse meg a **SQL Server AlwaysOn fürt**.
   ![AlwaysOn sablon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. A **telepítési modell kiválasztása**, válassza a **erőforrás-kezelő**.

### <a name="basics"></a>Alapvető beállítások
Kattintson a **alapjai** és a következő beállítások hello konfigurálása:

* **Rendszergazdai felhasználónév** tartományi rendszergazdai jogosultságokkal rendelkező felhasználói fiók, és mind az SQL Server-példányokon hello SQL Server SysAdmin (rendszergazda) rögzített kiszolgálói szerepkör tagja. A jelen oktatóanyag esetében használja **tartománygazda**.
* **Jelszó** van hello hello tartományi rendszergazdai fiók jelszavát. Használjon erős jelszót hozzon létre. Hello jelszót.
* **Előfizetés** hello előfizetés, hogy a Azure toorun minden telepített erőforrások hello rendelkezésre állási csoport váltók stb. Ha a fiókja több előfizetéssel rendelkezik, megadhat egy másik előfizetést.
* **Erőforráscsoport** az összes Azure-sablon által létrehozott erőforrás tartozik hello csoport toowhich hello neve. A jelen oktatóanyag esetében használja **SQL-magas rendelkezésre ÁLLÁSÚ-RG**. További információk: [Azure Resource Manager overview](../../../azure-resource-manager/resource-group-overview.md#resource-groups) (Az Azure Resource Manager áttekintése).
* **Hely** van hello Azure-régió, ahol hello az oktatóanyagban létrehoz hello erőforrások. Válassza ki a kívánt Azure-régiót.

hello alábbi képernyőfelvételen egy befejezett **alapjai** panel:

![Alapvető beállítások](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

Kattintson az **OK** gombra.

### <a name="domain-and-network-settings"></a>Tartomány és a hálózati beállítások
Az Azure katalógusában sablont hoz létre a tartomány és a tartományvezérlők. A hálózat és két alhálózat is létrehoz. hello sablon nem hozható létre kiszolgálók egy meglévő tartományhoz vagy a virtuális hálózat. következő lépés hello hello tartományi és hálózati beállítások konfigurálása.

A hello **tartomány és a hálózati beállítások** panelt, tekintse át hello beállított hello tartományi és hálózati beállítások értékeit:

* **Erdő gyökértartományának a neve** hello Active Directory-tartomány hello neve állomások hello fürtön van. Hello az oktatóanyaghoz használja **contoso.com**.
* **Virtuális hálózat neve** az Azure-beli virtuális hálózat hello hello hálózati neve. Hello az oktatóanyaghoz használja **autohaVNET**.
* **Tartományvezérlő alhálózat neve** hello név egy részének hello virtuális hálózati állomások hello tartományvezérlő van. Használjon **alhálózat-1**. Ez az alhálózat címét előtag- **10.0.0.0/24**.
* **SQL Server alhálózati név** hello neve, hogy az SQL Server és hello fájl futtató gazdagépek hello kiszolgálók közös tanúsító hello virtuális hálózat része. Használjon **alhálózat-2**. Ez az alhálózat címét előtag- **10.0.1.0/26**.

További információ az Azure, a virtuális hálózatok toolearn lásd [virtuális hálózat áttekintése](../../../virtual-network/virtual-networks-overview.md).  

Hello **tartomány és a hálózati beállítások** kell hello kinézetét következő képernyőkép:

![Tartomány és a hálózati beállítások](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Ha szükséges, módosíthatja ezeket az értékeket. A jelen oktatóanyag esetében használja hello beállított értékek.

Tekintse át a hello beállításokat, majd **OK**.

### <a name="availability-group-settings"></a>Rendelkezésre állási csoport beállítások
A **rendelkezésre állási csoport beállítások**, tekintse át hello hello rendelkezésre állási csoport értékeit az adott néven beállítás és hello figyelő.

* **Rendelkezésre állási csoport nevének** hello rendelkezésre állási csoport hello fürtözött erőforrás neve. A jelen oktatóanyag esetében használja **Contoso-ag**.
* **Rendelkezésre állási csoport figyelőjének nevével** hello fürt és a belső terheléselosztó hello használják. TooSQL kiszolgálóhoz csatlakozó ügyfelek használható a név tooconnect toohello megfelelő hello adatbázis replikája. A jelen oktatóanyag esetében használja **Contoso-figyelő**.
* **Rendelkezésre állási csoport figyelőjének portszámára** hello TCP-portot az SQL Server-figyelő hello határozza meg. Ebben az oktatóanyagban hello alapértelmezett portot, használhat **1433**.

Ha szükséges, módosíthatja ezeket az értékeket. A jelen oktatóanyag esetében használja hello beállított értékek.  

![Rendelkezésre állási csoport beállítások](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

Kattintson az **OK** gombra.

### <a name="virtual-machine-size-storage-settings"></a>Virtuális gép méretét, a tárolási beállítások
A **Virtuálisgép-méretet, a tárolási beállítások**, egy SQL Server virtuális gép méretének kiválasztása, és tekintse át hello egyéb beállításokat.

* **SQL Server virtuálisgép-méret** hello mérete mindkét SQL Server rendszerű virtuális gépekhez. Válasszon egy megfelelő virtuális gép mérete, a munkaterhelés számára. Ha ebben a környezetben hello az oktatóanyaghoz, használja **DS2**. Termelési számítási feladatokhoz válassza ki a virtuális gép méretét, amely képes támogatni a hello munkaterhelés. Sok termelési számítási feladatokhoz szükséges **DS4** vagy nagyobb. hello sablon két virtuális gép ekkora alapszik, és az egyes SQL Server telepítése. További információkért lásd: [virtuális gépek méretei](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Az Azure által telepített hello az SQL Server Enterprise kiadása. hello költség hello edition és a virtuálisgép-méret hello függ. Aktuális költségekre vonatkozó részletes információkért lásd: [virtuális gépek díjszabása](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Tartomány a tartományvezérlő virtuálisgép-méret** hello virtuálisgép-méret hello tartományvezérlők van. Az oktatóanyag használatra **D2**.
* **A fájl tanúsító fájlmegosztás virtuális gép mérete** hello virtuálisgép-méret hello tanúsító fájlmegosztás van. A jelen oktatóanyag esetében használja **A1**.
* **SQL-Storage-fiók** hello storage-fiók, amely tárolja a hello SQL Server-adatokat és operációsrendszer-lemezek hello neve. A jelen oktatóanyag esetében használja **alwaysonsql01**.
* **DC tárfiók** az hello neve hello tárfiók hello tartományvezérlők. A jelen oktatóanyag esetében használja **alwaysondc01**.
* **SQL Server-adatok lemezméret** TB van hello SQL Server adatlemez TB hello méretét. Adjon meg egy 1 és 4 közötti számot. A jelen oktatóanyag esetében használja **1**.
* **Tárolási optimalizálása** megadja a bizonyos tárolási konfigurációs beállításokat hello SQL Server virtuális gépek hello munkaterhelés típusától függően. Ebben a forgatókönyvben szereplő összes SQL Server virtuális gép prémium szintű storage használata az Azure gazdagép lemezgyorsítótár csak tooread. Ezenkívül ezek a beállítások egyikének kiválasztásával hello munkaterheléshez tartozó SQL Server-beállítások is optimalizálhatja:

  * **Általános munkaterhelés** nincs konfigurációs beállításait.
  * **Tranzakciós feldolgozást** beállítása nyomkövetési jelző 1117 és 1118.
  * **Az adatraktározás terén** beállítása nyomkövetési jelző 1117 és 610.

A jelen oktatóanyag esetében használja **általános munkaterhelés**.

![Virtuális gép mérete tárolási beállításai](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Tekintse át a hello beállításokat, majd **OK**.

#### <a name="a-note-about-storage"></a>Tárolási Megjegyzés
További optimalizálást hello SQL Server adatlemezek hello méretétől függ. Minden egyes adatlemez terabájt Azure hozzáadja egy további 1 TB prémium szintű storage. Ha egy kiszolgáló 2 TB, vagy több, akkor az hello sablon egy tárolókészletet hoz létre minden egyes SQL Server virtuális gépen. Tárolókészlet a tárhely-virtualizálás egy formája, amelyben több lemezből is konfigurált tooprovide nagyobb kapacitást, a rugalmasság és a teljesítmény.  hello sablon majd hello tárolókészlet tárolóhelyet hoz létre, és megadja a egyetlen lemez toohello operációs rendszert. hello sablon jelöli meg a lemez adatlemez hello az SQL Server. hello sablon hello tárolókészlet beállítja az SQL Server hello-beállítások a következő használatával:

* Paritásos mérete hello interleave hello virtuális lemezre vonatkozó beállítás. Tranzakciós munkaterhelések 64 KB-os használják. Adatraktározás számítási feladatainál 256 KB-os használja.
* Rugalmasságra egyszerű (rugalmasság nélküli).

> [!NOTE]
> Prémium szintű Azure storage helyileg redundáns és tartja hello adatok három másolatot egyetlen régión belül, így nincs szükség további erősítése: hello tárolókészlet.
>
>

* Oszlopszám hello tárolókészletben lévő lemezek hello száma egyenlő.

Tárolóhely és a tárolókészletek kapcsolatos további információkért lásd:

* [Tárolóhelyek – áttekintés](http://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server biztonsági másolat és Tárolókészletek](http://technet.microsoft.com/library/dn390929.aspx)

SQL Server technológiának az ajánlott eljárásokkal kapcsolatos további információkért lásd: [teljesítmény a bevált gyakorlat az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server beállításai
A **SQL Server-beállítások**, tekintse át és hello SQL Server virtuális gép nevének előtagja, az SQL Server-verzió, a SQL Server-szolgáltatásfiók és a jelszó módosítása és hello SQL automatikus javítás karbantartási ütemtervet.

* **SQL Server neve előtag** használt toocreate van minden egyes SQL Server virtuális gép nevét. A jelen oktatóanyag esetében használja **sqlserver**. hello sablon neve hello SQL Server virtuális gépek *SQL Server-0* és *SQL Server-1*.
* **SQL Server-verzió** hello SQL Server verziója. Az oktatóanyag használatra **SQL Server 2014**. Másik lehetőségként **SQL Server 2012** vagy **SQL Server 2016**.
* **SQL Server szolgáltatásfiókjának felhasználóneve** hello tartományi fiók neve hello SQL Server szolgáltatás. A jelen oktatóanyag esetében használja **sqlservice**.
* **Jelszó** az SQL Server-szolgáltatásfióknak hello hello jelszóval.  Használjon erős jelszót hozzon létre. Hello jelszót.
* **SQL automatikus javítás karbantartási ütemterv** azonosítja, hogy az Azure automatikusan javításokkal hello SQL-kiszolgálók hello hét napja hello. Ebben az oktatóanyagban írja be a következőt **vasárnap**.
* **SQL automatikus javítás karbantartási start óra** van hello időpontot hello Azure-régió, az automatikus javítás megkezdésekor.

> [!NOTE]
> hello ablak minden egyes virtuális gép javítását a rendszer egy órával lépcsőzetes elrendezésben. Csak egy virtuális gép telepítve, a idő tooprevent szüneteltetése szolgáltatások.
>
>

![SQL Server beállításai](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Tekintse át a hello beállításokat, majd **OK**.

### <a name="summary"></a>Összefoglalás
Hello összefoglaló oldalon az Azure érvényesíti hello beállításait. Hello sablont is letöltheti. Tekintse át a hello összegzése. Kattintson az **OK** gombra.

### <a name="buy"></a>Vásárlás
A végső panel tartalmaz **használati feltételek**, és **adatvédelmi**. Tekintse át ezt az információt. Amikor készen áll az Azure toostart toocreate hello virtuális gépekhez, és minden egyéb szükséges erőforrások hello rendelkezésre állási csoport hello, kattintson **létrehozása**.

hello Azure-portálon hello erőforráscsoport és erőforrások hello hoz létre.

## <a name="monitor-deployment"></a>A figyelő telepítése
Hello központi telepítésének állapotáról hello Azure-portálon való figyeléséhez. Hello telepítési ábrázoló ikon automatikusan rögzített toohello Azure-portál irányítópultjának.

![Az Azure irányítópult](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-toosql-server"></a>Csatlakozás tooSQL kiszolgáló
SQL Server-példányok új hello internetkapcsolattal rendelkező IP-címmel rendelkező virtuális gépeken futnak. Távoli asztal (RDP) közvetlenül tooeach SQL Server virtuális gép is.

SQL Server tooRDP tooa kövesse az alábbi lépéseket:

1. Az Azure-portál irányítópultjának hello győződjön meg arról, hogy hello telepítése sikerült.
2. Kattintson a **erőforrások**.
3. A hello **erőforrások** panelen kattintson a **SQL Server-0**, ez az SQL Servert futtató hello virtuális gépek egyik hello számítógép nevét.
4. Hello panelen a **SQL Server-0**, kattintson a **Connect**. A böngésző kéri, ha azt szeretné tooopen, vagy mentse hello távoli kapcsolat objektumot. Kattintson a **nyitott**.
5. **Távoli asztali kapcsolat** jelezheti, hogy hello a távoli kapcsolat közzétevője nem azonosítható. Kattintson a **Connect** (Csatlakozás) gombra.
6. Windows biztonsági meg tooenter a hitelesítő adatok tooconnect toohello hello elsődleges tartományvezérlő IP-cím megadását kéri. Kattintson a **másik fiók használata**. A **felhasználónév**, típus **contoso\DomainAdmin**. Hello sablon hello rendszergazdai felhasználónév beállításakor konfigurálta ezt a fiókot. Hello összetett jelszót, hogy úgy döntött, hogy hello sablon konfigurálásakor használja.
7. **A távoli asztal** előfordulhat, hogy figyelmezteti a hello távoli számítógép nem hitelesíthető esedékes tooproblems biztonsági tanúsítvány. Jelzi, hogy hello biztonsági tanúsítvány neve. Ha követte az oktatóanyag hello, hello értéke **SQL Server-0.contoso.com**. Kattintson a **Igen**.

Most már csatlakoztatott RDP toohello SQL Server virtuális gép. Nyissa meg az SQL Server Management Studio eszközt, csatlakozzon az SQL Server alapértelmezett példányát toohello, és ellenőrizze, hogy hello rendelkezésre állási csoport van-e beállítva.
