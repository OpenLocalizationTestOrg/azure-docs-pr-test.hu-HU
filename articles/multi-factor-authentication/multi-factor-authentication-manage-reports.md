---
title: "az Azure MFA aaaAccess és használati jelentések |} Microsoft Docs"
description: "Ez ismerteti, hogyan toouse hello Azure multi-factor Authentication szolgáltatás - jelentéseket."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: ae7ccceca4968d7ec7cf0cb1cf9e041d9997c840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure multi-factor Authentication jelentései
Az Azure multi-factor Authentication segítségével, és a szervezet több jelentéseket biztosít. Ezek a jelentések hello multi-factor Authentication kezelési portál is elérhetőek. hello az alábbiakban látható hello elérhető jelentések listája:

| Jelentés | Leírás |
|:--- |:--- |
| Használat |hello használati jelentések megjelenítési adatok összesített használatát, a – felhasználói összefoglalás és a felhasználó adatait. |
| Kiszolgáló állapota |Ez a jelentés a multi-factor Authentication kiszolgáló a fiókjához társított hello állapotát jeleníti meg. |
| Letiltott felhasználók előzményei |Ezek a jelentések kérelmek tooblock hello előzmények megjelenítése, és a felhasználók feloldása. |
| Kihagyott felhasználók előzményei |Kérelmek toobypass többtényezős hitelesítés a felhasználó telefonszámának hello előzményeit jeleníti meg. |
| Csalási riasztás |Során megadott hello dátumtartományban küldött visszaélési riasztások előzményeit jeleníti meg. |
| A várólistára |Listák sorba állított jelentések feldolgozási és azok állapotát. Hivatkozás toodownload vagy nézet hello jelentés készül róla hello jelentés befejezésekor. |

## <a name="view-reports"></a>Jelentések megtekintése
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldalon válassza ki az Active Directory.
3. Kövesse az e két beállítás megadása, attól függően, hogy használ-e hitelesítésszolgáltatók egyikét:
   * **1. lehetőség**: hello többtényezős hitelesítésszolgáltatók lapon. Válassza ki az MFA-szolgáltatóra, majd kattintson a hello **kezelése** hello alsó gombra.
   * **2. lehetőség**: válassza ki a könyvtárhoz, és lépjen toohello **konfigurálása** fülre. Hello a multi-factor authentication szakaszban válassza ki a **szolgáltatás beállításainak kezelése**. Hello a hello multi-factor Authentication szolgáltatás beállításainak lap alján kattintson hello Ugrás toohello portal-hivatkozás.
4. Hello Azure multi-factor Authentication kezelési portál, jelölje ki hello típusú kívánt jelentést hello **megtekint egy jelentést** bal oldali navigációs hello című szakasza.

<center>![Felhő](./media/multi-factor-authentication-manage-reports/report.png)</center>


**További források**

* [A felhasználók számára](end-user/multi-factor-authentication-end-user.md)
* [Az Azure többtényezős hitelesítés az MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn249471.aspx)
