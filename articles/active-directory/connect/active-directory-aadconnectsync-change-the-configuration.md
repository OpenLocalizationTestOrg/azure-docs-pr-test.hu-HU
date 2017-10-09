---
title: "Azure AD Connect szinkronizálása: olyan konfigurációs módosítást az Azure AD Connect szinkronizálási szolgáltatás |} Microsoft Docs"
description: "Bemutatja, hogyan hogyan toomake egy toohello konfigurációjának módosítása a az Azure AD Connect szinkronizálása."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 78e96d9166831a668439c2b8aa6a0022bc472da4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomake-a-change-toohello-default-configuration"></a>Azure AD Connect szinkronizálása: hogyan toomake egy módosítás toohello alapértelmezett konfiguráció
hello Ez a témakör célja toowalk le a hogyan toomake toohello alapértelmezett konfigurációját az Azure AD Connect szinkronizálási szolgáltatás módosítja. Lépéseket biztosít olyan gyakori forgatókönyveket tartalmaz. Ennek az információnak a néhány egyszerű módosításokkal tooyour saját konfigurációs a saját üzleti szabályok alapján képes toomake kell lennie.

## <a name="synchronization-rules-editor"></a>Szinkronizálási szabályok szerkesztő
Szinkronizálási szabályok szerkesztő hello a használt toosee és módosítási hello alapértelmezett konfiguráció. A Start menü hello hello alatt található **az Azure AD Connect** csoport.  
![Start menü szinkronizálási szabály szerkesztő](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Amikor megnyitja azt, lásd: hello alapértelmezett out-of-box szabályokat.

![Szinkronizálási szabály szerkesztő](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-hello-editor"></a>Hello-szerkesztőben lépjen
hello legördülő listákat hello szerkesztő hello tetején lehetővé teszik a tooquickly keresés egy adott szabályt. Például ha azt szeretné, ahol hello attribútum proxyAddresses megtalálható toosee hello szabályok, módosítania hello legördülő listákat toohello következő:  
![SRE szűrése](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
tooreset szűrést és a betöltés egy friss konfigurációban, nyomja le az **F5** hello billentyűzet.

toohello jobb felső sarokban van egy gomb **új szabály hozzáadása**. Ez a gomb van használt toocreate saját egyéni szabályt.

Hello lap alján gombok működött a kijelölt szinkronizálási szabály van. **Szerkesztés** és **törlése** várt módon működnek őket. **Exportálás** hoz létre egy PowerShell-parancsfájl hello szinkronizálási szabály újbóli létrehozására. Ez az eljárás lehetővé teszi egy kiszolgáló tooanother szinkronizálási szabály toomove.

## <a name="create-your-first-custom-rule"></a>Az első egyéni szabály létrehozása
hello leggyakoribb módosítása módosítások toohello attribútumfolyamok. hello adatok a forráskönyvtárban nem lehet beállítani, mint az Azure AD. Ebben a szakaszban hello példában toomake meg arról, hogy a felhasználó utóneve hello mindig a kívánt **megfelelő eset**.

### <a name="disable-hello-scheduler"></a>Tiltsa le a hello Feladatütemező
Hello [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) alapértelmezés szerint 30 percenként fut.. Azt szeretné, hogy toomake meg arról, hogy nem indul, amíg változtatásokat végez, és az új szabályok hibaelhárítása. tootemporarily hello Feladatütemező tiltsa le, indítsa el a Powershellt, és futtassa`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Tiltsa le a hello Feladatütemező](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-hello-rule"></a>Hello szabály létrehozása
1. Kattintson a **új szabály hozzáadása**.
2. A hello **leírás** lapon írja be a következő hello:  
   ![Bejövő szabály szűrése](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Name: Ad hello szabály egy leíró nevet.
   * Leírás: Bizonyos tisztázására, milyen hello szabály valaki más is tisztában van.
   * Rendszer csatlakoztatott: hello rendszer hello objektum is található. Ebben az esetben válassza ki a hello Active Directory-összekötőt.
   * Csatlakoztatott rendszer/Metaverzum-objektum típusa: Válassza ki **felhasználói** és **személy** kulcsattribútumokkal.
   * Kapcsolat típusa: Módosítsa ezt az értéket túl**csatlakozás**.
   * Sorrend: Adjon meg egy értéket, amely egyedi a hello rendszer. Alacsonyabb numerikus érték magasabb prioritással.
   * Címke: Hagyja üresen. A Microsoft csak out-of-box szabályok ebben a mezőben számnak töltődik kell rendelkeznie.
3. A hello **Scoping szűrő** lapján adja meg **givenName ISNOTNULL**.  
   ![Bejövő szabály hatókör-beállítási szűrője](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Ez a szakasz az használt toodefine, mely objektumok hello szabály vonatkozik. Ha üres, hello szabályt alkalmazni tooall felhasználói objektumok. Azonban, amely konferenciaterem szolgáltatásfiókok és más nem személyek felhasználói objektumokat tartalmazhat.
4. A hello **szabályok csatlakozás**, hagyja üresen.
5. A hello **átalakítások** lapon, majd hello FlowType túl**kifejezés**. Jelölje be hello Target attribútummal **givenName**, és írja be a forrás `PCase([givenName])`.
   ![Bejövő szabály átalakítások](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   hello szinkronizálási motor a kis-és nagybetűket is hello függvény és hello hello attribútum nevét. Ha valami nem megfelelő, egy figyelmeztetés megjelenítése hello szabály hozzáadásakor. hello szerkesztő toosave lehetővé teszi, és továbbra is, így tooreopen hello szabályok és a megfelelő hello szabályok kellene lennie.
6. Kattintson a **Hozzáadás** toosave hello szabály.

Az új egyéni szabályt kell lennie az egyéb hello látható szabályok szinkronizálása hello rendszerben.

### <a name="verify-hello-change"></a>Ellenőrizze a hello módosítása
Az új módosítását azt szeretné, hogy a várt módon működik, és nem szűrész hibákról toomake. Attól függően, hogy objektumok hello száma nincsenek két különböző módon toodo ezt a lépést.

1. A teljes szinkronizálás futtatása az összes objektum
2. Egyetlen objektum futtatni egy kép és a teljes szinkronizálás

Start **szinkronizálási szolgáltatás** hello start menüből. Ebben a szakaszban található lépéseket hello összes szerepelnek, az eszköz.

1. **Teljes szinkronizálás az összes objektum**  
   Válassza ki **összekötők** hello tetején. Hello végzett módosítás tooin hello előző szakaszban összekötő azonosítása, ebben az esetben hello Active Directory tartományi szolgáltatásokban, és válassza ki azt. Válassza ki **futtatása** műveleteket, és válassza a **teljes szinkronizálás** és **OK**.
   ![Teljes szinkronizálás](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   hello objektumok hello metaverse most frissülnek. Most szeretne toolook: hello metaverse hello objektumban.
2. **Kép és a teljes szinkronizálás az egy adott objektum**  
   Válassza ki **összekötők** hello tetején. Hello végzett módosítás tooin hello előző szakaszban összekötő azonosítása, ebben az esetben hello Active Directory tartományi szolgáltatásokban, és válassza ki azt. Válassza ki **Összekötőtér keresési**. Hatókör toofind egy objektumot a toouse tootest hello módosítás használja. Válasszon hello objektumot, majd kattintson a **előzetes**. Új üdvözlő képernyőt, válassza ki **véglegesítése előzetes**.  
   ![Előzetes verzió véglegesítése](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   már véglegesített toohello metaverse hello módosítása.

**Nézze meg hello metaverse hello objektum**  
Most szeretne toopick néhány minta objektumok toomake meg arról, hogy hello értéknek kell szerepelnie, és adott hello szabályt. Válassza ki **Metaverzum-keresés** hello elejéről. Adja hozzá a bármely szűrő toofind hello vonatkozó objektumok van szüksége. Hello keresési eredmény nyissa meg az objektum. Nézze meg hello attribútumértékek és hello is ellenőrizheti **szinkronizálási szabályok** oszlopot, amely a várt módon szabályt hello.  
![Keresés a metaverzumban](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-hello-scheduler"></a>Hello Feladatütemező engedélyezése
Ha a várt módon van, engedélyezheti hello Feladatütemező újra. A PowerShell, futtassa a `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Más közös Attribútummódosítások folyamata
hello előző szakaszban leírt, hogyan toomake változik tooan Attribútumfolyam. Ez a szakasz néhány további példákat rendelkezésre állnak. Hogyan toocreate hello szinkronizálási szabály rövidítése hello lépései, de az előző szakaszban hello hello teljes lépéseket találja.

### <a name="use-another-attribute-than-hello-default"></a>Hello alapértelmezett egy másik attribútum használja.
A Fabrikam, hol használják a megadott névvel, a Vezetéknév és a megjelenített név hello helyi ábécé erdő van. Ezek az attribútumok latin betűkből álló ábrázolását hello hello bővítményattribútumokat találhatók. Az Azure Active Directory globális címlista hello fejlesztéskor és az Office 365 hello szervezet célja az alábbi attribútumok toobe használja helyette.

Az alapértelmezett beállításokkal helyi erdő hello objektum néz ki:  
![Attribútumfolyam 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

egy szabály egyéb attribútumfolyamok toocreate hello a következő:

* Start **szinkronizálási szabály szerkesztő** hello start menüből.
* A **bejövő** továbbra is kiválasztott toohello balra, gombra hello **új szabály hozzáadása**.
* Adjon hello szabály nevét és leírását. Hello a helyszíni Active Directory és a hello megfelelő típusú objektumokat válasszon. A **kapcsolattípus**, jelölje be **csatlakozás**. A sorrend válasszon ki egy szám, amely egy másik szabály nem használja. hello out-of-box szabályok indítsa el a 100, így hello érték 50 használhatók ebben a példában.
  ![Attribútumfolyam 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Hatókör hagyja üresen (Ez azt jelenti, hogy vonatkozzon tooall felhasználói objektumok hello erdő).
* Hagyja illesztési üres (lehetővé tevő szabályokat van, out-of-box szabály leíró hello illesztések).
* Átalakítások hozzon létre a következő adatfolyamok hello:  
  ![Attribútumfolyam 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Kattintson a **Hozzáadás** toosave hello szabály.
* Nyissa meg túl**Synchronization Service Managert**. A **összekötők**, válassza ki a hello szabály hozzáadott ahol összekötő hello. Válassza ki **futtatása**, és **teljes szinkronizálás**. A teljes szinkronizálás újraszámítja az összes objektum hello aktuális szabályokkal.

Ez az azonos objektum a következő egyéni szabálynak hello hello eredménye:  
![Attribútumfolyam 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Attribútumok hossza
Karakterlánc attribútumok szerint alapértelmezett készletet toobe példánycímkének és hello hossza legfeljebb 448 karakter lehet. Ha karakterláncot tartalmazó attribútumok előfordulhat, hogy több dolgozik, majd győződjön meg arról, hogy tooinclude hello következő hello Attribútumfolyam:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-hello-userprincipalsuffix"></a>Hello userPrincipalSuffix módosítása
hello userPrincipalName attribútum az Active Directory hello felhasználók mindig nem ismert, és nem lehet megfelelő, mert hello bejelentkezési azonosítót. hello Azure AD Connect sync telepítési varázsló lehetővé teszi, hogy egy másik attribútum kiadási például az e-mail. De az egyes esetekben hello attribútum ki kell számítani. Például hello Contoso vállalatnak két Azure AD-címtártól, az egyik az éles és egy tesztelési. Szeretnék hello felhasználók a saját tesztelési bérlői toouse hello bejelentkezési azonosító egy másik utótag  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Ebben a kifejezésben, hajtsa végre a megfelelő mindent bal részén hello először @-sign (Word) és ÖSSZEFŰZ rögzített karakterláncot.

### <a name="convert-a-multi-value-tooa-single-value"></a>Alakítsa át a többértékű tooa egyértékű
Egyes attribútumok az Active Directory többértékű hello sémában, annak ellenére, hogy a keresőmotorok is egyetlen Active Directory – felhasználók és számítógépek értékelni. Példa: hello leírás attribútuma.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

Ez a kifejezés arra az esetre hello attribútum értéke, végezze el a hello első elem (elem) szereplő hello attribútum, távolítsa el kezdő és záró szóközök (vágás), és majd megtartása hello hello karakterláncban először 448 karakterek (bal oldali).

### <a name="do-not-flow-an-attribute"></a>Nem egymást követő attribútum
Az ebben a szakaszban hello forgatókönyvön háttér, lásd: [hello attribútum folyamata folyamatot vezérlő](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Két módon toonot flow attribútum. hello először hello telepítővarázsló érhető el, és lehetővé teszi a túl[távolítsa el a kiválasztott attribútumok](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Ez a beállítás akkor működik, ha még nem szinkronizálta hello attribútum előtt. Azonban ha kezdték toosynchronize ezt az attribútumot, és később eltávolítja az ezzel a szolgáltatással, majd hello szinkronizálási motor leállítja hello attribútum kezelése és hello meglévő értékükön Azure AD-ben.

Ha szeretné, hogy tooremove hello attribútum értékének, és győződjön meg arról, hogy a jövőbeni hello nem terjeszthetők, ehelyett kell létrehoz egy egyéni szabályt.

A Fabrikam rájöttünk kell arról, hogy néhány hello attribútumok toohello szinkronizálja azt a felhő nem lehet van. Azt szeretnénk, hogy ezek az attribútumok el lesznek távolítva az Azure AD toomake.  
![Hibás a Bővítményattribútumokat](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Egy új bejövő szinkronizálási szabályának létrehozása és feltöltése hello leírás ![leírása](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Hozzon létre attribútumfolyamok típusú **kifejezés** és hello-forrással rendelkező **AuthoritativeNull**. literális hello **AuthoritativeNull** arra utalnak, hogy hello érték kell a hello MV üres akkor is, ha egy alacsonyabb sorrend szinkronizálási szabály megpróbál toopopulate hello érték.
  ![A kiterjesztési attribútumot átalakítása](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Hello szinkronizálási szabály mentéséhez. Start **szinkronizálási szolgáltatás**, megkeresi hello összekötő, válassza ki **futtatása**, és **teljes szinkronizálás**. Ezt a lépést minden attribútumfolyamok újraszámítja.
* Győződjön meg arról, hogy hello szánt módosítások hello összekötőtér keresése által exportált toobe készül.
  ![Előkészített törlése](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Szabályok létrehozása a PowerShell használatával
Jól hello szinkronizálási szabály szerkesztőjével működik, ha csak néhány módosítások toomake. Ha sok módosításokat kell toomake, PowerShell jobb megoldás lehet. Néhány speciális funkció csak érhetők el a PowerShell használatával.

### <a name="get-hello-powershell-script-for-an-out-of-box-rule"></a>PowerShell-parancsfájl hello lekérése az out-of-box szabály
toosee hello PowerShell-parancsfájl egy out-of-box szabály, jelölje be hello szabály hello szinkronizálási létrehozott szabályok szerkesztőt, és kattintson **exportálása**. A művelet által biztosított, hello PowerShell parancsfájl-adott létrehozott hello szabály.

### <a name="advanced-precedence"></a>Speciális sorrendje
hello out-of-box szinkronizálási szabályok a sorrend értéke 100 kezdődik. Ha több erdővel rendelkezik, és toomake kell sok egyéni módosításokat, majd 99 szinkronizálási szabályok nem elég lehet.

Hello szinkronizálási motor, amely azt szeretné, hogy további szabályok előtt hello out-of-box szabályok találhatók. tooget ezt a viselkedést, kövesse az alábbi lépéseket:

1. Megjelölés hello első out-of-box szinkronizálási szabály (Ez a szabály szerepel a hello **a az AD-felhasználó csatlakozás**) hello szinkronizálási szabály szerkesztőt, és válasszon **exportálása**. Másolja a hello SR azonosító értékét.  
![PowerShell módosítása előtt](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Hello új szinkronizálási szabály létrehozása. Hello szinkronizálási szabály szerkesztő toocreate használhatja azt. Hello szabály tooa PowerShell-parancsfájlba exportálni.
3. Hello tulajdonságban **PrecedenceBefore**, szúrja be a hello azonosító értéket hello out-of-box szabályból. Set hello **sorrend** túl**0**. Ellenőrizze, hogy hello azonosító attribútum egyedi-e, és nem egy GUID Azonosítót, egy másik szabály újbóli felhasználása. Emellett győződjön meg arról, hogy hello **ImmutableTag** tulajdonsága nincs beállítva; ez a tulajdonság csak az out-of-box szabályok állítható be. Mentse hello PowerShell-parancsfájlt, és futtassa. hello eredménye, hogy az egyéni szabály nincs hozzárendelve hello sorrend értéke 100, és minden más szabállyal out-of-box növeli.  
![PowerShell módosítása után](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Akkor is sok egyéni szinkronizálási szabályok használatával hello azonos **PrecedenceBefore** értéke, ha szükséges.


## <a name="enable-synchronization-of-preferreddatalocation"></a>PreferredDataLocation szinkronizálásának engedélyezése
Az Azure AD Connect gondoskodik a hello **PreferredDataLocation** az attribútum **felhasználói** objektumok verziójában 1.1.524.0 és után. Pontosabban a következő módosításokat vezettek:

* hello séma hello objektumtípus **felhasználói** hello az Azure AD-összekötő ki van bővítve tooinclude PreferredDataLocation attribútum, amely karakterlánc típusú, és egyértékű.

* hello séma hello objektumtípus **személy** hello a Metaverse ki van bővítve tooinclude PreferredDataLocation attribútum, amely karakterlánc típusú, és egyértékű.

Alapértelmezés szerint hello PreferredDataLocation attribútum nincs engedélyezve a szinkronizáláshoz, mert nincs megfelelő PreferredDataLocation attribútum a helyszíni Active Directoryban. Manuálisan kell engedélyezné a szinkronizálást.

> [!IMPORTANT]
> Jelenleg az Azure AD lehetővé teszi, hogy hello PreferredDataLocation attribútum szinkronizált felhasználói objektumok és a felhő felhasználói objektumok toobe közvetlenül az Azure AD PowerShell segítségével konfigurálható. Miután engedélyezte a hello PreferredDataLocation attribútum szinkronizálását, le kell állítania az Azure AD PowerShell tooconfigure hello attribútum használatával **szinkronizált felhasználói objektumok** , az Azure AD Connect felülírják őket alapján forrás-attribútumértékek hello a helyszíni Active Directoryban.

> [!IMPORTANT]
> A szeptember 1 2017, az Azure AD nem engedélyezi többé hello PreferredDataLocation attribútum a **szinkronizált felhasználói objektumok** toobe közvetlenül az Azure AD PowerShell segítségével konfigurálható. tooconfigure PreferredLocation attribútum a szinkronizált felhasználói objektumok, csak az Azure AD Connect kell használnia.

A szinkronizálás hello PreferredDataLocation attribútum engedélyezése előtt kell:

 * Először döntse el, mely a helyszíni Active Directory attribútum toobe hello forrásattribútum használja. Típusúnak kell lennie **karakterlánc** és **egyértékű**.

 * Ha korábban konfigurált hello PreferredDataLocation attribútum a felhasználói objektumok meglévő szinkronizálva az Azure AD PowerShell Azure AD-ben, meg kell **backport** hello attribútum értékei toohello megfelelő felhasználói objektumok a helyszíni Active Directoryban.
 
    > [!IMPORTANT]
    > Ha így tesz, nem backport hello attribútum értékek toohello megfelelő felhasználói objektumokat a helyszíni Active Directoryban, az Azure AD Connect eltávolítja hello meglévő attribútumértékek szinkronizálási hello PreferredDataLocation attribútum esetén az Azure AD-ben engedélyezve van.

 * Ajánlott hello forrásattribútum konfigurálja a helyszíni legalább néhány AD-felhasználó objektumokat, ellenőrzésre később is használható.
 
hello lépéseket tooenable szinkronizálási hello PreferredDataLocation attribútum összegezhető, mint:

1. Tiltsa le a szinkronizálásütemező, és ellenőrizze, hogy nincs folyamatban lévő szinkronizálás

2. Adja hozzá a hello forrás attribútum toohello a helyszíni AD-összekötő séma

3. PreferredDataLocation toohello Azure AD-összekötő séma hozzáadása

4. Hozzon létre egy vonatkozó bejövő szinkronizálási szabály tooflow hello attribútum értékét a helyszíni Active Directoryból

5. Egy kimenő szinkronizálási szabály tooflow hello attribútum értéke tooAzure AD létrehozása

6. Futtasson teljes szinkronizálási ciklust

7. Szinkronizálásütemezési engedélyezése

> [!NOTE]
> Ez a szakasz többi hello ezeket a lépéseket, részleteket ismertet. Ezek hello környezet az Azure AD központi telepítés egyerdős topológiával rendelkező és anélküli egyéni szinkronizálási szabályait ismerteti. Ha Többerdős topológia, egyéni szinkronizálási szabályok konfigurálva, vagy egy átmeneti kiszolgálón rendelkezik, ennek megfelelően kell tooadjust hello lépéseket.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. lépés: Tiltsa le a szinkronizálásütemező, és ellenőrizze, hogy nincs folyamatban lévő szinkronizálás
Ellenőrizze a szinkronizálás nem történik meg, amíg hello középső frissítésére a szinkronizálási szabályok tooavoid nem változik lesznek exportált tooAzure AD. toodisable hello beépített szinkronizálásütemezési:

 1. Indítsa el a PowerShell-munkamenetet a hello Azure AD Connect-kiszolgáló.

 2. Tiltsa le az ütemezett szinkronizálás parancsmag futtatásával:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Indítsa el a hello **Synchronization Service Managert** fog tooSTART → által szinkronizálási szolgáltatás.
 
 4. Nyissa meg toohello **műveletek** lapra, és ellenőrizze, nincs művelet, amelynek állapota *"folyamatban."*

![Synchronization Service Managert - ellenőrizze, nincs folyamatban lévő műveletekkel](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-hello-source-attribute-toohello-on-premises-ad-connector-schema"></a>2. lépés: Hello forrás attribútum toohello a helyszíni AD-összekötő hozzáadása séma
Nem minden AD attribútumokat importálta hello a helyszíni AD kapcsolódási térbe. tooadd hello forrás attribútum toohello listája hello importált attribútumok:

 1. Nyissa meg toohello **összekötők** lapján hello Synchronization Service Managert.
 
 2. Kattintson a jobb gombbal a hello **helyszíni AD-összekötő** válassza **tulajdonságok**.
 
 3. Hello előugró párbeszédpanelen nyissa meg toohello **attribútumok kiválasztása** fülre.
 
 4. Ellenőrizze, hogy hello adatforrás-attribútum be van jelölve, a hello attribútum listában.
 
 5. Kattintson a **OK** toosave.

![Adja hozzá a forrás attribútum tooon helyszíni AD-összekötő séma](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-toohello-azure-ad-connector-schema"></a>3. lépés: PreferredDataLocation toohello Azure AD-összekötő séma hozzáadása
Alapértelmezés szerint hello PreferredDataLocation attribútum nincs hello Azure AD Connect terület importálni. tooadd hello PreferredDataLocation attribútum toohello importált attribútumok listáját:

 1. Nyissa meg toohello **összekötők** lapján hello Synchronization Service Managert.

 2. Kattintson a jobb gombbal a hello **Azure AD-összekötő** válassza **tulajdonságok**.

 3. Hello előugró párbeszédpanelen nyissa meg toohello **attribútumok kiválasztása** fülre.

 4. Ellenőrizze, hogy hello PreferredDataLocation attribútum be van jelölve, a hello attribútum listában.

 5. Kattintson a **OK** toosave.

![Adja hozzá a forrás attribútum tooAzure Címtárösszekötőben séma](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-tooflow-hello-attribute-value-from-on-premises-active-directory"></a>4. lépés: A helyszíni Active Directoryból hozzon létre egy vonatkozó bejövő szinkronizálási szabály tooflow hello attribútumérték
hello vonatkozó bejövő szinkronizálási szabályának hello attribútum értéke tooflow a hello forrásattribútumot a helyszíni Active Directory toohello Metaverse lehetővé teszi:

1. Indítsa el a hello **szinkronizálási szabályok szerkesztő** fog tooSTART → által szinkronizálási szabályok szerkesztő.

2. Keresési szűrő beállítása hello **irány** toobe **bejövő**.

3. Kattintson a **új szabály hozzáadása** gomb toocreate új bejövő szabály.

4. A hello **leírás** lapra, adja meg a következő konfigurációs hello:
 
    | Attribútum | Érték | Részletek |
    | --- | --- | --- |
    | Név | *Adjon meg egy nevet* | Például *"az ad-felhasználó PreferredDataLocation"* |
    | Leírás | *Adjon meg egy leírást* |  |
    | Csatlakoztatott rendszer | *Válassza ki a hello a helyszíni AD-összekötő* |  |
    | Objektumtípus csatlakoztatva | **Felhasználó** |  |
    | Metaverzum-objektum típusa | **Személy** |  |
    | Kapcsolat típusa | **Csatlakozás** |  |
    | Sorrend | *Válassza ki az 1 – 99 közötti szám* | 1 – 99 egyéni szinkronizálási szabályok számára van fenntartva. Nem válasszon egy másik szinkronizálási szabály által használt érték. |

5. Nyissa meg toohello **Scoping szűrő** lapra, és adja hozzá a **egyetlen tartalmazó szűrő csoportban található, a következő záradék hello**:
 
    | Attribútum | Operátor | Érték |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Felhasználó\_ | 
 
    Tartalmazó szűrő határozza meg, amely a helyszíni AD-objektumokat a vonatkozó bejövő szinkronizálási szabály vonatkozik. Ebben a példában használjuk azonos tartalmazó szűrő vettük hello *"a az AD-felhasználó közös"* OOB szinkronizálási szabály, amely megakadályozó hello szinkronizálási szabály alkalmazza az Azure AD felhasználó-visszaírás keresztül létrehozott tooUser objektumok a szolgáltatás. Szükség lehet tootweak hello tartalmazó szűrő tooyour az Azure AD Connect telepítési alapján történik.

6. Nyissa meg toohello **átalakítása lapon** és valósítja meg a következő átalakítási szabállyal hello:
 
    | Típusa | TARGET attribútuma | Forrás | Egyszer alkalmazása | Egyesítési típus |
    | --- | --- | --- | --- | --- |
    | Közvetlen | PreferredDataLocation | Válassza ki a hello adatforrás-attribútum | Nincs bejelölve | Frissítés |

7. Kattintson a **Hozzáadás** toocreate hello bejövő szabályt.

![Bejövő szinkronizálási szabályának létrehozása](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-tooflow-hello-attribute-value-tooazure-ad"></a>5. lépés:, Hozzon létre egy kimenő szinkronizálási szabály tooflow hello attribútum értéke tooAzure AD
hello kimenő szinkronizálási szabály lehetővé teszi, az Azure AD hello attribútum értéke tooflow hello Metaverse toohello PreferredDataLocation attribútumból:

1. Nyissa meg toohello **szinkronizálási szabályok** szerkesztő.

2. Keresési szűrő beállítása hello **irány** toobe **kimenő**.

3. Kattintson a **új szabály hozzáadása** gombra.

4. A hello **leírás** lapra, adja meg a következő konfigurációs hello:

    | Attribútum | Érték | Részletek |
    | --- | --- | --- |
    | Név | *Adjon meg egy nevet* | Például "tooAAD – felhasználói PreferredDataLocation" lejárt |
    | Leírás | *Adjon meg egy leírást* |
    | Csatlakoztatott rendszer | *Válassza ki a hello AAD-összekötő* |
    | Objektumtípus csatlakoztatva | Felhasználó ||
    | Metaverzum-objektum típusa | **Személy** ||
    | Kapcsolat típusa | **Csatlakozás** ||
    | Sorrend | *Válassza ki az 1 – 99 közötti szám* | 1 – 99 egyéni szinkronizálási szabályok számára van fenntartva. YDo nem válasszon egy másik szinkronizálási szabály által használt érték. |

5. Nyissa meg toohello **Scoping szűrő** lapra, és adja hozzá egy **egyetlen tartalmazó szűrő csoportban található, két záradékok**:
 
    | Attribútum | Operátor | Érték |
    | --- | --- | --- |
    | sourceObjectType | EGYENLŐ | Felhasználó |
    | cloudMastered | NOTEQUAL | True (Igaz) |

    Hatókörként szűrő határozza meg, mely az Azure AD-objektumokat a kimenő szinkronizálási szabály vonatkozik. A jelen példában használjuk hello azonos tartalmazó szűrő a "Kimenő tooAD – felhasználói identitás" OOB-szinkronizálási szabály. Ez megakadályozza hello szinkronizálási szabály alkalmazott tooUser objektumok, amelyek nincsenek szinkronizálva a helyszíni Active Directoryból. Szükség lehet tootweak hello tartalmazó szűrő tooyour az Azure AD Connect telepítési alapján történik.
    
6. Nyissa meg toohello **átalakítása** lapra, és valósítja meg a következő átalakítási szabállyal hello:

    | Típusa | TARGET attribútuma | Forrás | Egyszer alkalmazása | Egyesítési típus |
    | --- | --- | --- | --- | --- |
    | Közvetlen | PreferredDataLocation | PreferredDataLocation | Nincs bejelölve | Frissítés |

7. Bezárás **Hozzáadás** toocreate hello kimenő forgalomra vonatkozó szabály.

![Kimenő szinkronizálási szabály létrehozása](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>6. lépés: Teljes szinkronizálás futtatása ciklus
Általában teljes szinkronizálási ciklust kell, mivel azt hozzáadta az új attribútumok tooboth hello AD és az Azure AD-összekötő séma, és egyéni szinkronizálási szabályokat vezetett be. Javasoljuk, hogy ellenőrizze az hello módosítások előtt tooAzure AD exportálja őket. Használhatja a hello követő lépéseket tooverify hello módosítások hello lépéseket egy teljes szinkronizálási ciklust alkotó manuális futtatása során. 

1. Futtatás **teljes importálás** hello lépéshez **helyszíni AD-összekötő**:

   1. Nyissa meg toohello **műveletek** lapján hello Synchronization Service Managert.

   2. Kattintson a jobb gombbal a hello **helyszíni AD-összekötő** válassza **futtatása...**

   3. Hello előugró párbeszédpanelen válassza ki a **teljes importálás** kattintson **OK**.
    
   4. Várjon, amíg a művelet toocomplete.

    > [!NOTE]
    > Teljes importálás kihagyhatja a hello a helyszíni AD-összekötő Ha hello adatforrás-attribútum már szerepel a hello importált attribútumok listáját. Ez azt jelenti, hogy nem volt toomake során bármilyen változás [2. lépés: hello forrás attribútum toohello a helyszíni AD-összekötő hozzáadása séma](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Futtatás **teljes importálás** hello lépéshez **Azure AD-összekötő**:

   1. Kattintson a jobb gombbal a hello **Azure AD-összekötő** válassza **futtatása...**

   2. Hello előugró párbeszédpanelen válassza ki a **teljes importálás** kattintson **OK**.
   
   3. Várjon, amíg a művelet toocomplete.

3. Ellenőrizze a felhasználói objektum hello szinkronizálási szabály változását:

hello forrásattribútumot a helyszíni Active Directory és az Azure AD PreferredDataLocation voltak importálja a megfelelő összekötő terület hello. Teljes szinkronizálás lépés folytatása előtt javasoljuk, hogy végezzen egy **előzetes** egy meglévő felhasználó hello objektumban a helyszíni AD kapcsolódási térbe. hello objektum kivételezett, meg kell adni a hello forrásattribútum feltöltve. A sikeres **előzetes** hello hello Metaverse üres PreferredDataLocation egy jól jelzi, hogy megfelelően van konfigurálva hello szinkronizálási szabályait. További információ toodo egy **előzetes**, tekintse meg a toosection [hello módosítás ellenőrizze](#verify-the-change).

4. Futtatás **teljes szinkronizálás** hello lépéshez **helyszíni AD-összekötő**:

   1. Kattintson a jobb gombbal a hello **helyszíni AD-összekötő** válassza **futtatása...**
  
   2. Hello előugró párbeszédpanelen válassza ki a **teljes szinkronizálás** kattintson **OK**.
   
   3. Várjon, amíg a művelet toocomplete.

5. Győződjön meg arról **függőben lévő exportálások** tooAzure AD:

   1. Kattintson a jobb gombbal a hello **Azure AD-összekötő** válassza **Összekötőtér keresési**.

   2. Hello Összekötőtér keresési előugró párbeszédpanelen:

      1. Állítsa be **hatókör** túl**függőben lévő exportálása**.
      
      2. Ellenőrizze a három jelölőnégyzetet, beleértve a **hozzáadása, módosítása és törlése**.
      
      3. Kattintson a hello **keresési** gomb tooget hello exportált módosítások toobe rendelkező objektumok listája. egy adott objektum tooexamine hello módosítások kattintson duplán a hello objektum.
      
      4. Ellenőrizze, hogy hello változások várhatók.

6. Futtatás **exportálása** hello lépéshez **Azure AD Connectoron**
      
   1. Kattintson a jobb gombbal hello **Azure AD-összekötő** válassza **futtatása...**
   
   2. Hello futtatása összekötő előugró párbeszédpanelen válassza ki a **exportálása** kattintson **OK**.
   
   3. Várjon, amíg exportálás tooAzure AD toocomplete.

> [!NOTE]
> Előfordulhat, hogy hello lépéseket nem tartalmaznak hello teljes szinkronizálást és a hello Azure AD-összekötő exportálási lépést. hello lépéseket esetén nincs szükség, mert hello attribútumértékek vannak áramlanak a helyszíni Active Directory tooAzure AD csak.

### <a name="step-7-re-enable-sync-scheduler"></a>7. lépés: Engedélyezze újra a szinkronizálásütemező
Engedélyezze újra a beépített szinkronizálásütemezési hello:

1. Indítsa el a PowerShell-munkamenetben.

2. Újra engedélyezi az ütemezett szinkronizálás parancsmag futtatásával:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Következő lépések
* További információk a hello konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* További információk a hello kifejezés nyelvi [ismertetése deklaratív kiépítés kifejezések](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
