---
title: "Felderítési beállításjegyzék beállításainak proxyszolgáltatást aaaCloud |} Microsoft Docs"
description: "hello Ez a témakör célja tooprovide hello kapcsolatos lépéseket kell tooperform tooset szükséges hello port hello hello a Cloud App Discovery-ügynököt futtató számítógépeken."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bb1fe20016459160b4f67cb0125b1781a0260c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a><span data-ttu-id="27503-103">Cloud App Discovery beállításjegyzék-beállítások Proxy szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="27503-103">Cloud App Discovery Registry Settings for Proxy Services</span></span>
<span data-ttu-id="27503-104">Alapértelmezés szerint a konfigurált toouse csak hello portok 80-as vagy 443-as található hello a Cloud App Discovery-ügynök.</span><span class="sxs-lookup"><span data-stu-id="27503-104">By default, hello Cloud App Discovery agent is configured toouse only hello ports 80 or 443.</span></span> <span data-ttu-id="27503-105">Ha azt tervezi, a Cloud App Discovery telepítése által használt egyéni portot (80-as, sem 443-as) proxykiszolgálóval környezetben, szükséges tooconfigure az ügynökök toouse ezt a portot.</span><span class="sxs-lookup"><span data-stu-id="27503-105">If you are planning on installing Cloud App Discovery in an environment with a proxy server that is using a custom port (neither 80 nor 443), you need tooconfigure your agents toouse this port.</span></span> <span data-ttu-id="27503-106">hello konfigurálása egy beállításkulcs megadásával alapján történik.</span><span class="sxs-lookup"><span data-stu-id="27503-106">hello configuration is based on a registry key.</span></span>

<span data-ttu-id="27503-107">hello Ez a témakör célja tooprovide hello kapcsolatos lépéseket kell tooperform tooset szükséges hello port hello hello a Cloud App Discovery-ügynököt futtató számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="27503-107">hello objective of this topic is tooprovide you with hello steps you need tooperform tooset hello required port on hello computers running hello Cloud App Discovery agent.</span></span>

<span data-ttu-id="27503-108">**toomodify hello port hello a Cloud App Discovery-ügynököt futtató hello számítógép által használt hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="27503-108">**toomodify hello port used by hello computer running hello Cloud App Discovery agent, perform hello following steps:**</span></span>

1. <span data-ttu-id="27503-109">Hello beállításjegyzék-szerkesztő elindításához.</span><span class="sxs-lookup"><span data-stu-id="27503-109">Start hello registry editor.</span></span> <br> ![Futtassa a következőt:](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. <span data-ttu-id="27503-111">Keresse meg tooor létrehozása a következő beállításkulcs hello:</span><span class="sxs-lookup"><span data-stu-id="27503-111">Navigate tooor create hello following registry key:</span></span> <br> <span data-ttu-id="27503-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span><span class="sxs-lookup"><span data-stu-id="27503-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span></span> 
3. <span data-ttu-id="27503-113">Hozzon létre egy új **karakterláncsoros** nevű értéket **portok**.</span><span class="sxs-lookup"><span data-stu-id="27503-113">Create a new **multi-string** value called **Ports**.</span></span> <span data-ttu-id="27503-114">![Új](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span><span class="sxs-lookup"><span data-stu-id="27503-114">![New](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span></span>
4. <span data-ttu-id="27503-115">tooopen hello **Karakterláncsor szerkesztése** párbeszédpanel, kattintson duplán a hello portok érték.</span><span class="sxs-lookup"><span data-stu-id="27503-115">tooopen hello **Edit Multi-String** dialog, double-click hello Ports value.</span></span>
5. <span data-ttu-id="27503-116">Hello érték adatok szövegmezőben írja be a következő értékek hello, és adja hozzá a szervezet által használt összes egyéni portokat:</span><span class="sxs-lookup"><span data-stu-id="27503-116">In hello Value data textbox, type hello following values and add all custom ports that are used by your organization:</span></span> <br><br><span data-ttu-id="27503-117">
   **80**</span><span class="sxs-lookup"><span data-stu-id="27503-117">
   **80**</span></span> <br><span data-ttu-id="27503-118">
   **8080**</span><span class="sxs-lookup"><span data-stu-id="27503-118">
   **8080**</span></span> <br><span data-ttu-id="27503-119">
   **8118**</span><span class="sxs-lookup"><span data-stu-id="27503-119">
   **8118**</span></span> <br><span data-ttu-id="27503-120">
   **8888**</span><span class="sxs-lookup"><span data-stu-id="27503-120">
   **8888**</span></span> <br><span data-ttu-id="27503-121">
   **81**</span><span class="sxs-lookup"><span data-stu-id="27503-121">
   **81**</span></span> <br><span data-ttu-id="27503-122">
   **12080**</span><span class="sxs-lookup"><span data-stu-id="27503-122">
   **12080**</span></span> <br><span data-ttu-id="27503-123">
   **6999**</span><span class="sxs-lookup"><span data-stu-id="27503-123">
**6999**</span></span> <br><span data-ttu-id="27503-124">
**30606**</span><span class="sxs-lookup"><span data-stu-id="27503-124">
**30606**</span></span> <br><span data-ttu-id="27503-125">
**31595**</span><span class="sxs-lookup"><span data-stu-id="27503-125">
**31595**</span></span> <br><span data-ttu-id="27503-126">
**4080**</span><span class="sxs-lookup"><span data-stu-id="27503-126">
**4080**</span></span> <br><span data-ttu-id="27503-127">
**443**</span><span class="sxs-lookup"><span data-stu-id="27503-127">
**443**</span></span> <br><span data-ttu-id="27503-128">
**1110**</span><span class="sxs-lookup"><span data-stu-id="27503-128">
**1110**</span></span> <br><br><span data-ttu-id="27503-129">
![Karakterláncsor szerkesztése](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span><span class="sxs-lookup"><span data-stu-id="27503-129">
![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span></span>
6. <span data-ttu-id="27503-130">Kattintson a **OK** tooclose hello **Karakterláncsor szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="27503-130">Click **OK** tooclose hello **Edit Multi-String** dialog.</span></span>

<span data-ttu-id="27503-131">**További források**</span><span class="sxs-lookup"><span data-stu-id="27503-131">**Additional Resources**</span></span>

* [<span data-ttu-id="27503-132">Hogyan lehet használt, jóvá nem hagyott felhőalkalmazások felderítése a szervezeten belül</span><span class="sxs-lookup"><span data-stu-id="27503-132">How can I discover unsanctioned cloud apps that are used within my organization</span></span>](active-directory-cloudappdiscovery-whatis.md) 

