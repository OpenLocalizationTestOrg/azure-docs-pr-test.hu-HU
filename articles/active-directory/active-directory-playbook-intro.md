---
title: "Active Directory koncepció forgatókönyv bevezető aaaAzure |} Microsoft Docs"
description: "Vizsgálatát, és gyorsan végrehajthatja az identitás- és kezelési helyzetek"
services: active-directory
keywords: "az Azure active directory, a forgatókönyv, a koncepció igazolása, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 655524e9692de46e831fc68e1636e6c20d41958f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a>Az Azure Active Directory igazolása koncepció forgatókönyv: bemutatása

Ebben a cikkben talál útmutatást a koncepció igazolása (PoC) a tooexplore másik Azure AD lehetővé tevő szolgáltatásaival. hello célja, hogy a dokumentum az identitás mérnökök, informatikai szakemberek és rendszerintegrátorok

## <a name="how-toouse-this-playbook"></a>Hogyan toouse Ez a forgatókönyv

1. Használjon hello [téma](active-directory-playbook-ingredients.md#theme) szakaszt, és válassza ki az igényeinek megfelelően érdeklő hello megfelelőnek.  
2. Hatókör hello koncepció megfelel-e az üzleti céljaihoz hello forgatókönyvek kiválasztásával. hello rövidebb hello jobb. Azt javasoljuk, mint a rövid és tömör lehetséges tooconvey hello értékként toohello érdekelt felek, ugyanakkor minimalizálja a hello összetettsége toorealize azt.  
3. Használjon hello [megvalósítási](active-directory-playbook-implementation.md) szakasz toounderstand hello forgatókönyvek, és szeretné értelmezéséről alkalmaz környezetében. Minden esetben azt írják le hogyan tooset azt fel (úgynevezett [építőelemeket](active-directory-playbook-building-blocks.md)), és hogyan toonavigate hello forgatókönyvek. 
4. Minden egyes építőelem a szükséges előfeltételek hello szükséges, valamint az megközelítőleges időpont, amikor toocomplete ismerteti. Ez segítséget nyújthat hello tervezési folyamat során. 
5. Az 1-3 alapján, adja meg a hello [környezet](active-directory-playbook-ingredients.md#environment) mely tooexecute a. A termelési környezetben tooget hello felhasználói élmény, a helyes működését a toostrive javasoljuk. 
6. Ha ütköző követelményeket támasztó, használja a hasznos kompromisszumot mátrix 
   * Az érték téma-központú megjelenítése  
   * Simaság tooprepare tooset fel és tooexecute hello forgatókönyvek 
   * Minimális idő tooexecute hello cél forgatókönyvek 
   * Bezárás tooproduction a megkötések megvalósítható, mint 

>[!NOTE]
> Ez a cikk teljes jelenik meg néhány adott harmadik féltől származó alkalmazások és a termékek kényelmi célokat szolgál példaként említett. Az Azure AD-alkalmazások ezer támogatja a [alkalmazáskatalógusában](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) használható az Ön igényeinek és a környezet alapján. 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]