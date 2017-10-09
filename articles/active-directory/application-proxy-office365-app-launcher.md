---
title: "közzétett alkalmazások az Azure AD-alkalmazásproxy használatával egyéni kezdőlapját aaaSet |} Microsoft Docs"
description: "Magában foglalja az hello alapvető tudnivalók az Azure AD-alkalmazásproxy-összekötő"
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
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5bb695e904d285c3b440520f107c7bf63ba5cac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Közzétett alkalmazások egyéni kezdőlapját beállítása az Azure AD-alkalmazásproxy használatával

Ez a cikk azt ismerteti, hogyan tooconfigure alkalmazások toodirect felhasználók tooa egyéni kezdőlapján. Amikor közzétesz egy alkalmazást a Proxy, egy belső URL-Címének beállítása, de egyes esetekben a rendszer hello lapot a felhasználók először kell megjelennie. Egy egyéni kezdőlap beállításáról, hogy a felhasználók toohello jobb oldal nyissa meg a hello Azure Active Directory hozzáférési Panel vagy Office 365 hello alkalmazás indító hello alkalmazások elérésekor.

Amikor felhasználók hello alkalmazást, akkor még útmutatása alapján alapértelmezett toohello legfelső szintű tartomány URL-hello közzétett alkalmazást. általában be van állítva az hello kezdőlapja hello kezdőlap URL-CÍMÉT. Hello Azure AD PowerShell modul toodefine egyéni kezdőlap URL-címek használja, ha azt szeretné, hogy app felhasználók tooland hello alkalmazáson belül egy adott oldalon. 

Példa:
- A vállalati hálózaton belüli felhasználók nyissa meg túl*https://ExpenseApp/login/login.aspx* a toosign és az alkalmazás eléréséhez.
- Mivel más eszközök, például, hogy az alkalmazásproxy kell hello felső szintjén hello mappaszerkezet tooaccess képek, hello alkalmazás közzététele *https://ExpenseApp* hello belső URL-cím szerint.
- külső URL-cím alapértelmezett hello *https://ExpenseApp-contoso.msappproxy.net*, amely nem veszi a felhasználók toohello bejelentkezési oldalán.  
- Állítsa be *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* , hello kezdőlap URL-cím toogive a felhasználók zökkenőmentes élményt. 

>[!NOTE]
>Amikor a felhasználók hozzáférést toopublished alkalmazásokat adni, hello jelennek meg a hello alkalmazások [Azure AD hozzáférési Panel](active-directory-saas-access-panel-introduction.md) és hello [Office 365 alkalmazás indító](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Előkészületek

Hello kezdőlap URL-Címének beállítása előtt tartsa szem előtt tartva hello követelményeknek:

* Győződjön meg arról, hogy hello megadott elérési út egy altartomány elérési utat az hello legfelső szintű tartomány URL-cím.

  Ha hello gyökértartomány URL-CÍMÉT, például https://apps.contoso.com/app1/, hello kezdőlap URL-címet meg kell kezdődnie https://apps.contoso.com/app1/.

* Ha olyan módosítást toohello közzétett alkalmazás, hello módosítása előfordulhat, hogy alaphelyzetbe hello értékének hello kezdőlap URL-CÍMÉT. A jövőbeli hello hello alkalmazás frissítésekor kell újbóli ellenőrzéséhez és, ha szükséges, frissítse a hello kezdőlap URL-címe.

## <a name="change-hello-home-page-in-hello-azure-portal"></a>Hello kezdőlap módosítása a hello Azure-portálon

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Keresse meg a túl**Azure Active Directory** > **App regisztrációk** , és válassza ki az alkalmazás hello listáról. 
3. Válassza ki **tulajdonságok** hello beállításai közül.
4. Frissítés hello **Kezdőlap** mező az új elérési úttal. 

   ![Új kezdőlap URL-Címének megadása](./media/application-proxy-office365-app-launcher/homepage.png)

5. Válassza ki **mentése**

## <a name="change-hello-home-page-with-powershell"></a>Hello kezdőlap módosítása a Powershellel

### <a name="install-hello-azure-ad-powershell-module"></a>Hello Azure AD PowerShell modul telepítése

Egy egyéni kezdőlap URL-Címének megadása PowerShell segítségével, előtt telepítse az hello Azure AD PowerShell modult. Hello csomag letölthető hello [PowerShell-galériában](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), mely által használt hello Graph API-végpont. 

tooinstall hello csomag, kövesse az alábbi lépéseket:

1. Nyissa meg a standard PowerShell-ablakot, és futtassa a parancsot a következő hello:

    ```
     Install-Module -Name AzureAD
    ```
    Ha hello parancsot futtatja, a nem rendszergazda, akkor hello `-scope currentuser` lehetőséget.
2. Hello a telepítés során válassza **Y** tooinstall két Nuget.org a csomagokat. Mindkét csomagot szükség. 

### <a name="find-hello-objectid-of-hello-app"></a>Hello ObjectID hello alkalmazás keresése

Hello ObjectID hello alkalmazás beszerzése, és keressen a hello alkalmazás által a kezdőlap.

1. Nyissa meg a Powershellt, és az Azure AD hello modul importálásához.

    ```
    Import-Module AzureAD
    ```

2. Jelentkezzen be toohello az Azure AD-modullal hello bérlői rendszergazdaként.

    ```
    Connect-AzureAD
    ```
3. A kezdőlap URL-címe alapján hello alkalmazás megkereséséhez. Hello URL-címet az található hello portal túl címen**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**. Ez a példa *sharepoint-iddemo*.

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. Amely hasonló toohello itt látható egy eredményt kapja meg. Másolja a hello ObjectID GUID toouse hello a következő szakaszban.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-hello-home-page-url"></a>Hello kezdőlap URL-Címének frissítése

1. lépésben használt ugyanazon PowerShell-modul hello elvégezni hello a következő lépéseket:

1. Győződjön meg arról, hogy rendelkezik-e hello javítsa ki az alkalmazást, és cserélje le *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* a hello ObjectID hello az előző lépésben másolt.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Most, hogy hello app megerősítését most készen áll a tooupdate hello kezdőlap, az alábbiak szerint.

2. Hozzon létre egy üres alkalmazás objektum toohold, amelyet az toomake hello módosításokat. Ezt a változót, amelyet az tooupdate hello értékeket tartalmazza. Ezt a lépést nem jön létre.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Hello kezdőlap URL-cím toohello érték, amelyet állítható be. hello érték lehet egy altartomány hello közzétett alkalmazás elérési útját. Például, ha módosítja hello kezdőlap URL-CÍMÉT *https://sharepoint-iddemo.msappproxy.net/* túl*https://sharepoint-iddemo.msappproxy.net/hybrid/*, az alkalmazásaik felhasználóit lépjen közvetlenül toohello egyéni Kezdőlap.

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. Ellenőrizze a frissítés hello hello GUID azonosítója (ObjectID) a használatával "1. lépés: keresés hello hello alkalmazás ObjectID."

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. tooconfirm, hogy hello módosítása sikeres volt, indítsa újra a hello alkalmazást.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Bármely végrehajtott változtatásokat toohello app előfordulhat, hogy alaphelyzetbe hello kezdőlap URL-címe. Ha alaphelyzetbe állítja a kezdőlap URL-CÍMÉT, ismételje meg a 2.

## <a name="next-steps"></a>Következő lépések

- [Távelérés tooSharePoint az Azure AD alkalmazásproxy engedélyezése](application-proxy-enable-remote-access-sharepoint.md)
- [Alkalmazásproxy engedélyezése az Azure-portálon hello](active-directory-application-proxy-enable.md)
