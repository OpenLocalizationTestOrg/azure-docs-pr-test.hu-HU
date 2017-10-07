---
title: "aaaBest eljárások az Azure App Service"
description: "Ajánlott eljárások és hibaelhárítás az Azure App Service megismerése"
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a1d3127f5a746aa43f7b56b45f17c45df9087bee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-app-service"></a>Azure App Service – ajánlott eljárások
Ez a cikk foglalja össze az ajánlott eljárások a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Közös elhelyezés
Ha egy megoldást, például a web app és az adatbázis létrehozása az Azure erőforrások különböző régiókban hello hatások találhatók hello következő alábbiakból állhat:

* Nagyobb késéseket erőforrások közötti kommunikáció
* A kimenő adatok pénzügyi díjak átviteli kereszt-régió, a hello [Azure díjszabása](https://azure.microsoft.com/pricing/details/data-transfers).

Közös elhelyezést a hello ugyanabban a régióban hasonló megoldással például egy webalkalmazás létrehozása az Azure erőforrások és egy adatbázis vagy a tárolási fiók használt toohold tartalmat vagy adatokat. Ha létrehozása, győződjön meg arról, hogy az erőforrások hello azonos Azure-régió, kivéve, ha az adott üzleti van, vagy azokat nem toobe okát tervezése. Az App Service alkalmazás toohello áthelyezheti ugyanabban a régióban legyen az adatbázis, ami hello [App Service klónozási szolgáltatásának](app-service-web-app-cloning-portal.md) jelenleg elérhető alkalmazások prémium szintű App Service-csomag.   

## <a name="memoryresources"></a>Ha használnak az alkalmazások a vártnál több memória
Ha azt észleli, egy alkalmazás felügyelet jelöli a vártnál több memóriát fogyaszt, vagy szolgáltatási javaslatokat, érdemes lehet hello [App Service automatikus javítás funkció](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). A memória küszöbértéke alapuló egyéni műveletek tart hello beállítások egyikét a hello automatikus javítás funkció. E-mail értesítések tooinvestigation keresztül memória memóriakép tooon helyszíni megoldás újrahasznosítási hello munkavégző folyamat által a műveletek a span hello pontszámot. Automatikus javítás konfigurálható és egy rövid felhasználói felületen keresztül Web.config fájlban a következő blogbejegyzésben hello a részben ismertetett módon [szolgáltatás támogatási webhely bővítmény](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Ha használja a vártnál több CPU-alkalmazások
Ha azt észleli az alkalmazás feldolgozó további CPU mint várt vagy nem észlel ismétlődő CPU igényeiben jelentkező felügyelet jelöli, vagy szolgáltatási javaslatokat távolítsa el a vertikális felskálázásával vagy hello App Service-csomag kiterjesztése. Az alkalmazás statefull, vertikális felskálázásával akkor hello egyetlen, amíg az alkalmazás esetén állapotmentes, méretezési ki fogja diktálni nagyobb rugalmasságot és nagyobb skálázási lehetséges. 

"Statefull" vagy "állapotmentes" alkalmazások kapcsolatos információkért tekintse meg a videót: [méretezhető-végpontok Többrétegű alkalmazások tervezése a Microsoft Azure Web Apps](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). App Service méretezés és az automatikus skálázás beállításokkal kapcsolatos további információkért olvassa el: [egy webalkalmazás skálázása az Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>A szoftvercsatorna-erőforrások elfogytak, amikor
Egy skálázását kimenő TCP-kapcsolatok gyakori oka hello használata klienskódtárait, amelyek nem megvalósított tooreuse TCP-kapcsolatok vagy hello esetben egy magasabb szintű protokoll, például a HTTP - Keep-Alive nem alkalmazhatók. Tekintse át az egyes hello alkalmazások vannak konfigurálva, vagy a kimenő kapcsolatok hatékony használatának kódját érhetők el az App Service-csomag tooensure által hivatkozott hello szalagtárak hello dokumentációját. Hello library dokumentációjában útmutató megfelelő létrehozása és kapcsolatok megakadályozására kiadás vagy karbantartási tooavoid is kövesse. Az ilyen ügyfelek szalagtárak vizsgálatokat végrehajtása közben folyamatban hatás mérsékelheti toomultiple példányok kiterjesztése.  

## <a name="appbackup"></a>Ha az alkalmazás biztonsági mentés indítása sikertelen
hello miért alkalmazás biztonsági mentés hibája van két leggyakoribb okok: Érvénytelen tárolási beállításokat, és érvénytelen adatbázis-konfiguráció. Ezek a hibák általában fordulhat elő, ha módosítások toostorage vagy adatbázis-erőforrások, illetve arról, hogyan tooaccess ezeket az erőforrásokat (pl. hitelesítő adatok hello biztonsági mentés beállításait a kijelölt hello adatbázis frissítve). Biztonsági mentése általában ütemezhető és igényel hozzáférést toostorage (a fájlok biztonsági mentése hello írása) és az adatbázisok (a másolás, és a tartalom toobe hello biztonsági mentésben szereplő olvasása). Ezek az erőforrások valamelyike lenne tooaccess sikertelen eredményét hello konzisztens sikertelen biztonsági mentéshez. 

Ha biztonsági mentési hibák fordulhat elő, tekintse át a legutóbbi eredmények toounderstand milyen típusú hiba történik. Hozzáférés tárolóhibák hello esetben ellenőrizze és hello tárolási beállítások használt hello biztonsági mentési konfiguráció frissítése. Az adatbázis-hozzáférési hiba hello esetben ellenőrizze és Alkalmazásbeállítások; részeként a kapcsolatok karakterláncok frissítése majd folytassa a tooupdate a biztonsági mentési konfigurációhoz tooproperly szükséges hello adatbázisokat tartalmazza. További információk az alkalmazás biztonsági mentési: hello [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md) dokumentációját.

## <a name="nodejs"></a>Ha új Node.js-alkalmazások telepített tooAzure App Service
Node.js-alkalmazások az Azure App Service alapértelmezett konfigurációjának tervezett toobest színből hello igényeinek leggyakrabban használt alkalmazások. Ha a Node.js-alkalmazás konfigurációja volna hasznot húzhatnak a személyre szabott hangolási tooimprove teljesítmény vagy erőforrás-használat Processzor/memória/hálózati erőforrások optimalizálására, akkor lehetett tekintse át az ajánlott eljárásokról és a hibaelhárítási lépéseket. A dokumentáció cikkből hello iisnode beállítások szükség lehet a Node.js alkalmazás tooconfigure ismerteti, hogyan hello különböző forgatókönyveket vagy problémákat, amelyek az alkalmazás esetleg irányuló és mutat be tooaddress ezeket a problémákat: [ajánlott eljárások és Azure App Service csomópont alkalmazások hibaelhárítási útmutatójában](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   

