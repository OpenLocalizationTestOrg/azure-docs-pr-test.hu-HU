---
title: "Szolgáltatások közötti hitelesítés: Data Lake Store az Azure Active Directoryhoz |} Microsoft Docs"
description: "Útmutató: az Azure Active Directory használatával a Data Lake Store elérése a szolgáltatások közötti hitelesítés"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 820b7c5d-4863-4225-9bd1-df4d8f515537
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: 27ec0a4f48115d44da98dd048868b044aedf173c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Szolgáltatások közötti hitelesítés a Data Lake Store az Azure Active Directoryval
> [!div class="op_single_selector"]
> * [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md)
> * [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store az Azure Active Directory-hitelesítéshez. Egy alkalmazás, amely az Azure Data Lake Store vagy az Azure Data Lake Analytics szerzői, mielőtt először határozza meg, hogyan szeretné az Azure Active Directoryval (Azure AD) az alkalmazás hitelesítéséhez. A két fő elérhető lehetőségek a következők:

* Végfelhasználói hitelesítés 
* Szolgáltatások közötti hitelesítés (Ez a cikk) 

Mindkét ezeket a beállításokat az alkalmazás OAuth 2.0-tokenhez, lekérdezi az összes kérelmet az Azure Data Lake Store vagy az Azure Data Lake Analytics csatolva alatt megadott eredményez.

Ez a cikk megbeszélések arról, hogyan hozzon létre egy **szolgáltatások közötti hitelesítés az Azure AD-webalkalmazás**. További információ az Azure AD alkalmazás konfigurációban végfelhasználói hitelesítési [végfelhasználói hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>1. lépés: Az Active Directory-webalkalmazás létrehozása

Hozzon létre, és a szolgáltatások közötti hitelesítéshez az Azure AD webes alkalmazások konfigurálása az Azure Data Lake Store az Azure Active Directoryval. Útmutatásért lásd: [hozzon létre egy Azure AD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Közben megadott utasítások a fenti hivatkozásra, gondoskodjon róla, hogy **Web App / API** alkalmazás típusra, az alábbi képernyőfelvételen látható módon.

![A webalkalmazás létrehozása](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "webalkalmazás létrehozása")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>2. lépés: Le alkalmazásazonosító, a hitelesítési kulcs és a bérlő azonosítója
Bejelentkezéskor programozott módon, a azonosító szükséges az alkalmazáshoz. Ha az alkalmazás fut, a saját hitelesítő adatait, a hitelesítési kulcs is szüksége lesz.

* Hogyan lehet lekérni az alkalmazás azonosítója és a hitelesítési kulcs (más néven a titkos ügyfélkulcs) az alkalmazáshoz, lásd: [Get alkalmazás Azonosítóját és a hitelesítési kulcs](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Hogyan lehet lekérni a bérlő azonosítója, lásd: [-bérlőazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>3. lépés:, Rendelje hozzá az Azure AD-alkalmazás az Azure Data Lake Store-fiók fájlhoz vagy mappához (csak a szolgáltatások közötti hitelesítéshez)
1. Jelentkezzen be az új [Azure-portálon](https://portal.azure.com) , és nyissa meg az Azure Data Lake Store-fiókot az Azure Active Directory-alkalmazást a korábban létrehozott társítani kívánt.
2. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Könyvtárak létrehozása a Data Lake Store-fiók](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "könyvtárak létrehozása a Data Lake-fiókban")
3. Az a **adatkezelő** panelen kattintson a fájl vagy mappa számára biztosít hozzáférést az Azure AD-alkalmazást, és kattintson a kívánt **hozzáférés**. A fájlhoz való hozzáférés konfigurálásához kattintson **hozzáférés** a a **fájljának előnézete** panelen.
   
    ![Hozzáférés-vezérlési listák beállítása a Data Lake fájlrendszerben](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "beállítása ACL-ek Data Lake fájlrendszer")
4. A **hozzáférés** panel szabványos hozzáférési és egyéni hozzáférési már hozzá van rendelve a legfelső szintű sorolja fel. Kattintson a **Hozzáadás** ikonra kattintva adja hozzá az egyéni szintű hozzáférés-vezérlési listák.
   
    ![Normál és egyéni hozzáférési listában](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "normál és egyéni hozzáférési listában")
5. Kattintson a **Hozzáadás** ikonra kattintva nyissa meg a **egyéni hozzáférés hozzáadása** panelen. Ezen a panelen kattintson a **felhasználó vagy csoport kiválasztása**, majd a **felhasználó vagy csoport kiválasztása** panelen keresse meg a korábban létrehozott Azure Active Directory-alkalmazást. Ha a keresés csoportok számos, a szövegmezőbe írja felső szűréséhez használja a csoport nevére. Kattintson a csoport hozzáadása, és kattintson a kívánt **válasszon**.
   
    ![Csoport hozzáadása](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "csoport hozzáadása")
6. Kattintson a **Select engedélyeket**, engedélyek kiválasztása, és szeretne hozzárendelni az engedélyeket ACL alapértelmezés szerint, hogy elérhetők a hozzáférés-vezérlési lista, vagy mindkettőt. Kattintson az **OK** gombra.
   
    ![Engedélyeket csoportosításához](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "csoportosításához engedélyek hozzárendelése")
   
    A Data Lake Store és a hozzáférés-vezérlési listákat alapértelmezett/hozzáférési engedélyekkel kapcsolatos további információkért lásd: [hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).
7. Az a **egyéni hozzáférés hozzáadása** panelen kattintson a **OK**. Az újonnan létrehozott csoport a társított engedélyekkel most megjelenik a **hozzáférés** panelen.
   
    ![Engedélyeket csoportosításához](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "csoportosításához engedélyek hozzárendelése")

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>4. lépés: Az OAuth 2.0 token-végpont lekérése (csak a Java-alapú alkalmazások)

1. Jelentkezzen be az új [Azure-portálon](https://portal.azure.com) és a bal oldali ablaktáblán kattintson az Active Directory.

2. A bal oldali ablaktáblán kattintson a **App regisztrációk**.

3. Az alkalmazás regisztrációk panel felső részén kattintson **végpontok**.

    ![OAuth jogkivonat végpontjához](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "token OAuth-végpont")

4. Másolja az OAuth 2.0 token-végpont végpontok listáját.

    ![OAuth jogkivonat végpontjához](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "token OAuth-végpont")   

## <a name="next-steps"></a>Következő lépések
Ez a cikk az Azure AD-webalkalmazás létrehozása, és az információ áll rendelkezésre az ügyfél alkalmazások használata a .NET SDK, Java SDK stb szerzői van szüksége. Most már folytathatja, az alábbi cikkek tartalmazzák, amely először a Data Lake Store hitelesítéséhez, és végezze el más műveleteket végez a tároló az Azure AD-webes alkalmazás használatával kapcsolatban.

* [Az Azure Data Lake Store használatának első lépései a .NET SDK-val](data-lake-store-get-started-net-sdk.md)
* [Az Azure Data Lake Store Java SDK használatának első lépései](data-lake-store-get-started-java-sdk.md)

Ez a cikk telefonon keresztül az alapvető lépéseken, szükséges a felhasználó egyszerű fel, és az alkalmazás futtatása. Megnézheti szerezhet be további információt a következő cikkeket:
* [Egyszerű szolgáltatásnév létrehozása a PowerShell használatával](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [A szolgáltatás egyszerű hitelesítéshez Tanúsítványalapú hitelesítés használatára](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Más módszerekkel küld az Azure AD-hitelesítéshez](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios)


