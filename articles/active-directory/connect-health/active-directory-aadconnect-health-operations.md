---
title: Active Directory Connect Health operations aaaAzure
description: "Ez a cikk ismerteti a további műveleteket, miután telepítette az Azure AD Connect Health végrehajtható."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 1dddcee0bca3150ce08621c045a92a1b3ad9df30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-connect-health-operations"></a>Az Azure Active Directory Connect Health üzemeltetése
Ez a témakör ismerteti a hello különböző műveleteket hajthat végre Azure Active Directory (Azure AD) Connect Health használatával.

## <a name="enable-email-notifications"></a>E-mail értesítések engedélyezése
Hello Azure AD Connect Health szolgáltatás toosend értesítő konfigurálhatja, ha a riasztás azt jelzi, hogy a identitás-infrastruktúra nem működik megfelelően. Ez akkor fordul elő, amikor a rendszer riasztást állít elő, ha meg oldva.

![Képernyőfelvétel az Azure AD Connect Health e-mail értesítési beállítások](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> Értesítő e-mailek alapértelmezés szerint engedélyezve vannak.
>
>

### <a name="tooenable-azure-ad-connect-health-email-notifications"></a>tooenable az Azure AD Connect Health értesítő e-mailek
1. Nyissa meg hello **riasztások** tooreceive e-mail-értesítések kívánt hello szolgáltatás paneljét.
2. Hello műveletsávon kattintson **értesítési beállításainak**.
3. Hello e-mail értesítési kapcsoló válassza **ON**.
4. Jelölje be a hello jelölőnégyzetet, ha azt szeretné, hogy az összes globális rendszergazdák tooreceive értesítő e-mailek.
5. Ha azt szeretné, hogy tooreceive értesítő e-mailek, más e-mail címet, adja meg azokat a hello **további e-mailek címzettjeinek** mezőbe. tooremove egy e-mail címet a listából, kattintson a jobb gombbal a hello bejegyzést, és válassza ki **törlése**.
6. toofinalize hello módosításait, kattintson a **mentése**. Módosítások mentése után csak lépnek érvénybe.

## <a name="delete-a-server-or-service-instance"></a>Egy kiszolgáló vagy a szolgáltatás példány törlése

Bizonyos esetekben érdemes lehet a figyelt kiszolgáló tooremove. Mi szüksége tooknow tooremove hello Azure AD Connect Health szolgáltatás kiszolgáló.

Ha egy kiszolgálót törölni, hello következő figyelembe venni:

* Ez a művelet leáll, a kiszolgáló további adatok begyűjtését. A kiszolgáló szolgáltatás figyelése hello törlődik. Ez a művelet után nem képes tooview új riasztások, figyelés, vagy esetleg használati analitikai adatok ehhez a kiszolgálóhoz.
* Ez a művelet nem távolítja el a Health Agent hello a kiszolgálóról. Ha a lépés végrehajtása előtt nem távolította az hello rendszerállapot-ügynöke, hibák kapcsolódó toohello rendszerállapot-ügynöke jelenhet meg hello kiszolgálón.
* Ez a művelet nem törli a kiszolgálóról már begyűjtött hello adatok. El az adatok összhangban hello Azure adatmegőrzési házirend törlése.
* Ez a művelet elvégzése után ha szeretné toostart figyelési hello ugyanarra a kiszolgálóra ebben az esetben kell távolítsa el, majd telepítse újra a Health Agent hello ezen a kiszolgálón.

### <a name="toodelete-a-server-from-hello-azure-ad-connect-health-service"></a>toodelete hello Azure AD Connect Health szolgáltatás kiszolgáló
Az Azure AD Connect Health Active Directory összevonási szolgáltatások (AD FS) és az Azure AD Connect (Sync):

1. Nyissa meg hello **Server** hello a panel **kiszolgálólista** panel hello server name toobe kiválasztásával eltávolítja.
2. A hello **Server** paneljén hello műveletsávon kattintson **törlése**.
3. Győződjön meg róla, írja be a hello kiszolgálónév hello megerősítése mezőben.
4. Kattintson a **Törlés** gombra.

Az Azure AD Connect Health az Azure Active Directory tartományi szolgáltatások:

1. Nyissa meg hello **tartományvezérlők** irányítópult.
2. Jelölje be hello tartomány a tartományvezérlő toobe eltávolítva.
3. Hello műveletsávon kattintson **törli a kijelölt**.
4. Erősítse meg a hello művelet toodelete hello kiszolgáló.
5. Kattintson a **Törlés** gombra.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>A szolgáltatáspéldány törlése az Azure AD Connect Health szolgáltatással
Bizonyos esetekben érdemes lehet egy szolgáltatáspéldány tooremove. Ez mit kell tooknow tooremove szolgáltatás példány hello Azure AD Connect Health szolgáltatásból.

Amikor a szolgáltatáspéldány törölni, hello következő figyelembe venni:

* Ez a művelet eltávolítja hello aktuális szolgáltatáspéldányt hello szolgáltatás figyelése.
* Ez a művelet nem távolítja el hello rendszerállapot-ügynöke bármelyik hello kiszolgálók, a szolgáltatáspéldány részeként volt figyelése. Ha a lépés végrehajtása előtt nem távolította az hello rendszerállapot-ügynöke, hibák kapcsolódó toohello rendszerállapot-ügynöke jelenhet meg hello kiszolgálókon.
* A szolgáltatáspéldány összes adat törlődik a hello Azure adatmegőrzési házirend szerint.
* A művelet végrehajtása után toostart hello szolgáltatás figyelése, távolítsa el, majd telepítse újra a Health Agent hello hello minden olyan kiszolgálón. A művelet végrehajtása után ha azt szeretné, hogy a figyelést ugyanazon a kiszolgálón újra, távolítsa el, telepítse újra, és regisztrálja hello toostart hello rendszerállapot-ügynöke ezen a kiszolgálón.

#### <a name="toodelete-a-service-instance-from-hello-azure-ad-connect-health-service"></a>a szolgáltatáspéldány hello Azure AD Connect Health szolgáltatásból toodelete
1. Nyissa meg hello **szolgáltatás** hello a panel **lista** hello szolgáltatás azonosítója (farm neve), amelyet az tooremove kiválasztásával panelen.
2. A hello **Server** paneljén hello műveletsávon kattintson **törlése**.
3. Erősítse meg a hello megerősítő mezőbe írja be a hello szolgáltatás nevét (például: sts.contoso.com).
4. Kattintson a **Törlés** gombra.
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Hozzáférés kezelése a szerepköralapú hozzáférés-vezérlés
[Szerepköralapú hozzáférés-vezérlés (RBAC)](../role-based-access-control-configure.md) az Azure AD Connect Health biztosít hozzáférést toousers és a globális rendszergazdák eltérő csoportok. Az RBAC szerepkörök szánt toohello felhasználókat és csoportokat hozzárendeli, és lehetővé teszi a toolimit hello globális rendszergazdák a címtáron belül.

### <a name="roles"></a>Szerepkörök
Az Azure AD Connect Health támogatja a következő beépített szerepkörök hello:

| Szerepkör | Engedélyek |
| --- | --- |
| Tulajdonos |Tulajdonosai *hozzáférésének kezelése olyan* (például hozzárendelése egy szerepkörhöz tooa felhasználó vagy csoport), *összes információját megjeleníthetik* (például a riasztás megtekintése) hello portálról, és *beállításainak módosítása* () például e-mail értesítések) az Azure AD Connect Health belül. <br>Alapértelmezés szerint az Azure AD globális rendszergazdák vannak hozzárendelve ehhez a szerepkörhöz, és ez nem módosítható. |
| Közreműködő |A közreműködők is *összes adatot megjeleníthetik* (például a riasztás megtekintése) hello portálról, és *beállításainak módosítása* (például e-mail értesítések) az Azure AD Connect Health belül. |
| Olvasó |Olvasók is *összes információját megjeleníthetik* (például a riasztás megtekintése) az Azure AD Connect Health belül hello portálról. |

Minden más szerepkörök (például felhasználói rendszergazdák vagy a DevTest Labs felhasználók) belül az Azure AD Connect Health, nincs hatása tooaccess rendelkezik, akkor is, ha hello szerepkörök hello portál élmény érhető el.

### <a name="access-scope"></a>Hozzáférési hatókör
Az Azure AD Connect Health által támogatott két szintű hozzáférés-kezelés:

* **Minden szolgáltatáspéldány**: Ez az elérési út a legtöbb esetben ajánlott hello. Azt szabályozza a hozzáférést minden szolgáltatáspéldány (például AD FS-farm) az Azure AD Connect Health által figyelt összes szerepkör-típusa.
* **Szolgáltatáspéldány**: bizonyos esetekben szükség lehet a felhasználóiszerepkör-típusokhoz vagy egy szolgáltatás példánya alapján toosegregate hozzáférést. Ebben az esetben hozzáférését hello szolgáltatás példány szintjén kezelheti.  

Engedélyt kap, ha a felhasználó hozzáfér hello directory vagy a szolgáltatás szintű példányt.

### <a name="allow-users-or-groups-access-tooazure-ad-connect-health"></a>Lehetővé teszi a felhasználók vagy csoportok hozzáférés tooAzure AD Connect Health
hello következő lépések bemutatják, hogyan férnek hozzá az tooallow.
#### <a name="step-1-select-hello-appropriate-access-scope"></a>1. lépés: Hello megfelelő hozzáférési hatókör kiválasztása
egy felhasználó hozzáférést hello tooallow *minden szolgáltatáspéldány* szintű Azure AD Connect Health, nyissa meg hello Azure AD Connect Health fő paneljén belül.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>2. lépés: Felhasználók és csoportok hozzáadása, és szerepkörök hozzárendelése
1. A hello **konfigurálása** kattintson **felhasználók**.<br>
   ![Képernyőfelvétel az Azure AD Connect Health RBAC fő panelen a kijelölt felhasználókkal](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Válassza a **Hozzáadás** lehetőséget.
3. A hello **Szerepkörválasztás** ablaktáblán válassza ki a megfelelő szerepkör (például **tulajdonos**).<br>
   ![Képernyőfelvétel az Azure AD Connect Health RBAC felhasználók ablak](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Írja be a hello név vagy azonosító hello célzott felhasználó vagy csoport. Választhatja ki egy vagy több felhasználók vagy csoportok: hello ugyanannyi időt vesz igénybe. Kattintson a **Kiválasztás** gombra.
   ![Képernyőfelvétel az Azure AD Connect Health RBAC felhasználók ablak](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Kattintson az **OK** gombra.<br>
6. Szerepkör-hozzárendelés hello befejezése után hello felhasználók és csoportok szerepelnek hello listán.<br>
   ![Képernyőfelvétel az Azure AD Connect Health RBAC felhasználók-ablakot, kiemelve az új felhasználók](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Most hello felsorolt felhasználók és csoportok fér hozzá, szerepkört tootheir alapján történik.

> [!NOTE]
> * Globális rendszergazdák mindig rendelkezik teljes körű hozzáférési tooall hello műveleteket, de a globális rendszergazdai fiókok nem szerepelnek a listán megelőző hello.
> * hello felhasználók meghívása nem támogatja az Azure AD Connect Health belül.
>
>

#### <a name="step-3-share-hello-blade-location-with-users-or-groups"></a>3. lépés: Megosztás hello panel helye a felhasználók vagy csoportok
1. Engedélyek hozzárendelése után a felhasználó hozzáférhet-e az Azure AD Connect Health címen [Itt](http://aka.ms/aadconnecthealth).
2. Hello paneljén hello felhasználói rögzíthető hello panelen, vagy azt, toohello irányítópult különböző részeit. Egyszerűen kattintson a hello **PIN-kód toodashboard** ikonra.<br>
   ![Képernyőfelvétel az Azure AD Connect Health RBAC PIN-kód panelen, a rögzítés ikonja kiemelve](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> Hello olvasó szerepkörrel rendelkező felhasználók nem tud tooget az Azure AD Connect Health kiterjesztést a hello Azure piactéren. hello felhasználói hello szükséges "create" művelet toodo ezért nem lehet végrehajtani. hello felhasználói toohello panel által megelőző hivatkozást fog toohello elérheti. Későbbi használatra hello felhasználói hello panel toohello irányítópult is rögzítheti.
>
>

### <a name="remove-users-or-groups"></a>Felhasználók vagy csoportok eltávolítása
Eltávolíthatja, egy felhasználóhoz vagy csoporthoz hozzáadott tooAzure AD Connect Health RBAC. Egyszerűen kattintson a jobb gombbal a hello felhasználót vagy csoportot, és válassza **eltávolítása**.<br>
![Képernyőfelvétel az Azure AD Connect Health RBAC felhasználók-ablakot, távolítsa el a kijelölt](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="next-steps"></a>Következő lépések
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)
