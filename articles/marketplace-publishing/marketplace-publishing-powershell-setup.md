---
title: "PowerShell toocreate hello piactér virtuális gép mentése aaaSet |} Microsoft Docs"
description: "Beállítása az Azure PowerShell és a választható folytatásához használja azt egymást követő toocreate méretű képek toodeploy, és a eladásra, hello Azure piactéren"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: cd2ebad7472248b8f921706e1a8c82d41f33b9cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-powershell-toocreate-an-offer-for-hello-azure-marketplace"></a>Állítsa be az Azure PowerShell toocreate hello Azure piactér vonatkozó ajánlatot
Részletes információt az Azure PowerShell mentése tooset lásd: [hogyan tooinstall és konfigurálása az Azure PowerShell](/powershell/azure/overview). Egy egyszerű módszer toouse hello tanúsítvány metódus, amely tölti le, és importálja a hitelesítéshez szükséges tanúsítvány. tooobtain hello szükséges tanúsítvány, használja a hello **Get-AzurePublishSettingsFile** parancsmag. Amikor a rendszer kéri, mentse a hello fájlt. egy PowerShell-munkamenetbe, használjon hello tooimport hello tanúsítvány **Import-AzurePublishSettingsFile** parancsmag.

tooconfigure és a tároló hello közös Microsoft Azure-előfizetések beállításait hello PowerShell-munkamenethez, használja a hello **Set-AzureSubscription** és **válasszon-AzureSubscription** parancsmagokat:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

hello első parancs hozzárendeli egy alapértelmezett tárfiókot hello előfizetés (néhány virtuális gép üzembe helyezési műveletekhez szükséges).  hello második teszi hello előfizetés hello jelenlegivel (ismeri fel más parancsmagok).

## <a name="see-also"></a>Lásd még:
* [Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)
* [Hello piactér a virtuális gép lemezkép létrehozása](marketplace-publishing-vm-image-creation.md)

