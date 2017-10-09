---
title: "aaaAzure esemény rács kézbesítési, és próbálkozzon újra"
description: "Ismerteti, hogyan nyújt az Azure esemény rács a eseményeket, és hogyan kezeli az kézbesítetlen üzenetek."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: 874b3bf8892fbf803ef40f29d0ec10eb50150916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-message-delivery-and-retry"></a>Esemény rács üzenetkézbesítést, és próbálkozzon újra 

Ez a cikk ismerteti, miként kezeli az Azure esemény rács a kézbesítési nem vonatkozik, az eseményeket.

Esemény rács tartós kézbesítési biztosít. Minden üzenet legalább egyszer az egyes előfizetésekhez biztosítja. Események azonnal elküldi a rendszer minden egyes előfizetés regisztrálva toohello webhook. Ha egy webhook nem átvételét esemény hello első szállítási 60 másodpercen belül kísérlet, esemény rács újrapróbálja hello esemény kézbesítését.

## <a name="message-delivery-status"></a>Üzenet kézbesítési állapota

Esemény rács használja az események HTTP válasz kódok tooacknowledge fogadását. 

### <a name="success-codes"></a>Sikeres kódok

hello következő HTTP-válaszkódot azt jelzi, hogy az esemény szállított sikeresen tooyour webhook. Esemény rács úgy ítéli meg kézbesítési befejeződött.

- 200 OK
- 202 elfogadott

### <a name="failure-codes"></a>Hibát kódok

a következő HTTP-válaszkódot hello azt jelzi, hogy az esemény kézbesítési tett kísérlet meghiúsult. Esemény rács újrapróbálkozik toosend hello esemény. 

- 400 Hibás kérés
- 401 nem engedélyezett
- 404 – Nem található
- 408 kérelmi időkorlátot.
- 414 URI túl hosszú
- 500 belső kiszolgálóhiba
- 503-as szolgáltatás nem érhető el
- 504-es számú átjáró időtúllépése

Bármely más válaszkód vagy hiányoznak a választ egy hibát jelez. Esemény rács kézbesítési próbálkozások. 

## <a name="retry-intervals"></a>Újrapróbálkozási időközök

Esemény rács az exponenciális leállítási újrapróbálkozási házirend esemény kézbesítési használ. A webhook nem válaszol vagy hibakódot ad vissza, ha az esemény rács újrapróbálkozik a következő ütemezés hello kézbesítési:

1. 10 másodperc
2. 30 másodperc
3. 1 perc
4. 5 perc
5. 10 perc
6. 30 perc
7. 1 óra

Esemény rács hozzáadja a kisméretű véletlenszerű tooall újrapróbálkozási időközönként.

## <a name="retry-duration"></a>Ismételje meg az időtartama

Hello előzetes Azure esemény rács összes eseményt, amely nem érkeznek meg két órán belül lejár. Általánosan rendelkezésre álló verzió előtt a most nagyobb too24 óra lesz. 

## <a name="next-steps"></a>Következő lépések

* Egy bevezető tooEvent rács, lásd: [esemény rács](overview.md).
* tooquickly esemény rács használatának megkezdésében című [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).
