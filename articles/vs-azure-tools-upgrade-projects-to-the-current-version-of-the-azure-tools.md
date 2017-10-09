---
title: "aaaHow tooupgrade projektek toohello jelenlegi verziója hello Azure eszközök |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupgrade egy Azure projektre a Visual Studio toohello jelenlegi verziójával hello Azure-eszközök"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1d64070a-078d-468a-87f4-e6715de6475f
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: c89ba43af0f2fd9db46ce0c90f0da3d70dc1510b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupgrade-projects-toohello-current-version-of-hello-azure-tools-for-visual-studio"></a>Hogyan tooupgrade projektek a Visual Studio toohello hello Azure eszközök aktuális verziója
## <a name="overview"></a>Áttekintés
Hello Azure eszközök (vagy egy előző kiadása, amely újabb, mint az 1.6-os) jelenlegi kiadásában hello telepítése után minden projektek Azure-eszközök használatával létrehozott kiadási előtt 1.6 (November 2011) automatikusan frissíti, amint megnyitja őket. Ha Ön által létrehozott projektek hello ezekhez az eszközökhöz kiadása 1.6 (2011. November) és továbbra is fennáll, hogy a kiadás telepítve, nyissa meg ezeket a projektek hello régebbi verzióban, és döntse el, később e tooupgrade őket.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hogyan változik a projekthez, amikor a frissítés
A rendszer automatikusan frissíti a projekt, vagy megadja, hogy ha azt szeretné, a projekt már tooupgrade módosítva az egyes szerelvények aktuális verzióival toowork, és egyes tulajdonságok is van módosítani, mert ez a szakasz ismerteti. Ha a projekthez szükséges egyéb módosítások toobe hello eszközök hello újabb verziójával kompatibilis, módosítania kell azokat manuálisan.

* a webes szerepkörök hello web.config fájl és a hello app.config fájl feldolgozói szerepkörök is a frissített tooreference hello újabb verziójával Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.
* hello Microsoft.WindowsAzure.StorageClient.dll Microsoft.WindowsAzure.Diagnostics.dll és Microsoft.WindowsAzure.ServiceRuntime.dll szerelvények frissített toohello új verziója.
* Közzétételi profil hello (.ccproj) Azure-projekt fájlban tárolt áthelyezett tooa külön fájlt, hello bővítmény .azurePubXml, a hello **közzététel** alkönyvtár.
* Egyes tulajdonságok hello a közzétételi profil frissített toosupport új és módosított funkciók vannak. **AllowUpgrade** helyébe **DeploymentReplacementMethod** mert telepített felhőszolgáltatás egyidejűleg vagy fokozatosan frissítheti.
* hello tulajdonság **UseIISExpressByDefault** hozzáadott és toofalse be, hogy hello hibakereséshez használt webkiszolgáló nem változik az Internet Information Services (IIS) tooIIS Express. Az IIS Express hello alapértelmezett webkiszolgáló hello újabb kiadásaiban hello eszközök használatával létrehozott projektek.
* Ha egy vagy több, a projekt szerepkörök Azure gyorsítótárazás üzemelteti, néhány hello szolgáltatás konfigurációs (.cscfg fájl) és a szolgáltatás definíciós (.csdef fájl) tulajdonsága egy projekt frissítésekor módosult. Ha hello projekt hello Azure gyorsítótárazás NuGet-csomagot használ, a hello projekt frissített toohello hello csomag legutóbbi verzióját. Nyissa meg a web.config fájl hello kell, és győződjön meg arról, hogy hello ügyfél-konfigurációt megfelelően maradt hello frissítési folyamat során. Ha adott hello hivatkozások tooAzure gyorsítótárazását ügyfélszerelvények hello NuGet-csomag használata nélkül, nem lesz frissítve a szerelvények; Ezen hivatkozások toohello új verziói manuálisan kell frissíteni.

> [!IMPORTANT]
> Az F # projektek manuálisan kell frissíteni hivatkozások tooAzure szerelvényeket, hogy ezeket a szerelvények hello újabb verzióit hivatkozó.
> 
> 

### <a name="how-tooupgrade-an-azure-project-toohello-current-release"></a>Hogyan tooupgrade egy Azure projekt toohello jelenlegi kiadása
1. Telepítés hello aktuális verzióját hello Azure eszközök a Visual Studio telepítése hello toouse szánt hello frissíteni projekt, és a megjeleníteni kívánt tooupgrade majd nyitott hello projekt. Ha hello projekt, amely egy Azure eszközök készült kibocsátási előtt 1.6-os (2011. November), a hello projekt automatikusan frissített toohello aktuális verzió. Ha hello projekt létrehozásának hello a 2011. novemberi kiadásban és a kiadásban továbbra is telepítve van, a kiadás hello projekt nyílik meg.
2. A Megoldáskezelőben, nyissa meg hello helyi menüje hello projektcsomópontra, válassza a **tulajdonságok**, és válassza a hello **alkalmazás** , a megjelenő párbeszédpanel hello lapján.
   
    Hello **alkalmazás** lapon látható hello projekt társított hello eszközök verziója. Hello Azure eszközök aktuális verziója jelenik meg, ha hello projekt már frissítették. Ha telepített egy újabb verziója hello eszközök milyen hello lapon láthatók, mint egy **frissítése** gomb akkor jelenik meg.
3. Válassza ki a hello **frissítése** gomb tooupgrade projekt toohello aktuális verziója hello eszközök.
4. Hello projekt buildjének elkészítéséhez, és majd címet a API változások hibákról. További információ hogyan toomodify hello új verziója, a kódját a dokumentációban hello hello az adott API.

