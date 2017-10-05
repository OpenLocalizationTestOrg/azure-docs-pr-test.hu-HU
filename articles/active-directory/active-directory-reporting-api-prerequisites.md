---
title: "Előfeltételek az Azure AD reporting API eléréséhez. | Microsoft Docs"
description: "További tudnivalók az Azure AD reporting API eléréséhez Előfeltételek"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 6e409fc56b77f37dac7f37382e664c577666ad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Az Azure AD reporting API eléréséhez Előfeltételek
A [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) keresztül REST-alapú API-készlet programozott hozzáférést biztosít. Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.

A jelentéskészítési API által használt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) engedélyezéséhez a web API-k eléréséhez. 

Készítse elő a reporting API a hozzáférést, a következőket kell tennie:

1. Alkalmazás létrehozása az Azure AD-bérlőben 
2. Az alkalmazás az Azure AD-adatok eléréséhez szükséges engedélyeket adja meg
3. Konfigurációs beállítások összegyűjtése a címtárban

Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Az Azure AD-alkalmazás létrehozása
A címtár az Azure AD reporting API eléréséhez konfigurálásához be kell jelentkeznie Azure-előfizetéshez rendszergazdai fiókkal, amely tagja is az Azure AD-bérlő globális rendszergazdája directory szerepkörének a klasszikus Azure portálra.

> [!IMPORTANT]
> Ehhez hasonló "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így az alkalmazás azonosítója/titkos hitelesítő adatok a biztonságos ellenőrizze.
> 
> 

1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Az a **active directory** listára, válassza ki a címtárban.
3. Kattintson a felső menüben **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Kattintson az alsó sáv **Hozzáadás**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/03.png) 
5. Az a **mi történjen a teendő?** párbeszédpanel, kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**. 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/04.png) 
6. Az a **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre a következő lépéseket: 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. Az a **neve** szövegmező, írja be nevét (pl.: Reporting API-alkalmazás).
   
    b. Válassza ki **webalkalmazás és/vagy webes API-t**.
   
    c. Kattintson a **Tovább** gombra.
7. Az a **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre a következő lépéseket: 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. Az a **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.
   
    b. Az a **App ID URI** szövegmezőhöz típus ```https://localhost/ReportingApiApp```.
   
    c. Kattintson a **Befejezés** gombra.

## <a name="grant-your-application-permission-to-use-the-api"></a>Adja meg az alkalmazás számára az API-val
1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com/), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Az a **active directory** listára, válassza ki a címtárban.
3. Kattintson a felső menüben **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png)
4. Válassza ki az újonnan létrehozott alkalmazást az alkalmazások listáját.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. Kattintson a felső menüben **konfigurálása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. Az a **egyéb alkalmazások engedélyei** szakaszban, a a **Azure Active Directory** erőforrás, kattintson a **Alkalmazásengedélyek** legördülő listából válassza ki, és válassza ki **Címtáradatok olvasása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/09.png)
7. Kattintson az alsó sáv **mentése**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Konfigurációs beállítások összegyűjtése a címtárban
Ez a szakasz bemutatja, hogyan az a Directoryból olvassa ki az alábbi beállításokat:

* Tartománynév
* Ügyfél-azonosító
* Titkos ügyfélkulcs

A jelentéskészítési API hívásainak konfigurálásakor kell ezeket az értékeket. 

### <a name="get-your-domain-name"></a>A tartománynév beolvasása
1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Az a **active directory** listára, válassza ki a címtárban.
3. Kattintson a felső menüben **tartományok**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/11.png) 
4. Az a **tartománynév** oszlop, másolja a tartomány nevét.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a>Az alkalmazás ügyfél-azonosító lekérése
1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Az a **active directory** listára, válassza ki a címtárban.
3. Kattintson a felső menüben **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Válassza ki az újonnan létrehozott alkalmazást az alkalmazások listáját.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. Kattintson a felső menüben **konfigurálása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. Másolás a **ügyfél-azonosító**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a>Az alkalmazás ügyfél titkos kulcs beszerzése
Ahhoz, hogy az alkalmazás ügyfélkulcs, meg kell hozzon létre egy új kulcsot, és mentse az új kulcs mentése, mert nincs lehetőség a később többé lekérje ezt az értéket, az értékét.

1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Az a **active directory** listára, válassza ki a címtárban.
3. Kattintson a felső menüben **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Válassza ki az újonnan létrehozott alkalmazást az alkalmazások listáját.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. Kattintson a felső menüben **konfigurálása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. Az a **kulcsok** területen tegye a következőket: 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Időtartam listájából válasszon egy időtartamot
   
    b. Kattintson az alsó sáv **mentése**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Másolja a kulcs értékét.

## <a name="next-steps"></a>Következő lépések
* Szeretné az Azure AD reporting API-val programozott módon érheti el az adatait? Tekintse meg [Bevezetés az Azure Active Directory Reporting API használatába](active-directory-reporting-api-getting-started.md).
* Ha meg szeretne többet megtudni az Azure Active Directory reporting, tekintse meg a [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).  

