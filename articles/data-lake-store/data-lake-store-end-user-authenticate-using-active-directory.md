---
title: "Végfelhasználói hitelesítési: Data Lake Store az Azure Active Directoryhoz |} Microsoft Docs"
description: "Megtudhatja, hogyan végfelhasználói hitelesítés az Azure Active Directory használatával a Data Lake Store elérése"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ec586ecd-1b42-459e-b600-fadbb7b80a9b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: c20f5c39b00992d801909c8e5de292f3c2f12673
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>A Data Lake Store az Azure Active Directoryval végfelhasználói hitelesítés
> [!div class="op_single_selector"]
> * [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md)
> * [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store az Azure Active Directory-hitelesítéshez. Egy alkalmazás, amely az Azure Data Lake Store vagy az Azure Data Lake Analytics szerzői, mielőtt először határozza meg, hogyan szeretné az Azure Active Directoryval (Azure AD) az alkalmazás hitelesítéséhez. A két fő elérhető lehetőségek a következők:

* Végfelhasználói hitelesítési (Ez a cikk)
* Szolgáltatások közötti hitelesítés

Mindkét ezeket a beállításokat az alkalmazás OAuth 2.0-tokenhez, lekérdezi az összes kérelmet az Azure Data Lake Store vagy az Azure Data Lake Analytics csatolva alatt megadott eredményez.

Ez a cikk megbeszélések arról, hogyan hozzon létre egy **végfelhasználói hitelesítéshez natív Azure AD-alkalmazást**. További információ a szolgáltatások közötti hitelesítéshez az Azure AD-alkalmazás konfigurációja [szolgáltatások közötti hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* Az előfizetés-azonosító. Az Azure portálról le tudja kérni. Például érhető el a Data Lake Store-fiók paneljén.
  
    ![Előfizetés-azonosító lekérése](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Az Azure AD-tartomány neve. Azt az Azure portál jobb felső sarkában az egér rámutató által kérheti le. Az alábbi képernyőképen látható, a tartománynév nem **contoso.onmicrosoft.com**, a szögletes zárójelben GUID pedig a bérlői. 
  
    ![Az AAD tartományi beolvasása](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Végfelhasználói hitelesítés
Ez az az ajánlott módszer, ha azt szeretné, hogy a felhasználó bejelentkezni az alkalmazás az Azure AD használatával. Az alkalmazás tudnak hitelesítésszolgáltatóéval azonos szintű hozzáférés, a végfelhasználónak, a rendszer az Azure-erőforrások eléréséhez. Adja meg a hitelesítő adatokat rendszeres időközönként ahhoz, hogy az alkalmazás fér a végfelhasználó használni kell.

Az eredmény, hogy a végfelhasználó jelentkezzen be, hogy az alkalmazás olyan hozzáférési jogkivonatot, és egy frissítési jogkivonat. A hozzáférési jogkivonatot minden Data Lake Store vagy a Data Lake Analytics kérelmet lekérdezi kapcsolni, és alapértelmezés szerint egy órán keresztül érvényes legyen. A frissítési jogkivonat segítségével szerezzen be egy új jogkivonatot, és alapértelmezés szerint legfeljebb két hétig érvényes rendszeresen használatakor. Két különböző szempontok a végfelhasználói bejelentkezésnél is használhatja.

### <a name="using-the-oauth-20-pop-up"></a>Az OAuth 2.0 előugró használatával
Az alkalmazás is elindíthatja az OAuth 2.0 hitelesítési előugró ablakban, amelyben a végfelhasználói megadhatják hitelesítő adataikat. Az előugró ablak az Azure AD-kéttényezős hitelesítést (2FA) folyamat során is működik, ha szükséges. 

> [!NOTE]
> Ez a metódus még nem támogatott az az Azure AD Authentication Library (ADAL) Python vagy Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Felhasználói hitelesítő adatok közvetlenül benyújtása
Az alkalmazás közvetlenül adni a hitelesítő adatokat az Azure ad Szolgáltatásba. Ez a módszer csak használható a szervezeti azonosító felhasználói fiókok; Ez nem kompatibilis a személyes / a "live ID" felhasználói fiókok, beleértve a végződése @outlook.com vagy @live.com. Továbbá ez a metódus nem található felhasználói fiókok Azure AD-kéttényezős hitelesítést (2FA) igénylő kompatibilis.

### <a name="what-do-i-need-to-use-this-approach"></a>Mit kell ezt a módszert használja?
* Az Azure AD-tartomány nevét. Ez már szerepel-e ez a cikk az előfeltételek.
* Az Azure AD **natív alkalmazás**
* Az Azure AD natív alkalmazás azonosítója
* Az Azure AD natív alkalmazás átirányítási URI
* Delegált engedélyek beállítása


## <a name="step-1-create-an-active-directory-native-application"></a>1. lépés: Az Active Directory natív alkalmazás létrehozása

Hozzon létre, és a végfelhasználói hitelesítéshez az Azure AD natív alkalmazások konfigurálása az Azure Data Lake Store az Azure Active Directoryval. Útmutatásért lásd: [hozzon létre egy Azure AD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Közben megadott utasítások a fenti hivatkozásra, gondoskodjon róla, hogy **natív** alkalmazás típusra, az alábbi képernyőfelvételen látható módon.

![A webalkalmazás létrehozása](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "hozzon létre natív alkalmazás")

## <a name="step-2-get-application-id-and-redirect-uri"></a>2. lépés: Alkalmazásazonosító beszerzése és átirányítási URI

Lásd: [Alkalmazásazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) beolvasni az Azure AD-natív alkalmazás az alkalmazás azonosítóját (más néven az ügyfél-Azonosítót a klasszikus Azure portálon).

Az átirányítási URI-t lekéréséhez kövesse az alábbi lépéseket.

1. Az Azure portálon, válassza ki a **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott natív Azure AD-alkalmazás.

2. Az a **beállítások** az alkalmazás paneljén kattintson **átirányítási URI-azonosítók**.

    ![Get-átirányítási URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Másolja a megjelenített érték.


## <a name="step-3-set-permissions"></a>3. lépés: Engedélyek beállítása

1. Az Azure portálon, válassza ki a **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott natív Azure AD-alkalmazás.

2. Az a **beállítások** az alkalmazás paneljén kattintson **szükséges engedélyek**, és kattintson a **Hozzáadás**.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. A a **API-hozzáférés hozzáadása** panelen kattintson a **API kiválasztása**, kattintson a **Azure Data Lake**, és kattintson a **válassza**.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  Az a **API-hozzáférés hozzáadása** panelen kattintson **engedélyként válassza**, jelölje be a jelölőnégyzetet, hogy a **teljes hozzáférés a Data Lake store**, és kattintson a **kiválasztása**.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Kattintson a **Done** (Kész) gombra.

5. Ismételje meg az utolsó két lépést vonatkozó engedélyeket **Windows Azure szolgáltatásfelügyeleti API** is.
   
## <a name="next-steps"></a>Következő lépések
Ebben a cikkben egy natív Azure AD-alkalmazást létrehozni, és a szükséges információkat az ügyfél-alkalmazások használata a .NET SDK-val, a Java SDK-t, a REST API-t, a stb szerzői összegyűjtött. Most már folytathatja, az alábbi cikkek tartalmazzák, amely először a Data Lake Store hitelesítéséhez, és végezze el más műveleteket végez a tároló az Azure AD-webes alkalmazás használatával kapcsolatban.

* [Az Azure Data Lake Store használatának első lépései a .NET SDK-val](data-lake-store-get-started-net-sdk.md)
* [Az Azure Data Lake Store Java SDK használatának első lépései](data-lake-store-get-started-java-sdk.md)
* [Ismerkedés az Azure Data Lake Store REST API használatával](data-lake-store-get-started-rest-api.md)

