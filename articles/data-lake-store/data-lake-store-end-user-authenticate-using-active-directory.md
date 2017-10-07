---
title: "Végfelhasználói hitelesítési: Data Lake Store az Azure Active Directoryhoz |} Microsoft Docs"
description: "Megtudhatja, hogyan tooachieve végfelhasználói hitelesítési a Data Lake Store az Azure Active Directoryval"
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
ms.openlocfilehash: fd58f4f2d8fc915b8bc51d9e5b040d2cee34047e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>A Data Lake Store az Azure Active Directoryval végfelhasználói hitelesítés
> [!div class="op_single_selector"]
> * [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md)
> * [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store az Azure Active Directory-hitelesítéshez. Egy alkalmazás, amely az Azure Data Lake Store vagy az Azure Data Lake Analytics szerzői előtt döntse el, először hogyan szeretné tooauthenticate az alkalmazás Azure Active Directory (Azure AD). hello két fő elérhető lehetőségek a következők:

* Végfelhasználói hitelesítési (Ez a cikk)
* Szolgáltatások közötti hitelesítés

Mindkét ezeket a beállításokat az alkalmazás OAuth 2.0-tokenhez, amely lekérdezi a csatolt tooeach kérelmet tooAzure Data Lake Store vagy az Azure Data Lake Analytics alatt megadott eredményez.

Ez a cikk megbeszélések arról, hogyan hozzon létre egy **végfelhasználói hitelesítéshez natív Azure AD-alkalmazást**. További információ a szolgáltatások közötti hitelesítéshez az Azure AD-alkalmazás konfigurációja [szolgáltatások közötti hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* Az előfizetés-azonosító. Az Azure portál hello hozzáférhet. Például érhető el a Data Lake Store-fiók panelen hello.
  
    ![Előfizetés-azonosító lekérése](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Az Azure AD-tartomány neve. Ez által rámutató hello egér hello Azure portál jobb felső sarkában hello kérheti le. Az alábbi hello képernyőképet, hello tartománynév megadása **contoso.onmicrosoft.com**, és a szögletes zárójelben GUID hello az hello bérlő azonosítója. 
  
    ![Az AAD tartományi beolvasása](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Végfelhasználói hitelesítés
Ez az ajánlott megközelítést alkalmazva, ha azt szeretné, hogy egy végfelhasználói toolog az Azure AD alkalmazás tooyour hello. Az alkalmazás képes tooaccess Azure-erőforrások hello lesz, a rendszer a hello végfelhasználói hozzáférési azonos szinten. A végfelhasználói kell tooprovide rendszeres időközönként ahhoz, hogy az alkalmazás toomaintain hozzáférést a hitelesítő adataikat.

hello rendelkezik-e jelentkezni hello végfelhasználói eredménye, hogy olyan hozzáférési jogkivonatot és egy frissítési adta-e az alkalmazás. hello hozzáférési jogkivonat beolvasása tooData Lake Store-ból vagy a Data Lake Analytics csatolt tooeach kérelmet, és alapértelmezés szerint egy órán keresztül érvényes legyen. hello frissítési jogkivonat lehet egy új jogkivonatot, és esetén érvényes mentése tootwo hét alapértelmezés szerint ha rendszeresen használt használt tooobtain. Két különböző szempontok a végfelhasználói bejelentkezésnél is használhatja.

### <a name="using-hello-oauth-20-pop-up"></a>Előugró hello OAuth 2.0 használatával
Az alkalmazás is elindíthatja az OAuth 2.0 hitelesítési előugró ablakban mely hello a végfelhasználó adja meg a hitelesítő adataikat. Az előugró ablak hello Azure AD-kéttényezős hitelesítést (2FA) folyamat is működik, ha szükséges. 

> [!NOTE]
> Ez a metódus még nem támogatott az Azure AD Authentication Library (ADAL) hello Python vagy Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Felhasználói hitelesítő adatok közvetlenül benyújtása
Az alkalmazás közvetlenül képes biztosítani a felhasználói hitelesítő adatok tooAzure AD. Ez a módszer csak használható a szervezeti azonosító felhasználói fiókok; Ez nem kompatibilis a személyes / a "live ID" felhasználói fiókok, beleértve a végződése @outlook.com vagy @live.com. Továbbá ez a metódus nem található felhasználói fiókok Azure AD-kéttényezős hitelesítést (2FA) igénylő kompatibilis.

### <a name="what-do-i-need-toouse-this-approach"></a>Mire van szükségem az toouse ezt a módszert használja?
* Az Azure AD-tartomány nevét. Ez már szerepel-e ez a cikk hello előfeltétel.
* Az Azure AD **natív alkalmazás**
* Az Azure AD hello natív alkalmazás azonosítója
* A hello natív Azure AD-alkalmazás átirányítási URI
* Delegált engedélyek beállítása


## <a name="step-1-create-an-active-directory-native-application"></a>1. lépés: Az Active Directory natív alkalmazás létrehozása

Hozzon létre, és a végfelhasználói hitelesítéshez az Azure AD natív alkalmazások konfigurálása az Azure Data Lake Store az Azure Active Directoryval. Útmutatásért lásd: [hozzon létre egy Azure AD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Közben a következő hello található utasítások segítségével: hello hivatkozás felett, gondoskodjon róla, hogy **natív** alkalmazás típusra, az alábbi hello képernyőfelvételen látható módon.

![A webalkalmazás létrehozása](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "hozzon létre natív alkalmazás")

## <a name="step-2-get-application-id-and-redirect-uri"></a>2. lépés: Alkalmazásazonosító beszerzése és átirányítási URI

Lásd: [hello Alkalmazásazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello alkalmazásazonosítót (más néven hello ügyfél-azonosító hello a klasszikus Azure portálon) natív hello Azure AD-alkalmazás.

tooretrieve hello átirányítási URI-címe, kövesse az alábbi hello lépéseket.

1. Hello Azure portált, válassza ki **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott hello Azure AD-natív alkalmazás.

2. A hello **beállítások** hello alkalmazás paneljén kattintson **átirányítási URI-azonosítók**.

    ![Get-átirányítási URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Másolja a megjelenített hello érték.


## <a name="step-3-set-permissions"></a>3. lépés: Engedélyek beállítása

1. Hello Azure portált, válassza ki **Azure Active Directory**, kattintson a **App regisztrációk**, majd található, és kattintson az imént létrehozott hello Azure AD-natív alkalmazás.

2. A hello **beállítások** hello alkalmazás paneljén kattintson **szükséges engedélyek**, és kattintson a **Hozzáadás**.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. A hello **API-hozzáférés hozzáadása** panelen kattintson **API kiválasztása**, kattintson a **Azure Data Lake**, és kattintson a **kiválasztása**.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  Hello a **API-hozzáférés hozzáadása** panelen kattintson **engedélyként válassza**, válasszon hello jelölőnégyzetet toogive **teljes körű hozzáférés az tooData Lake Store**, és kattintson a **kiválasztása **.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Kattintson a **Done** (Kész) gombra.

5. Ismétlődő hello utolsó két lépést toogrant engedélyeinek **Windows Azure szolgáltatásfelügyeleti API** is.
   
## <a name="next-steps"></a>Következő lépések
Ez a cikk egy natív Azure AD-alkalmazást létrehozni, és hello információ áll rendelkezésre az ügyfél alkalmazások használata a .NET SDK-val, a Java SDK-t, a REST API-t, a stb szerzői van szüksége. Most már folytathatja a szolgáltatással kapcsolatban, hogyan toouse hello Azure AD webes alkalmazás toofirst a Data Lake Store hitelesítést, és hajtsa végre az egyéb műveletek hello tárolójában lévő cikkek a következő toohello.

* [Az Azure Data Lake Store használatának első lépései a .NET SDK-val](data-lake-store-get-started-net-sdk.md)
* [Az Azure Data Lake Store Java SDK használatának első lépései](data-lake-store-get-started-java-sdk.md)
* [Ismerkedés az Azure Data Lake Store REST API használatával](data-lake-store-get-started-rest-api.md)

