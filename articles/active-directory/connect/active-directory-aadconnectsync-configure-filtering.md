---
title: "Azure AD Connect szinkronizálása: szűrésének beállítása |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooconfigure szűrést a Azure AD Connect szinkronizálási szolgáltatás."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 97979b508c560a6de6cb091b1b621bc1d51b25c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Az Azure AD Connect szinkronizálása: a szűrés konfigurálása
Szűrés segítségével szabályozhatja mely objektumok jelennek meg az Azure Active Directory (Azure AD) a helyi címtárban lévő. hello alapértelmezett konfiguráció összes objektum minden tartományban konfigurált hello erdőkben vesz igénybe. Általában ez az ajánlott konfiguráció hello. Felhasználók Office 365-munkaterhelések, például az Exchange Online és Skype vállalati verzióra, a kihasználhassa a teljes globális címlista, hogy e-mailek küldése és mindenki hívja. Hello alapértelmezett konfigurációval akkor hello azonos tapasztalhat, hogy egy helyszíni Exchange-hez vagy a Lync végrehajtásának kell azokat.

Egyes esetekben azonban Ön szükséges egyes módosítások toohello alapértelmezett konfigurációs. Néhány példa:

* Azt tervezi, hogy toouse hello [több Azure Active directory-topológia](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Akkor kell a szűrő toocontrol, mely objektumok érhetők szinkronizált tooa bizonyos Azure Active directory tooapply.
* A próbaüzem futtatja az Azure vagy Office 365, és csak kívánt a felhasználók egy része az Azure ad-ben. Hello kis próbaüzem nincs fontos toohave egy teljes globális címlista toodemonstrate hello funkciót.
* Számos szolgáltatás és egyéb nem kívánt Azure AD-ben megbízhatóságának fiókok rendelkezik.
* Megfelelőségi okokból bármely felhasználói fiókok a helyszíni ne törölje. Csak letilthatja azokat. De az Azure AD-csak aktív fiókok toobe található.

Ez a cikk ismerteti, hogyan tooconfigure hello különböző szűrési módszerek.

> [!IMPORTANT]
> A Microsoft nem támogatja a módosítása, vagy az Azure AD Connect szinkronizálási hello műveletek hivatalosan ismertetett kívül működő. Ezek a műveletek okozhatja az Azure AD Connect szinkronizálási szolgáltatás inkonzisztens vagy nem támogatott állapotban. Ennek eredményeképpen a Microsoft nem adhatók meg a technikai támogatási szolgálathoz az ilyen környezetekben.

## <a name="basics-and-important-notes"></a>Alapvető tudnivalók és fontos megjegyzések
Az Azure AD Connect szinkronizálási szolgáltatás engedélyezheti a bármikor szűrést. Indítsa el a címtár-szinkronizálás alapértelmezett konfigurációja, és a szűrés konfigurálása, ha ki van szűrve hello objektumok nem lesznek szinkronizált tooAzure AD. Ez a változás miatt objektumokat az Azure ad-ben korábban szinkronizált, de majd szűrve voltak törlődnek az Azure ad-ben.

Módosítások toofiltering elvégzése előtt győződjön meg arról, hogy Ön [hello ütemezett feladat letiltása](#disable-scheduled-task) , akkor véletlenül ne exportálja a módosítások még nem még ellenőrizte, hogy helyes-e toobe.

Mert szűrés eltávolítása hello sok objektumait azonos idő, szeretne arról, hogy az új szűrőket helyesen-e bármely exportálása előtt toomake tooAzure AD változik. Miután végrehajtotta hello konfigurációs lépések, Határozottan javasoljuk, hogy kövesse a hello [ellenőrzési lépések](#apply-and-verify-changes) módosítások tooAzure AD ellenőrizze és exportálása előtt.

a beállítást, akkor törölje a nagy objektumokat, véletlenül hello tooprotect "[véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)" alapértelmezés szerint be van kapcsolva. Ha sok objektumot miatt törli toofiltering (alapértelmezés szerint 500), ez a cikk tooallow hello toofollow hello lépéseket kell törli toogo tooAzure AD keresztül.

2015. November előtti használatakor a build ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) olyan módosítás tooa szűrő konfigurációs és a jelszó-szinkronizálás használja, akkor szükséges, a teljes szinkronizálás az összes jelszavak tootrigger hello konfiguráció befejezése után. Hogyan tootrigger jelszó teljes szinkronizálás lépéseiért lásd: [indul el, a teljes szinkronizálás az összes jelszavak](active-directory-aadconnectsync-troubleshoot-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Ha a build 1.0.9125 vagy újabb, majd hello regular **teljes szinkronizálás** művelet is számítja ki, hogy szinkronizálni, és ha ezt a kiegészítő lépést már nincs szükség.

Ha **felhasználói** objektumok véletlenül törölték az Azure AD egy szűrési hiba miatt, létrehozhatja hello felhasználói objektumok Azure AD-ben a szűrési beállítások eltávolításával. Majd újból szinkronizálhatja a címtárakat. Ez a művelet visszaállítja az hello felhasználók hello Lomtárból Azure AD-ben. Más típusú objektumokat azonban nem törlés visszavonása. Például ha véletlenül törli egy biztonsági csoportot, és a használt tooACL erőforrás volt, hello csoport és a hozzáférés-vezérlési listák nem állítható helyre.

Az Azure AD Connect csak törli az objektumok, hogy egyszer figyelembe toobe hatókörében. Ha egy másik szinkronizálási motor által létrehozott objektumok az Azure ad-ben, és ezeket az objektumokat nem, a hatókör, hozzáadása a szűrés nem távolítja azokat. Például ha megkezdése a teljes címtár teljes másolatának az Azure ad-ben létrehozott DirSync-kiszolgálóval, és egy új Azure AD Connect szinkronizálási kiszolgáló telepít szűrése engedélyezve hello elejétől párhuzamosan, az Azure AD Connect nem távolítja el a felesleges hello a DirSync által létrehozott objektumok.

Szűrés konfigurációs hello megőrződik telepíteni vagy frissíteni az Azure AD Connect tooa újabb verziója. Az mindig a bevált gyakorlat tooverify, amely a konfigurációs hello véletlenül nem változik a frissítési tooa újabb verziójú hello első szinkronizálási ciklust futtatása előtt.

Ha egynél több erdővel rendelkezik, akkor telepítenie kell az ebben a témakörben tooevery erdő ismertetett konfigurációval szűrés hello (feltéve, hogy a kívánt hello azonos konfigurációját az összes).

### <a name="disable-hello-scheduled-task"></a>Tiltsa le a hello ütemezett feladat
toodisable hello beépített ütemezési, amely elindítja a szinkronizálási ciklust 30 percenként, kövesse az alábbi lépéseket:

1. Nyissa meg tooa PowerShell-promptot.
2. Futtatás `Set-ADSyncScheduler -SyncCycleEnabled $False` toodisable hello Feladatütemező.
3. Változtatásokat hello szerepelnek ebben a cikkben.
4. Futtatás `Set-ADSyncScheduler -SyncCycleEnabled $True` tooenable hello Feladatütemező újra.

**Ha az Azure AD Connect build 1.1.105.0 előtt**  
toodisable hello ütemezett feladatot, amely elindítja a szinkronizálási ciklust 3 óra, kövesse az alábbi lépéseket:

1. Start **Feladatütemező** a hello **Start** menü.
2. Közvetlenül a **Feladatütemező könyvtár**, keresés hello feladat nevű **Azure AD Sync Scheduler**, kattintson a jobb gombbal, majd válassza **letiltása**.  
   ![A Feladatütemező](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Mostantól konfigurációs módosításokat és hello szinkronizálási motor manuálisan futtassa a hello **Synchronization Service Managert** konzol.

A szűrési módosítások elvégzése után ne feledje toocome vissza és **engedélyezése** hello feladat újra.

## <a name="filtering-options"></a>Szűrési beállítások
A következő szűrési konfigurációs típusok toohello címtár-Szinkronizáló eszköz hello alkalmazhatja:

* [**Csoportalapú**](#group-based-filtering): szűrés egyetlen csoport alapján csak konfigurálható a kezdeti telepítés hello telepítés varázsló használatával.
* [**Tartományi**](#domain-based-filtering): Ez a beállítás használatával kiválaszthatja, mely tartományok tooAzure AD szinkronizálása. Is hozzá és távolíthat el tartományokat abból hello szinkronizálási motor konfigurációját, amikor módosításokat tooyour a helyszíni infrastruktúra az Azure AD Connect szinkronizálási szolgáltatás telepítése után.
* [**Szervezeti egység (OU) – alapú**](#organizational-unitbased-filtering): Ez a beállítás használatával kiválaszthatja, melyik szervezeti egységek tooAzure AD szinkronizálása. Ez a beállítás akkor a kiválasztott szervezeti minden objektum esetében.
* [**Attribútumalapú**](#attribute-based-filtering): Ez a beállítás használatával objektumok hello objektumokon attribútumértékei alapján szűrheti. A különböző objektumtípusokra különböző szűrőket is lehet.

Használhat több szűrési beállítások hello ugyanannyi időt vesz igénybe. Például használhatja OU-alapú szűrés tooonly objektumokat tartalmaznak egy szervezeti egységben. At hello azonos időben, Attribútumalapú szűrési toofilter hello objektumok további használható. Több szűrési módszerek, hello szűrők használata logikai "és" hello szűrők között.

## <a name="domain-based-filtering"></a>Tartományalapú szűrés
Ez a rész nyújt hello lépéseket tooconfigure a tartomány szűrő. Ha hozzáadott vagy eltávolított tartományok az erdőben, az Azure AD Connect telepítése után, akkor is tooupdate hello konfigurációs szűrést.

hello előnyben részesített módon toochange tartományalapú szűrés hello telepítési varázslót és módosítása [tartományok és szervezeti egységek szűrése](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). hello telepítővarázsló üdvözlő feladataival ebben a témakörben ismertetett automatizálja.

Ha valamilyen okból kifolyólag nem toorun hello telepítővarázsló most csak kövesse ezeket a lépéseket.

Szűrési konfigurálása tartományi alábbi lépésekből áll:

1. [Válassza ki a hello tartományok](#select-domains-to-be-synchronized) , amelyet az tooinclude hello szinkronizálásban.
2. Az egyes felvétele, illetve eltávolítása a tartományhoz, módosítsa a hello [futtatási profil](#update-run-profiles).
3. [Alkalmazza, és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

### <a name="select-hello-domains-toobe-synchronized"></a>Jelölje be hello tartományok toobe szinkronizálása
tooset hello tartomány szűréséhez hello a következő lépéseket:

1. Jelentkezzen be, amelyen fut az Azure AD Connect szinkronizálási szolgáltatás egy olyan fiókkal, amely tagja hello toohello server **ADSyncAdmins** biztonsági csoport.
2. Start **szinkronizálási szolgáltatás** a hello **Start** menü.
3. Válassza ki **összekötők**, és a hello **összekötők** listára, válassza ki a hello összekötő hello típusú **Active Directory tartományi szolgáltatások**. A **műveletek**, jelölje be **tulajdonságok**.  
   ![Összekötő tulajdonságai](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kattintson a **könyvtárpartíciók konfigurálása**.
5. A hello **válassza ki az a címtárpartíciókat** listában válassza ki, és törölje a tartományok, igény szerint. Győződjön meg arról, hogy vannak-e jelölve, hogy szeretné-e toosynchronize csak hello partíciókat.  
   ![Partíciók](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
   Ha megváltoztatta a helyszíni Active Directory-infrastruktúrát és a hozzáadott vagy tartományok távolítva hello erdő, majd kattintson a hello **frissítése** gomb tooget frissített listáját. Frissítésekor, megkérdezi a hitelesítő adatokat. Adjon olvasási hozzáférési tooWindows Server Active Directory rendelkező bármely hitelesítő adatokat. Az előre feltöltve hello párbeszédpanelen toobe hello felhasználó nem rendelkezik.  
   ![Frissítés szükséges](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Amikor elkészült, zárja be a hello **tulajdonságok** kattintva párbeszédpanel **OK**. Ha eltávolította a tartományok hello erdőből, előugró üzenet jelzi, hogy egy tartomány el lett távolítva, és ez a konfiguráció törlődnek.
7. Továbbra is tooadjust hello [futtatási profil](#update-run-profiles).

### <a name="update-hello-run-profiles"></a>Futtatás hello-profil frissítése
Ha a tartomány szűrő módosította, tooupdate futtatása hello profilok is kell.

1. A hello **összekötők** listában, győződjön meg arról, hogy hello hello előző lépésben megváltoztató csatlakozó van kiválasztva. A **műveletek**, jelölje be **Configure Run Profiles**.  
   ![Futtatási profil 1 összekötő](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  
2. Keresse meg, és azonosíthatja a profilok a következő hello:
    * Teljes importálás
    * Teljes szinkronizálás
    * Különbözeti importálás
    * A különbözeti szinkronizálás
    * Exportálás
3. Az egyes profilok beállítása hello **hozzáadott** és **eltávolított** tartományok.
    1. Az egyes hello öt profilok hello lépéseket követve minden **hozzáadott** tartomány:
        1. Futtatás hello-profil kiválasztása, és kattintson a **új lépés**.
        2. A hello **konfigurálása lépés** hello-oldalon, **típus** legördülő menüben válassza hello lépés típusa hello azonos nevét, ahogy hello-profilhoz, amikor a konfigurálja. Ezután kattintson a **Next** (Tovább) gombra.  
        ![Futtatási profil 2 összekötő](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
        3. A hello **összekötő-konfiguráció** lap hello **partíció** legördülő menü, hogy felvett tooyour tartomány szűrő hello tartomány válassza hello nevét.  
        ![Futtatási profil 3 összekötő](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
        4. tooclose hello **kísérleti profilba konfigurálása** párbeszédpanel, kattintson a **Befejezés**.
    2. Az egyes hello öt profilok hello lépéseket követve minden **eltávolított** tartomány:
        1. Válassza ki a futtatni hello-profil.
        2. Ha hello **érték** a hello **partíció** attribútum GUID, jelölje be hello futtatása lépést, és kattintson **lépés törlése**.  
        ![Futtatási profil 4 összekötő](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  
    3. Ellenőrizze a módosítást. Minden olyan tartományhoz, amelyet az toosynchronize szereplő minden futtatási profilnál lépésben szerepelnie kell.
4. tooclose hello **Configure Run Profiles** párbeszédpanel, kattintson a **OK**.
5.  toocomplete hello konfigurációban kell toorun egy **teljes importálás** és egy **különbözeti szinkronizálási**. Továbbra is hello szakasz olvasása [alkalmaz, és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Szervezeti egység-alapú szűrés
hello előnyben részesített módon toochange OU-alapú szűrés hello telepítési varázslót és módosítása [tartományok és szervezeti egységek szűrése](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). hello telepítővarázsló üdvözlő feladataival ebben a témakörben ismertetett automatizálja.

Ha valamilyen okból kifolyólag nem toorun hello telepítővarázsló most csak kövesse ezeket a lépéseket.

tooconfigure szervezeti egység-alapú szűrés hello a következő lépéseket:

1. Jelentkezzen be, amelyen fut az Azure AD Connect szinkronizálási szolgáltatás egy olyan fiókkal, amely tagja hello toohello server **ADSyncAdmins** biztonsági csoport.
2. Start **szinkronizálási szolgáltatás** a hello **Start** menü.
3. Válassza ki **összekötők**, és a hello **összekötők** listára, válassza ki a hello összekötő hello típusú **Active Directory tartományi szolgáltatások**. A **műveletek**, jelölje be **tulajdonságok**.  
   ![Összekötő tulajdonságai](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kattintson a **címtárpartíciók konfigurálása**, jelölje be hello tartomány tooconfigure szeretne, és kattintson **tárolók**.
5. Amikor a rendszer kéri, adja meg a hitelesítő adatok az olvasási hozzáférés tooyour a helyszíni Active Directory. Az előre feltöltve hello párbeszédpanelen toobe hello felhasználó nem rendelkezik.
6. A hello **tárolók kijelölése** egyértelmű hello szervezeti egységek, hogy nem szeretné, és a felhő címtárának hello toosynchronize, és kattintson a párbeszédpanelen **OK**.  
   ![Szervezeti egységek hello tárolók kijelölése párbeszédpanel](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
   * Hello **számítógépek** tároló számára a Windows 10 számítógépek toobe sikeresen kell szinkronizálnia a tooAzure AD ki. Ha a tartományhoz csatlakoztatott számítógépek egyéb szervezeti egységek találhatók, ellenőrizze, azok van kiválasztva.
   * Hello **ForeignSecurityPrincipals** Ha több erdő megbízhatósági kapcsolatok a tárolót kell választani. Ez a tároló lehetővé teszi, hogy az erdők közötti biztonsági csoport tagsága toobe feloldva.
   * Hello **RegisteredDevices** OU választják, ha engedélyezte a hello eszköz visszaírási szolgáltatás. Ha egy másik visszaírási szolgáltatás, például a csoportok visszaírásához győződjön meg arról, ezeket a helyeket van kiválasztva.
   * Válassza ki a bármely egyéb szervezeti egység, ahol a felhasználók, a iNetOrgPersons, a csoportokat, a névjegyeket és a számítógépek találhatók. Hello képen látható minden hozzá hello ManagedObjects szervezeti egység található.
   * Csoport-alapú szűrés használatakor majd hello ahol hello csoport OU kell foglalni.
   * Vegye figyelembe, hogy beállítható, hogy új szervezeti egységek szűrése konfigurációs hello befejeződése után hozzáadott szinkronizálva, vagy nincsenek szinkronizálva. Lásd az hello következő szakaszát.
7. Amikor elkészült, zárja be a hello **tulajdonságok** kattintva párbeszédpanel **OK**.
8. toocomplete hello konfigurációban kell toorun egy **teljes importálás** és egy **különbözeti szinkronizálási**. Továbbra is hello szakasz olvasása [alkalmaz, és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Új szervezeti egységek szinkronizálása
Alapértelmezés szerint új szervezeti egységek szűrése konfigurálása után létrehozott szinkronizálódnak. Ez az állapot jelzi jelölőnégyzet be van jelölve. Néhány sub szervezeti is törölheti. tooget ezt a viselkedést hello jelölőnégyzetre kattintva fehér, a kék jel (az alapértelmezett állapotába) válik. Bármely sub-szervezeti egységek, hogy nem szeretné, hogy toosynchronize majd eltávolításához.

Minden sub-szervezeti egység szinkronizálva van, majd hello be fehér, a kék jel.  
![Minden kiválasztott be a szervezeti egység](./media/active-directory-aadconnectsync-configure-filtering/ousyncnewall.png)

Néhány sub szervezeti jelöletlen törölték, majd hello be Szürke fehér jelölést.  
![Néhány sub-szervezeti egységek nincs bekapcsolva a szervezeti egység](./media/active-directory-aadconnectsync-configure-filtering/ousyncnew.png)

Ezzel a konfigurációval ManagedObjects alapján létrehozott új szervezeti egység szinkronizálva.

hello Azure AD Connect telepítővarázsló mindig létrehozza ezt a konfigurációt.

### <a name="dont-synchronize-new-ous"></a>Új szervezeti egységek nincs szinkronizálás
Konfigurálhatja a hello szinkronizálási motor toonot szinkronizálása új szervezeti egységek szűrése konfigurációs hello befejeződése után. Ebben az állapotban jelzett hello UI hello mező nincs bejelölve a szürkén jelenik meg. tooget ezt a viselkedést hello jelölőnégyzetre kattintva válik fehér, a jelölőnégyzet nincs bejelölve. Válassza ki a hello sub-ou-k esetében, amelyet az toosynchronize.

![Hello legfelső szintű nincs bekapcsolva a szervezeti egység](./media/active-directory-aadconnectsync-configure-filtering/oudonotsyncnew.png)

Ezzel a konfigurációval ManagedObjects alapján létrehozott új szervezeti egység nincs szinkronizálva.

## <a name="attribute-based-filtering"></a>Attribútum-alapú szűrés
Győződjön meg arról, hogy használata hello 2015. November ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) vagy későbbi buildre ezek lépések toowork.

Attribútum-alapú szűrés hello legrugalmasabb módon toofilter objektumok. Használhatja a hello hatványa [deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md) toocontrol szinte minden szempontját, ha egy objektum van szinkronizálva tooAzure AD.

Alkalmazhat [bejövő](#inbound-filtering) az Active Directory toohello metaverse, szűrési és [kimenő](#outbound-filtering) hello metaverse tooAzure AD a szűrés. Azt javasoljuk, hogy bejövő szűrés, mert hello legegyszerűbb toomaintain alkalmazni. Csak akkor ajánlott a kimenő szűrést, ha egynél több erdőből származó toojoin objektumok ez szükséges akkor, mielőtt hello értékelésére akkor kerül sor.

### <a name="inbound-filtering"></a>Bejövő szűrése
Bejövő szűrés hello alapértelmezett konfigurációt használ, ahol tooAzure AD fog objektumok hello metaverzum-attribútum cloudFiltered nincs beállítva a tooa érték toobe szinkronizálva kell rendelkeznie. Ha ez az attribútum értéke túl**igaz**, majd hello objektum nincs szinkronizálva. Nem szabad lennie állítva, akkor túl**hamis**, úgy lett kialakítva. a toomake, más szabályok hello képességét toocontribute egy értéke, ez az attribútum csak kellene toohave hello értékek **igaz** vagy **NULL** (távol).

A bejövő szűrés hello hatványa használja **hatókör** toodetermine toosynchronize objektumot, vagy nem szinkronizálja. Ez a panelen módosításának toofit saját szervezete követelményeinek. hello hatókör modul rendelkezik egy **csoport** és egy **záradék** toodetermine hatókörében szinkronizálási szabály esetén. A csoport egy vagy több záradékot tartalmaz. Nincs logikai "és" több záradékot, és több csoportok közötti logikai "vagy" között.

Ossza meg velünk Példaként tekintse meg:  
![Hatókör](./media/active-directory-aadconnectsync-configure-filtering/scope.png)  
Ez értelmezendő **(részleg = informatikai) vagy (részleg = értékesítés és c = US)**.

A következő hello minták és lépések, hello felhasználói objektum, például használatát, de ezzel az összes objektumtípus.

A következő minták hello hello sorrend érték 50 kezdődik. Ez a szám nem lehet, de 100-nál kisebbnek kell lennie.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatív szűrése: "szinkronizálja ezeket"
A következő példa hello, a kiszűrhetők (nem szinkronizálása) minden felhasználó ahol **extensionAttribute15** hello értéke **NoSync**.

1. Jelentkezzen be, amelyen fut az Azure AD Connect szinkronizálási szolgáltatás egy olyan fiókkal, amely tagja hello toohello server **ADSyncAdmins** biztonsági csoport.
2. Start **szinkronizálási szabályok szerkesztő** a hello **Start** menü.
3. Győződjön meg arról, hogy **bejövő** van kiválasztva, és kattintson a **új szabály hozzáadása**.
4. Adjon hello szabály egy leíró nevet, például "*a az AD-felhasználó DoNotSyncFilter*". Jelölje be hello megfelelő erdő, jelölje be **felhasználói** hello, **CS objektumtípus**, és válassza ki **személy** hello, **MV-objektum típusa**. A **kapcsolattípus**, jelölje be **csatlakozás**. A **sorrend**, írjon be egy értéket, amely jelenleg nem használja egy másik szinkronizálási szabály (például 50), és kattintson a **következő**.  
   ![Bejövő 1 leírása](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. A **Scoping szűrő**, kattintson a **csoport hozzáadása**, és kattintson a **záradék hozzáadása**. A **attribútum**, jelölje be **ExtensionAttribute15**. Győződjön meg arról, hogy **operátor** értéke túl**egyenlő**, és hello értéket **NoSync** a hello **érték** mezőbe. Kattintson a **Tovább** gombra.  
   ![Bejövő 2 hatókör](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Hagyja hello **csatlakozás** szabályok üres, és kattintson a **következő**.
7. Kattintson a **hozzáadása átalakítása**, jelölje be hello **FlowType** , **állandó**, és válassza ki **cloudFiltered** hello,  **Cél attribútum**. A hello **forrás** szövegmezőben **igaz**. Kattintson a **Hozzáadás** toosave hello szabály.  
   ![Bejövő 3 átalakítása](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. toocomplete hello konfigurációban kell toorun egy **Full sync**. Továbbra is hello szakasz olvasása [alkalmaz, és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Pozitív szűrése: "csak szinkronizálás ezek"
Pozitív szűrési kifejezése lehet további kihívást, mert nincsenek szinkronizálva, például konferenciaterem nyilvánvaló toobe tooconsider objektumokat is rendelkezik. Biztosan is folyamatos toooverride hello alapértelmezett szűrő hello out-of-box szabályban **a az AD - felhasználó csatlakozás**. Az egyéni szűrő létrehozásakor ellenőrizze, hogy toonot tartalmaznak kritikus rendszerobjektumok, replikációs ütközés objektumok, különleges postaládák és szolgáltatásfiókok hello az Azure AD Connect.

hello pozitív szűrési beállításnál két szinkronizálási szabály. Az objektumok toosynchronize hello megfelelő hatókörű egy szabály (vagy több) szükséges. Második általános szinkronizálási szabály, amely a összes objektum, amely olyan objektum, amely szinkronizálnia kell még nem még meg is kell.

A következő példa hello, csak szinkronizálás ahol hello részleg attribútum értéke hello felhasználói objektumok **értékesítési**.

1. Jelentkezzen be, amelyen fut az Azure AD Connect szinkronizálási szolgáltatás egy olyan fiókkal, amely tagja hello toohello server **ADSyncAdmins** biztonsági csoport.
2. Start **szinkronizálási szabályok szerkesztő** a hello **Start** menü.
3. Győződjön meg arról, hogy **bejövő** van kiválasztva, és kattintson a **új szabály hozzáadása**.
4. Adjon hello szabály egy leíró nevet, például "*a az AD-felhasználó értékesítési szinkronizálása*". Jelölje be hello megfelelő erdő, jelölje be **felhasználói** hello, **CS objektumtípus**, és válassza ki **személy** hello, **MV-objektum típusa**. A **kapcsolattípus**, jelölje be **csatlakozás**. A **sorrend**, írjon be egy értéket, amelyet egy másik szinkronizálási szabály (például 51) jelenleg nem használ, és kattintson a **következő**.  
   ![Bejövő 4 leírása](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. A **Scoping szűrő**, kattintson a **csoport hozzáadása**, és kattintson a **záradék hozzáadása**. A **attribútum**, jelölje be **részleg**. Győződjön meg arról, hogy túl van-e állítva a operátor**egyenlő**, és írja be a hello érték **értékesítési** hello a **érték** mezőbe. Kattintson a **Tovább** gombra.  
   ![Bejövő 5 hatókör](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Hagyja hello **csatlakozás** szabályok üres, és kattintson a **következő**.
7. Kattintson a **hozzáadása átalakítása**, jelölje be **állandó** hello, **FlowType**, és jelölje be hello **cloudFiltered** hello,  **Cél attribútum**. A hello **forrás** mezőbe írja be **hamis**. Kattintson a **Hozzáadás** toosave hello szabály.  
   ![Bejövő 6 átalakítása](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
   Ez egy különleges esetben, ha explicit módon beállított cloudFiltered túl az**hamis**.
8. Most már tudunk toocreate hello általános szinkronizálási szabály. Adjon hello szabály egy leíró nevet, például "*a az AD-felhasználó Catch – minden szűrő*". Jelölje be hello megfelelő erdő, jelölje be **felhasználói** hello, **CS objektumtípus**, és válassza ki **személy** hello, **MV-objektum típusa**. A **kapcsolattípus**, jelölje be **csatlakozás**. A **sorrend**, írjon be egy értéket, amelyet egy másik szinkronizálási szabály (például 99) jelenleg nem használ. Kijelölt magasabb (képest alacsonyabb fontossági sorrenddel) hello előző szinkronizálási szabály sorrend értéke. De hogy is hagyta valamennyi hely, hogy további szűrési szinkronizálási szabályok később veheti fel, ha azt szeretné, hogy további szervezeti egységek szinkronizálása toostart. Kattintson a **Tovább** gombra.  
   ![Bejövő 7 leírása](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Hagyja **Scoping szűrő** üres, és kattintson a **következő**. Egy üres szűrőnév jelzi, hogy hello szabály alkalmazása toobe tooall objektumok.
10. Hagyja hello **csatlakozás** szabályok üres, és kattintson a **következő**.
11. Kattintson a **hozzáadása átalakítása**, jelölje be **állandó** hello, **FlowType**, és válassza ki **cloudFiltered** hello,  **Cél attribútum**. A hello **forrás** mezőbe írja be **igaz**. Kattintson a **Hozzáadás** toosave hello szabály.  
    ![Bejövő 3 átalakítása](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. toocomplete hello konfigurációban kell toorun egy **Full sync**. Továbbra is hello szakasz olvasása [alkalmaz, és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

Ha szeretné, további objektumokat vegye hello szinkronizálásban hello első típusú további szabályok is létrehozhat.

### <a name="outbound-filtering"></a>Kimenő szűrése
Egyes esetekben szükséges toodo hello szűrés csak hello objektumok hello metaverse a tartományhoz csatlakoztatás után. Például hello levél attribútum hello erőforráserdőből és hello userPrincipalName attribútum hello fiók erdőből, ha egy objektum szinkronizálnia kell toodetermine szükséges toolook lehet. Ezekben az esetekben hello kimenő forgalomra vonatkozó szabály szűrés hello hoz létre.

Ebben a példában módosítja hello szűrés, hogy csak a felhasználók, amelyek rendelkeznek a levelezés és a userPrincipalName végződése @contoso.com szinkronizálja a rendszer:

1. Jelentkezzen be, amelyen fut az Azure AD Connect szinkronizálási szolgáltatás egy olyan fiókkal, amely tagja hello toohello server **ADSyncAdmins** biztonsági csoport.
2. Start **szinkronizálási szabályok szerkesztő** a hello **Start** menü.
3. A **szabályok típusa**, kattintson a **kimenő**.
4. Keresés hello szabály nevű **tooAAD – felhasználói csatlakozás kimenő**, és kattintson a **szerkesztése**.
5. Hello előugró ablakban fogadja a hívást **Igen** toocreate hello szabály egy példányát.
6. A hello **leírás** lapján módosítsa **sorrend** tooan nem használt érték, például az 50.
7. Kattintson a **Scoping szűrő** a bal oldali navigációs hello, és kattintson a **Hozzáadás záradék**. A **attribútum**, jelölje be **mail**. A **operátor**, jelölje be **megadott módon VÉGZŐDŐ**. A **érték**, típus  **@contoso.com** , és kattintson a **Hozzáadás záradék**. A **attribútum**, jelölje be **userPrincipalName**. A **operátor**, jelölje be **megadott módon VÉGZŐDŐ**. A **érték**, típus  **@contoso.com** .
8. Kattintson a **Save** (Mentés) gombra.
9. toocomplete hello konfigurációban kell toorun egy **Full sync**. Továbbra is hello szakasz olvasása [alkalmaz, és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Alkalmazza, és a módosítások ellenőrzéséhez
Miután a konfigurációs módosításokat hajtott végre, telepítenie kell őket toohello objektumok, amelyek már hello rendszerben. Is lehet, hogy fel kell dolgozni a hello objektumok, amelyek jelenleg nem a szinkronizálási motor hello (és hello szinkronizálási motor tooread hello forrásrendszerben újra tooverify benne lévő tartalom).

Ha módosította hello konfigurációját a **tartomány** vagy **szervezeti egység** szűréshez, akkor szükséges, hogy toodo egy **teljes importálás**, utána pedig **különbözeti szinkronizálás**.

Ha módosította hello konfigurációját a **attribútum** szűréshez, akkor szükséges, hogy toodo egy **teljes szinkronizálás**.

Hello a következő lépéseket:

1. Start **szinkronizálási szolgáltatás** a hello **Start** menü.
2. Válassza ki **összekötők**. A hello **összekötők** listára, válassza ki az összekötőt, ha olyan konfigurációs módosítást korábban végzett hello. A **műveletek**, jelölje be **futtatása**.  
   ![Összekötő futtatása](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. A **futtatási profil**, válassza ki, amely hello előző szakaszban említett hello műveletet. Ha toorun két műveletet kell, második után hello futtatási hello első befejeződött. (hello **állapot** oszlop **üresjáratban** kijelölt hello összekötő.)

Hello szinkronizálás után az összes változtatások előkészített toobe exportált. Ténylegesen hello módosítása előtt az Azure ad-ben, azt szeretné, hogy helyesen-e ezek a változások tooverify.

1. Nyisson meg egy parancsablakot, és nyissa meg túl`%Program Files%\Microsoft Azure AD Sync\bin`.
2. Futtassa az `csexport "Name of Connector" %temp%\export.xml /f:x` parancsot.  
   hello összekötő hello neve nem található szinkronizálási szolgáltatás. Rendelkezik egy neve hasonló too"contoso.com – AAD" az Azure AD.
3. Futtassa az `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` parancsot.
4. Most már rendelkezik a % temp %, amely megfelel a Microsoft Excel export.csv nevű fájl. Ezt a fájlt az exportált toobe készül minden hello módosításokat tartalmaz.
5. Hello szükséges változtatások toohello adatok vagy konfiguráció, és futtassa lépések újra (importálás, szinkronizálás, és győződjön meg arról) csak a változtatásokat, hogy kapcsolatos exportált toobe hello várt.

Ha elkészült, exportálja a hello módosítások tooAzure AD.

1. Válassza ki **összekötők**. A hello **összekötők** listára, válassza ki a hello Azure AD-összekötőt. A **műveletek**, jelölje be **futtatása**.
2. A **futtatási profil**, jelölje be **exportálása**.
3. Ha a konfigurációs módosítások sok objektumok törlése, majd hibaüzenet jelenik meg hello exportálás Ha hello száma több, mint a konfigurált hello küszöbértéket (alapértelmezésben 500). Ha ezt a hibaüzenetet látja, akkor meg kell tootemporarily letiltása hello "[véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)" funkció.

Most már idő tooenable hello Feladatütemező újra.

1. Start **Feladatütemező** a hello **Start** menü.
2. Közvetlenül a **Feladatütemező könyvtár**, keresés hello feladat nevű **Azure AD Sync Scheduler**, kattintson a jobb gombbal, majd válassza **engedélyezése**.

## <a name="group-based-filtering"></a>Csoport-alapú szűrés
Szűrési hello Csoportalapú először az Azure AD Connect telepítését használatával is beállíthat [egyéni telepítési](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups). Az a célja, hogy az a próbatelepítések, ha azt szeretné, hogy a szinkronizált objektumokat toobe csak egy kis készletét. Csoport-alapú szűrés letiltása esetén nem engedélyezhető újra. Rendelkezik *nem támogatott* toouse csoport alapú szűrést a egyéni konfiguráció. Azt csak támogatott tartalmaz tooconfigure Ez a szolgáltatás hello telepítés varázsló használatával. A próbaüzem végrehajtását, majd valamelyikével hello más szűrési beállítások ebben a témakörben. A csoport-alapú szűrés együtt OU-alapú szűrés használatakor hello kapcsolnia, amelyben hello csoportot és annak tagjait is szerepelnie kell.

Több AD-erdővel szinkronizálásakor beállíthatja egy másik csoportot minden egyes címtárösszekötőben megadásával csoport alapú szűrés. Ha egy felhasználó egy Active Directory-erdőben kívánja toosynchronize és hello ugyanaz a felhasználó egy vagy több megfelelő FSP (idegen rendszerbiztonsági tag) objektumokat más AD-erdőkkel, győződjön meg arról, hogy hello felhasználói objektum, a minden a megfelelő FSP objektum Csoportalapú belül van-e szűrési hatókör. Ha egy vagy több hello FSP objektumok szűréssel Csoportalapú kizárva, hello felhasználói objektum nem lesz szinkronizált tooAzure AD.

## <a name="next-steps"></a>Következő lépések
- További információ [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.
- További információ [a helyszíni identitások integrálása az Azure AD](active-directory-aadconnect.md).
