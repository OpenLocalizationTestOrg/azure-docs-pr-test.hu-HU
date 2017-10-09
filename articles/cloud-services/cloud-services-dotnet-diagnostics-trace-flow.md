---
title: "az Azure Diagnostics felhőalapú szolgáltatások alkalmazásokban aaaTrace hello folyamata |} Microsoft Docs"
description: "Adja hozzá a nyomkövetés üzenetek tooan Azure alkalmazás toohelp hibakeresés, méri a teljesítményt, figyelés, forgalom elemzése és több."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: d2ed7b5997ae1d298115b4ce593bb5051a9a0c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="trace-hello-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Az Azure Diagnostics Felhőszolgáltatások alkalmazás nyomkövetési hello folyamata
Nyomkövetés módja, toomonitor hello végrehajtásra az alkalmazás a futtatása. Használhatja a hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), és [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) osztályok toorecord hibákkal kapcsolatos információkat és alkalmazás végrehajtási naplók, szövegfájlok vagy más eszközök későbbi elemzés céljából. További információ a nyomkövetési: [nyomkövetés és tagolása alkalmazások](https://msdn.microsoft.com/library/zs6s4h68.aspx).

## <a name="use-trace-statements-and-trace-switches"></a>Nyomkövetési utasítások és nyomkövetési kapcsolók használata
Hello hozzáadásával a Felhőszolgáltatások alkalmazás nyomkövetési megvalósítása [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello alkalmazás konfigurációja, és hogy a tooSystem.Diagnostics.Trace vagy a System.Diagnostics.Debug meghívja a alkalmazás kódja. Hello konfigurációs fájlokat használjon *app.config* feldolgozói szerepköröket és hello *web.config* webes szerepkörök. Amikor létrehoz egy új üzemeltetett szolgáltatást, a Visual Studio-sablonnal, Azure Diagnostics automatikusan fel lesz véve toohello projektet, és a hello DiagnosticMonitorTraceListener toohello megfelelő konfigurációs fájl felvételekor hello szerepkörök ad hozzá.

Információ a nyomkövetési utasítások elhelyezéséhez: [hogyan: hozzáadása nyomkövetési utasítások tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).

Ha kialakít [nyomkövetési kapcsolók](https://msdn.microsoft.com/library/3at424ac.aspx) nyomkövetés következik be, és hogyan kiterjedt szabályozhatja a kódban. Ez lehetővé teszi az alkalmazás az éles környezetben hello állapotának figyelése. Ez különösen fontos a több számítógépen futnak több összetevőt használ üzleti alkalmazást. További információkért lásd: [hogyan: nyomkövetési kapcsolók konfigurálása](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-hello-trace-listener-in-an-azure-application"></a>Nyomkövetés-figyelő hello konfigurálása az Azure alkalmazásban
Nyomkövetési, a hibakeresési és TraceSource, ehhez "figyelői" toocollect és küldött üzenetek rekord hello beállítása. Figyelők gyűjtése, tárolására és nyomkövetés üzenetek. Azok a közvetlen hello nyomkövetési kimeneti tooan megfelelő cél, például a napló, ablakot vagy szövegfájl. Az Azure Diagnostics használ hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) osztály.

A következő eljárás hello befejezése előtt inicializálni kell hello Azure diagnosztikai figyelő. toodo a, lásd: [diagnosztika engedélyezése a Microsoft Azure-ban](cloud-services-dotnet-diagnostics.md).

Vegye figyelembe, hogy a Visual Studio által biztosított hello sablonok használatakor hello konfigurációs hello figyelő kerül automatikusan meg.

### <a name="add-a-trace-listener"></a>Adja hozzá a nyomkövetés-figyelő
1. Nyissa meg a szerepkör az hello web.config vagy az app.config fájlt.
2. Adja hozzá a következő kód toohello fájl hello. Módosítsa a hello attribútum toouse hello verzió verziószámát hivatkozott hello szerelvény. hello szerelvény verziója nem feltétlenül módosíthatja az egyes Azure SDK kiadások kivéve, ha nincsenek frissítések tooit.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Győződjön meg arról, hogy a projekt hivatkozás toohello Microsoft.WindowsAzure.Diagnostics szerelvény. Frissítés hello verziószáma hello xml hello toomatch hello verziója újabb Microsoft.WindowsAzure.Diagnostics szerelvény hivatkozik.
   > 
   > 
3. Mentse a hello konfigurációs fájlt.

Figyelők kapcsolatos további információkért lásd: [nyomkövetésfigyelők](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Hello lépéseket tooadd hello figyelő befejezése után a nyomkövetési utasítások tooyour kód is hozzáadhat.

### <a name="tooadd-trace-statement-tooyour-code"></a>tooadd nyomkövetési utasítás tooyour kód
1. Az alkalmazás a forrásfájl megnyitásakor. Például hello <RoleName>hello feldolgozói szerepkör és a webes szerepkör .cs fájlban.
2. Adja hozzá a következő hello utasítás használatával, ha már nincs hozzáadva:
    ```
        using System.Diagnostics;
    ```
3. Nyomkövetési utasítások, ahol az alkalmazás állapota hello toocapture információt szeretne hozzáadni. Számos különféle módszereket tooformat hello kimenete hello nyomkövetési utasítás használható. További információkért lásd: [hogyan: hozzáadása nyomkövetési utasítások tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Mentse a hello forrásfájl.

