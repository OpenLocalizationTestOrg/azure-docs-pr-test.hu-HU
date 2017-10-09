---
title: "VMware virtuális gép replikációs tooAzure (CSP program) aaaMulti-bérlő támogatása |} Microsoft Docs"
description: "Ismerteti, hogyan egy több-bérlős környezet tooorchestrate replikációs, feladatátvételének és helyreállításának az Azure Site Recovery toodeploy helyszíni VMware virtuális gépek (VM) tooAzure hello CSP programon keresztül hello Azure-portál használatával"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: manayar
ms.openlocfilehash: 9be555c9a438f66e6d3dfcdc9f507a84763846d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-tooazure-through-csp"></a>Több-bérlős támogatás az Azure Site Recovery VMware virtuális gépek tooAzure keresztül CSP replikálása

Az Azure Site Recovery támogatja a több-bérlős környezetekben bérlői előfizetések. Több vállalat kiszolgálása a bérlői előfizetések létrehozott és kezelt hello Microsoft Cloud Solution Provider (CSP) programon keresztül is támogatja. Ez a cikk részletesen telepítését és felügyeletét a több-bérlős VMware-Azure forgatókönyvek hello útmutatást. Azt is magában foglalja, létrehozását és kezelését a bérlői előfizetések CSP keresztül.

Ez az útmutató fokozottan megrajzolja a hello meglévő dokumentáció a VMware virtuális gépek tooAzure replikálására. További információkért lásd: [replikálása VMware virtuális gépek tooAzure a Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Több-bérlős környezetekben
Három fő több-bérlős modell van:

* **A megosztott üzemeltetési szolgáltatásokat szolgáltató (HSP)**: hello partner hello fizikai infrastruktúra tulajdonosa, és megosztott erőforrásokat (vCenter, adatközpontok, fizikai tárhelyet, és így tovább) használó toohost több bérlő futó virtuális gépek hello ugyanabban az infrastruktúrában. hello partner biztosíthat a vész-helyreállítási felügyeleti felügyelt szolgáltatásként, vagy hello bérlői önkiszolgáló megoldás vész-helyreállítási rendelkezhet.

* **Üzemeltetési szolgáltató dedikált**: hello partner birtokolja a fizikai infrastruktúra hello de használja a dedikált erőforrások (több Vcenter, fizikai datastores, és így tovább) toohost a külön infrastruktúra minden egyes bérlői virtuális gépekre. hello partner biztosíthat a vész-helyreállítási felügyeleti felügyelt szolgáltatásként, vagy hello bérlői rendelkezhet önkiszolgáló megoldás.

* **Felügyelt szolgáltatások szolgáltató (MSP)**: hello ügyfél hello virtuális gépeket üzemeltető fizikai infrastruktúra hello tulajdonában van, és hello partner biztosít a vész-helyreállítási engedélyezése és kezelése.

## <a name="shared-hosting-multi-tenant-guidance"></a>Több-bérlős megosztott üzemeltetési útmutató
Ez a fejezet hello megosztott üzemeltetési forgatókönyv részletesen. hello más két esetben hello megosztott üzemeltetési forgatókönyv részhalmaza, és hello használata azonos alapelveket. hello megosztott üzemeltetési útmutató hello végén hello különbségeket ismerteti.

hello egy több-bérlős forgatókönyvben alapvető vonatkozó követelmény akkor tooisolate különböző bérlők hello. Egy bérlő nem lehet képes tooobserve milyen más bérlőket birtokolt. Egy partner által felügyelt környezetben ez a követelmény nem ugyanolyan fontos, mert az egy önkiszolgáló környezetben, ahol azok kritikus. Ez az útmutató feltételezi, hogy a bérlők elszigetelésére szükséges.

a következő diagram hello hello architektúra oszlik:

![Egy vcenter megosztott HSP](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**A megosztott üzemeltetési a forgatókönyvet, és egy vCenter**

Hello a fenti ábrán látható, minden egyes ügyfélnek van, külön felügyeleti kiszolgáló. A konfigurációs korlátok bérlői tootenant-specifikus gépek elérésének, és lehetővé teszi a bérlők elkülönítését. A VMware virtuális gépek replikálásának forgatókönyv hello konfigurációs kiszolgáló toomanage fiókok toodiscover virtuális gépeket használ, és ügynökök telepítése. A Microsoft hello hajtsa végre több-bérlős környezetekben, a virtuális gép felderítési vCenter keresztül korlátozása hello hozzáadásával azonos elvek hozzáférés-vezérlést.

hello adatforgalom-elkülönítés követelmény, hogy minden bizalmas infrastrukturális információk (például a hozzáférési hitelesítő adatok) továbbra is nyilvánosságra tootenants teszi szükségessé. Ezért azt javasoljuk, hogy minden összetevő hello felügyeleti kiszolgáló hello partner hello kizárólagos vezérlése alatt maradnak. hello felügyeleti kiszolgáló-összetevők a következők:
* Konfigurációs kiszolgáló (CS)
* Folyamatkiszolgáló (PS)
* Fő célkiszolgáló (MT) 

A kibővített PS is hello partner ellenőrzés alatt áll.

### <a name="every-cs-in-hello-multi-tenant-scenario-uses-two-accounts"></a>Minden CS hello több-bérlős forgatókönyvben két fiókot használja

- **vCenter hozzáférési fiók**: a fiók toodiscover bérlői virtuális gépek használata. VCenter hozzáférési jogosultságait tooit (hello következő részben leírt) rendelkezik. toohelp hozzáférés véletlen kiszivárgásának elkerülésére, azt javasoljuk, hogy partnerek adja meg ezeket a hitelesítő adatokat, maguk hello konfigurálása eszköz.

- **Virtuális gép hozzáférési fiók**: a fiók tooinstall hello mobilitási ügynök használatát hello bérlői virtuális gépek egy automatikus leküldéses keresztül. Akkor általában egy olyan tartományi fiók, hogy a bérlő nyújthatnak tooa partner vagy, hogy azt is megteheti, hello partner előfordulhat, hogy kezelheti közvetlenül. Ha a bérlő nem szeretné, hogy tooshare hello részletek hello partnerrel közvetlenül, ő engedélyezése tooenter hello hitelesítő adatokat korlátozott idejű keresztül toohello CS eléréséhez, vagy a hello partner esetén kapott segítséggel mobilitási ügynökök manuális telepítése.

### <a name="requirements-for-a-vcenter-access-account"></a>A vCenter-hozzáférési fiók követelményei

Ahogyan az előző szakaszban hello, konfigurálnia kell egy különleges szerepkörrel tooit fiókkal hello CS. szerepkör-hozzárendelés hello kell toohello vCenter hozzáférési fiók minden egyes vCenter objektumra alkalmazza, és nem propagálja toohello gyermekobjektum. Ez a konfiguráció biztosítja a bérlők elszigetelésére, mert hozzáférési propagálás tooother objektumok véletlen hozzáférést eredményezhet.

![hello propagálása tooChild objektumok beállítás](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

hello alternatív tooassign hello felhasználói fiókkal és hello datacenter objektumazonosítóra szerepkör, és továbbíthatja őket toohello gyermekobjektum. Adja meg hello fiók egy *nem lehet hozzáférni* szerepkör legyen elérhetetlen tooa adott bérlő összes objektumhoz (például a többi bérlő virtuális gépeken). Ez a konfiguráció nehézkes, és véletlen hozzáférés-vezérléssel érheti el, mert minden új gyermekobjektum is hozzáférést kap a hello szülő örökölt. Ezért azt javasoljuk, hogy használja-e hello első módszer.

hello vCenter fiók-hozzáférési eljárás a következőképpen történik:

1. Új szerepkör létrehozása a klónozásával előre meghatározott hello *csak olvasható* szerepkört, majd adjon egy kényelmes nevét (például Azure_Site_Recovery, ebben a példában látható módon).

2. Rendelje hozzá a következő engedélyek toothis szerepkör hello:

    * **Datastore**: szóköz, Tallózás datastore, alacsony szintű fájlműveletek, távolítsa el a fájlt, frissítés virtuálisgép-fájlok foglalása

    * **Hálózati**: hálózati hozzárendelése

    * **Erőforrás**: rendelje hozzá a virtuális gép tooresource készlet, ki van kapcsolva a virtuális gép áttelepítése, az alkalmazás bekapcsolja a virtuális gép áttelepítése

    * **Feladatok**: a feladat, a frissítési feladat létrehozása

    * **Virtuális gép**: 
        * Konfigurációs > minden
        * Interakció > válaszoljon a kérdést, eszköz kapcsolat, CD konfigurálása media, konfigurálás hajlékonylemez media, kapcsolja ki a bekapcsolás, VMware-eszközök telepítése
        * Készlet > létrehozása meglévő, létrehozhat új, regisztrálása, Unregister
        * Kiépítés > engedélyezése virtuális gép letölthető, a virtuális gép fájlok feltöltése
        * Pillanatkép-kezelő > pillanatképek eltávolítása

    ![hello szerep szerkesztése párbeszédpanel](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Hozzáférési szintek toohello vCenter (használt fiók hello bérlői CS) a különböző objektumok, rendelje hozzá a következőképpen:

>| Objektum | Szerepkör | Megjegyzések |
>| --- | --- | --- |
>| vCenter | Csak olvasható | Csak tooallow vCenter különböző objektumok kezeléséhez szükséges hozzáférési. Ezzel az engedéllyel is eltávolíthat, ha hello fiókot soha nem lesz toobe megadott tooa bérlői vagy hello vcenter felügyeleti műveleteket sem használja. |
>| Adatközpont | Azure_Site_Recovery |  |
>| Gazdagép és a gazdagépfürtön | Azure_Site_Recovery | Újra biztosítja, hogy hozzáférési hello objektum szinten, hogy csak elérhető gazdagépeken a bérlői virtuális gépek a feladatátvétel előtt és után feladat-visszavételre. |
>| Datastore és adattároló-fürt | Azure_Site_Recovery | Ugyanaz, mint megelőző. |
>| Network (Hálózat) | Azure_Site_Recovery |  |
>| Felügyeleti kiszolgáló | Azure_Site_Recovery | Tartalmazza az access tooall összetevőket (CS PS és fő Célkiszolgáló), ha bármelyik hello CS gép kívül. |
>| Bérlői virtuális gépek | Azure_Site_Recovery | Biztosítja, hogy minden új bérlői virtuális gépek egy adott bérlő is a hozzáférés, vagy nem is felderíthető hello Azure-portálon keresztül. |

hello vCenter felhasználóifiók-hozzáférés befejeződött. Ez a lépés hello minimálisan szükséges engedélyeket követelmény toocomplete feladat-visszavétel műveletek teljesíti. A hozzáférési engedélyeket is használható a meglévő szabályzatokat. Csak a meglévő engedélyek beállítása tooinclude szerepkör engedélyeinek módosításához 2,. lépésben részletes korábban.

amíg hello feladatátadást végez toorestrict vész-helyreállítási műveletek (Ez azt jelenti, hogy feladat-visszavétel kezelésére), kövesse az előző eljárás, egy kivétellel hello: ne hello *Azure_Site_Recovery* szerepkör toohello vCenter-hozzáférési fiókként, hozzárendelése csak egy *írásvédett* szerepkör toothat fiók. Ez az engedélycsoport lehetővé teszi a virtuális gép replikációs és feladatátvételi, és nem teszi lehetővé feladat-visszavételre. Minden más, a folyamat megelőző hello marad van. tooensure bérlők elkülönítésének, és korlátozhatja a virtuális gép felderítése, minden engedély továbbra is hozzá van rendelve: hello objektum szint csak, és nem propagált toochild objektumok.

## <a name="other-multi-tenant-environments"></a>Más több-bérlős környezetekben

a szakasz ismerteti, hogyan megelőző hello tooset egy több-bérlős környezet megosztott üzemeltetési megoldás. hello más két fő megoldások is dedikált futtató és a felügyelt szolgáltatást. Ezek a megoldások hello architektúra hello a következő részekben ismertetett.

### <a name="dedicated-hosting-solution"></a>Dedikált üzemeltetési megoldás

Ahogy az a következő diagram hello, hello architekturális egy dedikált üzemeltetési megoldás különbség, hogy a bérlő infrastruktúra csak a bérlő van beállítva. Bérlők el választva egymástól külön Vcenter keresztül, mert a hello szolgáltató továbbra is végre kell hajtaniuk a megosztott üzemeltetési előírt hello CSP lépést, de nem kell a bérlők elszigetelésére kapcsolatos tooworry. Kriptográfiai Szolgáltató beállítása változatlan marad.

![architektúra megosztott hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Dedikált üzemeltetés több Vcenter**

### <a name="managed-service-solution"></a>Felügyelt megoldás

Látható hello alábbi ábrán, hello architekturális különbség a felügyelt megoldás lesz, hogy minden bérlő infrastruktúrát is fizikailag nem más bérlők infrastruktúra. Ebben a forgatókönyvben általában akkor beszélünk, amikor hello bérlői hello infrastruktúra tulajdonosa, és a megoldás szolgáltató toomanage vész-helyreállítási szeretné. Ebben az esetben bérlőkre fizikailag elkülönített különböző infrastruktúrák keresztül, mert hello partner toofollow hello CSP itt ismertetett lépéseket megosztott üzemeltetéséhez szükséges tartalmaz, de nem kell a bérlők elszigetelésére kapcsolatos tooworry. Kriptográfiai Szolgáltató kiépítés változatlan marad.

![architektúra megosztott hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Felügyelt szolgáltatást a forgatókönyvet, és több Vcenter**

## <a name="csp-program-overview"></a>Kriptográfiai Szolgáltató program áttekintése
Hello [CSP program](https://partner.microsoft.com/en-US/cloud-solution-provider) elősegíti a megfelelőbb együtt szövegek által biztosított partnerek minden Microsoft felhőszolgáltatásai, beleértve az Office 365, a nagyvállalati mobilitási csomag és a Microsoft Azure. CSP partnereink hello végpontok közötti kapcsolat saját ügyfelekkel és válnak hello kapcsolat elsődleges kapcsolódási pontot. Partnerek Azure-előfizetések ügyfelek esetén telepítheti és kombinálhatja hello előfizetéssel, valamint a saját hozzáadott, testreszabott ajánlatokat.

Az Azure Site Recovery partnerek hello teljes vész-helyreállítási megoldás felügyelheti az ügyfelek közvetlenül a CSP-hez. Vagy használja fel a Site Recovery környezetek CSP tooset és tudassa az ügyfelekkel önkiszolgáló módon kezelhetik a saját vész-helyreállítási igényekre. Mindkét esetben a partnerek olyan hello összekötő Site Recovery és az ügyfelek között. Partnerek ügyfélkapcsolat hello szolgáltatást, és a Site Recovery használata az ügyfelek kiszámlázni.

## <a name="create-and-manage-tenant-accounts"></a>Bérlői fiókok létrehozása és kezelése

### <a name="step-0-prerequisite-check"></a>0. lépés: Az előfeltétel-ellenőrzés

hello VM Előfeltételek vannak hello hello leírtak szerint [Azure Site Recovery dokumentáció](site-recovery-vmware-to-azure.md). A hozzáadása toothose Előfeltételek meg kell rendelkezik hello említett hozzáférés intézkedések CSP bérlő kezelése előtt. Az egyes bérlők számára hozzon létre egy külön felügyeleti kiszolgáló, amely képes kommunikálni a hello bérlői virtuális gépek és a partner által létrehozott vCenter. Csak hello partner hozzáférési jogok toothis server van.

### <a name="step-1-create-a-tenant-account"></a>1. lépés: Bérlői fiók létrehozása

1. Keresztül [Microsoft Partner Center](https://partnercenter.microsoft.com/), jelentkezzen be tooyour CSP-fiókjával. 
 
2. A hello **irányítópult** menü **ügyfelek**.

    ![hello Microsoft Partner Center felhasználóinak hivatkozás](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. A megnyíló hello lapon kattintson a hello **Hozzáadás ügyfél** gombra.

    ![hello ügyfél hozzáadása gomb](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. A hello **új ügyfél** lapon adja meg a fiók adatai hello hello bérlő számára, és kattintson a **tovább: előfizetések**.

    ![hello fiók adatai lap](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Hello előfizetések kiválasztása lapon válassza ki a hello **Microsoft Azure** jelölőnégyzetet. Most, vagy más bármikor másik előfizetés is hozzáadhat.

    ![hello Microsoft Azure-előfizetés jelölőnégyzetet](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. A hello **felülvizsgálati** lapon erősítse meg hello bérlői adatokat, és kattintson a **Submit**.

    ![hello nyomtatási kép](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Hello bérlői fiók létrehozása után megjelenik-e a Jóváhagyás lap, hello hello részleteit megjelenítő alapértelmezett fiók és hello az adott előfizetéshez tartozó jelszót. 

7. Mentse hello adatokat, és módosítani hello jelszavát később szükséges hello Azure portál bejelentkezési oldalának keresztül.  
 
    Ezek az információk megosztása hello bérlői, vagy létrehozhat és megosztani egy külön fiókot, ha szükséges.

### <a name="step-2-access-hello-tenant-account"></a>2. lépés: Hello bérlői fiók eléréséhez.

Érheti el hello bérlői előfizetéshez hello Microsoft Partner Center irányítópultjának, lásd: "1. lépés: bérlői fiók létrehozása." 

1. Nyissa meg toohello **ügyfelek** lapon, és kattintson a hello hello bérlői fiók nevét.

2. A hello **előfizetések** lap hello bérlői fiók, figyelheti hello meglévő fiók-előfizetések, és adja hozzá legalább egy előfizetésre, szükség szerint. Válassza ki a vész-helyreállítási műveletek toomanage hello bérlői, **összes erőforrást (Azure-portál)**.

    ![hello összes erőforrás-hivatkozás](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Kattintson a **összes erőforrás** hozzáférést biztosít toohello bérlői Azure-előfizetések. Hozzáférés hello hello Azure Active Directory hivatkozására kattintva ellenőrizheti az hello Azure-portálon jobb.

    ![Az Azure Active Directory-hivatkozás](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Mostantól hello bérlői hello Azure-portálon keresztül az összes webhely-helyreállítási műveleteinek elvégzéséhez és hello vész-helyreállítási műveleteinek a felügyeletét. tooaccess hello bérlői előfizetéshez CSP keresztül felügyelt vész-helyreállítási, kövesse hello korábban leírt folyamatot.

### <a name="step-3-deploy-resources-toohello-tenant-subscription"></a>3. lépés: Erőforrások toohello bérlői előfizetéshez központi telepítése
1. A hello Azure-portálon hozzon létre egy erőforráscsoportot, és a Recovery Services-tároló hello szokásos folyamatonként telepíteni. 
 
2. Hello tárolóbeli regisztrációs kulcs letöltése.

3. Regisztrálja a hello CS hello bérlő hello tárolóbeli regisztrációs kulcs használatával.

4. Adja meg a két hello hozzáférési fiókok hello hitelesítő adatait: vCenter hálózatelérési fiók és a virtuális gép fér hozzá a fiókjához.

    ![Kezelői konfigurációs kiszolgáló fiókok](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-toohello-recovery-services-vault"></a>4. lépés: Regisztráció a Site Recovery-infrastruktúra toohello Recovery Services-tároló
1. Az Azure-portálon, a korábban létrehozott hello tároló hello, regisztrálja hello vCenter server toohello CS a regisztrált "3. lépés: erőforrások toohello bérlői előfizetéshez telepítése." Hello vCenter hálózatelérési fiókot használja erre a célra.
2. Fejezze be a Site Recovery hello "Infrastruktúra előkészítése" folyamat hello szokásos folyamatonként.
3. hello virtuális gépek most már készen áll a replikált toobe áll. Győződjön meg róla, hogy csak hello bérlői virtuális gépek láthatók a hello **válassza ki a virtuális gépek** részen hello **replikálása** lehetőséget.

    ![A bérlői virtuális gépek listájában a hello válassza virtuális gépek panelje](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-toohello-subscription"></a>5. lépés: Bérlői hozzáférés toohello előfizetés hozzárendelése

Az adatok önkiszolgáló helyreállítását, adja meg toohello bérlői hello fiók adatait, hello 6. lépésben említett "1. lépés: bérlői fiók létrehozása" szakasz. A művelet végrehajtása után hello partner hello vész-helyreállítási infrastruktúra beállítása. Hogy hello vész-helyreállítási típus kezelt vagy az önkiszolgáló, partnerek hello CSP portálon keresztül hozzáférést bérlői előfizetések kell. Ezek hello partner által birtokolt tároló beállítását, és infrastruktúra toohello bérlői előfizetések regisztrálni.

Partnerek is hozzáadhat egy új felhasználó toohello bérlői előfizetéshez hello CSP portálon keresztül hello következő tevékenységek végrehajtásával:

1. Nyissa meg toohello bérlői CSP-előfizetés lapján, majd válassza ki a hello **felhasználók és licencek** lehetőséget.

    ![hello bérlő CSP-előfizetés lapján](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Most vagy is létrehozhat egy új felhasználó belépés hello vonatkozó adatokat, majd válassza az engedélyek hello azoknak a felhasználóknak a CSV-fájl feltöltésével.

2. Új felhasználó létrehozása után visszaléphet toohello Azure portál, majd a hello **előfizetés** panelen, jelölje be hello vonatkozó előfizetést.

3. A megnyíló hello panelen válassza ki a **hozzáférés-vezérlés (IAM)**, és kattintson a **Hozzáadás** tooadd hello vonatkozó hozzáférési szinttel rendelkező felhasználó.      
    hello CSP portálon létrehozott hello felhasználók automatikusan megjelenik a hello panel, amely egy hozzáférési szint gombra kattintva megnyílik.

    ![Felhasználó hozzáadása](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    A legtöbb felügyeleti műveleteket, hello *közreműködő* szerepkör is elegendő. Ez a hozzáférési szint rendelkező felhasználók is mindent meg azzal a különbséggel hozzáférési szint módosítása előfizetés (amelynek *tulajdonos*-szintű hozzáféréssel kell). Úgy is finomhangolhatja hello hozzáférési szintek szükség szerint.
