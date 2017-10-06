---
title: "aaaPrerequisites tooaccess hello Azure AD jelentéskészítési API. | Microsoft Docs"
description: "További tudnivalók: hello Előfeltételek tooaccess hello Azure AD jelentéskészítési API"
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
ms.openlocfilehash: e9d7ceaedb07d18fbd75b70d68b5cfbebc756c36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a>Előfeltételek tooaccess hello Azure AD jelentéskészítési API
Hello [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) REST-alapú API-készlet toohello adatokat programozott hozzáférést biztosít. Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.

a reporting API által használt hello [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize hozzáférés toohello webes API-k. 

tooprepare a reporting API access toohello, kell:

1. Alkalmazás létrehozása az Azure AD-bérlőben 
2. Támogatás hello alkalmazás megfelelő engedélyekkel tooaccess hello Azure AD-adatok
3. Konfigurációs beállítások összegyűjtése a címtárban

Kérdéseit, vagy visszajelzést, lépjen kapcsolatba [AAD jelentéskészítési súgó](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Az Azure AD-alkalmazás létrehozása
tooconfigure a directory tooaccess hello Azure AD jelentéskészítési API, be kell jelentkeznie a toohello Azure-előfizetéshez rendszergazdai fiókkal, amely tagja is hello globális rendszergazda directory szerepkör az Azure AD-bérlő a klasszikus Azure portálon.

> [!IMPORTANT]
> A "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így Felhívjuk biztonságos meg arról, hogy tookeep hello alkalmazás azonosítója/titkos hitelesítő adatait.
> 
> 

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. A hello **active directory** listára, válassza ki a címtárban.
3. Hello hello felső menüben kattintson a **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Az alsó hello menüsávon kattintson **Hozzáadás**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/03.png) 
5. A hello **miről szeretne toodo?** párbeszédpanel, kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**. 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/04.png) 
6. A hello **adja meg azt az alkalmazás** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello: 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. A hello **neve** szövegmező, írja be nevét (pl.: Reporting API-alkalmazás).
   
    b. Válassza ki **webalkalmazás és/vagy webes API-t**.
   
    c. Kattintson a **Tovább** gombra.
7. A hello **alkalmazás tulajdonságainak** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello: 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.
   
    b. A hello **App ID URI** szövegmezőhöz típus ```https://localhost/ReportingApiApp```.
   
    c. Kattintson a **Befejezés** gombra.

## <a name="grant-your-application-permission-toouse-hello-api"></a>Adja meg az alkalmazás engedélyt toouse hello API
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. A hello **active directory** listára, válassza ki a címtárban.
3. Hello hello felső menüben kattintson a **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png)
4. Hello alkalmazások listáját válassza ki az újonnan létrehozott alkalmazás.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. Hello hello felső menüben kattintson a **konfigurálása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. A hello **engedélyek tooother alkalmazások** szakasz hello **Azure Active Directory** erőforrás, hello kattintson **Alkalmazásengedélyek** legördülő listában, majd Válassza ki **címtáradatok olvasása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/09.png)
7. Az alsó hello menüsávon kattintson **mentése**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Konfigurációs beállítások összegyűjtése a címtárban
Ez a szakasz bemutatja, hogy a címtárban lévő beállítások következő tooget hello:

* Tartománynév
* Ügyfél-azonosító
* Titkos ügyfélkulcs

Hívások toohello jelentéskészítési API konfigurálásakor kell ezeket az értékeket. 

### <a name="get-your-domain-name"></a>A tartománynév beolvasása
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. A hello **active directory** listára, válassza ki a címtárban.
3. Hello hello felső menüben kattintson a **tartományok**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/11.png) 
4. A hello **tartománynév** oszlop, másolja a tartomány nevét.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-hello-applications-client-id"></a>Hello alkalmazás ügyfél-azonosító lekérése
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. A hello **active directory** listára, válassza ki a címtárban.
3. Hello hello felső menüben kattintson a **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Hello alkalmazások listáját válassza ki az újonnan létrehozott alkalmazás.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. Hello hello felső menüben kattintson a **konfigurálása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. Másolás a **ügyfél-azonosító**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-hello-applications-client-secret"></a>Szeretne hello alkalmazás ügyfélkulcsot
tooget az alkalmazás ügyfél titkos, meg kell toocreate egy új kulcsot, és mentse után hello új kulcs mentése, mert már nem lehetséges tooretrieve értéke ezt az értéket később már.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/01.png) 
2. A hello **active directory** listára, válassza ki a címtárban.
3. Hello hello felső menüben kattintson a **alkalmazások**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Hello alkalmazások listáját válassza ki az újonnan létrehozott alkalmazás.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/07.png)
5. Hello hello felső menüben kattintson a **konfigurálása**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/08.png)
6. A hello **kulcsok** csoportjában hajtsa végre az alábbi lépésekkel hello: 
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Hello időtartama listájából válassza ki a időtartama
   
    b. Az alsó hello menüsávon kattintson **mentése**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Másolja a hello kulcs értékét.

## <a name="next-steps"></a>Következő lépések
* Reporting volna, például tooaccess hello hello Azure AD adatait API-val programozott módon? Tekintse meg [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).
* Ha szeretne további információk az Azure Active Directory reporting toofind, tekintse meg a hello [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).  

