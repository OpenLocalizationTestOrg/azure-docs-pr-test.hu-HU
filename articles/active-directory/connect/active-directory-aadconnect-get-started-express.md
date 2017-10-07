---
title: "Azure AD Connect: Első lépések a gyorsbeállításokkal | Microsoft Docs"
description: "Ismerje meg, hogyan toodownload, telepítése és az Azure AD Connect hello telepítővarázsló futtatásához."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: curtand
ms.assetid: b6ce45fd-554d-4f4d-95d1-47996d561c9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 79f796fa7738b85e9236e856bddb529379f60390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Első lépések az Azure AD Connecttel a gyorsbeállítások használatával
Az Azure AD Connect **Express Settings** (Gyorsbeállítások) akkor használható, ha egyerdős topológiával rendelkezik, és a hitelesítéshez [jelszó-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) alkalmaz. **Gyorsbeállítások** hello alapértelmezett beállítás, és a leggyakrabban telepített hello forgatókönyvben használható. Ön csak pár rövid kattintásnyira kötelező tooextend vannak a helyszíni címtár toohello felhő.

Az Azure AD Connect telepítése előtt győződjön meg arról, hogy túl[töltse le az Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) és teljes hello előzetesen szükséges lépések [az Azure AD Connect: hardver és előfeltételek](active-directory-aadconnect-prerequisites.md).

Ha a gyorsbeállítások nem felelnek meg a topológiának, lásd az egyéb forgatókönyvek [vonatkozó dokumentációját](#related-documentation).

## <a name="express-installation-of-azure-ad-connect"></a>Az Azure AD Connect gyorstelepítése
Láthatja, hogy ezeket a lépéseket működés közben hello [videók](#videos) szakasz.

1. Jelentkezzen be az Azure AD Connect tooinstall kívánja a helyi rendszergazda toohello kiszolgálóként. Akkor tegye ezt hello kiszolgálón kívánja toobe hello szinkronizálási kiszolgálót.
2. Keresse meg a tooand dupla **AzureADConnect.msi**.
3. Hello üdvözlőképernyőn jelölje ki a hello mezőben elfogadja toohello licencelési időszakonként, és kattintson **Folytatás**.  
4. Hello expressz beállításokat képernyőn kattintson a **gyorsbeállítások használata**.  
   ![Üdvözli a tooAzure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. A csatlakozás tooAzure AD üdvözlő képernyőt adja meg hello felhasználónevet és jelszót egy globális rendszergazda az Azure AD. Kattintson a **Tovább** gombra.  
   ![Csatlakozás tooAzure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Ha hibaüzenetet kap és problémák adódnak a kapcsolódással, majd tekintse meg [kapcsolódási problémák megoldásáról](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Hello Connect tooAD DS képernyőn egy vállalati rendszergazdai fiók hello felhasználónév és jelszó megadása. Hello tartományrészt megadhatja NetBios vagy FQDN formátumban, azaz Fabrikam endszergazda vagy Fabrikam.com endszergazda alakban. Kattintson a **Tovább** gombra.  
   ![Csatlakozás tooAD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Hello [ **az Azure AD-bejelentkezés konfigurálása** ](active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) csak látható lapon, ha nem végezte el [a tartományok ellenőrzését](../active-directory-add-domain.md) a hello [Előfeltételek](active-directory-aadconnect-prerequisites.md).
   ![Nem ellenőrzött tartományok](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
   Ha ez a lap megjelenik, tekintse át az összes **Not Added** (Hozzá nem adott) és **Not Verified** (Nem ellenőrzött) megjelöléssel rendelkező tartományt. Bizonyosodjon meg róla, hogy az Ön által használt tartományok ellenőrizve lettek az Azure AD szolgáltatásban. Kattintson a hello frissítés jelre, miután ellenőrizte a tartományokat.
8. Hello készen tooconfigure képernyőn kattintson a **telepítése**.
   * Opcionálisan hello készen tooconfigure oldalon törölheti hello **hello szinkronizálási folyamat indítása, amint konfigurálás befejeződik** jelölőnégyzetet. Akkor érdemes törölnie ezt a jelölőnégyzetet, ha azt szeretné, toodo további konfigurálást, például a [szűrés](active-directory-aadconnectsync-configure-filtering.md). Ha törli ezt a beállítást, a hello varázsló konfigurálja a szinkronizálást, de hello Feladatütemező letiltva hagyja. Nem fut, amíg manuálisan engedélyezi [újrafuttatásakor valószínűleg megtörténik a hello telepítővarázsló](active-directory-aadconnectsync-installation-wizard.md).
   * Ha rendelkezik Exchange a helyszíni Active Directoryban, akkor azt is, hogy egy beállítás tooenable [ **Exchange hibrid telepítés**](https://technet.microsoft.com/library/jj200581.aspx). Engedélyezze ezt a beállítást, ha megtervezése toohave Exchange-postaládák mindkét hello felhőben és helyszíni: hello azonos idő.
     ![Az Azure AD Connect készen tooconfigure](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Hello telepítésének befejezése után kattintson **kilépési**.
10. Miután hello telepítés befejeződött, jelentkezzen ki, és jelentkezzen be újra mielőtt a Synchronization Service Managert vagy a szinkronizálási szabály szerkesztő.

## <a name="videos"></a>Videók
Az Expressz telepítés hello videót lásd:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Express-Settings/player]
> 
> 

## <a name="next-steps"></a>Következő lépések
Most, hogy az Azure AD Connect telepítése is [hello telepítésének ellenőrzése és licencek hozzárendelése](active-directory-aadconnect-whats-next.md).

További információk, amelyek hello telepítéssel engedélyezett szolgáltatásokkal: [automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md), [véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md), és [az Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Tudjon meg többet a következő általános témaköröket: [Feladatütemező, és hogyan tootrigger szinkronizálása](active-directory-aadconnectsync-feature-scheduler.md).

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

## <a name="related-documentation"></a>Kapcsolódó dokumentáció
| Témakör |
| --- | --- |
| Az Azure AD Connect áttekintése |
| Telepítés testreszabott beállítások használatával |
| Frissítés a DirSync szolgáltatásról |
| Telepítési fiókok |

