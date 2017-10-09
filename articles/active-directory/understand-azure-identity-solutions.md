---
title: "Azure-identitás aaaUnderstand |} Microsoft Docs"
description: "Ismerkedjen meg a Microsoft Azure identitáskezelési megoldás feltételeit, fogalmak és javaslatok meg toomake hello legjobb identitás irányítás döntés a szervezete."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/17/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4d9c90bd7a6bcc9637be3107998f9da5bd4cbdae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-identity-solutions"></a>Az Azure identitáskezelési megoldásairól ismertetése
A Microsoft Azure Active Directory (Azure AD) egy identitás- és hozzáférés kezelési felhőalapú megoldás, amely címtárszolgáltatások, identitás-irányítás és alkalmazáshozzáférés-kezeléshez. Az Azure AD gyorsan [lehetővé teszi az egyszeri bejelentkezés (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) too1, 000 hello előre integrált kereskedelmi és egyéni alkalmazások [az Azure AD application gallery](https://azure.microsoft.com/marketplace/active-directory/all/). Ezekhez az alkalmazásokhoz számos valószínűleg már használhatja például az Office 365, a Salesforce.com, a mezőbe, a ServiceNow és a Workday.

Egyetlen Azure AD-címtár létrehozásakor automatikusan az Azure-előfizetéssel társítva. Hello identitás szolgáltatást az Azure-ban majd az Azure AD felhőalapú erőforrások Identitáskezelés és a hozzáférés-vezérlési funkciók biztosít. Ezeket az erőforrásokat például felhasználók, az alkalmazások és a csoportok az egy adott bérlő (szervezet) a hello a következő ábrán látható módon:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Microsoft Azure kínál számos módon tooleverage identitás szolgáltatásként (IDaaS) összetettsége toomeet különböző szintű egyedi szervezete igényeinek. Ez a cikk többi hello segít megérteni a alapszintű Azure-identitás terminológia és a fogalmakat, valamint a javaslatok, toomake hello legjobb választás hello rendelkezésre álló lehetőségek közül.

## <a name="terms-tooknow"></a>Feltételek tooknow

A szervezet egy Azure-identitás megoldás dönthet, meg kell hello feltételeket, amikor Azure-identitás szolgáltatások van szó gyakran használt alapvető ismeretekkel.

|Kifejezés tooknow| Leírás|
|-----|-----|
|Azure-előfizetés |Előfizetések az Azure-szolgáltatásokhoz használt toopay és általában csatolt tooa hitelkártya. Több előfizetéssel is rendelkezik, de nehéz tooshare erőforrások előfizetések között lehet.|
|Azure-bérlőhöz | Az Azure AD-bérlő egyetlen szervezet képviselője. Az Azure AD, amely automatikusan jön létre, amikor egy szervezet előfizet a Microsoft felhőalapú szolgáltatás előfizetésének például Azure, az Intune-ban vagy az Office 365 dedikált, megbízható példánya. Bérlők kaphatnak hozzáférést tooservices vagy dedikált környezetben (egybérlős) vagy más szervezetekkel (több-bérlős) megosztott környezetben.|
|Azure AD-címtár | Minden Azure-bérlőhöz tartozik egy dedikált, megbízható Azure AD-címtár, amely tartalmazza a hello bérlő felhasználók, csoportok és alkalmazások. Használt tooperform identitások és hozzáférések felügyeleti funkciók bérlői erőforrások. Mivel egy egyedi Azure AD-címtár automatikusan kiosztott toorepresent a szervezet, amikor regisztrál egy Microsoft felhőszolgáltatásra, például az Azure, a Microsoft Intune, illetve az Office 365, látni fogja, néha hello feltételek *bérlői*, *Az azure AD*, és *Azure AD-címtár* jelentése megegyezik. |
|Egyéni tartomány | Amikor először iratkozik fel a Microsoft felhőalapú szolgáltatás előfizetési, a bérlő (szervezet) használ egy *. onmicrosoft.com* tartomány nevét. Azonban a legtöbb szervezet egy vagy több tartomány névvel rendelkeznek használt toodo üzleti és, hogy a végfelhasználók tooaccess vállalati erőforrásokat használnak. Az egyéni tartomány nevét tooAzure AD, hogy hello tartománynév megszokott tooyour felhasználók, például hozzáadhat  *alice@contoso.com*  helyett  *alice@contoso.onmicrosoft.com* . |
|Azure AD-fiók | Ezek a identitásokat tartalmaz, amelyek az Azure AD használatával létrehozott vagy egy másik Microsoft felhőszolgáltatásra, például az Office 365. Az Azure AD tárolásuk és hozzáférhető tooany hello szervezete cloud service előfizetések. |
|Azure-előfizetési rendszergazda| hello fiókadminisztrátort hello személy, aki regisztrált a vagy vásárolt Azure-előfizetés hello. Használhatnak hello [Account Center](https://account.windowsazure.com/Home/Index) különböző kezelési feladatokat, például előfizetések létrehozása, előfizetések megszakítja, hello számlázási-előfizetéshez tartozó módosítása, vagy módosítsa tooperform hello szolgáltatás-rendszergazdát. |
|Az Azure AD globális rendszergazda | Az Azure AD globális rendszergazdák rendelkezik teljes körű hozzáférés az Azure AD tooall felügyeleti funkcióhoz. hello személy, aki előfizet egy felhőalapú Microsoft-szolgáltatás előfizetésének automatikusan alapértelmezés szerint a globális rendszergazdája lesz. Egynél több globális rendszergazda is rendelkezik, de csak globális rendszergazdák rendelhetik hozzá bármelyik [egyéb rendszergazdai szerepköröket hello](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) toousers. |
|Microsoft-fiók | Microsoft-fiókok (létrehozott Ön által személyes használatra) adja meg a hozzáférés tooconsumer célú Microsoft-termékek, és a felhőalapú szolgáltatások, mint például az Outlook (Hotmail), OneDrive, Xbox LIVE, vagy az Office 365. Ezeket az identitásokat létrehozása és Microsoft fiókkal futtatott végfelhasználói identitásrendszer Microsoft hello tárolja.|
|Munkahelyi vagy iskolai fiókok | Munkahelyi vagy iskolai fiókját (üzleti vagy oktatási használatra rendszergazdája által kibocsátott) tooenterprise vállalati szintű Microsoft felhőszolgáltatások, például Azure, az Intune-ban vagy az Office 365 hozzáférést biztosítanak.|


## <a name="concepts-toounderstand"></a>Fogalmak toounderstand

Most, hogy tudja, hogy hello alapszintű Azure-identitás feltételeket, akkor kell tudhat meg többet a Azure-identitás fogalmakat, amelyek segítenek a döntést egy tájékozott Azure-identitás szolgáltatás.

|A koncepció toounderstand |Leírás|
|-----|-----|
|[How Azure subscriptions are associated with Active Directory? (Hogyan kapcsolódnak az Azure-előfizetések az Azure Active Directory-hoz?)](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Minden Azure-előfizetést egy Azure-ral megbízhatósági kapcsolatban áll az Active directory tooauthenticate felhasználók, szolgáltatások és eszközök. *Több előfizetés is megbízhat hello ugyanazt az Azure AD directory, de egy előfizetés csak egyetlen megbízik az Azure AD-címtár*. A bizalmi kapcsolat ellentétben, amelyek más Azure-erőforrások (webhelyek, adatbázisok és így tovább), amelyek több mint egy előfizetés tartozik előfizetés hello kapcsolat van. Ha egy előfizetés lejár, majd az Azure AD eltérő hello az előfizetéshez tartozó hozzáférési tooresources is leáll. Azonban hello Azure Active directory marad, az Azure-ban, hogy más előfizetéseket társíthat a könyvtárhoz, és továbbra is toomanage bérlői erőforrásokhoz.|
|[Licencelés működéséről az Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | Ha és a nagyvállalati mobilitási csomag, a prémium szintű Azure AD és a Azure AD alapvető, a címtár aktiválása hello előfizetéssel, beleértve az érvényességi frissül, és előre fizetett licenccel. Ha hello előfizetése aktív, hello szolgáltatást az Azure AD globális rendszergazdái által felügyelt, és a licenccel rendelkező felhasználók által használt. Az előfizetési adatai, beleértve rendelt vagy rendelkezésre álló licencek számát hello érhető el hello Azure-portálon a hello **Azure Active Directory** > **licencek** panelen. Ez akkor is hello legjobb helyen toomanage a licenc-hozzárendelések.|
|[Szerepköralapú hozzáférés-vezérlés a hello Azure-portálon](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)|Azure szerepköralapú hozzáférés-vezérlés (RBAC) segítségével adja meg a részletes hozzáféréskezelést az Azure-erőforrások. Túl sok engedélyek teszi közzé, és tooattackers fiók. Túl kevés engedélyek, az azt jelenti, hogy az alkalmazottak nem munkavégzéséhez hatékony. Az RBAC használata, adhat az alkalmazottak hello pontos jogot kell három alapvető szerepkörökhöz tooall erőforráscsoport-sablonok alapján: tulajdonos, közreműködő, olvasó. Too2, saját 000 másolatot is létrehozhat [egyéni RBAC-szerepkörök](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) toomeet a specifikus kell. |
|[Hibrid identitás](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Hibrid identitás úgy érhető el a helyszíni Windows Server Active Directory (AD DS) integrálása az Azure AD használatával [az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Így közös identitást a felhasználók az Office 365, Azure-ban és a helyszíni alkalmazások vagy SaaS-alkalmazásokhoz az Azure ad-vel integrált tooprovide. Hibrid identitás, az gyakorlatilag kiterjeszti a helyszíni környezet toohello felhőalapú identitás- és hozzáférés az.|

### <a name="hello-difference-between-windows-server-ad-ds-and-azure-ad"></a>Windows Server Active Directory tartományi szolgáltatások és az Azure AD hello különbségének
Azure Active Directory (Azure AD), mind a helyszíni Active Directory (Active Directory tartományi szolgáltatások vagy az AD DS) a rendszerek, a címtáradatok tárolására és kezelésére a felhasználókat és erőforrásokat, például a bejelentkezési folyamatot, a hitelesítés és a directory-keresések közötti kommunikációt.

Ha már ismeri a helyszíni Windows Server Active Directory tartományi szolgáltatások (AD DS), első bevezetése Windows 2000 Server, akkor valószínűleg megismerte az Alkalmazásidentitás szolgáltatás hello alapvető fogalma. Azonban célszerű is fontos toounderstand győződjön meg arról, hogy az Azure AD nem csak egy tartományvezérlő hello felhőben. Egy teljesen új módon számbavétele toofully következőket foglalják magukban felhőalapú képességek igényel, és a szervezet védett modern fenyegetésekkel szembeni hatékony Azure egy teljesen új módon identitás biztosító szolgáltatásként (IDaaS). 

Active Directory tartományi szolgáltatások egy kiszolgálói szerepkör, Windows Server, ami azt jelenti, hogy központilag telepíthető fizikai vagy virtuális gépeken. Van olyan hierarchikus struktúra X.500 alapján. DNS-objektumok, keresése képes kezelni az LDAP segítségével, és elsősorban használ Kerberos hitelesítéshez használ. Az Active Directory lehetővé teszi, hogy szervezeti egységben (OU) és csoportházirend-objektumok (GPO) továbbá toojoining gépek toohello tartomány és a megbízhatósági kapcsolatok jönnek létre tartományok között.

IT-védelemmel látta el a biztonsági szegélyhálózati évre Active Directory tartományi szolgáltatások használatával, de identitás igények támogatása az alkalmazottak, ügyfelek és partnerek szükséges egy új vezérlő vezérlősík modern, a peremhálózati nélküli vállalatok. Az Azure AD, hogy identitás vezérlő vezérlősík. Biztonsági túljutott hello vállalati tűzfal toohello felhő ahol az Azure AD védi a vállalati erőforrásokhoz és a hozzáférés egy közös identitás biztosítása a felhasználók számára (helyi vagy hello felhőben). Ezzel a módszerrel a felhasználók hello rugalmasságot toosecurely hozzáférés hello alkalmazások tooget a munkájukat, szinte bármilyen eszközről van szükségük. Zökkenőmentes kockázat-alapú védelmi adatvezérlők, biztonsági gépi tanulásra képességek, valamint részletes reporting is biztosítja, hogy informatikai igények tookeep vállalati adatok biztonságát.

Az Azure AD egy több ügyfél nyilvános címtárszolgáltatás, ami azt jelenti, hogy az Azure AD belül hozhat létre a bérlőt, a felhő kiszolgálókon és az alkalmazások, mint az Office 365. Felhasználók és csoportok jönnek létre egy egyszerű struktúra nélkül szervezeti egységek vagy csoportházirend-objektumokat. Történik hitelesítés, SAML, például protokollokon keresztül WS-Federation és OAuth. Az Azure AD lehetséges tooquery, de az LDAP használata helyett a REST API hívása AD Graph API-t kell használnia. A HTTP és HTTPS protokollt használó összes működik.

### <a name="extend-office-365-management-and-security-capabilities"></a>Office 365 felügyeleti és biztonsági lehetőségek bővítése
Már az Office 365-tel? Felgyorsítható a digitális átalakításában, a erőforrások az Azure AD toosecure beépített Office 365 képességeket kiterjesztésével biztonságos termelékenység engedélyezése a teljes munkaerő igényeire. Az Azure AD használatakor továbbá tooOffice 365 képességeit, biztonságossá teheti a teljes alkalmazás-portfóliót, amely lehetővé teszi, hogy egyszeri bejelentkezést az összes olyan alkalmazáshoz egy identitással. A feltételes hozzáférési képességeket, ne csak az eszköz állapotát, de a felhasználó helyét, az alkalmazás és a kockázatot is alapú bővítheti. Többtényezős hitelesítés (MFA) képességek még több védelmet nyújt, ha esetleg szükség lenne rá. További felügyelet felhasználói jogosultságokat kapnak lesz, és igény szerint közvetlenül az időponthoz kötött rendszergazdai hozzáférést biztosítanak. A felhasználó hatékonyabban dolgozhasson, és kevesebb segélyszolgálat hoznak létre, Köszönjük toohello önkiszolgálói képességeit az Azure AD biztosít, például a rendszer visszaállítja az Elfelejtett jelszó, alkalmazás-hozzáférési kérelmeket, és a csoportok létrehozása és kezelése.

> [!TIP]
> További információk az Azure AD identity management használatával és az Office 365 toolearn szeretné? [Hello e-könyv beolvasása](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-azure-identity-solutions"></a>A Microsoft Azure identitáskezelési megoldásairól

A Microsoft Azure kínál számos módon toomanage a felhasználók identitását, hogy azok megmaradnak teljes helyi, csak a felhő hello, vagy akár valahol a kettő között. Ezek a lehetőségek a következők: Azure, az Azure Active Directory (Azure AD), a hibrid identitás és a Azure AD tartományi szolgáltatások saját munka (DIY) AD DS.

### <a name="do-it-yourself-diy-ad-ds"></a>Saját munka (DIY) Active Directory tartományi szolgáltatások
A vállalatok számára, amelyeket csak kevés erőforrást hello felhőben **saját munka (DIY) Active Directory tartományi szolgáltatások** az Azure-ban is használható. Ez a beállítás szituációkat számos Windows Server Active Directory tartományi szolgáltatások, Azure virtuális gépek (VM) központi telepítés jól alkalmazható. Például egy Azure virtuális gép is létrehozhat, amely csatlakoztatott toohello távoli hálózat közösségihez adatközpontban fut tartományvezérlőként működik. Ott hello VM lenne képes toosupport a távoli felhasználók érkező hitelesítési kéréseket és növelheti a hitelesítés teljesítményét. Ez a beállítás akkor is jól alkalmazható, mert egy viszonylag alacsony költségű helyettesítő toootherwise költséges vész-helyreállítási helyek tartományvezérlők és az Azure-on egyetlen virtuális hálózat kis számú üzemeltetnie. Végül előfordulhat, hogy toodeploy egy alkalmazást az Azure, például a SharePoint, a Windows Server Active Directory tartományi szolgáltatások igényel, de nem függőségi rendelkezik hello a helyi hálózaton kell, vagy vállalati Windows Server Active Directory hello. Ebben az esetben telepíthet egy elkülönített erdő Azure toomeet hello SharePoint server farm követelményei. Kapcsolat toohello a helyszíni hálózat igénylő toodeploy hálózati alkalmazások is támogatott tartalmaz, és hello a helyszíni Active Directoryban.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
**Az Azure AD önálló** van egy teljes mértékben felhőalapú identitás és hozzáférés-kezelés szolgáltatás (IDaaS) megoldás. Az Azure AD lehetővé teszi egy robusztus képességek toomanage felhasználókat és csoportokat. Ennek segítségével biztonságos hozzáférés tooon helyszíni és felhőalapú alkalmazások, beleértve a Microsoft web szolgáltatást például Office 365, és sok nem Microsofttól származó szoftverek egy szolgáltatott szoftverként (SaaS) alkalmazások is. Az Azure AD három kiadásai származik: ingyenes, a Basic és a prémium szintű. Az Azure AD növekedhet szervezeti hatékonyságát és kiterjesztik túl hello szegélyhálózati tűzfal tooa új vezérlő vezérlősík védi az Azure machine learning és egyéb biztonsági speciális biztonsági szolgáltatásokat.

### <a name="hybrid-identity"></a>Hibrid identitás
Helyett válassza ki a helyszíni vagy felhőalapú identitás-megoldások között, számos előre-számbavétele igazgatók és vállalatok számára, amely a vállalat távon előrejelző megkezdődött, kiterjesztése a helyszíni címtárak toohello felhő keresztül**hibrid identitás** megoldásokat. Hibrid identitás, az valóban globális identitás kap, és hatékony és biztonságos hozzáférést biztosít az alkalmazások felhasználók toohello hozzáférés-kezelési megoldásnak kell toodo a munkájukat.

> [!TIP]
> További részletek toolearn hogyan igazgatók rendelkezik készült Azure Active Directory központi az IT-stratégiákat, töltse le a hello [CIO's Guide tooAzure Active Directory](https://aka.ms/AzureADCIOGuide).

### <a name="azure-ad-domain-services"></a>Azure AD Domain Services
**Azure AD tartományi szolgáltatások** biztosít a felhő alapú beállítás toouse AD DS egyszerűsített Azure virtuális gép konfigurációs vezérlő és egy módon toomeet helyszíni identitáskövetelmények hálózati alkalmazások fejlesztését és tesztelését. Azure AD tartományi szolgáltatások nem böngészésre toolift, és az eltolás mértékét megadó a helyszíni Active Directory tartományi szolgáltatások infrastruktúra tooAzure virtuális gépek Azure AD tartományi szolgáltatások által felügyelt. Ehelyett hello Azure virtuális gépek kezelt tartományokban kell használt toosupport hello fejlesztési, tesztelési és Active Directory tartományi szolgáltatások hitelesítési módszerek toohello felhő igénylő helyszíni alkalmazások mozgása.

## <a name="common-scenarios-and-recommendations"></a>Gyakori forgatókönyvek és javaslatok

Mert lehet, hogy az egyes leginkább megfelelő toowhich Azure-identitás beállítás az alábbiakban néhány gyakori identitások és hozzáférések forgatókönyvek a javaslatok.

|Identitás használatára| Ajánlás|
|-----|-----|
|A szervezet által végrehajtott nagy beruházások a helyi Windows Server Active Directory, de azt szeretnénk, ha tooextend identitás toohello felhő.| hello legszélesebb körben használt Azure-identitás megoldás [hibrid identitás](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Ha már végrehajtott beruházások a helyszíni Active Directory tartományi Szolgáltatásokban, könnyen kiterjesztheti identitás toohello felhőalapú Azure AD Connect használatával.|
|A saját üzleti született hello felhőben, és nem beruházások értékét kell a helyszíni identitáskezelési megoldások.| [Az Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hello legjobb választás, csak felhőalapú nem rendelkező vállalatok számára a helyszíni beruházások értékét.|
|Kis méretű Azure virtuális gép konfigurációs és vezérlés toomeet helyszíni identitáskövetelmények fejlesztés és tesztelés van szükség.|[Azure AD tartományi szolgáltatások](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) akkor hasznos, ha toouse Active Directory tartományi szolgáltatások egyszerűsített Azure virtuális gép konfigurációs vezérlő kell, vagy olyan eszközökre toodevelop vagy az örökölt, directory-t támogató helyszíni alkalmazások toohello felhő át.|  
|Néhány virtuális gépek Azure-ban kell toosupport, de a vállalatom továbbra is erősen fektetett a helyszíni Active Directory (AD DS).|Használjon [DIY Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/azure/jj156090.aspx) toouse Azure virtuális gépek toosupport kell néhány virtuális gépek és nagy Active Directory tartományi szolgáltatások beruházások helyszíni. |

## <a name="where-can-i-learn-more"></a>Hol kaphatok további?
Kell egy nagyszerű erőforrások online toohelp meg részletes tudnivalók az Azure AD tonna. Nagyszerű cikkek tooget használatba listája itt található:

* [A címtár az Azure AD Connect hibrid Management engedélyezése](active-directory-aadconnect.md)
* [Egy legalább egyszer világgal további biztonsági](../multi-factor-authentication/multi-factor-authentication.md)
* [Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához](active-directory-saas-app-provisioning.md)
* [Ismerkedés az Azure AD Reporting](active-directory-reporting-getting-started.md)
* [A jelszavak kezelése bárhonnan](active-directory-passwords.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához](active-directory-saas-app-provisioning.md)
* [Hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md)
* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Mi az a Microsoft Azure Active Directory licencelésének ismertetése](active-directory-licensing-what-is.md)
* [Hogyan lehet használt, jóvá nem hagyott felhőalkalmazások felderítése a szervezeten belül](active-directory-cloudappdiscovery-whatis.md)

## <a name="next-steps"></a>Következő lépések

Most, hogy megismerte az Azure-identitás fogalmakat és a rendelkezésre álló tooyou hello-beállítások, a következő erőforrások lépések tooget végrehajtási hello lehetőséget választotta hello is használhatja:

[További tudnivalók az Azure hibrid identitáskezelési megoldások](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)

[További információ a koncepció igazolása Azure környezetben:](https://aka.ms/aad-poc)

[Azure AD-t éles központi telepítése](https://aka.ms/aad-onboard)
