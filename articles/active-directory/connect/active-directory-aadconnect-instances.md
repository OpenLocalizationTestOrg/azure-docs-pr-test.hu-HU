---
title: "Az Azure AD Connect: Szolgáltatáspéldány szinkronizálása |} Microsoft Docs"
description: "Ezen a lapon dokumentumokat az Azure AD-példányban szempontjai."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2a0d8a599cf84cd6530bdbb24951156510d2cf3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Az Azure AD Connect: Különleges szempontok példányok
Az Azure AD Connect leggyakrabban használt hello világszerte példányát az Azure AD és az Office 365. Emellett vannak más esetekben és ezeket az URL-címek és más szempontot különböző követelményekkel rendelkezik.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
Hello [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) egy német adatok adatkezelő által működtetett szuverén felhőalapú.

| A proxykiszolgáló URL-címek tooopen |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Tanúsítvány-visszavonási listákat |

Azure AD-bérlő tooyour bejelentkezéskor hello onmicrosoft.de tartományban olyan fiókot kell használnia.

A szolgáltatások jelenleg nem található meg a Microsoft Cloud Németország hello:

* **Az Azure AD Connect Health** nem érhető el.
* **Az automatikus frissítések** nem érhető el.
* **A jelszóvisszaírás** érhető el az előzetes 1.1.570.0 az Azure AD Connect verziójával és után.
* Más Azure AD Premium-szolgáltatások nem érhetők el.

## <a name="microsoft-azure-government-cloud"></a>A Microsoft Azure Government felhő
Hello [a Microsoft Azure Government felhő](https://azure.microsoft.com/features/gov/) az Amerikai Egyesült Államok kormánya felhőalapú.

A felhő által a DirSync korábbi verziókban támogatott. A build 1.1.180 az Azure AD Connect hello hello felhő következő generációja esetén támogatott. Ebben a generációban csak USA alapján végpontok használja, és rendelkezik a proxykiszolgáló URL-címek tooopen különböző listáját.

| A proxykiszolgáló URL-címek tooopen |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Tanúsítvány-visszavonási listákat |

Az Azure AD Connect nem tud tooautomatically észleli, hogy az Azure AD-bérlő hello kormányzati felhőben található. Ehelyett az Azure AD Connect telepítése során a következő műveletek tootake hello van szüksége.

1. Hello Azure AD Connect telepítés elindításához.
2. Hello első oldal, ahol tooaccept hello EULA elvileg akkor jelenik meg, amikor nem folytatható, de hagyja hello telepítővarázslójának futtatása.
3. Indítsa el a regedit és hello beállításkulcs módosítása `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello érték `2`.
4. Lépjen vissza toohello az Azure AD Connect telepítővarázsló, fogadja el a végfelhasználói licencszerződés hello és továbbra is. A telepítés során győződjön meg arról, hogy toouse hello **egyéni konfigurációs** elérési útja (és nem Expressz telepítés). Ezt a szokásos módon hello telepítését.

A szolgáltatások jelenleg nem található meg a Microsoft Azure Government felhő hello:

* **Az Azure AD Connect Health** nem érhető el.
* **Az automatikus frissítések** nem érhető el.
* **A jelszóvisszaírás** érhető el az előzetes 1.1.570.0 az Azure AD Connect verziójával és után.
* Más Azure AD Premium-szolgáltatások nem érhetők el.

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
