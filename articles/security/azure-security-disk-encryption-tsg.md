---
title: "Azure lemez titkosítási hibaelhárítása |} Microsoft Docs"
description: "Ez a cikk hibaelhárítási tippek a Microsoft Azure lemez Encryption for Windows és Linux IaaS virtuális gépeket."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: ce0e23bd-07eb-43af-a56c-aa1a73bdb747
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: devtiw
ms.openlocfilehash: 5f482a92b8fcd71a1b767fcc5741bc57605997ea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk Encryption hibaelhárítási útmutatója

Ez az útmutató informatikai (IT) szakemberek készült, információkat adatbiztonsági elemzők, és amelynek szervezetek Azure disk encryption használ, és útmutatást lemeztitkosítás hibaelhárítás kell felhő rendszergazdák kapcsolatos hiba lépett fel.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Hibaelhárítási Linux operációsrendszer-lemez titkosítása

Linux operációsrendszer-lemez titkosítása az operációs rendszer meghajtó azt a teljes lemez titkosítása folyamatot futtatása előtt kell választania.   Ha nem tudja, egy hibaüzenet, a "nem tudta leválasztani után..." hibaüzenet valószínű.

Ennek az oka valószínűleg, amikor az operációs rendszer lemeztitkosítás kísérletet egy virtuális gép célkörnyezet módosított vagy a támogatott készlet gyűjtemény lemezképből megváltozott.  A támogatott kép is zavarják a bővítmény leválasztása az operációs rendszer meghajtó eltéréseket közé:
- Testreszabott lemezképeket, amelyek már nem felel meg egy támogatott fájlrendszeren és/vagy a particionálási sémát.
- Alkalmazások, például víruskereső, Docker, SAP, MongoDB vagy az operációs rendszer titkosítási előtt futó Apache Cassandra testreszabott képeket.  Ezeket az alkalmazásokat nehezen leáll, és az operációs rendszer nyitott fájlok leíró megőrzését, ha a meghajtó nem, hibát okozó nem lehet.
- Egyéni futtatott parancsfájlok, a zárja be a titkosítás lépéssel közelségi kapcsolat zavaró, és ez a hiba miatt idő. Ez akkor fordulhat elő, ha a Resource Manager-sablon meghatározása végrehajtása egyidejűleg több kiterjesztést, vagy ha egy egyéni parancsprogramok futtatására szolgáló bővítmény vagy egyéb művelet fut egyidejűleg a lemez titkosítása.   Szerializálása és elkülönítése lépéseket azzal megoldhatja a problémát.
- SELinux titkosítás engedélyezése előtt nem lett letiltva, a leválasztási lépés sikertelen lesz.  SELinux titkosítási befejeződését követően lehet újra engedélyezni.
- Ha az operációsrendszer-lemezképet használ egy LVM séma (bár korlátozott LVM adatok támogatása nem érhető el, az operációs rendszer LVM lemez nem)
- Ha minimális memória feltételeknek nem felel meg (az operációs rendszer lemeztitkosítás 7GB ajánlott)
- Amikor adatokat /mnt/ könyvtár, vagy egymással (például /mnt/data1 /mnt/data2, /data3 + /data3/data4, stb.) úton csatlakoztatta a rekurzív módon lett volna
- Ha más Azure Disk Encryption [Előfeltételek](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) Linux nem felel meg

## <a name="unable-to-encrypt"></a>Nem sikerült titkosítani a

Néhány esetben az adatok titkosítása úgy tűnik, hogy "Az operációs rendszer lemezén lépések titkosítási" Beragadt Linux és SSH le van tiltva. Között a készlet gyűjtemény lemezkép 3-16 órát is igénybe vehet.  Ha multi-TB-os méret adatlemezt ad hozzá, a nap vehet igénybe. A Linux operációs rendszert futtató lemez titkosítási feladatütemezési ideiglenesen leválasztja az operációs rendszer meghajtó, és végrehajtja a titkosított állapotában tudják, mielőtt a teljes operációsrendszer-lemez blokkonként titkosítását.   Eltérően Azure Disk Encryption használatát a Windows Linux lemez titkosítása nem engedélyezi a virtuális gép egyidejű használatát amíg folyamatban van a titkosítás.  A teljesítményt nyújt a virtuális gép, például a lemez, hogy standard vagy prémium (SSD) tárhelyre, a tárfiók alapját mérete jelentős mértékben befolyásolhatja a teljes titkosítás szükséges időt.

Állapotának ellenőrzéséhez a Feladatnézetben mező által visszaadott a [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) parancs lekérhető.   Az operációs rendszer meghajtó titkosított, miközben a virtuális gép egy karbantartási állapotba kerül, és SSH is le van tiltva fennakadást a folyamatban lévő folyamatban érdekében.  EncryptionInProgress titkosítás folyamatban, majd később egy VMRestartPending üzenet arra kéri a virtuális gép újraindítása az órákat közben az az idő, a legtöbb megjelenik.  Példa:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Miután kell indítania a virtuális gép után a virtuális gép újraindítása, és egyúttal jogosultságot ad az újraindítás és a befejező lépései végezhető el a célszámítógépen 2-3 percet, állapotüzenetet jelzi, hogy a titkosítás végül befejeződött.   Ez az üzenet nem érhető el, ha a titkosított meghajtón az operációs rendszer várhatóan készen áll a használatra, és a virtuális gép is használható újra.

Azokban az esetekben, ahol az ebben a sorozatban nem fordult elő vagy ha rendszerindítási információk, állapotüzenetet vagy egyéb hiba mutatók jelentést, hogy az operációs rendszer titkosítási közepén Ez a folyamat (például ha a "nem sikerült leválasztani" hiba történt a jelen útmutatóban ismertetett) sikertelen volt, a ajánlott visszaállítani a virtuális Gépet a pillanatkép vagy azonnal titkosítási előtt készült biztonsági másolatok.  A következő kísérlet előtt javasolt ismételt kiértékelése a virtuális gép jellemzőit, és győződjön meg arról, hogy a szükséges előfeltételek maradéktalanul teljesülnek-e.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Hibaelhárítás az Azure Disk Encryption tűzfal mögött
Kapcsolat egy tűzfal, a proxy követelmény vagy a hálózati biztonsági csoport (NSG) beállításainak korlátozza, amikor a bővítmény hajthatnak végre a szükséges feladatok is működni.   Ez például a "Nem érhető el a virtuális Gépen a bővítmény állapotát" állapotüzenetek eredményezhet és várt esetekben nem sikerült befejezni.  A következő szakaszok rendelkezik néhány tűzfal kapcsolatos gyakori hibák, lehetséges, hogy vizsgálni.

### <a name="network-security-groups"></a>Network security groups (Hálózati biztonsági csoportok)
A hálózati biztonsági csoport beállításai továbbra is engedélyeznie kell a végpontot a dokumentált hálózati konfigurációnak megfelelő [Előfeltételek](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) a lemez titkosításához.

### <a name="azure-keyvault-behind-firewall"></a>Tűzfal mögött található Azure Keyvault
A virtuális gép kulcstároló eléréséhez képesnek kell lennie. Tekintse meg a kulcs tartalék által kezelt tűzfalon keresztül hozzáférést nyújtani a [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) csapatával.

### <a name="linux-package-management-behind-firewall"></a>Linux-csomag felügyeleti tűzfal mögött
Futási időben Azure Disk Encryption Linux támaszkodik a cél terjesztési Csomagkezelő rendszeren titkosítás engedélyezése előtt szükséges előfeltételként szükséges összetevők telepítéséhez.  Ha a tűzfal beállításainak megakadályozza, hogy a virtuális gép igényt töltse le és telepítse ezeket az összetevőket, majd további hibák várható.    A lépések szerint konfigurálhatja ezt a terjesztési által változhat.  A Red Hat Ha a proxy szükség, győződjön meg arról, hogy előfizetés-kezelő és yum megfelelően vannak beállítva nélkülözhetetlen.  Lásd: [ez](https://access.redhat.com/solutions/189533) Red Hat támogatási cikk erről a témakörről.  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Server Core hibaelhárítása

A Windows Server 2016 Server Core az a bdehdcfg összetevő nem áll rendelkezésre, alapértelmezés szerint. Ez az összetevő Azure Disk Encryption használata szükséges.

A megoldás a probléma a c:\windows\system32 mappában, a Server Core-lemezkép a Windows Server 2016 Data Center VM fájlok másolása a következő 4:

```
bdehdcfg.exe
bdehdcfglib.dll
bdehdcfglib.dll.mui
bdehdcfg.exe.mui
```

Ezután futtassa a következő parancsot:

```
bdehdcfg.exe -target default
```

Ezzel létrehoz egy 550MB rendszerpartíció, és ezután a rendszer újraindítása után segítségével Diskpart ellenőrizze a kötetek, majd a folytatáshoz.  

Példa:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
## <a name="see-also"></a>Lásd még:
Ebből a dokumentumból megismerte további információk az Azure disk encryption és elhárítása gyakori problémákat. Ez a szolgáltatás és a funkció kapcsolatos további információk:

- [Az Azure Security Centerben lemeztitkosítás alkalmazása](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure virtuális gép titkosítása](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Az Azure Data Encryption nyugalmi](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
