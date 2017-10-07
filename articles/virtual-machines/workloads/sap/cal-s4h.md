---
title: "SAP S/4HANA vagy egy Azure virtuális gépen BW/4HANA aaaDeploy |} Microsoft Docs"
description: "SAP S/4HANA vagy BW/4HANA egy Azure virtuális Gépen lévő telepítése"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 7e57f7daa7667b7c7dbcb86f6892a54e4295b74c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>SAP S/4HANA vagy BW/4HANA Azure telepítéséhez
Ez a cikk ismerteti, hogyan toodeploy S/4HANA Azure használatával hello SAP felhő készülék könyvtára (SAP CAL) 3.0. toodeploy más SAP HANA-alapú megoldások, például a BW/4HANA, hajtsa végre a hello ugyanazokat a lépéseket.

> [!NOTE]
A SAP CAL hello kapcsolatos további információkért lásd a toohello [felhő készülék teszteléshez](https://cal.sap.com/) webhelyet. SAP is rendelkezik kapcsolatos hello blog [SAP felhő készülék könyvtár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).

> [!NOTE]
2017. május 29. hello Azure Resource Manager üzembe helyezési modellben használatával, továbbá toohello kevésbé előnyben részesített klasszikus üzembe helyezési modell toodeploy hello SAP-CAL. Azt javasoljuk, hogy hello új Resource Manager telepítési modellt használja, és figyelmen kívül hagyhatja a hello klasszikus üzembe helyezési modellel.

## <a name="step-by-step-process-toodeploy-hello-solution"></a>Részletes folyamat toodeploy hello megoldás

a következő feladatütemezési a képernyőfelvételek hello bemutatja, hogyan toodeploy S/4HANA Azure használatával hello SAP-CAL. hello folyamat hello működik ugyanúgy más megoldások, például a BW/4HANA.

Hello **megoldások** lapon látható néhány hello SAP HANA-CAL-alapú megoldások érhető el az Azure-on. **SAP S/4HANA 1610 FPS01, Fully-Activated készülék** hello középső sor:

![SAP CAL megoldások](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-hello-sap-cal"></a>Hozzon létre egy fiókot az SAP-CAL hello
1. az SAP-CAL toohello a hello toosign első alkalommal használja az SAP-felhasználó vagy más SAP regisztrált felhasználó. Majd adja meg egy SAP CAL hello SAP CAL toodeploy készülékek Azure által használt fiókot. Hello fiók definícióban kell:

    a. (Erőforrás-kezelő vagy klasszikus) Azure hello telepítési modell kiválasztása.

    b. Adja meg az Azure-előfizetéshez. Egy SAP naptár fiókja csak tooone előfizetés rendelhetők hozzá. Ha több előfizetése van szüksége, toocreate egy másik SAP CAL-fiók szükséges.

    c. Hello SAP CAL engedély toodeploy biztosítják azokat az Azure-előfizetéshez.

    > [!NOTE]
    hello következő lépésekből megtudhatja, hogyan toocreate egy SAP CAL Resource Manager üzembe helyezések fiókot. Ha már rendelkezik egy SAP naptár fiókja, amely csatolt toohello klasszikus üzembe helyezési modellel, hogy *kell* toofollow ezeket a lépéseket toocreate egy új SAP naptár fiókja. hello új SAP naptár fiókja toodeploy hello Resource Manager modellben van szüksége.

2. Hozzon létre egy új SAP naptár fiókja. Hello **fiókok** lapon látható három lehetősége az Azure-bA: 

    a. **A Microsoft Azure (klasszikus)** hello klasszikus üzembe helyezési modell, és már nem elsődleges.

    b. **A Microsoft Azure** hello új Resource Manager üzembe helyezési modellben van.

    c. **Windows Azure 21Vianet által működtetett** hello klasszikus üzembe helyezési modellt használ Kínában beállítás.

    toodeploy hello Resource Manager modellben, válassza ki **Microsoft Azure**.

    ![SAP CAL fiókadatok](./media/cal-s4h/s4h-pic-2a.png)

3. Adja meg a hello Azure **előfizetés-azonosító** , amely hello Azure-portálon található.

   ![SAP CAL fiókok](./media/cal-s4h/s4h-pic3c.png)

4. tooauthorize hello SAP CAL toodeploy hello Azure-előfizetés az Ön által definiált, kattintson a **engedélyezés**. a következő lap hello hello böngészőlapon jelenik meg:

   ![Az Internet Explorer cloud services – bejelentkezési](./media/cal-s4h/s4h-pic4c.png)

5. Ha egynél több felhasználó szerepel, válassza ki, amely a kijelölt Azure-előfizetés hello csatolt toobe hello coadministrator hello Microsoft-fiók. a következő lap hello hello böngészőlapon jelenik meg:

   ![Az Internet Explorer felhőalapú szolgáltatások megerősítése](./media/cal-s4h/s4h-pic5a.png)

6. Kattintson a **elfogadása**. Sikeres hello engedélyezési esetén hello SAP CAL fiók definition újra jeleníti meg. Rövid idő múlva üzenet jelzi, hogy hello engedélyezési folyamat sikeres volt-e.

7. tooassign hello az újonnan létrehozott SAP CAL tooyour felhasználója, adja meg a **Felhasználóazonosító** a szövegmezőben a megfelelő hello hello, és kattintson a **Hozzáadás**.

   ![Fiók toouser társítás](./media/cal-s4h/s4h-pic8a.png)

8. tooassociate hello felhasználói fiókját toosign használhatja az SAP-CAL, toohello kattintson **felülvizsgálati**. 
 
9. toocreate hello társítását a felhasználók és az újonnan létrehozott hello SAP naptár fiókja, kattintson a **létrehozása**.

   ![Felhasználói tooSAP CAL fiók társítása](./media/cal-s4h/s4h-pic9b.png)

Sikeresen létrehozott egy SAP naptár fiókja, amely képes:

- Hello Resource Manager telepítési modellt használja.
- SAP rendszerek üzembe helyezés az Azure-előfizetéshez.

Most elindíthatja toodeploy S/4HANA az Azure-ban, a felhasználó előfizetéssé.

> [!NOTE]
A folytatás előtt megállapításához, hogy rendelkezik-e az Azure core kvóták Azure H sorozatú virtuális gépekhez. Hello pillanatban SAP CAL hello Azure virtuális gépek H-sorozat toodeploy használ egyes hello SAP HANA-alapú megoldások. Az Azure-előfizetéshez esetleg nincs bármely H-sorozat core kvóták a H-adatsorozathoz. Ha igen, szükség lehet a toocontact az Azure támogatási tooget legalább 16 H-sorozat magszámra vonatkozó kvóta.

> [!NOTE]
SAP CAL hello Azure megoldást telepítésekor előfordulhat, hogy csak egy Azure-régió, választhat. az Azure-régiók eltérő hello toodeploy egy által javasolt hello SAP-CAL, toopurchase egy SAP CAL előfizetés szükséges. Is szükség lehet tooopen SAP toohave üzenet a naptár fiók engedélyezett toodeliver az Azure-régiók eltérő hello eredetileg javasolt néhányat a meglévők közül.

### <a name="deploy-a-solution"></a>A megoldás üzembe helyezéséhez

Helyezzünk üzembe egy megoldást hello **megoldások** hello SAP CAL oldalán. hello SAP CAL két sorozatok toodeploy rendelkezik:

- Egy alapszintű feladatütemezési egy lap toodefine hello rendszer toobe telepített használó
- Egy speciális részeként, amely lehetővé teszi az egyes lehetőségek a Virtuálisgép-méretek 

Itt hello alapvető elérési út toodeployment bemutatjuk.

1. A hello **fiókadatok** lapon kell:

    a. Válassza ki a SAP CAL fiókot. (Használja a társított toodeploy hello erőforrás-kezelő telepítési modellel rendelkező fiókkal.)

    b. Adjon meg egy példány **neve**.

    c. Válassza ki az Azure **régió**. hello SAP CAL javasol egy régiót. Ha egy másik Azure-régióban van szüksége, és nem kell egy SAP naptár-előfizetés, SAP tooorder Ügyféllicenccel előfizetés kell.

    d. Adjon meg egy fő **jelszó** hello megoldás nyolc vagy kilenc karaktereket. hello jelszót hello rendszergazdák hello különböző összetevőt használja.

   ![SAP CAL Basic mód: Példány létrehozása](./media/cal-s4h/s4h-pic10a.png)

2. Kattintson a **létrehozása**, és a hello megjelenő, kattintson a **OK**.

   ![SAP CAL támogatott Virtuálisgép-méretek](./media/cal-s4h/s4h-pic10b.png)

3. A hello **titkos kulcs** párbeszédpanel, kattintson a **tároló** toostore hello lévő titkos kulcs hello SAP-CAL. jelszavas védelem toouse hello titkos kulcs, kattintson a **letöltése**. 

   ![SAP CAL titkos kulcs](./media/cal-s4h/s4h-pic10c.png)

4. Olvasási hello SAP CAL **figyelmeztetés** üzenet, és kattintson a **OK**.

   ![SAP CAL figyelmeztetés](./media/cal-s4h/s4h-pic10d.png)

    Most hello központi telepítés akkor történik meg. Némi várakozás után attól függően, hogy hello méretét és összetettségét hello megoldás (hello SAP CAL biztosít becsült érték), hello állapota jelenik meg az aktív és használatra kész.

5. toofind hello virtuális gépek gyűjtik hello más kapcsolódó erőforrás egy erőforráscsoportban, nyissa meg az Azure portál toohello: 

   ![SAP CAL objektumok hello új portál telepítése](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. Hello SAP CAL-portál hello állapot jelenik meg **aktív**. tooconnect toohello megoldás, kattintson a **Connect**. Különböző beállítások tooconnect toohello különböző összetevői ebben a megoldásban belül vannak telepítve.

   ![SAP CAL-példányok](./media/cal-s4h/active_solution.png)

7. Használata előtt hello beállítások tooconnect telepített toohello rendszert, kattintson a **Getting Started Guide**. 

   ![Csatlakozás toohello példány](./media/cal-s4h/connect_to_solution.png)

    hello dokumentáció nevek felhasználók hello hello csatlakozási módszer. hello jelszava azoknak a felhasználóknak be van állítva az Ön által definiált toohello fő jelszó hello hello telepítési folyamat elején. Hello dokumentációban, több működési másoknak szerepel a listában a jelszavukat, amelynek segítségével toosign toohello a telepített rendszer. 

    Például használatakor hello SAP grafikus felhasználói felület, amely előre telepítve van a Windows távoli asztali gépen hello hello S/4 rendszer nézhet ki:

   ![A hello SM50 előtelepített SAP grafikus felhasználói felületen](./media/cal-s4h/gui_sm50.png)

    Vagy ha hello DBACockpit használatához hello példány nézhet ki:

   ![A hello DBACockpit SAP grafikus felhasználói Felülettel SM50](./media/cal-s4h/dbacockpit.png)

Néhány órán belül egy megfelelő SAP S/4 készülék a rendszer az Azure-ban.

Ha egy SAP naptár-előfizetés vásárolta, SAP teljes mértékben támogatja az SAP CAL hello környezeteit Azure. hello támogatási várólista BC-VCM-CAL.







