---
title: "Kiszolgáló rendelkezésre állása aaaSQL csoportok – az Azure virtual machines - Előfeltételek |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan tooconfigure hello Előfeltételek létrehozásához egy SQL Server Always On rendelkezésre állási csoport Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: eed0729ead25c7793bb17a04cd7fd996c7dc8c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="complete-hello-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Always On rendelkezésre állási csoportok létrehozása az Azure virtuális gépeken teljes hello előfeltételei

Ez az oktatóanyag bemutatja, hogyan toocomplete hello létrehozására vonatkozó Előfeltételek a [SQL Server Always On rendelkezésre állási csoport az Azure virtuális gépek (VM)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). Ha befejezte a hello Előfeltételek, vannak egy tartományvezérlő, két SQL Server virtuális gépen és a tanúsító kiszolgálói egyetlen erőforráscsoportként működnek.

**Becsült idő**: eltarthat néhány óra múlva toocomplete hello előfeltételek. Ezen idő nagy töltött, virtuális gépek létrehozását.

hello következő diagram azt ábrázolja hello oktatóanyag felépítéséhez.

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Tekintse át a rendelkezésre állási csoport dokumentáció

Ez az oktatóanyag feltételezi, hogy rendelkezik-e az SQL Server Always On rendelkezésre állási csoportokkal kapcsolatos alapvető ismeretekkel. Ha még nem ismeri ezt a technológiát, lásd: [az mindig a rendelkezésre állási csoportok áttekintése (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Azure-fiók létrehozása
Rendelkeznie kell Azure-fiókkal. Is [nyisson meg egy ingyenes Azure-fiók](/pricing/free-trial/?WT.mc_id=A261C142F) vagy [aktiválhatja a Visual Studio előfizetői előnyeit](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
1. Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com).
2. Kattintson a  **+**  toocreate hello portál új objektumot.

   ![Új objektum](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Típus **erőforráscsoport** a hello **piactér** keresési ablak.

   ![Erőforráscsoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Kattintson a **erőforráscsoport**.
5. Kattintson a **Create** (Létrehozás) gombra.
6. A hello **erőforráscsoport** panelen, a **erőforráscsoport-név**, írja be az hello erőforráscsoport nevét. Írja be például **sql-magas rendelkezésre állású-rg**.
7. Ha több Azure-előfizetéssel rendelkezik, győződjön meg arról, hogy hello előfizetés hello Azure-előfizetést, amelyet szeretne toocreate hello rendelkezésre állási csoport.
8. Válasszon ki egy helyet. hello hello Azure-régió, ahol azt szeretné, hogy toocreate hello rendelkezésre állási csoport helye. Ebben az oktatóanyagban az oktatóanyagban módosítjuk toobuild egy Azure-beli hely összes erőforrása.
9. Ellenőrizze, hogy **PIN-kód toodashboard** be van jelölve. A választható beállítás hello Azure-portál irányítópultjának hello erőforráscsoport parancsikont helyez el.

   ![Erőforráscsoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Kattintson a **létrehozása** toocreate hello erőforráscsoportot.

Azure erőforráscsoport hello és PIN-kód egy csoportot hoz létre helyi toohello erőforrás hello portálon.

## <a name="create-hello-network-and-subnets"></a>Hello hálózati és alhálózatok létrehozásával
következő lépés hello toocreate hello hálózatok és alhálózatok hello Azure erőforráscsoport.

hello megoldás egy virtuális hálózatot használ, két alhálózattal. Hello [virtuális hálózat áttekintése](../../../virtual-network/virtual-networks-overview.md) hálózatok az Azure-ban további információt nyújt.

toocreate hello virtuális hálózat:

1. Kattintson az erőforráscsoportban, Azure-portálon hello **+ Hozzáadás**. Az Azure megnyílik hello **mindent** panelen.

   ![Új elem](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Keresse meg **virtuális hálózati**.

     ![Keresési virtuális hálózat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Kattintson a **virtuális hálózati**.
4. A hello **virtuális hálózati** panelen kattintson hello **erőforrás-kezelő** üzembe helyezési modellt, és kattintson **létrehozása**.

    hello következő táblázatban hello beállításai hello virtuális hálózathoz:

   | **Mező** | Érték |
   | --- | --- |
   | **Name (Név)** |autoHAVNET |
   | **Címtér** |10.33.0.0/24 |
   | **Alhálózat neve** |Rendszergazda |
   | **Alhálózati címtartomány** |10.33.0.0/29 |
   | **Előfizetés** |Adja meg, hogy szeretné-e toouse hello előfizetés. **Előfizetés** üres, ha csak egy előfizetéssel rendelkezik. |
   | **Erőforráscsoport** |Válasszon **meglévő** , és válassza ki a hello hello erőforráscsoport nevét. |
   | **Hely** |Adja meg az Azure-beli hely hello. |

   A cím és az alhálózati címtartományt hello táblából eltérő lehet. Attól függően, hogy az előfizetés hello portal javasol, az elérhető és a megfelelő alhálózati címtartományt. Ha nincs elegendő címtérrel érhető el, használjon másik előfizetést.

   hello példa használ hello alhálózati név **Admin**. Ez az alhálózat hello tartományvezérlők van.

5. Kattintson a **Create** (Létrehozás) gombra.

   ![Hello virtuális hálózat konfigurálása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure portál Irányítópultjára toohello adja vissza, és értesíti, amikor új hello-hálózatot hoznak létre.

### <a name="create-a-second-subnet"></a>Hozzon létre egy második alhálózatot
hello új virtuális hálózat rendelkezik egy alhálózatot, nevű **Admin**. hello tartományvezérlők ezt használni. hello SQL Server virtuális gépen használni, a második nevű **SQL**. tooconfigure az alhálózaton:

1. Az irányítópulton, kattintson a létrehozott erőforráscsoport hello **SQL-magas rendelkezésre ÁLLÁSÚ-RG**. Keresse meg a hello hálózati hello erőforráscsoportban alatt **erőforrások**.

    Ha **SQL-magas rendelkezésre ÁLLÁSÚ-RG** nem látható, megkereshetjük **erőforráscsoportok** és hello erőforráscsoport-név szerinti szűrését.
2. Kattintson a **autoHAVNET** hello az erőforrások listájához a. Azure hello hálózati konfiguráció paneljének megnyitása.
3. A hello **autoHAVNET** virtuális hálózat panel alatt **beállítások** , kattintson a **alhálózatok**.

    Megjegyzés: hello alhálózatot, amelyet már hozott létre.

   ![Hello virtuális hálózat konfigurálása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. Hozzon létre egy második alhálózatot. Kattintson a **+ alhálózati**.
6. A hello **alhálózat hozzáadása** panelen hello alhálózati konfigurálásához írja be a **sqlsubnet** alatt **neve**. Azure automatikusan megadja egy érvényes **-címtartományt**. Ellenőrizze, hogy a címtartomány legalább 10 cím szerepel. Éles környezetben több címet lehet szükség.
7. Kattintson az **OK** gombra.

    ![Hello virtuális hálózat konfigurálása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

a következő táblázat hello hello hálózati konfigurációs beállításokat foglalja össze:

| **Mező** | Érték |
| --- | --- |
| **Name (Név)** |**autoHAVNET** |
| **Címtér** |Ez az érték hello elérhető címterek az előfizetésében függ. Jellemző értéke 10.0.0.0/16. |
| **Alhálózat neve** |**rendszergazda** |
| **Alhálózati címtartomány** |Ezt az értéket az előfizetés hello érhető el-címtartományokat függ. Jellemző értéke: 10.0.0.0/24. |
| **Alhálózat neve** |**sqlsubnet** |
| **Alhálózati címtartomány** |Ezt az értéket az előfizetés hello érhető el-címtartományokat függ. Jellemző értéke 10.0.1.0/24. |
| **Előfizetés** |Adja meg, hogy szeretné-e toouse hello előfizetés. |
| **Erőforráscsoport** |**SQL-MAGAS RENDELKEZÉSRE ÁLLÁSÚ-RG-N** |
| **Hely** |Adja meg ugyanazon a helyen hello erőforráscsoport kiválasztott hello. |

## <a name="create-availability-sets"></a>Rendelkezésre állási csoportok létrehozása

Virtuális gépek létrehozása előtt toocreate rendelkezésre állási csoportokra szüksége. Rendelkezésre állási készletek csökkentik a hello leállásának elvégzett tervezett vagy nem tervezett karbantartási események. Egy Azure rendelkezésre állási csoportok pedig a logikai csoport, amely a fizikai tartalék tartományok és a frissítési tartományok Azure-erőforrások. A tartalék tartomány biztosítja, hogy hello rendelkezésre állási csoport tagjai hello külön energia- és hálózati erőforrásokat. Egy frissítési tartományt biztosítja, hogy hello rendelkezésre állási csoport tagjai nem állapotba: hello karbantartás azonos idő. További információkért lásd: [hello virtuális gépek rendelkezésre állásának kezelése](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Két rendelkezésre állási csoportokra van szüksége. Egyik hello tartományvezérlők. hello második van hello SQL Server virtuális gépekre vonatkozóan.

rendelkezésre állási beállítása nyissa toohello erőforráscsoport és kattintson az toocreate **Hozzáadás**. Írja be a hello eredmények szűréséhez **rendelkezésre állási csoport**. Kattintson a **rendelkezésre állási csoport** a hello eredményeit, és kattintson a **létrehozása**.

Két rendelkezésre állási csoportok szerint toohello paraméterek a következő táblázat hello konfigurálása:

| **Mező** | Tartomány a tartományvezérlő a rendelkezésre állási csoport | SQL Server rendelkezésre állási csoport |
| --- | --- | --- |
| **Name (Név)** |adavailabilityset |sqlavailabilityset |
| **Erőforráscsoport** |SQL-MAGAS RENDELKEZÉSRE ÁLLÁSÚ-RG-N |SQL-MAGAS RENDELKEZÉSRE ÁLLÁSÚ-RG-N |
| **Tartalék tartományok** |3 |3 |
| **Tartományok frissítése** |5 |3 |

Hello rendelkezésre állási csoportok létrehozása után adja vissza toohello erőforráscsoport hello Azure-portálon.

## <a name="create-domain-controllers"></a>Tartományvezérlők létrehozása
Miután létrehozta a hello hálózati, alhálózatok, rendelkezésre állási készletek és egy internetre irányuló terheléselosztót, most készen áll a toocreate hello virtuális gépek hello tartományvezérlők.

### <a name="create-virtual-machines-for-hello-domain-controllers"></a>Hozzon létre a virtuális gépek hello tartományvezérlők
toocreate és hello tartományvezérlők konfigurálása, a toohello **SQL-magas rendelkezésre ÁLLÁSÚ-RG** erőforráscsoportot.

1. Kattintson az **Add** (Hozzáadás) parancsra. Hello **mindent** panel nyílik meg.
2. Típus **Windows Server 2016 Datacenter**.
3. Kattintson a **Windows Server 2016 Datacenter**. A hello **Windows Server 2016 Datacenter** panelen, győződjön meg arról, hogy hello telepítési modell **erőforrás-kezelő**, és kattintson a **létrehozása**. Az Azure megnyílik hello **hozzon létre virtuális gépet** panelen.

Ismételje meg a lépéseket toocreate két virtuális gép megelőző hello. Hello két virtuális gép neve:

* Active Directory-elsődleges-tartományvezérlők
* Active Directory-másodlagos-tartományvezérlők

  > [!NOTE]
  > Hello **ad – másodlagos-tartományvezérlő** virtuális gép nem kötelező, tooprovide a magas rendelkezésre állás, az Active Directory tartományi szolgáltatásokhoz.
  >
  >

hello következő táblázatban a két gépek hello beállításait:

| **Mező** | Érték |
| --- | --- |
| **Name (Név)** |Első tartományvezérlő: *ad-elsődleges-dc*.</br>Második tartományvezérlő *ad – másodlagos-tartományvezérlő*. |
| **Virtuális merevlemez típusa** |SSD |
| **Felhasználónév** |Tartományi rendszergazdai |
| **Jelszó** |Contoso! 0000 |
| **Előfizetés** |*Az Ön előfizetése* |
| **Erőforráscsoport** |SQL-MAGAS RENDELKEZÉSRE ÁLLÁSÚ-RG-N |
| **Hely** |*A hely* |
| **Méret** |DS1_V2 |
| **Tárolás** | **Felügyelt lemezeket használó** - **Igen** |
| **Virtuális hálózat** |autoHAVNET |
| **Alhálózat** |Rendszergazda |
| **Nyilvános IP-cím** |*Virtuális gép hello azonos néven* |
| **Hálózati biztonsági csoport** |*Virtuális gép hello azonos néven* |
| **A rendelkezésre állási csoport** |adavailabilityset </br>**Tartományok fault**: 2. régiója</br>**Tartományok frissítése**: 2. régiója|
| **Diagnosztika** |Engedélyezve |
| **Diagnosztikai tárfiók** |*Automatikusan létrehozott* |

   >[!IMPORTANT]
   >Csak egy virtuális gép elhelyezheti rendelkezésre állási készlet létrehozásakor. Hello rendelkezésre állási csoportot a virtuális gép létrehozása után nem módosítható. Lásd: [hello virtuális gépek rendelkezésre állásának kezelése](../manage-availability.md).

Azure virtuális gépek hello hoz létre.

Miután hello virtuális gépek jönnek létre, konfigurálja a hello tartományvezérlő.

### <a name="configure-hello-domain-controller"></a>Hello tartományvezérlő konfigurálása
Hello a következő lépéseket, konfigurálja a hello **ad-elsődleges-dc** gépi Corp.contoso.com tartományvezérlőként működik.

1. Hello portálon, nyissa meg a hello **SQL-magas rendelkezésre ÁLLÁSÚ-RG** erőforráscsoport és select hello **ad-elsődleges-dc** gép. A hello **Active Directory-elsődleges-tartományvezérlőt** panelen kattintson a **Connect** tooopen egy RDP fájlt a távoli asztal eléréséhez.

    ![Csatlakoztassa tooa virtuális gépet](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Jelentkezzen be a beállított rendszergazdai fiókkal (**\DomainAdmin**) és a jelszót (**Contoso! 0000**).
3. Alapértelmezés szerint hello **Kiszolgálókezelő** irányítópult üzenetnek kell megjelennie.
4. Kattintson a hello **szerepkörök és szolgáltatások hozzáadása** hello irányítópult hivatkozásra kattintva.

    ![Kiszolgálókezelő - Szerepkörök hozzáadása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Válassza ki **következő** amíg elér toohello **kiszolgálói szerepkörök** szakasz.
6. Jelölje be hello **Active Directory tartományi szolgáltatások** és **DNS-kiszolgáló** szerepkörök. Amikor a rendszer kéri, adja hozzá ezeket a szerepköröket szükséges további funkciókkal.

   > [!NOTE]
   > A Windows figyelmeztetést jelenít meg, hogy nincs-e statikus IP-cím. Ha a tesztelést hello konfigurációs, kattintson a **Folytatás**. Éles környezetben, állítsa be hello IP-cím toostatic hello Azure-portálon vagy [PowerShell tooset hello statikus IP-cím hello tartomány a tartományvezérlő számítógép](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Adja hozzá a szerepkörök párbeszédpanel](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Kattintson a **következő** hello addig **megerősítő** szakasz. Jelölje be hello **újraindítás hello célkiszolgáló szükség esetén automatikusan** jelölőnégyzetet.
8. Kattintson az **Install** (Telepítés) gombra.
9. Hello szolgáltatások telepítése, visszatérési toohello befejezése után **Kiszolgálókezelő** irányítópult.
10. Jelölje be új hello **Active Directory tartományi szolgáltatások** lehetőséget a bal oldali panelen hello.
11. Kattintson a hello **további** hello sárga figyelmeztető sáv hivatkozásra kattintva.

    ![Az Active Directory tartományi szolgáltatások párbeszédpanel hello DNS-kiszolgálói virtuális gép](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. A hello **művelet** hello oszlopa **összes kiszolgáló feladat részletei** párbeszédpanel, kattintson a **a kiszolgáló tooa tartományvezérlő előléptetéséhez**.
13. A hello **Active Directory tartományi szolgáltatások konfigurációs varázslója**, a következő értékek hello használata:

    | **Lap** | Beállítás |
    | --- | --- |
    | **Központi telepítés konfigurálása** |**Új erdő hozzáadása**<br/> **Gyökértartomány neve** = corp.contoso.com |
    | **Tartományvezérlő beállításai** |**DSRM-jelszót** = Contoso! 0000<br/>**Jelszó megerősítése** = Contoso! 0000 |
14. Kattintson a **következő** keresztül toogo hello más hello varázsló lapjain. A hello **szükséges előfeltételek ellenőrzése** lapon, győződjön meg arról, hogy megjelenik-e a következő üzenet hello: **az előfeltétel-ellenőrzés sikeresen lefutott**. Tekintse át a megfelelő figyelmeztető üzeneteket, de lehetséges toocontinue hello telepítés.
15. Kattintson az **Install** (Telepítés) gombra. Hello **ad-elsődleges-dc** virtuális gép automatikusan újraindul.

### <a name="note-hello-ip-address-of-hello-primary-domain-controller"></a>Vegye figyelembe az elsődleges tartományvezérlő hello hello IP-címe

A DNS hello elsődleges tartományvezérlőt használja. Vegye figyelembe a hello elsődleges tartományvezérlő IP-címét.

Egyirányú tooget hello elsődleges tartományvezérlő IP-címét hello Azure-portálon keresztül történik.

1. Nyissa meg a hello Azure-portálon, a hello erőforráscsoportot.

2. Kattintson a hello elsődleges tartományvezérlőn történik.

3. Hello elsődleges tartományvezérlő paneljén kattintson **hálózati illesztőt**.

![Hálózati illesztők](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Vegye figyelembe a hello magánhálózati IP-címet ehhez a kiszolgálóhoz.

### <a name="configure-hello-virtual-network-dns"></a>Hello virtuális hálózat DNS konfigurálása
Miután hello első tartományvezérlő létrehozása és engedélyezése a DNS hello első kiszolgálón, hello virtuális hálózati toouse a kiszolgáló konfigurálása a DNS.

1. Hello Azure-portálon kattintson a virtuális hálózati hello.

2. A **beállítások**, kattintson a **DNS-kiszolgáló**.

3. Kattintson a **egyéni**, és írja be az elsődleges tartományvezérlő hello hello magánhálózati IP-címe.

4. Kattintson a **Save** (Mentés) gombra.

### <a name="configure-hello-second-domain-controller"></a>Második tartományvezérlő hello konfigurálása
Hello elsődleges tartományvezérlő újraindul, miután hello második tartományvezérlő konfigurálhatja. Ez az opcionális lépés van a magas rendelkezésre állás érdekében. Kövesse a lépéseket tooconfigure hello második tartományvezérlő:

1. Hello portálon, nyissa meg a hello **SQL-magas rendelkezésre ÁLLÁSÚ-RG** erőforráscsoport és select hello **ad – másodlagos-tartományvezérlő** gép. A hello **ad – másodlagos-tartományvezérlő** panelen kattintson a **Connect** tooopen egy RDP fájlt a távoli asztal eléréséhez.
2. Jelentkezzen be toohello VM a konfigurált rendszergazdai fiókkal (**BUILTIN\DomainAdmin**) és a jelszót (**Contoso! 0000**).
3. Az előnyben részesített DNS-kiszolgáló címe toohello címe hello tartományvezérlő módosítása hello.
4. A **hálózati és megosztási központ**, kattintson a hello hálózati illesztőt.
   ![Hálózati illesztő](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. Kattintson a **Tulajdonságok** elemre.
6. Válassza ki **Internet Protocol Version 4 (TCP/IPv4)** kattintson **tulajdonságok**.
7. Válassza ki **a következő DNS-kiszolgálócímek használata hello** , és adja meg a hello elsődleges tartományvezérlő a hello címét **elsődleges DNS-kiszolgáló**.
8. Kattintson a **OK**, majd **Bezárás** toocommit hello módosításokat. Most már áll méretű képes toojoin hello túl**corp.contoso.com**.

   >[!IMPORTANT]
   >Ha elveszti hello kapcsolat tooyour távoli asztal hello DNS beállítás módosítása után, nyissa meg az Azure portálon, és indítsa újra a hello virtuális gép toohello.

9. Hello távoli asztali toohello másodlagos tartományvezérlő, nyissa meg a **a Kiszolgálókezelő irányítópultja**.
10. Kattintson a hello **szerepkörök és szolgáltatások hozzáadása** hello irányítópult hivatkozásra kattintva.

    ![Kiszolgálókezelő - Szerepkörök hozzáadása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Válassza ki **következő** amíg elér toohello **kiszolgálói szerepkörök** szakasz.
12. Jelölje be hello **Active Directory tartományi szolgáltatások** és **DNS-kiszolgáló** szerepkörök. Amikor a rendszer kéri, adja hozzá ezeket a szerepköröket szükséges további funkciókkal.
13. Hello szolgáltatások telepítése, visszatérési toohello befejezése után **Kiszolgálókezelő** irányítópult.
14. Jelölje be új hello **Active Directory tartományi szolgáltatások** lehetőséget a bal oldali panelen hello.
15. Kattintson a hello **további** hello sárga figyelmeztető sáv hivatkozásra kattintva.
16. A hello **művelet** hello oszlopa **összes kiszolgáló feladat részletei** párbeszédpanel, kattintson a **a kiszolgáló tooa tartományvezérlő előléptetéséhez**.
17. A **központi telepítés konfigurálása**, jelölje be **egy tartomány tartományvezérlői tooan meglévő tartomány hozzáadása**.
   ![Központi telepítés konfigurálása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. Kattintson a **Kiválasztás** gombra.
19. Csatlakozás hello rendszergazdai fiókkal (**vállalat esetében. CONTOSO.COM\domainadmin**) és a jelszót (**Contoso! 0000**).
20. A **jelöljön ki egy tartományt hello erdőből**, kattintson a tartomány, majd **OK**.
21. A **tartományvezérlő-beállítások**, hello alapértelmezett értékeket használja, és állítsa be a DSRM-jelszót.

   >[!NOTE]
   >Hello **DNS-beállítások** lap jelezheti, hogy a DNS-kiszolgáló egy delegálás nem hozható létre. Ez a figyelmeztetés nem éles környezetben figyelmen kívül hagyhatja.
22. Kattintson a **következő** csak hello párbeszédpanel elérésekor hello **Előfeltételek** ellenőrzése. Ezt követően kattintson a **Telepítés** gombra.

Hello server hello konfigurációs módosítások befejezése után indítsa újra a hello kiszolgálót.

### <a name="add-hello-private-ip-address-toohello-second-domain-controller-toohello-vpn-dns-server"></a>Hello magánhálózati IP-cím toohello második tartomány a tartományvezérlő toohello VPN DNS-kiszolgáló hozzáadása

Az Azure-portálon, a virtuális hálózat hello hello DNS-kiszolgáló tooinclude hello IP-cím hello másodlagos tartományvezérlő módosításához. Ez lehetővé teszi a DNS-szolgáltatás redundancia hello.

### <a name=DomainAccounts></a>Hello tartományi fiókok beállítása

A következő lépések hello hello Active Directory-fiókok konfigurálása. hello következő táblázatban hello fiókok:

| |Telepítési fiók<br/> |SQL Server-0 <br/>SQL Server és az SQL Agent szolgáltatás fiókja |SQL Server-1<br/>SQL Server és az SQL Agent szolgáltatás fiókja
| --- | --- | --- | ---
|**Utónév** |Telepítés |SQLSvc1 | SQLSvc2
|**Felhasználó SamAccountName** |Telepítés |SQLSvc1 | SQLSvc2

A következő lépéseket toocreate hello minden fiókot használja.

1. Jelentkezzen be toohello **ad-elsődleges-dc** gép.
2. A **Kiszolgálókezelő**, jelölje be **eszközök**, és kattintson a **Active Directory felügyeleti központ**.   
3. Válassza ki **corp (helyi)** hello bal oldali ablaktáblán.
4. A jobb oldali hello **feladatok** ablaktáblán válassza előbb **új**, és kattintson a **felhasználói**.
   ![Active Directory felügyeleti központ](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Az egyes fiókok számára összetett jelszót állíthat be.<br/> Nem éles környezetben set hello felhasználói fiók toonever lejár.

5. Kattintson a **OK** toocreate hello felhasználó.
6. Ismételje meg az előző lépésekben az egyes hello három fiókok hello.

### <a name="grant-hello-required-permissions-toohello-installation-account"></a>Adja meg a hello szükséges engedélyeket toohello telepítési fiók
1. A hello **Active Directory felügyeleti központ**, jelölje be **corp (helyi)** hello bal oldali ablaktáblán. Ezt a jobb oldali hello **feladatok** ablaktáblán kattintson a **tulajdonságok**.

    ![CORP felhasználó tulajdonságai](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Válassza ki **bővítmények**, majd kattintson a hello **speciális** hello gombjára **biztonsági** lapon.
3. A hello **speciális biztonsági beállítások vállalati** párbeszédpanel, kattintson a **Hozzáadás**.
4. Kattintson a **rendszerbiztonsági tag kiválasztása**, keressen **CORP\Install**, és kattintson a **OK**.
5. Jelölje be hello **az összes tulajdonság olvasása** jelölőnégyzetet.

6. Jelölje be hello **számítógép-objektumok létrehozása** jelölőnégyzetet.

     ![Corp felhasználói engedélyek](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Kattintson a **OK**, és kattintson a **OK** újra. Bezárás hello **corp** tulajdonságai ablakban.

Most, hogy az Active Directory és a felhasználói objektumok hello befejezése után hozzon létre két SQL Server virtuális gépen és a tanúsító kiszolgálói virtuális gép. Majd tartományhoz összes három toohello.

## <a name="create-sql-server-vms"></a>SQL Server virtuális gépek létrehozása

Hozzon létre három további virtuális gépeket. hello megoldást igényel a két virtuális gép az SQL Server-példányokat. A harmadik virtuális gép egy tanúsító fog működni. Windows Server 2016 használhatja egy [tanúsító cloud](http://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), azonban ez a dokumentum korábbi operációs rendszerekhez való konzisztenciáját a virtuális gép használja a tanúsító.  

Folytatás előtt fontolja meg a következő deisign döntések hello.

* **Tároló - Azure által kezelt lemezeken**

   A virtuálisgép-tároló hello Azure felügyelt lemezek használhatók. A Microsoft azt javasolja, hogy felügyelt lemezek SQL Server virtuális gépekhez. A felügyelt lemezek leírók tárolási hello színfalak mögött. Emellett, ha felügyelt lemezzel rendelkező virtuális gépek szerepelnek hello azonos rendelkezésre állási csoportot, Azure hello tárolási erőforrások tooprovide megfelelő redundancia osztja el. További információkért lásd az [Azure Managed Disks áttekintését](../managed-disks-overview.md). A rendelkezésre állási csoportba felügyelt lemezek kapcsolatos részletekért lásd: [használata felügyelt lemezeket a virtuális gépek rendelkezésre állási csoportba](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Hálózati - privát IP-címek éles környezetben**

   Hello virtuális gépek esetén ez az oktatóanyag nyilvános IP-címet használja. Ez lehetővé teszi a távoli kapcsolat a közvetlen toohello virtuális gép keresztül hello internet - konfigurációs lépések egyszerűbbé teszi. Éles környezetben Microsoft azt javasolja, hogy csak privát IP-címek a rendelés tooreduce hello biztonsági kockázatokat hello SQL Server-példány virtuális gép erőforrásához.

### <a name="create-and-configure-hello-sql-server-vms"></a>Hozza létre és konfigurálja a hello SQL Server virtuális gépen
Ezután hozzon létre három virtuális gépek – két SQL Server virtuális gépen és a virtuális gépek számára egy további fürtcsomópontra. toocreate minden hello virtuális gépek, lépjen vissza toohello **SQL-magas rendelkezésre ÁLLÁSÚ-RG** erőforráscsoport, kattintson a **Hozzáadás**, keresse meg a megfelelő gyűjteményelem hello, kattintson a **virtuális gép**, majd Kattintson a **a gyűjtemény**. Használja a következő tábla toohelp hello virtuális gépek létrehozása hello hello adatokat:


| Lap | VM1 | VM2 VIRTUÁLIS GÉPNEK | VM3 |
| --- | --- | --- | --- |
| Válassza ki a megfelelő gyűjteményelem hello |**Windows Server 2016 Datacenter** |**SQL Server 2016 SP1 Enterprise, Windows Server 2016** |**SQL Server 2016 SP1 Enterprise, Windows Server 2016** |
| Virtuálisgép-konfiguráció **alapjai** |**Név** fürt-fsw =<br/>**Felhasználónév** tartománygazda =<br/>**Jelszó** = Contoso! 0000<br/>**Előfizetés** az előfizetés =<br/>**Erőforráscsoport** SQL-magas rendelkezésre ÁLLÁSÚ-RG =<br/>**Hely** az Azure-beli hely = |**Név** sqlserver – 0 =<br/>**Felhasználónév** tartománygazda =<br/>**Jelszó** = Contoso! 0000<br/>**Előfizetés** az előfizetés =<br/>**Erőforráscsoport** SQL-magas rendelkezésre ÁLLÁSÚ-RG =<br/>**Hely** az Azure-beli hely = |**Név** SQL Server-1 =<br/>**Felhasználónév** tartománygazda =<br/>**Jelszó** = Contoso! 0000<br/>**Előfizetés** az előfizetés =<br/>**Erőforráscsoport** SQL-magas rendelkezésre ÁLLÁSÚ-RG =<br/>**Hely** az Azure-beli hely = |
| Virtuálisgép-konfiguráció **mérete** |**MÉRET** = DS1\_V2 (1 mag, 3.5-ös GB) |**MÉRET** = DS2\_V2 (2 mag, 7 GB)</br>hello mérete támogatnia kell az SSD-tárhelyre (prémium szintű támogatása. )) |**MÉRET** = DS2\_V2 (2 mag, 7 GB) |
| Virtuálisgép-konfiguráció **beállítások** |**Tárolási**: az kezelt lemezek.<br/>**Virtuális hálózati** = autoHAVNET<br/>**Alhálózati** = sqlsubnet(10.1.1.0/24)<br/>**Nyilvános IP-cím** automatikusan generált.<br/>**Hálózati biztonsági csoport** = None<br/>**Diagnosztikai figyelő** = engedélyezve<br/>**Diagnosztikai tárfiók** = egy automatikusan létrehozott tárfiók használata<br/>**A rendelkezésre állási csoport** = sqlAvailabilitySet<br/> |**Tárolási**: az kezelt lemezek.<br/>**Virtuális hálózati** = autoHAVNET<br/>**Alhálózati** = sqlsubnet(10.1.1.0/24)<br/>**Nyilvános IP-cím** automatikusan generált.<br/>**Hálózati biztonsági csoport** = None<br/>**Diagnosztikai figyelő** = engedélyezve<br/>**Diagnosztikai tárfiók** = egy automatikusan létrehozott tárfiók használata<br/>**A rendelkezésre állási csoport** = sqlAvailabilitySet<br/> |**Tárolási**: az kezelt lemezek.<br/>**Virtuális hálózati** = autoHAVNET<br/>**Alhálózati** = sqlsubnet(10.1.1.0/24)<br/>**Nyilvános IP-cím** automatikusan generált.<br/>**Hálózati biztonsági csoport** = None<br/>**Diagnosztikai figyelő** = engedélyezve<br/>**Diagnosztikai tárfiók** = egy automatikusan létrehozott tárfiók használata<br/>**A rendelkezésre állási csoport** = sqlAvailabilitySet<br/> |
| Virtuálisgép-konfiguráció **SQL Server-beállítások** |Nem alkalmazható |**SQL-kapcsolat** = Private (virtuális hálózaton belül)<br/>**Port** = 1433<br/>**SQL-hitelesítés** = letiltás<br/>**Tárolási konfiguráció** általános =<br/>**Automatikus javítás** vasárnap = 2:00<br/>**Automatikus biztonsági mentés** = letiltva</br>**Az Azure Key Vault-integráció** = letiltva |**SQL-kapcsolat** = Private (virtuális hálózaton belül)<br/>**Port** = 1433<br/>**SQL-hitelesítés** = letiltás<br/>**Tárolási konfiguráció** általános =<br/>**Automatikus javítás** vasárnap = 2:00<br/>**Automatikus biztonsági mentés** = letiltva</br>**Az Azure Key Vault-integráció** = letiltva |

<br/>

> [!NOTE]
> Itt javasolt hello méreteket úgy van kialakítva, a rendelkezésre állási csoportok tesztelése az Azure virtuális gépeken. Hello termelési számítási feladatokhoz a legjobb teljesítmény érdekében hello ajánlott méreteket SQL Server és a konfiguráció megtekintéséhez [teljesítmény a bevált gyakorlat az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Miután hello három virtuális gépek teljesen kiépített, toojoin kell őket toohello **corp.contoso.com** tartomány- és támogatási CORP\Install rendszergazdai jogosultságokkal toohello gépek.

### <a name="joinDomain"></a>Hello kiszolgálók toohello tartományhoz

Most már tudja toojoin hello virtuális gépek túl Ön**corp.contoso.com**. Hello következő hello SQL Server virtuális gépen és a hello fájl ugyanazt a tanúsító kiszolgálói:

1. Távoli kapcsolódás a virtuális gép toohello **BUILTIN\DomainAdmin**.
2. A **Kiszolgálókezelő**, kattintson a **helyi kiszolgáló**.
3. Kattintson a hello **munkacsoport** hivatkozásra.
4. A hello **számítógépnév** kattintson **módosítása**.
5. Jelölje be hello **tartomány** jelölőnégyzetet, és írja be **corp.contoso.com** hello szövegmezőben. Kattintson az **OK** gombra.
6. A hello **Windows biztonsági** előugró párbeszédpanelen adja meg a hello hello alapértelmezett tartományi rendszergazdai fiók hitelesítő adatait (**CORP\DomainAdmin**) és hello jelszó (**Contoso! 0000**).
7. Ha hello "Üdvözli toohello corp.contoso.com tartomány" üzenet jelenik meg, kattintson **OK**.
8. Kattintson a **Bezárás**, és kattintson a **Újraindítás most** hello előugró párbeszédpanelen.

### <a name="add-hello-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Minden virtuális gép fürtön rendszergazdaként hello Corp\Install felhasználó hozzáadása

Minden virtuális gép újraindításával hello tartomány tagjaként hozzáadása **CORP\Install** hello helyi Rendszergazdák csoport tagjaként.

1. Várjon, amíg a hello virtuális gép újraindul, majd indítsa el újra a hello elsődleges tartományvezérlő toosign az RDP-fájlban hello túl**SQL Server-0** hello segítségével **CORP\DomainAdmin** fiók.
   >[!TIP]
   >Győződjön meg arról, hogy jelentkezzen be hello tartományi rendszergazdai fiókot. Hello előző lépésekben hello IN beépített rendszergazdai fiókot használta. Most, hogy hello server hello tartományban, hello tartományi fiók használata. Az RDP-munkamenetbe, adja meg a *tartomány*\\*felhasználónév*.

2. A **Kiszolgálókezelő**, jelölje be **eszközök**, és kattintson a **számítógép-kezelés**.
3. A hello **számítógép-kezelés** ablakában bontsa ki a **helyi felhasználók és csoportok**, majd válassza ki **csoportok**.
4. Kattintson duplán a hello **rendszergazdák** csoport.
5. A hello **rendszergazdák tulajdonságok** párbeszédpanel, kattintson hello **Hozzáadás** gombra.
6. Adja meg a hello felhasználói **CORP\Install**, és kattintson a **OK**.
7. Kattintson a **OK** tooclose hello **rendszergazda tulajdonságainak** párbeszédpanel.
8. Ismételje meg a korábbi lépéseket hello **SQL Server-1** és **fürt-fsw**.

### <a name="setServiceAccount"></a>SQL Server szolgáltatásfiókok hello beállítása

Az SQL Server virtuális gépek hello SQL Server szolgáltatásfiók beállítása. Létrehozott mikor hello fiókok használata akkor [hello tartományi fiókok konfigurált](#DomainAccounts).

1. Nyissa meg **SQL Server Konfigurációkezelő**.
2. Kattintson a jobb gombbal a hello SQL Server szolgáltatás, és kattintson **tulajdonságok**.
3. Állítsa be a hello fiókot és jelszót.
4. Ismételje meg ezeket a lépéseket a hello más SQL Server virtuális gép.  

Az SQL Server rendelkezésre állási csoportok minden egyes SQL Server virtuális gép toorun tartományi fiókként van szüksége.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-hello-installation-account"></a>A bejelentkezés minden SQL Server virtuális gépen hello telepítési fiók létrehozása

Hello telepítési fiók (CORP\install) tooconfigure hello rendelkezésre állási csoportot használjon. Ezt a fiókot kell toobe hello tagja **sysadmin** rögzített kiszolgálói szerepkör minden egyes SQL Server virtuális gépen. hello következő lépéseit hozza létre a bejelentkezés hello telepítési fiók:

1. Hello használatával kapcsolódnak a toohello server hello Remote Desktop Protocol (RDP) révén  *\<MachineName\>\DomainAdmin* fiók.

1. Nyissa meg az SQL Server Management Studio eszközt, és csatlakozzon az SQL Server helyi példányát toohello.

1. A **Object Explorer**, kattintson a **biztonsági**.

1. Kattintson a jobb gombbal **bejelentkezések**. Kattintson a **új bejelentkezés**.

1. A **- új bejelentkezés**, kattintson a **keresési**.

1. Kattintson a **helyek**.

1. Adja meg a tartományi rendszergazda hitelesítő hello.

1. Hello telepítési fiókot használni.

1. Állítsa be a hello bejelentkezési toobe hello tagja **sysadmin** rögzített kiszolgálói szerepkör.

1. Kattintson az **OK** gombra.

Ismételje meg az előző lépésekben a hello más SQL Server virtuális gép hello.

## <a name="add-failover-clustering-features-tooboth-sql-server-vms"></a>Adja hozzá a Feladatátvételi fürtszolgáltatás szolgáltatások tooboth SQL Server virtuális gépen

tooadd Feladatátvételi fürtszolgáltatással, hello mindkét SQL Server virtuális gépekre a következő:

1. Hello használatával kapcsolódnak a toohello SQL Server virtuális gép hello Remote Desktop Protocol (RDP) révén *CORP\install* fiók. Nyissa meg **a Kiszolgálókezelő irányítópultja**.
2. Kattintson a hello **szerepkörök és szolgáltatások hozzáadása** hello irányítópult hivatkozásra kattintva.

    ![Kiszolgálókezelő - Szerepkörök hozzáadása](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Válassza ki **következő** amíg elér toohello **kiszolgálószolgáltatások** szakasz.
4. A **szolgáltatások**, jelölje be **feladatátvételi fürtszolgáltatás**.
5. A további szükséges szolgáltatások hozzáadását.
6. Kattintson a **telepítése** tooadd hello szolgáltatásokat.

Ismételje meg a hello hello a többi SQL Server virtuális gép.

## <a name="a-nameendpoint-firewall-configure-hello-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall">Minden egyes SQL Server virtuális gépen hello tűzfal konfigurálása

hello megoldás hello TCP-portok toobe hello tűzfal nyissa meg a következő szükséges:

- **SQL Server rendszerű virtuális Géphez**:<br/>
   Az SQL Server alapértelmezett példányán 1433-as port.
- **Terheléselosztói mintavétel Azure:**<br/>
   Minden elérhető port. Példák 59999 gyakran használnak.
- **Adatbázis-tükrözési végpontját:** <br/>
   Minden elérhető port. Példák 5022 gyakran használnak.

hello tűzfal portok kell toobe nyissa meg a mindkét SQL Server virtuális gépeken.

hello portok megnyitása hello módja attól függ, hogy hello tűzfal megoldáshoz, amelyet használni. hello a következő szakasz ismerteti, hogyan tooopen hello a Windows tűzfal portjai. Nyissa meg a szükséges hello portok minden egyes az SQL Server virtuális gépen.

### <a name="open-a-tcp-port-in-hello-firewall"></a>Nyissa meg a TCP-port hello tűzfal

1. Az első SQL-kiszolgáló hello **Start** indítsa el a jobb **fokozott biztonságú Windows tűzfal**.
2. Hello bal oldali ablaktáblában jelölje ki **bejövő szabályok**. Kattintson a jobb oldali hello **új szabály**.
3. A **szabálytípus**, válassza a **Port**.
4. Hello port megadása **TCP** és a megfelelő számú portok típus hello. Tekintse meg a következő példa hello:

   ![SQL-tűzfal](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. Kattintson a **Tovább** gombra.
6. A hello **művelet** lapon, tartsa **hello csatlakozás engedélyezése** kiválasztva, és kattintson **következő**.
7. A hello **profil** lapon fogadja el hello alapértelmezett beállításokat, és kattintson a **következő**.
8. A hello **neve** csoportjában adja meg a szabály neve (például **Azure LB mintavételi**) a hello **neve** szövegmezőbe, és kattintson **Befejezés**.

Ismételje meg ezeket a lépéseket a második SQL Server virtuális gép hello.

## <a name="next-steps"></a>Következő lépések

* [Azure virtuális gépeken futó SQL Server Always On rendelkezésre állási csoport létrehozása](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
