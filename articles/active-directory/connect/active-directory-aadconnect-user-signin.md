---
title: "Az Azure AD Connect: Felhasználói bejelentkezés |} Microsoft Docs"
description: "Az Azure AD Connect felhasználói bejelentkezés az egyéni beállításokhoz."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 7848b419f3855b25cfa074a46779d258bd534bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Az Azure AD Connect felhasználói bejelentkezés lehetőségei
Az Azure Active Directory (Azure AD) Connect lehetővé teszi, hogy a felhasználók toosign tooboth felhőben és helyszíni erőforrások használatával hello ugyanazt a jelszót. Ez a cikk ismerteti az alapfogalmakat minden identitás modell toohelp a beállítani kívánt toouse tooAzure AD bejelentkezés hello identitását választja.

Ha már ismeri hello Azure AD identity modell, és szeretné, hogy egy adott módszerrel kapcsolatos további toolearn, lásd: hello megfelelő hivatkozásra:

* [A jelszó-szinkronizálás](#password-synchronization) rendelkező [egyszeri bejelentkezés (SSO)](active-directory-aadconnect-sso.md)
* [Áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md)
* [Összevont egyszeri Bejelentkezést (az Active Directory összevonási szolgáltatások (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)

## <a name="choosing-hello-user-sign-in-method-for-your-organization"></a>Hello felhasználói bejelentkezési módszer kiválasztása szervezete számára
A legtöbb szervezet számára, tooenable felhasználói bejelentkezési tooOffice 365, SaaS-alkalmazásokhoz és más Azure AD-alapú erőforrások csak szeretne ajánlott hello alapértelmezett jelszó-szinkronizálási beállítás kérése. Egyes szervezetek azonban rendelkezik egy adott oka, hogy ezek nem képesek toouse ezt a beállítást. Azok vagy egy összevont bejelentkezési beállítást használhatja, például az AD FS vagy átmenő hitelesítés. A következő tábla toohelp akkor megfelelő választás hello hello is használhatja.

Túl van szükségem| PS-alapú egyszeri| Egyszeri bejelentkezési modellel PA| ACTIVE DIRECTORY ÖSSZEVONÁSI SZOLGÁLTATÁSOK |
 --- | --- | --- | --- |
Új felhasználói, lépjen kapcsolatba, és a helyszíni Active Directory toohello felhőben automatikus szinkronizálása.|x|x|x|
A bérlő az Office 365 hibrid forgatókönyvek beállítása.|x|x|x|
A helyszíni jelszavát segítségével engedélyezheti a felhasználók toosign a és a felhőalapú szolgáltatást.|x|x|x|
Egyszeri bejelentkezés megvalósítása a vállalati hitelesítő adatok használatával.|x|x|x|
Győződjön meg arról, hogy nincs jelszava hello felhő megtalálható-e.||x *|x|
A helyi multi-factor Authentication hitelesítés megoldások engedélyezése.|||x|

* Segítségével egy lightweight összekötőt.

>[!NOTE]
> Átmenő hitelesítés van néhány korlátozást a funkciógazdag ügyfeleket. Lásd: [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md) további részleteket.

### <a name="password-synchronization"></a>Jelszó-szinkronizálás
A jelszó-szinkronizálás a helyszíni Active Directory tooAzure AD kivonatokat a felhasználói jelszavak szinkronizálódnak. Jelszavak módosulnak, és az újraindítás a helyszíni, hello új jelszóban szinkronizált tooAzure AD azonnal, hogy a felhasználók mindig használjon hello azonos felhőben található erőforrásokat és a helyszíni erőforrások jelszavát. hello jelszavak soha nem küldött tooAzure AD vagy az Azure AD-szövegként tárolt. A jelszó-szinkronizálás jelszó késleltetve visszaírt tooenable önkiszolgáló módon új jelszót az Azure AD együtt használható.

Emellett engedélyezheti [SSO](active-directory-aadconnect-sso.md) tartományhoz csatlakoztatott számítógépeken, amelyek a vállalati hálózaton hello felhasználójához. Egyszeri bejelentkezés engedélyezve van, a felhasználók csak szükség tooenter egy felhasználónév toohelp őket biztonságosan hozzáférhessenek a felhőben található erőforrásokat.

![Jelszó-szinkronizálás](./media/active-directory-aadconnect-user-signin/passwordhash.png)

További információkért lásd: hello [jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) cikk.

### <a name="pass-through-authentication"></a>Áteresztő hitelesítés
Az átmenő hitelesítés hello jelszó összevetni hello a helyszíni Active Directory-tartományvezérlő. hello jelszó toobe szerepel az Azure AD semmilyen formában nem szükséges. Ez lehetővé teszi a helyi házirendek óra bejelentkezési korlátozásokat, például toobe kiértékelése során hitelesítési toocloud szolgáltatások.

Áteresztő hitelesítés egy egyszerű ügynök hello a helyszíni környezetben a Windows Server 2012 R2 tartományhoz gép használja. Ez az ügynök a jelszó érvényesítése kéréseket figyeli. Minden bejövő portra toobe nyitott toohello Internet nem igényel.

Emellett engedélyezheti egyszeri bejelentkezéshez a felhasználók számára, amelyek a vállalati hálózaton hello tartományhoz csatlakoztatott számítógépeken. Egyszeri bejelentkezés engedélyezve van, a felhasználók csak szükség tooenter egy felhasználónév toohelp őket biztonságosan hozzáférhessenek a felhőben található erőforrásokat.
![Áteresztő hitelesítés](./media/active-directory-aadconnect-user-signin/pta.png)

További információkért lásd:
- [Áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md)
- [Egyszeri bejelentkezés](active-directory-aadconnect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Amely egy új vagy meglévő farmot használ, a Windows Server 2012 R2 AD FS összevonási
Az összevont bejelentkezés a felhasználók bejelentkezhetnek tooAzure jelszavukat a helyszíni AD-alapú szolgáltatások. Amikor nem hello a vállalati hálózaton, azok nem is kell tooenter jelszavukat. Hello összevonási beállítás az AD FS használatával telepítheti az AD FS a Windows Server 2012 R2 új vagy meglévő farmhoz. Ha toospecify egy meglévő farmra, az Azure AD Connect a farm és az Azure AD között hello megbízhatósági konfigurálja, hogy a felhasználók bejelentkezhetnek.

<center>![Összevonás az AD FS a Windows Server 2012 R2 rendszerben](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Összevonás az AD FS a Windows Server 2012 R2 telepítése

Ha telepít egy új farmot, lesz szüksége:

* A Windows Server 2012 R2 server hello összevonási kiszolgáló.
* A Windows Server 2012 R2 hello webalkalmazás-Proxy kiszolgáló.
* A .pfx-fájlját egy SSL-tanúsítványt a kívánt összevonási szolgáltatás neve. Például: fs.contoso.com.

Ha telepít egy új farmot, vagy egy meglévő farmot használ, meg kell:

* Helyi rendszergazdai hitelesítő adatokat az összevonási kiszolgálókon.
* Helyi rendszergazdai hitelesítő adatokat egyetlen (nincsenek tartományhoz csatlakoztatva) munkacsoport-kiszolgálón, hogy szeretné-e toodeploy a webalkalmazás-Proxy szerepkör hello.
* hello gépen, hogy hello varázsló futtatása toobe képes tooconnect tooany a többi gép, amelyet az AD FS vagy a webalkalmazás-Proxy tooinstall Rendszerfelügyeleti webszolgáltatások használatával.

További információkért lásd: [egyszeri bejelentkezés konfigurálása az AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Jelentkezzen be egy korábbi AD FS vagy egy harmadik féltől származó megoldás használatával
Ha már konfigurálta az felhő bejelentkezhet egy korábbi verzióját (például az AD FS 2.0) AD FS vagy egy harmadik féltől származó összevonási szolgáltató használatával, dönthet úgy tooskip felhasználói-bejelentkezés konfigurálása az Azure AD Connect használatával. Ez lehetővé teszi az tooget hello legutóbbi szinkronizálás és más képességek az Azure AD Connect továbbra is a meglévő megoldást használni a bejelentkezés során.

További információkért lásd: hello [az Azure AD külső összevonási kompatibilitási lista](active-directory-aadconnect-federation-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Felhasználói bejelentkezés és a felhasználó egyszerű felhasználóneve
### <a name="understanding-user-principal-name"></a>Understanding egyszerű felhasználónév
Az Active Directory hello alapértelmezett egyszerű felhasználónév (UPN) utótagja hello tartomány, ahol hello felhasználói fiók létrejött hello DNS-nevét. A legtöbb esetben ez a hello tartomány nevét, amely a vállalati tartományhoz hello hello Internet néven van regisztrálva. Azonban több UPN-utótagot is hozzáadhat Active Directory – tartományok és megbízhatósági kapcsolatok segítségével.

hello hello felhasználó egyszerű Felhasználónevét hello formátuma username@domain. Például "contoso.com" nevű Active Directory-tartomány, a John nevű felhasználó lehet hello UPN "john@contoso.com". hello hello felhasználó egyszerű Felhasználónevét RFC 822 alapul. Bár hello UPN és e-mailek megosztás hello ugyanazt a formátumot, egy felhasználó UPN hello hello értékének lehetnek nem hello ugyanaz, mint hello hello felhasználó e-mail címét.

### <a name="user-principal-name-in-azure-ad"></a>Az Azure AD-felhasználó egyszerű felhasználóneve
hello Azure AD Connect varázsló hello userPrincipalName attribútumot használ, vagy adja meg, hogy lehetővé teszi a helyszíni vettük hello egyszerű felhasználónév az Azure Active Directory-attribútumban (egyéni telepítés) toobe hello. Ez érték hello tooAzure AD a aláírására szolgál. Ha a userPrincipalName attribútum hello hello értéke nem felel meg tooa ellenőrzése tartományhoz az Azure ad-ben, az Azure AD felülírja az alapértelmezett. onmicrosoft.com érték.

Minden címtárat az Azure Active Directoryban egy beépített a tartománynevet, hello formátum contoso.onmicrosoft.com, Azure vagy más Microsoft-szolgáltatások használatának megkezdésében lehetővé tévő tartalmaz. Révén jelentősen javítható és hello bejelentkezés során tapasztal élmény egyszerűsítése egyéni tartományok használatával. Egyéni tartománynevek az Azure AD információt, és hogyan tooverify egy tartományhoz, [adja hozzá az egyéni tartomány nevét tooAzure Active Directory](../add-custom-domain.md#add-your-custom-domain).

## <a name="azure-ad-sign-in-configuration"></a>Azure AD-bejelentkezés konfigurálása
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Az Azure AD-bejelentkezés konfigurálása az Azure AD Connecttel
hello Azure AD bejelentkezés során tapasztal élmény attól függ, hogy felel meg az Azure AD is hello UPN-utótag olyan felhasználó hello egyéni tartományok hello Azure AD-címtárban lévő ellenőrzött szinkronizált tooone folyamatban van. Az Azure AD Connect segítséget nyújt a beállításokat az Azure AD-bejelentkezés, amíg, hogy hello felhasználói bejelentkezés során tapasztal élmény hello felhőben található hasonló toohello helyszíni.

Az Azure AD Connect listák hello hello tartományok és záma toomatch definiált UPN-utótagot őket az Azure AD-ben egyéni tartományt. Majd segít hello megfelelő művelettel, amelyet a toobe venni.
hello Azure AD bejelentkezési oldalára sorolja fel a helyszíni Active Directory megadott hello UPN-utótagot, és minden utótag elleni hello a megfelelő állapotát jeleníti meg. hello állapotértékek hello a következők egyike lehet:

| Állapot | Leírás | Beavatkozás szükséges |
|:--- |:--- |:--- |
| Ellenőrzése |Az Azure AD Connect található megfelelő ellenőrzött tartományához Azure AD-ben. A tartomány minden felhasználó a helyszíni hitelesítő adatokkal jelentkezhetnek be. |Nincs szükség beavatkozásra. |
| Nincs ellenőrizve |Az Azure AD Connect található egyező egyéni tartományt az Azure ad-ben, de nem ellenőrzi, hogy. hello egyszerű Felhasználónévi utótagot a tartomány felhasználóinak hello lesz megváltozott toohello alapértelmezett. onmicrosoft.com utótag szinkronizálás Ha hello tartomány nem ellenőrzése után. | [Ellenőrizze a hello egyéni tartományt az Azure ad-ben.](../add-custom-domain.md#verify-the-domain-name-with-azure-ad) |
| Nincs hozzáadva |Az Azure AD Connect egyéni tartományt nem található a megfeleltetett toohello UPN-utótagot. Ebben a tartományban hello felhasználói UPN-utótagot hello lesz megváltozott toohello alapértelmezett. onmicrosoft.com utótag Ha hello tartomány nem hozzáadja, és ellenőrizte az Azure-ban. | [Adja hozzá, és ellenőrizze az egyéni tartományt, amely megfelel a toohello UPN-utótagot.](../add-custom-domain.md) |

hello Azure AD bejelentkezési oldalára sorolja fel, amely a helyszíni Active Directory határozza meg, és megfelelő egyéni tartomány hello hello állapotának ellenőrzése az Azure AD-ben hello UPN-utótagot. Egyéni telepítés, most kiválaszthatja hello egyszerű felhasználónév a hello hello attribútuma **az Azure AD-bejelentkezés** lap.

![Az Azure AD bejelentkezési oldalára](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Hello frissítés gomb toore-fetch hello legfrissebb állapotának hello egyéni tartományok Azure ad-gombra.

### <a name="selecting-hello-attribute-for-hello-user-principal-name-in-azure-ad"></a>Az Azure AD hello egyszerű felhasználónév hello attribútum kiválasztása
hello attribútum userPrincipalName felhasználók használni, amikor bejelentkeznek az tooAzure AD és az Office 365 hello attribútum. Hello tartományok (más néven UPN-utótagot), mielőtt hello felhasználók szinkronizált Azure AD-ben használt ellenőrizze.

Határozottan javasoljuk, hogy maradjon hello alapértelmezett attribútum userPrincipalName. Ha ez az attribútum útválasztást, és nem lehet ellenőrizni, akkor célszerű lehetséges tooselect egy másik attribútum (például e-mail), amely tárolja hello attribútumaként hello bejelentkezési azonosítót. Ez az úgynevezett hello másik azonosítót. hello másik azonosító értékének hello RFC 822 szabványos kell követnie. Egyszeri jelszó és a összevonási SSO hello bejelentkezési megoldásként használható a másik azonosító.

> [!NOTE]
> A másik azonosító használata nem felel meg az összes Office 365 számítási feladattal. További információkért lásd: [másodlagos bejelentkezési azonosító beállítása](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-hello-azure-sign-in-experience"></a>Egyéni tartomány különböző állapotok és azok hatása az Azure-bejelentkezés hello élmény
Nagyon fontos toounderstand hello kapcsolat az Azure AD-címtár hello egyéni tartomány állapotok közötti és hello UPN-utótagot, amelyek meghatározott helyszíni. Hello különböző módokon lehetséges az Azure bejelentkezési pedig ugorjunk, ha meg van-szinkronizálás beállítása az Azure AD Connect használatával.

A következő információ hello, tegyük fel, hogy a rendszer hello helyszíni címtárban UPN--részeként például használatos hello egyszerű Felhasználónévi utótagot contoso.com érintett user@contoso.com.

###### <a name="express-settingspassword-synchronization"></a>Express beállítások/jelszó-szinkronizálás
| Állapot | Az Azure-bejelentkezés a felhasználói élmény hatása |
|:---:|:--- |
| Nincs hozzáadva |Ebben az esetben nem contoso.com tartozó egyéni tartomány hello Azure AD-címtár lett hozzáadva. Egyszerű felhasználónév a helyszíni hello utótaggal rendelkező felhasználók @contoso.com nem fogja tudni toouse a helyszíni egyszerű Felhasználónévvel toosign a tooAzure. Azok helyette kell toouse egy új egyszerű felhasználónév, amely toothem megadta a Azure ad hello alapértelmezett Azure AD-címtár hello utótagja hozzáadásával. Például, ha a felhasználók toohello az Azure Active directory azurecontoso.onmicrosoft.com szinkronizálás majd hello a helyszíni felhasználói user@contoso.com kap egy egyszerű user@azurecontoso.onmicrosoft.com. |
| Nincs ellenőrizve |Ebben az esetben van egy egyéni tartománynév contoso.com, amely fel van véve hello Azure Active directory. Azonban ez még nincs ellenőrizve. Ha azokat, amelyek a felhasználók szinkronizálása hello tartomány ellenőrzése nélkül, majd hello felhasználók hozzá fog rendelni egy új UPN az Azure AD, ugyanúgy, mint a "Nem hozzáadott" forgatókönyv hello. |
| Ellenőrzése |Ebben az esetben van egy egyéni tartománynév contoso.com már hozzáadott és az Azure ad-val hello ellenőrzött UPN-utótagot. Felhasználók fognak tudni toouse a helyszíni egyszerű felhasználónevéhez, például user@contoso.com, tooAzure után azok szinkronizálva van a toosign tooAzure AD. |

###### <a name="ad-fs-federation"></a>AD FS összevonási
Nem hozható létre egy összevonási hello alapértelmezett. onmicrosoft.com tartományt az Azure AD vagy az Azure ad-ben nem ellenőrzött egyéni tartományt. Amikor futtatja hello Azure AD Connect varázsló, ha egy nem ellenőrzött tartományok toocreate az összevonási, majd meg a szükséges hello létrehozni, ahol a DNS-kiszolgáló tárolása hello tartomány toobe rögzíti az Azure AD Connect megjelenő utasításokat. További információkért lásd: [összevonási kiválasztott hello Azure AD-tartomány hitelesítése](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Hello felhasználói bejelentkezési lehetőséget választva **az AD FS összevonási**, akkor telepíteni kell egy egyéni tartomány toocontinue összevonási létrehozása az Azure ad-ben. A leírását ez azt jelenti, hogy kell-e azt egy egyéni tartomány contoso.com hello Azure Active directory szerepel.

| Állapot | Hello Azure bejelentkezés felhasználói élményt hatása |
|:---:|:--- |
| Nincs hozzáadva |Az Azure AD Connect ebben az esetben hello egyszerű Felhasználónévi utótagot contoso.com hello Azure AD-címtár nem található egyező egyéni tartományt. Ha a felhasználók toosign kell az AD FS segítségével, a helyszíni egyszerű Felhasználónévvel tooadd egy egyéni tartománynév contoso.com szüksége (például user@contoso.com). |
| Nincs ellenőrizve |Ebben az esetben az Azure AD Connect rákérdez, hogyan ellenőrizheti a tartomány egy későbbi időpontban a szükséges adatokat. |
| Ellenőrzése |Ebben az esetben megkezdheti hello konfigurációjával kapcsolatban további műveletek nélkül. |

## <a name="changing-hello-user-sign-in-method"></a>Hello felhasználói bejelentkezési módszer módosítása
Hello felhasználói bejelentkezési módszer módosíthatja a összevonási, a jelszó-szinkronizálás vagy az átmenő hitelesítés után hello kezdeti konfigurálása az Azure AD Connect hello varázsló az Azure AD Connectben elérhető hello feladatok használatával. Futtassa újra a hello Azure AD Connect varázsló, és láthatja, hogy elvégezhető feladatok listáját. Válassza ki **felhasználói bejelentkezés módosítása** hello feladatok listája.

![Felhasználói bejelentkezés módosítása](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

A következő lapon hello megkérdezi tooprovide hello hitelesítő adatokat az Azure AD.

![TooAzure AD Connect](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

A hello **felhasználói bejelentkezés** lapon, válassza ki a kívánt hello felhasználói bejelentkezés.

![TooAzure AD Connect](./media/active-directory-aadconnect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Ha csak hajt végre egy ideiglenes kapcsoló toopassword szinkronizálást, majd válassza ki hello **felhasználói fiókok nem konvertálhatók** jelölőnégyzetet. Minden felhasználó toofederated nem ellenőrzése hello beállítás konvertálja, és több órát is igénybe vehet.
>
>

## <a name="next-steps"></a>Következő lépések
- További információ [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
- További információ [az Azure AD Connect tervezési alapelvei](active-directory-aadconnect-design-concepts.md).
