---
title: "aaaDebug az alkalmazást a Visual Studio |} Microsoft Docs"
description: "Tovább fejlesztheti hello megbízhatóságának és teljesítményének a szolgáltatások fejlesztéséhez és a helyi fejlesztési fürtök hibakeresés a Visual Studio őket."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 8d08689e82195d09f57b462631ad04fd237bc4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával
> [!div class="op_single_selector"]
> * [A Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Egy helyi Service Fabric-alkalmazás hibakeresése
Időt és pénzt takaríthat telepítésével és az Azure Service Fabric-alkalmazás egy számítógép helyi fejlesztési fürtöt a hibakeresést. Visual Studio 2017 vagy Visual Studio 2015 hello alkalmazás toohello helyi fürt központi telepítése, és automatikusan kapcsolódhatnak hello hibakereső tooall példányok az alkalmazás.

1. Indítsa el a helyi fejlesztési fürtök hello utasításait követve [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started.md).
2. Nyomja le az **F5** , vagy kattintson a **Debug** > **Start Debugging**.
   
    ![Egy alkalmazást a hibakeresés elindításához][startdebugging]
3. Állítson be töréspontokat a kódot és hello alkalmazáson keresztül lépés hello parancsok kattintva **Debug** menü.
   
   > [!NOTE]
   > A Visual Studio csatol az alkalmazás tooall példányát. Kód most lépjen, amíg töréspontok előfordulhat, hogy beolvasása kattintson az egyidejű munkamenetek eredményezve több folyamat. Próbálja hello töréspontok letiltása után azok még találat, azáltal, hogy minden töréspont hello Szálazonosító függővé vagy diagnosztikai eseményeket használ.
   > 
   > 
4. Hello **a diagnosztikai** automatikusan megnyílik, megtekintheti a diagnosztikai valós időben.
   
    ![Valós idejű diagnosztikai eseményeinek megtekintése][diagnosticevents]
5. Hello is megnyithatja **a diagnosztikai** Cloud Explorer ablakban.  A **Service Fabric**, kattintson a jobb gombbal bármelyik csomópont, és válassza a **nézet adatfolyam-nyomkövetések**.
   
    ![Nyissa meg hello diagnosztikai események ablak][viewdiagnosticevents]
   
    Ha toofilter a nyomkövetések tooa adott szolgáltatással vagy alkalmazással, egyszerűen engedélyezze a folyamatos átviteli nyomkövetési adatokat az adott adott szolgáltatás vagy alkalmazás.
6. hello diagnosztikai események automatikusan generált hello látható **ServiceEventSource.cs** fájlt, és az alkalmazás kódjában nevezzük.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. Hello **a diagnosztikai** ablak támogatja a szűrést, a felfüggesztés és a valós idejű események ellenőrzése.  hello szűrő egy egyszerű karakterlánc-keresés hello eseményüzenet, beleértve annak tartalmát.
   
    ![Szűréséhez, szüneteltetése és folytatása és vizsgálja meg a valós idejű események][diagnosticeventsactions]
8. Hibakereső szolgáltatásának olyan, mint bármely más alkalmazásban. Visual Studio alkalmazással töréspontok általában könnyen hibakeresési állítja be. Annak ellenére, hogy a megbízható gyűjtemények replikálásához több csomópont, azok továbbra is valósítania az IEnumerable illesztőfelületet. Ez azt jelenti, hogy használható hello eredmények megtekintése a Visual Studio toosee hibakeresés során belül már tárolja. Egyszerűen állítson be egy töréspontot bárhol a kódban.
   
    ![Egy alkalmazást a hibakeresés elindításához][breakpoint]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>A távoli Service Fabric-alkalmazás hibakeresése
Ha a Service Fabric-alkalmazások az Azure Service Fabric-fürt futtatja, is tooremotely hibakeresése a Visual Studio-ről.

> [!NOTE]
> hello használatához [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Távoli hibakeresés szolgáltatásának fejlesztési/tesztelési és nem használja termelési környezetben futó alkalmazások hello hello gyakorolt miatt toobe tervezték.
> 
> 

1. Keresse meg a fürt tooyour **Cloud Explorer**, kattintson a jobb gombbal, és válassza a **hibakeresés engedélyezése**
   
    ![Távoli hibakeresésének engedélyezése][enableremotedebugging]
   
    Ez fogja, hogy a távoli hibakeresés a fürtcsomópontokon bővítmény hello hello folyamat indítsa, valamint hálózati konfiguráció szükséges.
2. Kattintson a jobb gombbal hello lévő fürtcsomópont **Cloud Explorer**, és válassza ki **csatolása hibakereső**
   
    ![Hibakereső csatlakoztatása][attachdebugger]
3. A hello **tooprocess csatolása** párbeszédpanelen válassza ki, szeretné, hogy toodebug, majd kattintson az hello folyamatot **csatolása**
   
    ![Folyamat kiválasztása][chooseprocess]
   
    hello neve hello folyamat azt szeretné, hogy tooattach, egyenlő hello a szolgáltatási projekt szerelvény neve alapján.
   
    hello hibakereső hello folyamatának futtatása tooall csomópontok kapcsolódni fog.
   
   * Ahol állapotmentes szolgáltatások hibakeresést hello esetben az összes olyan csomóponton hello szolgáltatás minden példányának részei hello hibakeresési munkamenetben.
   * Állapotalapú szolgáltatási hibakeresést, ha csak hello elsődleges másodpéldányát minden olyan partíció lesz aktív, és ezért kifogott hello hibakereső által. Hello elsődleges replika hello hibakeresési munkamenetben helyezi át, ha adott másodpéldány hello feldolgozását is hello hibakeresési munkamenet része.
   * Rendelés tooonly catch vonatkozó partíciók vagy egy megadott szolgáltatás példányának használhatja a feltételes töréspontok tooonly break egy adott partícióra vagy a példány.
     
     ![Feltételes töréspont][conditionalbreakpoint]
     
     > [!NOTE]
     > Jelenleg nem támogatjuk hibakeresés hello több példánya a Service Fabric-fürt azonos szolgáltatás végrehajtható fájljának nevét.
     > 
     > 
4. Ha az alkalmazás hibakeresése végzett, kattintson a jobb gombbal a fürt hello hello távoli hibakeresési bővítmény letiltása **Cloud Explorer** válassza **tiltsa le a hibakeresést.**
   
    ![Tiltsa le a távoli hibakeresés][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Adatfolyam-nyomkövetések távoli fürtcsomópontból
Hogy egyaránt közvetlenül egy távoli fürt csomópont tooVisual Studio képes toostream nyomkövetéseit. Ez a funkció lehetővé teszi toostream ETW nyomkövetési eseményeit, a Service Fabric fürtcsomóponton hozott létre.

> [!NOTE]
> A szolgáltatás használatához [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).
> Ez a funkció csak az Azure-ban futó fürtök támogatja.
> 
> 

<!-- -->
> [!WARNING]
> Adatfolyam-nyomkövetési szolgáltatásának fejlesztési/tesztelési és nem használja termelési környezetben futó alkalmazások hello hello gyakorolt miatt toobe tervezték.
> A termelési forgatókönyvek hagyatkozzon kizárólag az Azure Diagnostics használatával eseménye továbbítását.
> 
> 

1. Keresse meg a fürt tooyour **Cloud Explorer**, kattintson a jobb gombbal, és válassza a **adatfolyam-nyomkövetés engedélyezése**
   
    ![Távoli adatfolyam nyomkövetés engedélyezése][enablestreamingtraces]
   
    Ez lesz, ha engedélyezi a nyomkövetési adatokat bővítmény a fürtcsomópontokon streaming hello hello folyamat indítsa, valamint hálózati konfiguráció szükséges.
2. Bontsa ki a hello **csomópontok** elemében **Cloud Explorer**, kattintson a jobb gombbal hello csomópont szeretné, hogy toostream nyomkövetéseit, és válassza a **nézet adatfolyam-nyomkövetések**
   
    ![Távoli adatfolyam-nyomkövetések megtekintése][viewremotestreamingtraces]
   
    Azt szeretné, hogy toosee nyomkövetéseit csomópont ismételje meg a 2. Az egyes csomópontok adatfolyamokkal dedikált ablakban jelennek meg.
   
    A Service Fabric és a szolgáltatások által kibocsátott képes toosee hello nyomkövetések is. Ha azt szeretné, toofilter hello események tooonly megjelenítése egy adott alkalmazáshoz, egyszerűen az alkalmazástípus hello nevében hello hello szűrőben.
   
    ![Adatfolyam-nyomkövetések megtekintése][viewingstreamingtraces]
3. Miután a folyamatos átviteli nyomkövetési adatokat a fürtről, bármikor letilthatja távoli adatfolyam nyomkövetési adatok, kattintson a jobb gombbal a fürt hello **Cloud Explorer** válassza **adatfolyam-nyomkövetések letiltása**
   
    ![Tiltsa le a távoli adatfolyam nyomkövetések][disablestreamingtraces]

## <a name="next-steps"></a>Következő lépések
* [A Service Fabric szolgáltatás tesztelése](service-fabric-testability-overview.md).
* [A Service Fabric-alkalmazások, a Visual Studio kezelése](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
