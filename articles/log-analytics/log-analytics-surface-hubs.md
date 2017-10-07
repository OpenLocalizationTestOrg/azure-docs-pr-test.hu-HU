---
title: "az Azure Naplóelemzés Surface hub aaaMonitor |} Microsoft Docs"
description: "Hello Surface Hub megoldás tootrack hello állapotát a Surface hub használja, és megérteni, hogyan kívánják felhasználni."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 623d30e749cafdd4a34ba0c5b3408164f1b4a95b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-tootrack-their-health"></a>A Naplóelemzési tootrack Surface hub figyelő a rendszerállapot

![Surface Hub szimbólum](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Ez a cikk ismerteti, hogyan használhatja a Naplóelemzési toomonitor Microsoft Surface Hub eszközöket a Microsoft Operations Management Suite (OMS) hello hello Surface Hub megoldás. Jelentkezzen a Surface hub hello állapotát, valamint megérteni, hogyan kívánják felhasználni nyomon Analytics segítségével.

Minden egyes Surface Hub rendelkezik hello Microsoft Monitoring Agent telepítését. Az, hogy adatokat küldhet a Surface Hub tooOMS a hello ügynök keresztül. Naplófájlok a Surface hub és a olvasni, majd küldött toohello OMS szolgáltatáshoz. Problémák, mint a kiszolgáló nem kapcsolódik, a naptár nem szinkronizálása hello, vagy ha hello fiók Skype be nem toolog láthatók OMS hello Surface Hub irányítópulton. Hello irányítópult hello adatok használatával azonosíthatja az eszközök nem fut, vagy, hogy más problémákat, amelynek, potenciálisan alkalmazhat hello észlelt probléma javítását.

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
A következő információk tooinstall hello használja, és hello megoldás konfigurálása. A sorrend toomanage a hello Microsoft Operations Management Suite (OMS), a Surface hub szüksége lesz a következő hello:

* Egy érvényes előfizetéssel túl[OMS](http://www.microsoft.com/oms).
* Egy [OMS előfizetés](https://azure.microsoft.com/pricing/details/log-analytics/) szint hello szám támogató eszközök toomonitor szeretné. OMS árképzési változó attól függően, hogy hány eszköz regisztrálva van, és mennyi adatot azt folyamatokat. Érdemes tootake ezt számításba venni a Surface Hub-bevezetés tervezése során.

A következő fog egy OMS előfizetés tooyour meglévő Microsoft Azure-előfizetés felvétele vagy hozzon létre egy új munkaterületet hello OMS-portálon keresztül közvetlenül. Részletes utasításokat a következő módszerek egyikével érték [Ismerkedés a Naplóelemzési](log-analytics-get-started.md). Hello OMS-előfizetés beállítása után van két módon tooenroll a Surface Hub eszközöket:

* Automatikusan Intune-nal
* Manuálisan osztályig **beállítások** Surface Hub eszközén.

## <a name="set-up-monitoring"></a>Állítsa be a figyelést,
Hello állapotát és tevékenységét a Surface Hub OMS Naplóelemzési használatával figyelheti. Hello Surface Hub az OMS regisztrálása az Intune-ban, vagy helyileg segítségével **beállítások** a Surface Hub hello.

## <a name="connect-surface-hubs-toooms-through-intune"></a>Csatlakozás a Surface hub tooOMS Intune-nal
Akkor lesz kell hello munkaterület Azonosítóját és kulcsát az hello OMS-munkaterület, a Surface hub kezelésére. Hello OMS-portálon szerintiek kérheti le.

Intune-ban egy Microsoft-termék, amely lehetővé teszi toocentrally hello OMS beállítások, amelyek alkalmazott tooone vagy több, az eszközök kezelése. Kövesse ezeket a lépéseket tooconfigure az eszközök Intune-nal:

1. Jelentkezzen be tooIntune.
2. Keresse meg a túl**beállítások** > **csatlakoztatott források**.
3. Hozzon létre vagy hello Surface Hub sablonon alapuló házirend szerkesztése.
4. Keresse meg a toohello OMS (Azure Operational Insights) szakasz hello házirend, és adja hozzá a hello *munkaterület azonosítója* és *Munkaterületkulcsot* toohello házirend.
5. Hello szabályzat mentése.
6. Hello házirend társítása hello megfelelő eszközcsoportra.

   ![Az Intune-házirend](./media/log-analytics-surface-hubs/intune.png)

Az Intune majd szinkronizál hello OMS beállítások hello célcsoportot, ha regisztrálja azokat az OMS-munkaterület hello eszközöket.

## <a name="connect-surface-hubs-toooms-using-hello-settings-app"></a>Csatlakozás a Surface hub tooOMS hello-beállítások alkalmazással
Akkor lesz kell hello munkaterület Azonosítóját és kulcsát az hello OMS-munkaterület, a Surface hub kezelésére. Hello OMS-portálon szerintiek kérheti le.

Ha nem adja meg Intune toomanage a környezetében, manuálisan keresztül az eszközök regisztrálása **beállítások** minden Surface hub:

1. Nyissa meg a Surface Hub **beállítások**.
2. Adja meg a hello eszközt rendszergazdai hitelesítő adatokat, amikor a rendszer kéri.
3. Kattintson a **az eszköz**, és a hello **figyelés**, kattintson a **OMS-beállítások konfigurálása**.
4. Válassza ki **engedélyezze a megfigyelést**.
5. Hello OMS-beállítások párbeszédpanelen írja be a hello **munkaterület azonosítója** és típus hello **Munkaterületkulcsot**.  
   ![beállítások](./media/log-analytics-surface-hubs/settings.png)
6. Kattintson a **OK** toocomplete hello konfigurációs.

Megerősítést felszólító-e az OMS-konfigurációja sikeresen hello alkalmazott toohello eszköz jelenik meg. Amennyiben igen, megjelenik egy üzenet, amely meghatározza, hogy az hello ügynök sikeresen csatlakoztatva lett toohello OMS szolgáltatáshoz. hello eszköz ezután elindítja az adatok tooOMS, ahol megtekintheti és a működésre küldésekor.

## <a name="monitor-surface-hubs"></a>A figyelő hubok
Figyelés, a Surface hub OMS használata sokkal hasonlóan a további regisztrált eszközök figyelését.

1. Bejelentkezés toohello OMS-portálon.
2. Keresse meg a toohello Surface Hub megoldás pack irányítópult.
3. Az eszköz állapota akkor jelenik meg.

   ![Surface Hub irányítópult](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Létrehozhat [riasztások](log-analytics-alerts.md) meglévő vagy egyéni napló keresés alapján. Hello adatok hello OMS gyűjti össze a Surface hub használva kereshet problémákat és az eszközök meghatározó hello feltételeket a riasztás.

## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes Surface Hub-adatok.
* Hozzon létre [riasztások](log-analytics-alerts.md) toonotify, ha a probléma lép fel a Surface hub.
