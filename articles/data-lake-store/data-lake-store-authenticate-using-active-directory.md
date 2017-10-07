---
title: "Szolgáltatások közötti hitelesítés: Data Lake Store az Azure Active Directoryhoz |} Microsoft Docs"
description: "Megtudhatja, hogyan tooachieve szolgáltatások közötti hitelesítés a Data Lake Store az Azure Active Directoryval"
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
ms.openlocfilehash: 2e56237a75f020067b3248a1e1cfaf3c8df1371c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Szolgáltatások közötti hitelesítés a Data Lake Store az Azure Active Directoryval
> [!div class="op_single_selector"]
> * [Szolgáltatások közötti hitelesítés](data-lake-store-authenticate-using-active-directory.md)
> * [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store az Azure Active Directory-hitelesítéshez. Egy alkalmazás, amely az Azure Data Lake Store vagy az Azure Data Lake Analytics szerzői előtt döntse el, először hogyan szeretné tooauthenticate az alkalmazás Azure Active Directory (Azure AD). hello két fő elérhető lehetőségek a következők:

* Végfelhasználói hitelesítés 
* Szolgáltatások közötti hitelesítés (Ez a cikk) 

Mindkét ezeket a beállításokat az alkalmazás OAuth 2.0-tokenhez, amely lekérdezi a csatolt tooeach kérelmet tooAzure Data Lake Store vagy az Azure Data Lake Analytics alatt megadott eredményez.

Ez a cikk megbeszélések arról, hogyan hozzon létre egy **szolgáltatások közötti hitelesítés az Azure AD-webalkalmazás**. További információ az Azure AD alkalmazás konfigurációban végfelhasználói hitelesítési [végfelhasználói hitelesítés az Azure Active Directory használatával a Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>1. lépés: Az Active Directory-webalkalmazás létrehozása

Hozzon létre, és a szolgáltatások közötti hitelesítéshez az Azure AD webes alkalmazások konfigurálása az Azure Data Lake Store az Azure Active Directoryval. Útmutatásért lásd: [hozzon létre egy Azure AD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Közben a következő hello található utasítások segítségével: hello hivatkozás felett, gondoskodjon róla, hogy **Web App / API** alkalmazás típusra, az alábbi hello képernyőfelvételen látható módon.

![A webalkalmazás létrehozása](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "webalkalmazás létrehozása")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>2. lépés: Le alkalmazásazonosító, a hitelesítési kulcs és a bérlő azonosítója
Bejelentkezéskor programozott módon, hello azonosító szükséges az alkalmazáshoz. Hello alkalmazás fut, a saját hitelesítő adatait, ha egy hitelesítési kulcsát is szüksége lesz.

* Hogyan tooretrieve hello Alkalmazásazonosító vagy a hitelesítési kulcs (is hívott hello ügyfélkulcs) az alkalmazáshoz, lásd: [Get alkalmazás Azonosítóját és a hitelesítési kulcs](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Hogyan tooretrieve hello Bérlőazonosító, lásd: [-bérlőazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-hello-azure-ad-application-toohello-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>3. lépés: Hozzárendelése hello Azure AD alkalmazás toohello Azure Data Lake Store fiók fájlt vagy mappát (csak a szolgáltatások közötti hitelesítés)
1. Jelentkezzen be az új toohello [Azure-portálon](https://portal.azure.com) , és nyissa meg az Azure Active Directory-alkalmazást a korábban létrehozott hello tooassociate kívánt hello Azure Data Lake Store-fiók.
2. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Könyvtárak létrehozása a Data Lake Store-fiók](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "könyvtárak létrehozása a Data Lake-fiókban")
3. A hello **adatkezelő** paneljén kattintson hello fájlra vagy mappára, amelyhez szeretné, hogy tooprovide access toohello az Azure AD alkalmazás, és kattintson **hozzáférés**. tooconfigure access tooa fájlt, kattintson a **hozzáférés** a hello **fájljának előnézete** panelen.
   
    ![Hozzáférés-vezérlési listák beállítása a Data Lake fájlrendszerben](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "beállítása ACL-ek Data Lake fájlrendszer")
4. Hello **hozzáférés** panel hello szokásos hozzáférés sorolja fel, és egyéni hozzáférési már hozzá van rendelve toohello legfelső szintű. Kattintson a hello **Hozzáadás** ikon tooadd egyéni szintű hozzáférés-vezérlési listák.
   
    ![Normál és egyéni hozzáférési listában](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "normál és egyéni hozzáférési listában")
5. Kattintson a hello **hozzáadása** ikon tooopen hello **egyéni hozzáférés hozzáadása** panelen. Ezen a panelen kattintson a **felhasználó vagy csoport kiválasztása**, majd a **felhasználó vagy csoport kiválasztása** panelen keresse meg a korábban létrehozott hello Azure Active Directory-alkalmazás. Ha a csoportok toosearch számos, használhatja hello szövegmező hello felső toofilter a hello csoport nevére. Hello csoport tooadd szeretné, majd kattintson **válasszon**.
   
    ![Csoport hozzáadása](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "csoport hozzáadása")
6. Kattintson a **Select engedélyeket**, hello engedélyként válassza, és hogy tooassign hello engedélyek valóban ACL alapértelmezés szerint, elérhetők a hozzáférés-vezérlési lista, vagy mindkettőt. Kattintson az **OK** gombra.
   
    ![Rendelje hozzá az engedélyek toogroup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "toogroup engedélyek hozzárendelése")
   
    A Data Lake Store és a hozzáférés-vezérlési listákat alapértelmezett/hozzáférési engedélyekkel kapcsolatos további információkért lásd: [hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).
7. A hello **egyéni hozzáférés hozzáadása** panelen kattintson a **OK**. hello újonnan hozzáadott csoporthoz tartozó hello engedélyeivel mostantól szerepel hello **hozzáférés** panelen.
   
    ![Rendelje hozzá az engedélyek toogroup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "toogroup engedélyek hozzárendelése")

## <a name="step-4-get-hello-oauth-20-token-endpoint-only-for-java-based-applications"></a>4. lépés: Hello OAuth 2.0 token-végpont lekérése (csak a Java-alapú alkalmazások)

1. Jelentkezzen be az új toohello [Azure-portálon](https://portal.azure.com) és hello bal oldali ablaktáblán kattintson az Active Directory.

2. Hello bal oldali ablaktáblában kattintson **App regisztrációk**.

3. Hello hello App regisztrációk panel felső részén kattintson **végpontok**.

    ![OAuth jogkivonat végpontjához](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "token OAuth-végpont")

4. Hello végpontok listáját másolja a hello OAuth 2.0 token-végpont.

    ![OAuth jogkivonat végpontjához](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "token OAuth-végpont")   

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben az Azure AD-webalkalmazás létrehozása, és szükséges az ügyfélalkalmazások használatával a .NET SDK, Java SDK stb szerzői hello információ áll rendelkezésre. Most már folytathatja a szolgáltatással kapcsolatban, hogyan toouse hello Azure AD webes alkalmazás toofirst a Data Lake Store hitelesítést, és hajtsa végre az egyéb műveletek hello tárolójában lévő cikkek a következő toohello.

* [Az Azure Data Lake Store használatának első lépései a .NET SDK-val](data-lake-store-get-started-net-sdk.md)
* [Az Azure Data Lake Store Java SDK használatának első lépései](data-lake-store-get-started-java-sdk.md)

Ez a cikk telefonon, hello lépéseken szükséges tooget keresztül egy felhasználó be egyszerű és fut, az alkalmazás. Megnézheti a cikkek tooget további információt a következő hello:
* [PowerShell toocreate egyszerű szolgáltatás használata](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [A szolgáltatás egyszerű hitelesítéshez Tanúsítványalapú hitelesítés használatára](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Egyéb módszerek tooauthenticate tooAzure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios)


