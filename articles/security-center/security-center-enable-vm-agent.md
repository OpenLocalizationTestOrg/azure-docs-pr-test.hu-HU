---
title: "aaaEnable Virtuálisgép-ügynök az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése VM ügynök **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9bd71e638b020780537da25fd4cf7baf34d3e11a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a>Az Azure Security Centerben Virtuálisgép-ügynök engedélyezése
hello VM ügynököt telepíteni kell a virtuális gépek (VM) sorrendben túl[az adatgyűjtést](security-center-enable-data-collection.md).  Az Azure Security Center lehetővé teszi, hogy Ön toosee virtuális gépek igénylő hello Virtuálisgép-ügynök, és azt javasolja, hogy engedélyezze a virtuális gépek Virtuálisgép-ügynök hello.

Virtuálisgép-ügynök hello alapértelmezés szerint telepítve van a hello Azure Piactérről származó központilag telepített virtuális gépekhez. hello cikk [ügynök és Virtuálisgép-bővítmények – 2. rész](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) bemutatja, hogyan tooinstall hello Virtuálisgép-ügynök.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be. A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok panel**, jelölje be **Virtuálisgép-ügynök engedélyezése**.
   ![Virtuálisgép-ügynök engedélyezése][1]
2. Ekkor megnyílik hello panel **VM hiányzik vagy nem válaszol**. Ezen a panelen hello hello Virtuálisgép-ügynök igénylő virtuális gépek sorolja fel. Hello utasításokat kövesse hello panel tooinstall hello Virtuálisgép-ügynök.
   ![Hiányzik a Virtuálisgép-ügynök][2]

## <a name="see-also"></a>Lásd még:
További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md)– megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
