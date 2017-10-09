---
title: "az Active Directory tooAzure aaaHow alkalmazások felvétele."
description: "Ez a cikk ismerteti, hogyan alkalmazások felvétele az Azure Active Directory tooan példány."
services: active-directory
documentationcenter: 
author: shoatman
manager: kbrint
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 3ca710c58a403b52e8b728202ad9010f4873bcea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-and-why-applications-are-added-tooazure-ad"></a>Útmutató és érvek alkalmazások felvétele AD tooAzure
Egy hello kezdetben puzzling dolog, ha az alkalmazások listáját jeleníti meg az Azure Active Directory-példány az ismertetése, hello alkalmazások származási helyét, és hogy azok Miért vannak van.  Ez a cikk a magas szintű áttekintését hogyan alkalmazások hello könyvtárban vannak megadva, és a környezetet, amely segítséget nyújt a megértése, hogyan kérelmet kapott a címtárban toobe biztosítanak ad meg.

## <a name="what-services-does-azure-ad-provide-tooapplications"></a>Milyen szolgáltatásokat az Azure AD nyújt tooapplications?
Alkalmazások felvétele tooAzure AD tooleverage egy vagy több hello szolgáltatások biztosít.  Ezek a szolgáltatások a következők:

* Alkalmazás-hitelesítés és engedélyezés
* Felhasználói hitelesítés és engedélyezés
* Egyszeri bejelentkezés (SSO) összevonási vagy a jelszó használata
* A felhasználók átadása & szinkronizálás
* Szerepköralapú hozzáférés-vezérlés; Használjon hello directory toodefine alkalmazás szerepkörök tooperform szerepkörök alapú engedélyezési ellenőrzések alkalmazásban.
* oAuth-engedélyezési szolgáltatásokat (Office 365 és más Microsoft apps tooauthorize tooAPIs/erőforrások elérését által használt.)
* Alkalmazás közzététele & proxy; Egy magánhálózaton toohello az alkalmazás közzététele internet

## <a name="how-are-applications-represented-in-hello-directory"></a>Alkalmazások hogyan vannak megadva a hello directory?
Az Azure AD-objektumok 2 hello jelennek meg az alkalmazások: az alkalmazás objektum és a szolgáltatás egyszerű objektum.  Egy alkalmazás objektum, a regisztrált "otthoni" / "owner" vagy "közzétételének" könyvtárat és egy vagy több szolgáltatás egyszerű objektumokból hello alkalmazás minden könyvtárban, ahol működik.  

hello alkalmazásobjektum hello app tooAzure AD (hello több-bérlős szolgáltatást) ismerteti, és tartalmazhatnak hello következő: (*Megjegyzés*: Ez nem egy tárgyal minden releváns kérdést.)

* Nevét, emblémáját & Publisher
* A titkos kulcsokat (szimmetrikus vagy aszimmetrikus kulcsok használt tooauthenticate hello alkalmazás)
* API-függőségek (oAuth)
* API-k/erőforrások/hatókörök közzétett (oAuth)
* Alkalmazás-szerepkörök (RBAC)
* Egyszeri bejelentkezés metaadatai és (SSO)
* Felhasználók átadásához metaadatok és a konfiguráció
* Proxy metaadatai és

hello szolgáltatás egyszerű hello alkalmazás minden könyvtárban, ahol hello alkalmazás működik, beleértve a saját címtárukkal rekordja.  hello szolgáltatás egyszerű:

* Tooan alkalmazásobjektum hello app id tulajdonsága révén hivatkozik
* Rekordok helyi felhasználó és csoport alkalmazás-szerepkör-hozzárendelések
* Rekordok helyi felhasználói és rendszergazdai engedélyek toohello alkalmazás
  * Például: hello app tooaccess egy adott felhasználók e-mailt engedély
* Rögzíti a helyi házirendek többek között a feltételes hozzáférési házirend
* Rekordok helyi alternatív helyi beállítások alkalmazás
  * Jogcím átalakítási szabályok:
  * (A felhasználók átadása) attribútum-leképezésekhez
  * Adott alkalmazást szerepkörök bérlői (ha hello alkalmazás támogatja az egyéni szerepkörök)
  * Név vagy embléma

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Egy alkalmazás objektumának és könyvtárak között szolgáltatásnevekről ábrája
![Hogyan alkalmazás objektumokat, és az Azure AD-példányban meglévő rendszerbiztonsági tagok szolgáltatás ábrázoló diagram.][apps_service_principals_directory]

A fenti diagram hello látható.  A Microsoft fenntartja a két címtár belső (a hello balra) toopublish alkalmazásokat használ.

* Egy a Microsoft Apps (Microsoft-szolgáltatások könyvtára)
* Egy előre integrált 3. fél alkalmazások (Alkalmazásgyűjtemény könyvtár)

Alkalmazás közzétevők/szállítók, akik az Azure AD integrálása szükséges toohave közzétételi könyvtárban.  (Egyes Szolgáltatottszoftver-könyvtár).

Alkalmazások, amelyeket saját maga a következők:

* Alkalmazások kidolgozott (integrált AAD-ben)
* A csatlakoztatott alkalmazások single-sign-on
* Közzétett használó alkalmazások az Azure AD-alkalmazásproxy hello.

### <a name="a-couple-of-notes-and-exceptions"></a>Megjegyzések és kivételek néhány
* Nem minden szolgáltatás rendszerbiztonsági tagok hátsó tooapplication objektumok mutasson.  Huh? Az Azure AD eredetileg készült hello szolgáltatások rendelkezésre tooapplications korlátozottabb és hello szolgáltatás egyszerű alkalmazás identitás létrehozásához elegendő volt.  hello eredeti egyszerű szolgáltatásnév az alakzat toohello Windows Server Active Directory-szolgáltatásfiók szorosabb volt.  Ezen okból továbbra is lehetséges toocreate használatával szolgáltatásnevekről hello Azure AD PowerShell alkalmazásobjektum létrehozása nélkül.  hello Graph API app objektum szükséges egyszerű szolgáltatás létrehozása előtt.
* A fenti hello információk nem minden jelenleg kommunikál programozott módon.  hello következő hello felhasználói felület csak érhetők el:
  * Jogcím átalakítási szabályok:
  * (A felhasználók átadása) attribútum-leképezésekhez
* További részletes információk hello szolgáltatás egyszerű, és az alkalmazás objektumának tekintse meg a toohello Azure AD Graph REST API referenciadokumentációt.  *Mutató*: hello Azure AD Graph API-JÁNAK dokumentációja hello legközelebbi dolog tooa adatbázisséma hivatkozása az Azure AD, hogy jelenleg rendelkezésre áll.  
  * [Alkalmazás](https://msdn.microsoft.com/library/azure/dn151677.aspx)
  * [Egyszerű szolgáltatásnév](https://msdn.microsoft.com/library/azure/dn194452.aspx)

## <a name="how-are-apps-added-toomy-azure-ad-instance"></a>Hogyan alkalmazások toomy Azure AD-példányhoz hozzáadott?
Az alkalmazás felvehető tooAzure AD számos módja van:

* Hozzáadhat egy alkalmazást a hello [Azure Active Directory-Alkalmazásgyűjtemény](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Jelentkezzen be- / egy 3. fél alkalmazás Azure Active Directoryval integrált be (például: [Smartsheet](https://app.smartsheet.com/b/home) vagy [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Fel/felhasználók a bejelentkezés során a profilját és egyéb engedélyek vannak ismételt toogive engedély toohello app tooaccess.  hello első személy toogive hozzájárulás okok hello app toobe hozzáadott toohello directory képviselő szolgáltatásnevet.
* Fel/a Microsoft online services bejelentkezési, például [Office 365](http://products.office.com/)
  * Ha az előfizetett tooOffice 365 megkezdéséhez próba egy vagy több szolgáltatás rendszerbiztonsági tagok jönnek létre hello képviselő hello directory különböző szolgáltatások, amelyek az Office 365 társított összes hello funkció használt toodeliver.
  * Néhány Office 365-szolgáltatásokhoz, például a SharePoint szolgáltatás rendszerbiztonsági tagoknak egy folyamatos alapján tooallow közötti biztonságos kommunikációhoz összetevők többek között a munkafolyamatok hozható létre.
* Kidolgozása alkalmazás hozzáadása az Azure felügyeleti portálon lásd: hello: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Adja hozzá a Visual Studio használatával kidolgozása alkalmazás lásd:
  * [ASP.Net hitelesítési módszerek](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Kapcsolódó szolgáltatások](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Adja hozzá az alkalmazás toouse toouse hello [Azure AD alkalmazásproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Az egyszeri bejelentkezéshez a SAML-alapú vagy a jelszó SSO alkalmazás csatlakoztatása
* Sok más többek között a különböző fejlesztői lép az Azure-ban és a/az API explorer észlel fejlesztői erőforrások között

## <a name="who-has-permission-tooadd-applications-toomy-azure-ad-instance"></a>Kinek van engedélye tooadd alkalmazások toomy Azure AD-példányban?
Csak a globális rendszergazdák a következőket teheti:

* Hozzáadhat alkalmazásokat hello Azure AD-alkalmazásgyűjtemény (előre integrált 3. fél alkalmazások)
* Hello Azure AD alkalmazásproxy használó alkalmazások közzététele

A címtárban szereplő összes felhasználó jogok tooadd azok fejleszt alkalmazásokat és mely alkalmazások keresztül azok megosztás/adjon hozzáférést tootheir szervezeti adatok megítélése rendelkeznek.  *Ne feledje, a felhasználói bejelentkezési fel/tooan alkalmazást, és támogatást nyújtó engedélyek eredményezheti egy egyszerű szolgáltatásnév létrehozása folyamatban.*

Ez előfordulhat, hogy kezdetben hang vonatkozó, de tartsa hello szem előtt a következő:

* Alkalmazások képes a Windows Server Active Directory felhasználói hitelesítés anélkül, hogy a regisztrált/rögzített hello directory hello alkalmazás toobe sok éve tooleverage törölték.  Most hello szervezet fog nagyobb Láthatóság tooexactly hány alkalmazások hello könyvtár és what for használ.
* A rendszergazda által vezérelt alkalmazás közzétételi/regisztrációs folyamat nem szükséges.  Az Active Directory összevonási szolgáltatások volt valószínű, hogy egy rendszergazda volt tooadd egy alkalmazást, a függő entitások fejlesztők nevében.  A fejlesztők mostantól önkiszolgáló is.
* Be- / mentése tooapps üzleti célra használja a szervezet fiókok bejelentkező felhasználók azért hasznos.  Ha ezt követően adja hello szervezet elvesztik hozzáférési tootheir fiók melyikét használta éppen hello alkalmazásban.
* Feljegyzés az adatok megosztásának időpontját, amellyel alkalmazás azért hasznos rendelkezik.  Adatok minden eddiginél több hordozható, és hogy milyen adatokat osztanak ki mely alkalmazások törlése rekordja hasznos.
* Az Azure AD az oAuth használó alkalmazások döntse el, hogy pontosan milyen engedélyekkel, hogy felhasználók képes toogrant tooapplications és jogosultságokat igényel egy rendszergazda tooagree való.  Azt, hogy a csak a rendszergazdák toolarger hatókörök és jelentősebb engedélyeket is hozzájárulás értetődő kell lépjen.
* Felhasználók hozzáadásával és az adatok alkalmazások tooaccess naplózott események belül hello Azure Managmentet portál toodetermine hello naplózási jelentések megtekintéséhez, hogy hogyan az alkalmazások hozzá lett adva toohello könyvtár.

**Megjegyzés:** *magát a Microsoft operációs a következő sok hónapig hello alapértelmezett konfiguráció használatával.*

Az összes, hogy a címtárban alkalmazások hozzáadása és eltávolítása megítélése gyakorló milyen információkat az alkalmazások által hello Azure felügyeleti portál címtár konfigurációjához osztoznak keresztül lehetséges tooprevent felhasználók említett.  hello következő konfigurációs elérhető hello Azure felügyeleti portálon a címtár "Beállítása" lapon.

![Egy integrált alkalmazás-beállítások konfigurálása a felhasználói felület hello képernyőképe][app_settings]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
További tudnivalók tooadd alkalmazások tooAzure AD, és hogyan tooconfigure szolgáltatások alkalmazásokhoz.

* A fejlesztők: [további hogyan toointegrate egy alkalmazás AAD-ben](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* A fejlesztők: [tekintse át a Githubon Azure Active Directoryval integrált alkalmazások mintakód](https://github.com/AzureADSamples)
* A fejlesztők és informatikai szakemberek számára: [tekintse át az Azure Active Directory Graph API hello hello REST API dokumentációja](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* Az informatikusok: [megtudhatja, hogyan toouse Azure Active Directory előre integrált alkalmazások hello Alkalmazásgyűjtemény](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* Az informatikusok: [oktatóprogramjai meghatározott előre integrált alkalmazások konfigurálása](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* Az informatikusok: [ismerje meg, hogy egy alkalmazás használatával toopublish hello Azure Active Directory Alkalmazásproxyjával](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](../active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
