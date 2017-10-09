---
title: "fájlok és mappák, az Azure Backup lassú másolata aaaTroubleshoot |} Microsoft Docs"
description: "Nyújt hibaelhárítási útmutatót toohelp akkor diagnosztizálhatja hello Azure biztonsági mentés teljesítményproblémák okait"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: e379180a-db13-4e0c-90e4-28e5dd6f5b14
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 21d79bbd03c2706bc43fcc7c14020cffd6b919c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Az Azure Backup-fájlok és -mappák lassú biztonsági mentésének hibaelhárítása
Ez a cikk nyújt hibaelhárítási útmutatót toohelp akkor diagnosztizálhatja a fájlok és mappák biztonsági mentési teljesítménycsökkenést hello okát használatakor Azure Backup szolgáltatás. Hello Azure Backup agent tooback fájlok használata esetén hello biztonsági mentési folyamat hosszabb ideig tarthat. Ez a késés oka lehet egy vagy több hello következő:

* [Nincsenek szűk keresztmetszetek hello számítógépen, amelyek biztonsági mentése van folyamatban.](#cause1)
* [Egy másik folyamat vagy a víruskereső szoftver zavarja hello Azure biztonsági mentési folyamat.](#cause2)
* [hello Backup-ügynök fut egy Azure virtuális gép (VM).](#cause3)  
* [Biztonsági mentést készít a fájlok sok (több millió).](#cause4)

Mielőtt elkezdené a problémák elhárításához, azt javasoljuk, hogy letölti és telepíti a hello [legújabb Azure Backup szolgáltatás ügynökének](http://aka.ms/azurebackup_agent). Azt a gyakori frissítések toohello biztonsági mentési ügynök toofix teheti különböző problémákat, szolgáltatások hozzáadása, és a teljesítmény javítása.

Emellett ajánlott áttekinteni hello [Azure Backup szolgáltatás – gyakori kérdések](backup-azure-backup-faq.md) toomake meg arról, hogy nem minden hello közös konfigurációs problémák tapasztalhatók.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-hello-computer"></a>OK: Szűk keresztmetszetek hello számítógépen
A szűk keresztmetszetek hello számítógép, amelyek biztonsági mentése van folyamatban késést okozhat. Például a számítógép képes tooread vagy írási toodisk hello, vagy a rendelkezésre álló sávszélesség toosend adatok hello hálózaton okozhat a szűk keresztmetszeteket.

A Windows beépített eszközt nevezett biztosít [Teljesítményfigyelő](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) toodetect a szűk keresztmetszeteket.

Íme, néhány teljesítményszámlálók és a tartományt a szűk keresztmetszetek optimális biztonsági másolatok diagnosztizálásához hasznos lehet.

| A számláló | status |
| --- | --- |
| Logikai lemez (fizikai lemez) – üresjárati % |• 100 %-os üresjárati too50 % üresjárati = kifogástalan</br>• 49 % üresjárati too20 % üresjárati = figyelmeztetés vagy a figyelő</br>• 19 % üresjárati too0 % üresjárati kritikus vagy kívüli Spec = |
| Logikai lemez (fizikai lemez) – % Átl. Lemez mp Olvasás vagy írás |• 0,001 ms too0.015 ms = kifogástalan</br>• 0,015 ms too0.025 ms = figyelmeztetés vagy a figyelő</br>• 0.026 ms vagy kritikus vagy kívüli Spec már = |
| Logikai lemez (fizikai lemez) – Lemezvárólista jelenlegi hossza (az összes példányra) |80 kérelem 6 percnél hosszabb ideig |
| Memória – nem lapozható memória |• Felhasznált készlet 60 %-nál kisebb = kifogástalan<br>• 61 % too80 % felhasznált készlet = figyelmeztetés vagy a figyelő</br>• Nagyobb, mint a 80 %-készlet felhasznált kritikus vagy kívüli Spec = |
| Memória – lapozható készlet mérete bájtban |• Felhasznált készlet 60 %-nál kisebb = kifogástalan</br>• 61 % too80 % felhasznált készlet = figyelmeztetés vagy a figyelő</br>• Nagyobb, mint a 80 %-készlet felhasznált kritikus vagy kívüli Spec = |
| Memória – rendelkezésre álló hely MB-ban |• 50 %-a rendelkezésre álló szabad memória vagy további = kifogástalan</br>• 25 %-a rendelkezésre álló szabad memória = figyelője</br>• 10 %-a rendelkezésre álló szabad memória = figyelmeztetés</br>• Kevesebb mint 100 MB vagy 5 %-a rendelkezésre álló szabad memória kritikus vagy kívüli Spec = |
| Processzor –\%processzor kihasználtsága () |• 60 %-nál kisebb felhasznált = kifogástalan</br>• 61 % too90 % felhasznált = figyelő vagy figyelmeztetés</br>• 91 % too100 % felhasznált kritikus = |

> [!NOTE]
> Ha azt állapítja meg, hogy hello infrastruktúra hello hibát okozó, azt javasoljuk, hogy töredezettségmentesítése hello lemezek, rendszeresen a jobb teljesítmény érdekében.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>OK: Egy másik folyamat vagy az Azure Backup zavarja, a víruskereső szoftver
Ha más folyamatok, a rendszer a Windows hello negatívan befolyásolt hello Azure Backup szolgáltatás ügynökének folyamatánál teljesítményének több példány is láttuk. Például hello Azure Backup szolgáltatás ügynöke és az adatokat egy másik program tooback használható, vagy ha víruskereső fut, és a fájlok toobe zárolását biztonsági másolatot készített, hello fájlokon több zárral okozhat a versengés. Ebben a helyzetben hello biztonsági mentés sikertelen lehet, vagy hello feladat a vártnál hosszabb ideig is tarthat.

Ebben a forgatókönyvben a legjobb javaslat hello van a más biztonsági mentési program toosee tooturn hello ki e hello biztonsági mentésének ideje hello Azure Backup szolgáltatás ügynöke változik. Általában, annak biztosítása, hogy több biztonsági mentési feladatok nem futnak, hello egyszerre nem elegendő tooprevent befolyásolják egymástól őket.

A víruskereső programok, azt javasoljuk, hogy zárja ki a következő hello fájlok és helyek:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe folyamatként
* C:\Program Files\Microsoft Azure Recovery Services Agent\ mappák
* Ideiglenes helyre (Ha nem használja a szokásos helyre hello)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>OK: Egy Azure virtuális gépen futó Backup szolgáltatás ügynökének
Ha a hello Backup szolgáltatás ügynökének a virtuális gép fut, teljesítmény lesz lassabb, mint ha is futtat, a fizikai gépen. Ez egy várható üzenet, tooIOPS korlátozásai miatt.  Hello teljesítmény azonban hello adatmeghajtók, amelyekről tooAzure prémium szintű Storage alatt váltásával is optimalizálhatja. A probléma megoldásán dolgozunk, és hello javítás egy későbbi kiadásban elérhető lesz.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>OK: Biztonsági másolatának fájljait sok (több millió)
Nagy mennyiségű adat áthelyezését-nál kisebb mennyiségű adat áthelyezését hosszabb ideig tart. Bizonyos esetekben biztonságimásolat-készítési időpont kapcsolódó toonot csak hello hello adatokat, hanem a fájlok vagy mappák hello száma méretét. Ez különösen igaz, ha több millió kis fájlt (néhány bájt tooa néhány kilobájtostól) biztonsági mentése van folyamatban.

Ez akkor fordul elő, mert amíg hello adatok biztonsági mentése és tooAzure áthelyezni, Azure egyidejűleg katalogizálni a fájlokat. Bizonyos ritka esetekben hello katalógus művelet hosszabb ideig is tarthat.

a következő mutatók hello segítségével hello szűk ismertetése, és ennek megfelelően működnek a hello lépések:

* **Felhasználói felülete látható hello adatátvitel folyamatban**. hello továbbra is adatátvitel. hello hálózati sávszélesség vagy adatok hello mérete okozza késést.
* **Felhasználói felület nem látható folyamatjelző hello adatátvitel**. Megnyitás hello naplókat C:\Microsoft Azure Recovery Services Agent\Temp helyen, és ezután ellenőrizze a hello hello naplók FileProvider::EndData bejegyzést. Ez a bejegyzés azt jelzi, hogy, hogy hello adatátvitel befejeződött, és hello katalógus művelet történik. Hello biztonsági mentési feladat nem szakítható meg. Ehelyett Várjon, amíg már kissé hello katalógus művelet toofinish. Ha hello a probléma továbbra is fennáll, forduljon a [az Azure támogatási](https://portal.azure.com/#create/Microsoft.Support).
