---
title: "aaaMigrating virtuális gépek tooAzure prémium szintű Storage |} Microsoft Docs"
description: "Telepítse át a meglévő virtuális gépek tooAzure prémium szintű Storage. Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása az Azure virtuális gépeken futó I/O-igényes munkaterhelések kínál."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: 19aaf2b7594e570f5a964baa00958a7a8eaae97b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-premium-storage-unmanaged-disks"></a>Prémium szintű Storage (felügyelet lemezek) áttelepítése tooAzure

> [!NOTE]
> A cikk ismerteti, hogyan toomigrate egy virtuális gép által használt virtuális gép által használt nem felügyelt standard lemezek tooa nem felügyelt premium lemezek. Javasoljuk, hogy Azure felügyelt lemezek használatakor az új virtuális gépekhez, és, hogy az előző nem felügyelt lemezek toomanaged lemezek konvertálása. Felügyelt lemezek leíró hello alapul szolgáló storage-fiókok, így nem kell. További információkért lásd: a [felügyelt lemezekhez – áttekintés](../../virtual-machines/windows/managed-disks-overview.md).
>

Prémium szintű Storage nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása nyújt. Telepítse át az alkalmazás virtuális lemezek tooAzure prémium szintű Storage is igénybe vehet hello sebesség előnyeit, és ezek a lemezek teljesítményét.

hello Ez az útmutató célja toohelp új prémium szintű Azure Storage jobb felhasználói előkészítése toomake a jelenlegi rendszer tooPremium tárolási zavartalan átmenetet. hello útmutató foglalkozik három hello legfontosabb összetevők, a folyamat:

* [Hello áttelepítési tooPremium tárolás tervezése](#plan-the-migration-to-premium-storage)
* [Készítse elő és másolási virtuális merevlemezeket (VHD) tooPremium tároló](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Prémium szintű Storage használata Azure virtuális gép létrehozása](#create-azure-virtual-machine-using-premium-storage)

Virtuális gépek áttelepítése a más platformok tooAzure prémium szintű Storage, vagy meglévő Azure virtuális gépek áttelepítésére Standard tárolási tooPremium tároló. Ez az útmutató mindkét két forgatókönyv lépéseit ismerteti. A forgatókönyvtől függően hello vonatkozó szakaszában megadott hello lépéseit kövesse.

> [!NOTE]
> A szolgáltatás áttekintése és a prémium szintű Storage, a prémium szintű Storage árképzési található: [nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](storage-premium-storage.md). Azt javasoljuk, hogy minden virtuális gép lemezét magas IOPS tooAzure prémium szintű Storage igénylő hello legjobb teljesítmény érdekében az alkalmazás áttelepítése. Ha a lemez nem igényli a magas iops értéket, korlátozhatja a költségek megőrizve a szabványos tárolóban, a (merevlemezes HDD) meghajtók SSD-k helyett virtuálisgép-lemez adatokat tárolja.
>

Teljes egészében hello áttelepítési folyamat befejezése szükség lehet további műveletek előtt és után a jelen útmutatóban hello lépéseket. Például a virtuális hálózatok és a végpontok konfigurálásához, vagy a kód módosítása hello alkalmazásban, saját magát, és amely előfordulhat, hogy az alkalmazás bizonyos időre leállítást. Ezek a műveletek egyedi tooeach alkalmazás, és el kell végeznie ezeket hello ismertetett lépéseket: Ez az útmutató toomake hello teljes átmenet tooPremium, zökkenőmentes lehető tárolási együtt.

## <a name="plan-the-migration-to-premium-storage"></a>Hello áttelepítési tooPremium tárolás tervezése
Ez a szakasz biztosítja, hogy készen áll a toofollow hello áttelepítési cikkben leírt lépéseket, és segít toomake hello legjobb döntést a virtuális gép és a lemez típusok.

### <a name="prerequisites"></a>Előfeltételek
* Szüksége lesz egy Azure-előfizetés. Ha még nincs fiókja, létrehozhat egy hónapos [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/) előfizetés, vagy keresse fel [Azure díjszabása](https://azure.microsoft.com/pricing/) további lehetőségekért.
* PowerShell-parancsmagok tooexecute, szüksége lesz a hello Microsoft Azure PowerShell modul. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello telepítse pont és a telepítési utasításokat.
* Ha azt tervezi, toouse Azure virtuális gépeken futó a prémium szintű Storage, prémium szintű Storage képes a virtuális gépek toouse hello kell. Prémium szintű Storage képes a virtuális gépek a Standard és prémium szintű Storage lemezek is használhatók. Prémium szintű storage lemezek lesz jövőbeli hello további Virtuálisgép-típusokon érhető el. További információ az összes elérhető Azure virtuális lemez típusát és méretét: [virtuális gépek méretei](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Felhőszolgáltatások mérete](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Megfontolandó szempontok
Egy Azure virtuális gép támogatja a prémium szintű Storage több lemezt csatolni, hogy az alkalmazások too256 TB-nyi tárhelyre virtuális gépenként legfeljebb tartalmazhat. Prémium szintű Storage az alkalmazások egy második lemez adatátviteli sebessége virtuális gépenként a rendkívül alacsony késleltetésű az olvasási műveletek érhet 80000 iops-értéket (bemeneti/kimeneti műveletek száma másodpercenként) virtuális Gépet és 2000 MB. Virtuális gépek és a lemezek számos lehetősége van. Ez a szakasz az toohelp toofind olyan beállítás, amely a legjobban megfelel a számítási feladathoz.

#### <a name="vm-sizes"></a>A virtuális gépek mérete
hello Azure virtuális gép mérete specifikációk szereplő [virtuális gépek méretei](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a hello leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok hello teljesítményétől. Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális gép toodrive hello lemez forgalom.

#### <a name="disk-sizes"></a>Lemezméretek
Öt különböző típusú lemezek, amelyek együtt a virtuális Gépet, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok. Vegye figyelembe a működés felső korlátjának hello lemez kiválasztása a virtuális gép alapján az alkalmazás kapacitása, teljesítmény, méretezhetőség hello igényeinek, és a maximális tölti be.

| Prémium szintű lemezek típusa  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Lemezméret           | 128 GB| 512 GB| 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 
| IOPS-érték lemezenként       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Adattovábbítás lemezenként | 100 MB / s | 150 MB / s | 200 MB / s | 250 MB / s | 250 MB / s |

Attól függően, hogy a munkaterhelés annak eldöntése, hogy adatlemeznek a virtuális gép szükséges. Több állandó adatok lemezek tooyour VM csatolható. Ha szükséges, a is paritásos hello lemezek tooincrease hello kapacitást és teljesítményt hello kötet között. (Megtudhatja, mi lemez csíkozást [Itt](storage-premium-storage-performance.md#disk-striping).) Ha Ön paritásos prémium szintű Storage adatlemezek használata [tárolóhelyek][4], úgy kell beállítania, egyoszlopos használt lemezek. Ellenkező esetben hello teljes csíkozott hello kötet lehet a teljesítmény kisebb, mint a várt forgalom toouneven terjesztési miatt hello lemezek között. A Linux virtuális gépek használhatják hello *mdadm* segédprogram tooachieve hello azonos. Cikke [szoftver RAID konfigurálása Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részleteiről.

#### <a name="storage-account-scalability-targets"></a>Tárfiókra vonatkozó méretezhetőségi célok
Prémium szintű Storage-fiókok rendelkezik méretezhetőségi célok hozzáadása toohello a következő hello [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md). Ha az alkalmazás követelményeinek ennél nagyobb hello méretezhetőségi célok egyetlen tárfiók, hozza létre a az alkalmazás toouse több tárfiókot, és az adatok particionálása adott tárfiókok között.

| Teljes kapacitásával | A helyileg redundáns tárolás fiók teljes sávszélesség |
|:--- |:--- |
| Lemez kapacitás: 35TB<br />Pillanatkép-kapacitás: 10 TB |Too50 Gigabit / másodperc, a bejövő + kimenő mentése |

A prémium szintű Storage specifikációk bővebben hello, tekintse meg [méretezhetőséget és a prémium szintű Storage használatakor Performance Targets](storage-premium-storage.md#scalability-and-performance-targets).

#### <a name="disk-caching-policy"></a>Lemez gyorsítótárazási házirend
Alapértelmezés szerint a gyorsítótárazási házirend lemez van *írásvédett* az összes hello prémium adatlemezek, és *írható-olvasható* hello prémium operációsrendszer-lemez csatolni a virtuális gép toohello. A konfigurációs beállítás ajánlott tooachieve hello optimális teljesítménye az alkalmazás IOs-hez. Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el. a meglévő adatlemezek hello gyorsítótár beállításait is frissítve [Azure Portal](https://portal.azure.com) vagy hello *- HostCaching* hello paramétere *Set-AzureDataDisk* parancsmag.

#### <a name="location"></a>Hely
Jelölje ki a helyet, ahol a prémium szintű Azure Storage áll rendelkezésre. Lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást. Virtuális gépek található hello azonos módon, hogy a tárolja a virtuális gép hello ad sok különböző régiókban azok jobb teljesítményt hello lemezeket a tárfiók hello régióban.

#### <a name="other-azure-vm-configuration-settings"></a>Egyéb Azure virtuális gép konfigurációs beállításai
Egy Azure virtuális gép létrehozásakor meg kell adnia tooconfigure egyes virtuális gép beállításait. Ne feledje, hogy néhány beállításainak rögzítése a virtuális gép, hello hello élettartama során módosítása, vagy később fel más. Tekintse át a Azure virtuális gép konfigurációs beállítások, és győződjön meg arról, hogy ezek a munkaterhelés igényeihez toomatch megfelelően konfigurálva.

### <a name="optimization"></a>Optimalizálás
[Prémium szintű Storage: Nagy teljesítményű kialakítása](storage-premium-storage-performance.md) útmutatást nyújt a prémium szintű Azure Storage használatával nagy teljesítményű alkalmazások létrehozásához. Teljesítmény bevált gyakorlatok alkalmazható tootechnologies az alkalmazás által használt együtt hello irányelvek követésével.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Készítse elő, és másolja a virtuális merevlemezek (VHD) tooPremium tároló
a következő szakasz hello előkészítése virtuális merevlemezek a virtuális gépről, és másolja a VHD-k tooAzure tárolási útmutatásokat.

* [1. forgatókönyv: "I vagyok áttelepíti meglévő Azure virtuális gépek tooAzure prémium szintű Storage."](#scenario1)
* [2. forgatókönyv: "I vagyok telepít virtuális gépeket más platformokon tooAzure prémium szintű Storage."](#scenario2)

### <a name="prerequisites"></a>Előfeltételek
virtuális merevlemezek tooprepare hello az áttelepítéshez, szüksége lesz:

* Azure-előfizetéssel, a tárfiók és egy tárolóját, hogy a tárolási fiók toowhich másolhatja a VHD-t. Vegye figyelembe, hogy a cél tárfiókkal hello lehet-e a Standard vagy prémium szintű Storage fiók igényektől függően.
* Egy eszköz toogeneralize hello VHD-t, ha azt tervezi, toocreate több Virtuálisgép-példányok belőle. Például a Windows vagy adatb-sysprep Ubuntu a sysprep.
* Egy eszköz tooupload hello VHD fájl toohello tárfiók. Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md) , vagy használjon egy [Azure Tártallózó](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Ez az útmutató ismerteti, hogy a virtuális merevlemez hello AzCopy eszközzel másolása.

> [!NOTE]
> A lehetőséghez szinkron másolatot az AzCopy, az optimális teljesítmény érdekében másolja a VHD-való futtatásával az eszközöket egy Azure virtuális Gépen, amely hello hello cél tárfiókkal megegyező régióban. Virtuális merevlemez másolása az Azure virtuális gép egy másik régióban található, a teljesítmény csökkenhet.
>
> Nagy mennyiségű adatot felülírását korlátozott sávszélességű, fontolja meg [hello Azure Import/Export szolgáltatás tootransfer adatok tooBlob Storage használatával](../storage-import-export-service.md); ez tootransfer lehetővé teszi az adatok szállítási merevlemez-meghajtók tooan Azure Datacenter. Hello Azure Import/Export szolgáltatás toocopy adatok tooa standard szintű tárfiók csak is használhatja. Ha a standard szintű tárfiók hello adatok, vagy hello használhatja [másolási Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) vagy AzCopy tootransfer hello adatok tooyour prémium szintű storage-fiók.
>
> Ügyeljen arra, hogy a Microsoft Azure csak támogatja-e a rögzített méretű VHD-fájlokat. A VHDX-fájlok vagy a dinamikus VHD-k nem támogatottak. Ha egy dinamikus VHD-t, akkor átalakíthatja toofixed méretét hello segítségével [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) parancsmag.
>
>

### <a name="scenario1"></a>1. forgatókönyv: "I vagyok áttelepíti meglévő Azure virtuális gépek tooAzure prémium szintű Storage."
Ha az áttelepítés meglévő Azure virtuális gépek, virtuális gépek leállítási hello készítse elő a virtuális merevlemezek hello típusonkénti kívánt VHD-fájl, és másolja hello VHD AzCopy vagy a PowerShell használatával.

hello VM kell toobe toomigrate tiszta le teljesen. Lesz olyan állásidő hello áttelepítésének befejeződéséig.

#### <a name="step-1-prepare-vhds-for-migration"></a>1. lépés Virtuális merevlemezek Felkészülés az áttelepítésre
Ha a meglévő Azure virtuális gépek tooPremium tárolási telepít át, a virtuális merevlemez lehet:

* Általános operációsrendszer-lemezkép elkészítése
* Egy egyedi operációsrendszer-lemez
* Adatlemez

Az alábbiakban azt végezze el a virtuális merevlemez előkészítése 3 forgatókönyvekben.

##### <a name="use-a-generalized-operating-system-vhd-toocreate-multiple-vm-instances"></a>Egy általános operációs rendszer virtuális merevlemez toocreate több Virtuálisgép-példány használata
Ha virtuális Merevlemezt, amely lesz használt toocreate több általános Azure Virtuálisgép-példányok, először meg kell generalize virtuális Merevlemezt a sysprep segédprogrammal. Ez vonatkozik, amely a helyi virtuális merevlemez tooa vagy hello felhőben. Sysprep eltávolítása hello VHD-t bármely gépen-specifikus adatait.

> [!IMPORTANT]
> Készítsen pillanatképet, vagy biztonsági mentése a virtuális gép előtt normalizálása azt. A sysprep fut le fog állni, és hello Virtuálisgép-példány felszabadítani. Kövesse az alábbi Windows operációs rendszer virtuális merevlemez toosysprep lépéseket. Vegye figyelembe, hogy hello a Sysprep parancsot futtatja fogja a szükséges tooshut le hello virtuális gépet. További információ a Sysprep: [Sysprep áttekintése](http://technet.microsoft.com/library/hh825209.aspx) vagy [Sysprep műszaki útmutatója](http://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Nyisson meg egy parancssori ablakot rendszergazdaként.
2. Adja meg a következő parancs tooopen Sysprep hello:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. A hello rendszer-előkészítő eszközt, jelölje be adja meg a rendszer Out-of-Box élmény (OOBE), jelölje be hello Generalize jelölőnégyzetet, válassza ki **leállítási**, és kattintson a **OK**, az alábbi hello ábrán látható módon. Sysprep általánosítja hello operációs rendszert, és a hello rendszer leállítása.

    ![][1]

Egy Ubuntu virtuális gép használata a sysprep adatb tooachieve hello azonos. Lásd: [adatb-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) további részleteket. További információ az egyes hello nyílt forráskódú [Linux Server kiépítés szoftver](http://www.cyberciti.biz/tips/server-provisioning-software.html) más Linux operációs rendszerekhez.

##### <a name="use-a-unique-operating-system-vhd-toocreate-a-single-vm-instance"></a>Egy egyedi operációs rendszer virtuális merevlemez toocreate egyetlen Virtuálisgép-példány használata
Ha hello hello gép adatokat igénylő virtuális gép futó alkalmazást, nem generalize hello VHD-t. Nem általánosított virtuális Merevlemezt lehet használt toocreate egy egyedi Azure Virtuálisgép-példány. Például ha a tartományvezérlő van a VHD-t, sysprep végrehajtása megkönnyítő hatástalan tartományvezérlőként. Tekintse át a virtuális gép és hello hatása rajtuk sysprep futtatása előtt hello VHD normalizálása futó hello alkalmazásokhoz.

##### <a name="register-data-disk-vhd"></a>Virtuális merevlemez adatlemeze regisztrálása
Ha rendelkezik Azure toobe adatlemezek át, győződjön meg arról, hogy hello virtuális gépek, amelyek használják a lemezek állnak le ezeket az adatokat.

Kövesse az alábbiakban toocopy VHD tooAzure prémium szintű Storage hello lépéseket, és regisztrálja kiosztott adatok lemezként.

#### <a name="step-2-create-hello-destination-for-your-vhd"></a>2. lépés Hello cél a virtuális merevlemez létrehozása
Hozzon létre egy tárfiókot, a virtuális merevlemezek karbantartásához. Vegye figyelembe a where megtervezésekor a következő pontok hello toostore merevlemezek:

* hello célként prémium szintű storage-fiók.
* hello tárfiók helyének meg kell egyeznie a prémium szintű Storage kompatibilis Azure virtuális gépek létrehozhat hello utolsó szakaszában. Új tárfiók tooa vagy terv toouse hello igényei szerint ugyanazt a tárfiókot nem átmásolni.
* Másolja ki és mentse a hello tárfiók kulcsa hello cél tárfiók hello következő szakaszra.

Adatlemezek választhatja ki tookeep néhány adatlemezek egy standard szintű tárfiókot (például lemezek hűtőre tárhellyel rendelkező), de határozottan javasoljuk, hogy áthelyezése éles munkaterhelés toouse prémium szintű storage összes adatát.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>3. lépés. Másolja a VHD AzCopy vagy a PowerShell használatával
A tároló elérési útja és a tárolási fiók kulcs tooprocess valamelyik két lehetőséget toofind lesz szüksége. Tároló elérési útja és a tároló kulcsa megtalálható **Azure Portal** > **tárolási**. hello tároló URL-címe például a "https://myaccount.blob.core.windows.net/mycontainer/" lesz.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>1. lehetőség: Az AzCopy (aszinkron másolhatja azokat) egy virtuális merevlemez másolása
Használja az AzCopy, egyszerűen feltöltheti hello VHD hello interneten keresztül. Attól függően, hogy a virtuális merevlemezek hello hello méretét Ez időt vehet igénybe. Ne felejtse el toocheck hello tárfiókok be-és kilépési korlátai, ha ezt a lehetőséget választja. Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) részleteiről.

1. Töltse le és telepítse az AzCopy innen: [az AzCopy legújabb verzióját](http://aka.ms/downloadazcopy)
2. Nyissa meg az Azure PowerShell és amelyen telepítve van-e az AzCopy lépjen toohello mappát.
3. Használjon hello következő parancsot a toocopy hello VHD-fájlt a "Forrás" túl "Cél".

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Példa:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    Az alábbiakban az AzCopy parancs hello használt hello paraméterek leírását:

   * **/ Forrás:  *&lt;forrás&gt;:***  hello mappa vagy hello VHD-t tartalmazó tároló tárolói URL.
   * **/ SourceKey:  *&lt;forrás fiókkulcs&gt;:***  hello forrás tárfiók Tárfiók kulcsa.
   * **/ Dest:  *&lt;cél&gt;:***  tárolási tároló URL-cím toocopy hello VHD-fájlt.
   * **/ DestKey:  *&lt;cél fiókkulcs&gt;:***  hello céltárfiókot a Tárfiók kulcsára.
   * **/ Mintát:  *&lt;Fájlnév&gt;:***  hello hello VHD toocopy a fájl nevének megadása.

AzCopy használatával eszköz, lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>2. lehetőség: A PowerShell használatával (Synchronized másolás) virtuális merevlemez másolása
PowerShell-parancsmaggal hello Start-AzureStorageBlobCopy hello VHD-fájlt is másolhatja. Használja a következő parancs az Azure PowerShell toocopy VHD hello. Cserélje le a <> hello értékei a forrás és cél tárfiók megfelelő értékeivel. Ez a parancs, rendelkeznie kell a cél tárfiókkal nevezett VHD-k tárolója toouse. Ha hello tároló nem létezik, hozzon létre egyet hello parancs futtatása előtt.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Példa:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>2. forgatókönyv: "I vagyok telepít virtuális gépeket más platformokon tooAzure prémium szintű Storage."
Ha a virtuális merevlemez nem Azure felhőalapú tárolást tooAzure telepít, először exportálnia kell hello VHD tooa helyi könyvtárba. Hello teljes forrás könyvtár elérési útja hello helyi virtuális merevlemez tároló lesz szüksége van, és akkor használja az AzCopy tooupload azt tooAzure tároló.

#### <a name="step-1-export-vhd-tooa-local-directory"></a>1. lépés Exportálja a virtuális merevlemez tooa helyi könyvtár
##### <a name="copy-a-vhd-from-aws"></a>Másolja a VHD-t AWS
1. Ha AWS használ, exportálja a hello EC2 példány tooa az Amazon S3 gyűjtő VHD-n. Hello exportálása Amazon EC2 példányok tooinstall hello Amazon EC2 parancssori felület (CLI) eszköz Amazon dokumentációjában leírt hello lépésekkel, és futtassa a hello-példány-export-feladat létrehozása parancs tooexport hello EC2 példány tooa VHD-fájlt. Lehet, hogy toouse **VHD** hello lemez &#95; kép &#95; Hello futtatásakor formátum változó **-példány-export-feladat létrehozása** parancsot. hello exportált VHD-fájl kerül, a folyamat során megadott hello Amazon S3 gyűjtő.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. Hello S3 gyűjtő hello VHD-fájl letöltését. Jelölje be hello VHD-fájlt, majd **műveletek** > **letöltése**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Másolja a VHD-t más-Azure felhő
Ha a virtuális merevlemez nem Azure felhőalapú tárolást tooAzure telepít, először exportálnia kell hello VHD tooa helyi könyvtárba. Másolja a hello teljes forrás könyvtár elérési útja hello helyi virtuális merevlemez tárolásához.

##### <a name="copy-a-vhd-from-on-premises"></a>Másolja a VHD-t a helyszíni
Ha a VHD-t a helyszíni környezetben telepít, szüksége lesz a hello teljes forrás elérési útja a virtuális merevlemez tárolásához. hello forrás elérési útja egy helyen vagy kiszolgálómegosztás lehet.

#### <a name="step-2-create-hello-destination-for-your-vhd"></a>2. lépés Hello cél a virtuális merevlemez létrehozása
Hozzon létre egy tárfiókot, a virtuális merevlemezek karbantartásához. Vegye figyelembe a where megtervezésekor a következő pontok hello toostore merevlemezek:

* hello cél tárfiók lehet standard vagy prémium szintű storage, attól függően, hogy az alkalmazás követelményeinek.
* hello tárfiók régiója meg kell egyeznie a prémium szintű Storage kompatibilis Azure virtuális gépek létrehozhat hello utolsó szakaszában. Új tárfiók tooa vagy terv toouse hello igényei szerint ugyanazt a tárfiókot nem átmásolni.
* Másolja ki és mentse a hello tárfiók kulcsa hello cél tárfiók hello következő szakaszra.

Határozottan javasoljuk, áthelyezése éles munkaterhelés toouse prémium szintű storage összes adatát.

#### <a name="step-3-upload-hello-vhd-tooazure-storage"></a>3. lépés Hello VHD tooAzure tárolási feltöltése
Most, hogy a virtuális merevlemez hello helyi könyvtárban, az AzCopy vagy AzurePowerShell tooupload hello .vhd fájl tooAzure tárolási is használhatja. Mindkét lehetőség itt találhatók:

##### <a name="option-1-using-azure-powershell-add-azurevhd-tooupload-hello-vhd-file"></a>1. lehetőség: Az Azure PowerShell Add-AzureVhd tooupload hello .vhd fájl használata

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Példa <Uri> előfordulhat, hogy ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***. Példa <FileInfo> előfordulhat, hogy ***"C:\path\to\upload.vhd"***.

##### <a name="option-2-using-azcopy-tooupload-hello-vhd-file"></a>2. lehetőség: Az AzCopy tooupload hello .vhd fájl használata
Használja az AzCopy, egyszerűen feltöltheti hello VHD hello interneten keresztül. Attól függően, hogy a virtuális merevlemezek hello hello méretét Ez időt vehet igénybe. Ne felejtse el toocheck hello tárfiókok be-és kilépési korlátai, ha ezt a lehetőséget választja. Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) részleteiről.

1. Töltse le és telepítse az AzCopy innen: [az AzCopy legújabb verzióját](http://aka.ms/downloadazcopy)
2. Nyissa meg az Azure PowerShell és amelyen telepítve van-e az AzCopy lépjen toohello mappát.
3. Használjon hello következő parancsot a toocopy hello VHD-fájlt a "Forrás" túl "Cél".

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Példa:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    Az alábbiakban az AzCopy parancs hello használt hello paraméterek leírását:

   * **/ Forrás:  *&lt;forrás&gt;:***  hello mappa vagy hello VHD-t tartalmazó tároló tárolói URL.
   * **/ SourceKey:  *&lt;forrás fiókkulcs&gt;:***  hello forrás tárfiók Tárfiók kulcsa.
   * **/ Dest:  *&lt;cél&gt;:***  tárolási tároló URL-cím toocopy hello VHD-fájlt.
   * **/ DestKey:  *&lt;cél fiókkulcs&gt;:***  hello céltárfiókot a Tárfiók kulcsára.
   * **/ BlobType: lap:** határozza meg, hogy hello cél oldalakra vonatkozó blob.
   * **/ Mintát:  *&lt;Fájlnév&gt;:***  hello hello VHD toocopy a fájl nevének megadása.

AzCopy használatával eszköz, lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Más beállításokat a virtuális merevlemez feltöltése
Feltöltheti a virtuális merevlemez tooyour tárfiók azt jelenti, hogy a következő hello egyikének használatával:

* [Az Azure Storage másolási Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Az Azure Storage Explorer feltöltése a BLOB](https://azurestorageexplorer.codeplex.com/)
* [Storage Import/Export szolgáltatás REST API-referencia](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> Ajánlott Import/Export szolgáltatás használata, ha becsült a 7 napnál hosszabb idő feltöltése. Használhat [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) adatok méretét és átviteli egység tooestimate hello időpontját.
>
> Importálási/exportálási lehet toocopy tooa standard szintű tárfiókot használja. Standard szintű tárolást toopremium tárkonfigurációt AzCopy hasonló eszköz használatával toocopy kell.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Prémium szintű Storage használata Azure virtuális gépek létrehozása
Miután hello VHD szükséges feltöltött vagy másolt toohello tárfiókot, ez a szakasz tooregister hello VHD hello utasításait kövesse az operációsrendszer-lemezképek, vagy a forgatókönyvtől függően az operációsrendszer-lemez, és egy Virtuálisgép-példány készítése. hello adatlemez VHD lehet csatolt toohello virtuális gép létrehozása után.
Ez a szakasz hello végén egy áttelepítési parancsfájlt valósul meg. Ez egyszerű parancsprogram nem felel meg minden forgatókönyvben. Az adott forgatókönyv szükség lehet tooupdate hello parancsfájl toomatch. Ha ezt a parancsfájlt érvényes tooyour esetben toosee lásd az alábbi [A Parancsfájlpéldát áttelepítési](#a-sample-migration-script).

### <a name="checklist"></a>Feladatlista
1. Várja meg, amíg az összes hello VHD lemezek másolása befejeződött.
2. Ellenőrizze, hogy hello régió áttelepítés prémium szintű Storage áll rendelkezésre.
3. Döntse el, hello új Virtuálisgép-sorozat fog használni. Egy prémium szintű Storage képes legyen, és hello mérete attól függően, hello rendelkezésre állási hello régióban és igényei alapján kell.
4. Döntse el, hello pontos Virtuálisgép-méretet fogja használni. Virtuálisgép-méretet kell toobe elég nagy toosupport hello rendelkezik adatlemezek száma. Például Ha 4 adatlemezek, hello VM 2 vagy több maggal kell rendelkeznie. Fontolja meg is, a feldolgozási kapacitása, memória, és a hálózati sávszélesség igényeinek megfelelően.
5. Prémium szintű Storage-fiók létrehozása hello cél régióban. A rendszer a hello fiókot használja-e hello új virtuális Gépet.
6. Hello aktuális virtuális gép adatai lesz szüksége, beleértve a lemezek és a megfelelő VHD-blobok hello listája rendelkezik.

Készítse elő az állásidő alkalmazását. egy tiszta áttelepítési toodo, vannak toostop összes hello feldolgozási hello aktuális rendszer. Csak ezután beszerezheti tooconsistent állapotát, amely áttelepíthető toohello új platformon. Állásidő időtartama hello adatmennyiséget a hello lemezek toomigrate függ.

> [!NOTE]
> Az Azure Resource Manager virtuális gép speciális VHD lemez létrehozásakor, tekintse meg túl[sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) erőforrás-kezelő virtuális gépet a meglévő lemezt telepítéséhez.
>
>

### <a name="register-your-vhd"></a>A virtuális merevlemez regisztrálása
a virtuális gép operációs rendszer virtuális Merevlemezt vagy egy adatok lemez tooa tooattach toocreate új virtuális Gépet, először regisztrálnia kell őket. Kövesse az alábbi lépéseket attól függően, hogy a VHD-forgatókönyv.

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a>Operációs rendszer virtuális merevlemez toocreate általánosítva több Azure Virtuálisgép-példányok
Az operációsrendszer-lemezképek általánosított virtuális merevlemez feltöltése toohello tárfiókot, után regisztrálja azt egy **Azure Virtuálisgép-lemezkép** , hogy egy vagy több Virtuálisgép-példányok hozhat létre belőle. A következő PowerShell-parancsmagok tooregister hello a VHD-t használja egy Azure virtuális gép operációsrendszer-lemezképben. Adja meg a hello teljes tároló URL-CÍMÉT adott VHD lett másolva.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Másolja ki és mentse az új Azure Virtuálisgép-lemezkép hello nevét. Hello a fenti példában az is *OSImageName*.

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a>Egyedi operációs rendszer virtuális merevlemez toocreate egyetlen Azure Virtuálisgép-példány
Hello után egyedi az operációs rendszer virtuális merevlemez az feltöltött toohello tároló fiókot, regisztrálja őket, mint egy **Azure operációsrendszer-lemez** , hogy egy Virtuálisgép-példányt hozhat létre belőle. A virtuális merevlemez az alábbi PowerShell-parancsmagok tooregister használja Azure operációsrendszer-lemezként. Adja meg a hello teljes tároló URL-CÍMÉT adott VHD lett másolva.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Másolja ki és mentse az új Azure operációsrendszer-lemez hello nevét. Hello a fenti példában az is *OSDisk*.

#### <a name="data-disk-vhd-toobe-attached-toonew-azure-vm-instances"></a>Adatok lemez VHD toobe csatolt toonew Azure Virtuálisgép-példányokat
Hello virtuális merevlemez adatlemeze feltöltése után toostorage fiók, regisztrálja egy Azure-adatlemez, így azok csatolt tooyour új DS méretek, DSv2-méretek és GS adatsorozat Azure Virtuálisgép-példány.

A PowerShell-parancsmagok tooregister a VHD-t használja az Azure-adatlemez. Adja meg a hello teljes tároló URL-CÍMÉT adott VHD lett másolva.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Másolja ki és mentse az új Azure-adatlemez hello nevét. Hello a fenti példában az is *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Prémium szintű Storage képes a virtuális gép létrehozása
Az operációsrendszer-lemezképek egyszer hello vagy operációsrendszer-lemez van regisztrálva, hozzon létre egy új DS-méretek, DSv2-sorozat vagy GS sorozatnak virtuális gép. T hello operációs rendszeri lemezkép vagy operációs rendszer Lemeznév regisztrált fog használni. Válassza ki a Virtuálisgép-típussá hello hello prémium szintű Storage réteg alapján. Az alábbi példában használjuk hello *Standard_DS2* Virtuálisgép-méretet.

> [!NOTE]
> Frissítés hello lemez mérete toomake meg arról, hogy megegyezzen a kapacitás és a teljesítményre vonatkozó követelmények és a hello Azure rendelkezésre álló lemezterület méretét.
>
>

Hajtsa végre hello lépésről lépésre PowerShell-parancsmagok alatt toocreate hello új virtuális Gépet. Első lépésként állítsa be az általános paraméterek hello:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

Először hozzon létre egy felhőalapú szolgáltatás, amelyben, amelyen az új virtuális gépek.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Ezután a forgatókönyvtől függően hozzon létre hello Azure Virtuálisgép-példány vagy hello operációsrendszer-lemezképek vagy regisztrált operációsrendszer-lemezt.

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a>Operációs rendszer virtuális merevlemez toocreate általánosítva több Azure Virtuálisgép-példányok
Hozzon létre egy vagy több új DS adatsorozat Azure Virtuálisgép-példányok hello használata az hello **Azure operációsrendszer-lemezképek** regisztrált. Adja meg a operációsrendszer-lemezképek nevét hello Virtuálisgép-konfiguráció, amikor új virtuális gép létrehozása az alább látható módon.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a>Egyedi operációs rendszer virtuális merevlemez toocreate egyetlen Azure Virtuálisgép-példány
Hozzon létre új DS adatsorozat Azure virtuális példányt hello segítségével **Azure operációsrendszer-lemez** regisztrált. Adja meg az operációs rendszer lemezének neve hello Virtuálisgép-konfiguráció, amikor új virtuális gép létrehozása hello alább látható módon.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Adjon meg más Azure Virtuálisgép-adatok, például egy felhőalapú szolgáltatás, régió, tárfiókot, a rendelkezésre állási csoport és gyorsítótárazási házirend. Vegye figyelembe, hogy hello Virtuálisgép-példány közös elhelyezésű társított operációs rendszerrel kell lennie, vagy adatlemezek, így hello kijelölt felhőalapú szolgáltatás, valamint régió és tárolási fiók kell lenniük hello az alapul szolgáló virtuális merevlemezek lemezek hello és ugyanazon a helyen.

### <a name="attach-data-disk"></a>Adatlemez csatolása
Végül, ha regisztrált adatok lemez VHD-k, csatolja őket toohello új prémium szintű Storage kompatibilis Azure virtuális Gépen.

Használja a következő PowerShell parancsmagot tooattach adatok lemez toohello új virtuális Gépet, és adja meg a gyorsítótárazási házirend hello. Hello az alábbi példában a gyorsítótárazási házirend értéke túl*ReadOnly*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Előfordulhat, hogy további lépéseket szükséges toosupport az alkalmazás, amely nem elegendő az útmutatóban.
>
>

### <a name="checking-and-plan-backup"></a>Ellenőrzése és a biztonsági mentés tervezése
Egyszer hello új virtuális gép működik-e és fut, a hozzáférés segítségével hello azonos felhasználónév és jelszó megegyezik az eredeti virtuális gép hello, és győződjön meg arról, hogy minden a várt módon működik. Összes hello beállításának, köztük a hello csíkozott kötetek jelen lehet új virtuális gép hello.

hello utolsó lépése tooplan biztonsági mentésének és karbantartásának ütemezése hello hello alkalmazásnak alapuló új virtuális gép.

### <a name="a-sample-migration-script"></a>Egy áttelepítési parancsfájlt
Ha több virtuális gépek toomigrate, automatizálás PowerShell-parancsprogramok hasznos lehet. Az alábbiakban látható egy minta parancsfájlt, amely automatizálja a virtuális gép hello áttelepítését. Megjegyzés: alább parancsfájl, amely csak egy példa, és nincsenek hello aktuális virtuális gép lemezeivel kapcsolatos tett néhány feltételezéseket. Az adott forgatókönyv szükség lehet tooupdate hello parancsfájl toomatch.

hello Előfeltételek a következők:

* Klasszikus Azure virtuális gépek létrehozásakor.
* A forrás operációs rendszer és a forrás adatok lemezek vannak ugyanabban a tárfiókban és az ugyanabban a tárolóban. Ha az operációs rendszer és a lemezek adatok nincsenek az azonos hello helyezze, használhatja a storage-fiókok és a tárolók AzCopy vagy az Azure PowerShell toocopy virtuális merevlemezeket. Tekintse meg az előző lépésben toohello: [másolási VHD AzCopy vagy a PowerShell használatával](#copy-vhd-with-azcopy-or-powershell). A parancsfájl toomeet szerkesztése adott esetben más választási lehetőség, de azt javasoljuk, mert az egyszerűbb és gyorsabb AzCopy vagy a PowerShell használatával.

hello automatizálási parancsfájl lejjebb tekinthetők meg. Szöveg cseréje az adatait, és frissítse az Ön konkrét forgatókönyvei hello parancsfájl toomatch.

> [!NOTE]
> Hello meglévő parancsfájl használatával nem őrzi meg a virtuális gép – forrásként hello hálózati konfigurációját. Toore-config hello hálózati beállításokat kell az áttelepített virtuális gépeken.
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE tooshow how toomigrate a VM from a standard storage account tooa premium storage account. You can customize it according tooyour specific requirements.

    .Description
    hello script will copy hello vhds (page blobs) of hello source VM toohello new storage account.
    And then it will create a new VM from these copied vhds based on hello inputs that you specified for hello new VM.
    You can modify hello script toosatisfy your specific requirement, but please be aware of hello items specified
    in hello Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED toohello IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. hello ENTIRE RISK OF USE, INABILITY tooUSE, OR
    RESULTS FROM hello USE OF THIS CODE REMAINS WITH hello USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    toofind more information about how tooset up Azure PowerShell, refer toohello following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # hello cloud service name of hello VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # hello VM name toocopy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # hello destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # hello destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # hello destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # hello destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # hello location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not toocopy hello os disk, hello default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently tooreport hello copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # hello name suffix tooadd toonew created disks tooavoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import hello Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check hello Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer toothis article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ tooconnect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if hello VM is shut down
    #  Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - hello source VM doesn't exist in hello current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation. Azure does not support live migration at this time. If you'd like toocreate a VM from a generalized image, sys-prep hello Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish toostop $SourceVMName now? Input 'N' if you want tooshut down hello VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until hello VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName tooshut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting hello sourve vm tooa configuration file, you can restore hello original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration too$vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy hello vhds of hello source vm
    #  You can choose toocopy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly toobe $false. hello default is toocopy only data disk vhds
    #  and hello new VM will boot from hello original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering hello data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in hello destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need toocopy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting hello source VM so that dest VM can boot
        # from hello same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose toocopy data disks only. Moving VM requires removing hello original VM (hello disks and backing vhd files will NOT be deleted) so that hello new VM can boot from hello same vhd. This is an irreversible action. Do you wish tooproceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing hello original VM (hello vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill hello OS disk is released by source VM. This may take up tooseveral minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy hello os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update hello media link toopoint toohello target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy toocomplete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  hello new VM can be created from hello copied disks or hello original os disk.
    #  You can ddd your own logic here toosatisfy your specific requirements of hello vm.
    #######################################################################

    # Create a VM from hello existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached hello copied data disks toohello new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of hello source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want tooadd more custimization toohello new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>Optimalizálás
Az aktuális Virtuálisgép-konfigurációt testre szabható kifejezetten toowork jól igazodik a Standard lemezek. Például tooincrease hello teljesítmény sok csíkozott kötetek használatával. Például helyett 4 lemezek külön-külön a prémium szintű Storage, előfordulhat, hogy el tudja toooptimize hello költség azzal, hogy egyetlen lemez. Optimalizálás, például a kezelt eseti alapon kell toobe, és egyéni lépéseket igényelnek a hello áttelepítés után. Emellett vegye figyelembe, hogy ez a folyamat jól nem feltétlenül alkalmas adatbázisok és hello lemez elrendezése hello beállítása meghatározott függő alkalmazások.

##### <a name="preparation"></a>Előkészítése
1. Teljes hello egyszerű áttelepítési hello a korábbi szakaszban. Hello történik optimalizálás hello áttelepítés után új virtuális Gépet.
2. Adja meg a hello optimalizált hello konfigurálásához szükséges új lemez méretét.
3. Leképezési hello aktuális lemezek vagy kötetek toohello új lemez előírások határozza meg.

##### <a name="execution-steps"></a>Végrehajtási lépések
1. Hozzon létre hello új lemezek hello megfelelő méretek a prémium szintű Storage VM hello.
2. Bejelentkezési toohello virtuális gép, és másolja hello adatait hello kötet toohello új lemezhez, amely leképezhető toothat kötet. Ehhez az toomap tooa új lemez igénylő összes hello aktuális kötet.
3. A következő hello alkalmazás beállítások tooswitch toohello új lemezek módosítsa, majd hello régi kötetet leválasztani.

A lemez teljesítmény növelése érdekében hello alkalmazás hangolása, tekintse meg az túl[alkalmazások teljesítményének optimalizálása](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Alkalmazás-áttelepítések
Adatbázisok és más összetett alkalmazások lehet szükség különleges lépések hello alkalmazás szolgáltató hello áttelepítéshez NSA. Toorespective alkalmazás dokumentációjában tájékozódhat. Például általában adatbázisok telepíthetők át a biztonsági mentés és visszaállítás.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő virtuális gépek meghatározott forgatókönyvek erőforrásait hello:

* [Az Azure virtuális gépek közötti Storage-fiókok áttelepítése](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Hozzon létre, és töltse fel a Windows Server VHD tooAzure.](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Létrehozásával, majd ismét feltölteni a virtuális merevlemez a Contains hello Linux operációs rendszer](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Amazon AWS tooMicrosoft Azure virtuális gépek áttelepítése](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

A következő erőforrások toolearn további információk az Azure Storage és az Azure virtuális gépek hello lásd még:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Az Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
