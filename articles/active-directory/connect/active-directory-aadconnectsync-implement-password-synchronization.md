---
title: "és az Azure AD Connect-szinkronizálás jelszó-szinkronizálás aaaImplement |} Microsoft Docs"
description: "Információt nyújt a jelszó-szinkronizálás működése, és hogyan tooset fel."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: a0401640f2a4d914419ee4446f923bb3a972389d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása
A cikkben szükséges információk toosynchronize a felhasználói jelszavakat, egy a helyszíni Active Directory példány tooa felhőalapú Azure Active Directory (Azure AD) példányból.

## <a name="what-is-password-synchronization"></a>Mi az a jelszó-szinkronizálás
hello valószínűség arról, hogy van tiltva a munka tooa elfelejtett jelszó miatt a kapcsolódó toohello számát, különböző jelszavakat tooremember kell. hello tooremember, hello magasabb hello valószínűség tooforget egy van szüksége további jelszavakat. Kérdések és a jelszó alaphelyzetbe állítását és egyéb jelszó-kapcsolatos problémák igény szerinti hívások hello segélyszolgálat legtöbb erőforrást.

A jelszó-szinkronizálás, a szolgáltatás használható toosynchronize felhasználói jelszavak egy a helyszíni Active Directory példány tooa felhőalapú Azure AD példányával.
Ez a szolgáltatás toosign tooAzure AD szolgáltatások, például az Office 365, a Microsoft Intune, CRM Online-hoz, és Azure Active Directory tartományi szolgáltatások (az Azure AD DS) használja. Bejelentkezik toohello szolgáltatás hello segítségével tooyour toosign használja ugyanazt a jelszót a helyi Active Directory-példányban.

![Mi az az Azure AD Connect?](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Jelszavak hello számának csökkentésével a felhasználóknak kell toomaintain toojust egyet. A jelszó-szinkronizálás nyújt segítséget:

* A felhasználók hello termelékenység növelése.
* Csökkenti az ügyfélszolgálati kiadásokat.  

Is ha úgy dönt, hogy toouse [az Active Directory összevonási szolgáltatások (AD FS) összevonásának](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), opcionálisan állíthatja be a jelszó-szinkronizálás biztonsági abban az esetben, ha az AD FS infrastruktúra sikertelen lesz.

A jelszó-szinkronizálás az bővítmény toohello directory szinkronizálási szolgáltatása: az Azure AD Connect szinkronizálási szolgáltatás által megvalósított. toouse jelszó-szinkronizálás környezetében kell:

* Azure AD Connect telepítése.  
* Konfigurálja a címtár-szinkronizálás a helyszíni Active Directory példány és az Azure Active Directory-példány között.
* Jelszó-szinkronizálás engedélyezése.

További részletekért lásd: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

> [!NOTE]
> Azure Active Directory tartományi szolgáltatások FIPS és a jelszó-szinkronizálás beállítása kapcsolatos további tudnivalókért lásd: a "Jelszó-szinkronizálás és FIPS" a cikk későbbi részében.
>
>

## <a name="how-password-synchronization-works"></a>Jelszó-szinkronizálás használata
Active Directory tartományi szolgáltatások hello jelszavak, a kivonatoló érték megjelenítése hello tényleges felhasználói jelszó hello formában tárolja. A kivonat értéke egy egyirányú matematikai függvény eredményeként (hello *kivonatoló algoritmus*). Nincs a jelszó egyirányú függvény toohello egyszerű szöveges verziójának metódus toorevert hello eredmény. A jelszó kivonatát toosign tooyour a helyi hálózaton nem használható.

a jelszót, az Azure AD Connect szinkronizálási szolgáltatás kibontja a jelszó-kivonat toosynchronize hello a helyszíni Active Directory-példányban. További biztonsági feldolgozási alkalmazott toohello Jelszókivonat ahhoz, hogy szinkronizált toohello Azure Active Directory hitelesítési szolgáltatás. Jelszavak szinkronizálódnak, felhasználónkénti alapon és időrendben sorolja fel.

tényleges adatfolyama hello hello jelszó-szinkronizálási folyamat a felhasználói adatok, például DisplayName vagy E-mail címekre hasonló toohello szinkronizálását. Azonban a jelszavak szinkronizálódnak gyakrabban hello szabványos directory szinkronizálási időszak egyéb attribútumai. jelszó-szinkronizálási folyamat hello 2 percenként fut. Ez a folyamat gyakoriságának hello nem módosítható. A jelszó szinkronizálása során hello meglévő felhőalapú jelszó felülírja.

hello első alkalommal engedélyezi a hello jelszó-szinkronizálási szolgáltatás, akkor végrehajt egy kezdeti szinkronizálást hello jelszavak az összes hatókör felhasználót. Felhasználói jelszavak, amelyet az toosynchronize egy részét nem definiálhat explicit módon.

A helyszíni jelszó módosítása frissített hello jelszó legyenek szinkronizálva, általában a percek.
jelszó-szinkronizálási szolgáltatás hello automatikusan újrapróbálkozik a sikertelen szinkronizálás kísérletek. Ha a hiba akkor fordul elő, egy kísérlet toosynchronize során egy jelszót, hiba történt az Eseménynapló van bejelentkezve.

hello szinkronizálási jelszó ne legyen hatással hello aktuálisan bejelentkezett felhasználó rendelkezik.
A felhőalapú szolgáltatás jelenlegi munkamenet azonnal nem érinti a szinkronizált jelszómódosítás, amely akkor fordul elő, amikor be van jelentkezve tooa felhőszolgáltatásban. Azonban ha hello felhőszolgáltatás igényel tooauthenticate újra, meg kell tooprovide az új jelszót.

A felhasználó egy második idő tooauthenticate tooAzure AD, függetlenül attól, hogy azok van bejelentkezve tootheir vállalati hálózaton kell adnia a vállalati hitelesítő adataikkal. A minta minimalizálható, azonban ha hello felhasználó hello megtartása me aláírt választ jel (KMSI) jelölőnégyzetet. Ez a beállítás egy rövid ideig hitelesítési megkerüli munkamenetcookie-t állítja be. KMSI viselkedés engedélyezhető vagy hello Azure AD-rendszergazda letiltotta.

> [!NOTE]
> A jelszó-szinkronizálás csak támogatott hello objektum típusa felhasználóhoz az Active Directoryban. Hello iNetOrgPerson objektumtípus nem támogatott.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Jelszó-szinkronizálás részletes leírása
hello következő részletes jelszó-szinkronizálás működését ismerteti az Active Directory és az Azure AD között.

![A jelszó részletes folyamata](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Két percenként hello jelszó-szinkronizálási ügynök hello AD Connect-kiszolgáló a kérelmek tárolt jelszókivonatainak (hello unicodePwd attribútum) keresztül szabványos hello tartományvezérlőt [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) replikációs protokollt használja toosynchronize adatok közötti tartományvezérlők. hello szolgáltatásfiók Directory változások replikálása és replikálja könyvtár az összes AD jogosultságaitól (alapértelmezés szerint a telepítési) tooobtain hello jelszókivonatait kell rendelkeznie.
2. Küldés előtt, hello DC hello MD4 Jelszókivonat titkosítja a kulccsal, amely egy [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) hello RPC bejelentkezésimunkamenet-kulcsot és az egy kivonatát. RPC-n keresztül küldi hello eredmény toohello jelszó-szinkronizálási ügynök majd. hello DC is ad hello védőérték toohello szinkronizálási ügynök protokollal hello DC replikációs, ezért a hello ügynök során képes toodecrypt hello boríték.
3.  Miután hello jelszó-szinkronizálási ügynök hello titkosított boríték rendelkezik, használja [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) és védőérték toogenerate kulcs toodecrypt kapott hello hátsó tooits eredeti MD4 adatformátum hello. Hello jelszó-szinkronizálási ügynök nem rendelkezik hozzáféréssel toohello tiszta szöveges jelszavak. hello jelszó-szinkronizálási ügynök által használt MD5 használatát szigorúan hello DC-replikáció protokoll való kompatibilitás, és csak a helyszíni hello tartományvezérlő és a jelszó-szinkronizálási ügynök hello között használja.
4.  jelszó-szinkronizálási ügynök hello majd UTF-16 kódolással bináris formátumra konvertálni a karakterláncot a biztonsági bővíti első formátumba való átalakítása hello kivonatoló tooa 32 bájtos hexadecimális karakterlánc, hello 16 bájtos bináris jelszó kivonatát too64 bájt.
5.  jelszó-szinkronizálási ügynök hello hozzáadja a védőérték egy 10 bájtos hossza védőérték álló, toohello 64 bájtos bináris toofurther védelme hello eredeti kivonatát.
6.  jelszó-szinkronizálási ügynök hello majd hello MD4 kivonatoló plus védőérték egyesíti, és be hello a bemeneti [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) függvény. hello 1000 ismétlésének [HMAC-SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) létre kivonatoló algoritmust használnak. 
7.  hello jelszó-szinkronizálási ügynök hello eredményül kapott 32 bájtos kivonatoló vesz igénybe, mindkét hello védőérték összefűzi és hello SHA256 ismétlési tooit (az Azure ad használatára) száma, majd hello karakterlánc az Azure AD Connect tooAzure AD SSL-en keresztül továbbítja.</br> 
8.  Amikor egy felhasználó megpróbál toosign a tooAzure AD, és beírja a jelszavát, hello jelszó hello végigfuttatása azonos MD4 + védőérték + PBKDF2 + HMAC-SHA256 folyamat. Ha hello eredményül kapott kivonatoló megegyezik az Azure ad-ben tárolt hello kivonat, a hello felhasználói lépett hello helyes jelszót, és hitelesítése. 

>[!Note] 
>hello eredeti MD4 kivonatoló nincs átvitt tooAzure AD. Ehelyett átkerülnek a hello hello eredeti MD4 kivonatoló SHA256-kivonata. Ennek eredményeképpen az Azure ad-ben tárolt hello kivonatoló kapjuk meg, ha azt nem használható az helyszíni a pass-the-hash támadások.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Jelszó-szinkronizálás az Azure Active Directory tartományi szolgáltatások használata
Is használhatja hello jelszó-szinkronizálás szolgáltatás toosynchronize a helyszíni jelszavak túl[Azure Active Directory tartományi szolgáltatások](../../active-directory-domain-services/active-directory-ds-overview.md). Ebben a forgatókönyvben hello Azure Active Directory tartományi szolgáltatások példány hitelesíti a felhasználók hello felhő minden hello módszer áll rendelkezésre, a helyszíni Active Directory-példányban. Ebben a forgatókönyvben hello élménye hasonló toousing hello Active Directory áttelepítési eszköz (ADMT) helyszíni környezetben.

### <a name="security-considerations"></a>Biztonsági szempontok
Ha a jelszó-szinkronizálás, hello egyszerű szöveges a jelszó verziója nem jelszó-szinkronizálási szolgáltatás elérhetőségi toohello, tooAzure AD vagy hello társított szolgáltatások.

Felhasználóhitelesítés akkor történik meg az Azure AD ellen, nem pedig elleni hello szervezete saját Active Directory-példányban. Ha a szervezete helyszíni hello, javasoljuk, hogy a hello tényt, hogy SHA-256 jelszó adataihoz az Azure ad-ben – hello elhagyása űrlapok jelszó adatai kétségei a hello eredeti MD4 kivonatoló--kivonata jóval biztonságosabb Mi az Active Directoryban tárolja. További az SHA256-kivonata nem fejthető vissza, mert az nem lehet teszik vissza toohello szervezet Active Directory-környezet és mutatják be, a pass-the-hash támadások érvényes felhasználói jelszó.





### <a name="password-policy-considerations"></a>Jelszó házirenddel kapcsolatos megfontolások
A jelszó-szinkronizálás engedélyezése által érintett jelszóházirendek két típusa van:

* Jelszó bonyolultságára vonatkozó házirendet
* Jelszó-elévülési szabályzatának

#### <a name="password-complexity-policy"></a>Jelszó bonyolultságára vonatkozó házirendet  
Ha a jelszó-szinkronizálás engedélyezve van, hello jelszó összetettségét házirendek a helyszíni Active Directory-példányban összetettsége házirendek segítségével a szinkronizált felhasználók hello felhő bírálja felül. A helyszíni Active Directory példány tooaccess Azure AD szolgáltatásokba minden hello érvényes jelszavak használhatja.

> [!NOTE]
> A felhasználók közvetlenül a hello felhő létrehozott jelszavakat még tulajdonos toopassword házirendek a hello felhő.

#### <a name="password-expiration-policy"></a>Jelszó-elévülési szabályzatának  
Ha egy felhasználó a jelszó-szinkronizálás hello hatókörében, hello felhő fiók jelszava túl van-e állítva*nem jár le*.

Toosign elérhető tooyour felhőszolgáltatások kattintva folytathatja a helyszíni környezetben lejárt szinkronizált jelszavakat. A felhő jelszó frissül hello legközelebb hello jelszó hello a helyszíni környezet módosítása.

#### <a name="account-expiration"></a>Fiók lejárati
Ha a szervezet hello accountExpires attribútum felhasználóifiók-kezelés részeként használ, vegye figyelembe, hogy ez az attribútum nincs szinkronizált tooAzure AD. Emiatt egy olyan környezetben, a jelszó-szinkronizálás beállítása lejárt Active Directory-fiókkal marad aktív az Azure ad-ben. Azt javasoljuk, hogy ha hello fiók lejárt, a munkafolyamat-művelet indíthat el egy PowerShell-parancsfájlt, amely letiltja a hello felhasználói Azure AD-fiókot. Ezzel szemben ha hello fiók be van kapcsolva, hello Azure AD-példányhoz be kell kapcsolni.

### <a name="overwrite-synchronized-passwords"></a>Szinkronizált jelszavakat felülírása
A rendszergazda Windows PowerShell használatával manuálisan új jelszó.

Ebben az esetben hello új jelszó felülbírálja a szinkronizált jelszavát, és hello felhő definiált összes jelszóházirendet alkalmazott toohello új jelszót.

Ha újra módosítja a helyszíni jelszavát, hello új jelszó van-e toohello felhő szinkronizálja, és felülírja a manuálisan frissített hello jelszót.

hello szinkronizálási jelszó ne legyen hatással hello Azure van bejelentkezett felhasználó rendelkezik. A felhőalapú szolgáltatás jelenlegi munkamenet azonnal nem érinti a szinkronizált jelszómódosítás, amely akkor fordul elő, miközben be van jelentkezve tooa felhőalapú szolgáltatás. KMSI terjeszti ki ezt a különbséget hello időtartama. Hello felhőszolgáltatás igényel tooauthenticate újra, úgy kell tooprovide az új jelszót.

### <a name="additional-advantages"></a>További előnyei

- A jelszó-szinkronizálás általában egyszerűbb, mint egy összevonási szolgáltatás tooimplement. Nincs szükség semmilyen további kiszolgálókat, és megszünteti a magas rendelkezésre állású összevonási szolgáltatás tooauthenticate felhasználók függés. 
- A jelszó-szinkronizálás hozzáadása toofederation is engedélyezhető. Azt is használható tartalékként Ha az összevonási szolgáltatás kimaradás.












## <a name="enable-password-synchronization"></a>Jelszó-szinkronizálás engedélyezése
Ha telepíti az Azure AD Connect hello segítségével **Gyorsbeállítások** , jelszó-szinkronizálás automatikusan engedélyezve van. További részletekért lásd: [Ismerkedés az Azure AD Connect a gyorsbeállításokkal](active-directory-aadconnect-get-started-express.md).

Ha az Azure AD Connect telepítése egyéni beállításokat használja, ha hello felhasználói bejelentkezési oldala a jelszó-szinkronizálás érhető el. További részletekért lásd: [az Azure AD Connect egyéni telepítési](active-directory-aadconnect-get-started-custom.md).

![A jelszó-szinkronizálás engedélyezése](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>A jelszó-szinkronizálás és a FIPS
Ha a kiszolgáló zárolva lett tooFederal Information Processing Standard (FIPS) alapján történik, MD5 is le van tiltva.

**a jelszó-szinkronizáláshoz MD5 tooenable hajtsa végre a lépéseket követve hello:**

1. Nyissa meg az AD too%programfiles%\Azure Sync\Bin.
2. Nyissa meg a miiserver.exe.config.
3. Nyissa meg toohello konfigurációs/runtime csomópontot hello fájl hello végén.
4. Adja hozzá a következő csomópont hello:`<enforceFIPSPolicy enabled="false"/>`
5. Mentse a módosításokat.

Összehasonlításul a részlet, mi azt kell hasonlítania:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

További információ a biztonsági és FIPS: [AAD jelszó-szinkronizálás, a titkosítás és a FIPS megfelelőségi](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-synchronization"></a>Jelszó-szinkronizálás hibaelhárítása
Ha problémába ütközik a jelszó-szinkronizálás, lásd: [jelszó-szinkronizálás hibaelhárítása](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Következő lépések
* [Azure AD Connect szinkronizálása: szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
