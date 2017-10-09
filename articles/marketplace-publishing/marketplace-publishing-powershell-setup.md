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
# <a name="set-up-azure-powershell-toocreate-an-offer-for-hello-azure-marketplace"></a><span data-ttu-id="1cd3b-103">Állítsa be az Azure PowerShell toocreate hello Azure piactér vonatkozó ajánlatot</span><span class="sxs-lookup"><span data-stu-id="1cd3b-103">Set up Azure PowerShell toocreate an offer for hello Azure Marketplace</span></span>
<span data-ttu-id="1cd3b-104">Részletes információt az Azure PowerShell mentése tooset lásd: [hogyan tooinstall és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1cd3b-104">For detailed information on how tooset up PowerShell in Azure, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="1cd3b-105">Egy egyszerű módszer toouse hello tanúsítvány metódus, amely tölti le, és importálja a hitelesítéshez szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="1cd3b-105">A simple approach is toouse hello certificate method, which downloads and imports a certificate needed for authentication.</span></span> <span data-ttu-id="1cd3b-106">tooobtain hello szükséges tanúsítvány, használja a hello **Get-AzurePublishSettingsFile** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1cd3b-106">tooobtain hello needed certificate, use hello **Get-AzurePublishSettingsFile** cmdlet.</span></span> <span data-ttu-id="1cd3b-107">Amikor a rendszer kéri, mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="1cd3b-107">Save hello file when you're prompted.</span></span> <span data-ttu-id="1cd3b-108">egy PowerShell-munkamenetbe, használjon hello tooimport hello tanúsítvány **Import-AzurePublishSettingsFile** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1cd3b-108">tooimport hello certificate into a PowerShell session, use hello **Import-AzurePublishSettingsFile** cmdlet.</span></span>

<span data-ttu-id="1cd3b-109">tooconfigure és a tároló hello közös Microsoft Azure-előfizetések beállításait hello PowerShell-munkamenethez, használja a hello **Set-AzureSubscription** és **válasszon-AzureSubscription** parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="1cd3b-109">tooconfigure and store hello common Microsoft Azure subscription settings for hello PowerShell session, use hello **Set-AzureSubscription** and **Select-AzureSubscription** cmdlets:</span></span>

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

<span data-ttu-id="1cd3b-110">hello első parancs hozzárendeli egy alapértelmezett tárfiókot hello előfizetés (néhány virtuális gép üzembe helyezési műveletekhez szükséges).</span><span class="sxs-lookup"><span data-stu-id="1cd3b-110">hello first command associates a default storage account with hello subscription (needed for some VM provisioning operations).</span></span>  <span data-ttu-id="1cd3b-111">hello második teszi hello előfizetés hello jelenlegivel (ismeri fel más parancsmagok).</span><span class="sxs-lookup"><span data-stu-id="1cd3b-111">hello second makes hello subscription hello current one (recognized by other cmdlets).</span></span>

## <a name="see-also"></a><span data-ttu-id="1cd3b-112">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1cd3b-112">See also</span></span>
* [<span data-ttu-id="1cd3b-113">Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="1cd3b-113">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* [<span data-ttu-id="1cd3b-114">Hello piactér a virtuális gép lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cd3b-114">Creating a virtual machine image for hello Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)

