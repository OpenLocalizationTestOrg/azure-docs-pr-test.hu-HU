---
title: "Hibaelhárítás: Hiányzó adatok hello Azure Active Directory műveletnaplóban |} Microsoft Docs"
description: "Listák hello Azure Active Directory elérhető különböző jelentéseket."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 7bbec94ab42eb5b54a7e65e124060d057b4a1a34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-some-actions-that-i-performed-in-hello-azure-active-directory-activity-log"></a>Nem található néhány hello Azure Active Directory tevékenységnapló végrehajtott műveletek


## <a name="symptoms"></a>Probléma

I végrehajtott bizonyos műveleteket a hello Azure-portálon, és a várt toosee hello naplókat a csatolási műveleteket hello `Activity logs > Audit Logs` panelen, de azokat nem található.

 ![Jelentéskészítés](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Ok

Műveletek nem jelennek meg azonnal hello tevékenység naplót. Azt is igénybe vehet tooan óra toosee hello auditnaplókat hello portálon hello művelet hello idő 15 perc.

## <a name="resolution"></a>Megoldás:

Várjon 15 percig tooan órán keresztül, és ellenőrizze, hogy megjelennek-e hello műveletek hello naplóban. Ha még mindig nem látja őket, hozzon létre egy támogatási jegyet, és megvizsgáljuk az ügyet.


## <a name="next-steps"></a>Következő lépések
Lásd: hello [Azure Active Directory – gyakori kérdések reporting](active-directory-reporting-faq.md).

