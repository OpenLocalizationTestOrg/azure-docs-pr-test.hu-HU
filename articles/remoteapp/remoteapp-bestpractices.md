---
title: "aaaAzure RemoteApp gyakorlati tanácsok |} Microsoft Docs"
description: "Ajánlott eljárások az Azure RemoteApp konfigurálásához és használatához."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b851865b-bec4-4f29-82c0-7b9770c1a520
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f4d09ef30816eaebb74b69f26f3242c69ea27591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a><span data-ttu-id="021a9-103">Ajánlott eljárások az Azure RemoteApp konfigurálásához és használatához</span><span class="sxs-lookup"><span data-stu-id="021a9-103">Best practices for configuring and using Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="021a9-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="021a9-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="021a9-105">Olvasási hello [közlemény](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="021a9-105">Read hello [announcement](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) for details.</span></span>
> 
> 

<span data-ttu-id="021a9-106">hello a következő információ segítségével konfigurálhatja és használhatja megtartják az Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="021a9-106">hello following information can help you configure and use Azure RemoteApp productively.</span></span>

## <a name="connectivity"></a><span data-ttu-id="021a9-107">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="021a9-107">Connectivity</span></span>
* <span data-ttu-id="021a9-108">Mindig legyen hello ügyfél letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="021a9-108">Always use hello latest client version.</span></span> <span data-ttu-id="021a9-109">Használatával a régebbi ügyfelek kapcsolódási problémák és egyéb csökkent tapasztalatokat eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="021a9-109">Using older clients might result in connectivity issues and other degraded experiences.</span></span> <span data-ttu-id="021a9-110">Az eszköz az alkalmazás automatikus frissítések engedélyezése biztosítja, hogy hello legújabb ügyfél telepítését mindig a.</span><span class="sxs-lookup"><span data-stu-id="021a9-110">Enabling automatic application updates for your device will ensure that hello latest client is always installed.</span></span>
* <span data-ttu-id="021a9-111">Mindig használjon hello stabil és a megbízható internet kapcsolat elérhető tooyou.</span><span class="sxs-lookup"><span data-stu-id="021a9-111">Always use hello most stable and reliable internet connection available tooyou.</span></span>  
* <span data-ttu-id="021a9-112">Csak támogatott proxyn kapcsolat optimális teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="021a9-112">Use only supported proxy connections for optimal connectivity performance.</span></span>  <span data-ttu-id="021a9-113">hello SOCKS proxy nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="021a9-113">hello SOCKS proxy is not supported.</span></span>

## <a name="applications"></a><span data-ttu-id="021a9-114">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="021a9-114">Applications</span></span>
* <span data-ttu-id="021a9-115">Mentse és zárja be a RemoteApp-alkalmazásokat, ha hello alkalmazással végzett.</span><span class="sxs-lookup"><span data-stu-id="021a9-115">Save and close RemoteApp applications when you are done with hello application.</span></span> <span data-ttu-id="021a9-116">Nem a hello alkalmazás bezárása adatvesztést eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="021a9-116">Not closing hello application might result in data loss.</span></span>
* <span data-ttu-id="021a9-117">Ellenőrizze az egyéni alkalmazások az Azure RemoteApp használatához.</span><span class="sxs-lookup"><span data-stu-id="021a9-117">Validate custom applications before using them in Azure RemoteApp.</span></span> <span data-ttu-id="021a9-118">Ez magában foglalja, több munkamenet platformon működik, és szükségtelen erőforrást nem például memória és CPU, előfordulhat, hogy egy másik felhasználója hello ki is szoríthatják ugyanahhoz a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="021a9-118">This includes ensuring they work on a multi-session platform and don’t consume unnecessary resources such as memory and CPU that might starve another user in hello same collection.</span></span> <span data-ttu-id="021a9-119">Információkért töltse le, és tekintse át a hello [alkalmazás kompatibilitási gyakorlati tanácsok a távoli asztali szolgáltatások](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span><span class="sxs-lookup"><span data-stu-id="021a9-119">For information, download and review hello [Application Compatibility Best Practices for Remote Desktop Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).</span></span>

## <a name="configuration-and-management"></a><span data-ttu-id="021a9-120">Konfigurálása és kezelése</span><span class="sxs-lookup"><span data-stu-id="021a9-120">Configuration and management</span></span>
* <span data-ttu-id="021a9-121">Tartsa meg a sablon rendszerképek mentése toodate, szoftverfrissítések és más kritikus javításokat telepítése, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="021a9-121">Keep your template images up toodate, installing software updates and other critical fixes as needed.</span></span> <span data-ttu-id="021a9-122">Ez biztosítja, hogy mivel az Azure RemoteApp automatikusan-méretezik toomeet a kapacitás, minden példány telepítve.</span><span class="sxs-lookup"><span data-stu-id="021a9-122">This ensures that as Azure RemoteApp auto-scales toomeet your capacity, each instance is patched.</span></span>  
* <span data-ttu-id="021a9-123">Győződjön meg arról, hogy az Active Directory összevonási szolgáltatások (AD FS) központi telepítés biztonságos és megbízható.</span><span class="sxs-lookup"><span data-stu-id="021a9-123">Make sure your Active Directory Federation Services (AD FS) deployment is secure and reliable.</span></span> <span data-ttu-id="021a9-124">Ellenkező esetben ügyfél hitelesítése sikertelen lehet, az Azure RemoteApp letiltásáról.</span><span class="sxs-lookup"><span data-stu-id="021a9-124">Otherwise client authentications might fail, preventing users from accessing Azure RemoteApp.</span></span>
* <span data-ttu-id="021a9-125">Sablon rendszerképek konfigurálhatja a telepített alkalmazások, szerepkörök vagy szolgáltatások úgy, hogy-e az állapot nélküli.</span><span class="sxs-lookup"><span data-stu-id="021a9-125">Configure template images with installed applications, roles, or features such that they are stateless.</span></span> <span data-ttu-id="021a9-126">Ezek nem igazolható a virtuális gépek hello állandó állapotának RemoteApp szolgáltatás minden példányát.</span><span class="sxs-lookup"><span data-stu-id="021a9-126">They should not rely on any instances of hello virtual machines in a RemoteApp service being in a persistent state.</span></span>
  * <span data-ttu-id="021a9-127">Összes felhasználói adatot tárolja felhasználói profilok vagy egyéb tárolási helyek külső toohello szolgáltatás, például a helyi fájl megosztásokat vagy a onedrive vállalati verzió.</span><span class="sxs-lookup"><span data-stu-id="021a9-127">Store all user data in user profiles or other storage locations external toohello service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="021a9-128">Megosztott adatok tárolása a tárolási helyek külső toohello szolgáltatás, például a helyi fájl megosztásokat vagy a onedrive-on.</span><span class="sxs-lookup"><span data-stu-id="021a9-128">Store shared data in storage locations external toohello service, such as on-premises file shares or OneDrive.</span></span>
  * <span data-ttu-id="021a9-129">Bármely rendszerre kiterjedő beállítások hello sablonlemezkép helyett egy szolgáltatás egyes virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="021a9-129">Configure any system-wide settings in hello template image rather than on individual virtual machines in a service.</span></span>
  * <span data-ttu-id="021a9-130">Tiltsa le a közzétett alkalmazások automatikusan a szoftverfrissítéseket - helyette alkalmazza őket manuálisan toohello sablon rendszerképet, és tesztelését hello sablonból központi telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="021a9-130">Disable automatic software updates for published applications - instead apply them manually toohello template image and test them before you deploy  from hello template.</span></span>

