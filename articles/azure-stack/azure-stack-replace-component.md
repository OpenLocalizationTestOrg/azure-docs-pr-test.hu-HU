---
title: "Cserélje le az Azure-verem skálázási egység csomóponton hardverösszetevőt |} Microsoft Docs"
description: "Cserélje le az integrált Azure verem rendszeren hardverösszetevőt útmutató."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: c6e036bf-8c80-48b5-b2d2-aa7390c1b7c9
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: 4937b7725c8f39314ccc41584a8646b7197f6bdf
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/10/2018
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Cserélje le az Azure-verem skálázási egység csomóponton egy hardverösszetevő

*A következőkre vonatkozik: Azure verem integrált rendszerek*

Ez a cikk ismerteti, amelyek nem közbeni-cserélhető hardverösszetevők lecseréli általános folyamata. Tényleges helyettesítő szükséges lépések eltérhetnek az eredeti hardvergyártó (OEM) hardver szállítójával alapján. Részletes lépéseket, amelyek adott integrált Azure verem rendszerhez a gyártója által biztosított mező cserélhető Cisco egységet (FRU) dokumentációjában talál.

Nem közbeni-cserélhető összetevők közé tartoznak a következők:

- CPU *
- Memória *
- Alaplap/alaplapi felügyeleti vezérlővel (BMC) / videó kártya
- Lemez vezérlő/gazdabuszadaptert (HBA) / csatlakozópanel
- Hálózati kártya (NIC)
- Operációs rendszer lemez *
- Adatmeghajtók (meghajtókat, amelyek nem támogatják a működés közbeni swap, például PCI-e beépülő modul kártyák) *

* Ezek az összetevők támogathatja a gyakran használt adatok swap, de a szállító függően változhat. Részletes utasítások a OEM gyártója által biztosított FRU dokumentációjában talál.

A következő folyamatábra nem közbeni-cserélhető hardverösszetevő cseréje általános FRU folyamata látható.

![Összetevő helyettesítő-folyamatot bemutató folyamatábra](media/azure-stack-replace-component/replacecomponentflow.PNG)

* Ez a művelet nem lehet szükség a fizikai hardver-feltételen alapszik.

** E OEM hardvergyártójához hajt végre összetevő váltja fel, és a belső vezérlőprogram frissítése sikerült függően változnak, a szerződés támogatott.

## <a name="review-alert-information"></a>Riasztási információk áttekintése

Az Azure-verem és a felügyeleti rendszer követi nyomon a hálózati adapterek és a közvetlen tárolóhelyek által vezérelt adatmeghajtókon állapotát. Egyéb hardverelemek nem követi nyomon. Az összes egyéb hardverelemek riasztásokról értesítő a szállító-specifikus hardver felügyeleti megoldás, amely a hardver életciklus gazdagépen futtatja.

## <a name="component-replacement-process"></a>Az összetevő cseréjét.

Az alábbi lépéseket a összetevő cseréjét magas szintű áttekintését adja meg. Az OEM által biztosított FRU dokumentációját utaló nélkül nem kövesse az alábbi lépéseket.

1. Használja a [kiürítésére](azure-stack-node-actions.md#scale-unit-node-actions) műveletet a skálázási egység csomópont állítható karbantartási üzemmódba. Ez a művelet nem lehet szükség a fizikai hardver-feltételen alapszik.

   > [!NOTE]
   > Minden esetben merül le és ki van kapcsolva egy időben a S2D megszüntetése nélkül a csak egy csomópont (közvetlen tárolóhelyek).

2. Miután a skálázási egység csomópontot karbantartási módban van, a [kikapcsolásához](azure-stack-node-actions.md#scale-unit-node-actions) művelet. Ez a művelet nem lehet szükség a fizikai hardver-feltételen alapszik.
 
   > [!NOTE]
   > Az valószínű esetében, amelyek a kikapcsolási művelet nem működik használja helyette az alaplapi felügyeleti vezérlővel (BMC) webes felülete.

3. Cserélje le a sérült hardverösszetevő. Hogy OEM hardvergyártójához végez összetevő váltja fel a támogatási szerződése alapján változhatnak.  
4. A belső vezérlőprogram frissítése. Kövesse a hardver életciklus-állomás segítségével ellenőrizze, hogy a kicserélt hardverösszetevő rendelkezik a jóváhagyott belső vezérlőprogram szint alkalmazza a szállító-specifikus belső vezérlőprogram folyamatot. Hogy OEM hardvergyártójához végez-e ezt a lépést a támogatási szerződése alapján változhatnak.  
5. Használja a [javítási](azure-stack-node-actions.md#scale-unit-node-actions) annak érdekében, a méretezési egység csomópont ismét be a skálázási egység művelet.
6. Használja a rendszerjogosultságú végpontot [tekintse meg a virtuális lemezek javítási](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Új adatok meghajtók a teljes helyreállítási feladat rendszerterhelés függően több órát is igénybe vehet, és a felhasznált lemezterület.
7. A javítási művelet befejeződését követően ellenőrizze, hogy az összes aktív riasztás automatikusan bezárták.

## <a name="next-steps"></a>További lépések

- További információ a közbeni-cserélhető fizikai lemez cseréje: [olyan lemezt cserél ki](azure-stack-replace-disk.md).
- További információ a fizikai csomópont cseréje: [cserélje le a skálázási egység csomópont](azure-stack-replace-node.md).
- 
