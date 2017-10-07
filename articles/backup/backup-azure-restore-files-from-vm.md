---
title: "Az Azure Backup: Fájlok és mappák helyreállítása Azure virtuális gép biztonsági másolaton |} Microsoft Docs"
description: "Egy Azure virtuális gép helyreállítási pontból állítsa helyre a fájlok"
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: "elemszintű helyreállítás; a fájlok helyreállítása Azure virtuális gép biztonsági másolatból; Azure virtuális gép fájlok visszaállítása"
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pullabhk;markgal
ms.openlocfilehash: 1a62a0ed83d61272c032ac0377a54099ed118db4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Fájlok helyreállítása Azure virtuális gép biztonsági mentése

Azure biztonsági mentés biztosít hello funkció toorestore [Azure virtuális gépek és a lemezek](./backup-azure-arm-restore-vms.md) Azure virtuális gép biztonsági másolatból. Most már a cikk azt ismerteti, hogyan helyreállítható elemek, például fájlok és mappák egy Azure virtuális gép biztonsági mentése.

> [!Note]
> Ez a szolgáltatás Azure virtuális gépeken telepített hello Resource Manager modellt és a Recovery services-tároló védett tooa érhető el.
> A fájlok helyreállítása titkosított virtuális gép biztonsági másolaton nem támogatott.
>

## <a name="mount-hello-volume-and-copy-files"></a>Csatlakoztassa a hello kötet, és másolja a fájlokat

1. Jelentkezzen be a hello [Azure-portálon](http://portal.Azure.com). Hello vonatkozó Recovery services-tároló és a szükséges hello biztonsági mentési elem található.

2. Hello biztonsági másolati elem paneljén kattintson **a fájlok helyreállítása**

    ![Biztonsági mentési elem megnyitása Recovery Services tároló](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    Hello **fájlhelyreállítás** panel nyílik meg.

    ![Fájl-helyreállítási panel](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. A hello **válassza ki a helyreállítási pont** legördülő menüben válassza hello helyreállítási pont hello fájlokat tartalmazó szeretné. Hello legújabb helyreállítási pont alapértelmezés szerint ki van jelölve.

4. Kattintson a **végrehajtható fájl letöltése** (a Windows Azure virtuális gép) vagy **parancsfájl letöltése** (az Azure virtuális gépekhez Linux) toodownload hello szoftver azt ismertetjük toocopy fájlok hello helyreállítási pontból.

  hello végrehajtható fájl, parancsfájl kapcsolatot hoz létre hello helyi számítógép és a megadott hello helyreállítási pontot.

5. A jelszó toorun letöltött hello parancsfájl vagy végrehajtható fájl van szüksége. Hello jelszó átmásolhatja hello portal hello Másolás gombbal mellett hello létrehozott jelszó

    ![Létrehozott jelszót](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

6. Ha azt szeretné, hogy toorecover hello fájlok hello számítógépen futtassa a hello végrehajtható fájl, parancsfájl. Meg kell azt futtatnia a rendszergazdai hitelesítő adataival. Ha hello parancsfájl korlátozott hozzáféréssel rendelkező számítógépen futtatja, győződjön meg arról, érhető el:

    - jövőben a Microsoft
    - Az Azure virtuális gép biztonsági mentések használt Azure-végpontok
    - 3260-as kimenő port

   A Linux, hello parancsfájlhoz szükséges a "Megnyitás-iscsi" és "lshw" összetevők tooconnect toohello helyreállítási pontot. Ha azok nem léteznek hello gépen, amelyben fut, az engedély tooinstall hello releváns összetevők kér, és telepíti azokat jóváhagyását követően.
   
   Írja be a másolt hello portálról, ha a rendszer kéri hello jelszót. Miután hello érvényes jelszót is meg kell adni a hello parancsfájlok toohello helyreállítási pontot kapcsolja össze.
      
    ![Fájl-helyreállítási panel](./media/backup-azure-restore-files-from-vm/executable-output.png)
    
   
   Hello parancsprogrammal bármelyik olyan gépen, amelyben hello azonos (vagy kompatibilis) operációs rendszer hello biztonsági másolat virtuális gép. Lásd: hello [kompatibilis operációs rendszer tábla](backup-azure-restore-files-from-vm.md#compatible-os) kompatibilis operációs rendszerekhez. Ha hello védett Azure virtuális gép által használt Windows tárolóhelyek (a Windows Azure virtuális gépeken) vagy LVM/RAID Arrays(for Linux VMs), akkor nem futtatható hello végrehajtható fájl, parancsfájl hello ugyanahhoz a virtuális géphez. Ehelyett futtassa bármely más kompatibilis operációs rendszerrel rendelkező gép.

### <a name="compatible-os"></a>Kompatibilis operációs rendszer

#### <a name="for-windows"></a>Windows rendszerhez

hello a következő táblázat azt mutatja be hello kompatibilitási kiszolgáló és a számítógép operációs rendszerek között. Ha a fájlok helyreállítása nem kompatibilis operációs rendszerek közötti fájlok nem állítható vissza.

|Kiszolgáló OS | Kompatibilis ügyfél OS  |
| --------------- | ---- |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

#### <a name="for-linux"></a>A Linux rendszerhez

A Linux, hello alapvető mérete, hogy az operációs rendszer hello hello gépen, ahol hello parancsfájlt futtatja a hello filesystem támogatnia kell a hello hello található fájlokat a biztonsági mentésben Linux virtuális gép. Közben gép toorun hello parancsfájl választja, győződjön meg arról hello az alábbi táblázat a hello kompatibilis operációs rendszer és hello verziója van.

|A Linux operációs rendszer | Verziók  |
| --------------- | ---- |
| Ubuntu | 12.04 vagy újabb verzió |
| CentOS | 6.5-ös vagy újabb verzió  |
| RHEL | 6.7 vagy újabb verzió |
| Debian | 7 vagy újabb verzió |
| Oracle Linux | 6.4 vagy újabb verzió |

hello parancsfájlt is szükséges python és összetevők tooexecute bash, és csatlakoztassa biztonságosan toohello helyreállítási pontot.

|Összetevő | Verzió  |
| --------------- | ---- |
| Bash | 4 vagy újabb verzió |
| python | 2.6.6 vagy újabb verzió  |


### <a name="identifying-volumes"></a>Kötetek azonosítása

#### <a name="for-windows"></a>Windows rendszerhez

Hello exectuable futtatásakor hello operációs rendszer hello új kötetek csatlakoztatja, és hozzárendeli a meghajtóbetűjelet. A Windows Explorer vagy a Fájlkezelőben toobrowse e-meghajtókat használhatják. hello meghajtó-betűjelek toohello kötetek nem lehet hello azonos betűket, hello eredeti virtuális gép, azonban hello kötet neve megőrződik. Például ha hello hello eredeti virtuális gép kötet volt "adatlemez (E:\)", hogy a kötet csatolható "adatlemez ("Bármely meghajtóbetűjelet érhető el":\) hello helyi számítógépen. Tallózzon a hello parancsfájl kimenetében szerepel, amíg meg nem látja a fájl/mappa összes kötetet.  
       
   ![Fájl-helyreállítási panel](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>A Linux rendszerhez

A Linux hello hello helyreállítási pont fájlhelyekhez csatlakoztatott toohello mappát, ahol hello parancsfájlt futtatja. hello csatolt a lemezek, kötetek és hello megfelelő csatlakoztatási elérési utak ennek megfelelően jelennek meg. Ezek csatlakoztatási elérési utak gyökér szintű hozzáféréssel rendelkező látható toousers. Tallózzon a hello parancsfájl kimenetében említett hello kötetek.

  ![Linux-fájl helyreállítási panel](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-hello-connection"></a>Hello kapcsolat bezárása

Hello fájlok azonosítása után másolja őket tooa helyi tárolási helye, távolítsa el vagy leválasztani hello további meghajtók. toounmount hello meghajtók hello **fájlhelyreállítás** kattintson a panel az Azure-portálon hello **lemez leválasztása**.

![Lemez leválasztása](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Miután hello lemezek fürtköteteiről van szó, kap egy üzenetet, jelezve, hogy sikeres volt. Hello kapcsolat toorefresh néhány percig is eltarthat, így hello lemezek eltávolítása.

A Linux Miután hello kapcsolat toohello helyreállítási pont daraboltak van, hello az operációs rendszer nem távolítja hello megfelelő csatlakoztatási elérési utak automatikusan. Ezek nem létezik-e a "árva" kötetek és láthatók, de hibaüzenetet küldjön, amikor Ön hozzáférést/írás hello fájlokat. Manuálisan eltávolíthatók. hello parancsfájl futása közben ilyen bármely korábbi helyreállítási pontjait a meglévő köteteket azonosítja, és törli őket jóváhagyását követően.

## <a name="special-configurations"></a>Speciális konfigurációk

### <a name="dynamic-disks"></a>A dinamikus lemezek

Ha hello Azure virtuális gép mentett kiterjedő (átnyúló és csíkozott kötetek) több olyan lemezt és/vagy dinamikus lemezeken, hibatűrő kötetek (tükrözött vagy RAID-5) köteteket lejárt, a hello végrehajtható parancsfájl nem futtatható hello azonos virtuális gép. Ehelyett parancsprogrammal hello végrehajtható bármely más gépeken, amelyek egy kompatibilis operációs rendszert.

### <a name="windows-storage-spaces"></a>Windows tárolóhelyek

A tárolóhelyek a Windows egy olyan technológia, amely lehetővé teszi toovirtualize tárolási Windows Storage. A tárolóhelyek Windows csoport iparági szabványnak megfelelő lemezek tárolókészletbe csoportosítani, és majd hozza létre a virtuális lemezek, a tárolóhelyek a rendelkezésre álló területet hello ezeket a tárolókészleteket.

Ha hello Azure virtuális gép mentett tárolóhelyek szolgáltatást használja a Windows, akkor a hello végrehajtható parancsfájl nem futtatható hello azonos virtuális gép. Ehelyett parancsprogrammal hello végrehajtható bármely más gépeken, amelyek egy kompatibilis operációs rendszert.

### <a name="lvmraid-arrays"></a>LVM/RAID tömbök

A Linux, a logikai kötetkezelő (LVM) és/vagy a szoftver RAID tömbök használt toomanage logikai kötetek több lemez keresztül. Ha biztonsági másolatba mentett Linux virtuális gép hello LVM és/vagy RAID-tömbök használ, nem futtatható hello parancsfájl hello azonos virtuális gép. Ehelyett hello parancsprogrammal kompatibilis operációs rendszer bármelyik olyan gépen, és amely támogatja a biztonsági másolatba mentett virtuális gép hello fájlrendszer.

hello parancsfájl kimenete felsorolja hello LVM és/vagy RAID-tömbök lemezek és kötetek hello hello partíciótípusnak alább látható módon

   ![Linux LVM kimenete panelre](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
a következő hello parancsokat kell toobe által futtatott hello felhasználói toobring ezek a partíciók. 

**LVM partíciók**

```
$ pvs <volume name as shown above in hello script output> 
```
A parancs megjeleníti az hello kötet csoportnevek fizikai mennyiségi.

```
$ lvdisplay <volume-group-name from hello pvs command’s results> 
```
A parancs megjeleníti az összes logikai kötetek, nevét és az elérési utak a kötet csoport.

```
$ mount <LV path> </mountpath>
```
toomount hello logikai kötetek toohello elérési utat az Ön által választott.


**A RAID-tömbök**

```
$ mdadm –detail –scan
```
Ez az összes raid-lemez adatainak megjelenítése. hello megfelelő RAID lemez állapota`/dev/mdm/<RAID array name in hello backed up VM>`

Hello csatlakoztatási parancs használata, ha hello RAID lemez fizikai kötetekre.
```
$ mount [RAID Disk Path] [/mounthpath]
```

Ha a RAID-lemez van konfigurálva, egy másik LVM kövesse ugyanezt az eljárást hello ajánlásait LVM partíciók hello RAID lemez neve alatt hello kötet nevű

## <a name="troubleshooting"></a>Hibaelhárítás

Ha problémába ütközik a virtuális gépekről hello fájlok helyreállítása során, ellenőrizze a következő táblázat további információt hello.

| Hibaüzenet / forgatókönyv | Lehetséges ok | Javasolt művelet |
| ------------------------ | -------------- | ------------------ |
| A következő exe kimeneti: *toohello cél csatlakozás kivétel* |Parancsprogram nem tud tooaccess hello helyreállítási pont | Ellenőrizze, hogy hello gép megfelel-e a fent említett hello hozzáférési követelmények|  
|   A következő exe kimeneti: *hello cél már naplózta keresztül az iSCSI-munkamenetből.* |   hello parancsfájl már végre lett hajtva a hello ugyanazon számítógép és hello meghajtó csatlakoztatva |   hello kötetek hello helyreállítási pont már csatlakoztatva van. Akkor lehet, hogy csatlakoztatása nem ugyanazt a meghajtó betűjét hello az eredeti virtuális gép hello. Tallózzon az hello elérhető kötetek a Fájlkezelőben hello a fájl |
| A következő exe kimeneti: *ezt a parancsfájlt érvénytelen, mert hello lemezek portal/túllépte hello 12-hr korlát keresztül is le lett választva. Töltse le egy új parancsfájlt hello portálról.* |    hello lemezek rendelkezik le lett választva, hello portal vagy hello 12-hr korlátja túllépve |  Ez különösen exe most már érvénytelen, ezért nem futtatható. Ha azt szeretné, hogy tooaccess hello az adott helyreállítási pont időponthoz kötött fájlok, látogasson el a hello portál egy új exe-fájl|
| Hello gépen hello exe futtatják: hello új kötetek nem leválaszthatja, miután hello Leválasztás gombra kattint |    hello iSCSI-kezdeményező hello gépen nem válaszol vagy frissíteni a kapcsolat toohello célja és hello gyorsítótár karbantartása |    Néhány perc várakozás után hello dismount bekapcsolva. Ha hello új kötetek még nem leválasztották, keresse meg az összes hello kötetek keresztül. A kényszeríti hello kezdeményező toorefresh hello kapcsolat és hello kötet le van választva, egy hibaüzenet, amely hello lemez nem érhető el.|
| A következő exe kimeneti: parancsfájl futtatása sikeres volt, de "Új kötetek csatlakoztatva" nem jelenik meg a hello parancsfájl kimenetében |   Ez az egy átmeneti hiba   | volna hello kötetek már van csatolva. Nyissa meg a Explorer toobrowse. Ha ugyanaz a gép hello használ minden egyes parancsfájlok futtatásához, hello gép újraindítása, és hello lista hello későbbi exe futtatása kell megjelennie. |
| Linux-specifikus: nem sikerült tooview hello szükséges kötetek | az operációs rendszer hello hello gép ahol hello parancsfájl futtatása nem ismeri fel hello alapjául szolgáló fájlrendszer a biztonsági másolatba mentett virtuális gép hello | Ellenőrizze, hogy hello a helyreállítási pont-e összeomlás-konzisztens vagy fájlkonzisztens. Ha egy másik számítógépen, amelynek operációs rendszer felismeri hello egységes, futtassa hello parancsfájlját biztonsági másolatot készíteni a virtuális gép fájlrendszer |
| Windows-specifikus: nem sikerült tooview hello szükséges kötetek | hello lemezek csatolt, de hello kötetek nem voltak konfigurálva. | Hello lemez felügyeleti képernyőjén hello további lemezek kapcsolódó toohello helyreállítási pontot azonosító. Ezek a lemezek esetén a kapcsolat nélküli állapotban próbálja minősítené online kattintson a jobb gombbal a hello lemezen, és kattintson az "Online"|
