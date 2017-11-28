---
title: "Cloud App Discovery beállításjegyzék-beállítások proxyszolgáltatást |} Microsoft Docs"
description: "Ez a témakör célja biztosítja, hogy a lépéseket kell elvégeznie a szükséges port beállítása a Cloud App Discovery-ügynököt futtató számítógépeken."
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
ms.openlocfilehash: ea15dc9a9f20a296e622c8fb1011f7ee99de3e99
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a><span data-ttu-id="157e3-103">Cloud App Discovery beállításjegyzék-beállítások Proxy szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="157e3-103">Cloud App Discovery Registry Settings for Proxy Services</span></span>
<span data-ttu-id="157e3-104">Alapértelmezés szerint a Cloud App Discovery-ügynök csak a portok 80-as vagy 443-as használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="157e3-104">By default, the Cloud App Discovery agent is configured to use only the ports 80 or 443.</span></span> <span data-ttu-id="157e3-105">Ha azt tervezi, a Cloud App Discovery telepítése által használt egyéni portot (80-as, sem 443-as) proxykiszolgálóval környezetben, akkor kell konfigurálni az ügynököket, a port használatára.</span><span class="sxs-lookup"><span data-stu-id="157e3-105">If you are planning on installing Cloud App Discovery in an environment with a proxy server that is using a custom port (neither 80 nor 443), you need to configure your agents to use this port.</span></span> <span data-ttu-id="157e3-106">A konfiguráció egy beállításkulcs megadásával alapul.</span><span class="sxs-lookup"><span data-stu-id="157e3-106">The configuration is based on a registry key.</span></span>

<span data-ttu-id="157e3-107">Ez a témakör célja biztosítja, hogy a lépéseket kell elvégeznie a szükséges port beállítása a Cloud App Discovery-ügynököt futtató számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="157e3-107">The objective of this topic is to provide you with the steps you need to perform to set the required port on the computers running the Cloud App Discovery agent.</span></span>

<span data-ttu-id="157e3-108">**Módosítsa a portot használják a Cloud App Discovery-ügynököt futtató számítógépről, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="157e3-108">**To modify the port used by the computer running the Cloud App Discovery agent, perform the following steps:**</span></span>

1. <span data-ttu-id="157e3-109">A beállításjegyzék-szerkesztő elindításához.</span><span class="sxs-lookup"><span data-stu-id="157e3-109">Start the registry editor.</span></span> <br> ![Futtassa a következőt:](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. <span data-ttu-id="157e3-111">Keresse meg, vagy hozza létre a következő beállításkulcsot:</span><span class="sxs-lookup"><span data-stu-id="157e3-111">Navigate to or create the following registry key:</span></span> <br> <span data-ttu-id="157e3-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span><span class="sxs-lookup"><span data-stu-id="157e3-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span></span> 
3. <span data-ttu-id="157e3-113">Hozzon létre egy új **karakterláncsoros** nevű értéket **portok**.</span><span class="sxs-lookup"><span data-stu-id="157e3-113">Create a new **multi-string** value called **Ports**.</span></span> <span data-ttu-id="157e3-114">![Új](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span><span class="sxs-lookup"><span data-stu-id="157e3-114">![New](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span></span>
4. <span data-ttu-id="157e3-115">Lehetőségre a **Karakterláncsor szerkesztése** párbeszédpanel, kattintson duplán a portok értékre.</span><span class="sxs-lookup"><span data-stu-id="157e3-115">To open the **Edit Multi-String** dialog, double-click the Ports value.</span></span>
5. <span data-ttu-id="157e3-116">Az érték adatok szövegmezőben írja be a következő értékeket, és adja hozzá a szervezet által használt összes egyéni portokat:</span><span class="sxs-lookup"><span data-stu-id="157e3-116">In the Value data textbox, type the following values and add all custom ports that are used by your organization:</span></span> <br><br><span data-ttu-id="157e3-117">
   **80**</span><span class="sxs-lookup"><span data-stu-id="157e3-117">
   **80**</span></span> <br><span data-ttu-id="157e3-118">
   **8080**</span><span class="sxs-lookup"><span data-stu-id="157e3-118">
   **8080**</span></span> <br><span data-ttu-id="157e3-119">
   **8118**</span><span class="sxs-lookup"><span data-stu-id="157e3-119">
   **8118**</span></span> <br><span data-ttu-id="157e3-120">
   **8888**</span><span class="sxs-lookup"><span data-stu-id="157e3-120">
   **8888**</span></span> <br><span data-ttu-id="157e3-121">
   **81**</span><span class="sxs-lookup"><span data-stu-id="157e3-121">
   **81**</span></span> <br><span data-ttu-id="157e3-122">
   **12080**</span><span class="sxs-lookup"><span data-stu-id="157e3-122">
   **12080**</span></span> <br><span data-ttu-id="157e3-123">
   **6999**</span><span class="sxs-lookup"><span data-stu-id="157e3-123">
**6999**</span></span> <br><span data-ttu-id="157e3-124">
**30606**</span><span class="sxs-lookup"><span data-stu-id="157e3-124">
**30606**</span></span> <br><span data-ttu-id="157e3-125">
**31595**</span><span class="sxs-lookup"><span data-stu-id="157e3-125">
**31595**</span></span> <br><span data-ttu-id="157e3-126">
**4080**</span><span class="sxs-lookup"><span data-stu-id="157e3-126">
**4080**</span></span> <br><span data-ttu-id="157e3-127">
**443**</span><span class="sxs-lookup"><span data-stu-id="157e3-127">
**443**</span></span> <br><span data-ttu-id="157e3-128">
**1110**</span><span class="sxs-lookup"><span data-stu-id="157e3-128">
**1110**</span></span> <br><br><span data-ttu-id="157e3-129">
![Karakterláncsor szerkesztése](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span><span class="sxs-lookup"><span data-stu-id="157e3-129">
![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span></span>
6. <span data-ttu-id="157e3-130">Kattintson a **OK** bezárásához a **Karakterláncsor szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="157e3-130">Click **OK** to close the **Edit Multi-String** dialog.</span></span>

<span data-ttu-id="157e3-131">**További források**</span><span class="sxs-lookup"><span data-stu-id="157e3-131">**Additional Resources**</span></span>

* [<span data-ttu-id="157e3-132">Hogyan lehet használt, jóvá nem hagyott felhőalkalmazások felderítése a szervezeten belül</span><span class="sxs-lookup"><span data-stu-id="157e3-132">How can I discover unsanctioned cloud apps that are used within my organization</span></span>](active-directory-cloudappdiscovery-whatis.md) 

