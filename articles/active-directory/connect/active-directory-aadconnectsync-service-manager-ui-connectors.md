---
title: "az Azure AD Synchronization Service Manager felhasználói felületén hello aaaConnectors |} Microsoft Docs"
description: "Ismerje meg, az Azure AD Connect hello összekötők lapján hello Synchronization Service Managert."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0969630313178b1e299385b1289360c8f787cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-with-hello-azure-ad-connect-sync-service-manager"></a>Összekötők használata az Azure AD Connect szinkronizálási szolgáltatás Manager hello

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

hello összekötők lapon használt toomanage összes rendszerek hello szinkronizálási motor van csatlakoztatva.

## <a name="connector-actions"></a>Összekötő műveletek
| Műveletek | Megjegyzés |
| --- | --- |
| Létrehozás |Ne használjon. Csatlakozás tooadditional AD erdők, hello telepítés varázsló használatával. |
| Tulajdonságok |Tartomány és szervezeti egységek szűrése használható. |
| [Törlés](#delete) |Használt tooeither hello összekötő szóköz vagy toodelete kapcsolat tooa erdő hello adatok törlése. |
| [Futtatási profilok konfigurálása](#configure-run-profiles) |Szűrés, semmi nem tartományi kivételével tooconfigure itt. Ez a művelet is használhatja a futtatási profil toosee már be van állítva. |
| Futtassa a következőt: |Egy ad hoc futtassa egy profil toostart használt. |
| Leállítás |Jelenleg fut egy profil összekötő leáll. |
| Összekötő exportálása |Ne használjon. |
| Összekötő importálása |Ne használjon. |
| Frissítés összekötő |Ne használjon. |
| Séma frissítése |Hello gyorsítótárazott séma frissítése. Azt a beállítás előnyben részesített toouse hello hello telepítővarázslóban ehelyett óta, amely is frissítések szabályok szinkronizálása. |
| [Összekötőtér keresése](#search-connector-space) |Felhasznált toofind objektum és túl[hajtsa végre az objektum és az adatok hello rendszeren keresztül](#follow-an-object-and-its-data-through-the-system). |

### <a name="delete"></a>Törlés
két különböző fogalom hello delete művelet használható.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

a beállítás hello **törlése csak a kapcsolódási térbe** eltávolítható az összes adat, de tartsa hello konfigurációja.

beállítás hello **összekötő törlése és a connector** hello adatot és konfigurációs hello eltávolítja. Ez a beállítás akkor használatos, ha a nem kívánt tooconnect tooa erdő többé.

Mindkét lehetőség minden objektumokat szinkronizálni, és frissítése hello metaverzum-objektum. Ez a művelet hosszú ideig futó művelet.

### <a name="configure-run-profiles"></a>Futtatási profilok konfigurálása
Ez a beállítás lehetővé teszi toosee hello futtatási profil összekötő konfigurálva.

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Összekötőtér keresése
hello keresési összekötő terület művelet hasznos toofind objektumok és adatok problémák elhárításához.

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Kiválasztásával indítsa el a **hatókör**. Kereshet az adatok alapján (RDN, DN, rögzítési, részfájának), vagy állami hello objektum (minden más beállítás).  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Ha például egy részfájának keresést, egy szervezeti egység összes objektum beolvasása.  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)  
A rácsban jelöljön ki egy objektumot, válassza ki **tulajdonságok**, és [követve](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) hello adatforrás kapcsolódási térbe, keresztül hello metaverse és toohello cél kapcsolódási térbe.

### <a name="changing-hello-ad-ds-account-password"></a>Hello AD DS fiók jelszavának módosítása
Hello fiók jelszavának módosításakor hello Synchronization Service már nem lesz képes tooimport/export módosítások tooon helyszíni AD.   Hello következő jelenhetnek meg:

- hello importálási/exportálási lépést az AD-összekötő hello "no-start-hitelesítő adatok" hibaüzenettel meghiúsul.
- A Windows Eseménynapló hello alkalmazások eseménynaplójában keresse meg hibát tartalmaz Event ID 6000 és az üzenet "hello felügyeleti ügynök a"contoso.com"nem sikerült toorun mert hello hitelesítő adatok érvénytelenek voltak."

tooresolve hello ki, a frissítés hello Active Directory tartományi szolgáltatások felhasználói fiók hello alábbi:


1. Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás) hello.
</br>![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)
2. Nyissa meg toohello **összekötők** fülre.
3. Válassza ki a hello AD-összekötő, amely konfigurált toouse hello AD DS-fiókjához.
4. A műveletek, válassza ki a **tulajdonságok**.
5. Hello előugró párbeszédpanelen válassza ki a Connect tooActive Directory-erdő:
6. hello erdő neve jelzi hello megfelelő helyszíni AD.
7. hello felhasználónév azt jelzi, hogy a szinkronizáláshoz használt hello AD DS-fiókjához.
8. Adjon meg új jelszót hello hello AD DS-fiókjához hello jelszó szövegmezőjét ![az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key6.png)
9. Kattintson az OK toosave hello új jelszót, és indítsa újra a hello szinkronizálási szolgáltatás tooremove hello régi jelszó memória-gyorsítótárból.



## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
