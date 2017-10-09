---
title: "aaaManage hozzáférés toocloud alkalmazások bérlők - Azure korlátozásával |} Microsoft Docs"
description: "Hogyan toouse bérlői korlátozások toomanage mely felhasználók úgy férhetnek hozzá az alkalmazásokhoz az Azure AD-bérlő alapján."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: kgremban
ms.openlocfilehash: 6470fa217738b29104353ae17a2f53216f825c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-tenant-restrictions-toomanage-access-toosaas-cloud-applications"></a>Használja a bérlői korlátozások toomanage tooSaaS felhőalapú alkalmazásokat

Nagy méretű szervezeteknek, amelyek biztonsági emelje ki szeretné toomove toocloud szolgáltatások Office 365-höz hasonló, de a felhasználók számára elérhető csak szükség tooknow jóváhagyott erőforrások. Hagyományosan a vállalatok korlátozása tartománynév vagy IP-címek automatikusan toomanage hozzáférés kéri. Ez a megközelítés a Szolgáltatottszoftver-alkalmazásoknál a rendszer hol tárolja a nyilvános felhőben, például outlook.office.com és login.microsoftonline.com megosztott tartománynevek futó világ sikertelen lesz. Ezeknél a címeknél blokkolja szeretné, hogy a felhasználók hozzáférését az Outlook Web hello teljesen, csupán azokat tooapproved identitások és korlátozása erőforrások helyett.

Az Azure Active Directory megoldás toothis challenge bérlői korlátozások nevezett szolgáltatással. Bérlői korlátozások lehetővé teszi, hogy a szervezetek toocontrol hozzáférés tooSaaS felhőalapú alkalmazásokhoz, hello Azure AD bérlő hello alkalmazások használata az egyszeri bejelentkezés alapján. Például érdemes lehet tooallow hozzáférés tooyour szervezet Office 365-alkalmazásokhoz, közben megakadályozza az access tooother szervezetek példányok azonos alkalmazást.  

Bérlői korlátozások szervezetek hello képességét toospecify hello listát, hogy a felhasználók jogosultak tooaccess bérlők ad. Az Azure AD majd csak biztosít hozzáférést engedélyezett toothese bérlők.

Ez a cikk az Office 365 bérlői korlátozásokkal foglalkozik, de hello szolgáltatás bármely modern hitelesítési protokollok megvalósítását végzi az Azure ad-val egyszeri bejelentkezést használó SaaS felhőalkalmazás együtt kell működnie. Ha használja az Office 365 által használt hello bérlő bérlői alkalmazásokat egy másik Azure ad SaaS, győződjön meg arról, hogy minden szükséges bérlők engedélyezettek. A cloud SaaS-alkalmazások kapcsolatos további információkért lásd: hello [Active Directory Marketplace](https://azure.microsoft.com/en-us/marketplace/active-directory/).

## <a name="how-it-works"></a>Működés

hello a teljes megoldás magában foglalja a következő összetevők hello: 

1. **Az Azure AD** – Ha hello `Restrict-Access-To-Tenants: <permitted tenant list>` található, az Azure AD csak problémák biztonsági jogkivonatainak hello bérlők számára engedélyezett. 

2. **A helyszíni proxy kiszolgálói infrastruktúra** – SSL-ellenőrzést, hello listáját tartalmazó konfigurált tooinsert hello fejléc képes proxy eszköz engedélyezett bérlők adatforgalmat az Azure AD-be. 

3. **Ügyfélszoftver** – toosupport bérlői korlátozások, ügyfélszoftvert kell igényelnie jogkivonatok közvetlenül az Azure AD-ről, hogy a forgalom elfoghatják hello proxy infrastruktúra. Bérlői korlátozások jelenleg támogatott webböngésző-alapú Office 365-alkalmazásokat és az Office-ügyfelek (például az OAuth 2.0-s) a modern hitelesítés használatakor. 

4. **A modern hitelesítés** – felhőszolgáltatások kell használnia a modern hitelesítést toouse bérlői korlátozások és a hozzáférés letiltása az tooall-bérlők számára nem engedélyezett. Office 365 felhőszolgáltatások kell alapértelmezés szerint beállított toouse modern hitelesítési protokollok megvalósítását végzi. Hello legfrissebb információk az Office 365 modern hitelesítés támogatása [frissített Office 365 modern hitelesítést](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

a következő diagram hello hello magas szintű adatforgalmat mutatja be. Forgalom tooAzure Active Directory, nem toohello Office 365 felhőszolgáltatások csak szükséges SSL-ellenőrzést. Ezt a különbséget fontos, mert a hitelesítési tooAzure AD hello adatforgalma általában sokkal alacsonyabb, mint a forgalom kötet tooSaaS alkalmazások, például az Exchange Online és SharePoint Online.

![A bérlői forgalom áramlását korlátozások – ábra](./media/active-directory-tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Bérlői korlátozások beállítása

Nincsenek két lépéseket tooget bérlői korlátozások használatába. hello első lépéseként meg arról, hogy az ügyfelek kapcsolódhatnak-e toohello jobb címek toomake. hello második van tooconfigure proxy infrastruktúráját.

### <a name="urls-and-ip-addresses"></a>URL-címei és IP-címek

toouse bérlői korlátozások, az ügyfelek képesek tooconnect toohello követően az Azure AD URL-címek tooauthenticate kell lennie: login.microsoftonline.com login.microsoft.com és login.windows.net. Emellett tooaccess Office 365, az ügyfelek is kell tudni tooconnect toohello FQDN vagy URL-címek és IP-címek meghatározott [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxy konfigurációs és követelményei

a következő konfigurációs hello szükség a bérlői korlátozások a proxy-infrastruktúrán keresztül tooenable. Ez az útmutató nem általános, ezért a megadott megvalósítási lépéseket tooyour proxy gyártójának dokumentációját olvassa el.

#### <a name="prerequisites"></a>Előfeltételek

- hello proxy képes tooperform SSL-hozzáférés, a HTTP-fejléc beszúrási és a szűrő célok FQDN vagy URL-címekkel kell lennie. 

- Az ügyfelek SSL-kommunikációhoz hello proxy által bemutatott hello tanúsítványlánc megbízhatónak kell lennie. Például ha egy belső használatú PKI-tanúsítványokat használ, hello belső kibocsátó legfelső szintű tanúsítvány hitelesítésszolgáltatói tanúsítványt megbízhatónak kell lennie.

- Az Office 365-előfizetés tartalmazza ezt a szolgáltatást, de ha azt szeretné, hogy toouse bérlői korlátozások toocontrol hozzáférés tooother SaaS-alkalmazásokhoz szükség-e majd Azure AD Premium 1 licenceket.

#### <a name="configuration"></a>Konfiguráció

Az egyes bejövő kérelmek toologin.microsoftonline.com login.microsoft.com és login.windows.net, helyezze be a két HTTP-fejlécek: *korlátozása-Access-az-bérlők* és *korlátozása-Access-környezet*.

hello fejlécek hello a következő elemeket kell tartalmaznia: 
- A *korlátozása-Access-az-bérlők*, érték \<bérlői lista megengedett\>, ez az egy vesszővel tagolt listáját a bérlő tooallow felhasználók tooaccess szeretné. Bármely tartomány, amely regisztrálva van a bérlők lehet használt tooidentify hello bérlői ezen a listán. Például toopermit tooboth Contoso és Fabrikam bérlők hozzáférni, név-érték pár tűnik hello:`Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- A *korlátozása-Access-környezet*, egyetlen Azonosítóra, mely bérlői van korlátozásokkal hello bérlői deklaráló érték. Például Contoso, hello bérlő bérlői korlátozások házirend hello beállított toodeclare, hello név-érték pár néz ki:`Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> A könyvtár-azonosító található a hello [Azure-portálon](https://portal.azure.com). Jelentkezzen be rendszergazdaként, válassza **Azure Active Directory**, majd jelölje be **tulajdonságok**.

a saját HTTP-fejléc beszúrni a nem jóváhagyott bérlőkkel tooprevent felhasználóit, hello proxy kell tooreplace hello korlátozása-Access-az-bérlők fejléc Ha már létezik a bejövő kérelem hello. 

Ügyfelek kényszerített toouse hello proxy összes kérelmek toologin.microsoftonline.com, login.microsoft.com és login.windows.net kell lennie. Például ha a PAC-fájlokat használt toodirect ügyfelek toouse hello proxy, a végfelhasználók nem szabad képes tooedit vagy tiltsa le a hello PAC-fájlokat.

## <a name="hello-user-experience"></a>hello felhasználói élmény

Ez a szakasz ismerteti a végfelhasználók és rendszergazdák hello élményének.

### <a name="end-user-experience"></a>Végfelhasználói élmény

Egy példa felhasználó hello Contoso hálózatán lévő, de tooaccess hello Fabrikam példányát egy megosztott SaaS-alkalmazáshoz például az Outlookhoz online próbál. Ha a Contoso egy nem megengedett bérlőt annak a példánynak, hello felhasználónál a következő lap hello:

![Oldalt a bérlők számára nem engedélyezett a felhasználók a hozzáférés megtagadva](./media/active-directory-tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Rendszergazdai feladatok

Amíg a bérlő korlátozások konfigurálást hello vállalati proxy infrastruktúrán, rendszergazdák férhetnek hozzá hello bérlői korlátozások jelentéseket hello Azure-portálon közvetlenül. tooview hello azt jelenti, lépjen a toohello Azure Active Directory – áttekintés oldalra, majd keresse meg a többi képességeket.

Üdvözöljük a rendszergazdákat hello bérlő számára megadott hello korlátozott-Access-környezet bérlői használható ez a jelentés toosee hello miatt blokkolva összes bejelentkezések bérlői korlátozások házirend, beleértve hello identitását, és hello célkönyvtár a forráskönyvtárban azonosítóját.

![Korlátozza az Azure portál tooview bejelentkezési kísérletek hello használata](./media/active-directory-tenant-restrictions/portal-report.png)

Más hello Azure-portálon jelentései, például a szűrők toospecify hello hatókör a jelentés is használhatja. Egy adott felhasználó, alkalmazás, ügyfél vagy alatt az időtartam alatt jeleníthetők meg.

## <a name="office-365-support"></a>Az Office 365-támogatás

Office 365-alkalmazások két feltétel toofully támogatási bérlői korlátozásokat kell megfelelnie:

1. használt hello ügyfél támogatja a modern hitelesítés
2. Hello alapértelmezett hitelesítési protokoll hello felhőalapú szolgáltatás engedélyezve van a modern hitelesítést.

Tekintse meg a túl[frissített Office 365 modern hitelesítést](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) hello mely Office ügyfelek jelenleg támogatja a modern hitelesítés legújabb információkat. Ez az oldal is tartalmaz hivatkozásokat tooinstructions engedélyezésére adott Exchange Online és Skype vállalati Online bérlőkhöz modern hitelesítést. A modern hitelesítés alapértelmezés szerint a SharePoint online-hoz már engedélyezve van.

Bérlői korlátozások jelenleg az Office 365 webböngésző-alapú alkalmazások támogatják (Office-portál Yammer, SharePoint hello webhelyek, az Outlook hello Web, stb.). Vastag ügyfelek (Outlook, a Skype vállalati, a Word, Excel, PowerPoint, stb.) Bérlői korlátozások csak kényszerítheti a modern hitelesítés használatakor.  

Outlook és Skype a modern hitelesítést támogató üzleti ügyfelek továbbra is képes toouse örökölt protokollok bérlők ellen, ahol nincs engedélyezve a modern hitelesítést, hatékonyan kihagyásával bérlői korlátozások. Az Outlook, a Windows-ügyfelek meggátolja, hogy a végfelhasználók számára nem engedélyezett e-mail fiókok tootheir profilok hozzáadása tooimplement korlátozások is dönthet. Lásd például: hello [tiltása nem alapértelmezett Exchange-fiókok hozzáadása](http://gpsearch.azurewebsites.net/default.aspx?ref=1) csoportházirend-beállítás. Az Outlook, a nem Windows platformokon, és a Skype vállalati minden platformon bérlő korlátozások teljes körű támogatása jelenleg nem áll rendelkezésre.

## <a name="testing"></a>Tesztelés

Ha bérlői korlátozásokat tootry előtt bevezetné, a teljes szervezet számára, vannak-e két lehetőség közül választhat: egy állomásalapú megközelítés Fiddler, vagy a proxybeállítások egy előkészített bevezetésének hasonló eszköz használatával.

### <a name="fiddler-for-a-host-based-approach"></a>A fiddler állomásalapú megközelítésre

Fiddler egy ingyenes webes proxy használt toocapture és módosíthatja a HTTP/HTTPS-forgalmat, beleértve a HTTP-fejlécek beszúrása hibakeresés. tooconfigure Fiddler tootest bérlői korlátozások, hajtsa végre a lépéseket követve hello:

1.  [Töltse le és telepítse a Fiddler](http://www.telerik.com/fiddler).
2.  Konfigurálja a Fiddler toodecrypt HTTPS-forgalmat, egy [Fiddler tartozó súgót](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Konfigurálja a Fiddler tooinsert hello *korlátozása-Access-az-bérlők* és *korlátozása-Access-környezet* fejlécek egyéni szabályok használatával:
  1. A hello Fiddler webes Debugger eszköz, válassza ki a hello **szabályok** menüre, majd válassza **testreszabása szabályok...** tooopen hello CustomRules fájlt.
  2. Adja hozzá az alábbi hello hello elején hello *OnBeforeRequest* függvény. Cserélje le \<bérlői tartomány\> tartománnyal rendelkező regisztrálva a bérlő például contoso.onmicrosoft.com. Cserélje le \<könyvtár-azonosítója\> a bérlő Azure AD GUID azonosítóval.

  ```
  if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
  ```

  Ha több bérlő kell tooallow, használja az egy vesszővel tooseparate hello tenant nevek. Példa:

  ```
  oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
  ```

4. Mentse és zárja be hello CustomRules fájlt.

Miután konfigurálta a Fiddler, rögzítheti a forgalmat fog toohello által **fájl** menüben, majd válassza **forgalom rögzítése**.

### <a name="staged-rollout-of-proxy-settings"></a>A proxybeállítások előkészített bevezetésének

Attól függően, hogy a proxy infrastruktúra hello képességeit a beállítások tooyour felhasználók képes toostage hello bevezetésének is. Az alábbiakban a megfontolásra néhány általános beállítások:

1.  PAC használata fájlok toopoint felhasználók tooa teszt proxy infrastruktúra, míg a normál felhasználók toouse hello éles proxy infrastruktúra végrehajtja.
2.  Néhány proxykiszolgáló a csoportok különböző konfigurációt is támogatja.

Tekintse meg a tooyour proxy server dokumentációjában talál információt.

## <a name="next-steps"></a>Következő lépések

- További információ a [frissített Office 365 modern hitelesítést](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Felülvizsgálati hello [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
