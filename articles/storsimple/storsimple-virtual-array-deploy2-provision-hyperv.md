---
title: "a Hyper-v-ben a StorSimple virtuális tömb aaaProvision |} Microsoft Docs"
description: "A StorSimple virtuális tömb telepítési második oktatóanyag magában foglalja a kiépítés a Hyper-V egy virtuális tömb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f47d642f740827ae1440b819e07067c6a183527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>A StorSimple virtuális tömb - Provision a Hyper-V telepítése
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag leírja, hogyan tooprovision egy StorSimple virtuális tömb állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren. Ez a cikk az Azure-portál és a Microsoft Azure Government felhő toohello StorSimple virtuális tömbök telepítését vonatkozik.

Rendszergazdai jogosultságok tooprovision kell, és konfigurálja a virtuális tömb. hello üzembe helyezési és a kezdeti telepítés körülbelül 10 percig toocomplete is igénybe vehet.

## <a name="provisioning-prerequisites"></a>Telepítési előfeltételek
Itt megtalálja hello Előfeltételek tooprovision virtuális tömb állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren.

### <a name="for-hello-storsimple-device-manager-service"></a>A StorSimple eszköz Manager szolgáltatás hello
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Befejezte az összes hello lépéseket [Prepare hello portal a StorSimple virtuális tömb](storsimple-virtual-array-deploy1-portal-prep.md).
* A Hyper-V hello virtuális tömb kép hello Azure-portálon a töltötték le. További információkért lásd: **3. lépés: virtuális tömb kép letöltése hello** a [Prepare hello portál a StorSimple virtuális tömb útmutató](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > a StorSimple virtuális tömb hello futó hello szoftver csak hello StorSimple Device Manager szolgáltatás is használhatók.
  >
  >

### <a name="for-hello-storsimple-virtual-array"></a>A StorSimple virtuális tömb hello
Egy virtuális tömb telepítése előtt győződjön meg arról, hogy:

* Hozzáférés tooa gazdarendszeren futó Hyper-V a Windows Server 2008 R2 vagy újabb, hogy lehet használt tooa építhető ki egy eszközt, hogy.
* hello gazdarendszer képes toodedicate a virtuális tömb a következő erőforrások tooprovision hello:

  * Legalább 4 mag.
  * Legalább 8 GB RAM-mal. Ha azt tervezi, tooconfigure hello virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja. 16 GB RAM toosupport 2 – 4 millió fájlok van szüksége.
  * Egy hálózati csatoló.
  * 500 GB-os virtuális lemez adatait.

### <a name="for-hello-network-in-hello-datacenter"></a>Hello adatközpontban hello hálózat
Mielőtt elkezdené, tekintse át a hálózati követelmények toodeploy egy StorSimple virtuális tömb hello és hello adatközpont-hálózat megfelelő konfigurálása. További információkért lásd: [StorSimple virtuális tömb hálózati követelményei](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Részletes kiépítése
tooprovision és tooa virtuális tömb csatlakozni, a lépéseket követve tooperform hello van szüksége:

1. Győződjön meg arról, hogy rendelkezik-e elegendő erőforrást toomeet hello minimális virtuális tömb követelmények hello gazdarendszer.
2. Egy virtuális tömbhöz a hipervizor kiépítéséhez.
3. Indítsa el a hello virtuális tömb, és tud hello IP-címet.

A lépések esetén, tekintse meg a következő részekben hello.

## <a name="step-1-ensure-that-hello-host-system-meets-minimum-virtual-array-requirements"></a>1. lépés: Győződjön meg arról, hogy hello gazdagép rendszere megfelel-e a minimális virtuális tömb követelményeknek
egy virtuális tömb toocreate, lesz szüksége:

* hello Hyper-V szerepkör telepítése a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 SP1.
* A Microsoft Hyper-V Manager a Microsoft Windows-ügyfélen toohello állomás csatlakoztatva.

Győződjön meg arról, hogy az alapul szolgáló hardver (gazdarendszer), amelyen hello virtuális tömb létrehozásakor hello a következő erőforrások tooyour virtuális tömb képes toodedicate hello:

* Legalább 4 mag.
* Legalább 8 GB RAM-mal. Ha azt tervezi, tooconfigure hello virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja. 16 GB RAM toosupport 2 – 4 millió fájlok van szüksége.
* Egy hálózati csatoló.
* 500 GB-os virtuális lemez Rendszeradatok.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>2. lépés: A hipervizor egy virtuális tömb kiépítése
Hajtsa végre a következő lépéseket tooprovision egy eszközt a hipervizor hello.

#### <a name="tooprovision-a-virtual-array"></a>egy virtuális tömb tooprovision
1. A Windows Server-állomáson hello virtuális tömb kép tooa helyi meghajtóra másolja. Ez a rendszerkép (VHD- vagy VHDX-) letöltött hello Azure-portálon keresztül. Jegyezze fel a hello helyet használ a kép hello eljárás későbbi szakaszában, ahová másolni hello kép.
2. Nyissa meg **Kiszolgálókezelő**. Hello jobb felső sarokban, kattintson **eszközök** válassza **Hyper-V kezelője**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Ha Windows Server 2008 R2 fut, nyissa meg a Hyper-V kezelője hello. A Kiszolgálókezelőben kattintson **szerepkörök > Hyper-V > Hyper-V kezelője**.
3. A **Hyper-V kezelője**, hello hatókör ablaktáblán, kattintson a jobb gombbal a rendszer csomópont tooopen hello helyi menü, és kattintson a **új** > **virtuális gép**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. A hello **megkezdése előtt** hello új virtuális gép varázsló oldalán kattintson **következő**.
5. A hello **név és hely megadása** lapján adjon meg egy **neve** a virtuális tömbhöz. Kattintson a **Tovább** gombra.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. A hello **adnia generációját** lapon válassza ki a lemezkép eszköztípus hello, és kattintson a **következő**. Ez a lap nem jelenik meg, a Windows Server 2008 R2 használata esetén.

   * Válasszon **2. generációs** Ha letöltötte a .vhdx-lemezkép a Windows Server 2012 vagy újabb.
   * Válasszon **1. generáció** Ha letöltötte a .vhd lemezképek a Windows Server 2008 R2 vagy újabb.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. A hello **memóriát rendelnek** adja meg azokat a **indítási memória** a legalább **8192 MB**, ne engedélyezze a dinamikus memóriát, és kattintson a **következő**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. A hello **Hálózatkezelés beállítása** lapon, adja meg a hello virtuális kapcsolóhoz csatlakoztatott toohello Internet majd **következő**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. A hello **virtuális merevlemez csatlakoztatása** lapon, válassza ki **meglévő virtuális merevlemez használata**, adja meg a hello virtuális tömb lemezképének (.vhdx vagy .vhd) hello helyét, és kattintson **tovább**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. Felülvizsgálati hello **összegzés** majd **Befejezés** toocreate hello virtuális gép.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. 4 mag kell toomeet hello minimális követelményeknek. tooadd 4 virtuális processzor, jelölje ki a gazdarendszer hello **Hyper-V kezelője** ablak. Hello jobb oldali ablaktáblában hello listája alatt **virtuális gépek**, keresse meg az imént létrehozott hello virtuális gépet. Válassza ki, és kattintson a jobb gombbal a hello gép nevét, majd **beállítások**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. A hello **beállítások** a hello bal oldali ablaktáblában kattintson **processzor**. A jobb oldali ablaktáblában hello beállítása **a virtuális processzorok száma** too4 (vagy több). Kattintson az **Alkalmaz** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. toomeet hello minimális követelményeknek, akkor is kell tooadd 500 GB-os virtuális adatlemezt. A hello **beállítások** lap:

    1. Hello bal oldali ablaktáblában jelöljön ki **SCSI-vezérlő**.
    2. Hello jobb oldali ablaktáblában jelöljön ki **merevlemez,** kattintson **Hozzáadás**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. A hello **merevlemez-meghajtóról** lapra, jelölje be hello **virtuális merevlemez** lehetőséget, majd kattintson a **új**. Hello **új virtuális merevlemez varázsló** kezdődik.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. A hello **megkezdése előtt** hello új virtuális merevlemez varázsló oldalán kattintson **következő**.
16. A hello **lemezformátum lap**, fogadja el az alapértelmezett lehetőség hello a **VHDX** formátumban. Kattintson a **Tovább** gombra. Ez a képernyő nem jelenik meg, ha a Windows Server 2008 R2 rendszerű.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. A hello **lemeztípus lap**, állítsa be a virtuális merevlemez típusát, **dinamikusan bővülő** (ajánlott). **Rögzített méretű** lemez működik, de előfordulhat toowait hosszú ideig. Javasoljuk, hogy ne használjon hello **Differencing** lehetőséget. Kattintson a **Tovább** gombra. A Windows Server 2012 R2 és Windows Server 2012-ben **dinamikusan bővülő** hello alapértelmezett beállítás van, mivel a Windows Server 2008 R2, a hello alapértelmezett **mérete rögzített**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. A hello **név és hely megadása** lapján adja meg a **neve** , valamint **hely** (tallózással is kikeresheti tooone) hello adatok lemez. Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. A hello **lemez beállítása** lapon, jelölje be hello beállítás **hozzon létre egy új üres virtuális merevlemez** , és adja meg a méret a hello **500 GB** (vagy több). Míg 500 GB hello vonatkozó minimális követelménynek, egy nagyobb méretű lemezt mindig oszthat meg. Vegye figyelembe, hogy nem bontsa vagy Zsugorítás többször kiépített hello lemez. Kapcsolatos további adatokat lemez tooprovision hello mérete, tekintse át a hello méretezési szakasz hello [ajánlott eljárások a dokumentum](storsimple-ova-best-practices.md). Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. A hello **összegzés** lapon ellenőrizze a virtuális adatlemez hello részleteit, és ha mindent megfelelőnek talált, kattintson a **Befejezés** toocreate hello lemez. hello varázsló bezárása után, és a virtuális merevlemez tooyour gép ad hozzá.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. Térjen vissza a toohello **beállítások** lap. Kattintson a **OK** tooclose hello **beállítások** lapon, és térjen vissza a tooHyper-V Manager ablakát.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-hello-virtual-array-and-get-hello-ip"></a>3. lépés: Indítsa el a virtuális tömb hello és hello IP beolvasása
Hajtsa végre a következő lépéseket toostart hello a virtuális tömb, és csatlakozzon a tooit.

#### <a name="toostart-hello-virtual-array"></a>toostart hello virtuális tömb
1. Indítsa el a hello virtuális tömb.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Miután hello-eszközön fut, hello eszközt, kattintson a jobb gombbal válassza ki és **Connect**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. Előfordulhat, hogy toowait hello eszköz toobe készen áll az 5-10 perc. Állapotüzenet jelenik meg hello konzol tooindicate hello folyamatban van. Után hello eszköz készen áll, folytassa az túl**művelet**. Nyomja le az `Ctrl + Alt + Delete` toolog toohello virtuális tömbben. hello alapértelmezett felhasználó *StorSimpleAdmin* hello alapértelmezett jelszó pedig *jelszó1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Biztonsági okokból hello eszköz rendszergazdai jelszava lejár, a hello első bejelentkezéskor. Biztosan felszólító toochange hello jelszót.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   Adjon meg legalább 8 karakterből álló jelszót. hello jelszót meg kell felelnie legalább 4-követelményeknek hello kívül 3: nagybetűk, nagybetűk, numerikus és speciális karaktereket. Adja meg újból hello jelszó tooconfirm azt. Értesítés jelenik meg, hogy hello a jelszó megváltozott.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Hello jelszava sikeresen módosult, miután hello virtuális tömb újraindulhat. Várjon, amíg hello eszköz toostart.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    Windows PowerShell-konzolban hello hello eszköz együtt egy folyamatjelző jelenik meg.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. 6 – 8. lépések csak akkor alkalmazható, ha a-DHCP környezetben rendszerindítást. Ha egy DHCP-környezetben, hagyja ki ezeket a lépéseket, és nyissa meg toostep 9. Az eszköz nem DHCP-környezet telepítése rendszerindításhoz, a következő képernyő hello látják.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Ezután konfigurálja a hello hálózati.
7. Használjon hello `Get-HcsIpAddress` toolist hello hálózati csatolót engedélyezni kell a virtuális tömb parancsot. Ha az eszköz engedélyezve egyetlen hálózati adapterrel rendelkezik, hello alapértelmezett hozzárendelt név toothis felülete `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Használjon hello `Set-HcsIpAddress` parancsmag tooconfigure hello hálózati. Tekintse meg a következő példa hello:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. Hello kezdeti telepítés befejezése után, hello eszköz be betöltődött, hello eszköz szalagcím szöveg jelenik meg. Jegyezze fel a hello IP-cím és hello URL hello szalagcím szöveg toomanage hello eszköz jelenik meg. Az IP cím tooconnect toohello webes felhasználói Felületét a virtuális tömb és a teljes hello helyi telepítés és a regisztráció használja.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (Választható) Hajtsa végre ezt a lépést, csak akkor, ha az eszköz hello kormányzati felhő telepíti. Most már az eszközön hello az Amerikai Egyesült Államok Federal Information Processing Standard (FIPS) mód lehetővé teszi. hello FIPS 140 szabvány határozza meg a bizalmas adatok védelmének hello amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat.

    1. tooenable hello FIPS-módban, futtassa a következő parancsmag hello:

        `Enable-HcsFIPSMode`
    2. Miután engedélyezte a hello FIPS-módban, hogy hello kriptográfiai érvényesítést érvénybe lépéséhez, indítsa újra az eszközt.

       > [!NOTE]
       > Engedélyezheti vagy letilthatja a FIPS-módban az eszközön. Váltakozó hello eszköz közötti FIPS és a FIPS módban nem támogatott.
       >
       >

Ha az eszköz nem felel meg a hello minimális konfigurációs követelményeinek, lásd: hello a következő hiba történt a hello szalagcím szöveg (lásd alább). Hello eszköz konfigurációjának módosítása, így hello gépnek hello minimális követelményeknek megfelelő források toomeet. Ezután indítsa újra, és csatlakoztassa toohello eszközt. Tekintse meg a minimális konfigurációs követelményeinek toohello [1. lépés: Győződjön meg arról, hogy hello gazdagép rendszere megfelel-e a minimális virtuális tömb követelmények](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Ha bármilyen hiba hello kezdeti konfiguráció hello helyi webes felhasználói felület használata során szembesülnek, tekintse meg a következő munkafolyamatok toohello:

* Diagnosztikai tesztek futtatása túl[webes felhasználói felületen való beállításának hibaelhárítása](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Napló csomagot és naplófájlban](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Következő lépések
* [Állítsa be a StorSimple virtuális tömb fájlkiszolgálóként](storsimple-virtual-array-deploy3-fs-setup.md)
* [Állítsa be a StorSimple virtuális tömb iSCSI-kiszolgálóként](storsimple-virtual-array-deploy3-iscsi-setup.md)
