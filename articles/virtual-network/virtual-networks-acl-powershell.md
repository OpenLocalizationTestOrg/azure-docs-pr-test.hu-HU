---
title: "aaaManage Azure-végpont hozzáférés szabályozásához listák |} PowerShell |} Klasszikus |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage hozzáférés-vezérlési listákat a PowerShell használatával"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a7ca241ea108a266085bfb689b742d781e58da1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-hello-classic-deployment-model"></a>Kezelése a PowerShell használatával hello klasszikus üzembe helyezési modellel végpont hozzáférés-vezérlési listák
Hozzon létre, és kezelheti a hálózati hozzáférés-vezérlési listák (ACL) végpontok Azure PowerShell használatával vagy a kezelési portál hello. Ebben a témakörben található eljárások a hozzáférés-vezérlési lista leggyakoribb feladatokat, amelyek a PowerShell segítségével hajthatja végre. Hello listája Azure PowerShell parancsmagok: [Azure parancsmagokat](http://go.microsoft.com/fwlink/?LinkId=317721). Hozzáférés-vezérlési listák kapcsolatos további információkért lásd: [Mi az a hálózati hozzáférés-vezérlési lista (ACL)?](virtual-networks-acl.md). Ha azt szeretné toomanage a hozzáférés-vezérlési listák hello felügyeleti portál használatával, lásd: [hogyan tooSet be végpontok tooa virtuális gép](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Hálózati hozzáférés-vezérlési listák kezelése az Azure PowerShell használatával
Használhatja az Azure PowerShell-parancsmagok toocreate, távolítsa el, és beállítása (set) hálózati hozzáférés-vezérlési listák (ACL). Néhány példa a néhány hello módon egy hozzáférés-vezérlési lista PowerShell használatával is konfigurálhatja azt szerepel.

tooretrieve hello ACL PowerShell-parancsmagok teljes listája, hello következő választhat:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Hozzon létre egy hálózati hozzáférés-vezérlési lista szabályokat, amelyek lehetővé teszik a hozzáférést egy távoli alhálózatból
hello az alábbi példa bemutatja, egy módon toocreate egy új hozzáférés-vezérlési lista szabályokat tartalmaz. A rendszer ezután alkalmazza a hozzáférés-vezérlési lista tooa virtuális gép végpontja. hello ACL-szabályok az alábbi példa hello lehetővé teszi a hozzáférést a távoli alhálózati. az Azure PowerShell ISE toocreate egy új hálózati hozzáférés-vezérlési lista az engedélyezési szabályainak távoli alhálózati, nyissa meg. Másolja és illessze be az alábbi parancsfájl hello konfigurálása a saját értékekkel hello parancsfájlt, majd futtassa az hello parancsfájl.

1. Hozzon létre új hálózati hozzáférés-vezérlési lista hello objektum.
   
        $acl1 = New-AzureAclConfig
2. Olyan szabály, amely lehetővé teszi a hozzáférést a távoli alhálózati beállítása. Hello az alábbi példában a szabály beállítása *100* (amely elsőbbséget élveznek szabály 200 és újabb) tooallow hello távoli alhálózati *10.0.0.0/8* toohello virtuális gép végpontjának eléréséhez. Cserélje le a saját konfigurációs követelmények hello értékeket. hello neve "SharePoint ACL-config" hello rövid nevet, amelyet toocall Ez a szabály cserélni.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Ismételje meg a további szabályok hello parancsmag hello értékeket a saját konfigurációs követelményeinek, és cserélje le. Lehet, hogy toochange hello szabály számú tooreflect hello rendelés hello szabályok toobe alkalmazni kívánt. hello szabály kevesebb élvez hello nagyobb számot.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Ezután hozzon létre egy új végpontot (Hozzáadás), vagy állítsa be a hozzáférés-vezérlési lista hello meglévő a végpont (Set). Ebben a példában adunk hozzá egy új virtuális gép végpontjának neve "web" és a frissítés hello virtuális gép végpontjának hello az ACL beállításokról.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. A következő hello parancsmagjait, és hello parancsprogrammal. Ehhez a példához hello kombinált parancsmagok néz ki:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Távolítsa el a hálózati hozzáférés-vezérlési lista szabályt, amely lehetővé teszi a hozzáférést egy távoli alhálózatból
az alábbi példa hello egy módon tooremove hálózati hozzáférés-vezérlési lista szabályt mutatja be.  a távoli alhálózati, nyissa meg az Azure PowerShell ISE szabályainak tooremove egy hálózati hozzáférés-vezérlési lista szabály engedélyezése. Másolja és illessze be az alábbi parancsfájl hello konfigurálása a saját értékekkel hello parancsfájlt, majd futtassa az hello parancsfájl.

1. Első lépés egy olyan tooget hello hálózati hozzáférés-vezérlési lista objektum hello virtuális gép végpontja. Hello ACL-szabályának távolítsa el lesz. Ebben az esetben megszüntetjük azt által szabály azonosítója. Hello Szabályazonosító 0 hello hozzáférés-vezérlési lista csak a művelettel eltávolítja. Hello ACL objektum nem távolítja el a virtuális gép végpontjának hello.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Ezt követően kell alkalmaznia hello hálózati hozzáférés-vezérlési lista objektum toohello virtuális gép végpontjának, és frissíti a hello virtuális gépet.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Távolítsa el a hálózati hozzáférés-vezérlési lista egy virtuális gép végpontja
Bizonyos esetekben érdemes lehet tooremove egy virtuális gép végpontjának a hálózati hozzáférés-vezérlési lista objektumot. Ezt követően nyissa meg az Azure PowerShell ISE toodo. Másolja és illessze be az alábbi parancsfájl hello konfigurálása a saját értékekkel hello parancsfájlt, majd futtassa az hello parancsfájl.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Következő lépések
[Mi az a hálózati hozzáférés-vezérlési lista (ACL)?](virtual-networks-acl.md)

