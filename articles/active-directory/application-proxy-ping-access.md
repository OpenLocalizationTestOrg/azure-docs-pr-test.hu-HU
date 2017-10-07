---
title: "aaaHeader-alapú hitelesítés PingAccess az az Azure AD alkalmazásproxy |} Microsoft Docs"
description: "Alkalmazások közzététele az PingAccess és alkalmazás Proxy toosupport fejléc-alapú hitelesítés."
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
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 38fe3e7a41a71f4ae6c75f014e44c722f773bd22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Az egyszeri bejelentkezés az alkalmazásproxy és PingAccess fejléc-alapú hitelesítés

Az Azure Active Directory Alkalmazásproxyjával és PingAccess közösen együtt tooprovide Azure Active Directory felhasználók hozzáférési tooeven további alkalmazások. PingAccess bővíti hello [meglévő alkalmazásproxy ajánlatok](active-directory-application-proxy-get-started.md) tooinclude egyszeri bejelentkezéses hozzáférést tooapplications, fejlécek használata a hitelesítéshez.

## <a name="what-is-pingaccess-for-azure-ad"></a>Mi az az PingAccess az Azure AD?

Az Azure Active Directory PingAccess egy ajánlat, amely lehetővé teszi a toogive felhasználók hozzáférést PingAccess és egyszeri bejelentkezés tooapplications, fejlécek használata a hitelesítéshez. Alkalmazásproxy ezekhez az alkalmazásokhoz, mint bármely más, az Azure AD tooauthenticate hozzáféréssel, és a majd áthaladó forgalmat keresztül hello összekötő szolgáltatás kezeli. PingAccess hello alkalmazások előtt helyezkedik el, és hello hozzáférési jogkivonatot az Azure AD fordítja fejléc, hogy hello alkalmazás hello hitelesítési kap az elolvashatják hello formátumban.

A felhasználók nem bármilyen figyelje, amikor bejelentkeznek a toouse a vállalati alkalmazásokat. Akkor is még mindig bárhonnan dolgozhatnak bármilyen eszközön. 

Hello alkalmazásproxy összekötők távoli forgalom tooall alkalmazásoknak a hitelesítési típus közvetlen, mivel azok tooload egyenleg automatikusan, illetve fogja folytatni.

## <a name="how-do-i-get-access"></a>Hogyan szerezhetek hozzáférést?

Mivel ebben a forgatókönyvben az Azure Active Directory és PingAccess közötti partnerség kínált, licencek kell mindkét szolgáltatás esetében. Azonban az Azure Active Directory Premium előfizetéshez tartozik egy PingAccess alaplicenc, amely lefedi too20 alkalmazások fel. Ha több mint 20 fejléc-alapú alkalmazások kell toopublish, PingAccess vásárolhat további licencre. 

További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).

## <a name="publish-your-application-in-azure"></a>Az alkalmazás közzététele az Azure-ban

Ez a cikk tesznek közzé egy alkalmazást ebben a forgatókönyvben hello először személyek számára készült. Az végigvezeti hogyan tooget el az alkalmazás- és PingAccess, továbbá toohello közzétételi lépéseket. Ha már beállított mindkét szolgáltatás, de a lépések közzétételi hello frissítő, hello összekötő telepítése, és helyezze át, amely túl[adja hozzá az alkalmazás tooAzure AD az alkalmazásproxy](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Mivel ez a forgatókönyv az Azure AD közötti partnerség és PingAccess, bizonyos hello utasítások találhatók a hello Ping identitás hely.

### <a name="install-an-application-proxy-connector"></a>Az alkalmazásproxy-összekötő telepítése

Ha már Application Proxy engedélyezve van, és egy összekötő telepítve van, hagyja ki ezt a szakaszt, és helyezze át, amely túl[adja hozzá az alkalmazás tooAzure AD az alkalmazásproxy](#add-your-app-to-azure-ad-with-application-proxy).

hello alkalmazásproxy-összekötő egy Windows Server szolgáltatás, amely arra utasítja a távoli alkalmazottak tooyour hello forgalmát közzétett alkalmazást. Telepítési utasításokat, lásd: [alkalmazásproxy engedélyezése az Azure portál hello](active-directory-application-proxy-enable.md).

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.
2. Válassza ki **Azure Active Directory** > **alkalmazásproxy**.
3. Válassza ki **összekötő letöltése** toostart hello Application Proxy connector download. Kövesse a hello telepítési utasításokat.

   ![Alkalmazásproxy engedélyezése és hello összekötő letöltése](./media/application-proxy-ping-access/install-connector.png)

4. Hello összekötő letöltése kell automatikusan alkalmazásproxy engedélyezése a címtáron, de ha nem választhat **alkalmazásproxy engedélyezése**.


### <a name="add-your-app-tooazure-ad-with-application-proxy"></a>Adja hozzá az alkalmazás tooAzure AD-alkalmazásproxyval

Nincsenek két műveletet kell tootake a hello Azure-portálon. Először toopublish az alkalmazás az alkalmazásproxy. Ezt követően kell toocollect hello app hello PingAccess lépések során használható kapcsolatos információkat.

Kövesse ezeket a lépéseket toopublish az alkalmazást. A részletes lépéseket bemutató 1-8, tekintse meg a még [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](application-proxy-publish-azure-portal.md).

1. Ha nem hello utolsó szakaszában, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.
2. Válassza ki **Azure Active Directory** > **vállalati alkalmazások**.
3. Válassza ki **Hozzáadás** hello panel hello tetején.
4. Válassza ki **helyszíni alkalmazás**.
5. Töltse ki az új alkalmazás adatait tartalmazó hello kötelező mezőket. Útmutató a hello-beállítások a következő hello használata:
   - **Belső URL-cím**: általában hello URL-címet, amely toohello app bejelentkezési oldal Ha hello a vállalati hálózaton már megadta. Ebben a forgatókönyvben hello összekötő tootreat hello PingAccess proxy hello alkalmazás hello első lapként van szüksége. Ezt a formátumot használja: `https://<host name of your PA server>:<port>`. hello port 3000 alapértelmezés szerint, de a PingAccess konfigurálhatja.
   - **Előhitelesítési módszer**: Azure Active Directoryban
   - **URL-címet a fejlécekben fordítása**: nincs

   >[!NOTE]
   >Ha ez az első alkalmazás, port 3000 toostart használja, és térjen vissza tooupdate ezt a beállítást, ha az PingAccess konfigurációjának módosítását. Ha ez a második vagy újabb alkalmazáshoz, ez kell toomatch hello figyelő PingAccess konfigurálta. További információ [PingAccess a figyelők](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Válassza ki **Hozzáadás** hello hello panel alsó részén. Az alkalmazás kerül, és hello quick start menü megnyitása.
7. Hello gyors üzembe helyezési menüben válasszon ki **rendelje hozzá a felhasználót tesztelési**, és legalább egy felhasználói toohello alkalmazás hozzáadása. Ellenőrizze, hogy a teszt fiók rendelkezik hozzáféréssel toohello a helyszíni alkalmazások.
8. Válassza ki **hozzárendelése** toosave hello teszt felhasználó hozzárendelése.
9. Hello felügyeleti panelen jelölje ki **egyszeri bejelentkezés**.
10. Válasszon **fejléc-alapú bejelentkezés** hello legördülő menüből. Kattintson a **Mentés** gombra.

   >[!TIP]
   >Ha ez az első alkalommal használja a fejléc-alapú egyszeri bejelentkezést, tooinstall PingAccess kell. toomake meg arról, hogy az Azure-előfizetéshez tartozik automatikusan az PingAccess telepítése, az egyszeri bejelentkezési oldalra toodownload PingAccess a hello kapcsolat használata. Most nyissa meg a letöltési hely hello, vagy visszatérhet, toothis lap később. 

   ![Válassza ki a fejléc-alapú bejelentkezés](./media/application-proxy-ping-access/sso-header.PNG)

11. Hello vállalati alkalmazások panel bezárása, vagy minden hello módon toohello bal oldali tooreturn toohello Azure Active Directory menü görgessen.
12. Válassza ki **App regisztrációk**.

   ![Válassza ki az alkalmazás-regisztráció](./media/application-proxy-ping-access/app-registrations.png)

13. Az előzőekben adott hozzá, majd jelölje be hello app **válasz URL-címek**.

   ![Válassza ki a válasz URL-címek](./media/application-proxy-ping-access/reply-urls.png)

14. Ellenőrizze a toosee, ha hello külső URL-címe, hogy hozzárendelt tooyour alkalmazást az 5 hello válasz URL-címei listáján. Ha nem, vegye fel azt.
15. A hello alkalmazás-beállítások panelen válassza ki a **szükséges engedélyek**.

  ![Válassza ki a szükséges engedélyek](./media/application-proxy-ping-access/required-permissions.png)

16. Válassza a **Hozzáadás** lehetőséget. Hello API, válasszon **Windows Azure Active Directory**, majd **válasszon**. Hello engedélyek, válassza **olvasási és írása az összes alkalmazás** és **jelentkezzen be a felhasználói profil olvasása és**, majd **válassza ki** és **végzett**.  

  ![Engedélyek kiválasztása](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-hello-pingaccess-steps"></a>Információgyűjtés a hello PingAccess lépéseket

1. Válassza a beállítások panelen **tulajdonságok**. 

  ![Válassza a Tulajdonságok](./media/application-proxy-ping-access/properties.png)

2. Mentse a hello **alkalmazásazonosító** érték. Ügyfél-azonosító hello szolgál PingAccess konfigurálásakor.
3. A hello alkalmazás-beállítások panelen válassza ki a **kulcsok**.

  ![Válassza ki a kulcsok](./media/application-proxy-ping-access/Keys.png)

4. Hozzon létre egy kulcsot a fő leírás, majd válassza a lejárati dátum hello legördülő menüből.
5. Kattintson a **Mentés** gombra. Megjelenik egy GUID hello **érték** mező.

  Mentse ezt az értéket, nem fogja tudni toosee az után megismételni az ablak bezárása.

  ![Hozzon létre egy új kulcsot](./media/application-proxy-ping-access/create-keys.png)

6. Hello App regisztrációk panel bezárása, vagy minden hello módon toohello bal oldali tooreturn toohello Azure Active Directory menü görgessen.
7. Válassza ki **tulajdonságok**.
8. Mentse a hello **könyvtár-azonosítója** GUID.

### <a name="optional---update-graphapi-toosend-custom-fields"></a>Nem kötelező – a frissítés GraphAPI toosend egyéni mezők

A biztonsági jogkivonatok, amelyek az Azure AD küld a hitelesítéshez listájáért lásd: [az Azure AD-jogkivonatok referenciájából](./develop/active-directory-token-and-claims.md). Egy egyéni jogcím, amelyet más tokenekbe küld van szüksége, mezőjét GraphAPI tooset hello app *acceptMappedClaims* túl**igaz**. Azure AD Graph Explorer vagy a MS Graph toomake használhatja ezt a konfigurációt. 

Ez a példa Graph Explorer:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a>Töltse le a PingAccess, és állítsa be alkalmazását

Most, hogy befejezte az összes hello Azure Active Directory telepítési lépéseinek tooconfiguring PingAccess továbbléphet. 

hello részletes, lépésenkénti leírását a forgatókönyv részét PingAccess hello folytatását hello Ping identitáskezelési dokumentáció, [PingAccess konfigurálása az Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Ezek a lépések végigvezetik hello folyamat első egy PingAccess fiókot, ha még nem rendelkezik egy, a hello PingAccess kiszolgáló telepítése és a hello hello Azure-portálon fájlból másolt könyvtár-azonosítója az Azure AD OIDC szolgáltató kapcsolat létrehozása. Ezt követően hello alkalmazás Azonosítóját és kulcsát értékek toocreate a webes munkamenet használja PingAccess. Ezt követően Identitásleképezés beállítása, és hozzon létre egy virtuális gazdagép, a hely és az alkalmazás.

### <a name="test-your-app"></a>Az alkalmazás tesztelése

Ha végrehajtotta ezeket a lépéseket, az alkalmazás működik, és kell lennie. tootest, nyisson meg egy böngészőt, és keresse meg a toohello külső URL-címet, amelyet közzé a hello alkalmazást az Azure-ban hozott létre. Bejelentkezés hello fiókot a hozzárendelt toohello alkalmazást.

## <a name="next-steps"></a>Következő lépések

- [Az Azure AD PingAccess konfigurálása](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Hogyan nyújt az Azure AD-alkalmazásproxy egyszeri bejelentkezéshez?](application-proxy-sso-overview.md)
- [Alkalmazásproxy hibaelhárítása](active-directory-application-proxy-troubleshoot.md)
