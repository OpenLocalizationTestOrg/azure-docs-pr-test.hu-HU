---
title: "mentése a DPM-munkaterhelések tooAzure klasszikus portál aaaBack |} Microsoft Docs"
description: "Egy bevezető toobacking hello Azure Backup szolgáltatás használatával a DPM-kiszolgáló"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: "A System Center Data Protection Manager, a data protection manager, a dpm biztonsági mentése"
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: f408957db69d45f745d5e89bd97030a341405b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-tooazure-with-dpm"></a>Mentése a dpm-mel munkaterhelések tooAzure tooback előkészítése
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Az Azure Backup Server (klasszikus)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasszikus)](backup-azure-dpm-introduction-classic.md)
>
>

A cikkben egy bevezető toousing a Microsoft Azure Backup szolgáltatás tooprotect, a System Center Data Protection Manager (DPM) kiszolgálók és munkaterhelések. Az olvasásával lesz tisztában:

* Hogyan működik a Azure DPM-kiszolgáló biztonsági mentése
* hello Előfeltételek tooachieve egy zökkenőmentes biztonsági mentés élmény
* hello tipikus hibák történtek, és hogyan velük toodeal
* Támogatott helyzetek

A System Center DPM fájl-és alkalmazásadatok készít. TooDPM biztonsági másolatba mentett adatok tárolása a lemezen, szalagon, vagy biztonsági másolat tooAzure a Microsoft Azure Backup szolgáltatásnál. A DPM a következőképpen együttműködik az Azure Backup szolgáltatással:

* **Fizikai kiszolgáló vagy a helyszíni virtuális gépként telepített DPM** – Ha a DPM fizikai kiszolgálóként vagy helyszíni Hyper-V virtuális gépként készíthet biztonsági mentése Azure biztonsági mentési adatok tooan tároló továbbá toodisk, és szalagos biztonsági mentés.
* **Az Azure virtuális gépként telepített DPM** – a System Center 2012 R2 Update 3, a DPM telepíthető Azure virtuális gépként. Ha a DPM központi telepítése egy Azure virtuális gépként készíthet biztonsági mentést tooAzure adatlemezek toohello DPM Azure virtuális géphez csatolt vagy által a biztonsági mentés tooan Azure mentési tárolóba is kiszervezése a hello adattárolás.

## <a name="why-backup-your-dpm-servers"></a>Miért érdemes biztonsági másolatot készíteni a DPM-kiszolgáló?
az Azure Backup használatával biztonsági mentéséről a DPM-kiszolgálók hello üzleti előnyöket nyújtja:

* A helyszíni DPM-telepítés az Azure biztonsági mentés akár egy alternatív toolong-kifejezés telepítési tootape is használhatja.
* A DPM az Azure-ban történő telepítés esetén Azure Backup szolgáltatás lehetővé teszi hello Azure lemez toooffload tárhelyhez lehetővé tooscale mentése régebbi adatokat tárol az Azure biztonsági mentés és a lemezen lévő új adatokat.

## <a name="how-does-dpm-server-backup-work"></a>Hogyan működik a DPM-kiszolgáló biztonsági mentése?
tooback be a virtuális gépet, az első pont időponthoz kötött pillanatképet hello adatokról van szükség. hello Azure Backup szolgáltatás kezdeményezi hello biztonsági mentési feladat hello következő ütemezett időpontban, és eseményindítók hello tartalék mellék tootake pillanatképet. tartalék mellék koordináták hello hello vendégen VSS tooachieve konzisztencia szolgáltatásra, és meghívja a hello blob pillanatkép API-ja hello Azure Storage szolgáltatás konzisztencia elérése után. Ez kész tooget hello lemezek hello virtuális gép, konzisztens pillanatképének anélkül, hogy tooshut le azt.

Miután hello pillanatkép vették, hello adatátvitel által hello Azure Backup szolgáltatás toohello mentési tároló. hello szolgáltatás azonosítja, és csak a legfrissebb biztonsági másolatból hello hello biztonsági másolatok tárolási és hálózati hatékony és módosított hello blokkok átvitele gondoskodik. Hello adatátvitel befejezése után hello pillanatkép törlődik, és egy helyreállítási pontot hoz létre. Ez a helyreállítási pont látható a klasszikus Azure portálon hello.

> [!NOTE]
> Linux virtuális gépek esetében kizárólag fájlkonzisztens biztonsági mentés lehetőség.
>
>

## <a name="prerequisites"></a>Előfeltételek
Az alábbiak szerint készítse elő a DPM-adatok Azure biztonsági mentés tooback:

1. **Hozzon létre egy biztonsági mentési tárolót**. Ha az előfizetés még nem hozott létre a biztonsági másolatok tárolóját, tekintse meg a hello Azure portál verziója – Ez a cikk [tooback mentése a dpm-mel munkaterhelések tooAzure előkészítése](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > 2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.
  > Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
  >- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
  >- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
  >

2. **Töltse le a tárolói hitelesítő adatokat** – az Azure Backup szolgáltatásban toohello tároló létrehozott tanúsítványt feltöltés hello.
3. **Hello Azure biztonságimásolat-készítő ügynök telepítése és regisztrálása hello server** – az Azure biztonsági mentési, hello ügynök telepítése minden DPM-kiszolgálón és hello DPM-kiszolgáló regisztrálása hello mentési tároló.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Követelmények (és korlátozások)
* DPM fizikai kiszolgálóként vagy a System Center 2012 SP1 vagy a System Center 2012 R2 rendszeren telepített Hyper-V virtuális gép futtathat. Is futhat egy Azure virtuális gépként legalább fut a System Center 2012 R2 DPM 2012 R2 3. kumulatív frissítéssel vagy egy Windows virtuális gépként VMWare legalább fut a System Center 2012 R2 kumulatív frissítés 5.
* Ha a System Center 2012 SP1 DPM használ, telepítenie kell a System Center Data Protection Manager SP1 2. kumulatív frissítést. Erre azért szükség, hello Azure Backup szolgáltatás ügynökének telepítése előtt.
* hello DPM-kiszolgálóval kell rendelkeznie a Windows PowerShell és a .net Framework 4.5 telepítve.
* A DPM biztonsági legtöbb munkaterhelések tooAzure biztonsági mentését is. A teljes listáját lásd: Mi van támogatott hello Azure biztonsági mentés támogatja az alábbi elemek.
* Azure biztonsági mentés tárolt adatok nem állíthatók hello "másolható tootape" beállítással.
* Hello Azure Backup szolgáltatás engedélyezve van az Azure-fiókra lesz szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ a [Azure Backup árairól](https://azure.microsoft.com/pricing/details/backup/).
* Az Azure Backup használatához hello Azure Backup szolgáltatás ügynökének toobe tooback akarja hello kiszolgálókon telepítve. Minden olyan kiszolgálón kell hello adatok, amelyek biztonsági mentése van folyamatban, rendelkezésre álló szabad helyi tárolóként hello mérete legalább 10 %. Például 10 GB szabad lemezterület hello ideiglenes helyen legalább 100 GB adat másolatának igényel. 10 %-os minimális hello pedig 15 % szabad helyi tárterület terület toobe hello gyorsítótárazáshoz használt ajánlott.
* Adatokat tároló Azure storage hello tárolódnak. Nincs korlát toohello adatmennyiség készíthet biztonsági mentést tooan Azure Backup-tárolóban van, de az adatforrás (például egy virtuális géphez vagy adatbázis) hello mérete nem haladhatja meg a 54,400 GB.

Ilyen típusú biztonsági mentését tooAzure támogatja:

* Titkosított (csak teljes biztonsági mentések)
* Tömörített (növekményes biztonsági mentések támogatva)
* Ritka (növekményes biztonsági mentések támogatva)
* Tömörített és ritka (Ritkaként kezelve)

És ezek nem támogatottak:

* A kis-és nagybetűket fájlrendszerek kiszolgálók nem támogatottak.
* A rögzített hivatkozások (kimaradnak)
* Újraelemzési pontok (kimaradnak)
* Titkosított és tömörített (kimaradnak)
* Titkosított és ritka (kimaradnak)
* Tömörített adatfolyam
* Ritka adatfolyam

> [!NOTE]
> A System Center 2012 DPM SP1-től, a készíthet biztonsági másolatot a Microsoft Azure Backup szolgáltatás használatával DPM tooAzure által védett munkaterhelések be.
>
>
