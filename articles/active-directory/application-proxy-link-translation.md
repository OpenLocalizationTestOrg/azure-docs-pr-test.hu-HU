---
title: "aaaTranslate hivatkozásokat és URL-címek Azure AD alkalmazás Proxy |} Microsoft Docs"
description: "Az Azure AD-alkalmazásproxy összekötők hello alapjairól ismerteti."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7ec2b9fb01617067cf5d676037877bf72c19217b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Az Azure AD alkalmazásproxy közzétett alkalmazásokhoz szoftveresen kötött hivatkozások átirányítása

Az Azure AD-alkalmazásproxy lehetővé teszi a helyszíni alkalmazások elérhető toousers távoli és a saját eszközeiken. Egyes alkalmazások esetében azonban fejlesztett hello HTML ágyazott helyi hivatkozásokkal. Ezek a hivatkozások nem működnek megfelelően hello app távolról szolgál. Ha egyszerre több helyszíni alkalmazások pont tooeach más, a felhasználók várható hello hivatkozások tookeep működik, ha azok nem hello irodában. 

hello legjobb módja toomake, hogy a hivatkozások működjenek hello azonos mind a vállalaton belüli és a vállalati hálózaton kívüli tooconfigure hello külső URL-címei a alkalmazások toobe hello azonos, a belső URL-címeit. Használjon [egyéni tartományok](active-directory-application-proxy-custom-domains.md) tooconfigure a külső URL-címek toohave a vállalati tartománynév hello alapértelmezett application proxy tartomány helyett.

A bérlő egyéni tartományok nem használhatók, ha az alkalmazásproxy hello hivatkozás fordítási szolgáltatása tartja a hivatkozásokat, függetlenül attól, hol találhatók a felhasználók működik. Ha alkalmazásokat, amelyek közvetlenül toointernal végpontok vagy a portok, leképezheti ezeket belső URL-címek toohello közzétett külső Proxy URL-címeket. Ha engedélyezve van a hivatkozás fordítása, és a közzétett belső hivatkozások használatával HTML, CSS, és válassza a JavaScript-címkék alkalmazásproxy történő. Majd hello alkalmazásproxy-szolgáltatás fordítja le azokat, hogy a felhasználók folyamatos élményt beolvasása.

>[!NOTE]
>hello hivatkozásokat fordítási funkcióját van a bérlők számára, hogy valamilyen okból nem használható egyéni tartományok toohave hello azonos belső és külső URL-címéből alkalmazásaikat. Ez a funkció engedélyezése előtt tekintse meg, ha [egyéni tartományok az Azure AD alkalmazásproxy](active-directory-application-proxy-custom-domains.md) akkor is képes működni.
>
>Vagy ha hello szükséges a hivatkozás fordítási tooconfigure alkalmazás SharePoint, tekintse meg a [másodlagos címek leképezése a SharePoint 2013 rendszerhez konfigurálása](https://technet.microsoft.com/library/cc263208.aspx) egy másik módszer toomapping hivatkozások.

## <a name="how-link-translation-works"></a>Hogyan hivatkozás fordítási működik

A hitelesítés után hello proxykiszolgáló fázisok hello Alkalmazásfelhasználó adatok toohello, a alkalmazásproxy hello application változtatható hivatkozások megvizsgálja, és lecseréli azokat a megfelelő külső URL-címek közzétett.

Alkalmazásproxy feltételezi, hogy az UTF-8 alkalmazások kódolt. Ha ez nem hello eset, adja meg hello kódolási típusát http-válaszfejléc, például `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Érintett kapcsolatokat?

hello hivatkozás fordítási funkció csak a kód címkék egy alkalmazás hello törzsében lévő hivatkozásokat keresi. Alkalmazásproxy rendelkezik egy külön szolgáltatással a cookie-k vagy URL-címek fordítása a fejlécekben. 

A helyszíni alkalmazások belső hivatkozások két közös típusa van:

- **Belső relatív hivatkozásokat** adott pont tooa megosztott erőforrás egy helyi fájl struktúra, például a `/claims/claims.html`. Ezek a hivatkozások automatikusan az alkalmazásokat proxyn keresztül történő közzétett, és vagy a hivatkozás fordítási nélkül toowork továbbra is működnek. 
- **Szoftveresen kötött belső hivatkozások** tooother a helyszíni alkalmazások, például a `http://expenses` vagy például a közzétett `http://expenses/logo.jpg`. hello hivatkozásokat fordítási funkcióját szoftveresen kötött belső hivatkozások működik, és módosítja azokat toopoint toohello külső URL-címekről a távoli felhasználók toogo keresztül.

### <a name="how-do-apps-link-tooeach-other"></a>Hogyan tegye hivatkozás tooeach más alkalmazások?

Hivatkozás fordítási minden alkalmazáshoz engedélyezve van, így befolyásolni hello felhasználói élmény hello alkalmazásonkénti szinten. Kapcsolja be az alkalmazás fordítása hivatkozásra, ha azt szeretné, hogy hello hivatkozások *a* adott alkalmazás toobe lefordítva, nem hivatkozások *való* alkalmazást. 

Tegyük fel, hogy rendelkezik-e, hogy az összes hivatkozás más tooeach proxyn keresztül történő közzétett három alkalmazásokról: előnyeit, a költségek és utazás. A negyedik alkalmazások visszajelzést, nem közzétett proxyn keresztül történő van.

Hivatkozás fordítási hello előnyöket alkalmazás engedélyezésekor hello hivatkozások tooExpenses és utazás átirányított toohello külső URL-címéből az alkalmazások, de hello hivatkozás tooFeedback nem irányítja át, mert nincsenek külső URL-CÍMÉT. Hivatkozások a költségeket és utazás hátsó tooBenefits nem működnek, mert a hivatkozás fordítási nincs engedélyezve ezen két alkalmazáshoz.

![Hivatkozások előnyöket tooother alkalmazásokból hivatkozás fordítási engedélyezésekor a rendszer](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Nem szerepelnek a kapcsolatokat?

tooimprove teljesítmény és biztonság, az egyes hivatkozások nem lefordítani:

- Hivatkozások nem kód címkék belül. 
- Hivatkozások nem HTML, CSS vagy JavaScript. 
- Belső hivatkozások megnyitása más. E-mail vagy azonnali üzenet keresztül küldött, vagy más dokumentumban szereplő hivatkozások nem fordítható le. hello felhasználók tooknow toogo toohello külső URL-cím szükséges.

Ha ezek helyzetek egyikében toosupport van szüksége, használja hello azonos belső és külső URL-címek hivatkozás fordítási helyett.  

## <a name="enable-link-translation"></a>Hivatkozás fordítási engedélyezése

Első lépések hivatkozásra fordítási a lehető legkönnyebben egy gombra való kattintás:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Nyissa meg túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** > Válassza hello-alkalmazáshoz toomanage > **Alkalmazásproxy**.
3. Kapcsolja be **URL-címek fordítása a kérelem törzsében** túl**Igen**.

   ![A kérelem törzsében Igen tootranslate URL-címek kiválasztása](./media/application-proxy-link-translation/select_yes.png).
4. Válassza ki **mentése** tooapply a módosításokat.

Most amikor a felhasználók hozzáférése az alkalmazáshoz, hello proxy automatikusan keresni kezdi a bérlő a proxyn keresztül történő közzétett belső URL-címek.

## <a name="send-feedback"></a>Visszajelzés küldése

Azt szeretnénk, a Súgó toomake Ez a szolgáltatás a munkahelyi az alkalmazásokhoz. Azt több mint 30 címkék keresse meg a HTML-vagy CSS és mely JavaScript esetekben toosupport készül. Ha rendelkezik, amelyek nem szerepelnek alatt létrehozott hivatkozások példát, küldjön egy kódrészletet túl[Application Proxy visszajelzés](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Következő lépések
[Egyéni tartományok használata az Azure AD-alkalmazásproxy](active-directory-application-proxy-custom-domains.md) toohave hello azonos belső és külső URL-címe

[Konfigurálja a másodlagos címek leképezése a SharePoint 2013 rendszerhez](https://technet.microsoft.com/library/cc263208.aspx)
