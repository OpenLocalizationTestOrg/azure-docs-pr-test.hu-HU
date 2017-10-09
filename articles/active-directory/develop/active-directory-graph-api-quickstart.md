---
title: az Azure AD Graph API hello aaaQuickstart |} Microsoft Docs
description: "hello Azure Active Directory Graph API-val programozott hozzáférés tooAzure AD OData REST API-végpontokon keresztül biztosít. Alkalmazások használhatják hello Graph API tooperform létrehozása, olvasása, frissítése és Törlés (CRUD) típusú műveletek directory adatok és objektumok."
services: active-directory
documentationcenter: n/a
author: viv-liu
manager: mbaldwin
editor: 
tags: 
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: b4d3c57f06d212b1d095578f19bb86c932dbcc33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-hello-azure-ad-graph-api"></a>Az Azure AD Graph API hello gyors üzembe helyezés
hello Azure Active Directory (AD) Graph API-val programozott hozzáférés tooAzure AD OData REST API-végpontokon keresztül biztosít. Alkalmazások használhatják hello Graph API tooperform létrehozása, olvasása, frissítése és Törlés (CRUD) típusú műveletek directory adatok és objektumok. Például akkor is hello Graph API toocreate egy új felhasználóhoz nézet használata vagy felhasználó tulajdonságainak frissítése, jelszó módosítása, ellenőrizze a szerepköralapú hozzáférés-, tiltsa le vagy delete hello felhasználói csoport tagságát. Hello Graph API-funkciókat és az alkalmazás-forgatókönyvek további információkért lásd: [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) és [Azure AD Graph API Előfeltételek](https://msdn.microsoft.com/library/hh974476.aspx). 

> [!IMPORTANT]
> Határozottan javasoljuk, hogy használjon [Microsoft Graph](https://developer.microsoft.com/graph) helyett az Azure AD Graph API tooaccess Azure Active Directory-erőforrásokra. A fejlesztési energiáinkat mostantól a Microsoft Graph-ra koncentráljuk, az Azure AD Graph API-hoz nem tervezünk további fejlesztéseket. Nagyon korlátozott számú forgatókönyvek, amelyek az Azure AD Graph API továbbra is megfelelő lehet; További információkért lásd: hello [Microsoft Graph vagy hello Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) hello Office fejlesztői központban a blogbejegyzést.
> 
> 

## <a name="how-tooconstruct-a-graph-api-url"></a>Hogyan tooconstruct egy grafikonon API URL-címe
A Graph API-val, tooaccess címtáradatokat és objektumokat (más szóval erőforrásokat és entitások) amely tooperform CRUD műveleteihez URL-címek alapján hello Open Data (OData) protokollt is használhatja. hello Graph API-ban használt URL-címeket négy fő részből állnak: szolgáltatás a legfelső szintű, a bérlő azonosítója, az erőforrás elérési útja és a lekérdezési karakterlánc lehetőségek: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Hello példánál az URL-cím a következő hello: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Szolgáltatás legfelső szintű**: az Azure AD Graph API hello szolgáltatás gyökere mindig https://graph.windows.net.
* **A bérlői azonosító**: Ez a szakasz lehet (regisztrált) ellenőrzött tartomány nevét, az előző példában hello contoso.com. Azt is egy bérlő objektum azonosítója vagy hello "Futrinka nevű" vagy "me" alias. További információkért lásd: [címzési entitásokat és a Graph API hello műveletek](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
* **Erőforrás elérési útja**: Ez a szakasz egy URL-cím azonosítja hello erőforrás toobe kezelni (felhasználók, csoportok, egy adott felhasználó vagy egy adott csoport stb.) Hello a fenti példában az erőforráshoz beállított hello legfelső szintű "csoportok" tooaddress is. Is megoldható, egy adott entitás, például "felhasználók / {objectId}" vagy "felhasználók/userPrincipalName tulajdonsághoz".
* **Lekérdezési paramétert**: A kérdőjel (?) elválasztja hello erőforrás elérésiút-szakaszával hello lekérdezési paraméterek szakaszban. Graph API hello összes kérelem hello "api-verzió" lekérdezési paraméter megadása szükséges. hello Graph API is támogatja a következő OData-lekérdezés beállításai hello: **$filter**, **$orderby**, **$expand argumentum**, **$top**, és **$format**. a következő lekérdezési lehetőségek hello jelenleg nem támogatottak: **$count**, **$inlinecount**, és **$skip**. További információkért lásd: [támogatott lekérdezések, szűrők és beállítások lapozás Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API-verziók
A Graph API-kérelmek hello verzió hello "api-verzió" lekérdezési paraméter adható meg. Az 1.5-ös vagy újabb verzió az numerikus verzió érték; az API-version 1.6-os =. Korábbi verzióihoz egy dátumkarakterlánc toohello formátum éééé-hh-nn; megfelelő használata például az api-version = 2013-11-08. Előzetes verziójú funkciók használata a hello karakterlánc "béta"; például az api-version = béta. A Graph API-verziók közötti különbségekről további információkért lásd: [Azure AD Graph API Versioning](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API-metaadatok
tooreturn hello Graph API metaadatfájl, vegye fel hello "$metadata" szegmens után hello bérlő-azonosító hello URL-címet a példában hello URL-cím beolvasása metaadatok bemutató számára a következő: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Ez az URL egy webes böngésző toosee hello metaadatok hello címsorába adhat meg. hello CSDL metaadat-dokumentum visszaadott hello entitások és komplex típusok, és írja le a tulajdonságait, és hello funkciók által kért Graph API hello verziója műveletek. Az alkalmazás legújabb verziójára hello metaadatokat hello api-version paraméter kihagyása adja vissza.

## <a name="common-queries"></a>Általános lekérdezések
[Az Azure AD Graph API általános lekérdezések](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) együtt hello Azure AD Graph, beleértve a lekérdezések, amelyek a címtárban a címtár és a lekérdezések tooperform műveleteket használt tooaccess a legfelső szintű erőforrások lehetnek közös lekérdezések sorolja fel.

Például `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` értéket ad vissza a vállalati címtár contoso.com adatait.

Vagy `https://graph.windows.net/contoso.com/users?api-version=1.6` hello directory contoso.com összes felhasználói objektumok listája.

## <a name="using-hello-graph-explorer"></a>Hello Graph Explorer használatával
Használhat hello Graph Explorer hello Azure AD Graph API tooquery hello címtáradatok az alkalmazás létrehozása.

hello az alábbiakban látható hello kimeneti volna toonavigate toohello Graph Explorer látjuk, bejelentkezés és írja be `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` összes hello felhasználók toodisplay hello bejelentkezett felhasználó könyvtára:

![Az Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Betöltési hello Graph Explorer**: tooload hello eszköz, keresse meg a túl[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/). Kattintson a **bejelentkezési** , és jelentkezzen be az Azure AD-fiók hitelesítő adatait toorun Graph Explorer hello a bérlő ellen. Graph Explorer futtatni saját bérlőt, ha Ön vagy a rendszergazda kell tooconsent bejelentkezés során. Ha olyan Office 365-előfizetéssel rendelkezik, automatikusan rendelkezik az Azure AD-bérlő. hello segítségével toosign tooOffice 365 hitelesítő adatok, valójában, az Azure AD-fiókok, és ezeket a hitelesítő adatokat használhatja Graph Explorer.

**A lekérdezés futtatásához**: toorun lekérdezés, írja be a lekérdezést hello kérelem szöveges értéket, majd kattintson a **beolvasása** , vagy kattintson a hello **adja meg** kulcs. hello eredmények hello válasz mezőben jelennek meg. Például `https://graph.windows.net/myorganization/groups?api-version=1.6` hello bejelentkezett felhasználó az összes csoport objektumok listája.

Megjegyzés: hello a következő szolgáltatásokat és a Graph Explorer hello korlátozásai:

* Erőforrás automatikus kiegészítési funkció beállítása. toosee ezt a funkciót, kattintson a hello kérelmet (ahol hello vállalat URL-címe megjelenik) szövegmezőben. Kiválaszthatja, hogy egy erőforráskészletet a hello legördülő listából.
* Támogatja a "me" és "futrinka" nevű aliasok címzési hello. Használhat például `https://graph.windows.net/me?api-version=1.6` tooreturn hello felhasználói objektum hello bejelentkezett felhasználó vagy `https://graph.windows.net/myorganization/users?api-version=1.6` tooreturn hello jelenlegi címtárban szereplő összes felhasználó.
* A válasz fejlécek szakasz. Ez a szakasz használható toohelp lekérdezések futtatásakor előforduló problémák elhárítása.
* Bontsa ki és összecsukása képességekkel hello válasz egy JSON megjelenítőből.
* Nem támogatja a miniatűr fényképre megjelenítése.

## <a name="using-fiddler-toowrite-toohello-directory"></a>Fiddler toowrite toohello directory használatával
Az első lépések útmutató hello célokra használhatja hello Fiddler webes hibakereső toopractice "write" az Azure AD-címtár műveleteket végez. További információk és tooinstall Fiddler [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Hello az alábbi példában a Fiddler webes hibakereső toocreate egy új biztonsági csoport "MyTestGroup" az Azure AD-címtár segítségével.

**Szerezze be a hozzáférési token**: Azure AD Graph tooaccess, az ügyfelek kapcsolódnak szükséges toosuccessfully tooAzure AD először hitelesítéséhez. További információkért lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

**Írása, és futtassa a lekérdezést**: teljes hello a következő lépéseket:

1. Nyissa meg a Fiddler webes hibakereső és egy kapcsolót toohello **szerkesztő** fülre.
2. Mivel a kívánt toocreate egy új biztonsági csoportot, válassza **Post** , hello hello legördülő menüből a HTTP-metódus. Objektum műveletek és engedélyekkel kapcsolatos további információkért lásd: [csoport](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) belül hello [az Azure AD Graph REST API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. A hello mező melletti túl**Post**, hello következő adja meg, a kérelem URL-címe hello: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.
   
   > [!NOTE]
   > A saját Azure AD-címtár hello tartománynévvel mytenantdomain kell behelyettesíteni.
   > 
   > 
4. Hello közvetlenül alatt Post legördülő mezőben írja be a hello következő:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Helyettesítő a &lt;a hozzáférési token&gt; hello jogkivonatokkal az Azure AD-címtárát.
   > 
   > 
5. A hello **Request body** mezőbe írja be a következő hello:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Csoportok létrehozásával kapcsolatos további információkért lásd: [csoport létrehozása](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

További információk az Azure AD entitásokat és a grafikon által típusok és a Graph végezhető rajtuk hello-műveletekkel kapcsolatos információkat lásd: [az Azure AD Graph REST API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* További információ [az Azure AD Graph API-Engedélyhatókörök](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

