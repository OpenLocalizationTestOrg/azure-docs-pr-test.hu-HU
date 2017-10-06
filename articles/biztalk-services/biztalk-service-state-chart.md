---
title: "engedélyezett állapotot váltani vagy a BizTalk szolgáltatások állapotok aaaTasks |} Microsoft Docs"
description: "a különböző MABS állapotú engedélyezett műveletek/műveletek hello: állítsa le, indítsa el, indítsa újra, felfüggesztése, folytatása, törlése, méretezhető, frissítheti a konfigurációs és biztonsági mentése"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: 643307ba6fa9b05c82b867912feab249c42b65dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-you-can-and-cant-do-using-hello-biztalk-service-state"></a>Lehet, illetve mit nem használatával hello BizTalk szolgáltatás állapota

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Attól függően, hogy hello hello BizTalk szolgáltatás aktuális állapotát nincsenek vagy hello BizTalk szolgáltatás nem hajtható végre műveleteket.

Például a klasszikus Azure portálon hello új BizTalk szolgáltatás kiépítése. Sikeresen befejeződött, hello BizTalk szolgáltatás esetén a `active` állapotát. Hello aktív állapotban állítsa le, felfüggesztése és hello BizTalk szolgáltatás törléséhez. Ha le hello BizTalk szolgáltatás leállítása sikertelen, majd hello BizTalk szolgáltatás kerül tooa `StopFailed` állapotát. A hello `StopFailed` állapotba kerül, hello BizTalk szolgáltatásokat. Hello a következő hiba akkor fordul elő, ha egy művelet nem engedélyezett, folytatása, például:

`Operation not allowed`

## <a name="view-hello-possible-states"></a>Hello lehetséges állapota nézet

hello alábbi táblázatok a lista hello műveletek vagy meghatározott állapotban lévő hello BizTalk szolgáltatás esetén végezhető műveleteket. Egy ✔ azt jelenti, hogy hello művelet engedélyezett az adott állapotban. Egy üres bejegyzést azt jelenti, hogy hello művelet nem hajtható végre az adott állapotban.

| Szolgáltatás állapota | Indítás | Leállítás | Újraindítás | Felfüggesztése | Folytatás | Törlés | Méretezés | Frissítés <br/> Konfiguráció | Biztonsági mentés |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Aktív |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Letiltva |  |  |  |  |  | ✔ | |  |  | 
| Felfüggesztve |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Leállítva | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Szolgáltatás frissítése sikertelen |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Lásd még:
* [A klasszikus Azure portálon hello BizTalk szolgáltatás létrehozása](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [Mi mindent hello irányítópult, a figyelő és a skála lapon BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk szolgáltatások hello fejlesztői, Basic, Standard és prémium kiadás beolvasása](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Hogyan tooback össze, és a BizTalk szolgáltatás visszaállítása](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Sávszélesség-szabályozás, tekintse meg a BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [A BizTalk szolgáltatás hello Service Bus- és hozzáférés-vezérlés kibocsátó neve és a kiállító kulcsértékei beolvasása](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)

