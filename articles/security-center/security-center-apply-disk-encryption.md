---
title: "lemeztitkosítás aaaApply az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** lemez titkosítási ** alkalmazni."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: cd803f1120018c5c86da91186eec1e59d425ede7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Az Azure Security Centerben lemeztitkosítás alkalmazása
Az Azure Security Center azt javasolja, hogy a lemez titkosítása alkalmazni, ha a Windows vagy Linux rendszerű virtuális lemezek nem titkosított Azure Disk Encryption használatával. Adatok titkosítása lehetővé teszi a Windows és Linux rendszerű infrastruktúra-szolgáltatási virtuális lemezek titkosításához.  Titkosítási ajánlott hello az operációs rendszer, mind az adatkötetek a virtuális gépen.

Lemeztitkosítás használ hello iparági szabvány [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) és a Windows hello szolgáltatás [DM-crypt program segítségével](https://en.wikipedia.org/wiki/Dm-crypt) Linux szolgáltatása. Ezeket a szolgáltatásokat biztosítanak az operációs rendszer és az adatok titkosítás toohelp védelme és az adatok védelme és szervezeti biztonsági és megfelelőségi jár kötelezettségekkel. Lemeztitkosítás integrálva van [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp meg vezérlésére és felügyeletére hello lemez titkosítási kulcsok és titkos kulcsokat a Key Vault az előfizetést, biztosítva, hogy a virtuális gépek lemezei hello az összes adat titkosítva legyenek a lévő,aktívan[ Az Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Az Azure Disk Encryption a következő Windows server operációs rendszerek – Windows Server 2008 R2, Windows Server 2012 és Windows Server 2012 R2 hello támogatott. Lemeztitkosítás Linux server operációs rendszerek - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) a következő hello támogatott.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok** panelen válassza **lemeztitkosítás alkalmazása**.
2. A hello **lemeztitkosítás alkalmazása** panelen, amelynek lemeztitkosítás ajánlott virtuális gépek listájának megtekintéséhez.
3. Hajtsa végre a hello utasításokat tooapply titkosítási toothese virtuális gépeket.

![][1]

tooencrypt, titkosítást igénylő Security Center által azonosított Azure virtuális gépek, ajánlott hello a következő lépéseket:

* Az Azure PowerShell telepítése és konfigurálása. Ez lehetővé teszi toorun hello PowerShell parancsok szükséges tooset hello Előfeltételek szükséges tooencrypt Azure virtuális gépek fel.
* Szerezze be és hello Azure lemez titkosítási Előfeltételek Azure PowerShell-parancsfájlt futtathat.
* A virtuális gépek titkosítása.

[Azure virtuális gép titkosítása](security-center-disk-encryption.md) végigvezeti a lépéseken.  Ez a témakör feltételezi, hogy a Windows 10, amelyen lemeztitkosítás konfigurálhat hello ügyfélszámítógépen.

Nincsenek számos különböző módszer használható az Azure virtuális gépek. Ha már jól ismeri az Azure PowerShell vagy Azure CLI-t, majd célszerű toouse más megközelítést választani. Ezek más módszerekkel kapcsolatos toolearn lásd [Azure disk encryption](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Lásd még:
Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "alkalmaz lemeztitkosítás." További információ az adatok titkosítása toolearn hello következő lásd:

* [Titkosítás és a kulcs-kezelés az Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (videó, 36 perc 39 másodperc) – megtudhatja, hogyan toouse lemez-e titkosítási felügyeleti az infrastruktúra-szolgáltatási virtuális gépek és az Azure Key Vault toohelp és megvédeni az adatok.
* [Azure virtuális gép titkosítása](security-center-disk-encryption.md) (dokumentumok) – megtudhatja, hogyan tooencrypt Azure virtuális gépeken.
* [Az Azure lemeztitkosítás](../security/azure-security-disk-encryption.md) (dokumentumok) – megtudhatja, hogyan tooenable lemeztitkosítás Windows és Linux virtuális gépek.

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendeket.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
