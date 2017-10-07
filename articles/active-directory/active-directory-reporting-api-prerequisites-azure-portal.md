---
title: "aaaPrerequisites tooaccess hello Azure AD jelentéskészítési API |} Microsoft Docs"
description: "További tudnivalók: hello Előfeltételek tooaccess hello Azure AD jelentéskészítési API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ec28a7530f341dda31268a978754b615c727d66f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a>Előfeltételek tooaccess hello Azure AD jelentéskészítési API

Hello [az Azure AD reporting API-k](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) REST-alapú API-készlet toohello adatokat programozott hozzáférést biztosít. Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.

a reporting API által használt hello [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize hozzáférés toohello webes API-k. 

tooget adatelérés toohello jelentéskészítési hello API-n keresztül, szükséges toohave hello szerepkörök hozzárendelése a következő egyikét:

- Biztonsági olvasó
- Biztonsági rendszergazda
- Globális rendszergazda


tooprepare a reporting API access toohello, kell:

1. Egy alkalmazás regisztrálása 
2. Engedélyek 
3. Konfigurációs beállítások összegyűjtése 

Kérdéseit, vagy visszajelzést, kérjük, [fájlt egy támogatási jegy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Egy Azure Active Directory-alkalmazás regisztrálása

Akkor is, ha a reporting API-val parancsprogrammal hello elérésére kell tooregister egy alkalmazást. Ez lehetővé teszi egy **Alkalmazásazonosító**, ami azonban szükséges engedélyezési-hívások, és lehetővé teszi, hogy a kód tooreceive jogkivonatokat.

tooconfigure a directory tooaccess hello Azure AD jelentéskészítési API, be kell jelentkeznie a toohello Azure-portálon az Azure rendszergazdai fiókkal, amely tagja is hello **globális rendszergazda** az Azure AD-bérlő a címtár szerepkörrel .

> [!IMPORTANT]
> A "rendszergazda" jogosultságokkal credentials futó alkalmazások nagyon hatékonyak lehetnek, így Felhívjuk biztonságos meg arról, hogy tookeep hello alkalmazás azonosítója/titkos hitelesítő adatait.
> 


**egy Azure Active Directory-alkalmazás tooregister:**

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. A hello **Azure Active Directory** panelen kattintson a **App regisztrációk**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. A hello **App regisztrációk** panelen hello legfelül hello eszköztáron kattintson **új alkalmazás regisztrációja**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. A hello **létrehozása** panelen, hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. A hello **neve** szövegmezőhöz típus `Reporting API application`.

    b. Mint **alkalmazástípus**, jelölje be **Web app / API**.

    c. A hello **bejelentkezési URL-cím** szövegmezőhöz típus `https://localhost`.

    d. Kattintson a **Create** (Létrehozás) gombra. 


## <a name="grant-permissions"></a>Engedélyek 

Ez a lépés hello célját toogrant van az alkalmazás **címtáradatok olvasása** engedélyek toohello **Windows Azure Active Directory** API.

![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**toogrant az alkalmazás engedélyt toouse hello API:**

1. A hello **App regisztrációk** panelen hello alkalmazások listájában, kattintson a **Reporting API-alkalmazás**.

2. A hello **Reporting API-alkalmazás** panelen hello legfelül hello eszköztáron kattintson **beállítások**. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. A hello **beállítások** panelen kattintson a **szükséges engedélyek**. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. A hello **szükséges engedélyek** paneljén, hello **API** listában, kattintson **Windows Azure Active Directory**. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. A hello **hozzáférés engedélyezése** panelen válassza **címtáradatok olvasása**. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Hello hello felső eszköztárán kattintson **mentése**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a>Konfigurációs beállítások összegyűjtése 
Ez a szakasz bemutatja, hogy a címtárban lévő beállítások következő tooget hello:

* Tartománynév
* Ügyfél-azonosító
* Titkos ügyfélkulcs

Hívások toohello jelentéskészítési API konfigurálásakor kell ezeket az értékeket. 

### <a name="get-your-domain-name"></a>A tartománynév beolvasása

**tooget a tartománynév:**

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. A hello **Azure Active Directory** panelen kattintson a **tartománynevek**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. A tartománynév másolása tartományok hello listáját.


### <a name="get-your-applications-client-id"></a>Az alkalmazás ügyfél-azonosító lekérése

**tooget az alkalmazás ügyfél-azonosító:**

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. A hello **App regisztrációk** panelen hello alkalmazások listájában, kattintson a **Reporting API-alkalmazás**.

3. A hello **Reporting API-alkalmazás** penge, hello **Alkalmazásazonosító**, kattintson a **toocopy kattintson**.

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Az alkalmazás ügyfél titkos kulcs beszerzése
tooget az alkalmazás ügyfél titkos, meg kell toocreate egy új kulcsot, és mentse után hello új kulcs mentése, mert már nem lehetséges tooretrieve értéke ezt az értéket később már.

**tooget az alkalmazás ügyfélkulcs:**

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. A hello **App regisztrációk** panelen hello alkalmazások listájában, kattintson a **Reporting API-alkalmazás**.


3. A hello **Reporting API-alkalmazás** panelen hello legfelül hello eszköztáron kattintson **beállítások**. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. A hello **beállítások** paneljén, hello **APIR hozzáférés** területén kattintson **kulcsok**. 

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. A hello **kulcsok** panelen, hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazás regisztrálása](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. A hello **leírás** szövegmezőhöz típus `Reporting API`.

    b. Mint **Expires**, jelölje be **2 évben**.

    c. Kattintson a **Save** (Mentés) gombra.

    d. Másolja a hello kulcs értékét.


## <a name="next-steps"></a>Következő lépések
* Reporting volna, például tooaccess hello hello Azure AD adatait API-val programozott módon? Tekintse meg [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).
* Ha szeretne további információk az Azure Active Directory reporting toofind, tekintse meg a hello [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).  

