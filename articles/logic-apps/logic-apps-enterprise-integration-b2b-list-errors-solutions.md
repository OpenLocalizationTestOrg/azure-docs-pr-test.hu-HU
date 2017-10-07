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
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a><span data-ttu-id="c68eb-103">Logic Apps B2B hibák és listája megoldások</span><span class="sxs-lookup"><span data-stu-id="c68eb-103">Logic Apps B2B list of errors and solutions</span></span>  
<span data-ttu-id="c68eb-104">Ez a cikk segítséget nyújt a Logic Apps B2B esetekben fordulhat elő, és ezek a hibák kijavítása megfelelő műveleteket javasol előforduló hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="c68eb-104">This article helps you troubleshoot errors that might happen in Logic Apps B2B scenarios and suggests appropriate actions for correcting those errors.</span></span>


## <a name="agreement-resolution"></a><span data-ttu-id="c68eb-105">A szerződés felbontás</span><span class="sxs-lookup"><span data-stu-id="c68eb-105">Agreement Resolution</span></span>

### <a name="no-agreement-found"></a><span data-ttu-id="c68eb-106">* Nem található megállapodás</span><span class="sxs-lookup"><span data-stu-id="c68eb-106">*No agreement found</span></span> 

|   |   |  
|---|---|
| <span data-ttu-id="c68eb-107">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-107">Error description</span></span> | <span data-ttu-id="c68eb-108">Nem felel meg a szerződés feloldási paraméterek megállapodás</span><span class="sxs-lookup"><span data-stu-id="c68eb-108">No agreement found with Agreement Resolution Parameters</span></span>|    
| <span data-ttu-id="c68eb-109">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-109">User action</span></span> | <span data-ttu-id="c68eb-110">hello megállapodás egyeztetett üzleti identitások toohello integrációs fiókot hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="c68eb-110">hello agreement should be added toohello integration account with agreed business identities.</span></span></br> <span data-ttu-id="c68eb-111">hello üzleti identitások meg kell felelnie a toohello bemeneti üzenet azonosítók</span><span class="sxs-lookup"><span data-stu-id="c68eb-111">hello business identities should match toohello input message ids</span></span>|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a><span data-ttu-id="c68eb-112">* Nem felel meg a identitások megállapodás</span><span class="sxs-lookup"><span data-stu-id="c68eb-112">* No agreement found with identities</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c68eb-113">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-113">Error description</span></span> | <span data-ttu-id="c68eb-114">Nem található olyan identitásokkal szerződés: 'AS2Identity':: "Partner1" és "AS2Identity":: "Partner3"</span><span class="sxs-lookup"><span data-stu-id="c68eb-114">No agreement found with identities: 'AS2Identity'::'Partner1' and'AS2Identity'::'Partner3'</span></span>| 
| <span data-ttu-id="c68eb-115">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-115">User action</span></span> | <span data-ttu-id="c68eb-116">Érvénytelen AS2-a vagy AS2-tooconfigured szerződés.</span><span class="sxs-lookup"><span data-stu-id="c68eb-116">Invalid AS2-From or AS2-tooconfigured for agreement.</span></span> </br> <span data-ttu-id="c68eb-117">Megfelelő AS2 üzenet AS2-a vagy AS2 AS2-tooheaders vagy megállapodás toomatch AS2 azonosítók üzenet megállapodás konfigurációjú fejlécek</span><span class="sxs-lookup"><span data-stu-id="c68eb-117">Correct AS2 message AS2-From or AS2-tooheaders or agreement toomatch AS2 ids in AS2 message headers with agreement configurations</span></span> |
|   |   |     

## <a name="as2"></a><span data-ttu-id="c68eb-118">AS2</span><span class="sxs-lookup"><span data-stu-id="c68eb-118">AS2</span></span>

### <a name="-missing-as2-message-headers"></a><span data-ttu-id="c68eb-119">* Hiányzó AS2 üzenetfejlécek</span><span class="sxs-lookup"><span data-stu-id="c68eb-119">* Missing AS2 message headers</span></span>  

|   |   |  
|---|---|
| <span data-ttu-id="c68eb-120">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-120">Error description</span></span>| <span data-ttu-id="c68eb-121">Érvénytelen AS2-fejléceket.</span><span class="sxs-lookup"><span data-stu-id="c68eb-121">Invalid AS2 headers.</span></span> <span data-ttu-id="c68eb-122">Egy "AS2-a" vagy "AS2-a" fejléc üres</span><span class="sxs-lookup"><span data-stu-id="c68eb-122">One of 'AS2-To' or 'AS2-From' headers are empty</span></span>| 
| <span data-ttu-id="c68eb-123">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-123">User action</span></span> | <span data-ttu-id="c68eb-124">Az AS2-üzenet érkezett, amely nem tartalmazott hello AS2-a vagy AS2-tooor mindkét fejléceket.</span><span class="sxs-lookup"><span data-stu-id="c68eb-124">An AS2 message was received that did not contain hello AS2-From or AS2-tooor both headers.</span></span> </br> <span data-ttu-id="c68eb-125">Ellenőrizze az AS2 üzenet AS2-származó és AS2-tooheaders és javítsa ki azokat a szerződés konfiguráció alapján</span><span class="sxs-lookup"><span data-stu-id="c68eb-125">Check AS2 message AS2-From and AS2-tooheaders and correct them based on agreement configuration</span></span> |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a><span data-ttu-id="c68eb-126">* Hiányzó AS2 üzenettörzs és fejlécek</span><span class="sxs-lookup"><span data-stu-id="c68eb-126">* Missing AS2 message body and headers</span></span>    

|   |   |  
|---|---|
| <span data-ttu-id="c68eb-127">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-127">Error description</span></span>| <span data-ttu-id="c68eb-128">hello kérés tartalma értéke null vagy üres</span><span class="sxs-lookup"><span data-stu-id="c68eb-128">hello request content is null or empty</span></span> | 
| <span data-ttu-id="c68eb-129">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-129">User action</span></span> | <span data-ttu-id="c68eb-130">Egy AS2-üzenet érkezett, hello az üzenet törzse nem tartalmazó</span><span class="sxs-lookup"><span data-stu-id="c68eb-130">An AS2 message was received that did not contain hello message body</span></span> |
|  |  | 

### <a name="-as2-message-decryption-failure"></a><span data-ttu-id="c68eb-131">* AS2 üzenet visszafejtés sikertelen</span><span class="sxs-lookup"><span data-stu-id="c68eb-131">* AS2 message decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c68eb-132">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-132">Error description</span></span> |  <span data-ttu-id="c68eb-133">[feldolgozott/hiba: a visszafejtés sikertelen]</span><span class="sxs-lookup"><span data-stu-id="c68eb-133">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="c68eb-134">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-134">User action</span></span> | <span data-ttu-id="c68eb-135">Adja hozzá @base64ToBinary tooAS2Message toopartner elküldése előtt</span><span class="sxs-lookup"><span data-stu-id="c68eb-135">Add @base64ToBinary tooAS2Message before sending toopartner</span></span> 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a><span data-ttu-id="c68eb-136">* MDN visszafejtés sikertelen</span><span class="sxs-lookup"><span data-stu-id="c68eb-136">* MDN decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c68eb-137">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-137">Error description</span></span> |  <span data-ttu-id="c68eb-138">[feldolgozott/hiba: a visszafejtés sikertelen]</span><span class="sxs-lookup"><span data-stu-id="c68eb-138">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="c68eb-139">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-139">User action</span></span> | <span data-ttu-id="c68eb-140">Adja hozzá @base64ToBinary tooMDN toopartner elküldése előtt</span><span class="sxs-lookup"><span data-stu-id="c68eb-140">Add @base64ToBinary tooMDN before sending toopartner</span></span> 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a><span data-ttu-id="c68eb-141">* Hiányzó aláíró tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="c68eb-141">* Missing signing certificate</span></span>

|   |   |  
|---|---|
| <span data-ttu-id="c68eb-142">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-142">Error description</span></span>| <span data-ttu-id="c68eb-143">hello aláíró tanúsítvány konfigurációja nem AS2 fél számára.</span><span class="sxs-lookup"><span data-stu-id="c68eb-143">hello Signing Certificate has not been configured for AS2 party.</span></span> </br> <span data-ttu-id="c68eb-144">AS2-származó: partner1 AS2-való: partner2</span><span class="sxs-lookup"><span data-stu-id="c68eb-144">AS2-From: partner1 AS2-To: partner2</span></span> | 
| <span data-ttu-id="c68eb-145">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-145">User action</span></span> | <span data-ttu-id="c68eb-146">Az aláírás megfelelő tanúsítvánnyal rendelkező AS2 megállapodás beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c68eb-146">Configure AS2 agreement settings with correct certificate for signature</span></span> |
|  |  | 

## <a name="x12-and-edifact"></a><span data-ttu-id="c68eb-147">X12 és EDIFACT</span><span class="sxs-lookup"><span data-stu-id="c68eb-147">X12 and EDIFACT</span></span>

### <a name="-leading-or-trailing-space-found"></a><span data-ttu-id="c68eb-148">* Kezdő vagy záró található terület</span><span class="sxs-lookup"><span data-stu-id="c68eb-148">* Leading or trailing space found</span></span>    
    
|   |   | 
|---|---|
| <span data-ttu-id="c68eb-149">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-149">Error description</span></span> | <span data-ttu-id="c68eb-150">Hiba történt az elemzés során.</span><span class="sxs-lookup"><span data-stu-id="c68eb-150">Error encountered during parsing.</span></span> <span data-ttu-id="c68eb-151">hello beállítása "szereplő interchange (csoport) nélkül 987654" ID"ID" 123456 Edifact tranzakció, Küldőazonosító "Partner1", a fogadó azonosítója "Partner2" felfüggeszti a következő hibákkal: vezető záró elválasztó található</span><span class="sxs-lookup"><span data-stu-id="c68eb-151">hello Edifact transaction set with id '123456' contained in interchange (without group) with id '987654', with sender id 'Partner1', receiver id 'Partner2' is being suspended with following errors: Leading Trailing separator found</span></span> |
| <span data-ttu-id="c68eb-152">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-152">User action</span></span> | <span data-ttu-id="c68eb-153">hello megállapodás beállítások toobe tooallow kezdő és záró terület konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c68eb-153">hello agreement settings toobe configured tooallow leading and trailing space.</span></span> </br> <span data-ttu-id="c68eb-154">Szerződés beállítások tooallow kezdő és záró hely szerkesztése</span><span class="sxs-lookup"><span data-stu-id="c68eb-154">Edit agreement settings tooallow leading and trailing space</span></span> |
|   |   |

![hely engedélyezése](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-hello-agreement"></a><span data-ttu-id="c68eb-156">* Ismétlődések hello szerződésben engedélyezve</span><span class="sxs-lookup"><span data-stu-id="c68eb-156">* Duplicate check has enabled in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="c68eb-157">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-157">Error description</span></span> | <span data-ttu-id="c68eb-158">Ismétlődő ellenőrző szám</span><span class="sxs-lookup"><span data-stu-id="c68eb-158">Duplicate Control Number</span></span> |
| <span data-ttu-id="c68eb-159">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-159">User action</span></span> | <span data-ttu-id="c68eb-160">Ez a hiba azt jelzi, kapott üdvözlőüzenetére ismétlődő vezérlő számokat.</span><span class="sxs-lookup"><span data-stu-id="c68eb-160">This error indicates hello received message has duplicate control numbers.</span></span> </br> <span data-ttu-id="c68eb-161">Javítsa ki a hello ellenőrző szám, és küldje el újból a köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="c68eb-161">Correct hello control number and resend hello message</span></span> |
|   |   |

### <a name="-missing-schema-in-hello-agreement"></a><span data-ttu-id="c68eb-162">* Hiányzó séma hello megállapodás</span><span class="sxs-lookup"><span data-stu-id="c68eb-162">* Missing schema in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="c68eb-163">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-163">Error description</span></span> | <span data-ttu-id="c68eb-164">Hiba történt az elemzés során.</span><span class="sxs-lookup"><span data-stu-id="c68eb-164">Error encountered during parsing.</span></span> <span data-ttu-id="c68eb-165">hello X12 tranzakció set azonosítójú "564220001' funkcionális csoportban szereplő azonosítójú"56422', "000056422" azonosítóval rendelkező Küldőazonosító "12345678 adatcsere a", "87654321 fogadó azonosítója" felfüggeszti a következő hibák miatt "üdvözlőüzenetére rendelkezik egy ismeretlen dokumentum most PE és nem tudta feloldani hello meglévő séma hello megállapodás konfigurált tooany "</span><span class="sxs-lookup"><span data-stu-id="c68eb-165">hello X12 transaction set with id '564220001' contained in functional group with id '56422', in interchange with id '000056422', with sender id '12345678       ', receiver id '87654321       ' is being suspended with following errors "hello message has an unknown document type and did not resolve tooany of hello existing schemas configured in hello agreement"</span></span> |
| <span data-ttu-id="c68eb-166">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-166">User action</span></span> | <span data-ttu-id="c68eb-167">Séma hello megállapodás beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c68eb-167">Configure schema in hello agreement settings</span></span>  |
|   |   |

### <a name="-incorrect-schema-in-hello-agreement"></a><span data-ttu-id="c68eb-168">* Hibás sémát hello megállapodás</span><span class="sxs-lookup"><span data-stu-id="c68eb-168">* Incorrect schema in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="c68eb-169">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-169">Error description</span></span> | <span data-ttu-id="c68eb-170">üdvözlőüzenetére Ismeretlen dokumentum típussal rendelkezik, és nem tudta feloldani tooany hello meglévő séma hello megállapodás konfigurált.</span><span class="sxs-lookup"><span data-stu-id="c68eb-170">hello message has an unknown document type and did not resolve tooany of hello existing schemas configured in hello agreement.</span></span> |
| <span data-ttu-id="c68eb-171">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-171">User action</span></span> | <span data-ttu-id="c68eb-172">Konfigurálja a megfelelő sémát hello megállapodás beállításai</span><span class="sxs-lookup"><span data-stu-id="c68eb-172">Configure correct schema in hello agreement settings</span></span>  |
|   |   |

## <a name="flat-file"></a><span data-ttu-id="c68eb-173">Egybesimított fájl</span><span class="sxs-lookup"><span data-stu-id="c68eb-173">Flat file</span></span>

### <a name="-input-message-with-no-body"></a><span data-ttu-id="c68eb-174">* Bemeneti üzenet nem a szervezethez</span><span class="sxs-lookup"><span data-stu-id="c68eb-174">* Input message with no body</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c68eb-175">Hiba leírása:</span><span class="sxs-lookup"><span data-stu-id="c68eb-175">Error description</span></span> | <span data-ttu-id="c68eb-176">InvalidTemplate.</span><span class="sxs-lookup"><span data-stu-id="c68eb-176">InvalidTemplate.</span></span> <span data-ttu-id="c68eb-177">Nem lehet tooprocess tartozó sablonnyelvi kifejezéseket a művelet "Flat_File_Decoding" bemeneti sor "1" és "1902" oszlop: "tulajdonság"tartalom"vár egy értéke azonban null kapott szükséges.</span><span class="sxs-lookup"><span data-stu-id="c68eb-177">Unable tooprocess template language expressions in action 'Flat_File_Decoding' inputs at line '1' and column '1902': 'Required property 'content' expects a value but got null.</span></span> <span data-ttu-id="c68eb-178">Elérési út ".".</span><span class="sxs-lookup"><span data-stu-id="c68eb-178">Path ''.'.</span></span> |
| <span data-ttu-id="c68eb-179">Felhasználói művelet</span><span class="sxs-lookup"><span data-stu-id="c68eb-179">User action</span></span> | <span data-ttu-id="c68eb-180">Ez a hiba azt jelzi, hogy hello bemeneti üzenet nem tartalmaz egy szervezet</span><span class="sxs-lookup"><span data-stu-id="c68eb-180">This error indicates hello input message does not contain a body</span></span> |
|   |   | 

## <a name="learn-more"></a><span data-ttu-id="c68eb-181">Részletek</span><span class="sxs-lookup"><span data-stu-id="c68eb-181">Learn more</span></span>
[<span data-ttu-id="c68eb-182">További tudnivalók hello vállalati integrációs csomag</span><span class="sxs-lookup"><span data-stu-id="c68eb-182">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
