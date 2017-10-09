---
title: "aaaHow toouse egy Azure Active Direcory bérlői directory áttekintése |} Microsoft Docs"
description: "Ismerteti, mi az Azure AD-bérlő van, és hogyan toomanage Azure-ban az Azure Active Directory használatával"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.openlocfilehash: ddb16d89bf06a3753ed5106bd95162130ad51b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-azure-ad-directory"></a>Azure AD-címtár kezelése

## <a name="what-is-an-azure-ad-tenant"></a>Mi az az Azure AD-bérlő?
Az Azure Active Directoryban (Azure AD) a bérlő az Azure AD olyan dedikált példányának tekinthető, amelyet a szervezet megkap és a tulajdonában áll, amikor regisztrál egy Microsoft felhőszolgáltatásra, például az Azure-ra vagy az Office 365-re. Mindegyik Azure AD-címtár önálló, és el van választva a többi Azure AD-címtártól. Csakúgy, mint a vállalat irodaépülete van egy biztonságos eszköz adott tooonly a szervezet, az Azure AD-címtár is egy biztonságos eszköz kizárólag az adott szervezet általi használatra kialakított toobe. hello Azure AD architektúrájával elkülöníti az ügyfél- és identitásadatok, hogy a felhasználók és az Azure AD-címtár rendszergazdái véletlenül vagy szándékosan férhetnek hozzá más címtárak adataihoz.

![Az Azure Active-címtár kezelése](./media/active-directory-administer/aad_portals.png)

## <a name="how-can-i-get-an-azure-ad-directory"></a>Hogyan juthatok Azure AD-címtárhoz?
Az Azure AD hello core címtár- és identitáskezelési funkciókat biztosít a Microsoft felhőszolgáltatásai, beleértve a legtöbb mögött:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Azure AD-címtárhoz úgy juthat, ha regisztrál bármelyik fenti Microsoft felhőszolgáltatásra. Szükség szerint több címtárat is létrehozhat. Az első címtárat fenntarthatja például éles címtárként, majd létrehozhat egy másik címtárat is tesztelési vagy előkészítési céllal.

### <a name="using-hello-azure-ad-directory-that-comes-with-a-new-azure-subscription"></a>Egy új Azure-előfizetéshez mellékelt hello Azure AD-címtár segítségével

Azt javasoljuk, hogy használja-e hello rendszergazdai fiókot használta az első szolgáltatás Feliratkozás más Microsoft-szolgáltatásokhoz. hello adatok hello első alkalommal regisztrál Microsoft-szolgáltatást nem használt toocreate egy új Azure AD-címtárpéldányának a szervezet számára. Ha használ, hogy directory tooauthenticate bejelentkezési kísérletek tooother Microsoft szolgáltatások előfizetéskor, használhatnak a meglévő felhasználói fiókok, házirendek, beállítások vagy az alapértelmezett címtárban konfigurálja a helyszíni címtár-integráció hello.

Például ha bejelentkezési akár a Microsoft Intune-előfizetéshez tartozó, majd további szinkronizálja a helyszíni Active Directoryból az Azure AD-címtár, regisztráljon egy másik Microsoft-szolgáltatás például az Office 365, és könnyen elérése hello ugyanabban a könyvtárban a Microsoft Intune-nal rendelkező integrációs lehetőségeket.

További információk a helyszíni címtár Azure AD-integrációjáról: [Directory integration with Azure AD Connect](active-directory-aadconnect.md) (Címtár-integráció az Azure AD Connecttel).

### <a name="associate-an-existing-azure-ad-directory-with-a-new-azure-subscription"></a>Új Azure-előfizetés társítása meglévő Azure AD-címtárhoz
Új Azure-előfizetést társíthat hello bejelentkezés meglévő Office 365-öt vagy a Microsoft Intune-előfizetés hitelesítő címtár. A forgatókönyv további információkért lásd: [egy Azure-előfizetés tooanother fiók tulajdonjogának átruházása](../billing/billing-subscription-transfer.md)

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Azure AD-címtár létrehozása szervezeti feliratkozással egy Microsoft felhőszolgáltatásra
Ha még nem rendelkezik egy előfizetés tooa Microsoft felhőszolgáltatásra, hivatkozások toosign a következő hello egyikét használhatja. Amikor először iratkozik fel egy ilyen szolgáltatásra, az Azure AD-címtár automatikusan létrejön.

* [Microsoft Azure](https://account.azure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="how-toochange-hello-default-directory-for-a-subscription"></a>Hogyan toochange hello alapértelmezett címtár az előfizetéshez

1. Jelentkezzen be toohello [Azure Account Center](https://account.windowsazure.com/Home/Index) egy olyan fiókkal, amely hello hello előfizetés tootransfer előfizetés tulajdonjogának fiók rendszergazdájához.
2. Győződjön meg arról, hogy hello felhasználóját, aki toobe hello előfizetés tulajdonosa megcélzott hello a könyvtárban van.
3. Kattintson az **Előfizetés átadása** elemre.
4. Adja meg a hello címzett. hello címzett automatikusan kap egy e-maileket az elfogadási kapcsolaton.
5. hello címzett hello hivatkozásra kattint, és a következő hello utasításokat, beleértve a fizetési adatok megadása. Hello címzett sikeres, hello előfizetés kerül. 
6. hello alapértelmezett címtár hello előfizetés megváltozott sikeres hello előfizetés tulajdonjogának átadása akkor a felhasználó hello toohello könyvtárban.

### <a name="manage-hello-default-directory-in-azure"></a>Az Azure-ban hello alapértelmezett címtár kezelése
Amikor feliratkozik az Azure szolgáltatásra, a rendszer egy alapértelmezett Azure AD-címtárat társít az előfizetéséhez. Az Azure AD használata ingyenes, és a címtárak ingyenes erőforrásként használhatóak. Léteznek fizetős Azure AD szolgáltatások is, amelyek licencelése külön történik, és olyan további funkciókat biztosítanak, mint például a vállalati arculat megjelenítése a bejelentkezési felületen vagy az önkiszolgáló jelszó-visszaállítás. Saját DNS-név helyett hello alapértelmezett egyéni tartományt is létrehozhat *. onmicrosoft.com tartományt.

## <a name="how-can-i-manage-directory-data"></a>Hogyan történik a címtáradatok kezelése?
tooadminister egy vagy több Microsoft felhőszolgáltatás-előfizetés, használhatja a hello [az Azure AD felügyeleti központban](https://aad.portal.azure.com), a Microsoft Intune-fiókportál hello vagy hello [Office 365 felügyeleti központban](https://portal.office.com/) toomanage a szervezet directory adatait. Is [Azure Active Directory PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/active-directory) toohelp Azure AD-ben tárolt adatok kezelését.

E portálok (vagy parancsmagok) a következőket teszik lehetővé:

* Felhasználói és csoportfiókok létrehozása és kezelése
* A szervezet előfizetéséhez kapcsolódó felhőszolgáltatások felügyelete
* Helyszíni integráció beállítása az Azure AD identitás- és hitelesítési szolgáltatásaival

hello Azure AD felügyeleti központot, Office 365 felügyeleti központ, a Microsoft Intune-fiókportál és hello Azure AD-parancsmagok minden olvasni, és az Azure AD, a szervezet címtárához társított megosztott példány egyetlen tooa írása. Ezeknek az eszközöknek mindegyike a címtáradatok bekérésére vagy módosítására használt kezelőfelületként működik.

Ha módosítja a szervezeti adatok bármely hello portálok vagy adott szolgáltatások hello környezetében bejelentkezve-parancsmagok használatával, hello módosításokat is láthatók hello más portálok hello legközelebb bejelentkezik. Ezek az adatok által megosztott hello Microsoft cloud services toowhich regisztrációt.

Például használatakor hello Office 365 felügyeleti központban tooblock egy felhasználó bejelentkezésének, amely művelet blokkok hello tooany felhasználó más szolgáltatás toowhich a szervezet jelenleg előfizetője. Ha a hello ugyanazzal a fiókkal hello Microsoft Intune fiókportálon, is látható, hogy hello a felhasználó le van tiltva.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Hogyan vehetek fel és kezelhetek több címtárat?
Is [adja hozzá az Azure AD-címtár hello Azure-portálon](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Hello adatok, és válassza ki **létrehozása**.

Az egyes címtárakat teljesen független erőforrásként kezelheti: mindegyikük társüzemű, minden funkcióval ellátott és logikailag független a többi, Ön által kezelt címtártól, nincs közöttük szülő-gyermek típusú kapcsolat. Ez a függetlenség az erőforrás, a felügyelet és a szinkronizálás függetlenségét is jelenti.

* **Erőforrás-függetlenség**. Hoz létre, vagy pedig egy címtárban erőforrás törlése, ha található erőforrásokat egy másik címtárban, a külső felhasználók hello részleges kivétellel van hatással. Ha egy címtárat egy „contoso.com” egyedi tartománnyal használja, azt egyetlen másik címtárral sem használhatja.
* **Felügyeleti függetlenség**.  Ha a „Contoso” címtár valamely nem rendszergazda felhasználója létrehoz egy „Teszt” nevű tesztelési címtárat:
  
  * a "Contoso" címtár hello rendszergazdák nem lesznek közvetlen rendszergazdai jogosultságai toodirectory "Teszt" rendelkezik, kivéve, ha a "Teszt" rendszergazdája kifejezetten megadja számára ezeket a jogokat. A "Contoso" rendszergazdái szabályozhatja a hozzáférést toodirectory "Teszt" címtár hozzáférésének szabályozására hello felhasználói fiók létrehozása a "Teszt".
    
  * Ha egy rendszergazdai szerepkört a felhasználó pedig egy címtárban, hello módosítás eltávolítása nem befolyásolja a vonatkozó rendszergazdai szerepkörét vagy rendelhet hozzá, hogy a felhasználó rendelkezhet egy másik címtárban.
* **Szinkronizálási függetlenség**. Beállíthatja, hogy minden Azure AD bérlői egymástól függetlenül tooget származó adatok szinkronizálásához egy egypéldányos hello Azure AD Connect címtár-Szinkronizáló eszköz.

A többi Azure-erőforrástól eltérően a címtárak nem egy Azure-előfizetés alsóbb szintű erőforrásai. Ezért ha megszüntetését vagy az Azure-előfizetés tooexpire, továbbra is hozzáférhet a címtár adataihoz az Azure AD PowerShell, hello Azure Graph API vagy egyéb felületek például hello Office 365 felügyeleti központban. Hello Directory másik előfizetést is rendelhet.

## <a name="how-tooprepare-toodelete-an-azure-ad-directory"></a>Hogyan tooprepare toodelete az Azure AD-címtár
Egy globális rendszergazda törölheti az Azure AD-címtár hello portálról. Egy címtár törlésekor hello könyvtárban található összes erőforrást is törlődnek. Győződjön meg arról, hogy azt nem kell hello directory törlés előtt.

> [!NOTE]
> Ha hello felhasználó munkahelyi vagy iskolai fiókkal van bejelentkezve, hello felhasználói kell nem kell kísérlet toodelete saját címtárában. Például ha hello van bejelentkezett felhasználó szerint joe@contoso.onmicrosoft.com, nem törölheti megegyező contoso.onmicrosoft.com alapértelmezett tartományként hello könyvtárat.

Az Azure AD szükséges, hogy bizonyos feltételek fennállnak toodelete könyvtár-e. Ez csökkenti a kockázata, hogy egy könyvtár törlése negatívan befolyásolja a felhasználók vagy alkalmazások, például a felhasználók toosign a tooOffice 365 vagy az Azure-erőforrások eléréséhez hello képessége. Például ha egy könyvtárat az előfizetéshez akaratlanul töröltek, majd felhasználók nem férhetnek hozzá hello az adott előfizetéshez tartozó Azure-erőforrások.

a következő feltételek hello ellenőrzi:

* hello csak felhasználó hello címtárban kell hello globális rendszergazda, aki toodelete hello könyvtár. Más felhasználók hello címtár törlése előtt törölni kell. Ha a felhasználók szinkronizálása a helyszíni, akkor szinkronizálási ki kell kapcsolni, és hello felhasználók törölni kell a felhőalapú címtárban hello segítségével hello Azure-portálon vagy az Azure PowerShell-parancsmagokat. Nincs követelmény toodelete csoportok vagy névjegyek, például a hello Office 365 felügyeleti központban felvett névjegyek.
* Hello könyvtárban alkalmazás lehet. Minden alkalmazás hello címtár törlése előtt törölni kell.
* Nem a multi-factor authentication-szolgáltatók csatolt toohello könyvtár is lehet.
* Lehet például a Microsoft Azure, az Office 365, Microsoft Online Services-előfizetést, vagy Azure AD Premium társított hello könyvtár. Ha például az alapértelmezett címtár az Azure-ban lett létrehozva, nem törölheti azt mindaddig, amíg Azure-előfizetésének hitelesítése továbbra is ezen a címtáron alapul. Hasonlóképpen olyan címtárat sem törölhet, amelyhez egy másik felhasználó előfizetést társított. 


## <a name="next-steps"></a>Következő lépések
* [Azure AD fórum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
* [Azure Multi-Factor Authentication fórum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
* [Azure Stack Overflow-kérdések](http://stackoverflow.com/questions/tagged/azure)
* [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory)
* [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md)
