---
title: "a Windows és Linux IaaS virtuális gépeket lemeztitkosítás aaaAzure |} Microsoft Docs"
description: "Ez a cikk áttekintést nyújt a Microsoft Azure lemez titkosítása a Windows és Linux IaaS virtuális gépeket."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: kakhan
ms.openlocfilehash: b685abdcc908e66d2352ec5ac2d9996aa75af1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Windows és Linux IaaS virtuális gépeket az Azure Disk Encryption
A Microsoft Azure erősen véglegesített tooensuring az adatvédelem, az adatok közös joghatóság alá és teszi lehetővé az Azure-t toocontrol speciális technológiák tooencrypt számos adatokat vezérlik, és a titkosítási kulcsok kezeléséhez az adatok vezérlő & naplózási hozzáférést. Ez az Azure ügyfelek hello rugalmasságot toochoose hello megoldást nyújt, amely az üzleti igényeinek leginkább megfelelő. A dokumentum azt kódelemeit tooa új technológia megoldás "A Windows és Linux rendszerű infrastruktúra-szolgáltatási virtuális Azure Disk Encryption" toohelp és megvédeni az adatok toomeet a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel. hello papír hogyan toouse hello Azure lemez titkosítási szolgáltatások többek között a hello támogatott forgatókönyvek és hello felhasználói élményt nyújt részletes útmutatást.

> [!NOTE]
> Bizonyos ajánlások növelheti az adatok, hálózati vagy számítási erőforrás-használat, ami azt eredményezi, hogy további licencek vagy előfizetések költségeit.

## <a name="overview"></a>Áttekintés
Az Azure Disk Encryption egy új képesség, amely segít a Windows és Linux IaaS virtuális gépek lemezeit titkosításához. Az Azure Disk Encryption kihasználja hello iparági szabvány [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) és a Windows hello szolgáltatás [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux tooprovide kötet titkosítási hello az operációs rendszer és adatlemezek hello szolgáltatást. hello megoldás integrálva van [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp vezérlik, és hello lemez-titkosítási kulcsokat és titkos kódokat a kulcstároló-előfizetésben. hello megoldás biztosítja azt is, hogy a virtuálisgép-lemezeket hello lévő összes adat titkosítva legyenek az Azure tárolás közben.

A Windows és Linux IaaS virtuális gépeket Azure lemeztitkosítás jelenleg **általánosan rendelkezésre álló** minden nyilvános Azure-régiók és AzureGov régióban a prémium szintű Storage szabványos virtuális gépek és virtuális gépek.

### <a name="encryption-scenarios"></a>Titkosítási forgatókönyvek
hello Azure Disk Encryption megoldás támogatja a következő forgatókönyvet hello:

* Engedélyezze a titkosítást a előzetes titkosítással VHD- és titkosítási kulcsok alapján létrehozott új IaaS virtuális gépeken
* Engedélyezze a titkosítást a hello támogatott Azure-gyűjtemény lemezképei alapján létrehozott új IaaS virtuális gépeken
* Az Azure-ban futó meglévő infrastruktúra-szolgáltatási virtuális gépeket a titkosítás engedélyezése
* Tiltsa le a titkosítást a Windows IaaS virtuális gépeken
* Tiltsa le a titkosítást a adatmeghajtókon Linux IaaS virtuális gépek
* Felügyelt lemezes virtuális gépek titkosításának engedélyezése
* Egy meglévő titkosított nem prémium szintű storage VM titkosítási beállításainak módosítása
* Biztonsági mentési és helyreállítási kulcs titkosítási kulccsal titkosított titkosított virtuális gépek

hello megoldás támogatja a következő forgatókönyvek IaaS virtuális gépek esetén, ha engedélyezve van a Microsoft Azure-ban hello:

* Az Azure Key Vault integrációja
* Standard szint virtuális gépek: [A, D, DS, G, GS, F és stb adatsorozat IaaS virtuális gépeket](https://azure.microsoft.com/pricing/details/virtual-machines/)
* A Windows és Linux IaaS virtuális gépeket és a felügyelt lemezes virtuális gépek a hello engedélyezése titkosítása támogatott Azure-katalógus lemezképek
* Tiltsa le a titkosítást az operációs rendszer és az adatok meghajtókon Windows IaaS virtuális gépeket és a felügyelt lemezes virtuális gépek
* A Linux IaaS virtuális gépeket és a felügyelt lemezes virtuális gépek adatmeghajtók titkosításának letiltása
* Engedélyezheti a titkosítást IaaS virtuális gépeket futtató Windows ügyfél operációs rendszer
* Engedélyezze a titkosítást a csatlakoztatási elérési utak köteteken
* Engedélyezze a titkosítást a Linux virtuális gépeken lemezzel konfigurált csíkozást (RAID) mdadm használatával
* Linux virtuális gépek LVM adatlemezek titkosításának engedélyezése
* Engedélyezze a titkosítást a tárolóhelyekkel konfigurált Windows virtuális gépeken
* Egy meglévő titkosított nem prémium szintű storage VM titkosítási beállításainak módosítása
* Minden Azure nyilvános és AzureGov régiók támogatottak.

hello megoldás nem támogatja a következő forgatókönyvek, funkcióit és technológia hello:

* Az alapszintű csomag IaaS virtuális gépeket
* Linux IaaS virtuális gépeket egy operációs rendszer meghajtóján titkosításának letiltása
* Egy adatmeghajtó titkosítás letiltása, ha az operációs rendszer meghajtó hello titkosított Linux Iaas virtuális gépek
* Infrastruktúra-szolgáltatási virtuális gépeket hello klasszikus virtuális gép létrehozási módszerének használatával létrehozott
* Engedélyezze a titkosítást a Windows és Linux IaaS virtuális gépeket a felhasználói egyéni lemezképek nem támogatott. Engedélyezze a enccryption Linux LVM operációs rendszer lemezek használata nem támogatott jelenleg. Ez a támogatás hamarosan határozza meg.
* Integráció a helyszíni kulcskezelő szolgáltatás
* Az Azure Files (megosztott fájlrendszer), a hálózati fájlrendszer (NFS), a dinamikus köteteket és a Windows alapú szoftveres RAID-rendszerek használatára konfigurált virtuális gépek
* Biztonsági mentési és visszaállítási titkosított virtuális gépek, titkosított kulcs titkosítási kulcs nélkül.
* Egy meglévő titkosított prémium szintű storage VM titkosítási beállításainak módosítása.

> [!NOTE]
> Biztonsági mentés és visszaállítás titkosított virtuális gépek csak támogatott virtuális gépek Titkosított hello KEK konfigurációjával kapcsolatban. Nélkül KEK titkosított virtuális gépeken nem támogatott. KEK nem kötelező paraméter, amely lehetővé teszi a virtuális gép titkosítása. Ez a támogatás hamarosan elérhető.
> Egy meglévő titkosított prémium szintű storage, virtuális gép nem támogatott titkosítási beállításainak módosítása. Ez a támogatás hamarosan elérhető.

### <a name="encryption-features"></a>Titkosítási funkciók
Engedélyezése és központi telepítésekor Azure Disk Encryption Azure IaaS virtuális gépekhez, hello alábbi képességeket engedélyezve vannak, a megadott hello konfigurációjától függően:

* Az operációs rendszer hello kötet tooprotect hello rendszerindító kötet a tárolás közben titkosítása
* Az adatok kötetek tooprotect hello az adatkötetek a tárolás közben titkosítása
* Windows IaaS virtuális meghajtók hello az operációs rendszer és az adatok titkosításának letiltása
* Hello adatok titkosításának letiltása meghajtók Linux IaaS virtuális gépek (csak akkor, ha az operációs rendszer a nem titkosított meghajtó)
* Hello titkosítási kulcsok és titkos kulcsokat a kulcstartót előfizetésben védelme
* Infrastruktúra-szolgáltatási virtuális gép hello titkosítási állapotát hello jelentéskészítés titkosított
* Lemeztitkosítás konfigurációs beállítások hello infrastruktúra-szolgáltatási virtuális gép eltávolítása
* Biztonsági mentés és visszaállítás titkosított virtuális gépek hello Azure Backup szolgáltatás használatával

> [!NOTE]
> Biztonsági mentés és visszaállítás titkosított virtuális gépek csak támogatott virtuális gépek Titkosított hello KEK konfigurációjával kapcsolatban. Nélkül KEK titkosított virtuális gépeken nem támogatott. KEK nem kötelező paraméter, amely lehetővé teszi a virtuális gép titkosítása.

Infrastruktúra-szolgáltatási virtuális gépek Windows és Linux-megoldás az Azure Disk Encryption tartalmazza:

* a Windows hello-lemeztitkosítás kiterjesztése.
* Linux hello lemeztitkosítás kiterjesztése.
* hello lemeztitkosítás PowerShell-parancsmagokkal.
* lemeztitkosítás Azure parancssori felület (CLI) parancsmagok hello.
* hello lemeztitkosítás Azure Resource Manager-sablonok.

hello Azure Disk Encryption megoldás Windows vagy Linux operációs rendszert futtató IaaS virtuális gépek esetén támogatott. Hello támogatott operációs rendszerekkel kapcsolatos további információkért lásd a "Hello"Előfeltételek"szakaszában.

> [!NOTE]
> Nem kell külön fizetni virtuális gépek lemezei az Azure Disk Encryption titkosítási van.

### <a name="value-proposition"></a>Értékajánlat
Ha hello Azure lemez titkosítási-kezelési megoldást alkalmaz, felel meg a következő üzleti igények hello:

* IaaS virtuális gépek védett aktívan, mert használhatja a szabványos titkosítási technológia tooaddress szervezeti biztonsági és megfelelőségi követelményeknek.
* Az ügyfél által felügyelt kulcsok és a házirendeket, és IaaS virtuális gépeket rendszerindító naplózhatja a key vaultban lévő használatát.

### <a name="encryption-workflow"></a>Titkosítási munkafolyamat
Windows és Linux virtuális gépek, tooenable lemeztitkosítás hello a következő:

1. Válasszon egy titkosítási forgatókönyv hello megelőző titkosítási forgatókönyvek közül.
2. Részt vevő tooenabling lemeztitkosítás hello Azure lemez titkosítási Resource Manager-sablon, a PowerShell-parancsmagokkal vagy a CLI parancsot, és hello titkosítási konfiguráció megadásához.

   * Hello ügyfél titkosított virtuális Merevlemezt a forgatókönyvben a Titkosított hello VHD tooyour tárfiók és hello titkosítási kulcs jelentős tooyour kulcstároló feltöltése. Ezt követően hello titkosítási konfigurációs tooenable titkosítást biztosíthat, egy új IaaS virtuális gépen.
   * A piactér hello létrehozott új virtuális gépek és a meglévő virtuális gépek, amelyeken már az Azure-ban adja meg hello titkosítás konfigurációs tooenable funkciót a hello infrastruktúra-szolgáltatási virtuális gép.

3. Adjon hozzáférést toohello Azure platformon tooread hello titkosítási kulcs anyagok (a BitLocker titkosítási kulcsok Windows rendszerekhez) és a Linux jelszava a kulcstartót tooenable titkosítás hello infrastruktúra-szolgáltatási virtuális gép.

4. Adja meg a hello Azure Active Directory (Azure AD) alkalmazás identitását toowrite hello titkosítási kulcs jelentős tooyour kulcstároló. Ezzel engedélyezi a titkosítás hello infrastruktúra-szolgáltatási virtuális gép 2. lépésben említett hello forgatókönyvek esetén.

5. Azure hello VM modell frissíti a titkosítás és hello kulcstároló konfigurációval, majd állít be a titkosított virtuális Gépet.

 ![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Visszafejtési munkafolyamat
infrastruktúra-szolgáltatási virtuális gépeket, a következő magas szintű lépései teljes hello toodisable lemeztitkosítás:

1. Toodisable titkosítás (visszafejtési) válasszon egy infrastruktúra-szolgáltatási virtuális gép az Azure-ban a hello Azure lemez titkosítási Resource Manager-sablon vagy a PowerShell-parancsmagok használatával, majd hello visszafejtési konfiguráció megadásához.

 Ezzel a lépéssel letiltja a titkosítást hello az operációs rendszer vagy hello adatmennyiség vagy mindkét szolgáltatás fut a Windows alapú infrastruktúra-szolgáltatási virtuális gép hello. Azonban mivel hello előző szakaszban említett, Linux operációs rendszer lemez titkosításának letiltása nem támogatott. hello visszafejtési lépés csak engedélyezett adatmeghajtók Linux virtuális gépeken, amíg az operációsrendszer-lemez hello nincs titkosítva.
2. Azure-hírt hello VM modell, és infrastruktúra-szolgáltatási virtuális gép hello visszafejtett van megjelölve. hello tartalmát hello virtuális gép már nem titkosított aktívan.

> [!NOTE]
> hello disable-titkosítási művelet nem törli a kulcs tárolóban és hello titkosítási kulcsokat tárol (a Windows rendszereken a BitLocker titkosítási kulcsok) vagy a Linux jelszava.
 > Nem támogatott Linux operációs rendszer lemez titkosításának letiltása. hello visszafejtési lépés csak a Linux virtuális gépeken adatmeghajtók engedélyezett.
Lemez adattitkosítás a Linux letiltása nem támogatott, ha hello operációsrendszer-meghajtó van titkosítva.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt engedélyezné a Azure Disk Encryption Azure IaaS virtuális gépeken hello támogatott forgatókönyvek hello "Overview" szakaszban tárgyalt, lásd: hello a következő előfeltételek:

* Rendelkeznie kell egy érvényes aktív Azure-előfizetéssel toocreate erőforrások az Azure-régiókban hello támogatott.
* A következő Windows Server-verziók hello támogatott Azure Disk Encryption: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 és Windows Server 2016.
* A következő Windows-ügyfélverziókat hello támogatott Azure Disk Encryption: Windows 8 ügyfél és a Windows 10-ügyfeleknek.

> [!NOTE]
> Windows Server 2008 R2 rendelkeznie kell az Azure-titkosítás engedélyezése előtt telepített .NET-keretrendszer 4.5. Telepítheti a Windows Update telepítésével hello opcionális frissítést a Microsoft .NET-keretrendszer 4.5.2-es Windows Server 2008 R2 x64-alapú rendszerekhez ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Az Azure Disk Encryption támogatott Azure-katalógus következő hello Linux server disztribúciók és verziók alapul:

| A Linux-Disztribúció | Verzió | Támogatott titkosítási a kötet típusa|
| --- | --- |--- |
| Ubuntu | 16.04-NAPI-ES LTS VERZIÓ | Operációs rendszer és az adatok lemezre |
| Ubuntu | 14.04.5-DAILY-LTS | Operációs rendszer és az adatok lemezre |
| Ubuntu | 12.10 | Adatlemez |
| Ubuntu | 12.04 | Adatlemez |
| RHEL | 7.3 | Operációs rendszer és az adatok lemezre |
| RHEL | 7.2 | Operációs rendszer és az adatok lemezre |
| RHEL | 6.8 | Operációs rendszer és az adatok lemezre |
| RHEL | 6.7 | Adatlemez |
| CentOS | 7.3 | Operációs rendszer és az adatok lemezre |
| CentOS | 7.2n | Operációs rendszer és az adatok lemezre |
| CentOS | 6.8 | Operációs rendszer és az adatok lemezre |
| CentOS | 7.1 | Adatlemez |
| CentOS | 7.0 | Adatlemez |
| CentOS | 6.7 | Adatlemez |
| CentOS | 6.6 | Adatlemez |
| CentOS | 6.5 | Adatlemez |
| openSUSE | 13.2 | Adatlemez |
| SLES | 12 SP1 | Adatlemez |
| SLES | 12-SP1 (prémium) | Adatlemez |
| SLES | HPC 12 | Adatlemez |
| SLES | 11-SP4 (prémium) | Adatlemez |
| SLES | 11 SP4 | Adatlemez |

* Az Azure Disk Encryption megköveteli, hogy a kulcstartót és virtuális gépek található hello azonos Azure-régió, és az előfizetés.

> [!NOTE]
> Hello erőforrások konfigurálása az külön régióban hibát okoz a hello Azure Disk Encryption funkció engedélyezése.

* tooset össze és a kulcstartót konfigurálása az Azure Disk Encryption, című rész **állítsa össze, és a kulcstartót konfigurálása az Azure Disk Encryption** a hello *Előfeltételek* című szakaszát.
* tooset össze és az Azure AD-alkalmazást az Azure Active directory konfigurálása Azure Disk Encryption, című **hello Azure AD alkalmazás az Azure Active Directory beállítása** a hello *Előfeltételek* című szakaszát.
* tooset össze és hello kulcstároló hozzáférési házirend hello Azure AD alkalmazás konfigurálása, című **hello kulcstároló hozzáférési házirend hello Azure AD-alkalmazás beállítása** a hello *Előfeltételek* szakasza Ez a cikk.
* című rész tooprepare előzetes titkosítással Windows virtuális merevlemez, **előzetes titkosítással Windows virtuális merevlemez előkészítése** a hello *függelék*.
* című rész tooprepare előzetes titkosítással Linux virtuális merevlemez, **előzetes titkosítással Linux virtuális merevlemez előkészítése** a hello *függelék*.
* hello Azure platformon kell hozzáférést toohello titkosítási kulcsok vagy titkos kulcsainak a kulcstartót toomake őket elérhető toohello virtuális gép indul el, és hello virtuális gép operációs rendszer mennyiségi visszafejti esetén. toogrant engedélyek tooAzure platform, set hello **EnabledForDiskEncryption** hello kulcstartó tulajdonságait. További információkért lásd: **állítsa össze, és a kulcstartót konfigurálása az Azure Disk Encryption** a hello függelék.
* A kulcstároló titkos kulcs és a KEK URL-címek rendszerverzióval ellátott kell lennie. Azure érvénybe lépteti az e versioning korlátozása. Érvényes titkos kulcs és KEK URL-címek tekintse meg a következő példák hello:

  * Érvényes titkos URL-címet: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Érvényes KEK URL-címet: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Az Azure Disk Encryption nem támogatja a kulcstároló titkok és KEK URL-címek részeként megadását portszámokat. Nem támogatott és a támogatott kulcstároló URL-címek, tekintse meg a következő hello:

  * Nem fogadható el kulcstároló URL-cím *xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx https://contosovault.vault.azure.net:443/titkos kulcsok/contososecret*
  * Elfogadható kulcstároló URL-cím: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* tooenable hello Azure Disk Encryption szolgáltatás hello IaaS virtuális gépeket hello hálózati végpont-konfigurációs követelményeknek kell megfelelniük:
  * tooget token tooconnect tooyour kulcstároló, hello infrastruktúra-szolgáltatási virtuális gép lehet képes tooconnect tooan Azure Active Directory végpontján \[login.microsoftonline.com\].
  * toowrite hello titkosítási kulcsok tooyour kulcstároló, hello infrastruktúra-szolgáltatási virtuális gép képes tooconnect toohello kulcstároló végpont kell lennie.
  * infrastruktúra-szolgáltatási virtuális gép hello képes tooconnect tooan az Azure storage végpont, hogy a gazdagépek hello Azure bővítménytárban és az Azure-tárfiók, hogy a gazdagépek hello VHD-fájlokat kell lennie.

  > [!NOTE]
  > Ha a biztonsági házirend korlátozza a hozzáférést a Azure virtuális gépek toohello Internet, URI megelőző hello megoldásához, és egy adott szabály tooallow kimenő kapcsolat toohello IP-címek konfigurálásához.
  >
  >tooconfigure és hozzáférés az Azure Key Vault (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall) tűzfal mögött

* Az Azure PowerShell SDK verzió tooconfigure Azure Disk Encryption hello legújabb verzióját használja. Hello legújabb verziójának letöltése [Azure PowerShell kiadás](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Nem támogatott az Azure Disk Encryption [Azure PowerShell SDK verzió 1.1.0-ás](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Ha hibaüzenetet kap kapcsolódó toousing Azure PowerShell 1.1.0-ás, lásd: [Azure lemez titkosítási hiba kapcsolódó tooAzure PowerShell 1.1.0-ás](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* toorun bármely Azure CLI parancsot, és társítsa azt az Azure-előfizetése, először telepítenie kell az Azure parancssori felület:
  * az Azure parancssori felület tooinstall és rendelje hozzá azt az Azure-előfizetéssel, lásd: [hogyan tooinstall és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md).
  * Lásd az Azure parancssori felület Mac, Linux és a Windows az Azure Resource Manager toouse [Azure parancssori felület parancsait erőforrás-kezelő módban](../virtual-machines/azure-cli-arm-commands.md).

* Egy kezelt lemez titkosítása esetén kötelező előfeltétel tootake felügyelt hello lemez pillanatképe vagy hello lemezen kívül Azure Disk Encryption előzetes tooenabling titkosítási biztonsági mentést.  A hely biztonsági másolat nélkül minden titkosítás során váratlan hiba okozhatja hello lemez és virtuális gép nem érhető el egy helyreállítási beállítás megadása nélkül.  Set-AzureRmVMDiskEncryptionExtension felügyelt lemezek biztonsági mentése, és jelenleg nem lesz a hiba, ha használni egy felügyelt lemezes szemben, kivéve, ha hello - skipVmBackup paraméter van megadva.  A paraméter nem biztonságos toouse, kivéve, ha a biztonsági mentés kívül Azure Disk Encryption már megtörtént.   Hello - skipVmBackup paraméter megadása esetén a hello parancsmag nem készítsen felügyelt hello lemez előzetes tooencryption biztonsági mentést.  Emiatt egy kötelező meg arról, hogy a virtuális gép helye előzetes tooenabling Azure Disk Encryption van, abban az esetben, ha újabb hello felügyelt lemezes biztonsági másolatának szükséges előfeltételek toomake számít.  
> [!NOTE]
 > hello - skipVmBackup paraméter soha nem kell használni, kivéve, ha egy pillanatkép vagy a biztonsági mentés már megtörtént Azure Disk Encryption kívül. 

* hello Azure Disk Encryption megoldás hello BitLocker külső kulcsvédő Windows IaaS virtuális gépeket használ. Tartományhoz csatlakoztatott virtuális gépeket, a nem leküldéses, amelyeket TPM védelmi csoport házirendekben. "A BitLocker engedélyezése kompatibilis TPM nélküli" hello a csoportházirenddel kapcsolatos információkért lásd: [a BitLocker csoportházirend-referenciája](https://technet.microsoft.com/library/ee706521).
* Egyéni csoportházirenddel tartományhoz csatlakoztatott virtuális gépeken a BitLocker házirendnek tartalmaznia kell a következő beállítás hello: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption sikertelenek lesznek, amikor egyéni csoportházirend-beállítások a BitLocker nem kompatibilisek. Gépek, amelyeknek nincs hello szükség lehet a megfelelő házirend-beállítás alkalmazása hello új házirend, újraindítás hello új házirend tooupdate (gpupdate.exe Force), és indítsa újra.  
* toocreate egy Azure AD-alkalmazást, hozzon létre egy kulcstartót, vagy állítson be egy meglévő kulcstároló és engedélyezheti a titkosítást, lásd: hello [Azure Disk Encryption előfeltétel PowerShell-parancsfájl](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* tooconfigure lemeztitkosítás Előfeltételek hello Azure parancssori felület használatával lásd: [a Bash parancsfájlok](https://github.com/ejarvi/ade-cli-getting-started).
* toouse hello Azure Backup szolgáltatás tooback mentése és visszaállítása titkosított virtuális gépeken, ha titkosítás engedélyezve van az Azure Disk Encryption használatát, a virtuális gépek titkosítására hello Azure Disk Encryption konfigurációja. hello biztonsági mentési szolgáltatás támogatja a virtuális gépek csak KEK-beállítás használatával titkosított. Lásd: [hogyan tooback mentése és visszaállítása titkosított virtuális gépek az Azure biztonsági másolatok titkosításának](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).

* A Linux operációs rendszer kötetének titkosításához, vegye figyelembe, hogy a virtuális gép újraindítása jelenleg szükséges hello hello folyamat végén. Ezt megteheti hello portálon, a powershell vagy a parancssori felület.   tootrack hello folyamatban lévő titkosítási, rendszeres időközönként lekérdezi a Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus által visszaadott hello állapotüzenetet.  Titkosítási végrehajtása után hello hello Ez a parancs által visszaadott állapotüzenet jelzi ezt.  Például "Feladatnézetben: operációsrendszer-lemez titkosítása sikeres volt, indítsa újra a virtuális gép hello", a pont hello virtuális gép újraindul, és használja.  

* Az Azure Disk Encryption Linux szükséges adatok lemezek toohave lévő Linux előzetes tooencryption csatlakoztatott fájlrendszer

* A rekurzív módon csatlakoztatott adatlemezek nem támogatottak az Azure Disk Encryption hello Linux. Például ha hello célrendszer rendelkezik csatlakoztatott lemez /foo/bar és majd egy másik /foo/bar/baz, /foo/bar/baz hello titkosítását a sikeres lesz, de/PEL/sáv titkosítása sikertelen lesz. 

* Az Azure Disk Encryption csak támogatott Azure katalógusában lemezképeket, amelyek megfelelnek a fentebb említett Előfeltételek hello támogatott. Ügyfél egyéni lemezképek használata nem támogatott fizetési toocustom partíciós séma és a folyamat viselkedéshez, amely ezeket a lemezképeket is létezik. További még akkor is gyűjteménye a Képalapú a virtuális gép kezdetben előfeltételek teljesülnek, de lehet, hogy nem kompatibilis létrehozása után módosított.  Az, hogy emiatt hello javasolt eljárás a Linux virtuális gépek titkosításához toostart tiszta gyűjtemény lemezképről, hello virtuális gép titkosítása, és hozzáadja az egyéni szoftverfrissítések vagy adatok toohello virtuális gép szükség szerint.  

> [!NOTE]
> Biztonsági mentés és visszaállítás titkosított virtuális gépek csak támogatott virtuális gépek Titkosított hello KEK konfigurációjával kapcsolatban. Nélkül KEK titkosított virtuális gépeken nem támogatott. KEK nem kötelező paraméter, amely lehetővé teszi a virtuális gép.

#### <a name="set-up-hello-azure-ad-application-in-azure-active-directory"></a>Beállítása hello Azure AD-alkalmazást az Azure Active Directoryban
Titkosítási toobe egy Azure-ban futó virtuális gépen engedélyezve van szüksége, amikor az Azure Disk Encryption hoz létre, és írja a hello titkosítási kulcsok tooyour kulcstároló. A kulcstartót a titkosítási kulcsok kezelése az Azure AD-hitelesítés szükséges.

Erre a célra hozzon létre egy Azure AD-alkalmazást. Az alkalmazás regisztrálásához hello blogbejegyzés hello "Identitás beolvasása az alkalmazás hello" szakaszában található részletes lépéseket [Azure Key Vault - részletes](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). A feladás egy vagy több beállítása és konfigurálása a kulcstartót hasznos példák számos is tartalmaz. Hitelesítési célokra is használhatja, vagy titkos kulcs-alapú ügyfélhitelesítés, vagy az Azure AD-alapú ügyfél-hitelesítéshez.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Az Azure AD ügyfél titkos kulcs alapú hitelesítés
hello szakaszok segíthetnek az Azure AD egy ügyfél titkos kulcs alapú hitelesítés beállítása.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Az Azure AD alkalmazás létrehozása az Azure PowerShell használatával
A következő PowerShell-parancsmag toocreate egy Azure AD-alkalmazást hello használata:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId hello Azure AD ClientID és $aadClientSecret hello ügyfélkulcs újabb tooenable Azure Disk Encryption kell használni. Az Azure AD hello ügyfélkulcs megfelelően védelme.

##### <a name="setting-up-hello-azure-ad-client-id-and-secret-from-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello hello Azure AD ügyfél-azonosító és a titkos kulcs beállítása
Is állíthatja be az ügyfél-azonosító az Azure AD és a titkos kulcs hello segítségével [a klasszikus Azure portálon]( https://manage.windowsazure.com). tooperform ennek a feladatnak a, a következő hello:

1. Kattintson a hello **Active Directory** fülre.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Kattintson a **alkalmazás hozzáadása**, és ezután hello alkalmazás neve.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Hello nyíl gombra, és adja meg hello alkalmazás tulajdonságait.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Kattintson a hello alacsonyabb bal sarok toofinish hello van jelölve. hello alkalmazás konfigurációs lap jelenik meg, és hello Azure AD ügyfél-azonosító hello aljához hello oldal jelenik meg.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. Mentse az Azure AD hello ügyfélkulcs hello kattintva **mentése** gombra. Megjegyzés: az Azure AD hello ügyfélkulcs hello kulcsok szövegmezőben. Annak megfelelően védelme.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > Attribútumfolyam megelőző hello hello a klasszikus Azure portálon nem támogatott.

##### <a name="use-an-existing-application"></a>Használjon egy meglévő alkalmazást
tooexecute hello a következő parancsokat, beszerzése, és hello használata [Azure AD PowerShell modult](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> hello következő parancsok kell végrehajtani egy új PowerShell-ablakban. Ne használja az Azure PowerShell vagy hello Azure Resource Manager ablak tooexecute hello parancsok. Ez a megközelítés azt javasoljuk, mivel ezek a parancsmagok hello MSOnline modul- vagy Azure AD PowerShell segítségével.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Az Azure AD-alapú hitelesítés
> [!NOTE]
> Az Azure AD-alapú hitelesítés jelenleg nem támogatott Linux virtuális gépeken.

hello a következő szakaszok megjelenítése hogyan tooconfigure egy az Azure AD-alapú hitelesítés.

##### <a name="create-an-azure-ad-application"></a>Az Azure AD-alkalmazás létrehozása
toocreate egy Azure AD-alkalmazást, a következő PowerShell-parancsmagok hello hajtható végre:

> [!NOTE]
> Cserélje le a következő hello `yourpassword` a biztonságos jelszó és a biztonságos működés érdekében hello jelszót karakterlánc.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Ez a lépés befejezése után PFX fájl tooyour kulcstároló feltöltése és hello hozzáférési házirend toodeploy adott tanúsítvány tooa VM engedélyezése.

##### <a name="use-an-existing-azure-ad-application"></a>Egy Azure AD-alkalmazást használja
Ha konfigurál egy meglévő alkalmazás tanúsítvány alapú hitelesítést, használja az itt látható hello PowerShell-parancsmagokkal. Lehet, hogy tooexecute őket a egy új PowerShell-ablakot.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Ez a lépés befejezése után PFX fájl tooyour kulcstároló feltöltése és toodeploy hello tanúsítvány tooa VM szükséges hello hozzáférési szabályzatának engedélyezéséhez.

##### <a name="upload-a-pfx-file-tooyour-key-vault"></a>PFX fájl tooyour kulcstároló feltöltése
Ez az eljárás részletes leírását, lásd: [hivatalos Azure Key Vault fejlesztői Blog hello](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Azonban a következő PowerShell-parancsmagok hello-e minden hello tevékenység van szüksége. Lehet, hogy tooexecute őket az Azure PowerShell-konzolon.

> [!NOTE]
> Cserélje le a következő hello `yourpassword` a biztonságos jelszó és a biztonságos működés érdekében hello jelszót karakterlánc.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-tooan-existing-vm"></a>A virtuális gép meglévő kulcstároló tooan a tanúsítvány központi telepítése
Hello PFX feltöltése után a virtuális gép meglévő hello következőre hello kulcstároló tooan tanúsítvány központi telepítése:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-hello-key-vault-access-policy-for-hello-azure-ad-application"></a>Hello kulcstároló hozzáférési házirend hello Azure AD-alkalmazás beállítása
Az Azure AD-alkalmazást kell jogok tooaccess hello kulcsok vagy titkos hello tárolóban lévő állapottal. Használjon hello [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) parancsmag toogrant engedélyek toohello alkalmazás, hello ügyfél-azonosító (amely készítésének hello alkalmazás regisztrálásakor) használja, mint hello _– ServicePrincipalName_ a paraméter értékét. toolearn több, tekintse meg a hello blogbejegyzésben [Azure Key Vault - részletes](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Íme egy példa hogyan tooperform ennek a feladatnak a keresztül PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Az Azure Disk Encryption igényel a következő hozzáférési házirendek az Azure AD tooyour ügyfélalkalmazás tooconfigure hello: _WrapKey_ és _beállítása_ engedélyek.

## <a name="terminology"></a>Terminológia
néhány gyakori használati hello használják ezt a technológiát használja hello következő terminológia táblázat toounderstand:

| Terminológia | Meghatározás |
| --- | --- |
| Azure AD | Azure ad [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Az Azure AD-fiókot hitelesítéséhez, tárolásának és titkos kulcsok beolvasása a kulcstároló előfeltétele. |
| Azure Key Vault | Key Vault egy kriptográfiai, kulcs szolgáltatást a Federal Information Processing szabványok FIPS érvényesített hardveres biztonsági modulok, amely segítségével megakadályozhatja a kriptográfiai kulcsokat és a bizalmas titkos alapul. További információkért lásd: [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentációját. |
| ARM | Azure Resource Manager |
| A BitLocker |[A BitLocker](https://technet.microsoft.com/library/hh831713.aspx) egy iparági felismerhető Windows kötet titkosítási technológia, amely Windows IaaS virtuális gépeken tooenable lemeztitkosítás használatban van. |
| BEK | A BitLocker titkosítási kulcsai használt tooencrypt hello az operációs rendszer rendszerindító kötet és az adatköteteket. hello BitLocker-kulcsok elő a kulcstároló, a titkos kulcsok. |
| parancssori felület | Lásd: [Azure parancssori felület](../cli-install-nodejs.md). |
| DM-crypt program segítségével |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) hello Linux-alapú, átlátszó lemeztitkosítás alrendszer tooenable lemeztitkosítás által használt Linux IaaS virtuális gépeken. |
| KEK | Kulcstitkosítás hello aszimmetrikus kulcs (RSA 2048), hogy tooprotect használhatja, vagy burkolása hello titkos kulcsa. Megadhat egy hardveres biztonsági modulok (HSM)-kulcs vagy kulcs szoftveres védelemmel ellátott védett. További részletekért lásd: [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentációját. |
| PS parancsmagok | Lásd: [Azure PowerShell-parancsmagok](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Állítson be és az Azure Disk Encryption key vaultban konfigurálása
Az Azure Disk Encryption segíti biztonságos működés érdekében hello lemez-titkosítási kulcsok és titkos key vaultban lévő. tooset mentése az Azure Disk Encryption key vaultban, teljes hello az egyes szakaszok a következő hello szükséges lépések.

#### <a name="create-a-key-vault"></a>Kulcstartó létrehozása
Kulcstároló, toocreate hello alábbi beállítások egyikét használja:

* ["101-kulcs-tároló-" Resource Manager-sablon létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Az Azure PowerShell kulcstároló-parancsmagok](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Hogyan túl[a kulcstartót biztonságos](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Ha már állított be kulcstároló az előfizetéséhez, hagyja ki toohello következő szakaszt.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Állítsa be a titkosítás kulcsot (nem kötelező)
Ha egy KEK toouse hello BitLocker titkosítási kulcsok biztonsági szint, vegye fel a KEK tooyour kulcstároló. Használjon hello [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) parancsmag toocreate hello a key vaultban egy kulcs titkosítási kulcs. A helyszíni kulcskezelő hardveres biztonsági MODULT is importálhat egy KEK. További részletekért lásd: [Key Vault dokumentáció](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Hello KEK tooAzure erőforrás-kezelő címen, vagy a kulcstartót felhasználói felületének használatával adhat hozzá.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Kulcstároló engedélyek beállítása
hello Azure platformon kell hozzáférést toohello titkosítási kulcsok vagy titkos kulcsainak a kulcstartót toomake őket elérhető toohello virtuális Gépet a rendszerindítás és hello kötetek visszafejtése. toogrant engedélyek toohello Azure platformon, set hello **EnabledForDiskEncryption** tulajdonság hello kulcs tároló hello kulcstároló PowerShell parancsmag segítségével:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Azt is beállíthatja hello **EnabledForDiskEncryption** hello felkeresésével tulajdonság [Azure erőforrás-kezelő](https://resources.azure.com).

Amint azt korábban említettük, meg kell adni a hello **EnabledForDiskEncryption** a kulcstartót tulajdonságát. Ellenkező esetben hello központi telepítés sikertelen lesz.

Beállíthat hozzáférési házirendek az Azure AD alkalmazás hello kulcstároló felületről itt látható módon:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

A hello **speciális hozzáférési házirendek** lapra, győződjön meg arról, hogy a kulcstartót engedélyezve van-e az Azure Disk Encryption:

![Az Azure key vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Lemeztitkosítás üzembe helyezési forgatókönyvek és a felhasználói élmény
Sok lemeztitkosítás lehetőségeket engedélyezhet, és hello lépések eltérhetnek függően toohello forgatókönyv. a következő szakaszok hello forgatókönyveket is lefedi nagyobb részletességgel hello.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-hello-marketplace"></a>Új IaaS virtuális gépeket a piactér hello létrehozott titkosításának engedélyezése
Hello segítségével engedélyezheti az új IaaS Windows virtuális gép hello Azure Piactérről származó lemeztitkosítás [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello titkosítási konfigurációs hello **paraméterek** panelt, és kattintson **OK**.

2. Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** tooenable titkosítási egy új IaaS virtuális gépen.

> [!NOTE]
> Ez a sablon létrehoz egy új titkosított Windows virtuális Gépet, amely a Windows Server 2012 hello gyűjtemény lemezképet használja.

Segítségével engedélyezheti az adatok titkosítása a új IaaS RedHat Linux 7.2 rendszerű virtuális gép egy 200 GB-os RAID-0 tömbbel rendelkező [Resource Manager-sablon](https://aka.ms/fde-rhel). Hello sablon telepítése után ellenőrizze a hello VM titkosítási állapotát hello segítségével `Get-AzureRmVmDiskEncryptionStatus` parancsmag, a [az operációs rendszer titkosított meghajtó a futó Linux virtuális gép](#encrypting-os-drive-on-a-running-linux-vm). Ha hello gép állapotot ad vissza _VMRestartPending_, indítsa újra a virtuális gép hello.

hello alábbi táblázat hello erőforrás-kezelő Sablonparaméterek hello piactér forgatókönyvet az Azure AD ügyfél-azonosító segítségével új virtuális gépek esetén:

| Paraméter | Leírás |
| --- | --- |
| adminUserName | Rendszergazda felhasználó hello virtuális gép nevét. |
| adminPassword | Rendszergazdai jelszóval hello virtuális géphez. |
| newStorageAccountName | Hello tárolási fiók toostore az operációs rendszer és a VHD-k adatokat neve. |
| vmSize | Hello virtuális gép méretét. Jelenleg csak a Standard A, a D és G adatsorozat támogatottak. |
| virtualNetworkName | Virtuális hálózat a virtuális gép hálózati hello hello nevét kell tartoznia. |
| subnetName | A virtuális hálózat a virtuális gép hálózati hello hello hello alhálózat nevét kell tartoznia. |
| AADClientID | Engedélyek toowrite titkok tooyour kulcstároló hello Azure AD alkalmazás ügyfél-azonosítója. |
| AADClientSecret | Hello Azure AD-alkalmazás, amely rendelkezik engedélyekkel toowrite titkok tooyour kulcstároló titkos ügyfélkulcsot. |
| keyVaultURL | Hello kulcs URL-tároló adott hello BitLocker kulcs fel kell tölteni. Hello parancsmaggal kaphat `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | Hello fő titkosítási kulcsát használt tooencrypt hello URL-címe jönnek létre, a BitLocker-kulcsot (nem kötelező). |
| keyVaultResourceGroup | Hello kulcstároló erőforráscsoportot. |
| vmName | Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre. |

> [!NOTE]
> _KeyEncryptionKeyURL_ egy nem kötelező paraméter. A key vaultban helyezheti a saját KEK toofurther védelmi hello adattitkosítási kulcsot (jelszó titkos).

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Új IaaS virtuális gépeket ügyfél titkosított virtuális merevlemez és a titkosítási kulcsokat a létrehozott titkosításának engedélyezése
Ebben a forgatókönyvben engedélyezheti a hello Resource Manager-sablon, a PowerShell-parancsmagokkal vagy a parancssori felület parancsait használatával titkosított. hello alábbi szakaszok ismertetik a nagyobb részletességgel hello Resource Manager-sablon és a parancssori felület parancsait.

Hello követésével egy ezekben a szakaszokban való felkészülés előzetes titkosítással képek használható az Azure-ban. Hello lemezkép létrehozása után hello következő szakasz toocreate egy titkosított Azure virtuális gép hello lépéseket is használhatja.

* [Előzetes titkosítással Windows virtuális merevlemez előkészítése](#preparing-a-pre-encrypted-windows-vhd)
* [Előzetes titkosítással Linux virtuális merevlemez előkészítése](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-hello-resource-manager-template"></a>Hello Resource Manager-sablon használatával
Hello használatával a titkosított virtuális merevlemezen lemeztitkosítás is engedélyezheti [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello titkosítási konfigurációs hello **paraméterek** panelt, és kattintson **OK**.

2. Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** tooenable titkosítás hello új IaaS virtuális Gépet.

hello alábbi táblázat hello Resource Manager sablon paramétereinek a titkosított virtuális merevlemez:

| Paraméter | Leírás |
| --- | --- |
| newStorageAccountName | Hello tárolási fiók toostore hello neve titkosítja az operációs rendszer virtuális Merevlemezt. Ezt a tárfiókot kell már létrehozott hello ugyanazt az erőforráscsoportot és hello VM ugyanazon a helyen. |
| osVhdUri | Az operációs rendszer virtuális merevlemez hello hello tárfiókból URI Azonosítóját. |
| osType | Az operációs rendszer terméktípusának (Windows/Linux). |
| virtualNetworkName | Virtuális hálózat a virtuális gép hálózati hello hello nevét kell tartoznia. hello nevét kell már létrehozott hello ugyanazt az erőforráscsoportot és hello VM ugyanazon a helyen. |
| subnetName | Hello alhálózatot a virtuális hálózat a virtuális gép hálózati hello hello nevét kell tartoznia. |
| vmSize | Hello virtuális gép méretét. Jelenleg csak a Standard A, a D és G adatsorozat támogatottak. |
| keyVaultResourceID | hello ResourceID hello kulcstároló erőforrásához az Azure Resource Manager azonosító. Hello PowerShell parancsmag használatával kaphat `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | Hello lemez-titkosítási kulcs, amely be van állítva a hello kulcstároló URL-CÍMÉT. |
| keyVaultKekUrl | Hello kulcs titkosítási kulcs jön létre hello lemez-titkosítási kulcs titkosításához URL-CÍMÉT. |
| vmName | Hello infrastruktúra-szolgáltatási virtuális gép neve. |

#### <a name="using-powershell-cmdlets"></a>PowerShell-parancsmagok használatával
Engedélyezheti lemeztitkosítás a titkosított virtuális merevlemezen hello PowerShell-parancsmaggal [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>Parancssori felület parancsai használatával
Ebben a forgatókönyvben a parancssori felület parancsait használva tooenable lemeztitkosítás hello a következő:

1. A kulcstároló hozzáférési házirendek meg:

   * Set hello **EnabledForDiskEncryption** jelző:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Állítsa be az engedélyeket tooAzure AD alkalmazás toowrite titkok tooyour kulcstároló:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. a titkosítás tooenable egy meglévő vagy a futó virtuális Gépre, típus:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Titkosítási állapot beolvasása:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. egy új virtuális Gépet, a titkosított virtuális merevlemezről, a következő paramétereket hello használata hello tooenable titkosítás `azure vm create` parancs:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Meglévő vagy az Azure-beli infrastruktúra-szolgáltatási Windows virtuális gép titkosításának engedélyezése
Ebben a forgatókönyvben engedélyezheti a hello Resource Manager-sablon, a PowerShell-parancsmagokkal vagy a parancssori felület parancsait használatával titkosított. hello alábbi szakaszok ismertetik részletesebben hogyan azt a Resource Manager-sablon és a parancssori felület parancsait hello tooenable.

#### <a name="using-hello-resource-manager-template"></a>Hello Resource Manager-sablon használatával
Engedélyezheti a meglévő vagy az Azure-beli infrastruktúra-szolgáltatási Windows virtuális gépek hello segítségével lemeztitkosítás [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello titkosítási konfigurációs hello **paraméterek** panelt, és kattintson **OK**.

2. Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** hello meglévő vagy az infrastruktúra-szolgáltatási virtuális gép fut tooenable titkosítás.

hello alábbi táblázat hello Resource Manager sablon paramétereinek meglévő vagy az Azure AD ügyfél-Azonosítót használó virtuális gépeken futó:

| Paraméter | Leírás |
| --- | --- |
| AADClientID | Engedélyek toowrite titkok toohello kulcstároló hello Azure AD alkalmazás ügyfél-azonosítója. |
| AADClientSecret | Hello Azure AD-alkalmazás, amely rendelkezik engedélyekkel toowrite titkok toohello kulcstároló titkos ügyfélkulcsot. |
| keyVaultName | Hello kulcs neve tároló adott hello BitLocker kulcs fel kell tölteni. Hello parancsmaggal kaphat `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Hello fő titkosítási kulcsát használt tooencrypt hello URL-CÍMÉT a BitLocker kulcs jön létre. Ez a paraméter nem kötelező, ha **nokek** hello UseExistingKek legördülő listában. Ha **kek** hello UseExistingKek legördülő listában, meg kell adnia hello _keyEncryptionKeyURL_ érték. |
| volumeType | Hello titkosítási műveletet végzi el a kötet típusa. Érvényes értékek a következők _OS_, _adatok_, és _összes_. |
| sequenceVersion | A BitLocker művelet hello feladatütemezési verzióját. Ez a verziószám növelése minden alkalommal, amikor egy lemezt-titkosítási művelet történik hello azonos virtuális gép. |
| vmName | Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre. |

> [!NOTE]
> _KeyEncryptionKeyURL_ egy nem kötelező paraméter. A key vaultban hello helyezheti a saját KEK toofurther védelmi hello adattitkosítási kulcsot (a BitLocker titkosítási titkos).

#### <a name="using-powershell-cmdlets"></a>PowerShell-parancsmagok használatával
Az Azure Disk Encryption titkosítás engedélyezése a PowerShell-parancsmagok használatával kapcsolatos információkért lásd: hello blogbejegyzések [Azure Disk Encryption megismerkedhet az Azure PowerShell - 1. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) és [Azure Disk Encryption felfedezés az Azure PowerShell - 2. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>Parancssori felület parancsai használatával
a meglévő vagy infrastruktúra-szolgáltatási Windows virtuális gép fut az Azure-ban a parancssori felület parancsait tooenable titkosítási hello a következő:

1. a key vaultban hello tooset a hozzáférési házirendek:
   * Set hello **EnabledForDiskEncryption** jelző:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Állítsa be az engedélyeket tooAzure AD alkalmazás toowrite titkok tooyour kulcstároló:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. egy meglévő vagy a futó virtuális gép tooenable titkosítás:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. tooget titkosítási állapotát:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. egy új virtuális Gépet, a titkosított virtuális merevlemezről, a következő paramétereket hello használata hello tooenable titkosítás `azure vm create` parancs:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Engedélyezheti a titkosítást egy már létező vagy nem futnak IaaS Linux virtuális Gépre az Azure-ban
Egy már létező vagy nem futnak IaaS Linux virtuális gépre az Azure-ban lemeztitkosítás hello használatával engedélyezheti [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Kattintson a **tooAzure telepítése** hello Azure gyors üzembe helyezési sablon, adja meg a hello hello titkosítási konfigurációs **paraméterek** panelt, és kattintson **OK**.

2. Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** hello meglévő vagy az infrastruktúra-szolgáltatási virtuális gép fut tooenable titkosítás.

hello a következő táblázat felsorolja a Resource Manager sablon paramétereinek meglévő vagy az Azure AD ügyfél-Azonosítót használó virtuális gépeken futó:

| Paraméter | Leírás |
| --- | --- |
| AADClientID | Engedélyek toowrite titkok toohello kulcstároló hello Azure AD alkalmazás ügyfél-azonosítója. |
| AADClientSecret | Hello Azure AD-alkalmazás, amely rendelkezik engedélyekkel toowrite titkok tooyour kulcstároló titkos ügyfélkulcsot. |
| keyVaultName | Hello kulcs neve tároló adott hello BitLocker kulcs fel kell tölteni. Hello parancsmaggal kaphat `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Hello fő titkosítási kulcsát használt tooencrypt hello URL-CÍMÉT a BitLocker kulcs jön létre. Ez a paraméter nem kötelező, ha **nokek** hello UseExistingKek legördülő listában. Ha **kek** hello UseExistingKek legördülő listában, meg kell adnia hello _keyEncryptionKeyURL_ érték. |
| volumeType | Hello titkosítási műveletet végzi el a kötet típusa. Érvényes támogatott értékek a következők _OS_ vagy _összes_ (az RHEL 7.2, CentOS 7.2 és Ubuntu 16.04), és _adatok_ (az összes többi terjesztéseket). |
| sequenceVersion | A BitLocker művelet hello feladatütemezési verzióját. Ez a verziószám növelése minden alkalommal, amikor egy lemezt-titkosítási művelet történik hello azonos virtuális gép. |
| vmName | Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre. |
| hozzáférési kód | Írjon be egy erős jelszót hello adattitkosítási kulcsot. |

> [!NOTE]
> _KeyEncryptionKeyURL_ egy nem kötelező paraméter. A key vaultban helyezheti a saját KEK toofurther védelmi hello adattitkosítási kulcsot (jelszó titkos).

#### <a name="cli-commands"></a>Parancssori felület parancsai
Engedélyezheti lemeztitkosítás a titkosított virtuális merevlemezen történő telepítéssel és használattal hello [CLI parancs](../cli-install-nodejs.md). meglévő vagy az Azure-beli infrastruktúra-szolgáltatási Linux virtuális gépek CLI-parancsok segítségével tooenable titkosítás hello a következő:

1. Állítsa be a hozzáférési házirendek hello a key vaultban:

 * Set hello **EnabledForDiskEncryption** jelző:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Állítsa be az engedélyeket tooAzure AD alkalmazás toowrite titkok tooyour kulcstároló:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. egy meglévő vagy a futó virtuális gép tooenable titkosítás:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Titkosítási állapot beolvasása:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. egy új virtuális Gépet, a titkosított virtuális merevlemezről, a következő paramétereket hello használata hello tooenable titkosítás `azure vm create` parancs:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-hello-encryption-status-of-an-encrypted-iaas-vm"></a>Hello titkosítási állapotát egy titkosított infrastruktúra-szolgáltatási virtuális gép beolvasása
Azure Resource Manager használatával kaphat hello titkosítási állapot [PowerShell-parancsmagok](/powershell/azure/overview), vagy a parancssori felület parancsait. hello a következő szakaszok azt ismertetik, hogyan toouse hello a klasszikus Azure portálon és parancssori felület parancsai tooget hello titkosítási állapottal.

#### <a name="get-hello-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Azure Resource Manager segítségével könnyebben nyerhet hello titkosítási állapotát egy titkosított Windows virtuális Gépre
Kaphat hello titkosítási hello infrastruktúra-szolgáltatási virtuális gép állapotát az Azure Resource Manager hello következő tevékenységek végrehajtásával:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://portal.azure.com/), és kattintson a **virtuális gépek** a hello bal oldali ablaktáblán toosee hello virtuális gépek az előfizetéshez összefoglaló áttekintést. Hello virtuális gépek nézet szűrheti hello hello előfizetés neve kiválasztásával **előfizetés** legördülő listából.

2. Hello hello tetején **virtuális gépek** kattintson **oszlopok**.

3. A hello **válasszon oszlop** panelen válassza **lemeztitkosítás**, és kattintson a **frissítés**. Megtekintheti az hello lemeztitkosítás oszlop ábrázoló hello titkosítási állapot _engedélyezve_ vagy _nincs engedélyezve a_ az egyes virtuális gépek, ahogy az ábra a következő hello:

 ![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-hello-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-hello-disk-encryption-powershell-cmdlet"></a>Egy titkosított (Windows/Linux) infrastruktúra-szolgáltatási virtuális gép hello titkosítási állapotát hello lemeztitkosítás PowerShell-parancsmag segítségével könnyebben nyerhet
Infrastruktúra-szolgáltatási virtuális gép hello hello titkosítási állapotát letölthető hello lemeztitkosítás PowerShell-parancsmag `Get-AzureRmVMDiskEncryptionStatus`. tooget hello fájltitkosítási beállításainak megadása a virtuális Gépet, írja be a hello következő:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Hello kimenete vizsgálhatja _Get-AzureRmVMDiskEncryptionStatus_ titkosítási kulcs az URL-címeket.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

hello OSVolumeEncrypted és a DataVolumesEncrypted beállítási értékei too_Encrypted_, amely mutatja, hogy mindkét kötet titkosítása Azure Disk Encryption használatával van beállítva. Az Azure Disk Encryption titkosítás engedélyezése a PowerShell-parancsmagok használatával kapcsolatos információkért lásd: hello blogbejegyzések [Azure Disk Encryption megismerkedhet az Azure PowerShell - 1. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) és [Azure Disk Encryption felfedezés az Azure PowerShell - 2. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> A Linux virtuális gépeken, hello három toofour percet vesz igénybe `Get-AzureRmVMDiskEncryptionStatus` parancsmag tooreport hello titkosítási állapottal.

#### <a name="get-hello-encryption-status-of-hello-iaas-vm-from-hello-disk-encryption-cli-command"></a>Hello lemeztitkosítás CLI parancs hello titkosítási állapotát hello infrastruktúra-szolgáltatási virtuális gép beolvasása
Hello titkosítási állapotát hello infrastruktúra-szolgáltatási virtuális gép hello lemeztitkosítás CLI parancs használatával beszerezheti `azure vm show-disk-encryption-status`. tooget hello fájltitkosítási beállításainak megadása a virtuális Gépet, adja meg a Azure CLI-munkamenethez:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Tiltsa le a titkosítást a windowsos infrastruktúra-szolgáltatási virtuális gép futó
Tiltsa le a titkosítást egy futó Windows vagy Linux rendszerű infrastruktúra-szolgáltatási virtuális hello Azure lemez titkosítási Resource Manager-sablon vagy a PowerShell-parancsmagok használatával, és hello visszafejtési konfiguráció megadásához.

##### <a name="windows-vm"></a>Windowsos VM
hello disable-titkosítás lépés letiltja a titkosítást hello az operációs rendszer, hello adatmennyiség vagy mindkét szolgáltatás fut a Windows alapú infrastruktúra-szolgáltatási virtuális gép hello. Nem hello operációs rendszer kötetén tiltása, és nem titkosított hello adatmennyiség hagyja. Hello disable-titkosítás a lépést, ha hello Azure klasszikus üzembe helyezési modellel frissíti hello VM modell, és IaaS-VM Windows hello visszafejtett van megjelölve. hello tartalmát hello virtuális gép már nem titkosított aktívan. hello visszafejtési nem törli a kulcs tárolóban és hello titkosítási kulcsokat tárol (a BitLocker titkosítási kulcsokat a Windows és Linux jelszava).

##### <a name="linux-vm"></a>Linux virtuális gép
hello disable-titkosítás lépés letiltja a titkosítást hello adatok kötetének hello futó Linux infrastruktúra-szolgáltatási virtuális gép. Ez a lépés csak akkor működik, ha hello operációsrendszer-lemez nem titkosított.

> [!NOTE]
> Hello operációsrendszer-lemez titkosításának letiltása a Linux virtuális gépeken nem engedélyezett.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Tiltsa le a titkosítást egy már létező vagy nem futnak IaaS virtuális gépen
Letilthatja a Windows IaaS virtuális gépeken futó hello segítségével lemeztitkosítás [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello visszafejtési konfigurációs hello **paraméterek** panelt, és kattintson **OK**.

2. Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** tooenable titkosítási egy új IaaS virtuális gépen.

Linux virtuális gépekhez, letilthatja titkosítási hello segítségével [tiltsa le a titkosítást a futó Linux virtuális gép](https://aka.ms/decrypt-linuxvm) sablont.

hello alábbi táblázat Resource Manager sablon paramétereinek egy infrastruktúra-szolgáltatási virtuális gép titkosításának letiltása:

| Paraméter | Leírás |
| --- | --- |
| vmName | Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre.
| volumeType | A visszafejtési művelet végrehajtott kötet típusa. Érvényes értékek a következők _OS_, _adatok_, és _összes_. A windowsos infrastruktúra-szolgáltatási virtuális gép operációs rendszer vagy rendszerindító kötet futó letiltása hello titkosítás nélküli titkosítás nem tiltható le _adatok_ kötet. Ne feledje, hogy az operációsrendszer-lemez hello titkosításának letiltása nem engedélyezett a Linux virtuális gépeken. |
| sequenceVersion | A BitLocker művelet hello feladatütemezési verzióját. Ez a verziószám növelése minden alkalommal, amikor a lemez visszafejtési művelet történik hello azonos virtuális gép. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Tiltsa le a titkosítást egy már létező vagy nem futnak IaaS virtuális gépen
egy már létező vagy nem futnak infrastruktúra-szolgáltatási virtuális gép hello PowerShell-parancsmag segítségével toodisable titkosítás lásd [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Ez a parancsmag a Windows és Linux virtuális gépeket is támogatja. toodisable titkosítási telepíti egy bővítmény hello virtuális gépen. Ha hello _neve_ paraméter nincs megadva, hello alapértelmezett nevű bővítménye _AzureDiskEncryption a Windows virtuális gépek_ jön létre.

A Linux virtuális gépeken hello AzureDiskEncryptionForLinux bővítmény szolgál.

> [!NOTE]
> Ez a parancsmag hello virtuális gép újraindul.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Engedélyezze a titkosítást a Azure felügyelt lemezes előzetes titkosítással IaaS virtuális gépen
Használja a hello Azure kezelt lemez ARM sablon toocreate hello ARM-sablon használatával előre titkosított virtuális Merevlemezt egy titkosított virtuális gép helye:   
[Előzetes titkosítással virtuális merevlemez tárolási blob hozhat létre új titkosított felügyelt lemezt] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Engedélyezheti a titkosítást egy új Linux IaaS virtuális Gépet az Azure Managed lemez
Hello Azure kezelt lemez ARM sablon toocreate új Linux infrastruktúra-szolgáltatási virtuális gép helye: hello ARM-sablon használatával titkosítva használja   
[A teljes lemez titkosítása RHEL 7.2 telepítését] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Engedélyezheti a titkosítást egy új Windows IaaS virtuális Gépet az Azure Managed lemez
 Hello Azure kezelt lemez ARM sablon toocreate új Linux infrastruktúra-szolgáltatási virtuális gép helye: hello ARM-sablon használatával titkosítva használja   
 [Új titkosított Windows IaaS kezelt lemez virtuális gép létrehozása gyűjtemény lemezképből] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Kötelező toosnapshot, és/vagy biztonsági mentési felügyelt lemezes alapú kívül a Virtuálisgép-példány és a korábbi tooenabling Azure Disk Encryption.  Hello felügyelt lemezes pillanatképet átvihető hello portálról, vagy az Azure Backup használható.  Biztonsági mentések győződjön meg arról, hogy egy helyreállítási lehetőséget a hello esetben bármely váratlan hiba lehetséges titkosítás során.  Biztonsági másolat elkészítése után a Set-AzureRmVMDiskEncryptionExtension hello parancsmag lehet használt tooencrypt kezelt lemezek hello - skipVmBackup paraméter megadásával.  Ez a parancs elleni felügyelt lemezes virtuális gép sikertelen lesz, amíg a biztonsági mentést készített, és ez a paraméter van megadva.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Meglévő titkosított nem prémium szintű virtuális titkosítási beállításainak módosítása
  Használjon hello meglévő Azure lemez támogatott titkosítási felületek a futó virtuális gép [PS parancsmagok, CLI és ARM-sablonok] tooupdate hello titkosítási beállításait, például AAD ügyfél azonosítója/titkos, fő titkosítási kulcs [KEK], a BitLocker titkosítási kulcs Windows virtuális gép vagy a jelszót Linux virtuális gép stb hello frissítés titkosítási beállítás csak a nem prémium szintű storage által támogatott virtuális gépek esetén támogatott. Prémium szintű storage által támogatott virtuális gépek esetén támogatott NNOT.

## <a name="appendix"></a>Függelék:
### <a name="connect-tooyour-subscription"></a>Csatlakozás tooyour előfizetés
Mielőtt folytatná, tekintse át a hello *Előfeltételek* című részben. Miután meggyőződött arról, hogy a szükséges előfeltételek maradéktalanul teljesülnek, csatlakozás tooyour előfizetés hello következő tevékenységek végrehajtásával:

1. Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancsot:

    `Login-AzureRmAccount`

2. Ha több előfizetéssel rendelkezik, és szeretné, hogy egy toouse toospecify, írja be a fiókhoz toosee hello előfizetések a következő hello:

    `Get-AzureRmSubscription`

3. azt szeretné, hogy toouse, toospecify hello előfizetés típusa:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. hogy helyesek-e a konfigurált hello előfizetés tooverify, írja be:

    `Get-AzureRmSubscription`

5. tooconfirm hello Azure Disk Encryption parancsmagjai telepítve vannak, típus:

    `Get-command *diskencryption*`

6. a következő kimeneti hello megerősíti, hogy hello Azure lemez titkosítási PowerShell telepítési:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Előzetes titkosítással Windows virtuális merevlemez előkészítése
hello szakaszok szükséges tooprepare előzetes titkosítással Windows virtuális központi telepítésben egy Azure IaaS titkosított VHD-t is. Hello információk tooprepare használja és az Azure Site Recovery vagy Azure friss Windows virtuális gép (VHD).

#### <a name="update-group-policy-tooallow-non-tpm-for-os-protection"></a>Csoport házirend tooallow nem TPM-OS Protection frissítése
Hello a BitLocker csoportházirend-beállítás konfigurálása **a BitLocker meghajtótitkosítás**, amely alatt található **helyi számítógép-házirend** > **számítógép konfigurációja**  >  **Felügyeleti sablonok** > **Windows-összetevők**. Ez a beállítás túl**operációsrendszer-meghajtók** > **indításkor további hitelesítést** > **kompatibilisTPMnélküliBitLockerengedélyezése**, ahogy az ábra a következő hello:

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>A BitLocker-szolgáltatás összetevőinek telepítése
A Windows Server 2012 és újabb verziók használja a következő parancs hello:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

A Windows Server 2008 R2 a következő parancs hello használata:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-hello-os-volume-for-bitlocker-by-using-bdehdcfg"></a>A BitLocker használatával hello rendszerkötet előkészítése`bdehdcfg`
toocompress hello OS partíció, és hello gép előkészítése a BitLocker, hajtsa végre a következő parancs hello:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-hello-os-volume-by-using-bitlocker"></a>A BitLocker használatával hello operációsrendszer-kötet védelme
Használjon hello [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) parancs tooenable titkosítást használ egy külső kulcsvédő hello rendszerindító köteten. Hello külső meghajtók vagy kötetek is elhelyezhető hello külső kulcs (.bek fájl). Engedélyezve van hello rendszerlemez vagy rendszerindító köteten hello tovább a rendszer újraindítása után.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> Készítsen elő egy külön adatok és az erőforrások virtuális Merevlemezt a virtuális gép hello hello külső kulcs kapcsolódnak a BitLocker használatával.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Az operációs rendszer meghajtóján futó Linux virtuális gép titkosítása
A Linux virtuális gép egy operációs rendszer meghajtójának titkosítás funkciót támogatja, a következő terjesztéseket hello:

* 7.2 RHEL
* 7.2 centOS
* Ubuntu 16.04

##### <a name="prerequisites-for-os-disk-encryption"></a>Az operációs rendszer lemeztitkosítás előfeltételei

* virtuális gép hello hello Piactéri lemezkép az Azure Resource Manager kell létrehozni.
* Az Azure virtuális gép, és legalább 4 GB RAM (mérete 7 GB ajánlott).
* (Az RHEL és CentOS) Tiltsa le a SELinux. toodisable SELinux, tekintse meg a "4.4.2. Letiltása SELinux"hello a [SELinux felhasználói és rendszergazdai útmutató](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) a virtuális gép hello.
* Letiltja a SELinux, újraindítása után hello virtuális gép legalább egy alkalommal.

##### <a name="steps"></a>Lépések
1. Hozzon létre egy virtuális Gépet egy korábban megadott hello terjesztéseket.

 7.2 – CentOS operációsrendszer-lemez titkosítása támogatott keresztül egy különös lemezképet. toouse a lemezképet, adja meg a "7.2n", SKU hello hello virtuális gép létrehozásakor:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Hello VM függően tooyour kell konfigurálni. Tooencrypt összes hello (az operációs rendszer + adatok) meghajtókat is, ha hello adatmeghajtókon a megadott és a /etc/fstab csatlakoztatható toobe van szükség.

 > [!NOTE]
 > Használja az UUID =... toospecify adatmeghajtókon a/etc/fstab hello blokk eszköz neve (például /dev/sdb1) megadása helyett. Titkosítás során meghajtók hello sorrendjének módosítja, a virtuális gép hello. Ha a virtuális Gépet a megadott sorrendben blokk eszközök támaszkodik, meghiúsul toomount titkosítás után őket.

3. Jelentkezzen ki hello SSH-munkamenetet.

4. tooencrypt hello az operációs rendszer, adja meg, mint volumeType **összes** vagy **az operációs rendszer** amikor Ön [engedélyezheti a titkosítást](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Minden felhasználói térben futó folyamatok, nem mint `systemd` rendelkező szolgáltatások el kell egy `SIGKILL`. Indítsa újra a virtuális gép hello. Ha engedélyezi az operációs rendszer lemeztitkosítás egy futó virtuális gépen, tervezze meg a virtuális gép állásidő.

5. Rendszeres időközönként előrehaladást hello titkosítási hello vonatkozó utasításokat használva a hello [következő szakasz](#monitoring-os-encryption-progress).

6. Get-AzureRmVmDiskEncryptionStatus "VMRestartPending" jeleníti meg, miután tooit bejelentkezés vagy hello portálon, a PowerShell vagy a parancssori felület használatával indítsa újra a virtuális Gépet.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot hello VM
    ```
Ahhoz, hogy újraindította, azt javasoljuk, hogy menti [rendszerindítási diagnosztika](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) a virtuális gép hello.

#### <a name="monitoring-os-encryption-progress"></a>Az operációs rendszer titkosítási folyamat figyelése
Figyelheti, hogy az operációs rendszer titkosítási folyamat három módon:

* Használjon hello `Get-AzureRmVmDiskEncryptionStatus` parancsmag és hello Feladatnézetben mező vizsgálata:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Miután hello VM eléri "Az operációs rendszer lemezén lépések titkosítási", körülbelül 40 vesz igénybe egy prémium szintű storage too50 percek biztonsági VM.

 Mert [#388 ki](https://github.com/Azure/WALinuxAgent/issues/388) WALinuxAgent, a `OsVolumeEncrypted` és `DataVolumesEncrypted` megjelennek, `Unknown` az egyes terjesztési. A WALinuxAgent verzió 2.1.5 és újabb verziók, ez a probléma fennáll automatikusan. Ha látja `Unknown` hello kimeneti ellenőrizheti lemeztitkosítás állapot hello Azure erőforrás-kezelő használatával.

 Nyissa meg túl[Azure erőforrás-kezelő](https://resources.azure.com/), majd bontsa ki a bal oldali panelen a hello kiválasztása ebben a hierarchiában:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 Hello InstanceView görgessen le a meghajtók toosee hello titkosítási állapotát.

 ![Virtuális gép példányait tartalmazó nézet](./media/azure-security-disk-encryption/vm-instanceview.png)

* Nézze meg [rendszerindítási diagnosztika](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/). Hello ADE bővítmény üzeneteit kell előtagként `[AzureDiskEncryption]`.

* Jelentkezzen be a virtuális gép SSH-kapcsolaton keresztül toohello, és hello bővítmény napló az beszerzése:

    /var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Azt javasoljuk, hogy nem bejelentkezik toohello VM közben az operációs rendszer titkosítás folyamatban van. Hello naplók másolása, csak ha hello más két módszer sikertelen.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Előzetes titkosítással Linux virtuális merevlemez előkészítése
##### <a name="ubuntu-16"></a>Ubuntu 16
Hello terjesztési telepítése közben titkosítás konfigurálása hello következő tevékenységek végrehajtásával:

1. Válassza ki **titkosított kötetek konfigurálása** amikor hello lemezek particionálása-e.

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Hozzon létre egy külön rendszerindítási meghajtót, amely nem lesznek titkosítva. A legfelső szintű meghajtó titkosítására.

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Adjon meg egy jelszót. Ez az, hogy töltsön toohello kulcstároló hello jelszót.

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Fejezze be a particionálást.

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Hello virtuális gép rendszerindítási és egy jelszót a rendszer felkéri, használja a 3. lépésben megadott hello jelszót.

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Hello virtuális gép előkészítése az Azure használatával történő feltöltése [ezeket az utasításokat](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Ne futtassa a még hello utolsó lépése (megszüntetési hello VM).

Titkosítási toowork konfigurálása az Azure-ral hello következő tevékenységek végrehajtásával:

1. Hozzon létre egy fájlt, a /usr/local/sbin/azure_crypt_key.sh, és a következő parancsfájl hello hello tartalom. Nagy figyelmet toohello KeyFileName, mert hello Azure által használt jelszót fájlnév.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED toofind suitable passphrase file ..." >&2
        echo -n "Try tooenter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. A hello a titkosítási config módosítása */etc/crypttab*. A listának így kell kinéznie:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Ha az éppen szerkesztett *azure_crypt_key.sh* Windows, és másolja át tooLinux futtatása `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Végrehajtható engedélyek toohello parancsfájl hozzáadása:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Szerkesztés */etc/initramfs-tools/modules* sorok hozzáfűzésével:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Futtatás `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` érvénybe léptetéséhez.

7. Most hello méretű képes kiosztásának megszüntetése.

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Továbbra is a következő lépés toohello és [a virtuális merevlemez feltöltéséhez](#upload-encrypted-vhd-to-an-azure-storage-account) az Azure.

##### <a name="opensuse-132"></a>openSUSE, 13.2
hello terjesztési a telepítés során tooconfigure titkosítási hello a következő:
1. Partícióazonosító hello lemezek, válassza ki **titkosítása kötet csoport**, majd írja be a jelszót. Ez az, hogy fel kell töltenie tooyour kulcstároló hello jelszót.

 ![openSUSE, 13.2 beállítása](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Rendszerindító hello VM az Ön jelszavát.

 ![openSUSE, 13.2 beállítása](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Prepare VM hello tooAzure feltöltése hello utasításait követve [SLES vagy openSUSE virtuális gép előkészítése Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Ne futtassa a még hello utolsó lépése (megszüntetési hello VM).

az Azure-tooconfigure titkosítási toowork hello a következő:
1. Hello /etc/dracut.conf szerkesztése, és adja hozzá a következő sor hello:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Ezek a sorok hello hello végéig megjegyzésbe /usr/lib/dracut/modules.d/90crypt/module-setup.sh fájl:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. A következő sor hello fájl /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello elején hello hozzáfűzése:
 ```
    DRACUT_SYSTEMD=0
 ```
És összes előfordulását módosítsa:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
erre:
```
    if [ 1 ]; then
```
4. /Usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh szerkesztése és fűznünk túl "# nyitott LUKS eszköz":

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Futtatás `/usr/sbin/dracut -f -v` tooupdate hello initrd.

6. Most azt is kiosztásának megszüntetése hello VM és [a virtuális merevlemez feltöltéséhez](#upload-encrypted-vhd-to-an-azure-storage-account) az Azure.

##### <a name="centos-7"></a>CentOS 7
hello terjesztési a telepítés során tooconfigure titkosítási hello a következő:
1. Válassza ki **az adatok titkosítása** amikor particionálni a lemezeket.

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Győződjön meg arról, hogy **titkosítása** van kiválasztva a legfelső szintű partíció.

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Adjon meg egy jelszót. Ez az, hogy fel kell töltenie tooyour kulcstároló hello jelszót.

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Hello virtuális gép rendszerindítási és egy jelszót a rendszer felkéri, használja a 3. lépésben megadott hello jelszót.

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Prepare hello VM feltöltése az Azure hello "CentOS 7.0 +" utasításait használatával [CentOS-alapú virtuális gép előkészítése Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Ne futtassa a még hello utolsó lépése (megszüntetési hello VM).

6. Most azt is kiosztásának megszüntetése hello VM és [a virtuális merevlemez feltöltéséhez](#upload-encrypted-vhd-to-an-azure-storage-account) az Azure.

az Azure-tooconfigure titkosítási toowork hello a következő:

1. Hello /etc/dracut.conf szerkesztése, és adja hozzá a következő sor hello:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Ezek a sorok hello hello végéig megjegyzésbe /usr/lib/dracut/modules.d/90crypt/module-setup.sh fájl:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. A következő sor hello fájl /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello elején hello hozzáfűzése:
```
    DRACUT_SYSTEMD=0
```
És összes előfordulását módosítsa:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
erre:
```
    if [ 1 ]; then
```
4. /Usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh szerkesztése és hozzáfűzni a "# nyitott LUKS eszköz" hello után:
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Futtassa a hello "/ usr/sbin/dracut - f - v" tooupdate hello initrd.

![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-tooan-azure-storage-account"></a>Töltse fel a titkosított virtuális merevlemez tooan Azure storage-fiók
Miután a BitLocker-titkosítást vagy a DM-Crypt titkosítás engedélyezve van, hello helyi titkosított VHD igényeinek feltöltött toobe tooyour tárfiók.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-hello-disk-encryption-secret-for-hello-pre-encrypted-vm-tooyour-key-vault"></a>Lemeztitkosítás titkos kulcsát hello hello előzetes titkosítással VM tooyour kulcstároló feltöltése
hello lemeztitkosítás secret beszerzett korábban fel kell tölteni, mint a key vaultban lévő titkos kulcs. hello kulcstároló kell toohave lemeztitkosítás és az Azure AD-ügyfél engedélyezve.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Lemez titkosítási titkos kulcs nem titkosított a KEK
a kulcstároló, használja a hello titkos másolatot tooset [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Ha egy Windows rendszerű virtuális gép, hello bek fájl base64 karakterláncként van kódolva, és majd feltöltött tooyour kulcstároló hello segítségével `Set-AzureKeyVaultSecret` parancsmag. Linux hello jelszó base64 karakterláncként van kódolva, és majd feltöltött toohello kulcstároló. Ezenkívül győződjön meg arról, hogy a következő címkék hello vannak beállítva hello kulcstároló hello titkos kulcs létrehozásakor.

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Használjon hello `$secretUrl` hello következő lépésben a [hello OS lemez csatolása KEK használata nélkül](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Lemez titkosítási titkos kulcsot egy KEK titkosítva
Mielőtt feltölti hello titkos toohello kulcstároló, opcionálisan titkosításhoz kulcs titkosítási kulcs használatával. Használjon hello sortörés [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst hello kulcs titkosítási kulcs használatával hello titkos kulcs titkosításához. hello sortörés művelet eredménye a base64 kódolású URL karakterláncot, majd feltöltheti egy titkos kulcsként hello segítségével [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) parancsmag.

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

Használjon `$KeyEncryptionKey` és `$secretUrl` hello következő lépésben a [hello operációsrendszer-lemez csatolása KEK használatával](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Adjon meg egy titkos URL-cím, egy operációsrendszer-lemez csatolása
#### <a name="without-using-a-kek"></a>Egy KEK használata nélkül
Hello operációsrendszer-lemez konfigurálhatóak, amíg toopass kell `$secretUrl`. hello URL-cím hello "lemez-titkosítás a KEK nem titkosított titkos" szakaszban hozott létre.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Egy KEK használatával
Hello operációsrendszer-lemez csatolása átadni `$KeyEncryptionKey` és `$secretUrl`. hello URL-cím hello "lemez-titkosítás a KEK nem titkosított titkos" szakaszban hozott létre.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Ez az útmutató letöltése
Ez az útmutató letölthető hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

## <a name="for-more-information"></a>További információ
[Fedezze fel az Azure Disk Encryption Azure PowerShell - 1. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Fedezze fel az Azure Disk Encryption Azure PowerShell - 2. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
