---
title: "Azure Automation hibrid forgatókönyv-feldolgozó aaaTroubleshoot |} Microsoft Docs"
description: "Hello jelenség, okok és a leggyakrabban használt hibrid forgatókönyv-feldolgozó állít ki az Azure Automationben hello megoldás leírása"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: magoedte
ms.openlocfilehash: 292af517c88d1ffd21262a286ccc079e7270bfad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-tips-for-hybrid-runbook-worker"></a>Hibaelhárítási tippek a hibrid forgatókönyv-feldolgozó

Ez a cikk ismerteti az Automation hibrid forgatókönyv-feldolgozók tapasztalhat, és lehetséges megoldásokat tooresolve javasol hibák elhárításához őket.

## <a name="a-runbook-job-terminates-with-a-status-of-suspended"></a>Egy runbook-feladat leállítása felfüggesztett állapotú

A runbook fel van függesztve, hamarosan tooexecute kísérlet után háromszor azt. Feltételek vonatkoznak, amelyek megszakíthatja hello runbook sikeres befejezését, és a kapcsolódó hello hibaüzenet tartalmazza, amely jelzi, hogy miért további adatokat. Ez a cikk hibaelhárítási lépéseket biztosít a problémák kapcsolódó toohello hibrid forgatókönyv-feldolgozó runbook végrehajtása sikertelen.

Ha az Azure nem problémával az ebben a cikkben, látogasson el az Azure-fórumok hello [MSDN és hello a Stack Overflow](https://azure.microsoft.com/support/forums/). A probléma a fórumokban beküldheti vagy túl[ @AzureSupport a Twitteren](https://twitter.com/AzureSupport). Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a hello [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.

### <a name="symptom"></a>Jelenség
A Runbook végrehajtása sikertelen, és hello hibaüzenet, "hello Feladatművelet"Aktiválás"Feladatművelet nem futtatható, mivel hello folyamat váratlanul leállt. hello feladat művelet végrehajtására történt kísérlet 3-szor."

Nincsenek hello hiba lehetséges okai: 

1. a proxy vagy az tűzfal mögött van hello hibridfeldolgozó
2. hello számítógép hello hibridfeldolgozó futó rendelkezik kisebb, mint a minimális hello [hardverkövetelmények](automation-offering-get-started.md#hybrid-runbook-worker)  
3. runbookokat hello nem tudja hitelesíteni a helyi erőforrások

#### <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>1. ok: Az hibrid forgatókönyv-feldolgozó proxy és tűzfal mögött van.
hello számítógép hello hibrid forgatókönyv-feldolgozó fut egy tűzfal vagy a proxy mögött van, és a kimenő hálózati hozzáférés nem engedélyezett, lehet, illetve megfelelően konfigurálva.

#### <a name="solution"></a>Megoldás
Győződjön meg arról, hello számítógép 443-as port kimenő hozzáférést too*.azure-automation.net rendelkezik. 

#### <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>2. ok: A számítógépben kisebb, mint a minimális hardverkövetelmények
Meg kell felelniük a hibrid forgatókönyv-feldolgozó hello futtató számítógépek hardverre vonatkozó minimális elvárásokat hello toohost kijelölő, mielőtt ezt a szolgáltatást. Ellenkező esetben az hello erőforrás-használat más háttérfolyamatot és versengés okozta a runbookok végrehajtása közben, attól függően, hogy hello számítógép túlterhelt fog válik, és runbook feladat működés lelassulhat, vagy időtúllépés miatt. 

#### <a name="solution"></a>Megoldás
Először ellenőrizze a kijelölt toorun hello hibrid forgatókönyv-feldolgozó szolgáltatás hello számítógép megfelel-e hello hardverre vonatkozó minimális elvárásokat.  Ha igen, figyelheti CPU és memória kihasználtsága toodetermine bármely korrelációs hibrid forgatókönyv-feldolgozó folyamat hello teljesítményét és a Windows között.  Ha memória vagy a CPU-terhelés, ez lehetséges, hogy hello kell tooupgrade jelzik, vagy további processzorok hozzáadásával, vagy növelje memória tooaddress erőforrás szűk hello és hello hiba megoldásához. Azt is megteheti válassza ki a különböző számítási erőforrása, amely támogatja a hello minimális követelményeknek, és ha terheléshez jelezheti növelését szükséges.         

#### <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>3. ok: A Runbookok nem tudják hitelesíteni magukat a helyi erőforrások

#### <a name="solution"></a>Megoldás
Ellenőrizze a hello **Microsoft-SMA** egy kapcsolódó esemény leírása az eseménynaplóban *Win32 a folyamat kilépett a következő kód [4294967295]*.  hello Ez a hiba oka még nem konfigurált hitelesítési a runbookok vagy hello futtató hitelesítő adatokat adott hello hibrid feldolgozócsoport számára.  Tekintse át [Runbookokra vonatkozó engedélyek](automation-hrw-run-runbooks.md#runbook-permissions) a runbookok megfelelően konfigurált hitelesítési tooconfirm.  

## <a name="next-steps"></a>Következő lépések

Az automatizálás egyéb problémák elhárításához talál segítséget [közös Azure Automation-problémák elhárítása](automation-troubleshooting-automation-errors.md) 