---
title: "aaaCreate és freebsd rendszerű virtuális gép feltöltés kép |} Microsoft Docs"
description: "Toocreate ismerje meg, majd töltse fel a virtuális merevlemez (VHD), amely tartalmazza a hello freebsd rendszerű operációs rendszer toocreate Azure virtuális gép"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kyliel
ms.openlocfilehash: f3bd155e496f1a2713d36bb66ea9824ed4c210ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-freebsd-vhd-tooazure"></a>Hozzon létre, és töltse fel a freebsd rendszerű virtuális merevlemez tooAzure
Ez a cikk bemutatja, hogyan toocreate és feltöltése egy virtuális merevlemez (VHD) tartalmazó hello freebsd rendszerű operációs rendszer. Miután a feltöltés, saját kép toocreate egy virtuális gép (VM) az Azure-ban, használhatja.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. További információ a hello Resource Manager modellt használó virtuális merevlemez feltöltése: [Itt](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek hello:

* **Azure-előfizetés**– Ha nincs fiókja, létrehozhat egyet, néhány perc alatt. Ha MSDN-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Ellenkező esetben megtudhatja, hogyan túl[hozzon létre egy próbafiókot](https://azure.microsoft.com/pricing/free-trial/).  
* **Az Azure PowerShell eszközök**– hello Azure PowerShell modul toouse az Azure-előfizetéshez konfigurált, és telepítve kell lennie. toodownload hello modult, tekintse meg [Azure letölti](https://azure.microsoft.com/downloads/). Egy oktatóanyag, amely ismerteti, hogyan tooinstall és konfigurálása hello modul érhető el itt. Használjon hello [Azure letölti](https://azure.microsoft.com/downloads/) parancsmag tooupload hello VHD-t.
* **Freebsd rendszerű operációs rendszer van telepítve, a .vhd-fájllá**--egy támogatott freebsd rendszerű operációs rendszernek kell lennie a telepített tooa virtuális merevlemezt. Több különféle eszköz toocreate .vhd fájlok léteznek. Például egy virtualizálási megoldást használja például a Hyper-V toocreate hello .vhd fájl, és hello operációs rendszer telepítéséhez. Leírja, hogyan tooinstall és használja a Hyper-V, tekintse meg a [telepítése Hyper-V virtuális gép létrehozása és](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello újabb VHDX formátum nem támogatott az Azure-ban. Hello tooVHD lemezformátum konvertálja a Hyper-V kezelőjével vagy parancsmag hello [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx). Emellett nincs egy [kapcsolatos útmutató az MSDN-en toouse freebsd rendszerű Hyper-v](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

Ez a feladat hello öt lépéseket tartalmazza:

## <a name="step-1-prepare-hello-image-for-upload"></a>1. lépés: Hello lemezképének előkészítése feltöltése
Hello freebsd rendszerű operációs rendszer telepítési hello virtuális gépen végezze el a következő eljárások hello:

1. Engedélyezze a DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. SSH engedélyezése.

    Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva. Alapértelmezés szerint engedélyezve van a freebsd rendszerű lemezről telepítése után. 
3. A soros konzol beállítása.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Telepítse a sudo.

    hello root fiókjának le van tiltva, az Azure-ban. Ez azt jelenti, hogy egy nem rendszerjogosultságú felhasználói toorun parancsokat emelt szintű jogosultságokkal a tooutilize sudo van szüksége.

        # pkg install sudo
   
5. Előfeltételek az Azure-ügynököt.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Telepítse az Azure Agent ügynököt.

    hello hello Azure ügynök legújabb kiadása mindig található [github](https://github.com/Azure/WALinuxAgent/releases). verzió 2.0.10 hello + hivatalosan támogatja a freebsd rendszerű 10 & 10.1 és hello verziója 2.1.4 + (beleértve a 2.2.x) hivatalosan támogatja a freebsd rendszerű 10.2 és a későbbi kibocsátásokban megtörténik.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0 most használja 2.0.16 példa:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1 most használja 2.1.4 példa:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Azure-ügynök telepítése után már fut egy jó ötlet tooverify:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Hello rendszer kiosztásának megszüntetése.

    Deprovision hello rendszer tooclean, és ellenőrizze, megfelelő-e reprovisioning. hello következő parancs is törli hello utolsó kiosztott felhasználói fiókkal és a kapcsolódó hello adatokat:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Most, állítsa le a virtuális Gépet.

## <a name="step-2-create-a-storage-account-in-azure"></a>2. lépés: Az Azure storage-fiók létrehozása
A storage-fiókot az Azure tooupload .vhd-fájllá, azok használt toocreate egy virtuális gép kell. Hello Azure klasszikus portál toocreate tárfiókot is használhat.

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello parancssávon válassza **új**.
3. Válassza ki **adatszolgáltatások** > **tárolási** > **Gyorslétrehozás**.

    ![Storage-fiók gyors létrehozása](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. Töltse ki a hello mezőket az alábbiak szerint:

   * A hello **URL-cím** mezőbe írja be egy altartomány neve toouse hello tárolási fiók URL-címben. hello bejegyzés 3-24 számokat és kisbetűket tartalmazhat. Ez a név lesz hello állomásnév hello URL-címen, amely használt tooaddress Azure Blob Storage tárolóban, az Azure Queue storage vagy a hello előfizetéshez tartozó Azure Table storage-erőforrások.
   * A hello **hely/Affinitáscsoport** legördülő menüben válassza ki a hello **helye vagy affinitáscsoportja** hello tárfiók. Az affinitáscsoportokban lehetővé teszi, hogy a felhőszolgáltatás és a tárolási be hello ugyanabban az adatközpontban.
   * A hello **replikációs** mezőben, döntse el, hogy toouse **Georedundáns** replikációs hello tárfiók. A georeplikáció alapértelmezés szerint be van kapcsolva. Ez a beállítás replikálja az adatokat tooa másodlagos helyen, semmilyen költség tooyou, hello elsődleges helyet, hogy a tároló átadja a feladatokat toothat helyre, ha súlyos hiba történik. másodlagos hely hello automatikusan hozzá van rendelve, és nem módosítható. Ha módosítania kell a felhőalapú tároló megfelelő toolegal követelmények vagy a szervezeti házirend hello helyét teljesebb körű vezérlése, kikapcsolhatja a georeplikáció. Azonban figyelembe, hogy később bekapcsolása georeplikáció, fizetnie kell egyszeri adatátviteli díjat tooreplicate a meglévő adatok toohello másodlagos helye. Tárolási szolgáltatások nélkül georeplikáció kedvezményes áron érhető el. A tárfiókok georeplikáció kezelésével kapcsolatos további részletekért itt található: [Azure Storage replikációs](../../../storage/common/storage-redundancy.md).

     ![Írja be a tárolási fiók adatait](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. Válassza ki **Storage-fiók létrehozása**. hello fiók ekkor már megjelenik a **tárolási**.

    ![A tárfiók sikeresen létrehozva](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. Ezután hozzon létre egy a feltöltött .vhd fájlokat tároló. Hello tárfiók neve, majd válassza ki és **tárolók**.

    ![Tárolási fiók részletei](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. Válassza ki **létrehozni egy tárolót**.

    ![Tárolási fiók részletei](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. A hello **neve** mezőbe írja be a tároló nevét. Ezt követően a hello **hozzáférés** legördülő menüben válassza kívánt hozzáférési házirendet milyen típusú.

    ![Tárolónév](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > Alapértelmezés szerint a hello tároló privát, és csak hello fiók tulajdonosának hozzáférhet. tooallow nyilvános olvasási hozzáférés toohello blobok hello tároló, de nem toohello a tároló tulajdonságainak és metaadatok, használja a hello **nyilvános Blob** lehetőséget. tooallow teljes nyilvános olvasási hozzáférés hello tároló és a blobokat, akkor hello **nyilvános tárolókban** lehetőséget.
   >
   >

## <a name="step-3-prepare-hello-connection-tooazure"></a>3. lépés: Hello kapcsolat tooAzure előkészítése
A .vhd fájlt tölthet, meg kell tooestablish biztonságos kapcsolatot a számítógép és az Azure-előfizetés között. Hello Azure Active Directory (Azure AD) módszert használja, vagy tanúsítvány metódus toodo hello azt.

### <a name="use-hello-azure-ad-method-tooupload-a-vhd-file"></a>Az Azure AD hello metódus tooupload .vhd fájl használata
1. Nyissa meg hello Azure PowerShell-konzolon.
2. Írja be a következő parancs hello:  
    `Add-AzureAccount`

    Ez a parancs megnyit egy bejelentkezési ablakot, ahol jelentkezhet be a munkahelyi vagy iskolai fiókjával.

    ![PowerShell-ablakot](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. Azure hitelesíti, és menti a hello hitelesítő adatokat. Majd lezárja hello ablak.

### <a name="use-hello-certificate-method-tooupload-a-vhd-file"></a>Hello tanúsítvány metódus tooupload .vhd fájl használata
1. Nyissa meg hello Azure PowerShell-konzolon.
2. Típus: `Get-AzurePublishSettingsFile`.
3. Egy böngészőablakban megnyitja, és hogy toodownload hello .publishsettings fájlt kér. Ez a fájl tartalmazza az adatokat és az Azure-előfizetés tanúsítványát.

    ![Böngésző letöltési oldala](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. Hello .publishsettings fájlt mentse.
5. Típus: `Import-AzurePublishSettingsFile <PathToFile>`, ahol `<PathToFile>` hello teljes elérési útja toohello .publishsettings fájlt.

   További információkért lásd: [Ismerkedés az Azure-parancsmagokkal](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   PowerShell konfigurálásával kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="step-4-upload-hello-vhd-file"></a>4. lépés: Hello .vhd fájl feltöltése
Hello .vhd fájlt tölt fel, ha azt bárhol elhelyezheti a Blob-tároló belül. Az alábbiakban néhány fogja használni, amikor hello-fájl feltöltése feltételek:

* **BlobStorageURL** 2. lépésben létrehozott tárfiók hello hello URL-cím.
* **YourImagesFolder** a Blob storage tárolóban hello ahol van toostore a képeket.
* **VHDName** hello címke sem, hogy megjelenik-hello Azure klasszikus portál tooidentify hello virtuális merevlemezt.
* **PathToVHDFile** hello teljes elérési útját és nevét hello .vhd fájl.

Az előző lépésben hello használt hello Azure PowerShell ablakban írja be:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-hello-uploaded-vhd-file"></a>5. lépés: Hello feltöltött .vhd fájl a virtuális gép létrehozása
Hello .vhd fájl feltöltése után az egyéni lemezképek, hogy az előfizetéshez, majd hozzon létre egy virtuális gépet egyéni lemezképpel kép toohello listájaként adhat hozzá.

1. Az előző lépésben hello használt hello Azure PowerShell ablakban írja be:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of hello VHD> -OS <Type of hello OS on hello VHD>

   > [!NOTE]
   > Linux használják hello operációs rendszer típusa. hello aktuális Azure PowerShell-verzió csak a "Linux" vagy "Windows" fogad paraméterként.
   >
   >
2. Hello előző lépések végrehajtása után a hello új lemezkép szerepel-e, ha úgy dönt, hogy hello **képek** hello klasszikus Azure portálra a lap.  

    ![Kép kiválasztása](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Hozzon létre egy virtuális gép hello gyűjteményből. Az új lemezképet már elérhető a **saját lemezképek**.
4. Hello új lemezkép kiválasztása. A következő halad át hello kér tooset be egy állomásnevet, a jelszót, az SSH-kulcs és a stb.

    ![Kép: egyéni](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Hello kiépítés befejezése után megjelenik az Azure-beli freebsd rendszerű virtuális gép.

    ![Az azure-ban freebsd rendszerű kép](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
