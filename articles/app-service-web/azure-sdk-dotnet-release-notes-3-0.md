---
title: "aaaAzure SDK for .NET 3.0 kibocsátási megjegyzései |} Microsoft Docs"
description: "Az Azure SDK for .NET 3.0 kibocsátási megjegyzések"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: 8970b4c9b64de40dc29a33d69006a00ae5e38a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a>Az Azure SDK for .NET 3.0 kibocsátási megjegyzései

Ez a témakör kibocsátási megjegyzések a hello Azure SDK 3.0-s verziója a .NET-hez.

##<a name="azure-sdk-for-net-30-release-summary"></a>Az Azure SDK .NET 3.0 kiadás összefoglaló

Kiadás dátuma: 03/07/2017
 
Ebben a kiadásban nem a legfrissebb változtatásokat toohello Azure SDK 3.0 vezettek be. Nincs még nincs szükség, a frissítési folyamat tooleverage az SDK-val meglévő Felhőszolgáltatás-projektek. hello Azure SDK 3.0 anélkül, hogy a frissítési folyamat, az Azure SDK 3.0 tooallow használatát toohello telepíti, az Azure SDK 2.9 ugyanazon könyvtárak. A legtöbb hello összetevők hello főverziója nem változott az 2.9, de ehelyett frissített hello buildszáma.

## <a name="visual-studio-2017-rtw"></a>A Visual Studio 2017 RTW

- A Visual Studio 2017 ezen kiadásával a hello Azure SDK for .NET toohello Azure munkaterhelés részét. Minden hello eszközökkel toodo Azure fejlesztési továbbítja a Visual Studio 2017 része lesz. A Visual Studio 2015 hello SDK továbbra is elérhetők WebPI keresztül. A Microsoft megszüntetése Azure SDK .NET kiadásokban a Visual Studio 2013 most, hogy a Visual Studio 2017 kiadása.

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Módosított hello viselkedés tooonly részleges kapcsolati karakterlánc szerepét a Cloud Services diagnosztika tárolási kapcsolati karakterlánc egy token hello kulccsal tárolja. hello tényleges biztonságitár-kulcs most tárolja a hello felhasználói profil mappában, így a hozzáférése vezérelhető. A Visual Studio hello biztonságitár-kulcs olvasását helyi Hibakeresés és a közzétételi folyamat felhasználói profil mappájába. 
- Válasz toohello módosítása a fent leírt, a Visual Studio Online csapat továbbfejlesztett hello Azure Cloud Services központi telepítési sablon, a felhasználók hello kulcs diagnosztika bővítmény beállításához folyamatos integrációt tooAzure közzétételekor kell megadni és üzembe helyezés.
- Az hajtottunk lehetséges toostore a biztonságos kapcsolati karakterláncot és létrehozása az Azure Diagnostics (ÜVEGVATTA), toohelp konfigurációjával kapcsolatos problémák megoldása environements között.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016-os virtuális gépek

- A Visual Studio mostantól támogatja a központi telepítés Felhőszolgáltatások tooOS termékcsalád 5 (Windows Server 2016) virtuális gépeket. A meglévő felhőszolgáltatások, módosíthatja a beállításokat tootarget új operációsrendszer-család hello. Amikor új felhőalapú szolgáltatások hoz létre, ha úgy dönt, hogy toocreate hello szolgáltatást használó .net 4.6-os vagy újabb, az alapértelmezés szerint hello szolgáltatás toouse operációsrendszer-család 5.  További információkért tekintse át hello [Vendég operációsrendszer-család támogatja a tábla](../cloud-services/cloud-services-guestos-update-matrix.md).

### <a name="known-issues"></a>Ismert problémák

- Az Azure .NET SDK 3.0-s bevezetett problémát, a Visual Studio 2015 párhuzamos konfigurációban a Visual Studio 2017 eltávolításakor.  Ha hello Azure SDK-t a Visual Studio 2015 telepítve van, a Microsoft Azure Storage Emulator hello és a Microsoft Azure Compute Emulator eltávolítja Ha eltávolítja a Visual Studio 2017.  Ez a művelet létrehoz az létrehozásakor, és új felhőalapú szolgáltatások projekteket a Visual Studio 2015-hibakeresés hiba. A rendezés toofix probléma, telepítse újra a hello Azure SDK-t a Webplatform-telepítő hello.  hello probléma fel lesz oldva az egy későbbi kiadásban Visual Studio 2017.  .

 
### <a name="azure-in-role-cache"></a>Azure szerepköralapú gyorsítótár 

- Azure szerepköralapú gyorsítótár terméktámogatás November 30 2016. További információért kattintson [Itt](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).




