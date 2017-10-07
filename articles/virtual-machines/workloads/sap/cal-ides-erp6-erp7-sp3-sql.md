---
title: aaaDeploy IDES EHP7 SP3 SAP SAP ERP 6.0 az Azure-on |} Microsoft Docs
description: "SAP ERP 6.0 Azure SAP IDES EHP7 SP3 telepítése"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 26d88c7b48a91d35602464c4f89ca7a30502c4b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a>SAP ERP 6.0 Azure SAP IDES EHP7 SP3 telepítése
Ez a cikk ismerteti, hogyan toodeploy egy SAP hello Windows operációs rendszerrel és az SQL Server fut az Azure-on keresztül hello SAP felhő készülék könyvtára (SAP CAL) 3.0 IDES rendszer. hello képernyőképeket hello részletes folyamat megjelenítése. egy másik megoldás toodeploy kövesse hello ugyanazokat a lépéseket.

az SAP-CAL, nyissa meg toohello hello toostart [felhő készülék teszteléshez](https://cal.sap.com/) webhelyet. SAP is rendelkezik egy blog hello kapcsolatos új [SAP felhő készülék könyvtár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 

> [!NOTE]
2017. május 29. hello Azure Resource Manager üzembe helyezési modellben használatával, továbbá toohello kevésbé előnyben részesített klasszikus üzembe helyezési modell toodeploy hello SAP-CAL. Azt javasoljuk, hogy hello új Resource Manager telepítési modellt használja, és figyelmen kívül hagyhatja a hello klasszikus üzembe helyezési modellel.

Ha már létrehozott egy SAP naptár fiókja hello Klasszikus modell használó *toocreate egy másik SAP CAL-fiókra lesz szüksége*. Ezt a fiókot kell tooexclusively központi telepítése az Azure Resource Manager modellt hello használatával.

Miután bejelentkezik toohello SAP-CAL, hello első oldal általában részletes útmutatást toohello **megoldások** lap. hello SAP CAL kínált hello megoldások folyamatosan növekvő, így tooscroll túl kicsit toofind hello kívánt megoldáshoz szükséges lehet. a kijelölt hello SAP IDES Windows-alapú megoldással, kizárólag az Azure hello telepítési folyamatának mutatja be:

![SAP CAL megoldások](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-hello-sap-cal"></a>Hozzon létre egy fiókot az SAP-CAL hello
1. az SAP-CAL toohello a hello toosign első alkalommal használja az SAP-felhasználó vagy más SAP regisztrált felhasználó. Majd adja meg egy SAP CAL hello SAP CAL toodeploy készülékek Azure által használt fiókot. Hello fiók definícióban kell:

    a. (Erőforrás-kezelő vagy klasszikus) Azure hello telepítési modell kiválasztása.

    b. Adja meg az Azure-előfizetéshez. Egy SAP naptár fiókja csak tooone előfizetés rendelhetők hozzá. Ha több előfizetése van szüksége, toocreate egy másik SAP CAL-fiók szükséges.
    
    c. Hello SAP CAL engedély toodeploy biztosítják azokat az Azure-előfizetéshez.

    > [!NOTE]
    hello következő lépésekből megtudhatja, hogyan toocreate egy SAP CAL Resource Manager üzembe helyezések fiókot. Ha már rendelkezik egy SAP naptár fiókja, amely csatolt toohello klasszikus üzembe helyezési modellel, hogy *kell* toofollow ezeket a lépéseket toocreate egy új SAP naptár fiókja. hello új SAP naptár fiókja toodeploy hello Resource Manager modellben van szüksége.

2. egy új SAP-CAL toocreate fiók, hello **fiókok** lapon látható két választási lehetőség az Azure-bA: 

    a. **A Microsoft Azure (klasszikus)** hello klasszikus üzembe helyezési modell, és már nem elsődleges.

    b. **A Microsoft Azure** hello új Resource Manager üzembe helyezési modellben van.

    ![SAP CAL fiókok](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    toodeploy hello Resource Manager modellben, válassza ki **Microsoft Azure**.

    ![SAP CAL fiókok](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. Adja meg a hello Azure **előfizetés-azonosító** , amely hello Azure-portálon található. 

    ![SAP CAL előfizetés-azonosító](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. tooauthorize hello SAP CAL toodeploy hello Azure-előfizetés az Ön által definiált, kattintson a **engedélyezés**. a következő lap hello hello böngészőlapon jelenik meg:

    ![Az Internet Explorer cloud services – bejelentkezési](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. Ha egynél több felhasználó szerepel, válassza ki, amely a kijelölt Azure-előfizetés hello csatolt toobe hello coadministrator hello Microsoft-fiók. a következő lap hello hello böngészőlapon jelenik meg:

    ![Az Internet Explorer felhőalapú szolgáltatások megerősítése](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. Kattintson a **elfogadása**. Sikeres hello engedélyezési esetén hello SAP CAL fiók definition újra jeleníti meg. Rövid idő múlva üzenet jelzi, hogy hello engedélyezési folyamat sikeres volt-e.

7. tooassign hello az újonnan létrehozott SAP CAL tooyour felhasználója, adja meg a **Felhasználóazonosító** a szövegmezőben a megfelelő hello hello, és kattintson a **Hozzáadás**. 

    ![Fiók toouser társítás](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. tooassociate hello felhasználói fiókját toosign használhatja az SAP-CAL, toohello kattintson **felülvizsgálati**. 

9. toocreate hello társítását a felhasználók és az újonnan létrehozott hello SAP naptár fiókja, kattintson a **létrehozása**.

    ![Felhasználói tooaccount társítása](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

Sikeresen létrehozott egy SAP naptár fiókja, amely képes:

- Hello Resource Manager telepítési modellt használja.
- SAP rendszerek üzembe helyezés az Azure-előfizetéshez.

> [!NOTE]
Hello SAP IDES megoldás, amely Windows és az SQL Server telepítése, akkor módosítania kell toosign feliratkozott egy SAP naptár-előfizetés. Ellenkező esetben hello megoldás lehet, hogy megjelennek, **zárolt** hello áttekintése lapon.

### <a name="deploy-a-solution"></a>A megoldás üzembe helyezéséhez
1. Miután beállította az SAP naptár fiókja, válassza ki a **SAP IDES megoldás a Windows és az SQL Server hello** megoldás. Kattintson a **példány létrehozása**, és erősítse meg a hello használati és a használati feltételeket. 

2. A hello **alapvető mód: példány létrehozása** lapon kell:

    a. Adjon meg egy példány **neve**.

    b. Válassza ki az Azure **régió**. Szükség lehet egy SAP CAL előfizetés tooget több Azure-régiók érhető el.

    c.  Adja meg a hello fő **jelszó** hello megoldás, ahogy:

    ![SAP CAL Basic mód: Példány létrehozása](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. Kattintson a **Create** (Létrehozás) gombra. Némi várakozás után attól függően, hogy hello méretét és összetettségét hello megoldás (hello SAP CAL biztosít becsült érték), hello állapota jelenik meg az aktív és használatra kész: 

    ![SAP CAL-példányok](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. toofind hello erőforráscsoport és annak által létrehozott objektumok hello SAP-CAL, nyissa meg az Azure portál toohello. hello virtuális gép található, hogy az SAP-CAL hello adott azonos példánynév hello kezdve.

    ![Erőforrás objektumok](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. Hello SAP CAL Portal, nyissa meg toohello telepített példányok és **Connect**. a következő megjelenő ablakban hello jelenik meg: 

    ![Csatlakozás toohello példány](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. Használata előtt hello beállítások tooconnect telepített toohello rendszert, kattintson a **Getting Started Guide**. hello dokumentáció nevek felhasználók hello hello csatlakozási módszer. hello jelszava azoknak a felhasználóknak be van állítva az Ön által definiált toohello fő jelszó hello hello telepítési folyamat elején. Hello dokumentációban, több működési másoknak szerepel a listában a jelszavukat, amelynek segítségével toosign toohello a telepített rendszer.

    ![Üdvözli a SAP-dokumentáció](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Néhány órán belül egy kifogástalan SAP IDES rendszer van telepítve az Azure-ban.

Ha egy SAP naptár-előfizetés vásárolta, SAP teljes mértékben támogatja az SAP CAL hello környezeteit Azure. hello támogatási várólista BC-VCM-CAL.

