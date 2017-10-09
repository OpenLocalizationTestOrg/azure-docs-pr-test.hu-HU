---
title: "aaaNever tárolóban lévő érzékeny adatokat az Azure RemoteApp egyéni lemezképek |} Microsoft Docs"
description: "Tudnivalók az adatok tárolását az Azure Remoteappban egyéni lemezképek hello:"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 86a0fea218f8826d6d25f50d3c4c36e368e26fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a><span data-ttu-id="9f818-103">Soha ne tároljon bizalmas adatokat az egyéni lemezképek</span><span class="sxs-lookup"><span data-stu-id="9f818-103">Never store sensitive data on custom images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9f818-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="9f818-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="9f818-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9f818-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="9f818-106">Ha az alkalmazását, az Azure Remoteappban, hello első lépése toocreate egy egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="9f818-106">When you host your own application in Azure RemoteApp, hello first step is toocreate a custom image.</span></span> <span data-ttu-id="9f818-107">A kép: egyéni toocreate Virtuálisgép-példányok, amely az alkalmazások kiszolgálására tooyour felhasználók használjuk.</span><span class="sxs-lookup"><span data-stu-id="9f818-107">We use that custom image toocreate VM instances that serve your apps tooyour users.</span></span> <span data-ttu-id="9f818-108">hello egyéni lemezképet csak alkalmazások és a nem bizalmas adatok elvesznek, például az SQL-adatbázisok, személyzet fájlok vagy különleges adatfájlok QuickBooks vállalati fájlok például kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="9f818-108">hello custom image should contain ONLY applications and never sensitive data that can be lost, such as SQL databases, personnel files, or special data files like QuickBooks company files.</span></span> <span data-ttu-id="9f818-109">Az összes bizalmas adatokat kell tárolni a külső tooAzure RemoteApp egy fájlkiszolgálón, egy másik Azure virtuális Gépen, vagy az SQL Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="9f818-109">All sensitive data should reside external tooAzure RemoteApp on a file server, another Azure VM, or in SQL Azure.</span></span> <span data-ttu-id="9f818-110">hello kép csak hello gazdaalkalmazást toohello adatforráshoz kapcsolódik, és megadja a hello adatok kell.</span><span class="sxs-lookup"><span data-stu-id="9f818-110">hello image should just host hello application that connects toohello data source and presents hello data.</span></span> <span data-ttu-id="9f818-111">Felülvizsgálati [Azure RemoteApp-lemezképek követelményei](remoteapp-imagereqs.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="9f818-111">Review [Requirements for Azure RemoteApp images](remoteapp-imagereqs.md) for more information.</span></span> 

<span data-ttu-id="9f818-112">Miért nem tárolja bizalmas adatok toounderstand Azure RemoteApp működése toounderstand van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9f818-112">toounderstand why you should not store sensitive data, you need toounderstand how Azure RemoteApp works.</span></span> <span data-ttu-id="9f818-113">Ha a gyűjtemény létrehozásakor vagy frissítésekor, hello háttérben több klónok vagy hello kép egy példánya jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9f818-113">When a collection is created or updated, behind hello scenes multiple clones or copies of hello image are created.</span></span> <span data-ttu-id="9f818-114">A Virtuálisgép-példányok másolatai pontos hello egyéni lemezkép; Amikor felhasználók alkalmazásokat a Virtuálisgép-példányok csatlakoztatott tooone.</span><span class="sxs-lookup"><span data-stu-id="9f818-114">All these VM instances are exact replicas of hello custom image; when users launch applications they are connected tooone of these VM instances.</span></span> <span data-ttu-id="9f818-115">De hello példányt nem garantált, és nem kell számít, mivel azok nem állandó.</span><span class="sxs-lookup"><span data-stu-id="9f818-115">But hello same instance is not guaranteed and should not matter because they are non-persistent.</span></span> <span data-ttu-id="9f818-116">hello Virtuálisgép-példányok üzemeltetési hello az alkalmazások nem állandó és lehet megsemmisítették, vagy törölték a alapú, például gyűjtemény frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="9f818-116">hello VM instances hosting hello applications are non-persistent and can be destroyed or deleted based, for example, during collection update.</span></span> 

<span data-ttu-id="9f818-117">Miután hello gyűjtemény ki van építve, és elindíthatók csatlakozó toohello virtuális gépeket, a felhasználói adatok állandó és védelme, mert a Mentés belül tartalmazó virtuális merevlemez nevezzük különálló tárhelyet a egy [felhasználói profil lemezre (UPD)](remoteapp-upd.md), vagyis hello felhasználói profil a c:\users\<userprofile >.</span><span class="sxs-lookup"><span data-stu-id="9f818-117">Once hello collection is provisioned and users start connecting toohello VMs, user data is persistent and protected because it is saved on separate storage within a VHD that we call a [user profile disk (UPD)](remoteapp-upd.md), which is hello user profile in c:\users\<userprofile>.</span></span> <span data-ttu-id="9f818-118">Az alkalmazás indításakor hello UPD csatlakozik és hasonlóan a helyi felhasználói profil hello operációs rendszer által kezelni.</span><span class="sxs-lookup"><span data-stu-id="9f818-118">When an application starts, hello UPD is mounted and treated just like a local user profile by hello operating system.</span></span> <span data-ttu-id="9f818-119">További információk [Azure RemoteApp menti a felhasználói adatok és beállítások](remoteapp-upd.md).</span><span class="sxs-lookup"><span data-stu-id="9f818-119">Read more about how [Azure RemoteApp saves user data and settings](remoteapp-upd.md).</span></span>

<span data-ttu-id="9f818-120">Példa adatokat kell hello kép nem található:</span><span class="sxs-lookup"><span data-stu-id="9f818-120">Example data that should not reside in hello image:</span></span>

* <span data-ttu-id="9f818-121">A felhasználók tooaccess megosztott adatok</span><span class="sxs-lookup"><span data-stu-id="9f818-121">Shared data for users tooaccess</span></span>
* <span data-ttu-id="9f818-122">SQL-adatbázis vagy a QuickBooks DB</span><span class="sxs-lookup"><span data-stu-id="9f818-122">SQL DB or QuickBooks DB</span></span>
* <span data-ttu-id="9f818-123">Egyetlen megadott adattal sem D:\\</span><span class="sxs-lookup"><span data-stu-id="9f818-123">Any data in D:\\</span></span>

<span data-ttu-id="9f818-124">Példa adatok hello alapértelmezett profil toobe átmásolja az összes felhasználói UPD is található:</span><span class="sxs-lookup"><span data-stu-id="9f818-124">Example data that can reside in hello default profile toobe copied into every users’ UPD:</span></span>

* <span data-ttu-id="9f818-125">Felhasználói szintű konfigurációs fájlok</span><span class="sxs-lookup"><span data-stu-id="9f818-125">Configuration files per user</span></span>
* <span data-ttu-id="9f818-126">Olyan parancsfájlok, amelyek lenne szükség a felhasználók saját UPD megőrzi</span><span class="sxs-lookup"><span data-stu-id="9f818-126">Scripts that users would need preserved in their UPD</span></span>

<span data-ttu-id="9f818-127">Kulcs mutat:</span><span class="sxs-lookup"><span data-stu-id="9f818-127">Key points:</span></span>

* <span data-ttu-id="9f818-128">Soha nem tárolnak a bizalmas adatokat, amelyek egyéni lemezkép létrehozásakor hello rendszerképre elveszett.</span><span class="sxs-lookup"><span data-stu-id="9f818-128">Never store sensitive data that can be lost on hello image when creating a custom image.</span></span>
* <span data-ttu-id="9f818-129">Bizalmas adatok mindig kell egy különálló fájlkiszolgálón található, Azure virtuális gép, hello felhőhöz, és az Azure RemoteApp alkalmazást üzemeltető mindig külső toohello Virtuálisgép-példány külön.</span><span class="sxs-lookup"><span data-stu-id="9f818-129">Sensitive data should always reside on a separate file server, separate Azure VM, on hello cloud, and always external toohello VM instances hosting your applications within Azure RemoteApp.</span></span> 
* <span data-ttu-id="9f818-130">Felhasználói adatok menti, és továbbra is fennáll, a hello felhasználói profil lemezre (UPD)</span><span class="sxs-lookup"><span data-stu-id="9f818-130">User data is saved and persists in hello user profile disk (UPD)</span></span>

