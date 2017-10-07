---
title: "az Azure Naplóelemzés és hello OMS-portálon aaaManage munkaterületek |} Microsoft Docs"
description: "Az Azure Naplóelemzés és hello OMS-portálon a felhasználók, fiókok, munkaterületekkel és az Azure használatával számos felügyeleti feladatot munkaterületek kezelheti."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/06/2017
ms.author: magoedte
ms.openlocfilehash: 570e6c1f13ad28f120ecd65052d00c4ca986ac98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-workspaces"></a>Munkaterületek kezelése

toomanage hozzáférés tooLog elemzés, akkor hajtsa végre a különböző felügyeleti feladatok kapcsolódó tooworkspaces. Ez a cikk ismerteti a legjobb tanácsadás és eljárások toomanage munkaterületek. A munkaterület alapvetően a fiókkal kapcsolatos információk és hello fiók egyszerű konfigurációs adatait tartalmazó tároló. Ön vagy a szervezet más tagjai használhatja a több munkaterületek toomanage más-más részhalmazához az összes gyűjtött adatok vagy az informatikai infrastruktúra részeit.

a munkaterület toocreate, kell:

1. Azure-előfizetés.
2. Egy választott név a munkaterületnek.
3. Az előfizetés hello munkaterület társítani.
4. Földrajzi hely kiválasztása.

## <a name="determine-hello-number-of-workspaces-you-need"></a>A munkaterületek kell hello számának meghatározása
A munkaterület egy Azure-erőforrás és egy olyan tároló, ahol adatok gyűjtése, összesítve, elemzése, és szerepel a hello Azure-portálon.

Több munkaterületek Azure előfizetésenként akkor is, és a hozzáférés toomore mint egy munkaterület rendelkezhetnek. Hello több minimalizálja a munkaterület lehetővé teszi a tooquery és összefüggéseket között hello legtöbb adatot, mert ez nem lehetséges tooquery több munkaterületek között. Ez a szakasz ismerteti, amikor egy munkaterület nagyobb hasznos toocreate lehet-e.

A munkaterületek jelenleg a következőket biztosítják:

* Egy földrajzi helyet az adattárolás számára
* A számlázáshoz szükséges részletes adatokat
* Az adatok elkülönítését
* Konfiguráció hatóköre

Hello megelőző jellemzők alapján, érdemes lehet toocreate több munkaterületek ha:

* Globális vállalatként az adatokat elkülönítési vagy megfelelőségi okok miatt külön régiókban kell tárolnia.
* Azure használ, és azt szeretné, hogy a kimenő adatforgalom tooavoid azzal, hogy a munkaterület díjak hello ugyanabban a régióban, hello Azure-erőforrások kezeli.
* Érdemes tooallocate díjak toodifferent részlegek számára vagy üzleti csoportok használatát alapján. Egy munkaterület minden részleg vagy üzleti csoport létrehozásakor az Azure számlázás és a használati utasítás külön-külön mutatja az egyes munkaterületeken hello díjakat.
* Egy felügyelt szolgáltató, és minden más ügyféladatok különítve kezelheti az ügyfél tookeep hello log analytics-adatok kell.
* Több ügyfél kezel, és azt szeretné, hogy minden ügyfél / szervezeti egység vagy üzleti csoport toosee saját adatokat, de nem hello adatok mások számára.

Ügynökök toocollect adatok használata esetén is [minden ügynök tooreport tooone vagy további munkaterületek](log-analytics-windows-agents.md).

A System Center Operations Manager használata esetén az Operations Manager egyes felügyeleti csoportjai csak egy munkaterülethez csatlakoztathatók. Hello Microsoft Monitoring Agent telepítését Operations Manager által felügyelt számítógépekre, és hello ügynök jelentés tooboth Operations Manager és egy másik Naplóelemzési munkaterületet.

### <a name="workspace-information"></a>Munkaterület-információk

Részletes tudnivalók a munkaterületről a hello Azure-portálon tekintheti meg. Az OMS-portálon hello részleteit is megtekintheti.

#### <a name="view-workspace-information-in-hello-azure-portal"></a>Munkaterület-információk megtekintése a hello Azure-portálon

1. Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) használata az Azure-előfizetéshez.
2. A hello **Hub** menüben kattintson a **további szolgáltatások** hello az erőforrások listájához, írja be a **Naplóelemzési**. Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Kattintson a **Log Analytics** elemre.  
    ![Azure-központ](./media/log-analytics-manage-access/hub.png)  
3. Hello Naplóelemzési előfizetések panelen válassza ki a munkaterületen.
4. hello munkaterület panel hello munkaterületet, és hivatkozásokat tartalmaz további információt adatainak megjelenítése.  
    ![munkaterület részletei](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Fiókok és felhasználók kezelése
Egyes munkaterületeken lehet több fiók társítva, és minden fiókot (a Microsoft-fiókkal vagy szervezeti fiókkal) rendelkezhet hozzáférés toomultiple munkaterületek.

Alapértelmezés szerint a hello Microsoft-fiókkal vagy szervezeti fiók hello munkaterület létrehozó hello hello munkaterület rendszergazdája lesz.

Két engedély-modellek hozzáférési tooa Naplóelemzési munkaterület szabályozó van:

1. Örökölt Log Analytics felhasználói szerepkörök
2. [Azure szerepkör-alapú hozzáférés](../active-directory/role-based-access-control-configure.md)

a következő táblázat hello foglalja össze, hogy minden engedélyezési modelljének paranccsal állítható hello hozzáférést:

|                          | Log Analytics-portál | Azure Portal | API (a PowerShellt is beleértve) |
|--------------------------|----------------------|--------------|----------------------------|
| Log Analytics-beli felhasználói szerepkörök | Igen                  | Nem           | Nem                         |
| Azure szerepkör-alapú hozzáférés  | Igen                  | Igen          | Igen                        |

> [!NOTE]
> A Naplóelemzési mozgatása toouse Azure szerepköralapú hozzáférés hello engedélyek modellként hello Naplóelemzési felhasználói szerepkörök cseréje.
>
>

hello örökölt Naplóelemzési felhasználói szerepkörök csak vezérlésére végrehajtott hello az access tooactivities [Naplóelemzés portáljáról](https://mms.microsoft.com).

a következő tevékenységek is hello Azure engedélyek szükségesek:

| Műveletek                                                          | Azure-engedélyek szükségesek | Megjegyzések |
|-----------------------------------------------------------------|--------------------------|-------|
| Felügyeleti megoldások hozzáadása és eltávolítása                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | |
| Hello tarifacsomag módosítása                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Adatok megtekintése az hello *biztonsági mentés* és *Site Recovery* megoldás csempéket | Rendszergazda / Társadminisztrátor | Hozzáférések erőforrások hello klasszikus telepítési modell használatával telepítve. |
| Az Azure-portálon hello egy munkaterület létrehozása                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-toolog-analytics-using-azure-permissions"></a>TooLog Analytics kezelése elérni az Azure-engedélyekkel
toogrant hozzáférés toohello Naplóelemzési munkaterület Azure jogosultságokkal sikeresen telepítették, a hello lépésekkel [szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](../active-directory/role-based-access-control-configure.md).

Az Azure két beépített felhasználói szerepkört biztosít a Log Analyticshez:
- Log Analytics olvasó
- Log Analytics közreműködő

Hello tagjai *napló Analytics olvasó* szerepkör is:
- Az összes monitorozási adat megtekintése és keresése 
- A nézet a figyelési beállításokat, beleértve az összes Azure-erőforrások Azure diagnostics hello beállításainak megtekintése.

| Típus    | Engedély | Leírás |
| ------- | ---------- | ----------- |
| Műveletek | `*/read`   | Képes tooview összes erőforrások és az erőforrás-konfigurációban. Ez a következők megtekintését foglalja magában: <br> Virtuális gépi bővítmény állapota <br> Az Azure Diagnostics konfigurációja az erőforrásokon <br> Az összes erőforrás tulajdonságai és beállításai |
| Műveletek | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Képes tooperform naplófájl-keresési v2 lekérdezések |
| Műveletek | `Microsoft.OperationalInsights/workspaces/search/action` | Képes tooperform naplófájl-keresési v1 lekérdezések |
| Műveletek | `Microsoft.Support/*` | Képes tooopen támogatási eseteket |
|Nem művelet | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Megakadályozza, hogy a munkaterület kulcs szükséges toouse hello adatok gyűjtemény API és tooinstall ügynökök olvasása |


Hello tagjai *napló Analytics közreműködői* szerepkör is:
- Az összes monitorozási adat olvasása 
- Automation-fiókok létrehozása és konfigurálása
- Felügyeleti megoldások hozzáadása és eltávolítása
- A tárfiókkulcsok olvasása 
- Az Azure Storage-naplók gyűjtésének konfigurálása
- Azure-erőforrások monitorozási beállításainak szerkesztése, beleértve a következőket:
  - Virtuálisgép-bővítmény tooVMs hello hozzáadása
  - Az Azure Diagnostics konfigurálása az összes Azure-erőforráson

> [!NOTE] 
> Hello képességét tooadd egy virtuális gép bővítmény tooa virtuális gép toogain teljes ellenőrzést a virtuális gépek is használhatja.

| Engedély | Leírás |
| ---------- | ----------- |
| `*/read`     | Képes tooview összes erőforrások és az erőforrás-konfigurációban. Ez a következők megtekintését foglalja magában: <br> Virtuális gépi bővítmény állapota <br> Az Azure Diagnostics konfigurációja az erőforrásokon <br> Az összes erőforrás tulajdonságai és beállításai |
| `Microsoft.Automation/automationAccounts/*` | Képes toocreate és Azure Automation-fiók, beleértve a hozzáadása és szerkesztése a runbookok konfigurálása |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Hozzáadása, frissítése és virtuálisgép-bővítmények, beleértve a hello Microsoft Monitoring Agent bővítményt, és az OMS-ügynököt hello Linux-kiterjesztés eltávolítása |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Nézet hello tárfiók kulcsa. Szükséges tooconfigure Naplóelemzési tooread naplók az Azure storage-fiókok |
| `Microsoft.Insights/alertRules/*` | Riasztási szabályok hozzáadása, frissítése és eltávolítása |
| `Microsoft.Insights/diagnosticSettings/*` | Diagnosztikai beállítások hozzáadása, frissítése és eltávolítása az Azure-erőforrásokról |
| `Microsoft.OperationalInsights/*` | Konfigurációk hozzáadása, frissítése és eltávolítása a Log Analytics-munkaterületekről |
| `Microsoft.OperationsManagement/*` | Felügyeleti megoldások hozzáadása és eltávolítása |
| `Microsoft.Resources/deployments/*` | Üzemelő példányok létrehozása és törlése. Megoldások, munkaterületek és Automation-fiókok hozzáadásához és eltávolításához szükséges |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Üzemelő példányok létrehozása és törlése. Megoldások, munkaterületek és Automation-fiókok hozzáadásához és eltávolításához szükséges |

tooadd és eltávolítás tooa felhasználói szerepkör, szükség toohave `Microsoft.Authorization/*/Delete` és `Microsoft.Authorization/*/Write` engedéllyel.

A szerepkörök toogive felhasználók férhetnek hozzá más hatóköröknek a használatát:
- Előfizetés - hozzáférés tooall munkaterületek hello az előfizetéshez
- Erőforráscsoport - hozzáférés tooall munkaterület hello erőforráscsoportban található
- Erőforrás - hozzáférési tooonly hello megadott munkaterület

Használjon [egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md) toocreate szerepkör hello szükséges engedélyek.

### <a name="azure-user-roles-and-log-analytics-portal-user-roles"></a>Az Azure felhasználói szerepkörei és a Log Analytics-portál felhasználói szerepkörei
Ha rendelkezik legalább Azure olvasási jogosultsággal a hello Naplóelemzési munkaterület, hello kattintva megnyithatja a hello Log Analytics-portálról **OMS-portálon** feladat hello Naplóelemzési munkaterület megtekintésekor.

Hello Log Analytics portál megnyitásakor toousing hello örökölt Naplóelemzési felhasználói szerepkörök vált. Ha nem rendelkezik egy szerepkör-hozzárendelés hello Log Analytics-portálról, hello szolgáltatás [ellenőrzések hello Azure engedélyekkel rendelkezik a hello munkaterületen](https://docs.microsoft.com/rest/api/authorization/permissions#Permissions_ListForResource).
A szerepkör-hozzárendelést hello Log Analytics-portálról meghatározása az alábbiak szerint:

| Feltételek                                                   | Hozzárendelt Log Analytics-beli felhasználói szerepkör | Megjegyzések |
|--------------------------------------------------------------|----------------------------------|-------|
| A fiókja tagja tooa örökölt Naplóelemzési felhasználói szerepkör     | hello megadott Naplóelemzési felhasználói szerepkör | |
| A fiók nem tartozik tooa örökölt Naplóelemzési felhasználói szerepkör <br> Teljes Azure engedélyek toohello munkaterület (`*` engedély <sup>1</sup>) | Rendszergazda ||
| A fiók nem tartozik tooa örökölt Naplóelemzési felhasználói szerepkör <br> Teljes Azure engedélyek toohello munkaterület (`*` engedély <sup>1</sup>) <br> *nem a következő* műveletek: `Microsoft.Authorization/*/Delete` és `Microsoft.Authorization/*/Write` | Közreműködő ||
| A fiók nem tartozik tooa örökölt Naplóelemzési felhasználói szerepkör <br> Azure-beli olvasási engedély | Csak olvasási engedély ||
| A fiók nem tartozik tooa örökölt Naplóelemzési felhasználói szerepkör <br> Az Azure-engedélyeket nem ismerhetők fel | Csak olvasási engedély ||
| Felhőszolgáltató (CSP) által kezelt előfizetések esetén <br> hello fiók bejelentkezett a van hello Azure Active Directory kapcsolódó toohello munkaterület | Rendszergazda | Általában hello ügyfelének egy CSP-hez |
| Felhőszolgáltató (CSP) által kezelt előfizetések esetén <br> a bejelentkezett áll hello fiók nincs hello Azure Active Directory kapcsolódó toohello munkaterület | Közreműködő | Általában hello kriptográfiai Szolgáltató |

<sup>1</sup> tekintse meg a túl[Azure engedélyek](../active-directory/role-based-access-control-custom-roles.md) további információt a szerepkör-definíciók. Szerepkörök, művelettel kiértékelésekor `*` nincs egyenértékű túl`Microsoft.OperationalInsights/workspaces/*`.

Egyes pontok tookeep figyelembe kapcsolatos hello Azure-portálon:

* Ha bejelentkezik http://mms.microsoft.com használatával toohello OMS-portálon, megjelenik az hello **munkaterület kiválasztása** listája. Ez a lista csak azokat a munkaterületeket tartalmazza, ahol Log Analytics felhasználói szerepkörrel rendelkezik. toosee hello munkaterületek rendelkezik hozzáférési toowith Azure előfizetések toospecify a bérlő hello URL-cím részeként van szüksége. Például: `mms.microsoft.com/?tenant=contoso.com`. hello bérlői azonosító gyakran hello e-mail címét a toosign utolsó része.
* Ha azt szeretné, toonavigate közvetlen tooa portálon, hogy rendelkezik toousing Azure hozzáférési engedélyeket, majd kell toospecify hello erőforrás hello URL-cím részeként. Ez lehetséges tooget van a PowerShell használatával URL-CÍMÉT.

  Például: `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  hello URL-címet a következőhöz hasonló:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-hello-oms-portal"></a>Az OMS-portálon hello felhasználók kezelése
Kezelheti a felhasználók és csoportok a hello **felhasználók kezelése** lapon a hello **fiókok** hello-beállítások lap lapján.   

![felhasználók kezelése](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-tooan-existing-workspace"></a>Adja hozzá a felhasználó tooan meglévő munkaterület
A következő lépéseket tooadd egy felhasználó vagy csoport tooa munkaterület hello használata.

1. Az hello OMS-portálon, kattintson a hello **beállítások** csempére.
2. Kattintson a hello **fiókok** fülre, majd hello **felhasználók kezelése** fülre.
3. A hello **felhasználók kezelése** területen válassza ki a hello fiók típusa tooadd: **szervezeti fiók**, **Microsoft Account**, **Microsoft Support**.

   * Ha úgy dönt, hogy a Microsoft Account, írja be a Microsoft Account hello társított hello felhasználó hello e-mail címét.
   * Ha szervezeti fiók lehetőséget választja, adja meg felhasználói hello részét / csoport nevet vagy e-mail címét és a megfelelő felhasználók és csoportok listája megjelenik egy legördülő listában. Válasszon ki egy felhasználót vagy csoportot.
   * Használja a Microsoft Support toogive egy Microsoft Support engineer vagy más Microsoft alkalmazott ideiglenes hozzáférést tooyour munkaterület toohelp a hibaelhárításban.

     > [!NOTE]
     > Hello legjobb teljesítmény érdekében korlátozza az egy egyetlen OMS-fiók toothree társított Active Directory-csoportok hello számát – egy a rendszergazdák, egy a közreműködők és egy csak olvasható felhasználók számára. Több olyan csoportot használó hatással lehet a Naplóelemzési hello teljesítményét.
     >
     >
4. Válassza ki a felhasználó vagy csoport tooadd hello típusa: **rendszergazda**, **közreműködő**, vagy **ReadOnly felhasználói**.  
5. Kattintson az **Add** (Hozzáadás) parancsra.

   A Microsoft-fiók hozzáadásakor egy meghívó toojoin hello munkaterület toohello e-mailt a megadott küldött. Miután hello felhasználói hello meghívó toojoin OMS hello utasításait követi, hello felhasználó hozzáférhet hello munkaterületen.
   Egy szervezeti fiók hozzáadásakor hello felhasználó hozzáférhessen a Naplóelemzési azonnal.  

#### <a name="edit-an-existing-user-type"></a>Meglévő felhasználótípus hozzáadása
Hello fiók szerepkör OMS-fiókjához társított felhasználó módosíthatja. Hello alábbi szerepkör-beállítások közül választhat:

* *Rendszergazda*: Kezelheti a felhasználókat, látja a riasztásokat és reagálhat rájuk, hozzáadhat és eltávolíthat kiszolgálókat.
* *Közreműködő*: Látja a riasztásokat és reagálhat rájuk, hozzáadhat és eltávolíthat kiszolgálókat.
* *ReadOnly felhasználó*: A csak olvasási jogosultsággal rendelkező felhasználók nem tudják végrehajtani a következőket:

  1. Megoldások hozzáadása és eltávolítása. hello megoldás gyűjteménye van-e rejtve.
  2. Csempék hozzáadása, módosítása és eltávolítása a **Saját irányítópult** felületen.
  3. Nézet hello **beállítások** lapokat. hello lapok rejtve maradnak.
  4. Hello keresési nézetben, a Power bi konfiguráció, a mentett keresések és a riasztások feladatok rejtve maradnak.

#### <a name="tooedit-an-account"></a>egy fiók tooedit
1. Az hello OMS-portálon, kattintson a hello **beállítások** csempére.
2. Kattintson a hello **fiókok** fülre, majd hello **felhasználók kezelése** fülre.
3. Válassza ki a megjeleníteni kívánt toochange hello felhasználói szerepkör hello.
4. Hello művelet megerősítését kérő párbeszédpanelen kattintson a **Igen**.

### <a name="remove-a-user-from-a-workspace"></a>Felhasználó eltávolítása a munkaterületről
Következő lépések tooremove egy felhasználó egy olyan munkaterületről, hello használja. Eltávolításakor hello felhasználó nem zárja be a hello munkaterületen. Ehelyett eltávolítja a felhasználói és hello munkaterület hello társítását. Ha egy felhasználó több munkaterületek társítva, hogy a felhasználó továbbra is bejelentkezhetnek tooOMS és tekintse meg az egyéb munkaterületekkel.

1. Az hello OMS-portálon, kattintson a hello **beállítások** csempére.
2. Kattintson a hello **fiókok** fülre, majd hello **felhasználók kezelése** fülre.
3. Kattintson a **eltávolítása** , amelyet az tooremove következő toohello felhasználónevet.
4. Hello művelet megerősítését kérő párbeszédpanelen kattintson a **Igen**.

### <a name="add-a-group-tooan-existing-workspace"></a>Egy csoport tooan meglévő munkaterület hozzáadása
1. Az előző szakaszban "tooadd egy felhasználó tooan meglévő munkaterület" hello kövesse a lépéseket 1-4.
2. A **Felhasználó vagy csoport kiválasztása** pontban válassza a **Csoport** lehetőséget.  
   ![egy csoport tooan meglévő munkaterület hozzáadása](./media/log-analytics-manage-access/add-group.png)
3. Adja meg a megjelenített név vagy E-mail cím hello hello csoport tooadd szeretné.
4. Jelölje ki hello csoportot hello lista eredményeket, és kattintson a **Hozzáadás**.

## <a name="link-an-existing-workspace-tooan-azure-subscription"></a>Egy meglévő Azure-előfizetés munkaterület tooan hivatkozás
2016 szeptemberétől 26 után létrehozott összes munkaterületek csatolt tooan létrehozáskor Azure-előfizetéssel kell lennie. Ez a dátum előtt létrehozott munkaterületek csatolt tooa munkaterület kell lennie, amikor bejelentkezik. Hello munkaterület hello Azure-portálon való létrehozásakor, vagy ha a munkaterületet tooan Azure-előfizetéssel, az Azure Active Directory kapcsolódik, a szervezeti fiókkal.

### <a name="toolink-a-workspace-tooan-azure-subscription-in-hello-oms-portal"></a>a munkaterület tooan hello OMS-portálon az Azure-előfizetés toolink

- Hello OMS-portálon történő bejelentkezéskor felszólító tooselect Azure-előfizetés áll. Válassza ki, hogy szeretné, hogy toolink tooyour munkaterületet, és kattintson a hello előfizetést **hivatkozás**.  
    ![Azure-előfizetés társítása](./media/log-analytics-manage-access/required-link.png)

    > [!IMPORTANT]
    > az Azure-fiókjával toolink munkaterület, már rendelkeznie kell toolink milyen hozzáférési toohello munkaterületen.  Más szóval hello használt tooaccess hello Azure-portálon kell lennie **hello azonos** hello fiókként tooaccess hello munkaterületen használja. Ha nem, lásd: [adja hozzá a felhasználó tooan meglévő munkaterület](#add-a-user-to-an-existing-workspace).

### <a name="toolink-a-workspace-tooan-azure-subscription-in-hello-azure-portal"></a>a munkaterület tooan hello Azure-portálon az Azure-előfizetés toolink
1. Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).
2. Keresse meg a **Log Analytics** elemet, majd jelölje ki.
3. Ekkor megjelenik a meglévő munkaterületek listája. Kattintson az **Add** (Hozzáadás) parancsra.  
   ![munkaterületek listája](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. Az **OMS-munkaterület** részen kattintson a **Vagy meglévő társítása** gombra.  
   ![meglévő társítása](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Kattintson a **Kötelező beállítások konfigurálása** lehetőségre.  
   ![kötelező beállítások megadása](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Megjelenik a hello listáját, amelyek nincsenek munkaterületek még kapcsolódó tooyour Azure-fiók. Jelöljön ki egy munkaterületet.  
   ![munkaterület kijelölése](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Szükség esetén módosíthatja a következő elemek hello értékeit:
   * Előfizetés
   * Erőforráscsoport
   * Hely
   * Tarifacsomag  
     ![értékek módosítása](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Kattintson az **OK** gombra. hello munkaterület már csatolt tooyour Azure-fiók.

> [!NOTE]
> Ha nem látja hello munkaterület toolink szeretné, majd az Azure-előfizetés nem rendelkezik hozzáféréssel toohello munkaterület használatával hello OMS-portálon létrehozott.  toogrant hálózatelérési toothis fiókjával OMS-portálon hello, lásd: [adja hozzá a felhasználó tooan meglévő munkaterület](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-tooa-paid-plan"></a>A munkaterület tooa terv fizetős frissítése
Az OMS három munkaterület díjcsomagtípust ismer: **Ingyenes**, **Önálló** és **OMS**.  Ha a hello *szabad* tervezze meg, legfeljebb 500 MB / nap küldött tooLog Analytics adatokat.  Ha túllépi ezt a mennyiséget, akkor toochange a munkaterület fizetett tooa terv tooavoid nem végez adatgyűjtést meghaladja ezt a határt. A díjcsomag típusa bármikor módosítható.  Az OMS díjszabásról a [díjszabás](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing) leírásában tájékozódhat.

### <a name="using-entitlements-from-an-oms-subscription"></a>Az OMS-előfizetésből származó jogosultságok használata
toouse hello jogosultságok OMS E1, OMS E2 OMS vagy OMS-bővítmény a System Center megvásárlásáról érkező válasszon hello *OMS* terv az OMS szolgáltatáshoz.

Során vásárol egy OMS-előfizetést, hello jogosultságok tooyour nagyvállalati szerződés lettek hozzáadva. A jelen szerződés értelmében létrehozott Azure-előfizetés hello jogosultságok használhatja. Ezeket az előfizetéseket a összes munkaterületek hello OMS jogosultságok használja.

tooensure, hogy a munkaterület használata az OMS-előfizetés hello alkalmazott tooyour jogosultságok, kell:

1. A munkaterület hello nagyvállalati szerződés, amely tartalmazza az hello OMS-előfizetés részét képező Azure-előfizetés létrehozása
2. Jelölje be hello *OMS* hello munkaterület tervezése

> [!NOTE]
> Ha a munkaterület 2016 szeptemberétől 26 előtt készült, és a terv árképzési Naplóelemzési *prémium*, akkor a munkaterület használja az OMS-bővítmény hello jogosultságok a System Center. Használhatja a jogosultságok toohello módosításával *OMS* tarifacsomagra vált.
>
>

hello OMS előfizetés jogosultságok nem láthatók el a hello Azure vagy az OMS-portálon. Jogosultságok és az Enterprise Portal hello használata tekintheti meg.  

Ha toochange hello Azure-előfizetéssel, amely a munkaterület csatolva van szüksége, használhatja a hello Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) parancsmag.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Azure kötelezettségvállalás használata egy Nagyvállalati szerződésből
Ha nem rendelkezik az OMS-előfizetéssel, után kell fizetnie az egyes OMS-összetevőkhöz külön-külön és hello használati jelenik meg az Azure számlázásának.

Ha rendelkezik Azure pénzügyi kötelezettségvállalást a hello vállalati beléptetési toowhich kapcsolódnak az Azure-előfizetések, Naplóelemzési használata automatikusan fog tartozik hátralévő pénzügyi véglegesítési hello ellen.

Ha toochange hello munkaterület hello Azure-előfizetéshez van csatolva, használhatja az Azure PowerShell hello [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) parancsmag.  

### <a name="change-a-workspace-tooa-paid-pricing-tier-in-hello-azure-portal"></a>Módosítsa a munkaterület tooa tarifacsomag fizetett hello Azure-portálon
1. Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).
2. Keresse meg a **Log Analytics** elemet, majd jelölje ki.
3. Ekkor megjelenik a meglévő munkaterületek listája. Jelöljön ki egy munkaterületet.  
4. Hello munkaterület panelen a **általános**, kattintson a **tarifacsomag**.  
5. A **Tarifacsomag** lapon kattintással jelölje ki a tarifacsomagot, majd kattintson a **Kiválasztás** gombra.  
    ![díjcsomag kiválasztása](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Ha az Azure-portálon hello nézetének frissítését, megjelenik az **tarifacsomag** kiválasztott hello réteg frissítve.  
    ![frissített csomag](./media/log-analytics-manage-access/manage-access-change-plan04.png)

> [!NOTE]
> Ha a munkaterületet csatolt tooan Automation-fiók, mielőtt kiválaszthatja hello *önálló (/ GB)* tarifacsomag törölnie kell minden **automatizálási és vezérlés** megoldások és hello Automation leválasztása fiók. Hello munkaterület panelen a **általános**, kattintson a **megoldások** toosee és delete megoldásokat. toounlink hello Automation-fiókot, kattintson a hello hello az Automation-fiók neve hello **tarifacsomag** panelen.
>
>

### <a name="change-a-workspace-tooa-paid-pricing-tier-in-hello-oms-portal"></a>Módosítsa a munkaterület tooa fizetett tarifacsomag kiválasztása az hello OMS-portálon

toochange hello tarifacsomag hello OMS-portállal, Azure-előfizetéssel kell rendelkeznie.

1. Az hello OMS-portálon, kattintson a hello **beállítások** csempére.
2. Kattintson a hello **fiókok** fülre, majd hello **Azure-előfizetés & Díjcsomag** lapon.
3. Kattintson a kívánt toouse tarifacsomag hello.
4. Kattintson a **Save** (Mentés) gombra.  
   ![előfizetés és adatforgalmi díjcsomagok](./media/log-analytics-manage-access/subscription-tab.png)

Az új adatok terv hello OMS portál menüszalag hello felső a weblap jelenik meg.

![OMS menüszalag](./media/log-analytics-manage-access/data-plan-changed.png)


## <a name="change-how-long-log-analytics-stores-data"></a>A Log Analytics adattárolási idejének módosítása

Hello szabad IP-címek Log Analyticshez révén elérhető hello adatok az elmúlt hét napban.
Hello Standard tarifacsomagot Log Analyticshez révén elérhető hello utolsó 30 napnyi adat.
Hello prémium tarifacsomag Log Analyticshez révén elérhető hello az adatok utolsó 365 nap.
Hello önálló és tarifacsomagok alapértelmezés szerint OMS Naplóelemzési révén elérhető hello adatok az elmúlt 31 napra.

Hello önálló használata esetén és a tarifacsomagok OMS, Ön továbbra is too2 évnyi adat (730 nap). Hello alapértelmezett 31 napnál hosszabb adat azt eredményezi, azok háromszorosa adatok megőrzési járnak. Az árakkal kapcsolatos további információkért lásd a [többletköltségek](https://azure.microsoft.com/pricing/details/log-analytics/) című szakaszt.

Az adatmegőrzés toochange hello hossza:

1. Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).
2. Keresse meg a **Log Analytics** elemet, majd jelölje ki.
3. Ekkor megjelenik a meglévő munkaterületek listája. Jelöljön ki egy munkaterületet.  
4. Hello munkaterület panelen a **általános**, kattintson a **megőrzési**.  
5. Hello csúszkát tooincrease vagy hello ennyi napra visszamenőleg megőrzési csökkentése majd **mentése**.  
    ![adatmegőrzés módosítása](./media/log-analytics-manage-access/manage-access-change-retention01.png)

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Munkaterülethez tartozó Azure Active Directory-szervezet módosítása

Igény szerint módosíthatja a munkaterülethez tartozó Azure Active Directory-szervezetet. Változó hello Azure Active Directory szervezeti lehetővé teszi tooadd felhasználókat és csoportokat, hogy directory toohello munkaterületről.

### <a name="toochange-hello-azure-active-directory-organization-for-a-workspace"></a>toochange hello Azure Active Directory szervezeti munkaterület

1. Az OMS-portálon hello hello beállítások lapján kattintson a **fiókok** majd hello **felhasználók kezelése** fülre.  
2. Tekintse át a munkahelyi és iskolai fiókok hello információkat, majd **módosítás szervezet**.  
    ![szervezet módosítása](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Adja meg az Azure Active Directory-tartomány hello rendszergazda hello azonosító információkat. Ezt követően megjelenik egy visszaigazoló arról, hogy a munkaterület csatolt tooyour Azure Active Directory-tartományhoz.  
    ![társított munkaterületek visszaigazolása](./media/log-analytics-manage-access/manage-access-add-adorg02.png)


## <a name="delete-a-log-analytics-workspace"></a>Log Analytics-munkaterület törlése
Naplóelemzési munkaterület törlése, ha az összes adat kapcsolatos tooyour munkaterület töröltek, amelyek hello OMS szolgáltatás számított 30 napon belül.

Ha rendszergazdaként, és több felhasználó hello munkaterület társított, ezen felhasználók és a hello munkaterület hello társítását megszakad. Ha hello felhasználók hozzárendelve egyéb munkaterületekkel, majd tovább OMS használatával ezek munkaterületeket. Azonban, ha nincs hozzájuk társítva az egyéb munkaterületekkel majd van szükségük a munkaterület toouse OMS toocreate.

### <a name="toodelete-a-workspace"></a>a munkaterület toodelete
1. Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).
2. Keresse meg a **Log Analytics** elemet, majd jelölje ki.
3. Ekkor megjelenik a meglévő munkaterületek listája. Válassza ki a megjeleníteni kívánt toodelete hello munkaterületen.
4. Hello munkaterület paneljén kattintson **törlése**.  
    ![törlés](./media/log-analytics-manage-access/delete-workspace01.png)
5. Hello delete munkaterület megerősítő párbeszédpanele, kattintson **Igen**.

## <a name="next-steps"></a>Következő lépések
* Lásd: [csatlakozás Windows számítógépek tooLog Analytics](log-analytics-windows-agents.md) tooadd ügynökök és az adatok gyűjtéséhez.
* [Log Analytics-megoldások hozzáadása hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkciókat és összefog adatokat.
* [A Naplóelemzési proxy és tűzfal beállításainak](log-analytics-proxy-firewall.md) Ha a szervezet használja a proxykiszolgálón vagy tűzfalon, hogy az ügynökök kommunikálni képes hello Naplóelemzés szolgáltatás.
