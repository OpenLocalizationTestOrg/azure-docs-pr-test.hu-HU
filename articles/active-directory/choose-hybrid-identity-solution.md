---
title: "egy Azure hibrid identitáskezelési megoldás aaaChoose |} Microsoft Docs"
description: "Ismerkedjen meg hello rendelkezésre álló hibrid identitáskezelési megoldások és a javaslatok, toomake hello legjobb identitás irányítás döntés a szervezete."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4ab8aff314f6308ab32f77e81cf0f07e1f5d7b41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft hibrid identitáskezelési megoldások
[A Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hibrid identitáskezelési megoldások lehetővé teszik a helyszíni címtárobjektumok toosynchronize továbbra is a felhasználók a helyszíni felügyelete során az Azure AD-val. hello először döntési toomake, a helyszíni Windows Server Active Directory és az Azure AD, akkor toouse identitás szinkronizált vagy összevont identitás toosynchronize tervezése során. Szinkronizált identitások, és szükség esetén jelszókivonatait, lehetővé teszi a felhasználók toouse hello azonos jelszó tooaccess mind a helyszíni és felhőalapú szervezeti erőforrások. A speciális forgatókönyv-követelményeinek, például egyszeri bejelentkezéses (SSO) vagy a helyi multi-factor Authentication szolgáltatás használatakor toodeploy Active Directory összevonási szolgáltatások (AD FS) toofederate identitások kell. 

Nincsenek elérhető hibrid identitásnak számos lehetőség közül választhat. Ez a cikk úgy dönt, egyszerű telepítés alapján szervezete számára legjobb egy hello és az adott identitások és hozzáférések felügyeletéhez szükséges információkat toohelp tartalmazza. Ajánlott identitás modellt a szervezet igényeinek megfelelő meghatározásához vegye figyelembe, szükség toothink kapcsolatos idő, a meglévő infrastruktúra, az összetettséget és a költségeket. Ezek a tényezők esetében minden egyes szervezet különböző, és idővel változhatnak. Azonban ha a követelmények változnak, akkor is hello rugalmasságot tooswitch tooa identitások modell.

> [!TIP]
> Ezek a megoldások összes érkeznek által [az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="synchronized-identity"></a>Szinkronizált identitás 
Szinkronizált identitás hello legegyszerűbb módja toosynchronize helyszíni címtárobjektumok (felhasználókat és csoportokat) az Azure ad-val. 

![Szinkronizált hibrid identitás](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Míg szinkronizált identitás hello legegyszerűbben és leggyorsabban metódus, a felhasználók továbbra is szükséges toomaintain külön jelszó felhőalapú erőforrások. tooavoid, ez is (opcionális) [kivonatát a felhasználói jelszavakat szinkronizálja](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) tooyour az Azure Active Directoryban. Jelszó kivonatok lehetővé teszi a felhasználók toolog toocloud szervezeti erőforrásokhoz, a szinkronizálás hello azonos felhasználónevet és jelszót használják-e a helyszíni. Az Azure AD Connect rendszeresen ellenőrzi a módosításokat, és az Azure AD-címtár szinkronizálva tartja a helyszíni címtárba. Amikor egy felhasználói attribútum, vagy a jelszó megváltozott a helyszíni Active Directory, automatikusan frissül az Azure ad-ben. 

A legtöbb szervezet, akik csupán tooenable tooOffice 365, SaaS-alkalmazásokhoz és más Azure AD-alapú erőforrások a felhasználók toosign hello alapértelmezett jelszó-szinkronizálási beállítás kérése ajánlott. Ha a probléma nem szűnik meg, az átmenő hitelesítés és az AD FS közötti toodecide lesz szüksége.

> [!TIP]
> Felhasználói jelszavak a helyi Windows Server Active Directory hello tényleges felhasználói jelszó jelölő kivonatot hello formában vannak tárolva. A kivonat értéke egy egyirányú matematikai függvény (hello kivonatoló algoritmus) eredményt. Nincs a jelszó egyirányú függvény toohello egyszerű szöveges verziójának metódus toorevert hello eredmény. A jelszó kivonatát toosign tooyour a helyi hálózaton nem használható. Amikor toosynchronize jelszavak mellett dönt, az Azure AD Connect kivonatok jelszókivonatait a hello a helyszíni Active Directory és feldolgozási toohello Jelszókivonat szinkronizált tooAzure AD ahhoz, hogy további biztonsági vonatkozik. A jelszó-szinkronizálás jelszó késleltetve visszaírt tooenable önkiszolgáló módon új jelszót az Azure AD együtt is használható. Emellett engedélyezheti az egyszeri bejelentkezés (SSO) a felhasználók számára, amelyek a vállalati hálózathoz csatlakoztatott toohello tartományhoz csatlakoztatott számítógépeken. Az egyszeri bejelentkezést az engedélyezett felhasználók csak kell tooenter egy felhasználónév toosecurely hozzáférés felhőben található erőforrásokat. 

## <a name="pass-through-authentication"></a>Áteresztő hitelesítés
[Az Azure AD átmenő hitelesítés](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) egyszerű jelszó érvényesítése megoldást kínál az Azure AD-alapú szolgáltatásokhoz a helyszíni Active Directory használatával. Ha a szervezet biztonsági és megfelelőségi házirendek nem teszik lehetővé a felhasználói jelszavak küldése még kivonatolt formában, és a tartományhoz csatlakoztatott eszközök csak kell toosupport asztali SSO, ajánlott átmenő hitelesítést használó kiértékeléséhez. Áteresztő hitelesítés nem szükséges a Szegélyhálózaton, amely egyszerűbbé teszi a hello telepítési infrastruktúra AD FS-hez képest hello központi telepítési. Amikor a felhasználók bejelentkeznek az Azure AD, ezt a hitelesítési módszert érvényesíti közvetlenül szemben a helyszíni Active Directory felhasználók jelszavát.

![Áteresztő hitelesítés](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Átmenő hitelesítéssel nincs szükség speciális hálózati infrastruktúra, és nem kell toostore a helyszíni jelszavak hello felhőben. Egyszeri bejelentkezés, kombinálva átmenő hitelesítés olyan valóban integrált környezetet biztosít, tooAzure AD vagy más felhőalapú szolgáltatásokat bejelentkezéskor.

Áteresztő hitelesítés van beállítva, az Azure AD Connect, amely használ egy egyszerű helyszíni ügynök, amely figyeli a jelszó érvényesítése kéréseket. hello ügynök lehet könnyen üzembe helyezett toomultiple gépek tooprovide magas rendelkezésre állás és a terheléselosztás. Mivel az összes kommunikáció kimenő csak, esetében nem követelmény a hello összekötő toobe szegélyhálózaton telepítve. hello kiszolgáló számítógép hello összekötőre vonatkozó követelmények a következők:

- Windows Server 2012 R2 vagy újabb
- Csatlakoztatott tooa tartomány hello erdőben, amelyen keresztül a felhasználók érvényesítése

> [!NOTE]
> Az Azure AD átmenő hitelesítés jelenleg előzetes verzióban érhetők, és a webes webböngésző-alapú ügyfelek és a modern hitelesítést támogató Office-ügyfelek esetén támogatott. Nem támogatott ügyfelek például az örökölt Office-ügyfelekhez és az Exchange ActiveSync (beleértve a mobileszközök natív levelezőprogramokat), esetén ajánlott toouse hello modern hitelesítést egyenértékű. Modern hitelesítést nemcsak lehetővé teszi, hogy átmenő hitelesítést, de lehetővé teszi a feltételes hozzáférési házirendek toobe vonatkoznak rájuk, például többtényezős hitelesítést is. 

Áteresztő hitelesítés jelenleg nem támogatott csatlakoztatásakor Windows 10-eszközök segítségével tooAzure AD. Azonban Jelszókivonat-szinkronizálást is használhatja az automatikus tartalék toosupport Windows 10, és azt korábban említettük a hello tanúsítványbeléptetési. Hello előzetes Jelszókivonat-szinkronizálást alapértelmezés szerint engedélyezve van Ha átmenő hitelesítés az Azure AD Connectben hello bejelentkezési lehetőséget választotta.


## <a name="federated-identity-ad-fs"></a>Összevont identitás (AD FS)
Jobban kézben tartani, hogy a felhasználók miként férhetnek hozzá a Office 365 és más felhőalapú szolgáltatásokat, állíthatja be a címtár-szinkronizálás egyszeri bejelentkezés (SSO) használatával [Active Directory összevonási szolgáltatások (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server-2016). A felhasználói bejelentkezéseket a az AD FS delegáltak hitelesítési tooan összevonását helyszíni kiszolgáló, amely ellenőrzi az felhasználói hitelesítő adatokat. Ebben a modellben a helyszíni Active Directory hitelesítő adatok soha nem átadott tooAzure AD.

![Összevont identitás](./media/choose-hybrid-identity-solution/federated-identity.png)

Más néven identitás-összevonást, a bejelentkezési módszer biztosítja, hogy a felhasználói hitelesítést ellenőrzött a helyszínen és lehetővé teszi a rendszergazdák tooimplement szigorúbb szintű hozzáférés-vezérlést. Az AD FS identitás-összevonási legtöbb bonyolult hello beállítás, és megköveteli, hogy a helyszíni környezetben további kiszolgáló üzembe helyezése. Identitás-összevonást is megmutatják a véglegesítések számát, az Active Directory és az AD FS infrastruktúra tooproviding 24 x 7 támogatása. A magas szintű támogatási szükség, mert ha a helyszíni Internet-hozzáférést, tartományvezérlő, vagy az AD FS-kiszolgálók nem érhetők el, felhasználók nem jelentkezhetnek be toocloud szolgáltatások.

> [!TIP]
> Ha úgy dönt, hogy az Active Directory összevonási szolgáltatások (AD FS) összevonásának toouse, opcionálisan állíthatja be a jelszó-szinkronizálás biztonsági abban az esetben, ha az AD FS infrastruktúra sikertelen lesz.


## <a name="common-scenarios-and-recommendations"></a>Gyakori forgatókönyvek és javaslatok
Az alábbiakban néhány gyakori hibrid identitás és access management forgatókönyvet az ajánlások toowhich hibrid identitáskezelési megoldással (vagy beállítások) megfelelő, az egyes lehet.

|Kell:|PWS és az egyszeri bejelentkezés<sup>1</sup>| ESP és az egyszeri bejelentkezés<sup>2</sup> | AZ AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Új felhasználói, lépjen kapcsolatba, és a helyszíni Active Directory toohello felhő automatikusan létrehozott szinkronizálása.|![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)| ![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png) |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|Állítsa be a bérlő Office 365 hibrid forgatókönyvek esetén|![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)| ![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png) |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|Engedélyezése a felhasználók toosign a és a helyszíni jelszavakat felhőszolgáltatások eléréséhez|![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)| ![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png) |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|Egyszeri bejelentkezés vállalati hitelesítő adataival megvalósítása|![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)| ![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png) |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|Győződjön meg arról, nincs jelszókivonatait hello felhőben vannak tárolva| |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|A helyi multi-factor Authentication hitelesítés megoldások| | |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|Intelligens kártyás hitelesítés támogatása a felhasználók számára<sup>4</sup>| | |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|
|Hello Office-portál és a hello Windows 10 asztali megjeleníteni a jelszó lejárati értesítéseinek| | |![Ajánlott](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> jelszó-szinkronizálás az egyszeri bejelentkezést. 

> <sup>2</sup> áteresztő hitelesítés és egyszeri bejelentkezést. 

> <sup>3</sup> összevont egyszeri bejelentkezés az AD FS.

> <sup>4</sup> a vállalati nyilvános kulcsokra épülő infrastruktúra tooallow integrálható az AD FS-bejelentkezés tanúsítványokkal. Ezek a tanúsítványok lehet például az MDM-mel vagy csoportházirend-objektum vagy intelligens kártyás tanúsítványok (beleértve a PIV/CAC kártyák) vagy Hello for Business (tanúsítvány-megbízhatósági) megbízható létesítési csatornákon keresztül telepített soft-tanúsítványok. Intelligens kártyás hitelesítés támogatásával kapcsolatos további információkért lásd: [ebben a blogban](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).


## <a name="next-steps"></a>Következő lépések
[További információ a koncepció igazolása Azure környezetben:](https://aka.ms/aad-poc)

[Az Azure AD Connect telepítése](http://go.microsoft.com/fwlink/?LinkId=615771)

[Hibrid identitásszinkronizálási figyelése](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)

