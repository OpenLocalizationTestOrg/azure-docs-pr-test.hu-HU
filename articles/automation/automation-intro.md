---
title: aaaWhat az Azure Automation |} Microsoft Docs
description: "Ismerje meg, milyen értéket Azure Automation biztosít, és választ kaphat toocommon kérdéseket, hogy elkezdheti létrehozása, használata a runbookokat és az Azure Automation DSC."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "az Automation ismertetése, Azure Automation, Azure Automation példák"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 1e5a90e272d6b2beb7b5007e2fea2c110dbd79b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-overview"></a>Az Azure Automation áttekintése
A Microsoft Azure Automation lehetőséget biztosít a felhasználók tooautomate hello manuális, hosszan futó, hibákhoz vezethet, és gyakran ismétlődő feladatokat gyakran kerül sor a felhőalapú és nagyvállalati környezetben. Az időt takaríthat meg, és növeli a megbízhatóságot hello a szokásos felügyeleti feladatok és az még ütemezések őket toobe automatikusan végre rendszeres időközönként. A folyamatokat automatizálhatja forgatókönyvek segítségével, vagy automatizálhat konfigurációkezelést a Célállapot-konfigurációval (DSC). Ez a cikk röviden áttekinti az Azure Automationt, valamint választ ad néhány gyakran felmerülő kérdésre. Ebben a könyvtárban lévő tooother cikkeket, olvassa el a hello különböző témakörök részletesebb tájékoztatást.

## <a name="automating-processes-with-runbooks"></a>Folyamatok automatizálása forgatókönyvekkel
A forgatókönyv egy feladatkészlet, amely bizonyos automatizált folyamatokat hajt végre az Azure Automationben. Lehet, például egy virtuális gép, és a naplóbejegyzés létrehozhat egy egyszerű folyamat, vagy előfordulhat, hogy más kisebb runbookok tooperform összetett folyamat több erőforrást, vagy még több felhők és a helyszíni környezetek összetett runbook.  

Például lehetséges, hogy egy meglévő manuális csonkítása SQL-adatbázis, ha azt hamarosan eléri maximális méretét, például a kiszolgálóhoz való csatlakozáskor toohello, csatlakozás toohello adatbázis több lépéseket a folyamatot, get hello adatbázis jelenlegi mérete, ellenőrizze, hogy küszöbérték túllépte csonkolja azt és felhasználó értesítése. Ahelyett, hogy ezeket a lépéseket manuálisan hajtaná végre, létrehozhat egy forgatókönyvet, amely egyetlen folyamatban hajtja végre az összes feladatot. Kívánja hello runbook indítása, adja meg például hello SQL-kiszolgáló neve, az adatbázisnév és a címzett e-mail hello szükséges információkat, majd ismét, amíg a hello folyamat befejeződik, majd elhelyezkedik. 

## <a name="what-can-runbooks-automate"></a>Mit lehet automatizálni forgatókönyvekkel?
Az Azure Automation forgatókönyvek Windows PowerShellen vagy Windows PowerShell-munkafolyamaton alapulnak, tehát képesek mindenre, amire a PowerShell. Ha egy alkalmazás vagy szolgáltatás rendelkezik API-val, a forgatókönyv tudja azt használni. Ha egy PowerShell-modul hello alkalmazáshoz, akkor modult betöltése az Azure Automation és a runbookban ezeket a parancsmagokat tartalmaznak. Azure Automation runbookjai hello Azure felhőalapú futnak, és a felhőben található erőforrásokat, vagy a külső erőforrásokat, hello felhőből elérhető. Használatával [hibrid forgatókönyv-feldolgozó](automation-hybrid-runbook-worker.md), futtathatja a helyi adatok center toomanage helyi erőforrásokhoz. 

## <a name="getting-runbooks-from-hello-community"></a>Első runbookokat hello Közösségtől
Hello [forgatókönyvek](automation-runbook-gallery.md#runbooks-in-runbook-gallery) használhatja változatlanul a környezetben a Microsoft és hello közösségének forgatókönyveket tartalmaz, vagy testre is szabhatja őket a saját célokra. Azok is hasznos tooas hivatkozások toolearn hogyan toocreate saját runbookok. Akkor is hozzájárulhat a saját runbookok toohello gyűjteménye, hogy úgy gondolja, hogy más felhasználók is hasznosak. 

## <a name="creating-runbooks-with-azure-automation"></a>Forgatókönyvek létrehozása Azure Automationnel
Is [létrehozhatja a saját runbookokat](automation-creating-importing-runbook.md) az ideiglenes, vagy módosítsa a runbookokat hello [forgatókönyvek](http://msdn.microsoft.com/library/azure/dn781422.aspx) saját igényeinek. Négy különböző [forgatókönyvtípus](automation-runbook-types.md) közül választhat az igényei és a PowerShell-tapasztalata alapján. Ha inkább közvetlenül a PowerShell-kódjába hello toowork, akkor is használhatja a [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) vagy [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks) offline vagy hello szerkesztése [szöveges szerkesztő](http://msdn.microsoft.com/library/azure/dn879137.aspx) a hello Azure-portálon. Ha jobban szeret tooedit anélkül, hogy egy runbook közzétett toohello az alapul szolgáló kódot, majd létrehozhat egy [grafikus forgatókönyvnek](automation-runbook-types.md#graphical-runbooks) hello segítségével [grafikus szerkesztő](automation-graphical-authoring-intro.md) a hello Azure-portálon. 

Tanul tooreading? Rá egy pillantást hello alábbi videó 2015. május Microsoft Ignite-munkamenetből. Megjegyzés: Hello fogalmakat és funkciók némelyike Ez a videó helyességét, Azure Automation hol sokkal mivel ez a videó lett felvéve, amíg most már rendelkezik egy szélesebb körű felhasználói felület hello Azure-portálon, és további funkciókat támogatja.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 

## <a name="automating-configuration-management-with-desired-state-configuration"></a>A konfigurációkezelés automatizálása a célállapot-konfigurációval (DSC)
[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) egy felügyeleti platform, amely lehetővé teszi a toomanage, bevezetésének és betartatásának konfigurációja a fizikai gazdagépek és virtuális gépek deklaratív PowerShell-szintaxis segítségével. Meghatározhat a célgépeken automatikusan lekérhető és alkalmazható konfigurációkat egy központi DSC lekéréses kiszolgálón. A DSC biztosít a PowerShell-parancsmagok toomanage konfigurációk és erőforrások használatát is.  

Az [Azure Automation DSC](automation-dsc-overview.md) egy felhőalapú megoldás a PowerShell DSC-hez, amely vállalati környezetekhez szükséges szolgáltatásokat biztosít.  Az Azure Automation DSC-erőforrások kezeléséhez, és konfigurációk toovirtual vagy fizikai gépek, amelyek egy kiszolgálóról DSC lekéréses hello Azure felhőben kérheti le azokat alkalmazni.  Jelentéskészítési szolgáltatásokat is biztosít, amelyek tájékoztatást adnak olyan fontos eseményekről, mint a csomók letérése a kijelölt konfigurációról vagy egy új konfiguráció alkalmazása 

## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Saját DSC-konfigurációk létrehozása az Azure Automationnel
[A DSC-konfigurációk](automation-dsc-overview.md) adja meg a csomópont szükséges hello állapotát.  Több csomópont is érvényesek lesznek hello azonos konfigurációs tooassure, hogy minden megőriznek állapota nem azonos.  Létrehozhat egy konfigurációt bármilyen szövegszerkesztővel a helyi gépén, majd importálhatja az Azure Automationbe, ahol lefordíthatja, majd alkalmazhatja a csomópontokra.

## <a name="getting-modules-and-configurations"></a>Modulok és konfigurációk beszerzése
Beszerezheti [PowerShell-modulok](automation-runbook-gallery.md#modules-in-powershell-gallery) használható a runbookok és a DSC-konfigurációk hello a parancsmagokat tartalmazó [PowerShell-galériában](http://www.powershellgallery.com/). Indítsa el ezt a tárat a hello Azure-portál és a modulok közvetlenül importálja az Azure Automation, vagy letöltheti, és importálja azokat manuálisan. Hello modulok nem telepíthető közvetlenül a hello Azure-portálon, de letöltheti azokat mint bármely más modulja a telepítést. 

## <a name="example-practical-applications-of-azure-automation"></a>Az Azure Automation gyakorlati alkalmazásai
Az alábbiakban néhány példát a hello különböző automatizálási esetekben az Azure Automation szolgáltatásban, melyek. 

* Különböző Azure-előfizetésekben lévő virtuális gépek létrehozása és másolása. 
* Ütemezés fájlt másolja át a helyi számítógép tooan Azure Blob Storage tárolóban. 
* Biztonsági funkciók automatizálása (pl. egy ügyfél kéréseinek megtagadása, amikor a rendszer szolgáltatásmegtagadási támadást észlel). 
* Annak biztosítása, hogy a gépek folyamatosan igazodnak a konfigurált biztonsági házirendhez.
* Az alkalmazáskód folyamatos telepítése a felhőben és a helyszíni infrastruktúrán. 
* Active Directory-erdő létrehozása az Azure-ban a tesztkörnyezethez. 
* Tábla csonkolása egy SQL-adatbázisban, ha az adatbázis már majdnem elérte a maximális méretet. 
* A környezeti beállítások távoli frissítése egy Azure-webhelyhez. 

## <a name="how-does-azure-automation-relate-tooother-automation-tools"></a>Milyen kapcsolatban áll a Azure Automation tooother automatizálási eszközeivel?
[Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) van tervezett tooautomate hello magánfelhő-alapú felügyeleti feladatait. Helyben van telepítve az adatközpontban a [Microsoft Azure csomag](https://www.microsoft.com/en-us/server-cloud/) részeként. SMA és Azure Automation hello használata Windows PowerShell és a Windows PowerShell munkafolyamat alapján runbook ugyanazt a formátumot, de nem támogatja az SMA [grafikus forgatókönyvek](automation-graphical-authoring-intro.md).  

A [System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) célja a helyszíni erőforrások automatizálása. Azure Automation és a Szolgáltatáskezelési automatizálás rendszereknél egy másik runbook formátumot használja, és van egy grafikus felület toocreate runbookok parancsfájlírás nélkül. A forgatókönyvei kifejezetten az Orchestrator számára írt integrációs csomag tevékenységeiből állnak össze. 

## <a name="where-can-i-get-more-information"></a>Hol kaphatok további információkat?
A különböző erőforrások meg toolearn Azure Automation és saját runbookok létrehozásáról további érhetők el. 

* Az **Azure Automation Library** a hely, ahol jelenleg van. hello a tár cikkekben teljes dokumentációját hello konfigurációs és felügyeleti Azure Automation és a saját runbookok. 
* Az [Azure PowerShell-parancsmagok](http://msdn.microsoft.com/library/jj156055.aspx) az Azure műveletek Windows PowerShell segítségével való automatizálásáról nyújtanak információkat. Runbookok használata Azure-erőforrások ezen parancsmagok toowork. 
* [Felügyeleti Blog](https://azure.microsoft.com/blog/tag/azure-automation/) Azure Automation és más felügyeleti technológiák – a Microsoft hello legújabb információt nyújt. Hello Azure Automation-csapat legújabb ajánlatos toothis blog toostay toodate hello a mentést. 
* [Automatizálási fórum](http://go.microsoft.com/fwlink/p/?LinkId=390561) lehetővé teszi a Microsoft és automatizálás közösségi hello Azure Automation toobe toopost kérdésekre. 
* Az [Azure Automation parancsmagok](https://msdn.microsoft.com/library/mt244122.aspx) az adminisztrációs feladatok automatizálásához nyújtanak információt. Parancsmagok toomanage Automation-fiók, az eszközök, a runbookokat, DSC tartalmaz.

## <a name="can-i-provide-feedback"></a>Küldhetek visszajelzést?
**Várjuk a visszajelzését!** Ha egy Azure Automation-forgatókönyv megoldást vagy egy integrációs modult keres, küldjön egy Parancsfájlkérelmet a Script Centerbe. Ha visszajelzést vagy funkciókérést küldene az Azure Automation vonatkozásában, tegye azt közzé a [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback) fórumon. Köszönjük! 

