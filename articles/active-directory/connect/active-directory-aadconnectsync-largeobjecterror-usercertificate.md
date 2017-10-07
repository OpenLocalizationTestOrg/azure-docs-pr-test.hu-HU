---
title: "Azure AD Connect szinkronizálása: userCertificate attribútum által okozott kezelése LargeObject hibákat |} Microsoft Docs"
description: "Ez a témakör hello javítási lépéseket userCertificate attribútum által okozott LargeObject hibákat."
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e547fb5f601b92f6f5154de9997651b1f28c51b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect szinkronizálása: userCertificate attribútum által okozott kezelése LargeObject hibák

Az Azure AD érvénybe lépteti a jelenlegi maximális műveletszámot **15** hello értékét tanúsítvány **userCertificate** attribútum. Az Azure AD Connect több mint 15 értékek tooAzure AD objektum exportálása, az Azure AD vissza egy **LargeObject** hiba történt a következő üzenetet:

>*"hello telepített objektum mérete túl nagy. Trim hello számát ezen az objektumon. hello műveletet ismét megkísérli hello a következő szinkronizálási ciklusban..."*

hello LargeObject hiba okozhatja más AD attribútumok. valóban okozta hello userCertificate attribútum tooconfirm, kell tooverify hello objektum vagy a helyszíni AD ellen, vagy a hello [Synchronization Service Managert Metaverzum-keresés](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

az Ön bérlőjében LargeObject hibákkal objektumok listájának tooobtain hello hello a következő módszerek valamelyikével:

 * Ha a bérlő az Azure AD Connect Health szinkronizálási szolgáltatás engedélyezve van, olvassa el a toohello [szinkronizálási esetleges hibajelentésben való megjelenítéshez](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync#object-level-synchronization-error-report-preview) megadott.
 
 * hello értesítő e-mailt a címtár-szinkronizálási hibák hello végén lévő minden egyes szinkronizálási ciklusban küldött rendelkezik hello LargeObject hibákkal objektumok listája. 
 * Hello [Synchronization Service Manager Operations lapon](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) LargeObject hibákkal objektumok hello listáját jeleníti meg, ha hello legújabb tooAzure AD exportálás kattint.
 
## <a name="mitigation-options"></a>Megoldás beállításai
Hello LargeObject hiba megoldásáig más attribútum módosítások toohello ugyanazt az objektumot nem lehet exportálni tooAzure AD. tooresolve hello hiba, érdemes lehet a következő beállítások hello:

 * Az Azure AD Connect toobuild 1.1.524.0 frissítése vagy után. Az Azure AD Connectben build 1.1.524.0, hogy hello out-of-box szinkronizálási szabályok frissített toonot exportálási attribútumok userCertificate és userSMIMECertificate Ha hello attribútumok értéke meghaladja a 15. Kapcsolatos részletes tudnivalókért tooupgrade az Azure AD Connect, tekintse meg a tooarticle [az Azure AD Connect: frissítés a legújabb egy korábbi verziója toohello](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

 * Alkalmazzon egy **kimenő szinkronizálási szabályt** az Azure AD Connectben exportálhatók egy **null érték helyett hello tényleges értékek-objektumok több mint 15 tanúsítvány értékekkel**. Ez a beállítás akkor megfelelő, ha nincs szüksége a hello tanúsítvány értékek exportált toobe tooAzure AD objektumok több mint 15 értékekkel. Részletes tájékoztatás a szinkronizálási szabály, tekintse meg a toonext szakasz tooimplement [megvalósítása szinkronizálási szabály toolimit exportálási userCertificate attribútum](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Tanúsítvány értékek a hello hello számának csökkentése a helyszíni AD-objektum (legfeljebb 15), amelyeket már nem használja a szervezet értékek eltávolításával. Ez a megfelelő, ha a lejárt vagy nem használt tanúsítványok hello attribútum túlzott növekedést okozta. Használhatja a hello [PowerShell parancsfájl érhető el itt](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) toohelp található, biztonsági mentését, és törli a lejárt tanúsítványokat a helyszíni AD. Hello tanúsítványok törlése, előtt ajánlott, hogy ellenőrizze a hello nyilvános-kulcs-infrastruktúra rendszergazdái a szervezetében.

 * Konfigurálja az Azure AD Connect tooexclude hello userCertificate attribútum nem exportált tooAzure AD. Általában nem ezt a beállítást ajánlott, mivel hello attribútum is használható a Microsoft Online Services tooenable meghatározott forgatókönyvek szerint. Ebben az esetben:
    * hello userCertificate attribútum hello User objektum használják az Exchange Online és az Outlook ügyfelek üzenet aláíráshoz és titkosításhoz. További információ az ezzel a funkcióval toolearn tekintse meg a tooarticle [S/MIME, az üzenet aláírási és titkosítási](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * az Azure AD tooallow Windows 10 helyi tartományhoz csatlakozó eszközök tooconnect tooAzure AD hello userCertificate attribútum hello számítógép-objektum használja. További információ az ezzel a funkcióval toolearn tekintse meg a tooarticle [csatlakozni a tartományhoz csatlakozó eszközök tooAzure AD, a Windows 10 észlel](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-toolimit-export-of-usercertificate-attribute"></a>Szinkronizálási szabály toolimit exportálási userCertificate attribútum megvalósítása
tooresolve hello LargeObject hiba hello userCertificate attribútum által okozott, is létrehozható egy kimenő szinkronizálási szabályt az Azure AD Connectben exportálhatók egy **null érték helyett hello tényleges értékek objektumok több mint 15 tanúsítvány értékek**. Ez a szakasz ismerteti a hello lépéseket szükséges tooimplement hello szinkronizálási szabály **felhasználói** objektumok. hello lépéseket is módosítani kell a **forduljon** és **számítógép** objektumok.

> [!IMPORTANT]
> Értékek korábban exportált sikeresen tooAzure AD tanúsítvány exportálása a null érték eltávolítja.

hello lépéseket összegezhető:

1. Tiltsa le a szinkronizálásütemező, és ellenőrizze, hogy nincs folyamatban szinkronizálás.
3. Hello meglévő kimenő szinkronizálási szabály található userCertificate attribútum.
4. Szükséges hello kimenő szinkronizálási szabály létrehozása.
5. Ellenőrizze az új szinkronizálási szabály hello LargeObject hibával objektum.
6. Hello új szinkronizálási szabály tooremaining objektumok LargeObject hibával alkalmazni.
7. Ellenőrizze, hogy nincsenek exportált toobe tooAzure AD Várakozás váratlan módosítások.
8. Exportálja a hello módosítások tooAzure AD.
9. Engedélyezze újra a szinkronizálásütemező.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. lépés Tiltsa le a szinkronizálásütemező, és ellenőrizze, hogy nincs folyamatban lévő szinkronizálás
Ellenőrizze a szinkronizálás nem történik meg, amíg egy új szinkronizálási végrehajtási hello középső szabály tooavoid nem változik lesznek exportált tooAzure AD. toodisable hello beépített szinkronizálásütemezési:
1. Indítsa el a PowerShell-munkamenetet a hello Azure AD Connect-kiszolgáló.

2. Tiltsa le az ütemezett szinkronizálás parancsmag futtatásával:`Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> hello előző lépések csak megfelelő toonewer verziója (1.1.xxx.x) az Azure AD Connect és hello beépített ütemező. Az Azure AD Connect Windows Feladatütemezőt használó régebbi verziók (1.0.xxx.x) használ, vagy használja a saját egyéni Feladatütemező (általános) tootrigger rendszeresen szinkronizálja, ha szüksége van-e toodisable őket ennek megfelelően.

3. Indítsa el a hello **Synchronization Service Managert** fog tooSTART → által szinkronizálási szolgáltatás.

4. Nyissa meg toohello **műveletek** lapra, és ellenőrizze, nincs művelet, amelynek állapota *"folyamatban."*

### <a name="step-2-find-hello-existing-outbound-sync-rule-for-usercertificate-attribute"></a>2. lépés Hello meglévő kimenő szinkronizálási szabály található userCertificate attribútum
Meglévő szinkronizálási szabály engedélyezve van, és konfigurált felhasználói objektumok tooAzure AD tooexport userCertificate attribútumot kell lennie. Keresse meg a szinkronizálási szabály toofind ki a **sorrend** és **tartalmazó szűrő** konfiguráció:

1. Indítsa el a hello **szinkronizálási szabályok szerkesztő** fog tooSTART → által szinkronizálási szabályok szerkesztő.

2. A következő értékek hello hello keresési szűrők konfigurálása:

    | Attribútum | Érték |
    | --- | --- |
    | Irány |**Kimenő** |
    | MV-objektum típusa |**Személy** |
    | összekötő |*az Azure AD-összekötő neve* |
    | Összekötő objektum típusa |**felhasználó** |
    | MV-attribútum |**userCertificate** |

3. Ha sávon kívüli (out-of-box) szinkronizálási szabályok tooAzure AD összekötő tooexport userCertficiate attribútum esetén a felhasználói objektumok használ, meg kell visszakapnia hello *"lejárt tooAAD – felhasználói ExchangeOnline"* szabály.
4. Jegyezze fel a hello **sorrend** értékét a szinkronizálási szabály.
5. Jelöljön ki hello szinkronizálási szabályt, majd **szerkesztése**.
6. A hello *"Fenntartott szabály megerősítő szerkesztése"* előugró párbeszédpanelen kattintson a **nem**. (Ne aggódjon, nem fogjuk toomake bármely toothis szinkronizálási szabály módosítása).
7. Hello Szerkesztés képernyőn válassza ki a hello **Scoping szűrő** fülre.
8. Jegyezze fel hello hatókörére szűrő konfigurálása. Ha hello OOB szinkronizálási szabályt használ, pontosan kell **egy hatókört szűrő csoportja két záradékok**, többek között a következőket:

    | Attribútum | Operátor | Érték |
    | --- | --- | --- |
    | sourceObjectType | EGYENLŐ | Felhasználó |
    | cloudMastered | NOTEQUAL | True (Igaz) |

### <a name="step-3-create-hello-outbound-sync-rule-required"></a>3. lépés Szükséges hello kimenő szinkronizálási szabály létrehozása
hello új szinkronizálási szabályt kell rendelkeznie hello azonos **tartalmazó szűrő** és **magasabb prioritással** , mint a meglévő szinkronizálási szabály hello. Ez biztosítja, hogy hello új szinkronizálási szabály azonos állítja be az objektumok hello meglévő szinkronizálási szabályok és felülbírálások hello meglévő szinkronizálási szabályok hello userCertificate attribútum toohello vonatkozik. toocreate hello szinkronizálási szabályt:
1. A szinkronizálási szabályok szerkesztő hello, kattintson a hello **új szabály hozzáadása** gombra.
2. A hello **Leírás lapon**, adja meg a következő konfigurációs hello:

    | Attribútum | Érték | Részletek |
    | --- | --- | --- |
    | Név | *Adjon meg egy nevet* | Például *"Kimenő tooAAD – egyéni felülírása userCertificate"* |
    | Leírás | *Adjon meg egy leírást* | Például *"UserCertificate attribútum több mint 15 értékkel rendelkezik, ha exportálja NULL."* |
    | Csatlakoztatott rendszer | *Válassza ki a hello Azure AD Connectoron* |
    | Objektumtípus csatlakoztatva | **felhasználó** | |
    | Metaverzum-objektum típusa | **személy** | |
    | Kapcsolat típusa | **Csatlakozás** | |
    | Sorrend | *1 és 99 közötti számnak választott* | hello számra megválasztani nem használhatja a meglévő szinkronizálási szabály és alsó értéke (és így magasabb prioritással), mint a meglévő szinkronizálási szabály hello. |

3. Nyissa meg toohello **Scoping szűrő** lapra, és azonos tartalmazó szűrő hello meglévő szinkronizálási szabály által használt hello megvalósításához.
4. Kihagyás hello **szabályok csatlakozás** fülre.
5. Nyissa meg toohello **átalakítások** tooadd egy új átalakítása használatával konfigurációs következő lapon:

    | Attribútum | Érték |
    | --- | --- |
    | Típusa |**Kifejezés** |
    | TARGET attribútuma |**userCertificate** |
    | Adatforrás-attribútum |*A következő kifejezés használata hello*:`IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Kattintson a hello **Hozzáadás** gomb toocreate hello szinkronizálási szabály.

### <a name="step-4-verify-hello-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>4. lépés Ellenőrizze az új szinkronizálási szabály hello LargeObject hibával objektum
Ez az, hogy a szinkronizálási hello tooverify létrehozott szabály megfelelően működik-e egy meglévő AD-objektum LargeObject hiba: a alkalmazása előtt tooother objektumok:
1. Nyissa meg toohello **műveletek** lapján hello Synchronization Service Managert.
2. Válassza ki a hello legutóbbi tooAzure AD exportálás, majd kattintson a LargeObject hibákkal hello objektumok közül.
3.  Hello Összekötőtér-objektum tulajdonságai előugró képernyő, kattintson a hello **előzetes** gombra.
4. Hello Preview előugró képernyőn válassza ki a **teljes szinkronizálás** kattintson **véglegesítése előzetes**.
5. Hello előnézet képernyőn és hello Összekötőtér-objektum tulajdonságai képernyő bezárása.
6. Nyissa meg toohello **összekötők** lapján hello Synchronization Service Managert.
7. Kattintson a jobb gombbal a hello **az Azure AD** összekötő, és válassza ki **futtatása...**
8. Hello futtatása összekötő előugró ablakban válassza **exportálása** . lépés:, és kattintson a **OK**.
9. Várjon, amíg az Exportálás tooAzure AD toocomplete, és ellenőrizze, nincs több LargeObject hiba ezen az objektumon.

### <a name="step-5-apply-hello-new-sync-rule-tooremaining-objects-with-largeobject-error"></a>5. lépés Hello új szinkronizálási szabály tooremaining objektumok LargeObject hibával alkalmazása
Miután hello szinkronizálási szabály hozzá lett adva, a teljes szinkronizálás lépéshez hello Címtárösszekötőben toorun kell:
1. Nyissa meg toohello **összekötők** lapján hello Synchronization Service Managert.
2. Kattintson a jobb gombbal a hello **AD** összekötő, és válassza ki **futtatása...**
3. Hello futtatása összekötő előugró ablakban válassza **teljes szinkronizálás** . lépés:, és kattintson a **OK**.
4. Várjon, amíg hello teljes szinkronizálás lépés toocomplete.
5. Ismételje meg a fennmaradó AD összekötők, ha egynél több AD összekötők hello hello a fenti lépéseket. Általában több összekötő szükség, ha több helyszíni címtárakban.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-toobe-exported-tooazure-ad"></a>6. lépés Ellenőrizze, hogy nincsenek exportált toobe tooAzure AD Várakozás váratlan módosítások
1. Nyissa meg toohello **összekötők** lapján hello Synchronization Service Managert.
2. Kattintson a jobb gombbal a hello **az Azure AD** összekötő, és válassza ki **Összekötőtér keresési**.
3. Összekötőtér keresési előugró hello:
    1. Hatókör beállítása túl**függőben lévő exportálása**.
    2. Ellenőrizze a 3 jelölőnégyzeteket, beleértve a **Hozzáadás**, **módosítás** és **törlése**.
    3. Kattintson a **keresési** gomb tooreturn minden exportált toobe tooAzure AD Várakozás módosításokkal objektumokat.
    4. Ellenőrizze, hogy nincsenek váratlan módosítások. egy adott objektum tooexamine hello módosítások kattintson duplán a hello objektum.

### <a name="step-7-export-hello-changes-tooazure-ad"></a>7. lépés Hello módosítások tooAzure AD exportálása
tooexport hello módosítások tooAzure AD:
1. Nyissa meg toohello **összekötők** lapján hello Synchronization Service Managert.
2. Kattintson a jobb gombbal a hello **az Azure AD** összekötő, és válassza ki **futtatása...**
4. Hello futtatása összekötő előugró ablakban válassza **exportálása** . lépés:, és kattintson a **OK**.
5. Várjon, amíg az Exportálás tooAzure AD toocomplete, és ellenőrizze, hogy nincsenek további LargeObject hibák.

### <a name="step-8-re-enable-sync-scheduler"></a>8. lépés. Engedélyezze újra a szinkronizálásütemező
Most, hogy hello probléma megoldódik, engedélyezze újra a beépített szinkronizálásütemezési hello:
1. Indítsa el a PowerShell-munkamenetben.
2. Újra engedélyezi az ütemezett szinkronizálás parancsmag futtatásával:`Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> hello előző lépések csak megfelelő toonewer verziója (1.1.xxx.x) az Azure AD Connect és hello beépített ütemező. Az Azure AD Connect Windows Feladatütemezőt használó régebbi verziók (1.0.xxx.x) használ, vagy használja a saját egyéni Feladatütemező (általános) tootrigger rendszeresen szinkronizálja, ha szüksége van-e toodisable őket ennek megfelelően.

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

