---
title: "Hálózatok ismert a klasszikus Azure portálon |} Microsoft Docs"
description: "Ismert hálózatok konfigurálásával elkerülheti, ha szerepel a bejelentkezési különböző földrajzi régiókból és bejelentkezési modulokat IP-címekről a gyanús tevékenység jelentések a szervezet tulajdonában lévő IP-címek."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: e4d51d1d2f09fca34d749879e21d49f785eac35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="known-networks"></a>Ismert hálózatok

> [!div class="op_single_selector"]
> * [klasszikus Azure portál](active-directory-known-networks.md)
> * [Azure Portal](active-directory-known-networks-azure-portal.md)
> 
> 


Azure Active Directory hozzáférési és használati jelentések segítségével hogy lássák az integritásra és a munkahely címtárában biztonságát. Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat a vizsgálandó, hogy azok megfelelően megtervezheti kockázatok csökkentésének lehetőségeit.

Lehetséges, hogy a "*több földrajzi területről indított bejelentkezések*"és"*IP-címekről a gyanús tevékenységeket bejelentkezések*" jelentések helytelenül jelzőt a szervezet ténylegesen tulajdonában lévő IP-címeket. 

Ez történik például is, ha: 

* Az office bejelentkezett az távolról az adatközpont, a San Francisco Boston egy felhasználó elindítja a "Bejelentkezések különböző földrajzi régiókból" jelentés 
* A felhasználó a szervezet próbál jelentkezhessen be többször egy helytelen jelszót eseményindítók az "IP-címekről a gyanús tevékenységeket bejelentkezések" jelentés 

Ezekben az esetekben félrevezető biztonsági jelentések létrehozása érdekében ismert IP-címtartományok kell hozzáadása a listához, a vállalat nyilvános IP-cím.    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Adja hozzá a szervezet nyilvános IP-címtartományok, hajtsa végre az alábbi lépéseket:

1. Bejelentkezés a [Azure felügyeleti portálján](https://manage.windowsazure.com).

2. Kattintson a bal oldali ablaktáblában **Active Directory**. 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-01.png)

3. Az a **Directory** lapra, válassza ki azt a címtárat.

4. Kattintson a felső menüben **konfigurálása**. 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-02.png)

5. Lépjen a konfigurálása lapon **a szervezetek nyilvános IP-címtartományok** 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-03.png)

6. Kattintson a **adja hozzá az ismert IP-címtartományok**.

7. Adja hozzá a címtartomány a megjelenő párbeszédpanelen, és amikor elkészült, kattintson a pipa gombra. 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-04.png)

**További források:**

* [A hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md)
* [IP-címekről a gyanús tevékenységeket bejelentkezések](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Több földrajzi területről indított bejelentkezések](active-directory-reporting-sign-ins-from-multiple-geographies.md)

