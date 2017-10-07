---
title: "tudnivalók a kétlépéses ellenőrzést, az Azure MFA aaaLearn |} Microsoft Docs"
description: "Mi az Azure multi-factor Authentication, MFA, további információt hello multi-factor Authentication ügyfél és hello különböző módszereket és verziók miért érdemes használni. "
keywords: "Mi az az mfa bemutatása tooMFA többtényezős hitelesítés áttekintése"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: kgremban
ms.openlocfilehash: a91b8d6941d2b6ce72a789a97c57e10e594e7ada
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a>Mi az az Azure Multi-Factor Authentication?
Kétlépéses ellenőrzés, hogy egynél több ellenőrzési módszert igényel, és a kritikus fontosságú második réteget biztonsági toouser bejelentkezéseket és tranzakciókat ad hitelesítési mód. Azzal, hogy bármely két vagy több ellenőrzési módszer a következő hello működik:

* Valami tudja (általában jelszót)
* Valami (megbízható eszközzel rendelkezik, amely nem egyszerű ismétlődik, például a telefon)
* Valami áll (biometria)

<center>![Felhasználónév és jelszó](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![tanúsítványok](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Phone intelligens](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![intelligens kártya](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![virtuális intelligens kártya](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![felhasználónév és jelszó](./media/multi-factor-authentication/cert.png)</center>

Az Azure Multi-Factor Authentication (MFA) a Microsoft kétlépéses hitelesítési megoldása. Az Azure MFA segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Számos (például telefonos megerősítést, szöveges üzenetet vagy mobilalkalmazást használó) ellenőrzési módszerének köszönhetően erős hitelesítést biztosít.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a>Miért érdemes használni az Azure multi-factor Authentication?
Ma, több mint legalább egyszer személyek egyre csatlakoznak. Az intelligens telefonok, táblagépek, laptopok és számítógépek személyek közül több különböző hogyan azok tooconnect fog, és bármikor maradhat. Személyek férhetnek a fiókok és az alkalmazások bárhonnan, ami azt jelenti, hogy hatékonyabb munkavégzésben, és az ügyfelek kiszolgálásához jobban.

Az Azure multi-factor Authentication könnyen toouse, méretezhető, és megbízható megoldás, amely biztosít egy második hitelesítési módszer, így a felhasználók mindig védettek.

| ![Egyszerű tooUse](./media/multi-factor-authentication/simple.png) | ![Méretezhető](./media/multi-factor-authentication/scalable.png) | ![Mindig védve](./media/multi-factor-authentication/protected.png) | ![Megbízható](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Egyszerű toouse** |**Méretezhető** |**Mindig védve** |**Megbízható** |

* **Egyszerű tooUse** -Azure multi-factor Authentication egy egyszerű tooset be és használja. hello Azure multi-factor Authentication a további védelem lehetővé teszi a felhasználók toomanage a saját eszközeiket. Ajánlott az összes sok esetben azt is beállítható néhány egyszerű kattintással.
* **Méretezhető** -Azure multi-factor Authentication hello felhő hello fogyaszt, és integrálható a helyszíni AD és az egyéni alkalmazások. Ez a védelem még akkor is ki van bővítve tooyour nagy mennyiségű, a kritikus fontosságú forgatókönyvek.
* **Mindig védett** -Azure multi-factor Authentication hello legmagasabb ipari szabványok használatával erős hitelesítést nyújt.
* **Megbízható** -garantáljuk Azure multi-factor Authentication 99,9 %-os rendelkezésre állását. hello szolgáltatás tekinthető nem érhető el, ha nem tudja a kétlépéses ellenőrzéshez hello tooreceive vagy folyamat ellenőrzési kérelmekről.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Következő lépések

- További tudnivalók [Azure multi-factor Authentication működése](multi-factor-authentication-how-it-works.md)

- További információ a különböző hello [a Azure multi-factor Authentication verziók és használat](multi-factor-authentication-versions-plans.md)
