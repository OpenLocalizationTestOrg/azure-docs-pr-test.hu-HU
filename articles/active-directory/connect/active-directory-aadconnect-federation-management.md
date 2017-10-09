---
title: "aaaActive Directory összevonási szolgáltatások kezelése és testreszabása az Azure AD Connect |} Microsoft Docs"
description: "AD FS felügyelet az Azure AD Connect és felhasználói AD FS bejelentkezési élmény és az Azure AD Connect PowerShell testreszabása."
keywords: "Az AD FS-, ADFS, az AD FS felügyeleti AAD-csatlakozás, csatlakozás, bejelentkezés, az AD FS testreszabás javítani bizalmi kapcsolat, O365, összevonási, függő entitás"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 361a2bfd6d7a6993dbe773d6ea039ad1afc6346a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Kezelése és testreszabása az Active Directory összevonási szolgáltatások az Azure AD Connect használatával
Ez a cikk ismerteti, hogyan toomanage és testre szabhatja az Active Directory összevonási szolgáltatások (AD FS) Azure Active Directory (Azure AD) Connect használatával. Egyéb gyakori AD FS feladatokat, hogy szükség lehet toodo egy AD FS-farm teljes konfiguráció is tartalmaz.

| Témakör | Ez vonatkozik |
|:--- |:--- |
| **Az AD FS kezelése** | |
| [Hello megbízhatóság javítása](#repairthetrust) |Hogyan toorepair hello összevonási megbízhatóság, és az Office 365. |
| [Az Azure AD használatával másodlagos bejelentkezési Azonosítóval összevonni](#alternateid) | Másodlagos bejelentkezési azonosítóval összevonás konfigurálása  |
| [Az AD FS-kiszolgáló hozzáadása](#addadfsserver) |Hogyan tooexpand az AD FS farm kerül egy további AD FS-kiszolgálóval. |
| [AD FS webalkalmazás-Proxy kiszolgáló hozzáadása](#addwapserver) |Hogyan tooexpand az AD FS farm kerül egy további webalkalmazás-proxykiszolgálóként (WAP) kiszolgálóval. |
| [Összevont tartomány hozzáadása](#addfeddomain) |Hogyan tooadd egy összevont tartományt. |
| [Frissítés hello SSL-tanúsítvány](active-directory-aadconnectfed-ssl-update.md)| Hogyan tooupdate hello SSL tanúsítvány egy AD FS-farm. |
| **Az AD FS testreszabása** | |
| [Adjon hozzá egy egyéni vállalati emblémát vagy ábra](#customlogo) |Hogyan toocustomize az AD FS-Bejelentkezés lapon a vállalati embléma és ábra. |
| [Adjon meg egy bejelentkezési leírást](#addsignindescription) |A bejelentkezés tooadd hogyan lap leírása. |
| [AD FS jogcím szabályok módosítása](#modclaims) |Hogyan toomodify AD FS összevonási több, különböző esetekre jogcímek. |

## <a name="manage-ad-fs"></a>Az AD FS kezelése
Különböző AD FS-sel kapcsolatos feladat hello Azure AD Connect varázsló segítségével elvégezhető az Azure AD Connect minimális felhasználói beavatkozás. Miután befejezte a futó hello varázsló az Azure AD Connect telepítése, hello varázsló futtatásával újra tooperform további feladatokat.

## Hello megbízhatóság javítása<a name=repairthetrust></a>
Az Azure AD Connect toocheck hello aktuális állapotának hello AD FS és az Azure AD-megbízhatóság, és megfelelő műveleteket toorepair hello megbízhatósági igénybe vehet. Kövesse ezeket a lépéseket toorepair az Azure AD és az AD FS megbízható.

1. Válassza ki **javítási aad-ben és az AD FS megbízható** hello további feladatok listája.
   ![Javítsa az aad-ben és az AD FS megbízható](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)

2. A hello **tooAzure AD Connect** lapon adja meg az Azure AD globális rendszergazdai hitelesítő adataival, és kattintson a **következő**.
   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)

3. A hello **távelérési hitelesítő adatainak** lapján adja meg hello tartományi rendszergazda hello hitelesítő adatait.

   ![Távoli hozzáférési hitelesítő adatok](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    Miután rákattintott **következő**, az Azure AD Connect tanúsítványállapot keres, és az esetleges problémákat jeleníti meg.

    ![A tanúsítványok állapota](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    Hello **készen tooconfigure** lapra mutat be hello listája, amely lesz műveletek végre toorepair hello megbízhatósági.

    ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. Kattintson a **telepítése** toorepair hello megbízhatósági.

> [!NOTE]
> Az Azure AD Connect is csak javítása vagy a törvény önaláírt tanúsítványokat. Az Azure AD Connect harmadik féltől származó tanúsítványok nem lehet kijavítani.

## Az Azure AD használatával AlternateID összevonni<a name=alternateid></a>
Javasoljuk, hogy hello a helyszíni egyszerű Name(UPN) és egyszerű felhasználónév tartják hello felhő hello azonos. Ha hello a helyszíni egyszerű Felhasználónévvel (pl. egy nem átirányítható tartományt használja. Contoso.local), vagy nem módosítható miatt toolocal alkalmazásfüggőségek, azt javasoljuk másodlagos bejelentkezési azonosító beállítása Másodlagos bejelentkezési Azonosítóval is lehetővé teszi a bejelentkezés tapasztalhat, ha nem az egyszerű Felhasználónevük, például a mail attribútum felhasználók bejelentkezhetnek tooconfigure. hello választás az egyszerű felhasználónév az Azure AD Connect alapértelmezett toohello userPrincipalName attribútum az Active Directoryban. Ha az attribútum válasszon az egyszerű felhasználónév, és az AD FS segítségével összevonja, majd az Azure AD Connect AD FS beállítják másodlagos bejelentkezési azonosítóval. Egy másik attribútum kiválasztása az egyszerű felhasználónév példát alább találja:

![Alternatív ID attribútum kiválasztása](media/active-directory-aadconnect-federation-management/attributeselection.png)

Másodlagos bejelentkezési azonosító konfigurálása az AD FS két fő lépésből áll:
1. **Kiállítási jogcímek megfelelő halmazát hello konfigurálása**: hello kiállítási jogcímszabályok függő entitás hello Azure AD megbízik áll a másik azonosító hello felhasználó hello módosított toouse kijelölt hello UserPrincipalName attribútum.
2. **Engedélyezze a másodlagos bejelentkezési Azonosítóval is hello AD FS konfiguráció**: hello AD FS konfigurációs frissül, hogy az AD FS hello alternatív azonosítójával hello megfelelő erdőkben található felhasználókat is kereshet. Ez a konfiguráció az AD FS Windows Server 2012 R2 (az KB2919355) vagy újabb rendszeren támogatott. Ha hello AD FS-kiszolgáló 2012 R2, a hello hello meglétének ellenőrzése az Azure AD Connect KB szükséges. Ha a hello KB nem észlel, egy figyelmeztetés jelenik meg konfiguráció befejezése után alább látható módon:

    ![A hiányzó KB 2012R2 a figyelmeztetés](media/active-directory-aadconnect-federation-management/kbwarning.png)

    toorectify hello konfigurációs hiányzó KB esetén telepítse a szükséges hello [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) , és javítsa a hello megbízhatósági használatával [javítása aad-ben és az AD FS-megbízhatóság](#repairthetrust).

> [!NOTE]
> További információ a alternateID és lépéseket toomanually konfigurálása, olvassa el [másodlagos bejelentkezési azonosító beállítása](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## Az AD FS-kiszolgáló hozzáadása<a name=addadfsserver></a>

> [!NOTE]
> tooadd az AD FS-kiszolgáló, az Azure AD Connect hello PFX-tanúsítványokat igényel. Ezért a művelet végrehajtása, csak ha hello AD FS-farm konfigurálta az Azure AD Connect használatával.

1. Válassza ki **további összevonási kiszolgáló üzembe helyezése**, és kattintson a **következő**.

   ![További összevonási kiszolgáló](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. A hello **tooAzure AD Connect** lapon adja meg az Azure AD globális rendszergazdai hitelesítő adataival, és kattintson a **következő**.

   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. Hello tartományi rendszergazdai hitelesítő adatok megadásához.

   ![Tartományi rendszergazda hitelesítő adatai](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. Az Azure AD Connect hello PFX-fájl az új AD FS-farm konfigurálása az Azure AD Connect közben megadott hello jelszót kér. Kattintson a **jelszó megadása** tooprovide hello jelszó hello PFX-fájljának.

   ![Tanúsítvány jelszava](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Adja meg az SSL-tanúsítvány](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. A hello **AD FS-kiszolgálók** lapján adja meg a hello kiszolgáló neve vagy IP-cím toobe hozzáadott toohello AD FS-farm.

   ![AD FS-kiszolgálók](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. Kattintson a **következő**, és halad át a végső hello **konfigurálása** lap. Miután az Azure AD Connect befejezte a kiszolgálók toohello AD FS-farm hello hozzáadása, az aktiválási hello beállítás tooverify hello kapcsolat.

   ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![A telepítés befejeződött](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## AD FS WAP-kiszolgáló hozzáadása<a name=addwapserver></a>

> [!NOTE]
> a WAP-kiszolgáló tooadd, az Azure AD Connect hello PFX-tanúsítvány szükséges. Ezért csak végezheti el a művelet Ha hello AD FS-farm konfigurálta az Azure AD Connect használatával.

1. Válassza ki **webalkalmazás-Proxy telepítése** hello rendelkezésre álló feladatok közül.

   ![Webalkalmazás-Proxy telepítéséhez](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. Adja meg a hello Azure globális rendszergazdai hitelesítő adatokat.

   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. A hello **adja meg az SSL-tanúsítvány** lapon, az Azure AD Connect hello AD FS-farm konfigurálása során megadott hello PFX-fájl hello jelszót adjon meg.
   ![Tanúsítvány jelszava](media/active-directory-aadconnect-federation-management/WapServer3.PNG)

    ![Adja meg az SSL-tanúsítvány](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. Hello server toobe fel, mert a WAP-kiszolgáló hozzáadása. Hello WAP-kiszolgáló nem feltétlenül illesztett toohello tartományban, mert hello varázsló hozzáadni kívánt toohello kiszolgáló rendszergazdai hitelesítő adatokat kér.

   ![A felügyeleti kiszolgáló hitelesítő adatai](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. A hello **Proxy megbízhatósági hitelesítő adatok** lapján adja meg a rendszergazdai hitelesítő adatokkal tooconfigure hello proxykiszolgáló megbízhatóság és a hozzáférés hello elsődleges kiszolgáló hello AD FS-farm.

   ![Proxy megbízhatósági hitelesítő adatai](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. A hello **készen tooconfigure** lap, a hello varázsló végrehajtott műveletek hello listáját jeleníti meg.

   ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. Kattintson a **telepítése** toofinish hello konfigurációs. Hello konfiguráció befejezése után hello varázsló ad meg a beállítás tooverify hello hello kapcsolat toohello kiszolgálók. Kattintson a **ellenőrizze** toocheck kapcsolat.

   ![A telepítés befejeződött](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## Összevont tartomány hozzáadása<a name=addfeddomain></a>

A tartomány toobe összevont Azure AD-val az Azure AD Connect használatával könnyen tooadd. Az Azure AD Connect hello tartományt összevonásra hozzáadja, és módosítja a hello jogcím szabályok toocorrectly hello kibocsátó tükrözik, és az Azure AD összevont több tartomány esetén.

1. tooadd összevont tartományt, jelölje be hello feladat **adnia egy további Azure AD-tartomány**.

   ![További Azure AD-tartomány](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. Hello hello varázsló következő lapján adja meg az Azure AD hello globális rendszergazda hitelesítő adatait.

   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. A hello **távelérési hitelesítő adatainak** lapján hello tartományi rendszergazdai hitelesítő adatok megadásához.

   ![Távoli hozzáférési hitelesítő adatok](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. A következő lapon hello a hello varázsló lehet összevonást végezni a helyszíni címtár az Azure AD-tartományok listáját tartalmazza. Válassza ki a hello tartomány hello listából.

   ![Az Azure AD-tartomány](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    Hello tartomány kiválasztása után hello varázsló segítségével, akkor megfelelő információkkal kapcsolatos további műveleteket varázsló hello fogad, majd hello hello konfigurációs hatását. Bizonyos esetekben egy tartományhoz, amely nincs még ellenőrzése az Azure AD-hello varázsló nyújt információkat toohelp választásakor ellenőriznie hello tartomány. Lásd: [adja hozzá az egyéni tartomány nevét tooAzure Active Directory](../active-directory-add-domain.md) további részleteket.

5. Kattintson a **Tovább** gombra. Hello **készen tooconfigure** lap, amely az Azure AD Connect hajtja végre műveletek hello listáját jeleníti meg. Kattintson a **telepítése** toofinish hello konfigurációs.

   ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> Hello felhasználóit hozzá nem tud toologin tooAzure AD összevont tartományt kell szinkronizálni.

## <a name="ad-fs-customization"></a>AD FS Testreszabás
hello következő szakaszok részletesen bemutatják hello gyakori feladatokat, hogy tooperform előfordulhat, ha a az AD FS bejelentkezési oldal testreszabása.

## Adjon hozzá egy egyéni vállalati emblémát vagy ábra<a name=customlogo></a>
toochange hello embléma hello vállalat hello megjelenő **bejelentkezési** lapján hello a következő Windows PowerShell-parancsmagját és szintaxisát használja.

> [!NOTE]
> ajánlott a dimenziók a hello embléma 260 x 35 pixel, legfeljebb 10 KB-os fájlméretben 96 dpi hello.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> Hello *TargetName* paraméter megadása kötelező. hello alapértelmezett témájának az AD FS alapértelmezett neve.

## Adjon meg egy bejelentkezési leírást<a name=addsignindescription></a>
a bejelentkezési oldal leírás toohello tooadd **bejelentkezési oldal**, hello a következő Windows PowerShell-parancsmagját és szintaxisát használja.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in tooContoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## AD FS jogcím szabályok módosítása<a name=modclaims></a>
Az AD FS támogatja a gazdag jogcímszabályokat nyelv használható toocreate egyéni jogcímszabályokat. További információkért lásd: [hello Jogcímszabály nyelvének szerepe hello](https://technet.microsoft.com/library/dd807118.aspx).

hello következő részek a hogyan írhat egyéni szabályok a lehetséges, hogy tooAzure AD és az AD FS összevonási vonatkoznak.

### <a name="immutable-id-conditional-on-a-value-being-present-in-hello-attribute"></a>Feltételes a hello attribútum szerepel egy érték nem módosítható azonosítója
Az Azure AD Connect lehetővé teszi, hogy az attribútum toobe használt objektumok esetén egy forráshorgonyt szinkronizálva tooAzure AD meg. Ha hello hello egyéni attribútum értéke nem üres, érdemes tooissue egy nem módosítható Azonosítót jogcímet.

Például kiválaszthatja **ms-ds-consistencyguid** hello attribútumaként hello forráshorgony és probléma **ImmutableID** , **ms-ds-consistencyguid** az eset hello attribútum értéke rajta. Ha nincs érték hello attribútum ellen, **objectGuid** , hello nem módosítható azonosítót. Hogyan hozhat létre egyéni jogcímszabályok hello készlete, hello a következő szakaszban leírtak szerint.

**1. szabály: Lekérdezés attribútumok**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Ebben a szabályban hello értékének lekérdezésére **ms-ds-consistencyguid** és **objectGuid** hello felhasználóhoz az Active Directoryból. Hello tároló neve tooan megfelelő tároló nevének módosítása a az AD FS üzembe helyezése. Meghatározott is módosítani a hello jogcím típusa tooa megfelelő jogcím típusát az összevonási **objectGuid** és **ms-ds-consistencyguid**.

Továbbá használatával **hozzáadása** és nem **probléma**, elkerülheti a kimenő problémát hello entitás hozzáadása, és köztes értékként hello értékeket is használhat. Egy újabb szabály hello jogcímet állít ki, melyik érték toouse, nem módosítható azonosítót. hello létrehozása után

**2. szabály: Ellenőrizze, hogy létezik-e az ms-ds-consistencyguid hello felhasználó**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Ez a szabály meghatározása nevű ideiglenes jelzőt **idflag** , amely túl van-e állítva**useguid** esetén nem **ms-ds-consistencyguid** hello felhasználó feltöltve. hello logika ez mögött hello tényt, hogy az AD FS nem engedélyezi üres jogcímeket. Ezért amikor hozzáadja jogcímek http://contoso.com/ws/2016/02/identity/claims/objectguid és http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid Rule 1, hogy végül egy **msdsconsistencyguid** jogcímet csak akkor, ha hello érték hello felhasználó fel van töltve. Ha azt nem üres, az AD FS látja, hogy egy üres érték fog szerepelni, elutasítja azokat azonnal. Minden objektumot kell **objectGuid**, így az igény mindig van 1. szabály végrehajtása után.

**3. szabály: Ki az ms-ds-consistencyguid ID nem módosítható, ha telepítve**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Ez az egy implicit **Exist** ellenőrzése. Ha hello értéket hello jogcímet, hogyan adhat ki, hogy hello nem módosítható, azonosítóját. hello előző példa hello **nameidentifier** jogcímek. Összekapcsolta toochange a toohello megfelelő jogcímtípus hello nem módosítható a(z) a környezetben.

**4. szabály: Ki objectGuid ID nem módosítható, ha az ms-ds-consistencyGuid nincs jelen**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

A szabály, akkor még egyszerűen ellenőrzése hello ideiglenes jelző **idflag**. Úgy dönt, hogy tooissue hello igénylés alapú annak értékét.

> [!NOTE]
> Ezek a szabályok sorrendjének hello fontos.

### <a name="sso-with-a-subdomain-upn"></a>Egyszerű felhasználónév altartomány egyszeri bejelentkezés
Összevont Azure AD Connect használatával, a több tartomány toobe is hozzáadhat [hozzá egy új összevont tartományt](active-directory-aadconnect-federation-management.md#addfeddomain). Módosítania kell hello felhasználó egyszerű felhasználónév (UPN) jogcím, hogy hello kibocsátó azonosítója megegyezik toohello gyökértartományt, valamint nem hello altartomány, mert hello összevont gyökértartomány is magában foglalja a hello gyermek.

Alapértelmezés szerint hello jogcímszabály azonosítója be van állítva az kibocsátó:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Alapértelmezett kibocsátó azonosító jogcím](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

hello alapértelmezett szabály egyszerűen hello egyszerű Felhasználónévi utótagot vesz igénybe, és használja hello kibocsátó azonosító jogcímek. Például John sub.contoso.com felhasználóként, és contoso.com össze van vonva az Azure ad-val. John megadja john@sub.contoso.com , a felhasználónév hello tooAzure AD bejelentkezés során. hello alapértelmezett kibocsátó azonosító jogcímszabály az AD FS kezeli a következő módon hello:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**A jogcím értéke:** http://sub.contoso.com/adfs/services/trust/

toohave csak hello gyökértartomány hello kiállító jogcím értéke, hello jogcím szabály toomatch hello következő módosítása:

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Következő lépések
További információ [felhasználói bejelentkezés lehetőségei](active-directory-aadconnect-user-signin.md).
