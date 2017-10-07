---
title: "aaaConfigure Always On rendelkezésre állási csoport az Azure virtuális gépek (klasszikus) |} Microsoft Docs"
description: "Always On rendelkezésre állási csoport létrehozása az Azure virtuális gépek. Ez az oktatóanyag elsősorban hello felhasználói felület és eszközök helyett használja parancsfájlok."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: f428ad994a55378c281c959f4696fdcaf50632b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Always On rendelkezésre állási csoport konfigurálása az Azure virtuális gépek (klasszikus)
> [!div class="op_single_selector"]
> * [Klasszikus: felhasználói felület](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasszikus: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Mielőtt elkezdené, vegye figyelembe, hogy most hajthatja végre ezt a feladatot az Azure Resource Manager modellt. Azt javasoljuk, hogy az új központi telepítéseknél Azure Resource Manager modellt. Lásd: [SQL Server Always On rendelkezésre állási csoportok az Azure virtuális gépeken](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Azure két különböző üzembe helyezési modellek toocreate és az erőforrásokkal rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk azt ismerteti, hogyan toouse hello klasszikus üzembe helyezési modellben. 

toocomplete ezt a feladatot hello Azure Resource Manager modellt, lásd: [SQL Server Always On rendelkezésre állási csoportok az Azure virtuális gépeken](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

A végpont oktatóanyag bemutatja, hogyan tooimplement rendelkezésre állási csoportokat az SQL Server Always On Azure virtuális gépeken futó használatával.

Hello az oktatóanyag végén hello az SQL Server Always On megoldás az Azure-ban a következő elemek hello áll:

* Egy virtuális hálózatot, amely tartalmazza a több alhálózattal, és magában foglalja egy előtér- és a backend alhálózathoz
* Az Active Directory (Azure AD) tartományhoz rendelkező tartományvezérlő
* Két olyan virtuális gépet, amely SQL Server fut, illetve telepített toohello backend alhálózathoz és illesztett toohello az Azure AD-tartomány
* Hello csomóponttöbbség kvórummodell három csomópontos feladatátvevő fürtön
* Két szinkron véglegesítésű másodpéldányt a rendelkezésre állási adatbázis egy rendelkezésre állási csoport

a következő ábra hello hello megoldás grafikus ábrázolása.

![Tesztkörnyezet felépítése az Azure-ban rendelkezésre állási csoportokkal](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Vegye figyelembe, hogy ez a konfiguráció egy lehetséges. Például minimalizálhatja a két-replika rendelkezésre állási csoport a virtuális gépek hello száma. Ez a konfiguráció mentése az Azure számítási órák hello tartományvezérlő hello kvórum tanúsító fájlmegosztást egy két csomópontos fürt segítségével. Ez a módszer hello virtuális gépek száma csökkenti egy bemutatott hello konfigurációból.

Ez az oktatóanyag azt feltételezi, hogy a következő hello:

* Már rendelkezik Azure-fiókkal.
* Már tudja, hogyan toouse hello grafikus felhasználói Felülettel hello virtuális gép gyűjtemény tooprovision a hagyományos SQL Server rendszert futtató virtuális gép.
* Már van egy Always On rendelkezésre állási csoportok alapos ismerete. További információkért lásd: [Always On rendelkezésre állási csoportok (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Ha érdekli Always On rendelkezésre állási csoportok használata a SharePoint, [konfigurálása SQL Server 2012 Always On rendelkezésre állási csoportok a SharePoint 2013 rendszerhez](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-hello-virtual-network-and-domain-controller-server"></a>Hello virtuális hálózat és a tartományvezérlő kiszolgálót létrehozása
Új Azure próbafiókkal megkezdése. A fiók beállítása után a klasszikus Azure portálon hello hello kezdőképernyőn legyenek.

1. Kattintson a hello **új** gomb hello bal hello lap, ahogy az alábbi képernyőfelvétel a hello hello alsó sarkában.
   
    ![Kattintson az új hello portálon](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Kattintson a **hálózati szolgáltatások** > **virtuális hálózati** > **egyéni létrehozás**, ahogy az alábbi képernyőfelvétel a hello.
   
    ![Virtuális hálózat létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. A hello **virtuális hálózat létrehozása** párbeszédpanelen hozzon létre egy új virtuális hálózat lépésenkénti végrehajtás hello lapok és hello beállításokkal a következő táblázat hello. 
   
   | Lap | Beállítások |
   | --- | --- |
   | Virtuális hálózat adatai |**NAME = ContosoNET**<br/>**A régióban = USA nyugati régiója** |
   | DNS-kiszolgálók és a VPN-kapcsolat |None |
   | Virtuális hálózat |A következő képernyőkép hello beállítások láthatók: ![Virtuális hálózat létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Hello tartományvezérlővel (DC) használni kívánt hello virtuális gép létrehozása. Kattintson a **új** > **számítási** > **virtuális gép** > **a gyűjtemény**szerint a következő képernyőkép hello.
   
    ![Virtuális gép létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. A hello **A virtuális gép létrehozása** párbeszédpanelen adja meg egy új virtuális gép lépésenkénti végrehajtás hello lapok és hello beállításokkal a következő táblázat hello. 
   
   | Lap | Beállítások |
   | --- | --- |
   | Válassza ki a hello virtuális gép operációs rendszer |Windows Server 2012 R2 Datacenter |
   | Virtuálisgép-konfiguráció |**VERZIÓ Kiadás dátuma** = (legújabb)<br/>**VIRTUÁLIS gép neve** = ContosoDC<br/>**RÉTEG** = STANDARD<br/>**MÉRET** = A2 (2 mag)<br/>**ÚJ FELHASZNÁLÓNEVET** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Erősítse meg** = Contoso! 000 |
   | Virtuálisgép-konfiguráció |**A FELHŐALAPÚ szolgáltatás** = új felhőalapú szolgáltatás létrehozása<br/>**FELHŐALAPÚ szolgáltatás DNS-név** egy egyedi felhőszolgáltatás neve =<br/>**DNS-név** = egyedi nevét (pl.: ContosoDC123)<br/>**RÉGIÓ/AFFINITÁSCSOPORT/virtuális hálózati** = ContosoNET<br/>**VIRTUÁLIS hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**A TÁRFIÓK** = egy automatikusan létrehozott tárfiók használata<br/>**A rendelkezésre ÁLLÁSI csoport** = (nincs) |
   | Virtuális gép beállításai |Alapértelmezések használata |

Miután hello új virtuális gépet úgy konfigurál, várja meg a hello virtuális gép toobe provsioned. Ez a folyamat egyes idő toofinish vesz igénybe. Ha hello **virtuális gép** lapján hello klasszikus Azure portál, láthatja ContosoDC ciklus állapotok a **indítása (kiépítés)** túl**leállítva**, **Indítása**, **fut (kiépítés)**, és végül **futtató**.

most már sikeresen kiépítette hello tartományvezérlő kiszolgálóhoz. A következő konfigurál majd hello Active Directory-tartomány a tartományvezérlő kiszolgálón.

## <a name="configure-hello-domain-controller"></a>Hello tartományvezérlő konfigurálása
A lépéseket követve hello konfigurálja hello ContosoDC gépen Corp.contoso.com tartományvezérlőként működik.

1. Hello portálon, válassza ki a hello **ContosoDC** gép. A hello **irányítópult** lapra, majd **Connect** tooopen a távoli asztal (RDP) fájlt a távoli asztal eléréséhez.
   
    ![Csatlakoztassa a gépet tooVritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Jelentkezzen be a beállított rendszergazdai fiókkal (**\AzureAdmin**) és a jelszót (**Contoso! 000**).
3. Alapértelmezés szerint hello **Kiszolgálókezelő** irányítópult üzenetnek kell megjelennie.
4. Kattintson a hello **szerepkörök és szolgáltatások hozzáadása** hello irányítópult hivatkozásra kattintva.
   
    ![Server Explorer szerepkörök hozzáadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Kattintson a **következő** amíg elér toohello **kiszolgálói szerepkörök** szakasz.
6. Jelölje be hello **Active Directory tartományi szolgáltatások** és **DNS-kiszolgáló** szerepkörök. Amikor a rendszer kéri, adja hozzá ezeket a szerepköröket igénylő szolgáltatásokat.
   
   > [!NOTE]
   > Egy érvényesítési figyelmeztetés, hogy nincs-e statikus IP-cím fog megjelenni. Ha hello konfigurációs teszteli, kattintson a **Folytatás**. Éles környezetben [PowerShell tooset hello statikus IP-cím hello tartomány a tartományvezérlő számítógép](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Adja hozzá a szerepkörök párbeszédpanel](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Kattintson a **következő** hello addig **megerősítő** szakasz. Jelölje be hello **újraindítás hello célkiszolgáló szükség esetén automatikusan** jelölőnégyzetet.
8. Kattintson az **Install** (Telepítés) gombra.
9. Hello szolgáltatások telepítése után térjen vissza a toohello **Kiszolgálókezelő** irányítópult.
10. Jelölje be új hello **Active Directory tartományi szolgáltatások** beállítás hello bal oldali ablaktáblán.
11. Kattintson a hello **további** hello sárga figyelmeztető sáv hivatkozásra kattintva.
    
     ![Az Active Directory tartományi szolgáltatások párbeszédpanel DNS-kiszolgáló virtuális gépen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. A hello **művelet** hello oszlopa **összes kiszolgáló feladat részletei** párbeszédpanel, kattintson a **a kiszolgáló tooa tartományvezérlő előléptetéséhez**.
13. A hello **Active Directory tartományi szolgáltatások konfigurációs varázslója**, a következő értékek hello használata:
    
    | Lap | Beállítás |
    | --- | --- |
    | Központi telepítés konfigurálása |**Új erdő hozzáadása** kijelölt =<br/>**Gyökértartomány neve** = corp.contoso.com |
    | Tartományvezérlő beállításai |**Jelszó** = Contoso! 000<br/>**Jelszó megerősítése** = Contoso! 000 |
14. Kattintson a **következő** keresztül toogo hello más hello varázsló lapjain. A hello **szükséges előfeltételek ellenőrzése** lapon, győződjön meg arról, hogy megjelenik-e a következő üzenet hello: **az előfeltétel-ellenőrzés sikeresen lefutott**. Ne feledje, hogy tekintse át az összes alkalmazható figyelmeztető üzenetek, de lehetséges toocontinue hello telepítés.
15. Kattintson az **Install** (Telepítés) gombra. Hello **ContosoDC** virtuális gép automatikusan újraindul.

## <a name="configure-domain-accounts"></a>Tartományi fiókok konfigurálása
hello lépések hello Active Directory-fiókok későbbi használatra konfigurálja.

1. Jelentkezzen be ismét a toohello **ContosoDC** gép.
2. A **Kiszolgálókezelő**, kattintson a **eszközök** > **Active Directory felügyeleti központ**.
   
    ![Active Directory felügyeleti központ](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. A hello **Active Directory felügyeleti központ**, jelölje be **corp (helyi)** hello bal oldali ablaktáblán.
4. A jobb oldali hello **feladatok** ablaktáblán kattintson a **új** > **felhasználói**. A következő beállítások hello használata:
   
   | Beállítás | Érték |
   | --- | --- |
   | **Utónév** |Telepítés |
   | **Felhasználó SamAccountName** |Telepítés |
   | **Jelszó** |Contoso! 000 |
   | **Jelszó megerősítése** |Contoso! 000 |
   | **Egyéb jelszóbeállítások** |Kiválasztva |
   | **Jelszó sohasem jár le** |Bejelölve |
5. Kattintson a **OK** toocreate hello **telepítése** felhasználó. Ehhez a fiókhoz használt tooconfigure hello feladatátvevő fürt és hello rendelkezésre állási csoport lesz.
6. Hozzon létre két további felhasználó **CORP\SQLSvc1** és **CORP\SQLSvc2**, a hello ugyanazokat a lépéseket. Ezek a fiókok használandó hello SQL Server-példányokat. A következő lépésben toogive **CORP\Install** hello a szükséges engedélyek tooconfigure Windows feladatátvételi fürtszolgáltatás.
7. A hello **Active Directory felügyeleti központ**, kattintson a **corp (helyi)** hello bal oldali ablaktáblán. A hello **feladatok** ablaktáblán kattintson a **tulajdonságok**.
   
    ![CORP felhasználó tulajdonságai](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Válassza ki **bővítmények**, majd kattintson a hello **speciális** hello gombjára **biztonsági** lapon.
9. A hello **speciális biztonsági beállítások vállalati** párbeszédpanel, kattintson a **Hozzáadás**.
10. Kattintson a **rendszerbiztonsági tag kiválasztása**, keressen **CORP\Install**, és kattintson a **OK**.
11. Jelölje be hello **az összes tulajdonság olvasása** és **számítógép-objektumok létrehozása** engedélyek.
    
     ![Corp felhasználói engedélyek](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Kattintson a **OK**, és kattintson a **OK** újra. Bezárás hello corp tulajdonságai ablakban.

Most, hogy konfigurálta az Active Directory és a hello felhasználói objektumok, három SQL Server virtuális gépek létrehozása, és csatlakoztassa azokat toothis tartomány.

## <a name="create-hello-sql-server-virtual-machines"></a>Hello SQL Server virtuális gépek létrehozása
Hozzon létre három virtuális gépet. Még a fürt összes csomópontjára vonatkozóan, és az SQL Server kettő. toocreate minden hello virtuális gépek, nyissa meg a klasszikus Azure portálon toohello biztonsági, kattintson a **új** > **számítási** > **virtuális gép**  >  **Gyűjteményből**. Ezt követően használható hello sablonok a következő tábla toohelp hello virtuális gépek létrehozása hello.

| Lap | VM1 | VM2 VIRTUÁLIS GÉPNEK | VM3 |
| --- | --- | --- | --- |
| Válassza ki a hello virtuális gép operációs rendszer |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Virtuálisgép-konfiguráció |**VERZIÓ Kiadás dátuma** = (legújabb)<br/>**VIRTUÁLIS gép neve** = ContosoWSFCNode<br/>**RÉTEG** = STANDARD<br/>**MÉRET** = A2 (2 mag)<br/>**ÚJ FELHASZNÁLÓNEVET** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Erősítse meg** = Contoso! 000 |**VERZIÓ Kiadás dátuma** = (legújabb)<br/>**VIRTUÁLIS gép neve** = ContosoSQL1<br/>**RÉTEG** = STANDARD<br/>**MÉRET** = A3 (4 mag)<br/>**ÚJ FELHASZNÁLÓNEVET** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Erősítse meg** = Contoso! 000 |**VERZIÓ Kiadás dátuma** = (legújabb)<br/>**VIRTUÁLIS gép neve** = ContosoSQL2<br/>**RÉTEG** = STANDARD<br/>**MÉRET** = A3 (4 mag)<br/>**ÚJ FELHASZNÁLÓNEVET** = AzureAdmin<br/>**Új jelszó** = Contoso! 000<br/>**Erősítse meg** = Contoso! 000 |
| Virtuálisgép-konfiguráció |**A FELHŐALAPÚ szolgáltatás** = a korábban létrehozott egyedi felhőalapú szolgáltatás DNS-név (pl.: ContosoDC123)<br/>**RÉGIÓ/AFFINITÁSCSOPORT/virtuális hálózati** = ContosoNET<br/>**VIRTUÁLIS hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**A TÁRFIÓK** = egy automatikusan létrehozott tárfiók használata<br/>**Rendelkezésre ÁLLÁSI csoport** = létrehozása rendelkezésre állási beállítása<br/>**RENDELKEZÉSRE ÁLLÁSI KÉSZLET NEVE** = SQLHADR |**A FELHŐALAPÚ szolgáltatás** = a korábban létrehozott egyedi felhőalapú szolgáltatás DNS-név (pl.: ContosoDC123)<br/>**RÉGIÓ/AFFINITÁSCSOPORT/virtuális hálózati** = ContosoNET<br/>**VIRTUÁLIS hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**A TÁRFIÓK** = egy automatikusan létrehozott tárfiók használata<br/>**Rendelkezésre ÁLLÁSI csoport** = SQLHADR (beállíthatja úgy is hello a rendelkezésre állási csoportban hello gép létrehozása után. Mind a három gépet rendelhető toohello SQLHADR rendelkezésre állási csoportot.) |**A FELHŐALAPÚ szolgáltatás** = a korábban létrehozott egyedi felhőalapú szolgáltatás DNS-név (pl.: ContosoDC123)<br/>**RÉGIÓ/AFFINITÁSCSOPORT/virtuális hálózati** = ContosoNET<br/>**VIRTUÁLIS hálózati ALHÁLÓZAT** = Back(10.10.2.0/24)<br/>**A TÁRFIÓK** = egy automatikusan létrehozott tárfiók használata<br/>**Rendelkezésre ÁLLÁSI csoport** = SQLHADR (beállíthatja úgy is hello a rendelkezésre állási csoportban hello gép létrehozása után. Mind a három gépet rendelhető toohello SQLHADR rendelkezésre állási csoportot.) |
| Virtuális gép beállításai |Alapértelmezések használata |Alapértelmezések használata |Alapértelmezések használata |

<br/>

> [!NOTE]
> hello előző konfigurációs szabványos réteg virtuális gépein, javasol, mert az ALAPSZINTŰ csomag gépek nem támogatják az elosztott terhelésű végpont. Elosztott terhelésű végpont kell újabb toocreate egy rendelkezésre állási csoport figyelőjét. Emellett itt közölt hello méreteket célja, Azure virtuális gépek rendelkezésre állási csoportok teszteléshez. Hello termelési számítási feladatokhoz a legjobb teljesítmény érdekében hello ajánlott méreteket SQL Server és a konfiguráció megtekintéséhez [teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépek](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Miután hello három virtuális gépek teljesen kiépített, toojoin kell őket toohello **corp.contoso.com** tartomány- és támogatási CORP\Install rendszergazdai jogosultságokkal toohello gépek. toodo a, a lépéseket követve hello három virtuális gépek mindegyikének használata hello.

1. Adjon hello előnyben részesített DNS-kiszolgáló címét. Minden virtuális gép RDP tooyour helyi könyvtárát letöltése jelölje ki a virtuális gép hello hello listában, kattintson a hello **Connect** gombra. egy virtuális gép tooselect kattintson bárhova hello első cella hello sorban, de látható módon a következő képernyőkép hello.
   
    ![Hello RDP-fájl letöltése](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Nyissa meg hello RDP letöltött fájlra, és jelentkezzen be toohello virtuális gép a konfigurált rendszergazdai fiókkal (**BUILTIN\AzureAdmin**) és a jelszót (**Contoso! 000**).
3. Miután bejelentkezik, megtekintheti az hello **Kiszolgálókezelő** irányítópult. Kattintson a **helyi kiszolgáló** hello bal oldali ablaktáblán.
4. Kattintson a hello **IPv4-cím IPv6 engedélyezett DHCP által hozzárendelt** hivatkozásra.
5. A hello **hálózati kapcsolatok** párbeszédpanelen kattintson az hello található hálózat ikonra.
   
    ![Változás hello virtuális gép előnyben részesített DNS-kiszolgáló](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. A hello parancssávon kattintson **módosítani a kapcsolat beállításainak hello**. (Attól függően, hogy a Windows hello méretét, lehetséges, hogy tooclick hello dupla jobbra mutató nyílra toosee Ez a parancs).
7. Válassza ki **Internet Protocol Version 4 (TCP/IPv4)**, és kattintson a **tulajdonságok**.
8. Válassza ki **a következő DNS-kiszolgálócímek használata hello** , majd adja meg **10.10.2.4** a **elsődleges DNS-kiszolgáló**.
9. Hello **10.10.2.4** hello cím, amely hozzárendelt tooa virtuális gép hello 10.10.2.0/24 alhálózat egy Azure virtuális hálózatban. A virtuális gép **ContosoDC**. tooverify **ContosoDC**tartozó IP-cím használata **nslookup contosodc** hello parancssori ablakban, ahogy az alábbi képernyőfelvétel a hello.
   
    ![Tartományvezérlő NSLOOKUP toofind IP-cím használata](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Kattintson a **OK** > **Bezárás** toocommit hello módosításokat. Most csatlakozhat hello virtuális gép túl**corp.contoso.com**.
11. Vissza a hello **helyi kiszolgáló** ablakban kattintson hello **munkacsoport** hivatkozásra.
12. A hello **számítógépnév** kattintson **módosítása**.
13. Jelölje be hello **tartomány** jelölőnégyzetet, írja be **corp.contoso.com** hello szövegmezőbe, és kattintson a **OK**.
14. A hello **Windows biztonsági** párbeszédpanelen adja meg a hello hello alapértelmezett tartományi rendszergazdai fiók hitelesítő adatait (**CORP\AzureAdmin**) és hello jelszó (**Contoso! 000**).
15. Ha hello "Üdvözli toohello corp.contoso.com tartomány" üzenet jelenik meg, kattintson **OK**.
16. Kattintson a **Bezárás** > **Újraindítás most** hello párbeszédpanelen.

### <a name="add-hello-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Hello Corp\Install felhasználó hozzáadása rendszergazdaként minden egyes virtuális gépen
1. Várjon, amíg az hello virtuális gép újraindul, és ezután nyissa meg hello RDP fájl újra toosign toohello virtuális gépen hello segítségével **BUILTIN\AzureAdmin** fiók.
2. A **Kiszolgálókezelő** kattintson **eszközök** > **számítógép-kezelés**.
   
    ![Számítógép-kezelés](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. A hello **számítógép-kezelés** párbeszédpanelen bontsa ki **helyi felhasználók és csoportok**, és kattintson a **csoportok**.
4. Kattintson duplán a hello **rendszergazdák** csoport.
5. A hello **rendszergazdák tulajdonságok** párbeszédpanelen kattintson hello **Hozzáadás** gomb.
6. Adja meg a hello felhasználói **CORP\Install**, és kattintson a **OK**. Amikor a rendszer kéri a hitelesítő adatokat, használja a hello **AzureAdmin** hello fiókot **Contoso! 000** jelszót.
7. Kattintson a **OK** tooclose hello **rendszergazda tulajdonságainak** párbeszédpanel megnyitásához.

### <a name="add-hello-failover-clustering-feature-tooeach-virtual-machine"></a>Hello feladatátvételi fürtszolgáltatás funkció tooeach virtuális gépek hozzáadása
1. A hello **Kiszolgálókezelő** irányítópultján kattintson **szerepkörök és szolgáltatások hozzáadása**.
2. A hello **hozzáadása szerepkörök és szolgáltatások varázsló**, kattintson a **következő** amíg elér toohello **szolgáltatások** lap.
3. Válassza ki **feladatátvételi fürtszolgáltatás**. Amikor a rendszer kéri, adja hozzá az egyéb szolgáltatások.
   
    ![Feladatátvételi fürtszolgáltatás toovirtual gép hozzáadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Kattintson a **következő**, és kattintson a **telepítése** a hello **megerősítő** lap.
5. Ha hello **feladatátvételi fürtszolgáltatás** szolgáltatás telepítése befejeződött, kattintson a **Bezárás**.
6. Jelentkezzen ki hello virtuális gépet.
7. Hello mind a három kiszolgáló ebben a szakaszban található lépéseket: **ContosoWSFCNode**, **ContosoSQL1**, és **ContosoSQL2**.

SQL Server virtuális gépek most már telepített és futó hello, de mindegyik rendelkezik-e az SQL Server hello alapértelmezett beállítások.

## <a name="create-hello-failover-cluster"></a>Hello feladatátvevő fürt létrehozása
Ebben a szakaszban hello rendelkezésre állási csoport, amely a későbbiekben létrehozandó futtató hello feladatátvevő fürtöt hoz létre. Mostanra elvégezte kell hello tooeach hello feladatátvevő fürtben használni kívánt hello három virtuális gépek a következő:

* Teljesen kiosztott hello virtuális gépet az Azure-ban
* Hello virtuális gép toohello tartományhoz csatlakozott
* Hozzáadott **CORP\Install** toohello helyi Rendszergazdák csoport
* A hozzáadott hello Feladatátvételi fürtszolgáltatást

Minden olyan minden egyes virtuális gépen Előfeltételek toohello feladatátvevő fürt is csatlakoztatása előtt.

Emellett vegye figyelembe, hogy hello Azure-beli virtuális hálózat nem működik hello ugyanaz, mint egy helyszíni hálózat. A sorrend hello toocreate hello fürt lesz szüksége:

1. Hozzon létre egy egycsomópontos fürt egyik csomópontján (**ContosoSQL1**).
2. Hello fürt IP-cím tooan módosítása nem használt IP-cím (**10.10.2.101**).
3. Hello fürtnév online állapotba.
4. Adja hozzá más csomópontok hello (**ContosoSQL2** és **ContosoWSFCNode**).

A következő lépéseket toocomplete hello feladatok hello fürt beállításának hello használata.

1. Nyissa meg hello RDP-fájljának **ContosoSQL1**, és jelentkezzen be tartományi fiókkal hello **CORP\Install**.
2. A hello **Kiszolgálókezelő** irányítópultján kattintson **eszközök** > **Feladatátvevőfürt-kezelő**.
3. Hello bal oldali ablaktáblában kattintson a jobb gombbal **Feladatátvevőfürt-kezelő**, és kattintson a **hozzon létre egy fürtöt**, ahogy az alábbi képernyőfelvétel a hello.
   
    ![Fürt létrehozása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. A fürt létrehozása varázsló hello, hozzon létre egy egy csomópontos fürtre hello lapok lépésenkénti végrehajtás hello beállításokkal a következő táblázat hello:
   
   | Lap | Beállítások |
   | --- | --- |
   | Előkészületek |Alapértelmezések használata |
   | Kiszolgálók kiválasztása |Típus **ContosoSQL1** a **kiszolgáló nevének megadása** kattintson **hozzáadása** |
   | Érvényesítési figyelmeztetés |Válassza ki **szám I nem igényel Microsoft-támogatást ehhez a fürthöz, és ezért nem kívánja toorun hello Érvényesítési tesztek. Ha a Tovább gombra kattintva folytatni annak létrehozását hello fürt**. |
   | Administering hello fürt hozzáférési pont |Típus **Cluster1** a **a fürt neve** |
   | Megerősítés |Használhatja az alapértelmezett értékeket, kivéve, ha a tárolóhelyek használata. Lásd a táblázatot követő hello figyelmeztetés. |
   
   > [!WARNING]
   > Használata [tárolóhelyek](https://technet.microsoft.com/library/hh831739), amely több lemezre csoportok tárolókészletekbe, törölnie kell a hello **adja hozzá az összes megfelelő tárolót toohello fürtöt** hello jelölőnégyzet **megerősítése** lap. Ne törölje ezt a beállítást, ha a virtuális lemezek hello hello fürtszolgáltatás folyamat során választható le. Emiatt azok fog is csak akkor jelennek meg a lemezkezelő vagy Explorer hello tárolóhelyek hello fürt távolítva, és PowerShell használatával objektumkörnyezetben.
   > 
   > 
5. Hello bal oldali ablaktáblán bontsa ki a **Feladatátvevőfürt-kezelő**, és kattintson a **Cluster1.corp.contoso.com**.
6. Hello center panelen görgessen lefelé toohello **fürt alapvető erőforrásai** szakaszt, és bontsa ki a hello **Name: Clutser1** részleteit. Mindkét hello kell megjelennie **neve** és hello **IP-cím** hello erőforrások **sikertelen** állapotát. hello IP-cím erőforrás nem hozható online állapotba, mert hello fürt hozzá van rendelve hello IP-cím hello gépként, ez az ismétlődő címet.
7. Nem sikerült, kattintson a jobb gombbal hello **IP-cím** erőforrás, és kattintson **tulajdonságok**.
   
    ![Fürt tulajdonságai](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Válassza ki **statikus IP-cím**, adja meg **10.10.2.101** a hello **cím** szövegmezőbe, és kattintson **OK**.
9. A hello **fürt alapvető erőforrásai** területen kattintson a jobb gombbal **Name: Cluster1**, és kattintson a **online állapotba hozás**. Várjon, amíg mindkét erőforrás online. Hello fürt hálózatnév-erőforrás online állapotba kerül, ha egy új Active Directory-számítógépfiókja hello tartományvezérlő kiszolgálóhoz frissül. Az Active Directory-fiókkal használható toorun hello rendelkezésre állási csoport fürtözött szolgáltatás később.
10. Adja hozzá a fennmaradó csomópontok toohello fürt hello. A hello böngésző konzolfáján kattintson a jobb gombbal **Cluster1.corp.contoso.com**, és kattintson a **csomópont hozzáadása**, ahogy az alábbi képernyőfelvétel a hello.
    
     ![Csomópont toohello fürt hozzáadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. A hello **csomópont hozzáadása varázsló**, kattintson a **következő** a hello **kiszolgálók kiválasztása** lapon **ContosoSQL2** és **ContosoWSFCNode**  toohello listán írja be a kiszolgálónevet hello **kiszolgáló nevének megadása** , majd **Hozzáadás**. Amikor elkészült, kattintson a **következő**.
12. A hello **érvényesítési figyelmeztetés** kattintson **nem**, bár a egy éles telepítési forgatókönyvhöz, végre kell hajtania hello ellenőrző tesztek. Ezután kattintson a **Tovább** gombra.
13. A hello **megerősítő** kattintson **következő** tooadd hello csomópontok.
    
    > [!WARNING]
    > Ha használ [tárolóhelyek](https://technet.microsoft.com/library/hh831739), amely több lemezre csoportok tárolókészletekbe, törölnie kell a hello **adja hozzá az összes megfelelő tárolót toohello fürtöt** jelölőnégyzetet. Ne törölje ezt a beállítást, ha a virtuális lemezek hello hello fürtszolgáltatás folyamat során választható le. Ennek eredményeképpen fog is csak akkor jelennek meg lemezkezelő vagy Explorer hello tárolóhelyek fürt el lesznek távolítva, és objektumkörnyezetben a PowerShell használatával.
    > 
    > 
14. Miután hello csomópontokat ad hozzá toohello fürt, kattintson **Befejezés**. Feladatátvevőfürt-kezelő most meg kell jelennie, hogy a fürt három csomópontja rendelkezik-e, és rendezze őket a hello **csomópontok** tároló.
15. A távoli asztali munkamenetgazda hello kijelentkezni.

## <a name="prepare-hello-sql-server-instances-for-availability-groups"></a>Hello SQL Server-példány előkészítése rendelkezésre állási csoportok
Ebben a szakaszban ennek hello mind a következő **ContosoSQL1** és **contosoSQL2**:

* Adja hozzá a bejelentkezési adatait a következő **NT AUTHORITY\System** szükséges engedélyekkel rendelkező toohello alapértelmezett SQL Server-példány beállítása.
* Adja hozzá **CORP\Install** SysAdmin (rendszergazda) szerepkör toohello alapértelmezett SQL Server példányt.
* Nyissa meg az SQL Server távoli hozzáféréshez hello tűzfalat.
* Hello Always On rendelkezésre állási csoportok szolgáltatás engedélyezése.
* Hello SQL Server-szolgáltatásfiók módosítása túl**CORP\SQLSvc1** és **CORP\SQLSvc2**, illetve.

Bármilyen sorrendben ezeket a műveleteket végezheti el. Ettől függetlenül lépések hello haladhat révén azokat sorrendben. Hello lépésekkel mindkét **ContosoSQL1** és **ContosoSQL2**:

1. Ha a távoli asztali munkamenetgazda hello hello virtuális gép nem kijelentkezett, tegye meg.
2. Nyissa meg hello RDP-fájlokat **ContosoSQL1** és **ContosoSQL2**, és jelentkezzen be a **BUILTIN\AzureAdmin**.
3. Adja hozzá **NT AUTHORITY\System** toohello SQL Server bejelentkezési szükséges engedélyekkel. Nyissa meg az **SQL Server Management Studiót**.
4. Kattintson a **Connect** tooconnect toohello alapértelmezett SQL Server-példány.
5. A **Object Explorer**, bontsa ki a **biztonsági**, majd bontsa ki a **bejelentkezések**.
6. Kattintson a jobb gombbal hello **NT AUTHORITY\System** bejelentkezési, és kattintson **tulajdonságok**.
7. A hello **Securables** hello helyi kiszolgáló esetén válassza a lap **Grant** a hello az alábbi engedélyek használata, és kattintson a **OK**.
   
   * Az ALTER bármely rendelkezésre állási csoport
   * Csatlakozás SQL
   * Kiszolgáló állapotának megtekintése
8. Adja hozzá **CORP\Install** , egy **sysadmin** szerepkör toohello alapértelmezett SQL Server-példány. A **Object Explorer**, kattintson a jobb gombbal **bejelentkezések**, és kattintson a **új bejelentkezés**.
9. Típus **CORP\Install** a **bejelentkezési név**.
10. A hello **kiszolgálói szerepkörök** lapon jelölje be **sysadmin**, és kattintson a **OK**. Hello bejelentkezés létrehozása után megjelenik az kibontásával **bejelentkezések** a **Object Explorer**.
11. egy tűzfalszabály az SQL Server hello toocreate **Start** nyissa meg **fokozott biztonságú Windows tűzfal**.
12. Hello bal oldali ablaktáblában jelöljön ki **bejövő szabályok**. Hello jobb oldali ablaktáblában kattintson **új szabály**.
13. A hello **szabálytípus** kattintson **Program** > **következő**.
14. A hello **Program** lapon jelölje be **Ez a program elérési útja**, típus **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** hello szövegmezőbe, és kattintson a **következő**. Ha ezt az útmutatót követve, de a SQL Server 2012, SQL Server-könyvtár hello van **MSSQL11. MSSQLSERVER**.
15. A hello **művelet** lapon, tartsa **hello csatlakozás engedélyezése** kiválasztva, és kattintson **következő**.
16. A hello **profil** lapon fogadja el hello alapértelmezett beállításokat, és kattintson a **következő**.
17. A hello **neve** lapján adja meg a szabály nevét, például a **SQL Server (Program szabály)**, a hello **neve** szöveg mezőbe, majd kattintson **Befejezés**.
18. tooenable hello **Always On rendelkezésre állási csoportok** funkciót, hello **Start** nyissa meg **SQL Server Configuration Manager**.
19. Hello böngésző konzolfáján kattintson **SQL Server Services**, kattintson a jobb gombbal hello **SQL Server (MSSQLSERVER)** szolgáltatásra, és kattintson a **tulajdonságok**.
20. Hello kattintson **magas rendelkezésre állású mindig a** lapon jelölje be **engedélyezze az Always On rendelkezésre állási csoportok**, ahogy hello következő képernyőkép, és kattintson a **alkalmaz**. Kattintson a **OK** hello párbeszédpanel, és ne zárja be az hello **tulajdonságok** még a párbeszédpanel bezárásához. SQL Server szolgáltatás hello hello szolgáltatásfiók módosítása után újraindul.
    
     ![A rendelkezésre állási csoportok mindig engedélyezése](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. toochange hello SQL Server szolgáltatási fiókja, kattintson a hello **bejelentkezés** fülre, írja be **CORP\SQLSvc1** (a **ContosoSQL1**) vagy **CORP\SQLSvc2** () a **ContosoSQL2**) a **fióknév**, töltse ki és hello jelszót, és kattintson **OK**.
22. Hello párbeszédpanel, kattintson **Igen** toorestart hello SQL Server szolgáltatás. Hello SQL Server-szolgáltatás újraindítása után módosítja, hogy elvégezték az hello **tulajdonságok** párbeszédpanel érvényben.
23. Jelentkezzen ki hello virtuális gépek.

## <a name="create-hello-availability-group"></a>Hello rendelkezésre állási csoport létrehozása
Most már áll készen tooconfigure egy rendelkezésre állási csoporthoz. Alább mit fog röviden van:

* Hozzon létre egy új adatbázist (**MyDB1**) a **ContosoSQL1**.
* Teljes biztonsági mentés és a tranzakciós napló biztonsági mentési hello adatbázis is igénybe vehet.
* Állítsa vissza a teljes hello és naplófájl túl a biztonsági mentések**ContosoSQL2** a hello **NORECOVERY** lehetőséget.
* Hello rendelkezésre állási csoport létrehozása (**AG1**) szinkron véglegesítésre, automatikus feladatátvétel és olvasható másodlagos másodpéldányokra.

### <a name="create-hello-mydb1-database-on-contososql1"></a>ContosoSQL1 hello MyDB1 adatbázis létrehozásához.
1. Ha nem már kijelentkezett hello távoli asztali munkamenetek a **ContosoSQL1** és **ContosoSQL2**, most tegye meg.
2. Nyissa meg hello RDP-fájljának **ContosoSQL1**, és jelentkezzen be a **CORP\Install**.
3. A **Fájlkezelőben**a **C:\\**, hozzon létre egy könyvtárat nevű **biztonsági mentési**. A könyvtár tooback elhasználja fog, és visszaállítani az adatbázist.
4. Jobb gombbal az új könyvtár hello túl**megosztása**, és kattintson a **adott személyek**, ahogy az alábbi képernyőfelvétel a hello.
   
    ![Hozzon létre egy biztonsági mentési mappát](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Adja hozzá **CORP\SQLSvc1**, majd adjon hello **olvasási/írási** engedéllyel. Adja hozzá **CORP\SQLSvc2**, és hello adjon **olvasási** engedéllyel, ahogy az hello következő képernyőkép, és kattintson a **megosztás**. Hello fájlmegosztási folyamat befejezése után kattintson **végzett**.
   
    ![Engedélyeket a biztonsági mentési mappája](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. toocreate hello adatbázis, a hello **Start** menü megnyitása **SQL Server Management Studio**, és kattintson a **Connect** tooconnect toohello alapértelmezett SQL Server-példány.
7. A **Object Explorer**, kattintson a jobb gombbal **adatbázisok**, és kattintson a **új adatbázis**.
8. A **adatbázisnév**, típus **MyDB1**, és kattintson a **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Készítsen teljes biztonsági másolatot az MyDB1, majd állítsa vissza azt a ContosoSQL2
1. hello egy teljes biztonsági mentést az adatbázis toomake **Object Explorer**, bontsa ki a **adatbázisok**, kattintson a jobb gombbal **MyDB1**, pont túl**feladatok**, és Kattintson a **készítsen biztonsági másolatot**.
2. A hello **forrás** területen tartsa **biztonsági másolat típusa** túl beállítása**teljes**. A hello **cél** kattintson **eltávolítása** tooremove hello alapértelmezett elérési út hello biztonságimásolat-fájl.
3. A hello **cél** kattintson **Hozzáadás**.
4. A hello **Fájlnév** szövegmezőben  **\\ContosoSQL1\backup\MyDB1.bak**, kattintson a **OK**, és kattintson a **OK** újra hello adatbázis tooback. Hello biztonsági mentési művelet befejezése után kattintson **OK** újra tooclose hello párbeszédpanel.
5. egy tranzakció toomake hello adatbázis biztonsági másolatának jelentkezzen be **Object Explorer**, bontsa ki a **adatbázisok**, kattintson a jobb gombbal **MyDB1**, pont túl**feladatok**, és kattintson a **készítsen biztonsági másolatot**.
6. A **biztonsági másolat típusa**, jelölje be **tranzakciónapló**. Tartsa hello **cél** egy korábban megadott elérési út set toohello fájlt, és kattintson a **OK**. Hello biztonsági mentési művelet befejezése után kattintson **OK** újra.
7. teljes toorestore hello és a tranzakciós biztonsági mentések jelentkezzen be **ContosoSQL2**, nyissa meg a fájl hello RDP **ContosoSQL2**, és jelentkezzen be a **CORP\Install**. Hagyja hello a távoli asztali munkamenetgazda, a **ContosoSQL1** megnyitásához.
8. A hello **Start** menü megnyitása **SQL Server Management Studio**, és kattintson a **Connect** tooconnect toohello alapértelmezett SQL Server-példány.
9. A **Object Explorer**, kattintson a jobb gombbal **adatbázisok**, és kattintson a **Restore Database**.
10. A hello **forrás** szakaszban jelölje be **eszköz**, és hello három pont gombra **...** gombra.
11. A **válassza ki a biztonsági mentési eszközöket**, kattintson a **Hozzáadás**.
12. A **biztonsági másolat fájl helye**, típus  **\\ContosoSQL1\backup**, kattintson a **frissítése**, jelölje be **MyDB1.bak**, kattintson **OK**, és kattintson a **OK** újra. Most látnia kell hello teljes biztonsági mentés és hello napló biztonsági mentését a hello **biztonságimásolat-készletet toorestore** ablaktáblán.
13. Nyissa meg toohello **beállítások** lapon jelölje be **RESTORE WITH NORECOVERY** a **helyreállítási állapota**, és kattintson a **OK** toorestore hello adatbázis. Hello után állítsa vissza a művelet befejeződik, kattintson a **OK**.

### <a name="create-hello-availability-group"></a>Hello rendelkezésre állási csoport létrehozása
1. Nyissa meg toohello a távoli asztali munkamenetgazda, a biztonsági **ContosoSQL1**. A **Object Explorer** az SQL Server Management Studio, kattintson a jobb gombbal **magas rendelkezésre állású mindig a**, és kattintson a **új rendelkezésre állási csoport varázsló**hello szerint következő képernyőkép.
   
    ![Új rendelkezésre állási csoport létrehozása varázsló elindítása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. A hello **bemutatása** kattintson **következő**. A hello **adja meg a rendelkezésre állási csoport nevének** írja be **AG1** a **rendelkezésre állási csoport nevének**, majd kattintson a **következő** újra.
   
    ![Új rendelkezésre állási csoport varázsló, a rendelkezésre állási csoport nevének megadása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. A hello **válasszon adatbázisok** lapon, válassza ki **MyDB1**, és kattintson a **következő**. hello adatbázis egy rendelkezésre állási csoport hello előfeltételei megfelel, mert legalább egy teljes biztonsági mentést készített hello kívánt elsődleges replikán.
   
    ![Új Availabilty csoport varázsló, jelölje be adatbázisok](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. a hello **replikák megadása** kattintson **adja hozzá a replika**.
   
    ![Új Availabilty csoport varázsló, adjon meg replikák](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. A hello **tooServer csatlakozás** párbeszédpanelen írja be **ContosoSQL2** a **kiszolgálónév**, és kattintson a **Connect**.
   
    ![Új Availabilty csoport varázsló, csatlakozás tooserver](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Vissza a hello **replikák megadása** lapon meg kell jelennie **ContosoSQL2** felsorolt **elérhető replikák**. Ahogy az alábbi képernyőfelvétel a hello hello replikák konfigurálása Ha elkészült, kattintson a **következő**.
   
    ![Új Availabilty csoport varázsló, adjon meg replikák (kész)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. A hello **kezdeti adatszinkronizálás kiválasztása** lapon, hogy melyik **csak összekapcsolás**, és kattintson a **következő**. Ön már végrehajtotta a adatszinkronizálás kézzel történő hello teljes és a tranzakciós biztonsági mentések **ContosoSQL1** és vissza őket **ContosoSQL2**. Nem tooperform hello választhat az adatbázis biztonsági mentési és visszaállítási műveleteket, és ehelyett válassza **teljes** toolet hello új rendelkezésre állási csoport varázsló adatszinkronizálás végezheti el. Azonban nem ajánlott ezt a beállítást, az egyes vállalatoknál nagyon nagy méretű adatbázisokhoz.
   
    ![Új Availabilty csoport varázsló, a kezdeti adatszinkronizálás kiválasztása](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. A hello **érvényesítési** kattintson **következő**. Ezen a lapon a következő képernyőkép hasonló toohello kell kinéznie. Mivel nem állított be egy rendelkezésre állási csoport figyelőjének, nincs hello figyelő konfigurációs vonatkozó figyelmeztetés. Figyelmen kívül hagyhatja ezt a figyelmeztetést, mert ez az oktatóanyag nem konfigurálja a figyelőt. Miután elvégezte az oktatóanyag, lásd: tooconfigure hello figyelő [egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-int-listener.md).
   
    ![Új Availabilty csoport varázsló érvényesítése](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. A hello **összegzés** kattintson **Befejezés**, majd várjon, amíg a hello varázsló konfigurálja a hello új rendelkezésre állási csoport. A hello **folyamatban** lap, kattintson **további részleteket** tooview hello részletes folyamatban van. Hello varázsló befejezése után vizsgálja meg a hello **eredmények** lap tooverify hello rendelkezésre állási csoport sikeresen létrejött, ahogy az alábbi képernyőfelvétel a hello, és kattintson **Bezárás** tooexit hello varázsló.
   
    ![Új Availabilty csoport varázsló, eredmények](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. A **Object Explorer**, bontsa ki a **magas rendelkezésre állású mindig a**, majd bontsa ki a **rendelkezésre állási csoportok**. Meg kell jelennie hello új rendelkezésre állási csoport ebben a tárolóban. Kattintson a jobb gombbal **AG1 (elsődleges)**, és kattintson a **irányítópult megjelenítése**.
    
     ![Rendelkezésre állási csoport megjelenítése irányítópult](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. A **mindig irányítópult** kell megjelenése hasonló toohello egy hello a következő képernyőkép. Hello replikákat, hello feladatátvételi mód minden egyes replikát, és hello szinkronizációs állapota látható.
    
     ![Rendelkezésre állási csoport Irányítópult](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Térjen vissza a túl**Kiszolgálókezelő**, kattintson a **eszközök**, majd nyissa meg **Feladatátvevőfürt-kezelő**.
13. Bontsa ki a **Cluster1.corp.contoso.com**, majd bontsa ki a **szolgáltatások és alkalmazások**. Válassza ki **szerepkörök** és vegye figyelembe, hogy hello **AG1** rendelkezésre állási csoport szerepkör létrejött. Ne feledje, hogy AG1 nem egy IP-címet, mely adatbázis által az ügyfélszámítógépek csatlakozhatnak toohello rendelkezésre állási csoport, mert nincs konfigurálva egy figyelő. Csatlakozhat közvetlenül toohello elsődleges csomópont olvasási és írási műveletek és hello másodlagos csomópont csak olvasható lekérdezések.
    
     ![A Feladatátvevőfürt-kezelő AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Ne próbálja meg a Feladatátvevőfürt-kezelő hello hello rendelkezésre állási csoport toofail. Az összes feladatátvételi műveleteket kell elvégezni belül **mindig irányítópult** az SQL Server Management Studio. További információkért lásd: [Using korlátozásai hello Feladatátvevőfürt-kezelő rendelkezésre állási csoportokkal](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Következő lépések
Most már sikeresen megvalósítását SQL Server Always On rendelkezésre állási csoport létrehozása az Azure-ban. a rendelkezésre állási csoport egy figyelő tooconfigure lásd [egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-int-listener.md).

Egyéb Azure-ban az SQL Server használatával kapcsolatos információkért lásd: [SQL Server Azure virtuális gépeken](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

