---
title: "aaaPower BI-irányítópulton vehicle állapot-és befolyásoló tényezők szokásait - Azure |} Microsoft Docs"
description: "Használja a Cortana Intelligence toogain valós idejű és prediktív elemzések hello lehetőségeit a vehicle állapotát, és ki irányítja az szokásokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 0bd054d943387ecad7301236eebae22458173aba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle telemetriai analytics megoldás sablon Power BI-irányítópulton telepítési utasításokat
Ez **menü** Ez a forgatókönyv toohello fejezetek hivatkozásokat tartalmaz. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

hello Vehicle Telemetriai elemzési megoldások bővíthető hogyan car kereskedők, autó gyártók és biztosítási vállalatok kihasználhatják a Cortana Intelligence toogain valós idejű és prediktív áttekinthetik a vehicle állapotát, és ki irányítja az hello képességei szokások toodrive továbbfejlesztése hello területen felhasználói felület, a R & D és marketingkampányok. Ez a dokumentum konfigurálásának hello Power BI-jelentéseket és az irányítópult után hello megoldást már telepítették az előfizetésében részletes útmutatást tartalmaz. 

## <a name="prerequisites"></a>Előfeltételek
1. Hello telepítése [Telemetriai Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) megoldás  
2. [Telepítse a Microsoft Power BI Desktopba](http://www.microsoft.com/download/details.aspx?id=45331)
3. Egy [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/). Ha nem rendelkezik Azure-előfizetéssel, ingyenes Azure-előfizetés az első lépései
4. Microsoft Power BI-fiók

## <a name="cortana-intelligence-suite-components"></a>A Cortana Intelligence Suite összetevői
Hello Vehicle Telemetriai Analytics megoldássablonban részeként hello Cortana Intelligence szolgáltatások a következő előfizetéshez lettek telepítve.

* **Az Event Hubs** választásával dolgozhat fel a vehicle telemetriai események több millió az Azure.
* **Stream Analytics** való vehicle egészségügyi valós idejű elemzése és továbbra is fennáll, hogy az adatok a hosszú távú tároló gazdagabb kötegelt elemzéséhez.
* **Gépi tanulás** közüli valós idejű és kötegfeldolgozási toogain prediktív elemzések.
* **HDInsight** léptékű kihasználhatók tootransform adat
* **Adat-előállító** kezeli az orchestration, erőforrás-kezelés ütemezése és hello kötegelt folyamat figyelését.

**A Power BI** Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések. 

hello megoldás két különböző adatforrásból használ: **vehicle jelek és diagnosztikai adatkészlet szimulált** és **vehicle katalógus**.

A vehicle telematika szimulátor Ez a megoldás részét. Diagnosztikai adatok bocsát ki, és időben hello vehicle, valamint adott vezetői mintának megfelelő toohello állapotát jelzi. 

hello Vehicle katalógus hivatkozás adatkészletet tartalmazó VIN toomodel társítás

## <a name="power-bi-dashboard-preparation"></a>A Power BI-irányítópulton előkészítése
### <a name="setup-power-bi-real-time-dashboard"></a>A telepítő a Power BI-valós idejű irányítópulton

**Indítsa el a hello valós idejű irányítópulton alkalmazás** hello központi telepítés befejezése után hello manuális művelet utasításokat kövesse

* Töltse le a valós idejű irányítópulton alkalmazás RealtimeDashboardApp.zip, és bontsa ki azt.
*  Hello tömörítetlen mappába nyissa meg az alkalmazás konfigurációs fájl "RealtimeDashboardApp.exe.config", a név felülírandó appSettings hello értékek hello manuális művelet utasításokat, és mentse a módosításokat az Eventhub, a Blob Storage és a gépi tanulás összetevő szolgáltatás-kapcsolatokhoz.
* Futtassa az alkalmazást RealtimeDashboardApp.exe. A bejelentkezési ablak előugró ablak, érvényes Power bi hitelesítő adatok megadása és hello kattintson **elfogadás** gombra. Majd hello app toorun elindul.

   ![Bejelentkezési tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![A Power BI-irányítópulton engedélyek](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Bejelentkezési tooPowerBI webhelyét, és hozza létre a valós idejű irányítópulton.

Most készen áll a tooconfigure hello Power BI irányítópultot, amelynek valós idejű gazdag képi megjelenítések toogain és prediktív elemzések a vehicle állapotát, és ki irányítja a szokásokat. Körülbelül 45 percig tooan óra toocreate összes hello jelentéseket, és konfigurálja a hello irányítópult vesz igénybe. 

### <a name="configure-power-bi-reports"></a>A Power BI-jelentések konfigurálása
hello valós idejű jelentéseket és hello irányítópult eltarthat 30-45 percig toocomplete kapcsolatban. Keresse meg a túl[http://powerbi.com](http://powerbi.com) és a bejelentkezés.

![Bejelentkezési tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Új adatkészlet jön létre a Power bi-ban. Kattintson a hello **ConnectedCarsRealtime** adatkészlet.

![Be van jelölve csatlakoztatott autók valós idejű adatkészlet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Mentés hello üres jelentés használatával **Ctrl + s**.

![Üres jelentés mentése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Adja meg a jelentés neve *Vehicle Telemetriai elemzés valós idejű - jelentések*.

![Adja meg a jelentés neve.](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Valós idejű jelentéseket
Ez a megoldás három valós idejű jelentés áll:

1. A művelet járművekről gyűjtött
2. Karbantartási igénylő járművekről gyűjtött
3. Járművekről gyűjtött egészségügyi statisztikák

Választhat tooconfigure összes hello három valós idejű jelentéseket vagy leállítása után fel, és folytassa a toohello következő szakaszában hello kötegelt jelentések konfigurálása. Javasoljuk, hogy minden toocreate hello három jelentések toovisualize hello teljes insights hello hello megoldás a valós idejű elérési út.  

### <a name="1-vehicles-in-operation"></a>1. A művelet járművekről gyűjtött
Kattintson duplán a **1. oldal** és adjon neki túl "Műveletben járművekről gyűjtött"  
    ![Autók - csatlakoztatott járművekről gyűjtött műveletben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Válassza ki **vin** mezőjét a **mezők** , és válassza a típusú képi megjelenítés **"Kártyás"**.  

Kártya képi megjelenítés létrehozása ábrán látható módon.  
    ![Csatlakoztatott autók - válassza vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Válassza ki **Város** és **vin** mezők. Módosíthatja a képi megjelenítés túl**"Térkép"**. A csomóponthúzási **vin** az értékek területére. A csomóponthúzási **Város** mezők túl**jelmagyarázat** területen.   
    ![Csatlakoztatott autók - kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Válassza ki **formátum** szakasz **képi megjelenítések**, kattintson a **cím** , és módosítsa a hello **szöveg** túl**"járművekről gyűjtött a művelet városonként"**.  
    ![Autók - csatlakoztatott járművekről gyűjtött városonként műveletben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Végső képi megjelenítés keres az ábrán látható módon.    
    ![Csatlakoztatott autók - végső képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Válassza ki **Város** és **vin**, módosítsa a képi megjelenítés típusát túl**fürtözött oszlopdiagram**. Győződjön meg arról **Város** mezőjét **tengely terület** és **vin** a **érték-terület**  

Rendezés a diagram által **"Vin száma"**  
    ![Csatlakoztatott autók - vin száma](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Módosítsa a diagram **cím** túl**"Műveletben városonként járművekről gyűjtött"**  

Hello kattintson **formátum** területen, majd válassza ki **adatok színek**, kattintson a hello **"A"** túl**összes megjelenítése**  
    ![Csatlakoztatott autók - színek adatok megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Egyes város hello színének módosítása a szín ikonra kattintva.  
    ![Csatlakoztatott autók - módosítás színek](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Válassza ki **fürtözött oszlopdiagram** képi megjelenítéseket, a képi megjelenítés húzza **Város** mezőjét **tengely** területen **modell** a **Jelmagyarázat** terület és **vin** a **érték** területen.  
    ![Csatlakoztatott autók - csoportosított oszlopdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Csatlakoztatott autók - megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Ezen a lapon minden képi megjelenítés átrendezését, az ábrán látható módon.  
    ![Csatlakoztatott autók - képi megjelenítések](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Sikeresen konfigurálta a hello "Műveletben járművekről gyűjtött" valós idejű jelentés. Toocreate hello következő valós idejű jelentés folytassa vagy állítsa le a ide, és konfigurálja a hello irányítópult. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Karbantartási igénylő járművekről gyűjtött
Kattintson a ![Hozzáadás](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd új jelentést, nevezze át túl**"Járművekről gyűjtött igénylő karbantartási"**

![Csatlakoztatott autók - karbantartási igénylő járművekről gyűjtött](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Válassza ki **vin** mezőben, majd a képi megjelenítés típusának módosítása túl**kártya**.  
    ![Csatlakoztatott autók - Vin kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Tudunk hello adatkészletben levő "MaintenanceLabel" nevű mező. Ez a mező is rendelkezik értéke "0" vagy "1"." Szerint hello Azure Machine Learning modell megoldás részeként kiosztása és hello valós idejű elérési integrálva van állítva. hello "1" érték a vehicle igényel karbantartást. 

tooadd egy **oldal szintjének** karbantartási megköveteli, az járművekről gyűjtött adatok jelennek meg a szűrő: 

1. Húzza hello **"MaintenanceLabel"** mezőjét a **szint Lapszűrők**.  
   ![Csatlakoztatott autók - oldal szintjének szűrők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Kattintson a **alapszintű szűrési** menü MaintenanceLabel lap szint szűrő alsó részén található.  
   ![Csatlakoztatott autók - alapvető szűrése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Állítsa be az szűrő értékét túl**"1"**    
   ![Csatlakoztatott autók - szűrő értéke](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Válassza ki **fürtözött oszlopdiagram** a képi megjelenítések  
![Csatlakoztatott autók - Vind kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Csatlakoztatott autók - csoportosított oszlopdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Húzza a mező **modell** történő **tengely** területen **Vin** túl**érték** területen. Ezután szűrje a képi megjelenítés által **vin száma**.  Módosítsa a diagram **cím** túl**"Járművekről gyűjtött karbantartási igénylő modell"**  

Húzza **vin** a mezők **szín telítettségét** jelen **mezők** ![mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) szakasza **aképimegjelenítés**lap  
![Csatlakoztatott autók - szín telítettségét](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Változás **adatok színek** a képi megjelenítéseket **formátum** szakasz  
Módosítsa a minimális színhez: **F2C812**  
A maximális színhez módosítása: **FF6300**  
![Autók - szín módosítások csatlakoztatva](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Csatlakoztatott autók - új színeit](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Válassza ki **fürtözött oszlopdiagram** a képi megjelenítések, húzza **vin** mezőjét a **érték** terület, húzza **Város** mezőjét a **Tengely** területen. Rendezés a diagram által **"Vin száma"**. Módosítsa a diagram **cím** túl**"Járművekről gyűjtött városonként karbantartási megkövetelése"**   
![Csatlakoztatott autók - karbantartási városonként igénylő járművekről gyűjtött](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Válassza ki **többsoros kártya** képi megjelenítéseket, a képi megjelenítés húzza **modell** és **vin** történő hello **mezők** területen.  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Az összes hello képi megjelenítés átrendezése, hello végső jelentés a következőképpen néz ki:  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Sikeresen konfigurálta a hello "Járművekről gyűjtött igénylő karbantartás" valós idejű jelentést. Toocreate hello következő valós idejű jelentés folytassa vagy állítsa le a ide, és konfigurálja a hello irányítópult. 

### <a name="3-vehicles-health-statistics"></a>3. Járművekről gyűjtött egészségügyi statisztikák
Kattintson a ![Hozzáadás](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd új jelentést, nevezze át túl**"Járművekről gyűjtött állapotstatisztika"**  

Válassza ki **mérőműszer** képi megjelenítés a képi megjelenítések, majd húzza hello **sebesség** mezőjét a **, minimális érték, maximális érték** területeket.  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Módosítsa a hello alapértelmezett összesítése **sebesség** a **terület érték** túl**átlagos** 

Módosítsa a hello alapértelmezett összesítése **sebesség** a **minimális terület** túl**minimális**

Módosítsa a hello alapértelmezett összesítése **sebesség** a **maximális terület** túl**maximális**

![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Nevezze át a hello **mérőműszer cím** túl**"Átlagos sebesség"** 

![Csatlakoztatott autók - mérőműszer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Kattintson a hello üres területre tooadd új képi megjelenítést.  

Hasonlóképpen adja hozzá a **mérőműszer** a **motor olaj átlagos**, **üzemanyag átlagos**, és **átlagos motor mérsékelt**.  

Hello alapértelmezett összesítés minden mérőműszer megfelelően a fent ismertetett mezők módosítása **"Átlagos sebesség"** fel tudja mérni.

![Csatlakoztatott autók - mérőműszer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Kattintson a hello üres területre tooadd új képi megjelenítést.

Válassza ki **sor- és fürtözött oszlopdiagram** a képi megjelenítések, majd húzza **Város** mezőjét a **megosztott tengely**, húzza **sebesség**, **tirepressure és engineoil mezők** történő **oszlopok értékeinek** területen túl módosítsa az összesítési típusát**átlagos**. 

Húzza hello **engineTemperature** mezőjét a **sor értékek** területen hello összesítéstípusa túl módosítása**átlagos**. 

![Csatlakoztatott autók - képi megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Változás hello diagram **cím** túl**"Átlagos sebességét, kulcsszava nyomás, motor olaj- és motor"**.  

![Csatlakoztatott autók - képi megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Kattintson a hello üres területre tooadd új képi megjelenítést.

Válassza ki **Treemap** képi megjelenítéseket, a képi megjelenítés húzza hello **modell** mezőjét a hello **csoport** területen, és húzza hello mező  **MaintenanceProbability** történő hello **értékek** területen.

Változás hello diagram **cím** túl**"Vehicle modellek karbantartási igénylő"**.

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Kattintson a hello üres területre tooadd új képi megjelenítést.

Válassza ki **100 százalékos halmozott sáv-** a képi megjelenítés, húzza a hello **Város** mezőjét a hello **tengely** területen, és húzza hello **MaintenanceProbability**, **RecallProbability** hello mezők **érték** területen.

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Kattintson a **formátum**, jelölje be **adatok színek**, és a set hello **MaintenanceProbability** toohello érték szín **"F2C80F"**.

Változás hello **cím** hello a diagram túl**"Probability a Vehicle karbantartási & visszahívása által város"**.

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Kattintson a hello üres területre tooadd új képi megjelenítést.

Válassza ki **területdiagram** a képi megjelenítés a képi megjelenítések, húzza a hello **modell** mezőjét a hello **tengely** területen, és húzza hello **engineOil, tirepressure, sebesség és a MaintenanceProbability** hello mezők **értékek** területen. Az összesítési típusát túl módosítása**"Átlagos"**. 

![Csatlakoztatott autók - összesítési típusának módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Módosítás hello hello diagram túl**"Átlagos motor olaj, nyomás, a sebesség és a karbantartás valószínűség megunja modell"**.

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Kattintson a hello üres területre tooadd új képi megjelenítést:

1. Válassza ki **pontdiagram diagram** képi megjelenítések a képi megjelenítés.
2. Húzza hello **modell** mezőjét a hello **részletek** és **jelmagyarázat** területen.
3. Húzza hello **üzemanyag** mezőjét a hello **x tengely** területen hello összesítés túl módosítása**átlagos**.
4. A csomóponthúzási **engineTemparature** be **y tengely terület**, hello összesítés túl módosítása**átlagos**
5. Húzza hello **vin** mezőjét a hello **mérete** területen.

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Változás hello diagram **cím** túl**"Üzemanyag átlagok, motor hőmérséklet modell"**.

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

hello végső jelentést fog megjelenni, alább látható módon.

![Csatlakoztatott autók végleges jelentés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-hello-reports-toohello-real-time-dashboard"></a>PIN-kód képi megjelenítések hello jelentések toohello valós idejű irányítópulton
Hozzon létre egy üres irányítópult a következő tooDashboards hello plusz ikonra kattintva. "Vehicle Telemetriai elemzések irányítópultján" is neve

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

A jelentések toohello irányítópulton fent hello PIN hello képi megjelenítés. 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

hello irányítópult kell hasonlítania Ha hello három jelentések jönnek létre, és a megfelelő képi megjelenítések hello rögzített toohello irányítópult. Ha nem hozott létre minden hello jelentést, az irányítópult sikerült jelenik meg 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gratulálunk! Sikeresen létrehozta a hello valós idejű irányítópulton. Tooexecute CarEventGenerator.exe és RealtimeDashboardApp.exe továbbra is, mivel látni működés közbeni frissítések hello irányítópulton. Too15 körülbelül 10 percig toocomplete hello a következő lépéseket kell vennie.

## <a name="setup-power-bi-batch-processing-dashboard"></a>A telepítő a Power BI kötegelt feldolgozásra irányítópult
> [!NOTE]
> A hello end tooend kötegfeldolgozási feldolgozási sor toofinish végrehajtása (a hello hello központi telepítés sikeres befejezése) körülbelül két órát vesz igénybe, és egy év alatt érkezett létrehozott adatokat feldolgozni. Várja meg, hello feldolgozása toofinish hello következő lépések végrehajtása előtt. 
> 
> 

**Hello Power BI designer-fájl letöltése**

* Egy előre konfigurált Power BI designer fájl tartalmazzák a hello telepítési manuális művelet utasításokat
* Keresse meg a 2. Telepítő Power bi kötegelt feldolgozásra irányítópult letöltheti a Power bi sablon hello itt nevű kötegfeldolgozási irányítópult **ConnectedCarsPbiReport.pbix**.
* Mentse helyileg

**A Power BI-jelentések konfigurálása**

* Nyissa meg hello Tervező fájl "**ConnectedCarsPbiReport.pbix**" Power BI Desktop használatával. Ha nem már rendelkezik, telepíti a Power BI Desktop hello [Power BI Desktop telepítés](http://www.microsoft.com/download/details.aspx?id=45331). 
* Kattintson a hello **szerkesztése lekérdezések**.

![A Power BI lekérdezés szerkesztése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Kattintson duplán a hello **forrás**.

![A Power BI-forrás beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Frissítse a kiszolgáló kapcsolódási karakterlánc hello Azure SQL-kiszolgálót, amely hello központi telepítésének részeként lett kiépítve.  Hello manuális művelet utasításokat a hely 

    4. Azure SQL Database
    
    * Server: somethingsrv.database.windows.net
    * Adatbázis: connectedcar
    * Felhasználónév: felhasználónév
    * Jelszó: Kezelheti az SQL server jelszó Azure-portálon

* Hagyja **adatbázis** , *connectedcar*.

![A Power BI-adatbázis beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Kattintson az **OK** gombra.
* Látni fogja **Windows hitelesítő adatok** lapon alapértelmezés szerint kiválasztva, majd azt túl**adatbázis-hitelesítő adatok** kattintva **adatbázis** jobb fülre.
* Adja meg a hello **felhasználónév** és **jelszó** , az Azure SQL Database, a telepítés során megadott.

![Adja meg az adatbázis hitelesítő adatai](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Kattintson a **csatlakozás**
* Ismételje meg minden egyes hello három fennmaradó lekérdezések jobb oldali ablaktáblában található hello a fenti lépéseket, és frissítse a hello az adatforrás kapcsolódási adatait.
* Kattintson a **zárja be, és betölti**. A Power BI Desktop-fájl adatkészletek csatlakoztatott tooSQL Azure adatbázistáblák.
* **Bezárás** Power BI Desktop-fájlba.

![Zárja be a Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Kattintson a **mentése** toosave hello módosítások gombra. 

Ezzel beállította a megfelelő toohello kötegelt feldolgozásra elérési hello megoldásban minden hello jelentést. 

## <a name="upload-toopowerbicom"></a>Töltse fel a túl*powerbi.com webhelyre*
1. Keresse meg a Power BI webportál toohello http://powerbi.com és a bejelentkezés.
2. Kattintson a **adatok beolvasása**  
3. Hello Power BI Desktop-fájl feltöltése.  
4. tooupload, kattintson a **adatok beolvasása -> fájlok Get -> helyi fájl**  
5. Keresse meg a toohello **"**ConnectedCarsPbiReport.pbix**"**  
6. Hello fájl a feltöltést követően nyílik hátsó tooyour Power BI munkaterület fogja.  

A DataSet adatkészlet, jelentést és egy üres irányítópult meg hozható létre.  

PIN-kód diagramokat tooa új irányítópult nevű **Vehicle Telemetriai elemzések irányítópultján** a **Power BI**. Kattintson a fenti létrehozott hello üres irányítópult, és navigáljon a toohello **jelentések** szakaszban kattintson hello újonnan feltöltött jelentés.  

![Vehicle Telemetriai Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Megjegyzés: hello jelentés hat lapok rendelkezik:**  
1. oldal: Vehicle sűrűség  
2. lap: Valós idejű vehicle állapota  
3. oldal: Agresszív vezérelt járművekről gyűjtött   
4. lap: Visszaírt járművekről gyűjtött  
Lap 5: Hatékony vezérelt járművekről gyűjtött üzemanyag  
6. lap: Contoso-embléma  

![Csatlakoztatott autók Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**A lap 3**, PIN-kód hello következő:  

1. VIN száma  
   ![Csatlakoztatott autók Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Járművekről gyűjtött agresszív vezérlik modell – diagram vízesés egyes szintjei  
   ![Vehicle Telemetria - PIN-kód diagramok 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Az oldal 5**, PIN-kód hello következő: 

1. Vin száma    
   ![Vehicle Telemetria - 5 PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Üzemanyag-a hatékony járművekről gyűjtött modell: csoportosított oszlopdiagram  
   ![Vehicle Telemetria - PIN-kód diagramok 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**A lap 4**, PIN-kód hello következő:  

1. Vin száma  
   ![Vehicle Telemetria - PIN-kód diagramok 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Visszaírt járművekről gyűjtött városonként, a modell: Treemap  
   ![Vehicle Telemetria - PIN-kód diagramok 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**A lap 6**, PIN-kód hello következő:  

1. Contoso motorok embléma  
   ![Vehicle Telemetria - PIN-kód diagramok 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Hello irányítópult rendezése**  

1. Keresse meg a toohello irányítópult
2. Minden egyes diagram mutat, és nevezze át hello elnevezési alapján hello teljes irányítópult az alábbi képen találhatók. Például az alábbi hello irányítópult toolook körül hello diagramok is helyezheti.  
   ![Vehicle Telemetria - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetriai Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Ha a jelen dokumentum összes hello-jelentéseket készített, hello utolsó befejezett irányítópult kell hasonlítania. ábra a következő hello. 

![Vehicle Telemetria - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gratulálunk! Sikeresen létrehozott hello jelentések és a valós idejű, a prediktív irányítópult toogain hello és kötegelt áttekinthetik a vehicle állapotát, és ki irányítja a szokásokat.  
