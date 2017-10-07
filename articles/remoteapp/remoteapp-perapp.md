---
title: "aaaPublish alkalmazások tooindividual felhasználók egy Azure RemoteApp-gyűjteményben (előzetes verzió) |} Microsoft Docs"
description: "Ismerje meg, hogyan közzéteheti alkalmazások tooindividual felhasználók, ahelyett, hogy attól függően, hogy csoportok, az Azure Remoteappban."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
ms.openlocfilehash: 87b435552ddbfc2c6d03aeb01d95a44985e71f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-tooindividual-users-in-an-azure-remoteapp-collection-preview"></a>Alkalmazások tooindividual felhasználók közzététele egy Azure RemoteApp-gyűjteményben (előzetes verzió)
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ez a cikk azt ismerteti, hogyan toopublish alkalmazások tooindividual felhasználók egy Azure RemoteApp-gyűjteményben. Ez az új funkció az Azure Remoteappban, jelenleg magán előnézetben és rendelkezésre álló csak tooselect korai támogatókat tesztelési célra.

Eredetileg a Azure remoteappban csak egy módja volt az alkalmazás-közzététel: hello rendszergazda hello lemezképből alkalmazásokat tehetett közzé, és azok lenne látható tooall felhasználók hello gyűjteményben.

Egy általános forgatókönyv számos alkalmazás egyetlen rendszerképet, a rendelés tooreduce felügyeleti költségek egy gyűjtemény üzembe helyezése tooinclude. Gyakran nem minden alkalmazás olyan megfelelő tooall felhasználók – a rendszergazdák inkább toopublish alkalmazások tooindividual felhasználókat, hogy ne lássák a szükségtelen alkalmazásokat az alkalmazásfolyamukban.

Ez mostantól lehetséges az Azure RemoteAppban – jelenleg korlátozott előzetes verzióként. Íme hello új funkció rövid összefoglalása:

1. Egy gyűjtemény két módba állítható be:
   
   * hello eredeti gyűjtemény módja, ha egy gyűjtemény minden felhasználója láthatja az összes közzétett alkalmazást. Ez az alapértelmezett mód hello.
   * hello új alkalmazás módja, ha csak látnak explicit módon hozzárendelt toothem alkalmazások
2. Hello pillanatban hello alkalmazás mód csak akkor engedélyezhető, Azure RemoteApp PowerShell-parancsmagok használatával.
   
   * Ha tooapplication üzemmód beállítása, hello gyűjtemény felhasználó-hozzárendelése nem kezelhető hello Azure-portálon keresztül. Felhasználó-hozzárendelés PowerShell-parancsmagok segítségével kezelt toobe rendelkezik.
3. A felhasználók csak látni fogják hello alkalmazások közzétett közvetlenül toothem. Azonban továbbra is lehet az egy felhasználói toolaunch hello kép lévő többi alkalmazást hello közvetlenül hello operációs rendszerben férnek hozzájuk.
   
   * Ez a szolgáltatás nem biztosít az alkalmazások; biztonságos zárolását Ez csak a láthatóságot korlátozza az alkalmazásfolyamban hello.
   * Ha tooisolate felhasználókat egyes alkalmazásoktól, szüksége lesz toouse különálló gyűjteményeket, amelyek.

## <a name="how-tooget-azure-remoteapp-powershell-cmdlets"></a>Hogyan tooget Azure RemoteApp PowerShell-parancsmagok
tootry hello előzetes verzió új funkcióinak, szüksége lesz a toouse Azure PowerShell-parancsmagokat. Jelenleg nem lehetséges toouse hello Azure felügyeleti portál tooenable hello új alkalmazás-közzétételi mód.

Először ellenőrizze, hogy rendelkezik-e hello [Azure PowerShell modul](/powershell/azure/overview) telepítve.

Majd indítsa el a felügyeleti és a következő parancsmag futtatási hello hello PowerShell-konzolon:

        Add-AzureAccount

A rendszer felkéri az Azure felhasználói nevének jelszavának megadására. Miután bejelentkezett, szemben az Azure-előfizetések fogja tudni toorun Azure RemoteApp-parancsmagokat.

## <a name="how-toocheck-which-mode-a-collection-is-in"></a>Hogyan a egy gyűjtemény üzemmódjának toocheck
Futtassa a következő parancsmag hello:

        Get-AzureRemoteAppCollection <collectionName>

![Hello fizetési mód ellenőrzése](./media/remoteapp-perapp/araacllelvel.png)

hello AclLevel tulajdonság hello a következő értékeket veheti fel:

* Gyűjtemény: hello eredeti közzétételi mód. Az összes felhasználó láthat minden közzétett alkalmazást.
* Alkalmazás: hello új közzétételi mód. Csak a hello alkalmazásokat közzétenni közvetlenül toothem a felhasználók láthatják.

## <a name="how-tooswitch-tooapplication-publishing-mode"></a>Hogyan tooswitch tooapplication közzétételi mód
Futtassa a következő parancsmag hello:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

A rendszer megőrzi az alkalmazások közzétételének állapotát: kezdetben az összes felhasználó látni fog minden eredetileg közzétett alkalmazást hello.

## <a name="how-toolist-users-who-can-see-a-specific-application"></a>Hogyan toolist felhasználók, akik láthatnak egy adott alkalmazást
Futtassa a következő parancsmag hello:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

A parancs megjeleníti az összes felhasználó számára látható hello alkalmazás.

Megjegyzés: A látható hello alkalmazások aliasait (hello fenti szintaxisban "app alias" elnevezésű) futtassa a Get-AzureRemoteAppProgram - CollectionName <collectionName>.

## <a name="how-tooassign-an-application-tooa-user"></a>Hogyan tooassign tooa Alkalmazásfelhasználó
Futtassa a következő parancsmag hello:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

hello felhasználói jelenik meg hello alkalmazás hello Azure RemoteApp-ügyfelet, és képes tooconnect tooit lesz.

## <a name="how-tooremove-an-application-from-a-user"></a>Hogyan tooremove az alkalmazás a felhasználó
Futtassa a következő parancsmag hello:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Visszajelzés küldése
Nagyra értékeljük visszajelzését és javaslatait az előzetes verzióként elérhető szolgáltatással kapcsolatban. Töltse ki az hello [felmérés](http://www.instant.ly/s/FDdrb) arról, mit gondol toolet.

## <a name="havent-had-a-chance-tootry-hello-preview-feature"></a>Nem volt alkalommal tootry hello előzetes verziójú funkciók?
Ha nem használta a hello preview még, akkor használja ezt [felmérés](http://www.instant.ly/s/AY83p) toorequest hozzáférést.

