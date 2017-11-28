---
title: "aaaConfigure SSL-kiszervezés - Azure Application Gateway - Azure portálon |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate Alkalmazásátjáró SSL-kiszervezés hello portál használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: e87ac0bbe10ac45e307c18802741c7bc31764a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-portal"></a><span data-ttu-id="996aa-103">Konfigurálja az SSL kiszervezési Alkalmazásátjáró hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="996aa-103">Configure an application gateway for SSL offload by using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="996aa-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="996aa-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="996aa-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="996aa-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="996aa-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="996aa-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="996aa-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="996aa-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="996aa-108">Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet.</span><span class="sxs-lookup"><span data-stu-id="996aa-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="996aa-109">SSL kiszervezési is egyszerűbbé teszi a hello előtér-kiszolgáló beállítása és felügyelete hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="996aa-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="scenario"></a><span data-ttu-id="996aa-110">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="996aa-110">Scenario</span></span>

<span data-ttu-id="996aa-111">a következő forgatókönyv hello végighalad konfigurálása SSL kiszervezésére meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="996aa-111">hello following scenario goes through configuring SSL offload on an existing application gateway.</span></span> <span data-ttu-id="996aa-112">hello a forgatókönyv azt feltételezi, hogy már követte hello lépéseket túl[Alkalmazásátjáró létrehozása](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="996aa-112">hello scenario assumes that you have already followed hello steps too[Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="996aa-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="996aa-113">Before you begin</span></span>

<span data-ttu-id="996aa-114">tooconfigure SSL kiszervezési az Alkalmazásátjáró, egy tanúsítványra szükség.</span><span class="sxs-lookup"><span data-stu-id="996aa-114">tooconfigure SSL offload with an application gateway, a certificate is required.</span></span> <span data-ttu-id="996aa-115">Ez a tanúsítvány be töltve a hello Alkalmazásátjáró és tooencrypt használja, és SSL-en keresztüli küldött visszafejtése hello.</span><span class="sxs-lookup"><span data-stu-id="996aa-115">This certificate is loaded on hello application gateway and used tooencrypt and decrypt hello traffic sent via SSL.</span></span> <span data-ttu-id="996aa-116">hello tanúsítványt kell toobe személyes információcsere (pfx) formátumú.</span><span class="sxs-lookup"><span data-stu-id="996aa-116">hello certificate needs toobe in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="996aa-117">Ez lehetővé teszi a hello titkos kulcs toobe exportált hello alkalmazás átjáró tooperform hello titkosítási és visszafejtési forgalom által előírt.</span><span class="sxs-lookup"><span data-stu-id="996aa-117">This file format allows for hello private key toobe exported which is required by hello application gateway tooperform hello encryption and decryption of traffic.</span></span>

## <a name="add-an-https-listener"></a><span data-ttu-id="996aa-118">Adja hozzá a HTTPS-figyelő</span><span class="sxs-lookup"><span data-stu-id="996aa-118">Add an HTTPS listener</span></span>

<span data-ttu-id="996aa-119">hello HTTPS-figyelő konfigurációja alapján keres, és segít útvonal hello forgalom toohello háttérkészletek menüpontot.</span><span class="sxs-lookup"><span data-stu-id="996aa-119">hello HTTPS listener looks for traffic based on its configuration and helps route hello traffic toohello backend pools.</span></span>

### <a name="step-1"></a><span data-ttu-id="996aa-120">1. lépés</span><span class="sxs-lookup"><span data-stu-id="996aa-120">Step 1</span></span>

<span data-ttu-id="996aa-121">Nyissa meg a toohello Azure-portálon, és válassza ki a meglévő Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="996aa-121">Navigate toohello Azure portal and select an existing application gateway</span></span>

### <a name="step-2"></a><span data-ttu-id="996aa-122">2. lépés</span><span class="sxs-lookup"><span data-stu-id="996aa-122">Step 2</span></span>

<span data-ttu-id="996aa-123">Kattintson a figyelők, és kattintson a hello Hozzáadás gomb tooadd egy figyelő.</span><span class="sxs-lookup"><span data-stu-id="996aa-123">Click Listeners and click hello Add button tooadd a listener.</span></span>

![átjáró – áttekintés panelen][1]

### <a name="step-3"></a><span data-ttu-id="996aa-125">3. lépés</span><span class="sxs-lookup"><span data-stu-id="996aa-125">Step 3</span></span>

<span data-ttu-id="996aa-126">Töltse ki a szükséges adatokat hello figyelő hello és feltöltés hello .pfx tanúsítvány, amikor befejeződött az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="996aa-126">Fill out hello required information for hello listener and upload hello .pfx certificate, when complete click OK.</span></span>

<span data-ttu-id="996aa-127">**Név** -értéke hello figyelő rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="996aa-127">**Name** - This value is a friendly name of hello listener.</span></span>

<span data-ttu-id="996aa-128">**Előtérbeli IP-konfiguráció** -hello előtérbeli IP-konfigurációja hello figyelő használt értéke.</span><span class="sxs-lookup"><span data-stu-id="996aa-128">**Frontend IP configuration** - This value is hello frontend IP configuration that is used for hello listener.</span></span>

<span data-ttu-id="996aa-129">**Elülső rétegbeli portot (név/Port)** -kezelőfelületre hello hello Alkalmazásátjáró és hello tényleges használt port használt hello port rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="996aa-129">**Frontend port (Name/Port)** - A friendly name for hello port used on hello front end of hello application gateway and hello actual port used.</span></span>

<span data-ttu-id="996aa-130">**Protokoll** -egy kapcsoló toodetermine, ha a https vagy HTTP protokollt használja a hello előtér.</span><span class="sxs-lookup"><span data-stu-id="996aa-130">**Protocol** - A switch toodetermine if https or http is used for hello front end.</span></span>

<span data-ttu-id="996aa-131">**Tanúsítvány (és jelszó)** – Ha SSL kiszervezési szolgál, egy .pfx-tanúsítványra szükség a ezt a beállítást, és felhasználóbarát név és jelszó szükség.</span><span class="sxs-lookup"><span data-stu-id="996aa-131">**Certificate (Name/Password)** - If SSL offload is used, a .pfx certificate is required for this setting and a friendly name and password are required.</span></span>

![figyelő panel hozzáadása][2]

## <a name="create-a-rule-and-associate-it-toohello-listener"></a><span data-ttu-id="996aa-133">Hozzon létre egy szabályt, és társítsa azt toohello figyelő</span><span class="sxs-lookup"><span data-stu-id="996aa-133">Create a rule and associate it toohello listener</span></span>

<span data-ttu-id="996aa-134">hello figyelő létrehozása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="996aa-134">hello listener has now been created.</span></span> <span data-ttu-id="996aa-135">Idő toocreate egy hello figyelő szabály toohandle hello forgalmát is.</span><span class="sxs-lookup"><span data-stu-id="996aa-135">It is time toocreate a rule toohandle hello traffic from hello listener.</span></span> <span data-ttu-id="996aa-136">Szabályok határozzák meg, hogyan akkor irányított toohello háttérkészletek több konfigurációs beállításokat, beleértve, hogy a munkamenet cookie-alapú kapcsolat szolgál, protokoll, port és állapotteljesítmény alapján.</span><span class="sxs-lookup"><span data-stu-id="996aa-136">Rules define how traffic is routed toohello backend pools based on multiple configuration settings, including whether cookie-based session affinity is used, protocol, port, and health probes.</span></span>

### <a name="step-1"></a><span data-ttu-id="996aa-137">1. lépés</span><span class="sxs-lookup"><span data-stu-id="996aa-137">Step 1</span></span>

<span data-ttu-id="996aa-138">Kattintson a hello **szabályok** hello Alkalmazásátjáró, majd kattintson a Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="996aa-138">Click hello **Rules** of hello application gateway, and then click Add.</span></span>

![átjáró szabályok panelen][3]

### <a name="step-2"></a><span data-ttu-id="996aa-140">2. lépés</span><span class="sxs-lookup"><span data-stu-id="996aa-140">Step 2</span></span>

<span data-ttu-id="996aa-141">A hello **hozzáadása alapszintű szabály** panelen írja be a hello szabály hello rövid nevét, és válasszon hello figyelő hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="996aa-141">On hello **Add basic rule** blade, type in hello friendly name for hello rule and choose hello listener created in hello previous step.</span></span> <span data-ttu-id="996aa-142">Válassza ki a megfelelő háttérkészlet hello és a beállítás http és kattintson **OK**</span><span class="sxs-lookup"><span data-stu-id="996aa-142">Choose hello appropriate backend pool and http setting and click **OK**</span></span>

![HTTPS-beállítások ablak][4]

<span data-ttu-id="996aa-144">hello-beállítások mostantól mentése toohello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="996aa-144">hello settings are now saved toohello application gateway.</span></span> <span data-ttu-id="996aa-145">hello menteni ezeket a beállításokat a folyamat eltarthat egy ideig még nem érhető el tooview hello portálon keresztül vagy a Powershellen keresztül.</span><span class="sxs-lookup"><span data-stu-id="996aa-145">hello save process for these settings may take a while before they are available tooview through hello portal or through PowerShell.</span></span> <span data-ttu-id="996aa-146">Egyszer mentett hello Alkalmazásátjáró hello titkosítási és visszafejtési forgalmat kezeli.</span><span class="sxs-lookup"><span data-stu-id="996aa-146">Once saved hello application gateway handles hello encryption and decryption of traffic.</span></span> <span data-ttu-id="996aa-147">Hello Alkalmazásátjáró és hello háttér-webkiszolgálók közötti összes forgalom kezelése HTTP protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="996aa-147">All traffic between hello application gateway and hello backend web servers will be handled over http.</span></span> <span data-ttu-id="996aa-148">Bármely kommunikációs hátsó toohello ügyfélszoftverhez, ha a HTTPS-kapcsolaton keresztül kezdeményezett visszaadott toohello ügyfél titkosítva.</span><span class="sxs-lookup"><span data-stu-id="996aa-148">Any communication back toohello client if initiated over https will be returned toohello client encrypted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="996aa-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="996aa-149">Next steps</span></span>

<span data-ttu-id="996aa-150">toolearn hogyan tooconfigure egyéni állapotfigyelő mintavételi rendelkező Azure Application Gateway, lásd: [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="996aa-150">toolearn how tooconfigure a custom health probe with Azure Application Gateway, see [Create a custom health probe](application-gateway-create-gateway-portal.md).</span></span>

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
