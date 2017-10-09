---
title: "Lemez titkosítási hibaelhárítási aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 2ecb8df1fb869e3bf8f3be4be4494e6485e75695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Azure Disk Encryption hibaelhárítási útmutatója

Ez az útmutató informatikai (IT) szakemberek készült, információk biztonsági elemzőknek, és felhő rendszergazdák, amelynek a szervezet Azure disk encryption használ, és útmutatást tootroubleshoot lemez-titkosítást kell kapcsolatos hiba lépett fel.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Hibaelhárítási Linux operációsrendszer-lemez titkosítása

Linux operációsrendszer-lemez titkosítása operációs rendszer hello meghajtó előzetes toorunning kell választania azt a folyamatot, hello teljes lemez titkosítása.   Ha nem tudja, egy hibaüzenet, a "toounmount után nem sikerült..." a hibaüzenet valószínűleg toooccur.

Ennek az oka valószínűleg, amikor az operációs rendszer lemeztitkosítás kísérletet egy virtuális gép célkörnyezet módosított vagy a támogatott készlet gyűjtemény lemezképből megváltozott.  Hello támogatott kép, amely zavarhatja a hello bővítmény képességét toounmount hello OS meghajtó eltéréseket közé:
- Testreszabott lemezképeket, amelyek már nem felel meg egy támogatott fájlrendszeren és/vagy a particionálási sémát.
- Alkalmazások, például víruskereső, Docker, SAP, MongoDB vagy hello az operációs rendszer előzetes tooencryption futó Apache Cassandra testreszabott lemezképeket.  Ezek az alkalmazások nehéz tooterminate, és nyitott fájlok kezeli az operációs rendszer toohello meghajtó megőrzését, ha a hello meghajtó nem, hibát okozó nem lehet.
- Egyéni futtatott parancsfájlok, a közelségi kapcsolat toohello titkosítási lépés zavaró, és ez a hiba miatt idő bezárásához. Ez akkor fordulhat elő, ha a Resource Manager-sablon meghatározása több bővítmények tooexecute egyszerre, vagy ha egy egyéni parancsprogramok futtatására szolgáló bővítmény vagy a másik művelet fut egyidejűleg toodisk titkosítás.   Szerializálása és lépéseket elkülönítése hello probléma esetleg elhárítható.
- Ha SELinux nem lett letiltva előzetes tooenabling titkosítási, hello leválasztása lépés sikertelen lesz.  SELinux titkosítási befejeződését követően lehet újra engedélyezni.
- Ha hello operációsrendszer-lemez egy LVM sémát használ (bár korlátozott LVM adatok támogatása nem érhető el, az operációs rendszer LVM lemez nem)
- Ha minimális memória feltételeknek nem felel meg (az operációs rendszer lemeztitkosítás 7GB ajánlott)
- Amikor adatokat /mnt/ könyvtár, vagy egymással (például /mnt/data1 /mnt/data2, /data3 + /data3/data4, stb.) úton csatlakoztatta a rekurzív módon lett volna
- Ha más Azure Disk Encryption [Előfeltételek](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) Linux nem felel meg

## <a name="unable-tooencrypt"></a>Nem lehet tooencrypt

Bizonyos esetekben hello Linux lemeztitkosítás jelenik meg a "Az operációs rendszer lemeztitkosítás lépések" Beragadt toobe és SSH le van tiltva. A folyamat eltarthat egy készlet gyűjtemény rendszerképre 3-16 órás toocomplete között.  Ha multi-TB-os méret adatlemezt ad hozzá, hello folyamat napig is eltarthat. hello Linux operációs rendszert futtató lemez titkosítási feladatütemezési ideiglenesen leválasztja az operációs rendszer hello meghajtó, és végrehajtja blokkonként titkosítási hello teljes operációsrendszer-lemez, a titkosított állapotában tudják azt megelőzően.   Eltérően Azure Disk Encryption használatát a Windows, Linux lemez titkosítása nem egyidejű használatának engedélyezése hello VM amíg hello titkosítás folyamatban van.  hello teljesítményétől hello VM hello mérete hello lemez, és hogy hello tárfiók alapját standard vagy prémium (SSD) tárhelyre, például jelentős mértékben befolyásolhatja hello szükséges idő toocomplete titkosítás.

toocheck állapot hello Feladatnézetben mező hello által visszaadott [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) parancs lekérhető.   Amíg az operációs rendszer hello meghajtó titkosított, hello virtuális gép egy karbantartási állapotba kerül, és SSH is letiltott tooprevent bármely megszűnésének toohello folyamatban.  EncryptionInProgress készül hello többségének hello idő, amíg titkosítási van folyamatban, majd később több óráig arra kéri a virtuális gép toorestart hello VMRestartPending üzenetet.  Példa:


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
ProgressMessage            : OS disk successfully encrypted, please reboot hello VM
```

Egyszer kéri a tooreboot hello virtuális gép, és hello virtuális gép újraindítása, és mivel a 2-3 percet hello újraindítás és a végső lépéseket toobe hello cél végre, miután hello állapot üzenet jelzi, hogy a titkosítás végül befejeződött.   Ez az üzenet nem érhető el, ha titkosított hello OS meghajtó le várt toobe készen áll, és a hello VM toobe használható újra.

Azokban az esetekben, ahol az ebben a sorozatban nem fordult elő vagy ha rendszerindítási információk, állapotüzenetet vagy egyéb hiba mutatók jelentést, hogy az operációs rendszer titkosítási hello középső (például ha a jelen útmutatóban ismertetett hello "sikertelen toounmount" hiba azért jelent meg), a folyamat sikertelen toorestore hello VM hátsó toohello pillanatkép vagy készült biztonsági másolatok közvetlenül megelőző tooencryption ajánlott.  Előzetes toohello következő kísérlet, akkor javasolt toore-értékelje ki a virtuális gép hello hello jellemzői, és győződjön meg arról, hogy a szükséges előfeltételek maradéktalanul teljesülnek-e.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Hibaelhárítás az Azure Disk Encryption tűzfal mögött
Ha kapcsolatot korlátozza egy tűzfal, a proxy követelmény vagy a hálózati biztonsági csoport (NSG) beállításainak hello képességét hello bővítmény tooperform szükséges feladatok is megszakad.   Ez például a "Hello virtuális gép nem érhető el a bővítmény állapotát" állapotüzenetek eredményezhet és várt forgatókönyvekben toofinish sikertelenek lesznek.  hello szakaszok rendelkezik néhány tűzfal kapcsolatos gyakori hibák, lehetséges, hogy vizsgálni.

### <a name="network-security-groups"></a>Network security groups (Hálózati biztonsági csoportok)
A hálózati biztonsági csoport beállításai továbbra is engedélyeznie kell a hello végpont toomeet dokumentált hello hálózati konfiguráció [Előfeltételek](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) a lemez titkosításához.

### <a name="azure-keyvault-behind-firewall"></a>Tűzfal mögött található Azure Keyvault
hello méretű képes tooaccess kulcstároló kell lennie. Tekintse meg a hozzáférés tookey tartalék hello által kezelt tűzfal mögül tooguidance [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) csapatával.

### <a name="linux-package-management-behind-firewall"></a>Linux-csomag felügyeleti tűzfal mögött
Futási időben Azure Disk Encryption Linux hello cél terjesztési csomag felügyeleti rendszer tooinstall szükséges előfeltételként szükséges összetevők előzetes tooenabling titkosítási támaszkodik.  Ha a tűzfal beállításainak hello VM, hogy nem tud toodownload, és telepíti ezeket az összetevőket, majd további hibák várható.    hello lépéseket tooconfigure ez terjesztési változhat.  A Red Hat Ha a proxy szükség, győződjön meg arról, hogy előfizetés-kezelő és yum megfelelően vannak beállítva nélkülözhetetlen.  Lásd: [ez](https://access.redhat.com/solutions/189533) Red Hat támogatási cikk erről a témakörről.  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Windows Server 2016 Server Core hibaelhárítása

A Windows Server 2016 Server Core az hello bdehdcfg összetevő nem áll rendelkezésre, alapértelmezés szerint. Ez az összetevő Azure Disk Encryption használata szükséges.

tooworkaround ezt a problémát, a következő 4 fájlok mappából egy Windows Server 2016 Data Center VM toohello c:\windows\system32 hello Server Core-lemezkép másolása hello:

```
bdehdcfg.exe
bdehdcfglib.dll
bdehdcfglib.dll.mui
bdehdcfg.exe.mui
```

Ezután futtassa a következő parancs hello:

```
bdehdcfg.exe -target default
```

Ezzel létrehoz egy 550MB rendszerpartíció, és ezután a rendszer újraindítása után a Diskpart toocheck hello köteteket használ, és a folytatáshoz.  

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
Ebből a dokumentumból megismerte kapcsolatos gyakori problémákat, az Azure disk encryption használatát, és hogyan tootroubleshoot. Ez a szolgáltatás és a funkció kapcsolatos további információk:

- [Az Azure Security Centerben lemeztitkosítás alkalmazása](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure virtuális gép titkosítása](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Az Azure Data Encryption nyugalmi](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
