---
title: "a StorSimple virtuális tömb VMware-ben aaaProvision |} Microsoft Docs"
description: "A StorSimple virtuális tömb telepítési sorozat második oktatóanyag magában foglalja a VMware-ben a virtuális eszköz kiépítése."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0912c1c315a04ea46b6373a8fcd5554ecae14e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>A StorSimple virtuális tömb - Provision VMware-ben telepítése
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag leírja, hogyan tooprovision, és csatlakozzon a StorSimple virtuális tömb tooa a gazdagéphez, VMware ESXi 5.5 rendszerű vagy újabb rendszeren. Ez a cikk az Azure-portál és a Microsoft Azure Government felhő hello toohello StorSimple virtuális tömbök telepítését vonatkozik.

Rendszergazdai jogosultságok tooprovision kell, és csatlakoztassa tooa virtuális eszközt. hello üzembe helyezési és a kezdeti telepítés körülbelül 10 percig toocomplete is igénybe vehet.

## <a name="provisioning-prerequisites"></a>Telepítési előfeltételek
Előfeltételek tooprovision a virtuális eszköz a rendszer VMware ESXi 5.5 rendszerű gazdagép hello és újabb verzióiban a következők.

### <a name="for-hello-storsimple-device-manager-service"></a>A StorSimple eszköz Manager szolgáltatás hello
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Befejezte az összes hello lépéseket [Prepare hello portal a StorSimple virtuális tömb](storsimple-virtual-array-deploy1-portal-prep.md).
* A VMware hello virtuális eszköz kép már letöltötte a hello Azure-portálon. További információkért lásd: **3. lépés: virtuális eszköz kép letöltése hello** a [Prepare hello portál a StorSimple virtuális tömb útmutató](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-hello-storsimple-virtual-device"></a>Hello StorSimple virtuális eszköz
A virtuális eszköz telepítése előtt győződjön meg arról, hogy:

* Hozzáférés tooa állomás rendszer Hyper-v-T futtató (2008 R2 vagy újabb), hogy lehet használt tooa építhető ki egy eszközt.
* hello gazdarendszer képes toodedicate a virtuális eszköz a következő erőforrások tooprovision hello:

  * Legalább 4 mag.
  * Legalább 8 GB RAM-mal. Ha azt tervezi, tooconfigure hello virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja. 16 GB RAM toosupport 2 – 4 millió fájlok van szüksége.
  * Egy hálózati csatoló.
  * 500 GB-os virtuális lemez Rendszeradatok.

### <a name="for-hello-network-in-datacenter"></a>A datacenter hello hálózat
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Tekintse át hello hálózati követelmények toodeploy a StorSimple virtuális eszköz és a beállított hello adatközpont hálózatán hello követelményeknek. 

## <a name="step-by-step-provisioning"></a>Részletes kiépítése
tooprovision és tooa virtuális eszköz, a lépéseket követve tooperform hello van szüksége:

1. Ügyeljen arra, hogy hello gazdarendszer elegendő erőforrások toomeet hello virtuális eszköz minimális követelményeknek.
2. A hipervizor a virtuális eszköz kiépítése.
3. Indítsa el a virtuális eszköz hello és tud hello IP-címet.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>1. lépés: Győződjön meg arról, gazdagép rendszere megfelel a virtuális eszköz minimális követelményei
a virtuális eszköz toocreate, szüksége lesz:

* Hozzáférés tooa gazdarendszer VMware ESXi Server 5.5 rendszerű vagy újabb verzió.
* VMware vSphere client a rendszer toomanage hello ESXi-állomáson.

  * Legalább 4 mag.
  * Legalább 8 GB RAM-mal. Ha azt tervezi, tooconfigure hello virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja. 16 GB RAM toosupport 2 – 4 millió fájlok van szüksége.
  * Több hálózati adapter csatlakoztatva toohello hálózati forgalom útválasztási tooInternet képes. hello minimális internetes sávszélességet 5 MB/s tooallow hello eszköz optimális működéséhez kell lennie.
  * 500 GB-os virtuális lemez adatait.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>2. lépés: A hipervizor virtuális eszköz kiépítése
Hajtsa végre a következő lépéseket tooprovision a hipervizor a virtuális eszköz hello.

1. Másolja át a hello virtuális eszköz lemezképet a rendszeren. A virtuális lemezképet hello Azure-portálon keresztül letöltött.

   1. Győződjön meg arról, hogy már letöltötte hello legújabb képfájl. Ha korábban letöltött hello kép, újra letölteni tooensure hello legújabb kép rendelkezik. hello legújabb lemezképben van két fájlt (helyett egy).
   2. Jegyezze fel a hello helyet használ a kép hello eljárás későbbi szakaszában, ahová másolni hello kép.

2. Jelentkezzen be toohello hello vSphere ügyfélprogrammal ESXi-kiszolgálón. Toohave rendszergazdai jogosultságokkal toocreate egy virtuális gép van szüksége.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Hello vSphere ügyfél hello készlet szakaszban hello bal oldali panelen válassza ki a hello ESXi-kiszolgálón.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Töltse fel a hello VMDK toohello ESXi-kiszolgálón. Keresse meg a toohello **konfigurációs** hello jobb oldali lapon. A **hardver**, jelölje be **tárolási**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. Hello a jobb oldali ablaktáblán, a **Datastores**, válasszon hello tooupload hello VMDK, ahová adattárolót. hello datastore elegendő szabad terület az operációs rendszer hello és adatlemezek kell rendelkeznie.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Kattintson a jobb gombbal, és válassza ki **Tallózás Datastore**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. A **Datastore böngésző** ablak jelenik meg.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Kattintson az eszköztáron hello, ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) ikon toocreate egy új mappát. Hello mappanevet adjon meg, és jegyezze fel a azt. A mappa neve használandó később (ajánlott az ajánlott eljárás) virtuális gép létrehozásakor. Kattintson az **OK** gombra.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. hello hello bal oldali ablaktábláján megjelenik hello új mappa **Datastore böngésző**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Hello Feltöltés ikonra ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) válassza **fájl feltöltése**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Keresse meg és letöltött toohello VMDK fájlok. Két fájl van. Válassza ki a fájl tooupload.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Kattintson a **nyitott**. VMDK-fájl toohello hello hello feltöltésének megadott datastore elindul. Hello fájl tooupload több percig is eltarthat.
13. Hello feltöltés befejeződése után hello datastore hello mappában létrehozott fájl hello láthatja.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Töltsön fel hello második VMDK-fájl toohello ugyanazon adattár.
14. Visszatérési toohello vSphere client ablak. Kijelölt ESXi-kiszolgálóval, kattintson a jobb gombbal, és válassza ki **új virtuális gép**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. A **hozzon létre új virtuális gép** ablak jelenik meg. A hello **konfigurációs** lapra, jelölje be hello **egyéni** lehetőséget. Kattintson a **Tovább** gombra.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. A hello **név és hely** adja meg azokat a virtuális gép hello nevét. Ezt a nevet meg kell felelnie a 8. lépés korábban megadott hello mappanév (ajánlott).

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. A hello **tárolási** lapon, válassza ki a virtuális gép toouse tooprovision kívánt adattárolót.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. A hello **virtuális gép verziójának** lapon jelölje be **virtuális gép verziójának: 8**. 8 verzióit too11 használatát támogatja.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. A hello **vendég operációs rendszer** lapra, jelölje be hello **vendég operációs rendszer** , **Windows**. A **verzió**, hello legördülő listából válassza ki a **Microsoft Windows Server 2012 (64 bites)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. A hello **processzorok** lapján állítsa be a hello **virtuális szoftvercsatornák számát** és **virtuális szoftvercsatorna magok számát** úgy, hogy hello **magokszáma**4 (vagy több). Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. A hello **memória** csoportjában adja meg a 8 GB (vagy több) RAM-mal. Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. A hello **hálózati** adja meg azokat a hello hello hálózati adapterek száma. hello minimális mérete legalább egy hálózati illesztőt.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. A hello **SCSI-vezérlő** lap, fogadja el az alapértelmezett hello **LSI Logic vezérlő**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. A hello **válasszon ki egy lemezt** lapon, válassza ki **meglévő virtuális lemez használata**. Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. A hello **meglévő lemez kiválasztása** lap **lemezt fájlútvonallal**, kattintson a **Tallózás**. Ekkor megnyílik egy **Tallózás Datastores** párbeszédpanel. Keresse meg a toohello helyre, ahol feltöltött hello vmdk-fájl. Hello két fájlt kezdetben feltöltött egyesített, ekkor megjelenik a hello datastore csak egy fájl. Válassza ki hello fájlt, és kattintson a **OK**. Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. A hello **speciális beállítások** lapon fogadja el a hello alapértelmezett, és kattintson **következő**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. A hello **készen tooComplete** lapján tekintheti meg a hello új virtuális géphez tartozó összes hello beállításokat. Ellenőrizze **még a befejeződése előtt hello virtuális gép beállításainak módosítása**. Kattintson a **továbbra is**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. A hello **virtuális gépek tulajdonságai** lap hello **hardver** lapján keresse meg a hello hardver. Válassza ki **új merevlemez**. Kattintson az **Add** (Hozzáadás) parancsra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Megjelenik egy **hardver hozzáadása** ablak. A hello **eszköztípus** lap **válasszon hello eszköztípusba kívánja tooadd**, jelölje be **merevlemez**, és kattintson a **tovább**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. A hello **válasszon ki egy lemezt** lapon, válassza ki **hozzon létre egy új virtuális lemez**. Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. A hello **lemez** lapon, majd hello **lemezméretet** too500 GB (vagy több). Míg 500 GB hello vonatkozó minimális követelménynek, egy nagyobb méretű lemezt mindig oszthat meg. Vegye figyelembe, hogy nem bontsa vagy Zsugorítás többször kiépített hello lemez. Kapcsolatos további adatokat lemez tooprovision hello mérete, tekintse át a hello méretezési szakasz hello [ajánlott eljárások a dokumentum](storsimple-ova-best-practices.md). A **lemez kiépítés**, jelölje be **vékony létesítési**. Kattintson a **Tovább** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. A hello **speciális beállítások** lap, fogadja el a hello alapértelmezett.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. A hello **készen tooComplete** lapján tekintse át a hello lemez beállításai. Kattintson a **Befejezés** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Virtuális gép tulajdonságlapján toohello visszaadása. Egy új merevlemezre tooyour virtuális gép kerül. Kattintson a **Befejezés** gombra.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. A virtuális géphez kiválasztott hello jobb oldali ablaktáblán, keresse meg a toohello **összegzés** fülre. Tekintse át a virtuális gép hello beállításait.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

A virtuális gép ezzel ki van építve. következő lépés hello toopower ezen a számítógépen, és tud hello IP-címet.

## <a name="step-3-start-hello-virtual-device-and-get-hello-ip"></a>3. lépés: Indítsa el a virtuális eszköz hello és hello IP beolvasása
Hajtsa végre a következő lépéseket toostart hello a virtuális eszköz, és csatlakozzon a tooit.

#### <a name="toostart-hello-virtual-device"></a>toostart hello virtuális eszköz
1. Indítsa el a virtuális eszköz hello. A hello vSphere Configuration Manager hello bal oldali panelen válassza ki azt az eszközt, és kattintson a jobb gombbal a toobring hello helyi menü. Válassza ki **Power** majd **Bekapcsolással**. Ez a virtuális gépen kell kapcsolja. Hello állapotát megtekintheti a hello alsó **legutóbbi tevékenységek** hello vSphere client panelén.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. hello beállítási feladatok eltarthat néhány percig toocomplete. Miután hello-eszközön fut, nyissa meg a toohello **konzol** fülre. Ctrl + Alt + Delete toolog küldenek toohello eszköz. Azt is megteheti pont hello mutatót a hello konzolablakot, és nyomja le a Ctrl + Alt + Insert. hello alapértelmezett felhasználó *StorSimpleAdmin* hello alapértelmezett jelszó pedig *jelszó1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Biztonsági okokból hello eszköz rendszergazdai jelszava lejár, a hello első bejelentkezéskor. Biztosan felszólító toochange hello jelszót.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Adjon meg legalább 8 karakterből álló jelszót. hello jelszónak tartalmaznia kell a 3 kívüli 4. Ezek a követelmények: nagybetűk, nagybetűk, numerikus és speciális karaktereket. Adja meg újból hello jelszó tooconfirm azt. Értesítést fog kapni, hello a jelszó megváltozott.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Hello jelszava sikeresen módosult, hello virtuális eszköz előfordulhat, hogy újraindítása után. Várjon, amíg hello újraindítás toocomplete. hello hello eszköz Windows PowerShell-konzolban a lehetséges, hogy együtt egy folyamatjelző jelenik meg.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. 6 – 8. lépések csak akkor alkalmazható, ha a-DHCP környezetben rendszerindítást. Ha egy DHCP-környezetben, hagyja ki ezeket a lépéseket, és nyissa meg toostep 9. Az eszköz nem DHCP-környezet telepítése rendszerindításhoz, a következő képernyő hello látják.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Ezután konfigurálja a hello hálózati.
7. Használjon hello `Get-HcsIpAddress` toolist hello hálózati csatolót engedélyezni kell a virtuális eszközön parancsot. Ha az eszköz engedélyezve egyetlen hálózati adapterrel rendelkezik, hello alapértelmezett hozzárendelt név toothis felülete `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Használjon hello `Set-HcsIpAddress` parancsmag tooconfigure hello hálózati. Példa alatt:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. Hello kezdeti telepítés befejezése után, hello eszköz be betöltődött, hello eszköz szalagcím szöveg jelenik meg. Jegyezze fel a hello IP-cím és hello URL hello szalagcím szöveg toomanage hello eszköz jelenik meg. Az IP cím tooconnect toohello webes felhasználói Felületét a virtuális eszköz és a teljes hello helyi telepítés és a regisztrációs fogja használni.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Választható) Hajtsa végre ezt a lépést, csak akkor, ha az eszköz hello kormányzati felhő telepíti. Most már az eszközön hello az Amerikai Egyesült Államok Federal Information Processing Standard (FIPS) mód lehetővé teszi. hello FIPS 140 szabvány határozza meg a bizalmas adatok védelmének hello amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat.

    1. tooenable hello FIPS-módban, futtassa a következő parancsmag hello:

        `Enable-HcsFIPSMode`
    2. Miután engedélyezte a hello FIPS-módban, hogy hello kriptográfiai érvényesítést érvénybe lépéséhez, indítsa újra az eszközt.

       > [!NOTE]
       > Engedélyezheti vagy letilthatja a FIPS-módban az eszközön. Váltakozó hello eszköz közötti FIPS és a FIPS módban nem támogatott.
       >
       >

Ha az eszköz nem felel meg a hello minimális konfigurációs követelményeinek, látni fogja a hibát jelez hello szalagcím szöveg (lásd alább). Toomodify hello eszközkonfiguráció kell, hogy a megfelelő erőforrásokat toomeet hello minimális követelményei. Ezután indítsa újra, és csatlakoztassa toohello eszközt. Tekintse meg a minimális konfigurációs követelményeinek toohello [1. lépés: Győződjön meg arról, hogy hello gazdagép rendszere megfelel-e a virtuális eszköz minimális követelmények](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Ha bármilyen hiba hello kezdeti konfiguráció hello helyi webes felhasználói felület használata során szembesülnek, tekintse meg a következő munkafolyamatok toohello:

* Diagnosztikai tesztek futtatása túl[webes felhasználói felületen való beállításának hibaelhárítása](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Napló csomagot és naplófájlban](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Következő lépések
* [Állítsa be a StorSimple virtuális tömb fájlkiszolgálóként](storsimple-virtual-array-deploy3-fs-setup.md)
* [Állítsa be a StorSimple virtuális tömb iSCSI-kiszolgálóként](storsimple-virtual-array-deploy3-iscsi-setup.md)
