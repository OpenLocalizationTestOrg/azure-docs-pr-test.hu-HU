---
title: "Alkalmazások az Azure Active Directoryval aaaManaging |} Microsoft Docs"
description: "Ez a cikk hello előnyei Azure Active Directory integrálása a helyszíni, felhő- és SaaS-alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 95b96f10-2d5c-4b78-8af8-d3657a24140f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 0016f8b433e101d8a150bc6d9be3931851578241
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-applications-with-azure-active-directory"></a>Alkalmazások kezelése az Azure Active Directoryban
Túl hello tényleges munkafolyamat vagy a tartalmat a vállalatok rendelkeznek összes alkalmazás két alapvető követelményei:

1. tooincrease termelékenység alkalmazások könnyen toodiscover és hozzáférést kell lennie.
2. tooenable biztonsági és irányítási, hello szervezetnek támogatnia kell a és a szervezett a felhasználók és ténylegesen hozzáférő minden alkalmazás

A felhőalapú alkalmazások ez legjobb elérhető identitás toocontrol használatával hello world "*számára engedélyezett toodo mi*".

A számítástechnikai terminológia:

* *Aki* nevezik *identitás* -hello felhasználók és csoportok kezelése
* *Mi* nevezik *hozzáférések felügyeletével* – hello tooprotected erőforrások kezelése

Összetevőket egyaránt együtt nevezik *identitás- és hozzáférés-kezelés (IAM)*, hello által definiált [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) csoportot "*hello biztonsági állapotokban, amely lehetővé teszi a hello jobbra egyéni tooaccess hello jobb erőforrásaikhoz hello jobb hello jobb okokból alkalommal*".

Gépházban, de mi hello probléma? Ha IAM *nem kezelt* egy helyen, egy integrált megoldás részeként:

* Identitás rendszergazdák rendelkeznek tooindividually létrehozása és frissítése külön történik, az összes alkalmazás felhasználói fiókokat a redundancia és időigényes tevékenység.
* Felhasználó rendelkezik toomemorize több hitelesítő adatok tooaccess hello toowork a szükséges alkalmazásokhoz. Ennek eredményeképpen felhasználók általában toowrite le a jelszavukat, vagy más egyéb adatok biztonsági kockázatokkal jelszó megoldások használja.
* Redundáns, sok időt vesz igénybe tevékenységek csökkentse hello mennyiségét, amikor a felhasználók és rendszergazdák dolgozik, így növelik a lényeg az üzleti üzleti tevékenységét.

Igen milyen általában megakadályozza, hogy a szervezetek integrált IAM-megoldások bevezetése?

* A műszaki megoldások toobe hatályba, és minden egyes szervezet saját alkalmazásokhoz igazított igénylő szoftverplatformok alapulnak.
* A felhőalapú alkalmazásokhoz, mint az IT-szervezet integrálható a meglévő IAM-megoldások gyorsabb ütemben gyakran fogadnak el.
* Biztonsági és figyelési tooling szükséges további testreszabási és integrációs tooachieve átfogó E2E célokra.

## <a name="azure-active-directory-integrated-with-applications"></a>Az Azure Active Directory integrált alkalmazások
Az Azure Active Directory a Microsoft átfogó szolgáltatott identitási (IDaaS) szolgáltatás megoldás, amely:

* Lehetővé teszi a felhő alapú szolgáltatásként IAM 
* Központi hozzáférési kezelhető, egyszeri bejelentkezést (SSO) és a jelentéskészítési 
* Támogatja az integrált kezelés [alkalmazások ezer](https://azure.microsoft.com/marketplace/active-directory/) hello alkalmazáskatalógusában, például a Salesforce, a Google Apps, a mezőbe, a Concur és több. 

Az Azure Active Directoryval, minden alkalmazás közzététele a partnerek és a felhasználóknak (üzleti vagy fogyasztó) kell hello ugyanolyan identitások és hozzáférések felügyeleti funkciókkal rendelkeznek.<br> Ez lehetővé teszi, hogy toosignificantly a működési költségek csökkentése.

Mi történik, ha olyan alkalmazás, amely még nem szerepel az hello alkalmazáskatalógusában tooimplement van szüksége? Ez nem egy kicsit több időt vesz igénybe, mint az egyszeri bejelentkezés konfigurálása az alkalmazások hello alkalmazás gyűjteményből, akkor az Azure AD biztosít a egy varázsló, amely segít hello konfigurációjával kapcsolatban.

az Azure AD hello érték túllép "csak" felhőalapú alkalmazásokhoz. Is használhatja azt a helyszíni alkalmazások biztonságos távoli hozzáférést biztosító. A biztonságos távoli hozzáférés megszüntetheti hello hello szükség VPN vagy más hagyományos távelérési felügyeleti esetében.

Az alkalmazások központi kezelési és egyszeri bejelentkezés (SSO) megadásával az Azure AD hello megoldást kínál toohello fő adatok biztonsági és a termelékenység problémákat.

* Felhasználók férhetnek hozzá egy bejelentkezési jogosultságot ad a további időt tooincome előállítása vagy üzleti tevékenységekbe történik a több alkalmazást.
* Identitás-rendszergazdák kezelhetik hozzáférés tooapplications egy helyen.

hello hello felhasználó és a vállalat előnye nyilvánvaló. A következőkben a hello járó előnyök identitás rendszergazda, és hello szervezet részletes bemutatása.

## <a name="integrated-application-benefits"></a>Integrált alkalmazás előnyei
egyszeri bejelentkezési folyamat hello van két lépésből áll:

* Hitelesítés, hello folyamat hello felhasználóidentitás érvényesítése.
* Engedélyezési, hello döntési tooenable vagy blokk hozzáférés tooa erőforrást egy hozzáférési házirenddel.

Azure AD-toomanage alkalmazások és az egyszeri bejelentkezés engedélyezése használatakor:

* Hitelesítési hello felhasználói helyszíni (pl. AD) vagy az Azure AD-fiókkal történik.
* Engedélyezési végrehajtása a hello az Azure AD-hozzárendelés és a védelmi házirend egységes felhasználói élmény biztosítása, és így már tooadd hozzárendelés, helyek és bármely alkalmazás, függetlenül annak belső funkciói MFA feltételeket.

Ez fontos a hello célalkalmazás végrehajtása, amely módon hello engedélyezési hello toounderstand függ, hogyan hello alkalmazás integrálva lett-e az Azure AD.

* **Szolgáltató által előre integrált alkalmazások** például Office 365 és Azure, ezek a közvetlenül az Azure AD épül, és az átfogó identitás- és hozzáférés kezelésére alkalmas a tőle függő alkalmazások. Directory adatokat és -tokennel kiállítási keresztül engedélyezve van a toothese alkalmazásokat.
* **A Microsoft és az egyéni alkalmazások előre integrált alkalmazások** ezek a belső alkalmazásspecifikus címtár támaszkodnak, és az Azure AD függetlenül is működőképesek független felhőalapú alkalmazásokhoz. Az alkalmazás hitelesítő adatok leképezése tooan alkalmazás fiók kiállításával engedélyezve van a toothese alkalmazásokat. Hello alkalmazások képességei hello credential függően egy összevonási token vagy a felhasználónév- és a jelszót, amely korábban hello alkalmazás lett kiépítve.
* **A helyszíni alkalmazások** elsősorban a hozzáférési tooon helyszíni alkalmazások engedélyezése hello Azure AD alkalmazásproxy azzal közzétett alkalmazások. Ezek az alkalmazások egy központi helyszíni címtár, például a Windows Server Active Directory támaszkodnak. Toothese alkalmazásokat közben hello a helyi bejelentkezés követelmény érvényesítenie hello proxy toodeliver hello alkalmazás tartalom toohello végfelhasználói futtatásától engedélyezve van.

Például ha egy felhasználó csatlakozik a szervezetben, szüksége toocreate fiók hello felhasználói műveleteihez hello elsődleges bejelentkezés az Azure AD-ben. Ha a felhasználó hozzáférési tooa felügyelt alkalmazás, például a Salesforce igényel, is toocreate egy fiókot a felhasználó a Salesforce-ban és az toohello Azure-fiók toomake SSO munkahelyi. Amikor hello felhasználó elhagyja a szervezetet, ajánlott toodelete hello Azure AD-fiókot és a fiókokhoz tartozó hello IAM-tárolóinak hello alkalmazások hello felhasználó hozzáfért.

## <a name="access-detection"></a>Hozzáférés észlelése
A modern vállalatok informatikai részlegek ismerik gyakran nem minden hello felhőalapú alkalmazások vannak használatban lévő. A Cloud App Discovery együtt az Azure AD lehetőséget biztosít a megoldás toodetect ezeket az alkalmazásokat.

## <a name="account-management"></a>Fiókkezelés
Hagyományosan hello kezelésére számos olyan alkalmazás egy kézi művelet által végrehajtott informatikai vagy személyes hello szervezet támogatja. Az Azure AD fiókkezelés teljesen automatizált minden szolgáltató integrált alkalmazások és az automatizált felhasználókiépítése támogató Microsoft vagy SAML JIT előre integrált alkalmazások között.

## <a name="automated-user-provisioning"></a>Automatizált felhasználókiépítése
Egyes alkalmazások automation felületek létrehozására és Eltávolítás (vagy deaktiválása) fiókok adja meg. Ha egy szolgáltató ilyen interfész, az Azure ad elkészítéséhez használja. Ez csökkenti a működési költségek, mivel a felügyeleti feladatok automatikusan megtörténik, és javítja a hello biztonsági környezet, mivel így csökken a jogosulatlan hozzáférés esélyét hello.

## <a name="access-management"></a>Hozzáférés-kezelés
Az Azure AD használatával kezelheti a hozzáférési tooapplications használó egyéni vagy alapú hozzárendelések szabály. Access management toohello megfelelő személyeknek hello szervezet biztosítja hello legjobb felügyeletet és a támogató személyzet terhelésének csökkentése hello delegálhatja.

## <a name="on-premises-applications"></a>a helyszíni alkalmazások
a beépített alkalmazásproxy hello lehetővé teszi a helyszíni alkalmazások tooyour felhasználók mindkét egységes élmény modern felhőalapú alkalmazások és hello előnyt elérni az Azure AD-figyelési, jelentéskészítési és biztonsági képességeket toopublish .

## <a name="reporting-and-monitoring"></a>Jelentéskészítési és figyelés
Az Azure AD segítségével előre integrált jelentéskészítési és figyelési képességek, amelyek lehetővé teszik tooknow hozzáférés tooapplications kik és azok ténylegesen használatakor őket.

## <a name="related-capabilities"></a>Kapcsolódó képességei
Az Azure ad-val biztonságossá teheti az alkalmazások részletes hozzáférési házirendek és előre integrált többtényezős Hitelesítést. az Azure MFA kapcsolatos információkért tekintse meg toolearn [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Bevezetés
alkalmazások integrálása az Azure AD elindult tooget vessen egy pillantást hello [integrálása Azure Active Directory és az alkalmazások első lépések útmutató](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Lásd még:
[Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

