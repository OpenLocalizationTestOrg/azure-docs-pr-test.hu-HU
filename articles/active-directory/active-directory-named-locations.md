---
title: az Azure Active Directoryban aaaNamed helyek |} Microsoft Docs
description: "Úgy konfigurálja a helyek nevű, elkerülheti a rendelkező IP hello lehetetlen odautazás tooatypical helyek esemény kockázattípus létrehozni a szervezete tulajdonában lévő címek vakriasztások tömkelegére."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 591e4b94b2ec9d45e20c01711e922f9972e047e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Az Azure Active Directoryban elnevezett helyek

A hello nevű helyek szolgáltatás az Azure Active Directory a címkézés megbízható IP-címtartományok a szervezetben. A környezetben, használhatja a hello észlelése hello környezetben elnevezett helyét [kockázati események](active-directory-reporting-risk-events.md). hello funkció csökkenti a hello jelentett téves hello számát *lehetetlen odautazás tooatypical helyek* eseménytípus kockázatát. 

## <a name="configuration"></a>Konfiguráció

tooconfigure elnevezett helye:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.

2. Hello bal oldali ablaktáblában kattintson **Azure Active Directory**.

    ![hello bal oldali ablaktáblán hello Azure Active Directory-hivatkozás](./media/active-directory-named-locations/01.png)

3. A hello **Azure Active Directory** paneljén, hello **biztonsági** kattintson **feltételes hozzáférés**.

    ![Feltételes hozzáférés parancs hello](./media/active-directory-named-locations/05.png)


4. A hello **feltételes hozzáférés** paneljén, hello **kezelése** területén kattintson **helyek nevű**.

    ![hello nevesített helyek parancs](./media/active-directory-named-locations/06.png)


5. A hello **helyek nevű** panelen kattintson a **új helyre**.

    ![Új hely parancs hello](./media/active-directory-named-locations/07.png)


6. A hello **új** panelen a következő hello:

    ![hello új panel](./media/active-directory-named-locations/08.png)

    a. A hello **neve** mezőbe írja be a nevesített hely nevét.

    b. A hello **IP-címtartományok** mezőbe írjon be egy IP-címtartományt. hello IP-címtartomány van szüksége a hello toobe *Classless Inter-Domain Routing* (CIDR) formátumban.  

    c. Kattintson a **Create** (Létrehozás) gombra.



## <a name="what-you-should-know"></a>Tudnivalók

**Tömeges frissítés**: elnevezett helyek tömeges frissíti, frissítése vagy létrehozásakor feltöltheti, vagy az IP-címtartományok hello CSV-fájl letöltése. Feltöltés hello IP-címtartományok helyez toohello fájllista hello helyett hello lista felülírja.

![hello hivatkozások le- és feltöltése](./media/active-directory-named-locations/09.png)


**Korlátozások**: 60 elnevezett helyek, legfeljebb egy IP tartomány hozzárendelt tooeach azok adhat meg. Ha csak egy elnevezett helyen konfigurálva van, adhat meg too500 IP-címtartományok fel azt.


## <a name="next-steps"></a>Következő lépések

További információ a kockázati eseményekről, toolearn lásd [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md).

