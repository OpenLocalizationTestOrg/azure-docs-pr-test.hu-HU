---
title: "Windows 10-eszközökre az Azure Active Directory Join keresztül felhőalapú képességek kiterjesztése |} Microsoft Docs"
description: "Hogyan használhatják a Windows 10-eszközökre az Azure AD Join az Azure Active Directoryban regisztrálva részletes áttekintést nyújt."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 0cd4942f-7d54-474e-bd12-8e6764b0d42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d3a3d3efe1c43caff3b8d2956c14e8c90d05d22b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül
## <a name="what-is-azure-active-directory-join"></a>Az Azure Active Directory-csatlakozás bemutatása
Az Azure Active Directory Join (Azure AD Join) az a funkció, amely a vállalat által birtokolt eszközök regisztrálása az Azure Active Directoryban ahhoz, hogy az eszköz központi kezelését. Lehetővé teszi, hogy a felhasználók számára, például az alkalmazottak és a diákok a vállalati felhőalapú Azure Active Directoryn keresztül csatlakozni. Ez lehetővé teszi az egyszerűsített Windows központi telepítése és a szervezeti alkalmazások és erőforrások Windows-eszköz való hozzáférést, mind a vállalat által birtokolt és személyes tulajdonú (eszközök használata BYOD).

Az Azure AD Join vállalatok, amelyek az első vagy felhő-csak felhőalapú – általában kis és közepes méretű vállalatok számára a helyi Windows Server Active Directory infrastruktúrával nem rendelkező számára készült. Említett, Azure AD Join is is kell nagy méretű szervezeteknek, amelyek nem alkalmas Ez egy hagyományos tartományhoz való csatlakozást (mobil eszközök, például) eszközökön használják, vagy és a felhasználók, akik elsősorban hozzáférés Office 365 vagy más Szolgáltatottszoftver alkalmazások integrált Azure AD-val.

Bár a hagyományos tartományhoz való csatlakozást továbbra is kínál a legjobb a helyszíni eszközök, amelyek képesek tartományhoz való csatlakozás tapasztalhat, Azure AD Join alkalmas eszközöket, amelyek nem tartományhoz való csatlakozást. Az Azure AD Join egyben a felhőbeli felhasználók felügyeletére alkalmas. Ilyeneket ahelyett, hogy a mobileszköz-kezelési képességei használatával hagyományos tartományi felügyeleti eszközök például a csoportházirend és a System Center Configuration Manager (SCCM) használatával.

![Vállalati eszközök és személyes eszközöket a helyszíni Active Directory és az Azure AD – áttekintés](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)

## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Miért kell vállalkozások bevezessék az Azure AD Join?
* **Vállalatok számára, amelyek nagy mértékben a felhőben**: került, vagy a modell, amelyben a helyszíni erőforrásigényét csökkentheti, és működik a felhőben szeretné áthelyezni, ha az Azure AD Join sikerült előnyei. Lehet, hogy manuálisan, illetve a helyszíni Active Directory szinkronizálása az Azure AD-fiókok hozott létre. Mindkét esetben van fiókja Azure AD-ben, és jelentkezzen be a Windows 10 használhatja. A felhasználók az Azure AD vagy az out-of-box élmény (OOBE) vagy a beállítások menü keresztül csatlakozhat a számítógépükre. Után szeretne csatlakozni, a felhasználó élvezi az (egyszeri bejelentkezés SSO) hozzáférést a felhőalapú erőforrásokhoz Office 365-höz hasonló böngészőjük vagy egy Office-alkalmazásban.
* **Oktatási intézmények**:, oktatási intézmények rendelkezik-e a felhasználó két típusa egy hozzáadunk kapcsolatos gyakran forgatókönyv: faculty és a diákok. Faculty tagok között útmutatóul szolgálnak a szervezet hosszabb távú tagjai, a helyszíni fiókokat hoz létre nekik kívánatos. De a diákok a szervezet shorter-term tagja, és így kezelheti az Azure ad-ben. Ez azt jelenti, hogy a felhő helyett a helyszínen tárolt directory méretezési továbbíthatja. Azt is jelenti, hogy a diákok Windows jelentkezzen be a saját Azure AD-fiókok, és elérheti az Office 365-erőforrások, a böngészőjük vagy egy Office-alkalmazásban.
* **Kiskereskedelmi vállalatok**: hallottuk az ügyfelektől kapcsolatban egy másik helyzet lehet hogy határozza munkavállalók könnyebben kezelheti.  Ebben az esetben hosszabb távú, a teljes munkaidejű alkalmazottak vannak általában csak hozhatók létre fiókok a helyszíni fiókok, tartományhoz csatlakoztatott számítógépeken. Azonban határozza munkavállalók a szervezeti shorter-term tagjai, kezelhetők, ahol a felhasználói licencek könnyebben átvihetők kívánatos. A felhasználói fiókokat hozhat létre az Office 365 licencek felhőben lehetővé teszi, hogy a felhasználók a Windows és Office alkalmazások az Azure AD-fiókkal történő bejelentkezés előnyei el. Eközben a licenccel rendelkező nagyobb rugalmasságot karbantartása, hogy kilép a.
* **Egyéb üzleti szervezetektől**: annak ellenére, hogy a felhasználók a helyszíni Active Directoryban karbantartása, továbbra is hasznosak rendelkező felhasználók Azure AD-csatlakoztatott lehet. Ennek oka az Azure AD egy egyszerűsített csatlakozó élmény, hatékony kezelése, automatikus mobileszköz-kezelési beléptetés és egyszeri bejelentkezési képesség kínál az Azure AD és a helyszíni erőforrások.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Az Azure AD Join nyújtja a milyen lehetőségek?
Az Azure AD Join kapott a következő:

* **Vállalat által birtokolt eszközök az önálló kiépítése**: Windows 10-felhasználók konfigurálhatja egy új, légmentes fóliacsomagolásúak eszköz out-of-box élményt nyújt informatikai használata nélkül.
* **Modern kivitelűek támogatása**: Azure AD Join működik-e az eszközök, amelyek nem rendelkeznek a hagyományos képességek csatlakozás tartományhoz.  
* **Támogatja a meglévő munkahelyi és iskolai fiókok**: felhasználóknak nem kell hozhatja létre és kezelheti a személyes Microsoft-fiók beolvasása a vállalat által kiállított eszközökön, a legjobb élményt, mint a Windows 8. Akkor használja a meglévő munkahelyi fiókok az Azure AD. A legtöbb szervezet számára ez alapvetően azt jelenti, hogy a felhasználók állítsa be, és jelentkezzen be Windows azokkal a hitelesítő adatokkal, amelyekkel hozzáférés Office 365.
* **Automatikus mobileszköz-kezelési beléptetés**: az eszközök automatikus regisztrálása a mobileszköz-kezelés az Azure AD való csatlakozáskor. Ez a folyamat működik együtt a Microsoft Intune és a partner mobileszköz-kezelési megoldások. Kezelés az Intune-nal történik, amikor a rendszergazdák figyelő/kezelheti az Azure AD csatlakoztatott eszközök mellett a tartományhoz csatlakoztatott eszközök az SCCM felügyeleti konzolon.
* **Egyszeri bejelentkezés a vállalati erőforrások**: felhasználók egyszeri bejelentkezés a Windows asztali alkalmazások és erőforrások a felhőben, például az Office 365 és Azure AD hitelesítésében keresztül üzleti alkalmazások ezer élvez [az Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Az Azure AD-tartományhoz csatlakoztatott, vállalati tulajdonú eszközök is élvez egyszeri Bejelentkezést a helyszíni erőforrásokhoz, ha az eszköz a vállalati hálózaton, és bárhonnan, ha ezeket az erőforrásokat keresztül érhetők el a [az Azure AD-alkalmazásproxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).
* **Az operációs rendszer Állapothordozás**: kisegítő beállítást, webhelyek, Wi-Fi jelszavakat és egyéb beállítások vannak eszközök közötti szinkronizálását. a vállalat által birtokolt személyes Microsoft-fiók nélkül.
* **Vállalati Windows áruház**: A Windows áruház támogatja az alkalmazás beszerzési és a licencelés az Azure AD-fiókok. A szervezetek mennyiségi licenccel alkalmazásokat is, és elérhetővé teszi azokat a szervezet számára.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hogyan működik az Azure AD Joint a különböző eszközökön?
| Munkahelyi eszközök (a helyi tartományhoz csatlakozik) | Munkahelyi eszközök (a felhő csatlakozik) | Személyes eszköz |
| --- | --- | --- |
| Felhasználók bejelentkezhetnek a Windows a munkahelyi hitelesítő adatait (mint még ma). |Felhasználók bejelentkezve is a Windows Azure AD-ban kezelt munkahelyi hitelesítő adataival. Ez fontos vállalati eszközök három esetben: <ol><li>A szervezet nem rendelkezik az Active Directory, a helyszíni (például a kisvállalkozásnak).</li><li>A szervezet nem hoz létre felhasználói fiókokhoz az Active Directoryban (például a diákok, tanácsadókkal vagy határozza munkavállalók vannak nem hozhatók létre fiókok Active Directory).</li><li>A szervezet rendelkezik, amely nem kell tartományhoz csatlakoztatva egy (helyszíni), például telefonokon vagy táblagépek egy Mobile Termékváltozat (például egy másodlagos eszköz gyári/kereskedelmi emeletet fordítani) futtató vállalati eszközök.</li></ol> Az Azure AD Join támogatja a felügyelt és a összevont szervezetek vállalati eszközök csatlakoztatása. |Felhasználók a személyes Microsoft-fiók hitelesítő adataival (nincs módosítás) jelentkezzen be Windows. |
| Felhasználó rendelkezik barangoló beállítások eléréséhez és a vállalati Windows áruházban. Ezek a szolgáltatások munkahelyi fiókok dolgozni, és nem igényel személyes Microsoft-fiókkal. Ehhez a helyszíni Active Directoryban csatlakoztatása az Azure AD-szervezetek. |A telepítő önkiszolgáló felhasználók teheti meg. Ezek Lépkedjen végig a kezdőélmény (FRX) keresztül a munkahelyi fiókkal informatikai amelynek helyett az eszközök, bár mindkét módszer használata támogatott. |Felhasználók könnyedén adhat hozzá a kezelt munkahelyi fiókkal az Active Directoryban vagy az Azure AD. |
| A felhasználóknál SSO képes az asztali alkalmazások, webhelyek és az erőforrások – beleértve a helyi erőforrások és a felhőalkalmazások, amelyeknek az Azure AD használatára a hitelesítéshez. |Eszközök a vállalati címtárban (az Azure AD) automatikusan regisztrált és a mobileszköz-kezelés automatikusan regisztrálva vannak. (Az azure AD Premium-funkció). |A felhasználóknál SSO képes alkalmazások között és a webhelyek/erőforrásokhoz a munkahelyi fiókhoz. |
| Felhasználók a személyes Microsoft-fiókok vállalati adat befolyásolása nélkül a személyes képek és a fájlok elérését is hozzáadhat. (Központi továbbra is működnek munkahelyi fiókkal.) A Microsoft-fiók lehetővé teszi az egyszeri Bejelentkezést, és már nem a beállítások központi meghajtók. |Felhasználók teheti a winlogon, önkiszolgáló jelszó-visszaállítási (SSPR) visszaállíthatnák elfelejtett jelszó jelentését. (Az azure AD Premium-funkció). |Felhasználók hozzáférhetnek a vállalati Windows áruház, hogy szerezzen be és az üzletági alkalmazások használatát a személyes eszközeiket. |

## <a name="additional-information"></a>További információ
* [Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata](active-directory-azureadjoin-windows10-devices-overview.md)
* [A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

