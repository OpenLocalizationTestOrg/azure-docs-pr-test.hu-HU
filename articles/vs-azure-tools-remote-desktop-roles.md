---
title: "Távoli asztal Azure szerepkörök aaaUsing |} Microsoft Docs"
description: "A távoli asztal használata Azure szerepkörök"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d35fd421cde8be9e3caa474db95974a54e528bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a><span data-ttu-id="50dd6-103">A távoli asztal használata Azure szerepkörök</span><span class="sxs-lookup"><span data-stu-id="50dd6-103">Using Remote Desktop with Azure Roles</span></span>
<span data-ttu-id="50dd6-104">Hello Azure SDK-t és a távoli asztali szolgáltatások segítségével érheti el az Azure szerepkörök és az Azure által üzemeltetett virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="50dd6-104">By using hello Azure SDK and Remote Desktop Services, you can access Azure roles and virtual machines that are hosted by Azure.</span></span> <span data-ttu-id="50dd6-105">A Visual Studio a távoli asztali szolgáltatások egy Azure-felhőszolgáltatás-projekt t konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="50dd6-105">In Visual Studio, you can configure Remote Desktop Services from an Azure cloud service project.</span></span> <span data-ttu-id="50dd6-106">tooenable a távoli asztali szolgáltatások, létre kell hoznia egy működő projekt, amely tartalmaz egy vagy több szerepkört és tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="50dd6-106">tooenable Remote Desktop Services, you must create a working project that contains one or more roles and then publish it tooAzure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50dd6-107">Hibaelhárítás vagy fejlesztői csak egy Azure szerepkörét hozzá kell férnie.</span><span class="sxs-lookup"><span data-stu-id="50dd6-107">You should access an Azure role for troubleshooting or development only.</span></span> <span data-ttu-id="50dd6-108">minden virtuális gép céljának hello toorun egy adott szerepkör az Azure-alkalmazás nem toorun más olyan ügyfélalkalmazásokhoz van.</span><span class="sxs-lookup"><span data-stu-id="50dd6-108">hello purpose of each virtual machine is toorun a specific role in your Azure application, not toorun other client applications.</span></span> <span data-ttu-id="50dd6-109">Ha azt szeretné, hogy egy virtuális gép bármilyen célra használható Azure toohost toouse, tekintse meg az Azure virtuális gépek használata a Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="50dd6-109">If you want toouse Azure toohost a virtual machine that you can use for any purpose, see Accessing Azure Virtual Machines from Server Explorer.</span></span>
> 
> 

## <a name="tooenable-and-use-remote-desktop-for-an-azure-role"></a><span data-ttu-id="50dd6-110">tooenable és a távoli asztal használata az Azure-szerepkör</span><span class="sxs-lookup"><span data-stu-id="50dd6-110">tooenable and use Remote Desktop for an Azure Role</span></span>
1. <span data-ttu-id="50dd6-111">A Solution Explorerben nyissa meg a felhőszolgáltatás-projekt helyi menüjének hello, és válassza **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="50dd6-111">In Solution Explorer, open hello shortcut menu for your cloud service project, and then choose **Publish**.</span></span>
   
    <span data-ttu-id="50dd6-112">Hello **Azure-alkalmazás közzététele** varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="50dd6-112">hello **Publish Azure Application** wizard appears.</span></span>
   
    ![A parancs egy Felhőszolgáltatás-projekt közzététele](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. <span data-ttu-id="50dd6-114">Hello aljához **Microsoft Azure közzétételi beállítási** hello varázsló, jelölje be hello **távoli asztal engedélyezése** az összes szerepkörök jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="50dd6-114">At hello bottom of **Microsoft Azure Publish Settings** page of hello wizard, select hello **Enable Remote Desktop** for all roles check box.</span></span> 
   
    <span data-ttu-id="50dd6-115">Hello **a távoli asztal konfigurálásának** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="50dd6-115">hello **Remote Desktop Configuration** dialog box appears.</span></span>
3. <span data-ttu-id="50dd6-116">Hello hello alján **a távoli asztal konfigurálásának** párbeszédpanelen válassza ki a hello **további beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="50dd6-116">At hello bottom of hello **Remote Desktop Configuration** dialog box, choose hello **More Options** button.</span></span> 
   
    <span data-ttu-id="50dd6-117">Megjelenik egy legördülő lista, amely lehetővé teszi, hogy hozzon létre vagy válasszon egy tanúsítványt, így amikor a távoli asztali kapcsolatra hitelesítő adatok titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="50dd6-117">This displays a dropdown list box that lets you create or choose a certificate so that you can encrypt credentials information when connecting via remote desktop.</span></span>
4. <span data-ttu-id="50dd6-118">Hello legördülő listában válassza ki a  **&lt;létrehozása >**, vagy válasszon egy meglévő hello listából.</span><span class="sxs-lookup"><span data-stu-id="50dd6-118">In hello dropdown list, choose **&lt;Create>**, or choose an existing one from hello list.</span></span> 
   
    <span data-ttu-id="50dd6-119">Ha úgy dönt, hogy a meglévő tanúsítványt, hagyja ki a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="50dd6-119">If you choose an existing certificate, skip hello following steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="50dd6-120">hello tanúsítványokat, amelyekre szüksége van a távoli asztali kapcsolat eltérnek a többi Azure művelet használó hello tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="50dd6-120">hello certificates that you need for a remote desktop connection are different from hello certificates that you use for other Azure operations.</span></span> <span data-ttu-id="50dd6-121">hello távelérési tanúsítványnak rendelkeznie kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="50dd6-121">hello remote access certificate must have a private key.</span></span>
   > 
   > 
   
    <span data-ttu-id="50dd6-122">Hello **tanúsítvány létrehozása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="50dd6-122">hello **Create Certificate** dialog box appears.</span></span>
   
   1. <span data-ttu-id="50dd6-123">Adjon meg egy rövid nevet a hello új tanúsítványt, és válassza a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="50dd6-123">Provide a friendly name for hello new certificate, and then choose hello **OK** button.</span></span> <span data-ttu-id="50dd6-124">Új tanúsítvány hello hello legördülő lista megjelenik.</span><span class="sxs-lookup"><span data-stu-id="50dd6-124">hello new certificate appears in hello dropdown list box.</span></span>
   2. <span data-ttu-id="50dd6-125">A hello **a távoli asztal konfigurálásának** párbeszédpanelen adja meg egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="50dd6-125">In hello **Remote Desktop Configuration** dialog box, provide a user name and a password.</span></span>
      
       <span data-ttu-id="50dd6-126">Nem használhat egy meglévő fiókkal.</span><span class="sxs-lookup"><span data-stu-id="50dd6-126">You can’t use an existing account.</span></span> <span data-ttu-id="50dd6-127">Ne adjon meg rendszergazdai felhasználónév hello hello új fiókot.</span><span class="sxs-lookup"><span data-stu-id="50dd6-127">Don’t specify Administrator as hello user name for hello new account.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="50dd6-128">Ha hello jelszó nem felel meg a hello összetettségi követelményeknek, piros ikon látható a következő toohello mezőbe.</span><span class="sxs-lookup"><span data-stu-id="50dd6-128">If hello password doesn’t meet hello complexity requirements, a red icon appears next toohello password text box.</span></span> <span data-ttu-id="50dd6-129">hello jelszónak kell tartalmaznia, nagybetűk, kisbetűk, és a számok vagy szimbólumok.</span><span class="sxs-lookup"><span data-stu-id="50dd6-129">hello password must include capital letters, lowercase letters, and numbers or symbols.</span></span>
      > 
      > 
   3. <span data-ttu-id="50dd6-130">Válassza ki a dátum mely hello fiók lejár és után, amely távoli asztali kapcsolatok le lesz tiltva.</span><span class="sxs-lookup"><span data-stu-id="50dd6-130">Choose a date on which hello account will expire and after which remote desktop connections will be blocked.</span></span>
   4. <span data-ttu-id="50dd6-131">Miután megismerte a megadott összes hello szükséges adatokat, válassza ki azt a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="50dd6-131">After you've provided all hello required information, choose hello **OK** button.</span></span>
      
       <span data-ttu-id="50dd6-132">Számos beállítás, amelyek lehetővé teszik a távelérési szolgáltatások toohello .cscfg és az .csdef fájlok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="50dd6-132">Several settings that enable Remote Access Services are added toohello .cscfg and .csdef files.</span></span>
5. <span data-ttu-id="50dd6-133">A hello **Microsoft Azure közzétételi beállítási** varázsló, válassza ki a hello **OK** gombra kattint, ha már kész toopublish a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="50dd6-133">In hello **Microsoft Azure Publish Settings** wizard, choose hello **OK** button when you’re ready toopublish your cloud service.</span></span>
   
    <span data-ttu-id="50dd6-134">Ha még nem áll készen toopublish, válassza ki a hello **Mégse** gombra.</span><span class="sxs-lookup"><span data-stu-id="50dd6-134">If you're not ready toopublish, choose hello **Cancel** button.</span></span> <span data-ttu-id="50dd6-135">hello konfigurációs beállítások lesznek mentve, és később közzététele a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="50dd6-135">hello configuration settings are saved, and you can publish your cloud service later.</span></span>

## <a name="connect-tooan-azure-role-by-using-remote-desktop"></a><span data-ttu-id="50dd6-136">A távoli asztal használatával kapcsolódnak a tooan Azure szerepkör</span><span class="sxs-lookup"><span data-stu-id="50dd6-136">Connect tooan Azure Role by using Remote Desktop</span></span>
<span data-ttu-id="50dd6-137">Miutá közzéteszi az Azure felhőalapú szolgáltatás, használható Server Explorer toolog hello virtuális gépekké, amely az Azure futtatja.</span><span class="sxs-lookup"><span data-stu-id="50dd6-137">After you publish your cloud service on Azure, you can use Server Explorer toolog into hello virtual machines that Azure hosts.</span></span> 

1. <span data-ttu-id="50dd6-138">A Server Explorer eszközben bontsa ki a hello **Azure** csomópontot, majd bontsa ki a hello csomópont egy felhőalapú szolgáltatás, és egyet a szerepkörök toodisplay példányok listáját.</span><span class="sxs-lookup"><span data-stu-id="50dd6-138">In Server Explorer, expand hello **Azure** node, and then expand hello node for a cloud service and one of its roles toodisplay a list of instances.</span></span>
2. <span data-ttu-id="50dd6-139">Nyissa meg a helyi menü hello egy példány csomópont, és válassza **csatlakozzon a távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="50dd6-139">Open hello shortcut menu for an instance node, and then choose **Connect Using Remote Desktop**.</span></span>
   
    ![Távoli asztali kapcsolatra](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. <span data-ttu-id="50dd6-141">Adja meg a hello felhasználónevet és jelszót, amelyet korábban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="50dd6-141">Enter hello user name and password that you created previously.</span></span> <span data-ttu-id="50dd6-142">Most jelentkezett be a távoli munkamenethez.</span><span class="sxs-lookup"><span data-stu-id="50dd6-142">You are now logged into your remote session.</span></span>

