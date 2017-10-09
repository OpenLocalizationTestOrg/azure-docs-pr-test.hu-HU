---
title: "az Azure Directory összevonási szolgáltatások aaaActive |} Microsoft Docs"
description: "Ez a dokumentum megtudhatja, hogyan toodeploy AD FS az Azure-ban magas elérhetőségét."
keywords: "az azure AD FS üzembe helyezése, az azure AD FS, az azure AD FS, az azure Active Directory összevonási szolgáltatások telepítése, az AD FS telepítése, központi telepítése az ad fs-ben az AD FS az azure-ban, az azure AD FS telepítése, AD FS üzembe helyezése az azure, az AD FS azure, a bevezetés tooAD FS, Azure, az Azure Active Directory összevonási szolgáltatások iaas, az AD FS, helyezze át az AD FS tooazure"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2c39271f7569b9ce395dce2f53f5ba5a4897b132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Az Active Directory összevonási szolgáltatások üzembe helyezése az Azure-ban
Az AD FS egyszerű, mégis biztonságos identitás-összevonást, valamint webes egyszeri bejelentkezési (SSO) funkciókat biztosít. Az Azure AD összevonási vagy Office 365 lehetővé teszi, hogy a felhasználók tooauthenticate használatával a helyszíni hitelesítő adatok, és a felhő minden erőforrások elérését. Ennek eredményeképpen válik fontos toohave egy magas rendelkezésre állású AD FS infrastruktúra tooensure hozzáférési tooresources mind a helyszíni és felhőben hello. AD FS Azure telepítése segíthet hello magas rendelkezésre állásának eléréséhez szükséges minimális erőfeszítéseket.
Az AD FS Azure-ban történő üzembe helyezése számos előnnyel jár. Alább ezek közül sorolunk fel néhányat:

* **Magas rendelkezésre állású** -hello hatványra emelésének Azure rendelkezésre állási csoportokra, és a magas rendelkezésre állású infrastruktúrák biztosítása.
* **Egyszerű tooScale** – további teljesítmény kell? Könnyen toomore hatékony gépeket áttelepíthet néhány kattintással az Azure-ban
* **Kereszt-Georedundancia** – az Azure Georedundancia akkor biztos lehet abban, hogy az infrastruktúra nem magas rendelkezésre állású hello földgömb
* **Egyszerű tooManage** – az infrastruktúra kezelése magas egyszerűsített kezelési lehetőségek az Azure portálon, nagyon egyszerűen és problémamentes 

## <a name="design-principles"></a>Tervezési alapelvek
![Az üzembe helyezés megtervezése](./media/active-directory-aadconnect-azure-adfs/deployment.png)

hello a fenti ábrán hello ajánlott alapszintű topológia toostart központi telepítése az AD FS infrastruktúra Azure-ban. hello alapelvek mögött hello hello topológia különböző összetevői az alábbiak:

* **Tartományvezérlő/ADFS-kiszolgáló**: ha ezernél kevesebb felhasználóval rendelkezik, egyszerűen telepítse az AD FS szerepkört a tartományvezérlőkre. Ha nem szeretné, hogy a teljesítményre gyakorolt hatás hello olyan tartományvezérlőn, vagy ha több mint 1000 felhasználó rendelkezik, majd telepíteni az AD FS egyazon kiszolgálón.
* **WAP-kiszolgáló** – szükséges toodeploy webalkalmazás-Proxy kiszolgálók, fontos, hogy a felhasználók hello AD FS, ha azok nincsenek hello vállalati hálózaton is képes elérni.
* **DMZ**: hello webalkalmazás-Proxy kiszolgálók hello DMZ kerülnek, és csak a TCP/443-as elérését hello DMZ és hello belső alhálózat között.
* **Terheléselosztók**: tooensure magas rendelkezésre álláshoz az AD FS és a webalkalmazás-Proxy kiszolgálók használatát javasoljuk a belső terheléselosztók AD FS-kiszolgáló és az Azure terheléselosztó a webalkalmazás-Proxy kiszolgálók.
* **Rendelkezésre állási készletek**: tooprovide redundancia tooyour AD FS üzembe helyezése, javasoljuk, hogy két vagy több virtuális gépek rendelkezésre állási csoport hasonló munkaterhelések csoportosítása. Így legalább egy virtuális gép mindig elérhető lesz a tervezett vagy nem tervezett karbantartási események esetében is.
* **Storage-fiókok**: toohave két tárfiókok ajánlott. Egyetlen tárfiók vezethet a hibaérzékeny pontok kialakulását toocreation, és nem érhető el, ahol hello tárfiók leáll valószínű esetnél hello telepítési toobecome okozhat. Azzal, ha két tárfiókot használ, lefedheti mind a két meghibásodási lehetőséget.
* **Hálózatok szétválasztása**: a webalkalmazásproxy-kiszolgálókat eltérő DMZ-hálózatokban helyezze üzembe. Egy virtuális hálózati osztani két alhálózat, és telepítheti a hello webalkalmazás-Proxy kiszolgáló (k) egy elkülönített alhálózat. Egyszerűen konfigurálni hello hálózati biztonsági csoport beállításait az egyes alhálózatokon, és csak szükséges kommunikációt hello két alhálózat között. Alább részletes információkat is megtudhat ezzel kapcsolatban az egyes üzemelőpéldány-típusokra vonatkozóan.

## <a name="steps-toodeploy-ad-fs-in-azure"></a>Lépéseket toodeploy AD FS az Azure-ban
Ez a szakasz vázlat hello útmutató toodeploy hello alábbi említett hello lépéseket az AD FS infrastruktúra Azure ábrázolva.

### <a name="1-deploying-hello-network"></a>1. Hello hálózati telepítése
Ahogy fent már leírtuk, hozzon létre két, egyetlen virtuális hálózathoz tartozó különböző alhálózatot, vagy két teljesen különálló virtuális hálózatot (VNet). Ebben a cikkben egyetlen virtuális hálózatot hozunk létre, amelyet aztán két alhálózatra bontunk. Ez a funkció jelenleg egy egyszerűbb módszert, két külön Vnetek való kommunikáció VNet tooVNet átjáró igényel.

**1.1 Virtuális hálózat létrehozása**

![Virtuális hálózat létrehozása](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)

Hello Azure-portálon, a select virtuális hálózatot, és telepítheti hello virtuális hálózat és egy alhálózat azonnal csupán egyetlen kattintással. INT alhálózati is definiálva van, és hozzá virtuális gépek toobe most már készen áll.
következő lépés hello tooadd van egy másik alhálózat toohello hálózat, azaz hello DMZ alhálózat. toocreate hello DMZ alhálózati, egyszerűen

* Válassza ki az újonnan létrehozott hello hálózati
* Hello tulajdonságainál jelölje ki az alhálózatot
* Hello hello alhálózati panelen kattintson a Hozzáadás gomb
* Adja meg a hello alhálózat nevét és címét terület információk toocreate hello alhálózati

![Alhálózat](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)

![DMZ-alhálózat](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. Hello hálózati biztonsági csoportok létrehozása**

Hálózati biztonsági csoport (NSG) tartalmaz, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooyour Virtuálisgép-példány egy virtuális hálózati hozzáférés-vezérlési lista (ACL) szabályok listáját. Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni. Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello Virtuálisgép-példány alhálózaton.
Ez az útmutató a hello célra létrehozunk két NSG-ket: parancsikonja a belső hálózat és a Szegélyhálózaton. Ezek neve rendre NSG_INT, illetve NSG_DMZ lesz.

![Az NSG létrehozása](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Hello NSG jön létre, után nem lesznek 0 0 és bejövő kimenő szabályok. Miután hello szerepkörök hello megfelelő kiszolgálókon telepített és megfelelően működik, majd hello bejövő és kimenő szabályok lehet kapcsolódni a függően toohello szükséges biztonsági szintjét.

![Az NSG inicializálása](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Hello NSG-k létrehozása után NSG_INT társítandó alhálózat INT és NSG_DMZ DMZ alhálózattal. Alább láthatja az ezt bemutató képernyőképet:

![Az NSG konfigurálása](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Kattintson az alhálózatok tooopen hello panel alhálózatok
* Válassza ki a hello alhálózati tooassociate hello NSG-t a 

Konfigurálást követően az alhálózatok hello panel alatt kell hasonlítania:

![Alhálózatok panel az NSG társítását követően](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Helyszíni tooon kapcsolat létrehozása**

Fel kell egy kapcsolatot tooon helyszíni rendelés toodeploy hello tartományvezérlőn (DC) az azure-ban. Az Azure kínál különböző kapcsolati lehetőségek tooconnect a helyszíni infrastruktúra tooyour Azure-infrastruktúra.

* Pont–hely kapcsolat
* Virtuális hálózat, helyek közötti kapcsolat
* ExpressRoute

Toouse ExpressRoute ajánlott. Az ExpressRoute használatával privát kapcsolatok hozhatók létre az Azure-adatközpontok és a helyszíni vagy a bérelt kiszolgálói környezetben üzemelő infrastruktúra között. Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. További megbízhatóságát, gyorsabb sebességű, kisebb késések és nagyobb biztonságot nyújtana tipikus kapcsolatok biztosítanak hello interneten keresztül.
Ajánlott toouse ExpressRoute, amíg úgy is dönthet, a szervezete számára leginkább megfelelő kapcsolódási módszert. További információk az ExpressRoute és hello toolearn különböző kapcsolati lehetőségek használatával ExpressRoute, olvassa el [ExpressRoute műszaki áttekintés](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Tárfiókok létrehozása
A sorrend toomaintain magas rendelkezésre állású és egy tárfiók függés elkerüléséhez két storage-fiókok is létrehozhat. Egyes rendelkezésre állási csoport hello gépek felosztani két, majd rendelje hozzá minden csoport külön tárfiókot.

![Tárfiókok létrehozása](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Rendelkezésre állási csoportok létrehozása
Az egyes szerepkörökhöz (DC/AD FS és WAP) 2 számítógépet minden, a minimális hello tartalmazó rendelkezésre állási csoportok létrehozása. Így a szerepkörök magasabb fokú rendelkezésre állást tudnak garantálni. Létrehozás hello rendelkezésre állási állítja be, de napjainkban hello következő alapvető toodecide:

* **Tartományok fault**: a virtuális gépek hello tartalék tartomány megosztáshoz hello azonos áramforrásról és közös fizikai hálózati kapcsolóval működnek. Javasoljuk, hogy használjon legalább 2 különböző tartalék tartományt. hello alapértelmezett érték 3 és hagyhatja, mivel a központi telepítés hello célra
* **Tartományok frissítése**: gépek tartozó toohello azonos frissítési tartományban együtt újraindításáig frissítése közben. Azt szeretné, hogy toohave legalább 2 frissítési tartományok. hello alapértelmezett érték 5 és hagyhatja, mivel a központi telepítés hello célra

![Rendelkezésre állási csoportok](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

A következő rendelkezésre állási készletek hello létrehozása

| Rendelkezésre állási csoport | Szerepkör | Tartalék tartományok | Frissítési tartományok |
|:---:|:---:|:---:|:--- |
| contosodcset |Tartományvezérlő/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Virtuális gépek üzembe helyezése
hello következő lépésre toodeploy virtuális gépeket futtató kiszolgálón hello különböző szerepkörök a infrastruktúrában. Mindegyik rendelkezésre állási csoportban használjon legalább két gépet. Hozzon létre négy virtuális gépek hello alapszintű központi telepítést.

| Gép | Szerepkör | Alhálózat | Rendelkezésre állási csoport | Tárfiók | IP-cím |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |Tartományvezérlő/ADFS |INT |contosodcset |contososac1 |Statikus |
| contosodc2 |Tartományvezérlő/ADFS |INT |contosodcset |contososac2 |Statikus |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Statikus |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Statikus |

Talán észrevette, hogy egyetlen NSG-t sem adtunk meg. Ennek az az oka azure lehetővé teszi, hogy NSG hello alhálózati szinten. Ezután azt is szabályozhatja a gép hálózati forgalmának hello egyes NSG társított vagy hello alhálózatot, vagy pedig hello NIC objektum használatával. További információk: [Mi az a hálózati biztonsági csoport (NSG)?](https://aka.ms/Azure/NSG).
Statikus IP-cím akkor ajánlott, ha a kezelt hello DNS. Használja az Azure DNS-ben, és ehelyett hello a tartomány DNS-rekordjait, tekintse meg a toohello új gépek az Azure teljes tartománynevekkel.
A virtuális gép ablaktábla kell hasonlítania alatt hello központi telepítés befejezése után:

![Virtuális gépek üzembe helyezve](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-hello-domain-controller--ad-fs-servers"></a>5. Konfigurálás hello tartományvezérlő / AD FS-kiszolgáló
 A sorrend tooauthenticate a bejövő kérelem, az AD FS toocontact hello tartományvezérlő kell. toosave hello költséges út Azure tooon helyszíni tartományvezérlőt a hitelesítéshez, ajánlott toodeploy replika tartományvezérlő hello Azure-ban. Rendelés tooattain a magas rendelkezésre állás, a rendelkezésre állási készlet legalább 2 tartományvezérlők toocreate ajánlott.

| Tartományvezérlő | Szerepkör | Tárfiók |
|:---:|:---:|:---:|
| contosodc1 |Replika |contososac1 |
| contosodc2 |Replika |contososac2 |

* Két kiszolgáló hello előléptetni replika tartományvezérlőket, DNS-sel
* Hello AD FS-kiszolgálók konfigurálása hello Kiszolgálókezelővel hello AD FS szerepkör telepítésével.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. A belső terheléselosztó (ILB) üzembe helyezése
**6.1. Hello ILB létrehozása**

toodeploy egy ILB, jelölje be a hello Azure-portálon, majd kattintson a terheléselosztó hozzáadása (+).

> [!NOTE]
> Ha nem látja **Terheléselosztók** a menüben kattintson a **Tallózás** hello hello portál bal alsó, és görgessen, amíg megjelenik **Terheléselosztók**.  Kattintson a hello sárga csillag tooadd azt tooyour menü. Immár hello új ikon tooopen hello panel toobegin terheléselosztó hello terheléselosztó.
> 
> 

![Terheléselosztó tallózása](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Név**: biztosítják a megfelelő nevet toohello terheléselosztó
* **Rendszer**: mivel ez a terheléselosztó hello AD FS-kiszolgálók előtt kerülnek, és ez a belső hálózati kapcsolatok csak, válassza ki a "Belső"
* **Virtuális hálózati**: az AD FS telepítéséhez hello virtuális hálózat választása
* **Alhálózati**: hello Itt a belső alhálózat kiválasztása
* **IP-címkiosztás**: statikus

![Belső terheléselosztó](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)

Miután rákattintott létrehozása és hello ILB van telepítve, kell megjelennie a terheléselosztók hello listája:

![Terheléselosztók listája az ILB létrehozását követően](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)

Következő lépés az tooconfigure hello háttérkészlet és hello háttér mintavétel.

**6.2. Az ILB-háttérkészlet konfigurálása**

Válassza hello Példánynak újonnan létrehozottnak hello Terheléselosztók panelen. Az hello-beállítások panel nyílik meg. 

1. Válassza ki a háttérkészlet hello-beállítások panelen
2. Hello hozzá háttér címkészletet panelen, kattintson a virtuális gép hozzáadása
3. Megjelenik egy panel, amelyen kiválaszthatja a rendelkezésre állási csoportot.
4. Az AD FS hello rendelkezésre állási csoport kiválasztása

![Az ILB-háttérkészlet konfigurálása](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)

**6.3. A mintavétel konfigurálása**

Jelölje ki mintavételt hello ILB beállítások panelen.

1. Kattintson a Hozzáadás gombra.
2. Adja meg a mintavétel adatait. a. **Név**: a mintavétel neve. b. **Protokoll**: TCP. c. **Port**: 443 (HTTPS). d. **Időköz**: 5 (alapértelmezett érték) – Ez az hello időköz, ahol a ILB mintavételi fog hello háttérkészletét e. hello gépek. **Sérült küszöbérték korlát**: 2 (alapértelmezett val ue) – Ez az egymást követő sikertelen mintavételek elteltével ILB a gép deklarálja a hello háttér címkészletet nem válaszoló és leállíthatja küldési forgalom tooit hello küszöbértéket.

![Az ILB-mintavétel konfigurálása](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)

**6.4. Terheléselosztási szabályok létrehozása**

A sorrend tooeffectively egyenleg hello forgalom terheléselosztási szabályok hello Példánynak kell konfigurálni. A sorrend toocreate tartozó terheléselosztási szabályok 

1. Válassza ki a terheléselosztási szabály a hello ILB hello beállítások panelről
2. Kattintson a Hozzáadás hello a terhelés terheléselosztási szabály panel
3. A Hozzáadás hello betölteni a terheléselosztási szabály panel egy. **Név**: Adja meg a b hello szabály nevét. **Protokoll**: válassza a TCP lehetőséget. c. **Port**: 443. d. **Háttérport**: 443. e. **Háttérkészlet**: hello AD FS fürt korábbi f létrehozott hello készlet kiválasztása. **Mintavételi**: AD FS-kiszolgálók a korábban létrehozott válassza hello mintavétel

![ILB-terheléselosztási szabályok konfigurálása](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. A DNS frissítése az ILB-vel**

Nyissa meg tooyour DNS-kiszolgáló és a hello ILB egy CNAME rekordot kell létrehoznia. hello CNAME hello ILB toohello IP-címére mutató hello IP-címmel kell hello összevonási szolgáltatás. Ha hello ILB DIP cím 10.3.0.8 és hello összevonási szolgáltatás telepítve például: fs.contoso.com, hozzon létre egy CNAME REKORDOT a too10.3.0.8 mutat: fs.contoso.com.
Ezzel biztosíthatja, hogy az összes kommunikáció fs.contoso.com végpont mentése hello ILB vonatkozó és a megfelelő irányítsa.

### <a name="7-configuring-hello-web-application-proxy-server"></a>7. Hello webalkalmazás-Proxy kiszolgáló konfigurálása
**7.1. Hello webalkalmazás-Proxy kiszolgálók tooreach AD FS-kiszolgáló konfigurálása**

A sorrend tooensure, hogy a webalkalmazás-Proxy kiszolgálók képes tooreach hello AD FS kiszolgálók hello ILB mögött a hello %systemroot%\system32\drivers\etc\hosts hello ILB a rekordot kell létrehozni. Vegye figyelembe, hogy a megkülönböztető név (DN) hello kell hello összevonási szolgáltatás nevét, például: fs.contoso.com. És hello IP bejegyzést kell lennie, amely hello ILB IP-cím (10.3.0.8 hello példában látható módon).

**7.2. Hello webalkalmazás-Proxy szerepkör telepítése**

Miután meggyőződött arról, hogy a webalkalmazás-Proxy kiszolgálók képes tooreach hello AD FS-kiszolgálók ILB mögött, hello webalkalmazás-Proxy kiszolgálók ezután telepítheti. Webalkalmazás-Proxy kiszolgálók nem tartományhoz csatlakoztatott toohello kell lennie. Hello webalkalmazás-Proxy szerepkörök telepítésének hello két webalkalmazás-Proxy kiszolgáló a távelérési szerepkör hello kiválasztásával. a Kiszolgálókezelő hello végigvezeti toocomplete hello WAP telepítés.
További információt toodeploy WAP, olvassa el a [telepítse és konfigurálja a webalkalmazás-Proxy kiszolgáló hello](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-hello-internet-facing-public-load-balancer"></a>8.  Hello terheléselosztó Internet Facing (nyilvános) telepítése
**8.1.  Az internetre irányuló (nyilvános) terheléselosztó létrehozása**

Válassza ki a terheléselosztók hello Azure-portálon, és kattintson a Hozzáadás. Hello létrehozás terhelés terheléselosztó panelen adja meg a következő információ hello

1. **Név**: hello terheléselosztó nevét
2. **Séma**: Nyilvános – ezzel a beállítással közölheti az Azure-ral, hogy a terheléselosztóhoz nyilvános cím szükséges.
3. **IP-cím**: hozzon létre új (dinamikus) IP-címet.

![Internetre irányuló terheléselosztó](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

A központi telepítést követően hello terheléselosztó hello Load Balancer terheléselosztók lista megjelenik.

![Terheléselosztók listája](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)

**8.2. A DNS-címke toohello nyilvános IP-hozzárendelése**

Kattintson az újonnan létrehozott hello terhelés terheléselosztó bejegyzést hello terhelés terheléselosztók panel toobring hello panel konfigurációjának mentése. Hajtsa végre az alábbi lépéseket tooconfigure hello DNS-címke hello nyilvános IP-cím:

1. Kattintson a hello nyilvános IP-címe. Megnyílik a hello panel hello nyilvános IP-cím és a beállítások
2. Kattintson a Konfiguráció lehetőségre.
3. Adja meg a DNS-címkét. Ez lesz hello nyilvános DNS-címke, amely bárhonnan, például contosofs.westus.cloudapp.azure.com végezheti el. Hozzáadhat egy bejegyzést a hello külső DNS hello összevonási szolgáltatás (például: fs.contoso.com), amely toohello DNS-címke a hello külső terheléselosztó (contosofs.westus.cloudapp.azure.com).

![Az internetre irányuló terheléselosztó konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Az internetre irányuló terheléselosztó konfigurálása (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Az internetre irányuló (nyilvános) terheléselosztó háttérkészletének konfigurálása** 

Ugyanaz, mint hello belső terheléselosztói, lépések követése hello tooconfigure hello háttérkészlet terheléselosztó Internet Facing (nyilvános), hello rendelkezésre állási beállítása hello WAP-kiszolgálókkal. Példa: contosowapset.

![Az internetre irányuló terheléselosztó háttérkészletének konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)

**8.4. Mintavétel konfigurálása**

Ugyanaz, mint konfigurálása hello belső terheléselosztói tooconfigure hello mintavétel hello háttérkészlet WAP-kiszolgálókkal, a lépések hello kövesse.

![Az internetre irányuló terheléselosztó mintavételének konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)

**8.5. Terheléselosztási szabály(ok) létrehozása**

Ugyanezek a lépések hasonlóan ILB tooconfigure hello terheléselosztási szabály a TCP 443-as hello kövesse.

![Az internetre irányuló terheléselosztó terheléselosztási szabályainak konfigurálása](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)

### <a name="9-securing-hello-network"></a>9. Hello hálózat védelme
**9.1. Hello belső alhálózat biztonságossá tétele**

A teljes tooefficiently biztonságos (sorrendben hello az alább felsorolt) a belső alhálózat szabályainak hello szüksége

| Szabály | Leírás | Folyamat |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Hello HTTPS-kommunikáció engedélyezése a Szegélyhálózaton |Bejövő |
| DenyInternetOutbound |Nincs hozzáférés toointernet |Kimenő |

![INT-hozzáférési szabályok (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[comment]: <> (![INT-hozzáférési szabályok (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [comment]: <> (![INT-hozzáférési szabályok (kimenő)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))

**9.2. Hello DMZ alhálózati biztonságossá tétele**

| Szabály | Leírás | Folyamat |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |HTTPS engedélyezése az internet toohello DMZ |Bejövő |
| DenyInternetOutbound |HTTPS toointernet kivételével bármely más le van tiltva |Kimenő |

![EXT-hozzáférési szabályok (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[comment]: <> (![EXT-hozzáférési szabályok (bejövő)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [comment]: <> (![EXT-hozzáférési szabályok (kimenő)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

> [!NOTE]
> Ha az ügyfelek esetében felhasználói tanúsítvány hitelesítésére (clientTLS típusú hitelesítés X509 felhasználói tanúsítványok használatával) van szükség, az AD FS-nél engedélyezni kell a 49443-as TCP-porton a bejövő hozzáférést.
> 
> 

### <a name="10-test-hello-ad-fs-sign-in"></a>10. Az AD FS hello bejelentkezés tesztelése
hello legegyszerűbb módja az AD FS segítségével hello IdpInitiatedSignon.aspx lap el tootest. A sorrend toobe képes toodo, hogy-e szükséges tooenable hello IdpInitiatedSignOn hello AD FS-tulajdonságok. Kövesse az alábbi tooverify hello lépéseket az AD FS beállítására

1. Futtatás hello parancsmagot a powershellel, tooset hello AD FS-kiszolgáló alatt azt tooenabled.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Egy külső gépről nyissa meg a következő címet: https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. AD FS hello oldal megjelenik alá, például:

![Bejelentkezési lap tesztelése](./media/active-directory-aadconnect-azure-adfs/test1.png)

A sikeres bejelentkezést követően a rendszer az alább látható üzenetet jeleníti meg:

![Teszt sikeres](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Sablon az AD FS üzembe helyezéséhez az Azure-ban
hello sablon egy 6 machine telepítése, 2 egyes tartományvezérlők, az AD FS és WAP telepíti.

[AD FS az Azure Deployment Template-ben](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

A sablon telepítése közben használhat egy meglévő virtuális hálózatot, vagy létrehozhat egy újat. hello hello telepítés testreszabása használható különböző paramétereket az alábbiak hello leírás használati hello paraméter hello telepítési folyamat. 

| Paraméter | Leírás |
|:--- |:--- |
| Hely |hello régió toodeploy hello erőforrások be, pl. USA keleti régiója. |
| StorageAccountType |hello típusú hello Storage-fiók létrehozása |
| VirtualNetworkUsage |Jelzi, hogy új virtuális hálózat lesz-e létrehozva vagy egy meglévő kerül használatra |
| VirtualNetworkName |hello virtuális hálózati tooCreate, mind a meglévő vagy új virtuális hálózat használata kötelező hello neve |
| VirtualNetworkResourceGroupName |Hello hello hello már meglévő virtuális hálózatot tartalmazó erőforráscsoport nevét adja meg. Meglévő virtuális hálózat használatával, ez lesz egy kötelező paraméter, így hello telepítési hello azonosító hello meglévő virtuális hálózat található |
| VirtualNetworkAddressRange |hello címtartományán hello új VNETET, kötelező, ha egy új virtuális hálózat létrehozása |
| InternalSubnetName |hello belső alhálózat, kötelezően telepítendő mindkét virtuális hálózat használati lehetőség (új vagy meglévő) hello neve |
| InternalSubnetAddressRange |a címtartomány hello hello belső alhálózat, amely tartalmazza a hello tartományvezérlők és AD FS kiszolgálók, kötelező, ha új virtuális hálózat létrehozása. |
| DMZSubnetAddressRange |a címtartomány hello hello dmz alhálózat, amely tartalmazza a hello Windows proxy kiszolgálók, kötelező, ha egy új virtuális hálózat létrehozása. |
| DMZSubnetName |hello neve hello belső alhálózat, kötelezően telepítendő mindkét virtuális hálózat használati (új vagy meglévő) lehetőséget. |
| ADDC01NICIPAddress |hello belső IP-címet az első tartományvezérlő hello, az IP-cím statikusan rendeli toohello DC és hello belső alhálózaton belüli érvényes IP-címnek kell lennie. |
| ADDC02NICIPAddress |hello belső IP-címe hello második tartományvezérlő, az IP-cím statikusan rendeli toohello DC és hello belső alhálózaton belüli érvényes IP-címnek kell lennie. |
| ADFS01NICIPAddress |hello belső IP-cím hello első AD FS kiszolgáló, az IP-cím statikusan rendeli toohello ADFS-kiszolgáló és hello belső alhálózaton belüli érvényes IP-címnek kell lennie. |
| ADFS02NICIPAddress |hello belső IP-címe hello második ADFS-kiszolgáló, az IP-cím statikusan rendeli toohello ADFS-kiszolgáló és hello belső alhálózaton belüli érvényes IP-címnek kell lennie. |
| WAP01NICIPAddress |hello belső IP-címe hello első WAP-kiszolgáló, az IP-cím statikusan rendeli toohello WAP-kiszolgáló és hello DMZ alhálózaton belüli érvényes IP-címnek kell lennie. |
| WAP02NICIPAddress |hello belső IP-címe hello második WAP-kiszolgáló, az IP-cím statikusan rendeli toohello WAP-kiszolgáló és hello DMZ alhálózaton belüli érvényes IP-címnek kell lennie. |
| ADFSLoadBalancerPrivateIPAddress |hello belső IP-címe az AD FS hello terheléselosztó, az IP-cím statikusan rendeli toohello terheléselosztó és hello belső alhálózaton belüli érvényes IP-címnek kell lennie. |
| ADDCVMNamePrefix |A tartományvezérlők virtuális gépnév előtagja |
| ADFSVMNamePrefix |Az ADFS-kiszolgálók virtuális gépnév előtagja |
| WAPVMNamePrefix |A WAP-kiszolgálók virtuális gépnév előtagja |
| ADDCVMSize |Virtuálisgép-méretet hello hello tartományvezérlők |
| ADFSVMSize |Virtuálisgép-méretet hello hello AD FS-kiszolgáló |
| WAPVMSize |Virtuálisgép-méretet hello hello WAP-kiszolgálókkal |
| AdminUserName |hello neve hello hello virtuális gépek helyi rendszergazda |
| AdminPassword |helyi rendszergazdai fiók hello hello virtuális gépek hello jelszavát |

## <a name="additional-resources"></a>További források
* [Rendelkezésre állási csoportok](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Belső terheléselosztó](https://aka.ms/Azure/ILB/Internal)
* [Internetre irányuló terheléselosztó](https://aka.ms/Azure/ILB/Internet)
* [Tárfiókok](https://aka.ms/Azure/Storage)
* [Azure virtuális hálózatok](https://aka.ms/Azure/VNet)
* [Az AD FS és a webalkalmazás-proxy hivatkozások](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Következő lépések
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
* [Az AD FS konfigurálása és felügyelete az Azure AD Connect segítségével](active-directory-aadconnectfed-whatis.md)
* [Az AD FS nagy rendelkezésre állású, több földrajzi régióra kiterjedő üzembe helyezése az Azure Traffic Managerrel](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

