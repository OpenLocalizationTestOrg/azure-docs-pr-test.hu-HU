---
title: "Logic Apps B2B hibák és listája megoldások: Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Logic Apps B2B hibák és listája megoldások"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 0340e2979f1972ba631354e206c93969e55946e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a>Logic Apps B2B hibák és listája megoldások  
Ez a cikk segítséget nyújt a Logic Apps B2B esetekben fordulhat elő, és ezek a hibák kijavítása megfelelő műveleteket javasol előforduló hibák elhárítása.


## <a name="agreement-resolution"></a>A szerződés felbontás

### <a name="no-agreement-found"></a>* Nem található megállapodás 

|   |   |  
|---|---|
| Hiba leírása: | Nem felel meg a szerződés feloldási paraméterek megállapodás|    
| Felhasználói művelet | hello megállapodás egyeztetett üzleti identitások toohello integrációs fiókot hozzá kell adni.</br> hello üzleti identitások meg kell felelnie a toohello bemeneti üzenet azonosítók|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a>* Nem felel meg a identitások megállapodás

|   |   | 
|---|---|
| Hiba leírása: | Nem található olyan identitásokkal szerződés: 'AS2Identity':: "Partner1" és "AS2Identity":: "Partner3"| 
| Felhasználói művelet | Érvénytelen AS2-a vagy AS2-tooconfigured szerződés. </br> Megfelelő AS2 üzenet AS2-a vagy AS2 AS2-tooheaders vagy megállapodás toomatch AS2 azonosítók üzenet megállapodás konfigurációjú fejlécek |
|   |   |     

## <a name="as2"></a>AS2

### <a name="-missing-as2-message-headers"></a>* Hiányzó AS2 üzenetfejlécek  

|   |   |  
|---|---|
| Hiba leírása:| Érvénytelen AS2-fejléceket. Egy "AS2-a" vagy "AS2-a" fejléc üres| 
| Felhasználói művelet | Az AS2-üzenet érkezett, amely nem tartalmazott hello AS2-a vagy AS2-tooor mindkét fejléceket. </br> Ellenőrizze az AS2 üzenet AS2-származó és AS2-tooheaders és javítsa ki azokat a szerződés konfiguráció alapján |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a>* Hiányzó AS2 üzenettörzs és fejlécek    

|   |   |  
|---|---|
| Hiba leírása:| hello kérés tartalma értéke null vagy üres | 
| Felhasználói művelet | Egy AS2-üzenet érkezett, hello az üzenet törzse nem tartalmazó |
|  |  | 

### <a name="-as2-message-decryption-failure"></a>* AS2 üzenet visszafejtés sikertelen

|   |   | 
|---|---|
| Hiba leírása: |  [feldolgozott/hiba: a visszafejtés sikertelen] | 
| Felhasználói művelet | Adja hozzá @base64ToBinary tooAS2Message toopartner elküldése előtt 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a>* MDN visszafejtés sikertelen

|   |   | 
|---|---|
| Hiba leírása: |  [feldolgozott/hiba: a visszafejtés sikertelen] | 
| Felhasználói művelet | Adja hozzá @base64ToBinary tooMDN toopartner elküldése előtt 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a>* Hiányzó aláíró tanúsítvány

|   |   |  
|---|---|
| Hiba leírása:| hello aláíró tanúsítvány konfigurációja nem AS2 fél számára. </br> AS2-származó: partner1 AS2-való: partner2 | 
| Felhasználói művelet | Az aláírás megfelelő tanúsítvánnyal rendelkező AS2 megállapodás beállításainak konfigurálása |
|  |  | 

## <a name="x12-and-edifact"></a>X12 és EDIFACT

### <a name="-leading-or-trailing-space-found"></a>* Kezdő vagy záró található terület    
    
|   |   | 
|---|---|
| Hiba leírása: | Hiba történt az elemzés során. hello beállítása "szereplő interchange (csoport) nélkül 987654" ID"ID" 123456 Edifact tranzakció, Küldőazonosító "Partner1", a fogadó azonosítója "Partner2" felfüggeszti a következő hibákkal: vezető záró elválasztó található |
| Felhasználói művelet | hello megállapodás beállítások toobe tooallow kezdő és záró terület konfigurálva. </br> Szerződés beállítások tooallow kezdő és záró hely szerkesztése |
|   |   |

![hely engedélyezése](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-hello-agreement"></a>* Ismétlődések hello szerződésben engedélyezve

|   |   | 
|---|---| 
| Hiba leírása: | Ismétlődő ellenőrző szám |
| Felhasználói művelet | Ez a hiba azt jelzi, kapott üdvözlőüzenetére ismétlődő vezérlő számokat. </br> Javítsa ki a hello ellenőrző szám, és küldje el újból a köszönőüzenetei |
|   |   |

### <a name="-missing-schema-in-hello-agreement"></a>* Hiányzó séma hello megállapodás

|   |   | 
|---|---| 
| Hiba leírása: | Hiba történt az elemzés során. hello X12 tranzakció set azonosítójú "564220001' funkcionális csoportban szereplő azonosítójú"56422', "000056422" azonosítóval rendelkező Küldőazonosító "12345678 adatcsere a", "87654321 fogadó azonosítója" felfüggeszti a következő hibák miatt "üdvözlőüzenetére rendelkezik egy ismeretlen dokumentum most PE és nem tudta feloldani hello meglévő séma hello megállapodás konfigurált tooany " |
| Felhasználói művelet | Séma hello megállapodás beállításainak konfigurálása  |
|   |   |

### <a name="-incorrect-schema-in-hello-agreement"></a>* Hibás sémát hello megállapodás

|   |   | 
|---|---| 
| Hiba leírása: | üdvözlőüzenetére Ismeretlen dokumentum típussal rendelkezik, és nem tudta feloldani tooany hello meglévő séma hello megállapodás konfigurált. |
| Felhasználói művelet | Konfigurálja a megfelelő sémát hello megállapodás beállításai  |
|   |   |

## <a name="flat-file"></a>Egybesimított fájl

### <a name="-input-message-with-no-body"></a>* Bemeneti üzenet nem a szervezethez

|   |   | 
|---|---|
| Hiba leírása: | InvalidTemplate. Nem lehet tooprocess tartozó sablonnyelvi kifejezéseket a művelet "Flat_File_Decoding" bemeneti sor "1" és "1902" oszlop: "tulajdonság"tartalom"vár egy értéke azonban null kapott szükséges. Elérési út ".". |
| Felhasználói művelet | Ez a hiba azt jelzi, hogy hello bemeneti üzenet nem tartalmaz egy szervezet |
|   |   | 

## <a name="learn-more"></a>Részletek
[További tudnivalók hello vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md)
