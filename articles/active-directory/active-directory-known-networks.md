---
title: "a klasszikus Azure portálon hello aaaKnown hálózatok |} Microsoft Docs"
description: "Ismert hálózatok konfigurálásával elkerülheti, hogy hello bejelentkezési különböző földrajzi régiókból és bejelentkezési modulokat IP-címekről a gyanús tevékenység jelentések szerepel a szervezete tulajdonában lévő IP-címek."
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
ms.openlocfilehash: ec636cdda172cd3baeb1e606dd8d6e1949fbc63b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="known-networks"></a>Ismert hálózatok

> [!div class="op_single_selector"]
> * [klasszikus Azure portál](active-directory-known-networks.md)
> * [Azure Portal](active-directory-known-networks-azure-portal.md)
> 
> 


Használhatja az Azure Active Directory hozzáférési és használati jelentések toogain láthatósága hello adatintegritási és biztonsági a szervezete címtárát. Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat, hogy azok megfelelően megtervezheti toomitigate kockázatok vizsgálandó.

Lehetséges, hogy a hello "*több földrajzi területről indított bejelentkezések*"és"*IP-címekről a gyanús tevékenységeket bejelentkezések*" jelentések helytelenül Ez a jelző ténylegesen tulajdonában lévő IP-címek a szervezet. 

Ez történik például is, ha: 

* A felhasználó a Boston Office aláírta San Francisco eseményindítók hello "Különböző földrajzi régiókból bejelentkezési ins" jelentés távolról tooyour data Center 
* A szervezet egy felhasználó megpróbál toosign-meg többször a egy helytelen jelszót eseményindítók hello "IP-címekről a gyanús tevékenységeket bejelentkezési ins" jelentés 

tooprevent ezekben az esetekben a előállítása félrevezető biztonsági jelentések, ismert IP-címtartományok toohello címlista a vállalat nyilvános IP-cím hozzá kell adnia.    

### <a name="tooadd-your-organizations-public-ip-address-ranges-perform-hello-following-steps"></a>tooadd a vállalat nyilvános IP-címtartományt, hajtsa végre az alábbi lépésekkel hello:

1. Bejelentkezés toohello [Azure felügyeleti portálján](https://manage.windowsazure.com).

2. Hello bal oldali ablaktáblában kattintson **Active Directory**. 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-01.png)

3. A hello **Directory** lapra, válassza ki azt a címtárat.

4. Hello hello felső menüben kattintson a **konfigurálása**. 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-02.png)

5. Hello konfigurálása lapon lépjen túl**a szervezetek nyilvános IP-címtartományok** 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-03.png)

6. Kattintson a **adja hozzá az ismert IP-címtartományok**.

7. Adja hozzá a címtartomány hello párbeszédpanelt, amely akkor jelenik meg, és kattintson hello gombot, amikor elkészült. 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-04.png)

**További források:**

* [A hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md)
* [IP-címekről a gyanús tevékenységeket bejelentkezések](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Több földrajzi területről indított bejelentkezések](active-directory-reporting-sign-ins-from-multiple-geographies.md)

