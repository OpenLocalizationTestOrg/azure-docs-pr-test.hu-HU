---
title: "Hitelesítés az Amazon Web Services aaaConfigure |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate és az AWS erőforrások kezelése Azure Automation runbookjai az AWS hitelesítő adatainak ellenőrzése."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "aws-hitelesítés, aws konfigurálása"
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: magoedte
ms.openlocfilehash: 6edaa000c1b206d80fe64b18c729dac124849070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Forgatókönyvek hitelesítése az Amazon webszolgáltatásokkal
Az általános feladatoknak az Amazon webszolgáltatások (AWS) erőforrásaival történő automatizálása az Automation forgatókönyvekkel lehetséges az Azure szolgáltatásban.  Sok feladatot automatizálhat az AWS-ben az Automation forgatókönyvek használatával, ugyanúgy, mint az Azure erőforrásaival.  Mindössze két dologra van szükség:

* Egy AWS-előfizetésre és a hitelesítő adatokra.  Konkréten az AWS-hozzáférési kulcsára és a titkos kulcsára.  További információkért tekintse át az hello cikk [használatával AWS hitelesítő adatok](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Egy Azure-előfizetésre és egy Automation-fiókra.  Egy Azure Automation-fiók beállításával kapcsolatos további információkért tekintse át az hello cikk [konfigurálása Azure futtató fiók](automation-sec-configure-azure-runas-account.md).  

az AWS tooauthenticate, meg kell adnia egy készletét AWS hitelesítő adatok tooauthenticate az Azure Automation futó runbookok. Ha már rendelkezik egy Automation-fiók létrehozása és toouse, hogy az AWS tooauthenticate, lépésekkel hello hello a következő szakaszban található.  Ha runbookok adatforráselemhez AWS erőforrások toodedicated egy fiókot, először készítsen egy új [Automation Futtatás mint fiók](automation-sec-configure-azure-runas-account.md) (kihagyása hello beállítás toocreate egy egyszerű szolgáltatásnév) és kövesse az alábbi hello lépéseket.

## <a name="configure-automation-account"></a>Automation-fiók konfigurálása
Az Azure Automation toocommunicate az AWS először fog tooretrieve AWS hitelesítő adatait kell és tárolja őket az Azure Automationben eszközként.  Hajtsa végre a következő hello AWS dokumentumban leírt lépéseket hello [Tárelérési kulcsok kezelése az AWS fiók](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) toocreate egy hozzáférési kulcsot, és másolja hello **hozzáférési kulcs azonosító** és **titkos hívóbetű** (nem kötelező töltse le a kulcsfájl toostore azt biztonságos helyen).

Miután létrehozott és a AWS titkosítási kulcsok másolása, szüksége lesz a hitelesítőadat-eszköz egy Azure Automation-fiók toosecurely toocreate tárolja őket, és azok a runbookok hivatkozik.  Hello lépésekkel hello szakaszban **új hitelesítőadat-eszköz létrehozása** a hello [hitelesítőadat-eszköz az Azure Automationben](automation-credentials.md) a következő cikket, és írja be a következő információ hello:

1. A hello **neve** adja meg a **AWScred** vagy egy megfelelő értéket a elnevezési szabályai a következő.  
2. A hello **felhasználónév** mezőbe írja be a **hozzáférési azonosító** és a **titkos hívóbetű** a hello **jelszó** és **megerősítése jelszó** mezőbe.   

## <a name="next-steps"></a>Következő lépések
* Reivew hello megoldás cikk [automatizálása a virtuális gépet az Amazon Web Services](automation-scenario-aws-deployment.md) hogyan toocreate runbookok tooautomate feladatainak AWS toolearn.

