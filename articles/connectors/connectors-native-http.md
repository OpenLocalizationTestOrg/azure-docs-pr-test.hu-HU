---
title: "a tetszőleges végpontot HTTP - Azure Logic Apps aaaCommunicate |} Microsoft Docs"
description: "Hozzon létre a logic apps, amely képes kommunikálni a tetszőleges végpontot HTTP Protokollon keresztül"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 9793601839437a2b880bdb81e15881270cacc963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http-action"></a>Ismerkedés a hello HTTP-művelet

Hello HTTP-művelet, a munkafolyamatok kiterjesztheti a szervezet és tooany végpont HTTP protokollt használó kommunikációra.

A következőket teheti:

* Hozzon létre programot (trigger) aktiválása, ha leáll a webhelyet, ahol kezelheti az alkalmazás munkafolyamatok.
* Tooany végpont protokollt használó kommunikációra HTTP tooextend a munkafolyamatokat az más szolgáltatásaiba.

tooget használatának hello HTTP-művelet a logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-http-trigger"></a>Hello HTTP-eseményindító használata
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](connectors-overview.md).

Íme egy parancssorozat-példa hogyan tooset hello HTTP be trigger hello Logic App Tervező.

1. Adja hozzá a logikai alkalmazás hello HTTP-eseményindítóval.
2. Töltse ki a megjeleníteni kívánt toopoll hello HTTP-végpont hello paraméterek.
3. Milyen gyakran le kell kérdeznie a hello ismétlődési időköz módosítása.

   hello logikai alkalmazás most már minden egyes ellenőrzése során visszaküldött tartalommal következik be.

   ![HTTP eseményindító](./media/connectors-native-http/using-trigger.png)

### <a name="how-hello-http-trigger-works"></a>Hello HTTP-eseményindítóval működése

hello HTTP-eseményindítóval küld egy hívás tooHTTP végpont ismétlődési időköz. Alapértelmezés szerint a HTTP-válaszkód, amely kisebb, mint 300 hatására a logic app toorun. toospecify hello logikai alkalmazás érvényesítést kell, hogy szerkesztheti hello logikai alkalmazást a kód nézetre, és adja hozzá a HTTP-hívása után hello kiértékelésére szolgáló feltétel. Íme egy példa egy HTTP-eseményindítóval, amely akkor következik be, ha a hello adja vissza. állapotkód: nagyobb vagy egyenlő túl`400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Részletes információ hello HTTP-trigger paraméterek érhetők el a [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-hello-http-action"></a>Hello HTTP művelettel

Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi. 
[További információ a műveletek](connectors-overview.md).

1. Válasszon **új lépés** > **művelet hozzáadása**.
3. Hello művelet keresési mezőbe, írja be a **http** toolist hello HTTP-műveletek.
   
    ![Válassza ki a hello HTTP-művelet](./media/connectors-native-http/using-action-1.png)

4. Adja hozzá a szükséges paramétereket hello HTTP hívásához.
   
    ![Teljes hello HTTP-művelet](./media/connectors-native-http/using-action-2.png)

5. A hello designer eszköztáron kattintson **mentése**. A Logic Apps alkalmazást mentése és hello (aktivált) közzétett ugyanannyi időt vesz igénybe.

## <a name="http-trigger"></a>HTTP eseményindító
Az alábbiakban hello részletek hello eseményindító, amely támogatja ezt az összekötőt. hello HTTP összekötő egy eseményindító tartozik.

| Eseményindító | Leírás |
| --- | --- |
| HTTP |Egy HTTP-hívást, és hello válasz tartalmat adja vissza. |

## <a name="http-action"></a>HTTP-művelet
Az alábbiakban hello részleteit, amely támogatja ezt az összekötőt hello a művelethez. hello HTTP összekötő lehetséges egy-egy művelettel rendelkezik.

| Műveletek | Leírás |
| --- | --- |
| HTTP |Egy HTTP-hívást, és hello válasz tartalmat adja vissza. |

## <a name="http-details"></a>HTTP-részletek
hello alábbi táblázatok bemutatják az hello szükséges és választható beviteli mezők hello művelet és hello megfelelő kimeneti részletekért társított hello műveletével.

#### <a name="http-request"></a>HTTP-kérelem
Az alábbiakban hello hello művelet, amely a kimenő HTTP-kérelem a beviteli mezők.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Módszer * |Módszer |hello HTTP-művelet toouse |
| URI * |URI |hello URI hello HTTP-kérelem |
| Fejlécek |Fejlécek |A HTTP-fejlécek tooinclude egy JSON-objektum |
| Törzs |Törzs |hello HTTP-kérelem törzse |
| Authentication |Hitelesítés |Hello részleteket [hitelesítési](#authentication) szakasz |

<br>

#### <a name="output-details"></a>Kimeneti részletei
Az alábbiakban hello kimeneti részletes hello HTTP-válasz.

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Fejlécek |Objektum |Válaszfejlécek |
| Törzs |Objektum |Válasz objektum |
| Állapotkód |int |HTTP-állapotkód: |

## <a name="authentication"></a>Authentication
hello Logic Apps szolgáltatás lehetővé teszi toouse különböző hitelesítési HTTP-végpontokról ellen. Ez a hitelesítés használatával hello **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, és  **[HTTP Webhook](connectors-native-webhook.md)**  összekötők. a következő típusú hitelesítés hello amelyek konfigurálhatók:

* [Az egyszerű hitelesítés](#basic-authentication)
* [Ügyféltanúsítvány-alapú hitelesítés](#client-certificate-authentication)
* [Az Azure Active Directory (Azure AD) OAuth-hitelesítés](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Az egyszerű hitelesítés

a következő hitelesítési objektumot hello az egyszerű hitelesítés szükséges.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Típus * |type |Hitelesítés típusa (lehet `Basic` az egyszerű hitelesítés) |
| Felhasználónév * |felhasználónév |Felhasználói név tooauthenticate |
| Jelszó * |jelszó |Jelszó tooauthenticate |

> [!TIP]
> Ha azt szeretné, hogy egy jelszót, amely nem kérhető le hello definition toouse, egy `securestring` paraméter és hello `@parameters()`  
>  [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs).

Példa:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Ügyféltanúsítvány-hitelesítés

hello következő hitelesítési objektum szükséges ügyféltanúsítvány-alapú hitelesítés. A * azt jelenti, hogy mezőt kötelező kitölteni.

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Típus * |type |hello típusú hitelesítés (kell `ClientCertificate` az ügyfél SSL-tanúsítványok) |
| PFX * |PFX |hello Base64-kódolású hello személyes információcseréhez kapcsolódó (PFX) fájl tartalma |
| Jelszó * |jelszó |hello jelszó tooaccess hello PFX-fájlból |

> [!TIP]
> egy paraméter, amely nem lesz olvasható hello definícióban hello logikai alkalmazás mentését követően toouse, használhatja a `securestring` paraméter és hello `@parameters()`  
>  [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs).

Példa:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Az Azure AD OAuth-hitelesítés
a következő hitelesítési objektumot hello Azure AD OAuth-hitelesítés szükséges. A * azt jelenti, hogy mezőt kötelező kitölteni.

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Típus * |type |hello típusú hitelesítés (kell `ActiveDirectoryOAuth` az Azure AD OAuth) |
| Bérlői * |Bérlői |hello bérlői azonosító hello Azure AD-bérlő számára |
| A célközönség * |Célközönség |a kért engedélyezési toouse hello erőforrás. Például:`https://management.core.windows.net/` |
| Ügyfél-azonosító * |clientId |hello hello Azure AD alkalmazás ügyfél-azonosítója |
| Titkos kulcs * |titkos kulcs |hello titkos hello jogkivonatot kérő hello ügyfél |

> [!TIP]
> Használhatja a `securestring` paraméter és hello `@parameters()` [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs) toouse egy paraméter, amely nem lesz olvasható, a mentés után hello-definícióban.
> 
> 

Példa:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Következő lépések
Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Ismerje meg a más rendelkezésre álló összekötők Logic Apps hello megnézzük a [API-k lista](apis-list.md).

