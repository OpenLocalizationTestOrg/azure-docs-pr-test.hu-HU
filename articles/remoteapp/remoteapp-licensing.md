---
title: "aaaAzure RemoteApp-Licenckezelés |} Microsoft Docs"
description: "Ismerje meg az Azure RemoteApp licenckezelését."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: ff8ebd20-61a1-4f10-87a6-234a170534c9
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: dfa808a65ea6b1a78bf74f3daddb9a84e186eebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Hogyan működik a licenckezelés az Azure RemoteAppban?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Úgy állított be az Azure remoteappot, létrehozta a sablonokat, és készen áll a toopublish alkalmazások tooyour felhasználók. Azonban továbbra is fennáll, egy utolsó lépésként toofigure: licencelési. Csak hogyan működik a RemoteApp és Remoteappen keresztül megosztott hello alkalmazások licencelési munkahelyi?

A RemoteApp használatához nincs szükség semmilyen Windows-licencre vagy távoli asztali ügyféllicencre. Az előfizetés gondoskodik a RemoteApp oldaláról hello. (Kivétel hello hello részleteit [díjszabások](https://azure.microsoft.com/pricing/details/remoteapp).)

Az előfizetéshez mellékelt rendszerképet hello használatakor megoszthatja, különálló licenc nélkül rendszerképen telepített hello alkalmazások bármelyikét. Például ha hello Windows Server 2012 R2 sablon kép toobuild használja a gyűjtemény, megoszthatja a System Center Endpoint Protection a felhasználók. hello csak a kivételek toothis szabály olyan Office 365 ProPlus, amelyhez különálló előfizetés szükséges, és Office 2013, amely nem osztható meg termelési gyűjtemény.

Ha azt szeretné, hogy a Remoteapphoz mellékelt toouse hello Office 365 sablon rendszerképet, rendelkeznie kell egy *meglévő* Office 365 ProPlus terv. hello bármely Office 365-alkalmazás tesz közzé egy egyéni sablon használatával esetében is igaz. A saját előfizetésével kell tooactivate hello alkalmazásokat. Ez a próba- és a fizetett előfizetésekre is vonatkozik. Ha azt szeretné toouse hello Office 365 sablon rendszerképet hello próbaidőszakban *és még nem rendelkezik előfizetéssel*, toohello Office 365-oldal túl lépjen[regisztráljon](https://go.microsoft.com/fwlink/p/?LinkID=403802) egy próba-előfizetésre vonatkozó. További információkért lásd a [Hogyan működik együtt a RemoteApp és az Office?](remoteapp-o365.md) (Hogyan működik együtt a RemoteApp és az Office) témakört.

Ha hello próbaverziós időszak alatt nem kívánja tooget az Office 365 próba-előfizetést, használja a Remoteapphoz mellékelt hello Office 2013 Professional Plus sablon rendszerképet. Ez a sablon csak 30 napig használható, és alakítható át fizetett gyűjteménnyé.

Más alkalmazások esetén van szüksége, hogy rendelkezik-e hello licenc tooshare hello app toomake.

Ez így érthető, ugye? Közzéteheti a kívánt alkalmazást, hogy tooshare jogilag jogosultak. És meg kell toomake meg arról, hogy valóban jogosult tooshare a programokat.

Vegye figyelembe, hogy felhőalapú gyűjtemény esetében nem használhat ügyféllicencet vagy mennyiségi licencelési megállapodást. Ön *is* a mennyiségi licencelési megállapodást tooactivate alkalmazások használata a hibrid gyűjtemények (kivéve az Office esetében). Csupán be kell őket a sablon hello mennyiségi licencelési adathordozóról kép tooinstall. Kövesse hello információk hello alkalmazás szállító tooinstall licencek távoli asztali környezetben.

