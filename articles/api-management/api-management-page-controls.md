---
title: "az API Management Lapvezérlők aaaAzure |} Microsoft Docs"
description: "További információk a hello Lapvezérlők fejlesztői portál sablonok az Azure API Management használható."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1a16a6fce197c0a2e14807ac21e81a9a73b515b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-page-controls"></a>Az Azure API Management Lapvezérlők
Az Azure API Management vezérlők hello fejlesztői portál sablonok használható a következő hello biztosít.  
  
 a vezérlő toouse helyezze el szükséges hello helyen hello fejlesztői portálsablon. Egyes vezérlők, például a hello [alkalmazás-műveletek](#app-actions) kontrolljához rendelkezik paraméterekkel, ahogy az alábbi példa hello.  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 hello hello paraméter értékét a átadott hello adatmodell hello sablon részeként. A legtöbb esetben egyszerűen illessze be a hello megadott példa minden vezérlőjének, amíg toowork megfelelően. Hello paraméterértékeket a további információkért lásd: modell adatszakasz hello minden sablon egy vezérlő használja.  
  
 A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
## <a name="developer-portal-template-page-controls"></a>Fejlesztői portálsablon Lapvezérlők  
  
-   [alkalmazás-műveletek](#app-actions)  
  
-   [Basic-bejelentkezés](#basic-signin)  
  
-   [lapozófájl-vezérlő](#paging-control)  
  
-   [szolgáltatók](#providers)  
  
-   [keresési-vezérlő](#search-control)  
  
-   [-előfizetés](#sign-up)  
  
-   [előfizetés gomb](#subscribe-button)  
  
-   [előfizetés-Mégse](#subscription-cancel)  
  
##  <a name="app-actions"></a>alkalmazás-műveletek  
 Hello `app-actions` vezérlő felhasználói felülete lehetőséget nyújt az alkalmazások hello felhasználói profil lapon hello developer portálon.  
  
 ![alkalmazás &#45; műveletek vezérlő](./media/api-management-page-controls/APIM-app-actions-control.png "APIM alkalmazás-műveletek vezérlő")  
  
### <a name="usage"></a>Használat  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Paraméterek  
  
|Paraméter|Leírás|  
|---------------|-----------------|  
|Alkalmazásazonosító|hello hello alkalmazás azonosítója.|  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `app-actions` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [Alkalmazások](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a>Basic-bejelentkezés  
 Hello `basic-signin` vezérlő egy felügyeletét biztosítja, a hello adatok gyűjtését a felhasználói bejelentkezési oldal hello developer portálon bejelentkezhet.  
  
 ![Basic &#45; signin vezérlő](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin vezérlő")  
  
### <a name="usage"></a>Használat  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Paraméterek  
 nincs.  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `basic-signin` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [bejelentkezés](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a>lapozófájl-vezérlő  
 Hello `paging-control` lapozás a fejlesztői portálon lapokat biztosít, amelyek elemek listájának megjelenítéséhez.  
  
 ![vezérlő lapozási](./media/api-management-page-controls/APIM-paging-control.png "APIM lapozási vezérlő")  
  
### <a name="usage"></a>Használat  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Paraméterek  
 nincs.  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `paging-control` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [API-lista](api-management-api-templates.md#APIList)  
  
-   [Problémák listája](api-management-issue-templates.md#IssueList)  
  
-   [Termékek listáját](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a>szolgáltatók  
 Hello `providers` vezérlője egy vezérlő kiválasztásának hitelesítésszolgáltatók a hello bejelentkezési oldal hello developer portálon.  
  
 ![szolgáltatók vezérlő](./media/api-management-page-controls/APIM-providers-control.png "APIM szolgáltatók vezérlő")  
  
### <a name="usage"></a>Használat  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Paraméterek  
 nincs.  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `providers` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [bejelentkezés](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a>keresési-vezérlő  
 Hello `search-control` keresési funkciókat biztosítja a fejlesztői portál által felügyelt oldalak felhívásainak elemek listájának megjelenítéséhez.  
  
 ![Keresés vezérlő](./media/api-management-page-controls/APIM-search-control.png "APIM keresési vezérlőelemet")  
  
### <a name="usage"></a>Használat  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Paraméterek  
 nincs.  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `search-control` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [API-lista](api-management-api-templates.md#APIList)  
  
-   [Termékek listáját](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a>-előfizetés  
 Hello `sign-up` vezérlő biztosít egy vezérlő hello bejelentkezési oldal hello developer portálon a felhasználói profillal kapcsolatos információk gyűjtéséhez.  
  
 ![bejelentkezési &#45; vezérlő legfeljebb](./media/api-management-page-controls/APIM-sign-up-control.png "APIM előfizetési vezérlő")  
  
### <a name="usage"></a>Használat  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Paraméterek  
 nincs.  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `sign-up` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [feliratkozni](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a>előfizetés gomb  
 Hello `subscribe-button` vezérlőt biztosít a felhasználó tooa termék előfizetés.  
  
 ![előfizetés &#45; vezérlőelem](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM előfizetés gomb vezérlőelem")  
  
### <a name="usage"></a>Használat  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Paraméterek  
 nincs.  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `subscribe-button` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [A termék](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a>előfizetés-Mégse  
 Hello `subscription-cancel` vezérlője egy vezérlő törlésének egy előfizetés tooa termék hello felhasználói profil lapon hello developer portálon.  
  
 ![előfizetés &#45; szakítsa meg a vezérlő](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM előfizetés-Mégse vezérlő")  
  
### <a name="usage"></a>Használat  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Paraméterek  
  
|Paraméter|Leírás|  
|---------------|-----------------|  
|subscriptionId|hello előfizetés toocancel hello azonosítója.|  
|CancelUrl|hello előfizetés szakítsa meg az URL-CÍMÉT.|  
  
### <a name="developer-portal-templates"></a>Fejlesztői portál sablonok  
 Hello `subscription-cancel` hello fejlesztői portál sablonok a következő vezérlő is használható.  
  
-   [A termék](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Következő lépések
A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).
