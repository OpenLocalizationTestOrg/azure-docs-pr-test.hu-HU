---
title: "az Azure felhőalapú szolgáltatások szerepkör távoli asztali kapcsolat aaaEnable |} Microsoft Docs"
description: "Hogyan tooconfigure az azure felhőalapú szolgáltatás alkalmazás tooallow távoli asztali kapcsolatok"
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: 55d7043df571c2e88b04aa9ef01dc8ae1d6784f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="4bbcc-103">Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="4bbcc-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4bbcc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4bbcc-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="4bbcc-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="4bbcc-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="4bbcc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4bbcc-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="4bbcc-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bbcc-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="4bbcc-108">A távoli asztal tooaccess hello asztali egy Azure-beli szerepkör lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-108">Remote Desktop enables you tooaccess hello desktop of a role running in Azure.</span></span> <span data-ttu-id="4bbcc-109">A távoli asztali kapcsolat tootroubleshoot használja, és hibáinak az alkalmazás futtatása során.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-109">You can use a Remote Desktop connection tootroubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="4bbcc-110">Engedélyezheti a távoli asztali kapcsolat létrehozása a szerepkörben a fejlesztés során hello távoli asztal modulok belefoglalja a szolgáltatás definíciós, vagy választhatja azt a távoli asztal tooenable hello távoli asztali bővítmény keresztül.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-110">You can enable a Remote Desktop connection in your role during development by including hello Remote Desktop modules in your service definition or you can choose tooenable Remote Desktop through hello Remote Desktop Extension.</span></span> <span data-ttu-id="4bbcc-111">hello előnyben részesített megoldás, toouse hello távoli asztali bővítmény, a távoli asztal hello alkalmazás anélkül, hogy tooredeploy az alkalmazás központi telepítése után is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-111">hello preferred approach is toouse hello Remote Desktop extension as you can enable Remote Desktop even after hello application is deployed without having tooredeploy your application.</span></span>

## <a name="configure-remote-desktop-from-hello-azure-portal"></a><span data-ttu-id="4bbcc-112">Állítsa a távoli asztal hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4bbcc-112">Configure Remote Desktop from hello Azure portal</span></span>
<span data-ttu-id="4bbcc-113">hello Azure-portálon hello a távoli asztali bővítmény módszert használ, így a távoli asztal hello alkalmazás központi telepítése után is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-113">hello Azure portal uses hello Remote Desktop Extension approach so you can enable Remote Desktop even after hello application is deployed.</span></span> <span data-ttu-id="4bbcc-114">Hello **távoli asztal** panel a felhőalapú szolgáltatás lehetővé teszi a távoli asztal tooenable használt tooconnect toohello virtuális gépek módosítása hello helyi rendszergazdai fiókot, hello tanúsítvány-hitelesítésben használt, és állítsa be a hello lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-114">hello **Remote Desktop** blade for your cloud service allows you tooenable Remote Desktop, change hello local Administrator account used tooconnect toohello virtual machines, hello certificate used in authentication and set hello expiration date.</span></span>

1. <span data-ttu-id="4bbcc-115">Kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-115">Click **Cloud Services**, click hello name of hello cloud service, and then click **Remote Desktop**.</span></span>

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. <span data-ttu-id="4bbcc-117">Válassza ki, hogy a távoli asztal tooenable szeretné, hogy egy adott szerepkör vagy az összes szerepkör, majd hello kapcsoló hello értékének módosítása túl**engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-117">Choose whether you want tooenable Remote Desktop for an individual role or for all roles, then change hello value of hello switcher too**Enabled**.</span></span>

3. <span data-ttu-id="4bbcc-118">Töltse ki a felhasználónevet, jelszót, lejárati és tanúsítvány hello kötelező mezőket.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-118">Fill in hello required fields for user name, password, expiry, and certificate.</span></span>

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > <span data-ttu-id="4bbcc-120">Összes szerepkörpéldányt újra kell indítani, amikor először a távoli asztal engedélyezése és kattintson az OK gombra (jelölő).</span><span class="sxs-lookup"><span data-stu-id="4bbcc-120">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="4bbcc-121">Újraindítás, hello tanúsítványjelszavas használt tooencrypt hello tooprevent hello szerepkört telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-121">tooprevent a reboot, hello certificate used tooencrypt hello password must be installed on hello role.</span></span> <span data-ttu-id="4bbcc-122">Újraindítás, tooprevent [hello felhőszolgáltatás tanúsítvány feltöltése](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , és visszatér a toothis párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-122">tooprevent a restart, [upload a certificate for hello cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return toothis dialog.</span></span>
   >
   >
3. <span data-ttu-id="4bbcc-123">A **szerepkörök**, jelölje be hello szerepkör tooupdate kívánt **összes** összes szerepköre tekintetében.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-123">In **Roles**, select hello role you want tooupdate or select **All** for all roles.</span></span>

4. <span data-ttu-id="4bbcc-124">A konfigurációs módosítások befejeztével kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-124">When you finish your configuration updates, click **Save**.</span></span> <span data-ttu-id="4bbcc-125">Eltarthat egy kis ideig, mielőtt a szerepkörpéldányok készen tooreceive kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-125">It will take a few moments before your role instances are ready tooreceive connections.</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="4bbcc-126">A szerepkörpéldányok távoli</span><span class="sxs-lookup"><span data-stu-id="4bbcc-126">Remote into role instances</span></span>
<span data-ttu-id="4bbcc-127">A távoli asztal hello szerepkörök engedélyezése után közvetlenül az Azure portál hello kapcsolatot is kezdeményezhető:</span><span class="sxs-lookup"><span data-stu-id="4bbcc-127">Once Remote Desktop is enabled on hello roles, you can initiate a connection directly from hello Azure Portal:</span></span>

1. <span data-ttu-id="4bbcc-128">Kattintson a **példányok** tooopen hello **példányok** panelen.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-128">Click **Instances** tooopen hello **Instances** blade.</span></span>
2. <span data-ttu-id="4bbcc-129">Válassza ki a szerepkör példánya, amely a távoli asztal konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-129">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="4bbcc-130">Kattintson a **Connect** toodownload egy RDP-fájl hello szerepkörpéldányhoz.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-130">Click **Connect** toodownload an RDP file for hello role instance.</span></span>

    ![Cloud services távoli asztal](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. <span data-ttu-id="4bbcc-132">Kattintson a **nyitott** , majd **Connect** toostart hello távoli asztali kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-132">Click **Open** and then **Connect** toostart hello Remote Desktop connection.</span></span>

>[!NOTE]
> <span data-ttu-id="4bbcc-133">Ha a felhőszolgáltatás ülve mögött egy NSG-t, szükség lehet a toocreate szabályok, amelyek lehetővé teszik a forgalom a portokon **3389-es** és **20000**.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-133">If your cloud service is sitting behind an NSG, you may need toocreate rules that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="4bbcc-134">A távoli asztal-portot használja **3389-es**.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-134">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="4bbcc-135">Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt tooconnect számára.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-135">Cloud Service instances are load balanced, so you can't directly control which instance tooconnect to.</span></span>  <span data-ttu-id="4bbcc-136">Hello *RemoteForwarder* és *RemoteAccess* ügynökök RDP-forgalom kezelésére és hello ügyfél toosend RDP cookie-k engedélyezése, és adjon meg egy egyedi példány tooconnect való.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-136">hello *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow hello client toosend an RDP cookie and specify an individual instance tooconnect to.</span></span>  <span data-ttu-id="4bbcc-137">Hello *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.</span><span class="sxs-lookup"><span data-stu-id="4bbcc-137">hello *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4bbcc-138">További források</span><span class="sxs-lookup"><span data-stu-id="4bbcc-138">Additional resources</span></span>

<span data-ttu-id="4bbcc-139">[Hogyan tooConfigure Felhőszolgáltatások](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="4bbcc-139">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
