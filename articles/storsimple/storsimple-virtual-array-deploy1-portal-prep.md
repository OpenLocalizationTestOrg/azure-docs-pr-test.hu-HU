---
title: "a StorSimple virtuális tömb aaaPortal előkészítő |} Microsoft Docs"
description: "Első oktatóanyagot toodeploy StorSimple virtuális tömb magában foglalja a hello Azure-portálon előkészítése"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5332b235e7296a9274f2e7dafcdf72f4b9cdadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-hello-azure-portal"></a>A StorSimple virtuális tömb telepítése – hello Azure-portálon előkészítése

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Áttekintés

Ez az hello első cikkében hello sorozat szükséges az üzembehelyezési oktatóanyagok toocompletely egy fájl vagy hello Resource Manager modellt használja az iSCSI-kiszolgáló telepítése a virtuális tömbjét. Ez a cikk ismerteti a hello előkészületek toocreate, és konfigurálása a StorSimple Device Manager szolgáltatás előzetes tooprovisioning virtuális tömb. Ez a cikk is ki tooa üzembehelyezési konfigurációs ellenőrzőlista és konfigurációs Előfeltételek hivatkozásokat tartalmaz.

Rendszergazdai jogosultságok toocomplete hello beállítása és konfigurációja folyamat van szüksége. Ajánlott áttekinteni hello üzembehelyezési konfigurációs ellenőrzőlista megkezdése előtt. hello portál előkészítése kisebb, mint 10 percet vesz igénybe.

Ebben a cikkben közzétett hello információ toohello és központi telepítése a StorSimple virtuális tömbök hello Azure-portál a Microsoft Azure Government felhő vonatkozik.

### <a name="get-started"></a>Bevezetés
hello telepítési munkafolyamat hello portal előkészítése, a virtualizált környezetben egy virtuális tömb kiépítés és hello telepítést áll. tooget hello StorSimple virtuális tömb központi telepítésben is egy fájl vagy iSCSI-kiszolgáló használatába, a következő táblázatos erőforrások toorefer toohello kell.

#### <a name="deployment-articles"></a>Telepítési cikkek

toodeploy a StorSimple virtuális tömb, tekintse meg a következő cikkek feladatütemezési előírt hello toohello.

| **#** | **Ebben a lépésben** | **Ezt megteheti...** | **És használja ezeket a dokumentumokat.** |
| --- | --- | --- | --- |
| 1. |**Azure-portálon hello beállítása** |Létrehozása és konfigurálása a StorSimple Device Manager szolgáltatás előzetes tooprovisioning egy StorSimple virtuális tömb. |[Hello portal előkészítése](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Kiépítés hello virtuális tömb** |A Hyper-V kiépíteni, és csatlakozzon a StorSimple virtuális tömb tooa állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren. <br></br> <br></br> A VMware kiépíteni, és csatlakozzon a tooa StorSimple virtuális tömb a gazdagéphez, VMware ESXi 5.5 rendszerű vagy újabb rendszeren.<br></br> |[A Hyper-V egy virtuális tömb kiépítése](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [VMware-ben a virtuális tömb kiépítése](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Virtuális tömb hello beállítása** |A fájlkiszolgáló hajtsa végre a kezdeti beállítás regisztrálása a StorSimple fájlkiszolgáló és hello eszköz beállításának befejezése. SMB-megosztások majd oszthat. <br></br> <br></br> Az iSCSI-kiszolgáló hajtsa végre a kezdeti beállítás, a StorSimple iSCSI kiszolgáló regisztrálásához és hello eszköz beállításának befejezése. Az iSCSI-kötetek majd oszthat. |[Fájlkiszolgáló virtuális tömb beállítása](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Állítsa be a tömb virtuális iSCSI-kiszolgálóként](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Most már megkezdheti a tooset mentése hello Azure-portálon.

## <a name="configuration-checklist"></a>Konfigurációs ellenőrzőlista

hello konfigurációs ellenőrzőlista leírja, hogy a StorSimple virtuális tömb hello szoftver konfigurálása előtt szükséges toocollect hello információkat. Ezek az információk idő segítségével előre érdekében hello folyamat hello StorSimple eszközt az adott környezetben történő telepítésének előkészítése. Attól függően, hogy a StorSimple virtuális tömb van telepítve egy fájl vagy iSCSI-kiszolgáló, egyike szükséges a következő ellenőrzőlisták hello.

* Töltse le a hello [StorSimple virtuális tömb fájl kiszolgáló konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Töltse le a hello [StorSimple virtuális tömb iSCSI kiszolgáló konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Előfeltételek

Itt megtalálja az Ön hello konfigurációs előfeltételeit a StorSimple eszköz kezelő szolgáltatás, a StorSimple virtuális tömb és hello adatközponti hálózathoz.

### <a name="for-hello-storsimple-device-manager-service"></a>A StorSimple eszköz Manager szolgáltatás hello

Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Rendelkezik Microsoft-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* Rendelkezik Microsoft Azure Storage-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* A Microsoft Azure-előfizetés engedélyezni kell a StorSimple eszköz Manager szolgáltatáshoz.

### <a name="for-hello-storsimple-virtual-array"></a>A StorSimple virtuális tömb hello

Egy virtuális tömb telepítése előtt győződjön meg arról, hogy:

* Hozzáférés tooa állomás rendszer fut a Windows Server 2008 R2 vagy újabb Hyper-V vagy VMware (ESXi 5.5-ös vagy újabb), amely használt tooa eszköz kiépítése.
* hello gazdarendszer képes toodedicate a virtuális tömb a következő erőforrások tooprovision hello:
  
  * Legalább 4 mag.
  * Legalább 8 GB RAM-mal. Ha azt tervezi, tooconfigure hello virtuális tömb fájlkiszolgálóként, a 8 GB 2 millió fájlokat támogatja. 16 GB RAM toosupport 2 – 4 millió fájlok van szüksége.
  * Egy hálózati csatoló.
  * 500 GB-os virtuális lemez Rendszeradatok.

### <a name="for-hello-datacenter-network"></a>Hello adatközpont-hálózat

Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Az Adatközpont hálózatának hello hello a StorSimple eszköz hálózatkezelési követelményei szerint van konfigurálva. További információkért lásd: hello [StorSimple virtuális tömb rendszerkövetelmények](storsimple-ova-system-requirements.md).
* A StorSimple virtuális tömb nem rendelkezik egy dedikált 5 MB/s internetes sávszélességet (vagy több) mindenkor. A sávszélesség nem lehet megosztva egyéb alkalmazásokkal.

## <a name="step-by-step-preparation"></a>Részletes előkészítése

A következő lépésről tooprepare hello a portál használni hello StorSimple Device Manager szolgáltatást.

## <a name="step-1-create-a-new-service"></a>1. lépés: Új szolgáltatás létrehozása

Egyetlen példány futhat hello StorSimple Device Manager szolgáltatás több StorSimple virtuális tömbök kezelheti. Hajtsa végre a következő lépéseket toocreate hello StorSimple Device Manager szolgáltatás egy példánya hello. Ha egy meglévő StorSimple Device Manager szolgáltatás toomanage a virtuális tömbök, kihagyhatja ezt a lépést, és nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Ha nem engedélyezte a tárfiók automatikus létrehozását hello a szolgáltatással, szüksége lesz a toocreate legalább egy tárfiókot, miután sikeresen létrehozott egy szolgáltatást.
> 
> * Ha nem hozott létre egy tárfiókot automatikusan, nyissa meg túl[hello szolgáltatást egy új tárfiók konfigurálása](#optional-step-configure-a-new-storage-account-for-the-service) részletes információkra van szüksége.
> * Ha engedélyezte a tárfiók automatikus létrehozását hello, nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése

Miután hello StorSimple Device Manager szolgáltatás fut, akkor tooget hello Szolgáltatásregisztrációs kulcs. Ez a kulcs használt tooregister, és csatlakoztathatja StorSimple eszközét hello szolgáltatásban.

Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/).

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> hello Szolgáltatásregisztrációs kulcs használt tooregister összes hello StorSimple Device Manager eszközök, amelyeket a StorSimple Device Manager szolgáltatással tooregister.
> 
> 

## <a name="step-3-download-hello-virtual-array-image"></a>3. lépés: Hello virtuális tömb lemezkép letöltése

Miután hello szolgáltatás regisztrációs kulcsának, a gazdarendszer toodownload hello megfelelő virtuális tömb kép tooprovision virtuális tömb lesz szükség. hello virtuális tömb lemezképek operációs rendszer adott és tölthető le: első lépések oldaláról hello hello Azure-portálon.

> [!IMPORTANT]
> a StorSimple virtuális tömb hello futó hello szoftver csak hello StorSimple Device Manager szolgáltatás is használhatók.
> 
> 

Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/).

#### <a name="tooget-hello-virtual-array-image"></a>tooget hello virtuális tömb kép

1. Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com/). 
2. Hello Azure-portálon, kattintson **Tallózás > StorSimple eszköz kezelői**.
3. Válasszon ki egy meglévő StorSimple Device Manager szolgáltatást. A hello **StorSimple Device Manager** panelen kattintson a **gyors üzembe helyezés**. 
4. Kattintson a hello hivatkozás megfelelő toohello képe, amelyet a Microsoft Download Center hello toodownload. hello kép fájlok körülbelül 4.8 GB.
   
   * A Hyper-V a Windows Server 2012 és újabb verziók VHDX
   * Virtuális merevlemez Hyper-v a Windows Server 2008 R2 és újabb verziók
   * Vmdk-fájl, a VMWare ESXi 5.5 és újabb verziók
5. Töltse le és csomagolja ki a hello fájl tooa helyi meghajtó, ahol hello tömörítetlen fájl megtalálható-e a Megjegyzés elvégzése.

## <a name="optional-step-configure-a-new-storage-account-for-hello-service"></a>Nem kötelező lépés: hello szolgáltatást egy új tárfiók konfigurálása

Ez a lépés nem kötelező, és csak akkor, ha a szolgáltatással nem engedélyezte a tárfiók automatikus létrehozását hello kell végrehajtani.

Ha Azure-tárfiók más régióban toocreate van szüksége, tekintse meg [hogyan toocreate tárfiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) részletes útmutatásait.

Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://ms.portal.azure.com/) a hello StorSimple Device Manager szolgáltatás lapján tooadd egy meglévő Microsoft Azure storage-fiókot.

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a>a tárolási fiók hitelesítő adatait, amelynek tooadd hello hello Eszközkezelő szolgáltatásként azonos Azure-előfizetés

1. Keresse meg a tooyour Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán. Ekkor megnyílik a hello **áttekintése** panelen.
2. Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. A hello **tárfiók hozzáadása** panelen a következő hello:
   
    1. A **előfizetés**, jelölje be **aktuális**.
   
    2. Adja meg az Azure storage-fiók hello nevét.
   
    3. Válassza ki **engedélyezése** toocreate biztonságos csatornát a StorSimple eszköz és hello felhő közötti hálózati kommunikációhoz. Válassza ki **letiltása** csak akkor, ha magánfelhőben tevékenykedik.
   
    4. Kattintson az **Add** (Hozzáadás) parancsra. Értesítés jelenik meg a tárfiók hello sikeres létrehozása után.<br></br>
   
     ![Adja hozzá egy meglévő storage-fiók hitelesítő adatait](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Következő lépés

következő lépés hello tooprovision a StorSimple virtuális tömb virtuális gép. Attól függően, hogy a gazda operációs rendszer, lásd: hello részletes utasításait:

* [A Hyper-V egy StorSimple virtuális tömb kiépítése](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [Kiépíteni a StorSimple virtuális tömb VMware-ben](storsimple-virtual-array-deploy2-provision-vmware.md)

