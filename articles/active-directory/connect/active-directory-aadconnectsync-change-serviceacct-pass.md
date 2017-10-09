---
title: "Azure AD Connect szinkronizálása: hello Azure AD Connect szinkronizáláshoz használt szolgáltatásfióknak módosítása |} Microsoft Docs"
description: "Ez a témakör a dokumentum ismerteti a hello titkosítási kulcsot, és hogyan tooabandon után hello jelszó megváltozott."
services: active-directory
keywords: "Azure AD sync szolgáltatás-fiók, jelszó"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 11948ac4662f722e4f684ef6c9b9ccdc6387e60f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-azure-ad-connect-sync-service-account-password"></a>Hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavának módosítása
Ha hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavának módosításához hello szinkronizálási szolgáltatás nem lesz képes start megfelelően elhagyott hello titkosítási kulcs, és újra lesz inicializálva a hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavát. 

Az Azure AD Connect szinkronizálási szolgáltatások hello részeként egy titkosítási kulcs toostore hello jelszavakat hello Active Directory tartományi szolgáltatások és az Azure AD szolgáltatásfiókok használ.  Ezeket a fiókokat ahhoz, azok hello adatbázis tárolja titkosított. 

hello használt titkosítási kulcs használatával lett biztonságossá téve [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx). DPAPI védi hello titkosítási kulcsát hello **hello Azure AD Connect szinkronizálási szolgáltatás fiókja jelszavát**. 

Ha toochange hello szolgáltatás fiók jelszavát kell eljárásokkal hello a [Abandoning hello Azure AD Connect Sync titkosítási kulcs](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish ez.  Ezekkel az eljárásokkal kell is használható, ha bármilyen okból tooabandon hello titkosítási kulcs van szüksége.

##<a name="issues-that-arise-from-changing-hello-password"></a>Problémák merülnek fel a hello jelszó módosítása
Két dolgot toobe végre, ha hello szolgáltatás fiók jelszavát módosítani kell.

Először toochange hello jelszót adott-e a Windows szolgáltatásvezérlő hello.  A probléma megoldásáig jelenik meg a következő hibák:


- Ha toostart hello szinkronizálási szolgáltatás a Windows szolgáltatásvezérlő, hibaüzenet hello "**Windows nem tudta elindítani a hello Microsoft Azure AD Sync szolgáltatást a helyi számítógépen**". **1069. hiba: hello szolgáltatás nem indult el tooa bejelentkezési hiba miatt.** "
- A Windows Eseménynapló hello rendszer eseménynaplójában található hiba **Event ID 7038** és az üzenet "**hello ADSync szolgáltatás időpontja nem lehet toolog, mint a jelenleg konfigurált hello jelszó miatt toohello alábbi hiba: hello felhasználónév vagy jelszó érvénytelen.** "

Második meghatározott feltételek mellett hello jelszó frissül, ha hello Synchronization Service már nem lekérheti hello titkosítási kulcsa a DPAPI-t keresztül. Hello titkosítási kulcs nélkül hello szinkronizálási szolgáltatás nem tudja visszafejteni a hello jelszavak szükséges toosynchronize és a helyszíni AD és az Azure AD.
Hibák például jelenik meg:

- A Windows szolgáltatásvezérlő, próbál toostart hello szinkronizálási szolgáltatás, és nem tudja lekérni hello titkosítási kulcs, ha sikertelen a következő hiba "** Windows nem tudta elindítani a Microsoft Azure AD Sync hello a helyi számítógépen. További információkért tekintse át a hello rendszer-eseménynaplóban találhatók. Ha ez egy nem Microsoft-szolgáltatást, lépjen kapcsolatba a hello szolgáltatás szállítójával, és tekintse meg a tooservice-specifikus hibakód **-21451857952 x. "
- A Windows Eseménynapló hello alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6028** és hibaüzenet *"**hello kiszolgáló titkosítási kulcs nem érhető el.* *"*

tooensure, hogy nem jelenik meg ezeket a hibákat a hello eljárások [Abandoning hello Azure AD Connect Sync titkosítási kulcs](#abandoning-the-azure-ad-connect-sync-encryption-key) hello jelszó módosításakor.
 
## <a name="abandoning-hello-azure-ad-connect-sync-encryption-key"></a>Hello Azure AD Connect szinkronizálási szolgáltatás titkosítási kulcs megszakítása
>[!IMPORTANT]
>hello alábbi eljárások csak akkor jutnak érvényre tooAzure AD Connect build 1.1.443.0 vagy annál régebbi.

A következő eljárások tooabandon hello titkosítási kulcs hello használata.

### <a name="what-toodo-if-you-need-tooabandon-hello-encryption-key"></a>Milyen toodo, ha szüksége tooabandon hello titkosítási kulcs

Ha tooabandon hello titkosítási kulcs van szüksége, használja a következő eljárások tooaccomplish hello ezt.

1. [Szakítsa hello meglévő titkosítási kulcs](#abandon-the-existing-encryption-key)

2. [Adja meg az AD DS-fiókjához hello hello jelszavát](#provide-the-password-of-the-ad-ds-account)

3. [Hello Azure AD sync fiók jelszavát hello újrainicializálása](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [Indítsa el a Synchronization Service hello](#start-the-synchronization-service)

#### <a name="abandon-hello-existing-encryption-key"></a>Szakítsa hello meglévő titkosítási kulcs
Szakítsa hello meglévő titkosítási kulcsot, így az új titkosítási kulcs is létrehozható:

1. Bejelentkezés tooyour az Azure AD Connect kiszolgáló rendszergazdaként.

2. Indítson el egy új PowerShell-munkamenetet.

3. Keresse meg a toofolder:`$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Hello parancsot:`./miiskmu.exe /a`

![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-hello-password-of-hello-ad-ds-account"></a>Adja meg az AD DS-fiókjához hello hello jelszavát
Hello hello adatbázis tárolt meglévő jelszavakat nem lehet visszafejteni, ahogyan kell tooprovide hello szinkronizálási szolgáltatás hello jelszóval hello Tartományi fiók. Szinkronizálási szolgáltatás hello hello jelszavak hello új titkosítási kulcs használatával titkosítja:

1. Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás) hello.
</br>![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  
2. Nyissa meg toohello **összekötők** fülre.
3. Jelölje be hello **Címtárösszekötőben** , amely megfelel a tooyour a helyszíni AD. Ha több AD-összekötő, ismételje meg az egyes azokat a lépéseket követve hello.
4. A **műveletek**, jelölje be **tulajdonságok**.
5. Hello előugró párbeszédpanelen válassza ki a **tooActive Directory-erdő csatlakozás**:
6. Adjon meg Active Directory tartományi szolgáltatások hello fiók jelszavát hello hello **jelszó** szövegmező. Ha nem ismeri a jelszavát, be kell állítani a lépés végrehajtása előtt érték ismert tooa.
7. Kattintson a **OK** toosave hello új jelszót és Bezárás hello előugró párbeszédpanelen.
![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key6.png)

#### <a name="reinitialize-hello-password-of-hello-azure-ad-sync-account"></a>Hello Azure AD sync fiók jelszavát hello újrainicializálása
Közvetlenül a hello Azure AD szolgáltatás fiók toohello szinkronizálási szolgáltatás hello jelszavát nem alkalmas. Ehelyett szüksége toouse hello parancsmag **Add-ADSyncAADServiceAccount** tooreinitialize hello Azure AD-szolgáltatásfiók. hello parancsmag hello fiók jelszavának alaphelyzetbe állítása, és lehetővé teszi az elérhető toohello szinkronizálási szolgáltatás:

1. Indítsa el egy új PowerShell-munkamenetet a hello Azure AD Connect-kiszolgáló.
2. Parancsmag futtatásához `Add-ADSyncAADServiceAccount`.
3. Hello előugró párbeszédpanelen adja meg az Azure AD-bérlő hello Azure AD globális rendszergazda hitelesítő adataival.
![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key7.png)
4. Ha sikeres, megjelenik a hello PowerShell-parancssort.

#### <a name="start-hello-synchronization-service"></a>Indítsa el a Synchronization Service hello
Most, hogy a szinkronizálási szolgáltatás hello hozzáférés toohello titkosítási kulccsal rendelkezik, ezért minden hello jelszavak kell, a Windows szolgáltatásvezérlő hello újraindíthatja hello szolgáltatást:


1. Nyissa meg a szolgáltatásvezérlő kezelőjéhez (KEZDŐ → szolgáltatások) tooWindows.
2. Válassza ki **Microsoft Azure AD Sync** és levő Újraindítás lehetőségre.

## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)

* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
