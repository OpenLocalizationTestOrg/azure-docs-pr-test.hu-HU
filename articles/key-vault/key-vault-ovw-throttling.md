---
ms.assetid: 
title: "Key Vault szabályozási útmutatást aaaAzure |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: a75cf96bc6503e51f14378bee598bad57e85be82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-throttling-guidance"></a>Az Azure Key Vault útmutatást szabályozása

Sávszélesség-szabályozás rendkívül elindította folyamat, amely korlátozza a hello száma párhuzamos hívások toohello Azure szolgáltatás tooprevent túlhasználat erőforrások. Az Azure Key Vault (AKV) kialakított toohandle nagy mennyiségű kérést. Egy túlságosan kérelmek száma akkor fordul elő, ha az ügyfélkérelmek sávszélesség-szabályozás optimális teljesítménye és megbízhatósága hello AKV szolgáltatás karbantartása.

Sávszélesség-szabályozási korlátok hello forgatókönyv függően változhat. Például ha nagy mennyiségű írási műveleteket hajt végre, hello lehetősége, hogy a sávszélesség-szabályozás értéke magasabb, mint ha olvasási műveletek csak végzik.

## <a name="how-does-key-vault-handle-its-limits"></a>Hogyan nem kezelik a teljes kapacitásukkal működjenek a Key Vault?

A Key Vault szolgáltatásra vonatkozó korlátozások-e az erőforrások tooprevent visszaélés, és összes Key Vault ügyfelek minőségének biztosítása. A szolgáltatás küszöbérték túllépésekor a Key Vault semmilyen további kérelmeit, hogy az ügyfél korlátozza egy ideig. Ha ez történik, a Key Vault HTTP-állapotkód 429 adja vissza (túl sok kérelem), és hello kérelmek sikertelenek. Is sikertelen kérelmek 429-es jelű szerepleni hello szabályozási korlátok követik a Key Vault felé. 

Ha a nagyobb mértékű késleltetési korlátozás érvényes üzleti eset, lépjen kapcsolatba velünk a következő címen.


## <a name="how-toothrottle-your-app-in-response-tooservice-limits"></a>Hogyan toothrottle az alkalmazást a válasz tooservice korlátozza

hello az alábbiakban **ajánlott eljárások** szabályozás az alkalmazást:
- Csökkentse a kérelmenként műveletek hello számát.
- Csökkentse a kérelmek hello gyakoriságát.
- Ne használjon közvetlen újrapróbálkozások. 
    - Összes kérelem keletkeznek a használati korlátozások.

Az alkalmazás hibakezelési bevezetésekor használata hello HTTP hiba kód 429-es jelű toodetect hello ügyféloldali szabályozás kell. Ha hello kérelem végrehajtása ismét sikertelen hibakódú HTTP 429, továbbra is találkozik egy Azure-szolgáltatások korlátot. Továbbra is toouse hello ajánlott az ügyféloldali szabályozási metódus hello kérelem újrapróbálkozás, amíg azt nem jár sikerrel.

### <a name="recommended-client-side-throttling-method"></a>Ajánlott az ügyféloldali szabályozási módszer

A HTTP-hibakódot 429 kezdje a sávszélesség-szabályozás exponenciális leállítási módszer használatát az ügyfél el:

1. Várjon 1 másodperc, ismételje meg a kérést
2. Ha továbbra is halmozódni várjon 2 másodperc, próbálkozzon újra a kéréssel
3. Ha továbbra is halmozódni várakozási 4 másodperc, próbálkozzon újra a kéréssel
4. Ha továbbra is halmozódni várakozási 8 másodperc, próbálkozzon újra a kéréssel
5. Ha továbbra is halmozódni várakozási 16 másodperc, próbálkozzon újra a kéréssel

Ekkor meg kell jut HTTP 429 válaszkódot.

## <a name="see-also"></a>Lásd még:

A sávszélesség-szabályozás a Microsoft Cloud hello mélyebb tájolását, lásd: [sávszélesség-szabályozás mintát](https://docs.microsoft.com/azure/architecture/patterns/throttling).

