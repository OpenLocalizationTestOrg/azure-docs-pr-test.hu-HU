---
title: "aaaSetting mentése nevű hitelesítő adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan tootooprovide hitelesítő adatokat, hogy a Visual Studio használhatja tooauthenticate kérelmek tooAzure toopublish egy alkalmazás tooAzure a Visual Studio vagy egy meglévő toomonitor felhőalapú szolgáltatás... "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 3cc147a2f7a3e766293ca282f56078cf24281346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-named-authentication-credentials"></a>Nevű hitelesítő adatok beállítása
egy alkalmazás tooAzure a Visual Studio vagy egy meglévő felhőszolgáltatáshoz toomonitor toopublish, meg kell adni hitelesítő adatokat kéri, hogy a Visual Studio tooauthenticate használhatja a tooAzure. A Visual Studio, ahol bejelentkezhet tooprovide ezeket a hitelesítő adatokat több helyen van. Például a Server Explorer hello, megnyitható hello helyi menüje hello **Azure** csomópont válassza **csatlakozás Azure-előfizetés tooMicrosoft...** . Amikor bejelentkezik, az Azure-fiókjával társított hello előfizetési adatok érhető el a Visual Studio, és nincs szükség további toodo rendelkezik.

Azure-eszközök használatát is támogatja egy régebbi módját adja meg hitelesítő adatokat, a hello előfizetés fájlt (.publishsettings fájlt). Ez a témakör ismerteti a metódus, amely az Azure SDK 2.2 továbbra is támogatja.

a következő elemek hello hitelesítési tooAzure szükségesek:

* Az előfizetés-Azonosítóval
* Egy érvényes X.509 v3 tanúsítvány

> [!NOTE]
> hello hello X.509 v3 tanúsítvány kulcs hossza legalább 2048 bites kell lennie. Azure elutasítják bármely olyan tanúsítvány, amely nem felel meg ennek a követelménynek, vagy, amely nem érvényes.
>
>

A Visual Studio hitelesítő adatok az előfizetés-Azonosítóval hello Tanúsítványadatok együtt használja. hello előfizetés (fájl .publishsettings), amely tartalmaz egy hello tanúsítványhoz tartozó nyilvános kulcs hivatkozott hello megfelelő hitelesítő adatokkal. hello előfizetés fájl tartalmazhat egynél több előfizetésnek hitelesítő adatait.

Szerkesztheti a hello hello előfizetési adatok **új/szerkesztése előfizetés** párbeszédpanelen, a témakör későbbi részében leírtak szerint.

Ha azt szeretné toocreate tanúsítvány saját magának, olvassa el a toohello utasításait [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) , majd töltse fel manuálisan hello tanúsítvány toohello [a klasszikus Azure portálon ](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Ezeket a hitelesítő adatokat, amelyek a Visual Studio igényli a felhőszolgáltatások nem toomanage hello ugyanazokat a hitelesítő adatokat, amelyek a szükséges tooauthenticate hello Azure storage szolgáltatások egy kérelmet.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Importálja, állítsa be, vagy a Visual Studio hitelesítő adatok szerkesztése
Importálja, állítsa be, vagy módosítsa a hitelesítő adatok a hello **új előfizetés** párbeszédpanelt, amely akkor jelenik meg, ha hajt végre a következő művelet hello:

* A **Server Explorer**, nyissa meg hello helyi menüje hello **Azure** csomópontot, válassza a **kezelése és a szűrő előfizetések...** , válassza ki a hello **tanúsítványok** lapot, és hello a következőket:

    * Válasszon **importálása** tooopen hello importálása a Microsoft Azure-előfizetések párbeszédpanel le hello hello előfizetések fájl jelenleg betöltött előfizetés, a Tallózás tooits letöltési hely, és importálja a használatra hitelesítés.
    * Válasszon **új** tooopen-hello új előfizetés párbeszédpanel, ahol beállíthat egy új előfizetés hitelesítés használható.
    * Válasszon **szerkesztése** (Miután kiválasztotta a aktív előfizetéssel) tooopen hello előfizetés szerkesztése párbeszédpanelen egy meglévő előfizetéshez hitelesítési használható szerkesztésére. 

hello következő eljárás azt feltételezi, hogy hello **új előfizetés** párbeszédpanel nyitva.

### <a name="tooset-up-authentication-credentials-in-visual-studio"></a>a Visual Studio hitelesítő adatok mentése tooset
1. A hello **jelöljön ki egy létező tanúsítványt** hitelesítési listáját, válasszon egy tanúsítványt.
2. Válassza ki a hello **másolási hello teljes elérési útja** hivatkozásra. hello hello tanúsítvány (.cer-fájl) elérési út vágólapra másolt toohello.

   > [!IMPORTANT]
   > toopublish az Azure-alkalmazást a Visual Studio eszközből, fel kell töltenie a tanúsítvány toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. tooupload hello tanúsítvány toohello Azure-portálon:

   1. Nyissa meg hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   2. Ha a rendszer kéri, jelentkezzen be toohello portálon, és keresse meg a túl**beállítások**, **felügyeleti tanúsítványok**.
   3. Hello felügyeleti tanúsítványok ablaktábláján válassza **feltöltése**.
   4. Válassza ki az Azure-előfizetéssel, illessze be hello fájljának teljes elérési útját hello .cer imént létrehozott, és válassza **feltöltése**.
