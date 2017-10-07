---
title: "aaaAzure API Management hitelesítési házirendek |} Microsoft Docs"
description: "További tudnivalók hello hitelesítési házirendek az Azure API Management használható."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: ce93cced66cb67520e97c7c15f3685bffb08e1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-authentication-policies"></a>Az API Management hitelesítési házirendek
Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AuthenticationPolicies"></a>Hitelesítési házirendek  
  
-   [Basic hitelesítés](api-management-authentication-policies.md#Basic) -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.  
  
-   [Az ügyféltanúsítvány hitelesítéséhez](api-management-authentication-policies.md#ClientCertificate) -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.  
  
##  <a name="Basic"></a>Alapszintű hitelesítés  
 Használjon hello `authentication-basic` házirend tooauthenticate alapszintű hitelesítést használ, egy háttér szolgáltatással. Ez a házirend hatékonyan hello HTTP engedélyezési fejléc toohello érték megfelelő toohello hitelesítő adatok beállítása a hello házirendekben.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|egyszerű hitelesítés|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|felhasználónév|Hello felhasználónév a hello alapvető hitelesítő adatok megadása|Igen|N/A|  
|jelszó|A hello alapvető hitelesítő adatok hello jelszót ad meg.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** API  
  
##  <a name="ClientCertificate"></a>Az ügyféltanúsítvány hitelesítéséhez  
 Használjon hello `authentication-certificate` házirend tooauthenticate egy háttér szolgáltatással ügyféltanúsítvány használatával. hello tanúsítványt kell toobe [telepítve az API Management](http://go.microsoft.com/fwlink/?LinkID=511599) első és az ujjlenyomat azonosít.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|hitelesítési tanúsítvány|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|ujjlenyomat|hello hello-ügyfél tanúsítványának ujjlenyomata.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** API  
  

## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  