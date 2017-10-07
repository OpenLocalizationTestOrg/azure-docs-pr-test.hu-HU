---
title: "aaaAzure funkciók Runtime telepítésének |} Microsoft Docs"
description: "Hogyan tooInstall hello Azure Functions Futtatókörnyezettel"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 67c6d10b5c0ac43e880d29cff0ae7b099f82bdb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-functions-runtime-preview"></a><span data-ttu-id="6a49d-103">Hello Azure Functions futásidejű Preview telepítése</span><span class="sxs-lookup"><span data-stu-id="6a49d-103">Install hello Azure Functions Runtime Preview</span></span>

<span data-ttu-id="6a49d-104">Ha szeretné tooinstall hello Azure Functions Futtatókörnyezettel preview, akkor kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6a49d-104">If you would like tooinstall hello Azure Functions Runtime preview, you must follow these steps:</span></span>

1. <span data-ttu-id="6a49d-105">Győződjön meg arról, a gép megfelelt hello minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="6a49d-105">Ensure your machine passes hello minimum requirements</span></span>
1. <span data-ttu-id="6a49d-106">Töltse le a hello [Azure Functions futásidejű Preview telepítő](https://aka.ms/azafr).</span><span class="sxs-lookup"><span data-stu-id="6a49d-106">Download hello [Azure Functions Runtime Preview Installer](https://aka.ms/azafr).</span></span> 
1. <span data-ttu-id="6a49d-107">Hello Azure Functions Futtatókörnyezettel preview telepítése</span><span class="sxs-lookup"><span data-stu-id="6a49d-107">Install hello Azure Functions Runtime preview</span></span>
1. <span data-ttu-id="6a49d-108">Hello Azure Functions Futtatókörnyezettel preview teljes hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6a49d-108">Complete hello configuration of hello Azure Functions Runtime preview</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a49d-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6a49d-109">Prerequisites</span></span>

<span data-ttu-id="6a49d-110">Hello Azure Functions Futtatókörnyezettel előzetes verzió telepítése előtt rendelkeznie kell hello következő:</span><span class="sxs-lookup"><span data-stu-id="6a49d-110">Before you install hello Azure Functions Runtime preview, you must have hello following:</span></span>

1. <span data-ttu-id="6a49d-111">A Microsoft Windows Server 2016 vagy a Microsoft Windows 10 Creators frissítés (Professional és Enterprise Edition) rendszerű gépek.</span><span class="sxs-lookup"><span data-stu-id="6a49d-111">A machine running Microsoft Windows Server 2016 or Microsoft Windows 10 Creators Update (Professional or Enterprise Edition).</span></span>
1. <span data-ttu-id="6a49d-112">A hálózaton belül futó SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="6a49d-112">A SQL Server instance running within your network.</span></span>  <span data-ttu-id="6a49d-113">Minimális edition követelmény az SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="6a49d-113">Minimum edition requirement is SQL Server Express.</span></span>

## <a name="install-hello-azure-functions-runtime-preview"></a><span data-ttu-id="6a49d-114">Hello Azure Functions futásidejű Preview telepítése</span><span class="sxs-lookup"><span data-stu-id="6a49d-114">Install hello Azure Functions Runtime Preview</span></span>

<span data-ttu-id="6a49d-115">hello Azure Functions Futtatókörnyezettel preview telepítő végigvezeti hello Azure Functions Futtatókörnyezettel előzetes felügyeleti és feldolgozói szerepkörök hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="6a49d-115">hello Azure Functions Runtime preview installer guides you through hello installation of hello Azure Functions Runtime preview Management and Worker Roles.</span></span>  <span data-ttu-id="6a49d-116">Lehetséges tooinstall hello felügyeleti és a feldolgozói szerepkör a hello egyazon számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6a49d-116">It is possible tooinstall hello Management and Worker role on hello same machine.</span></span>  <span data-ttu-id="6a49d-117">Azonban további funkciók hozzáadása, telepítenie kell a további gépek toobe képes tooscale további feldolgozói szerepkörök a funkciók több Worker-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6a49d-117">However, as you add more Functions, you must deploy more worker roles on additional machines toobe able tooscale your functions onto multiple workers.</span></span>

## <a name="install-hello-management-and-worker-role-on-hello-same-machine"></a><span data-ttu-id="6a49d-118">Hello felügyeleti és a feldolgozói szerepkör telepítését hello egyazon számítógépen</span><span class="sxs-lookup"><span data-stu-id="6a49d-118">Install hello Management and Worker Role on hello same machine</span></span>

1. <span data-ttu-id="6a49d-119">Hello Azure Functions futásidejű Preview telepítő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="6a49d-119">Run hello Azure Functions Runtime Preview Installer.</span></span>

    ![Az Azure Functions futásidejű Preview telepítő][1]

1. <span data-ttu-id="6a49d-121">**Kattintson a Tovább gombra** előzetes túli hello hello telepítő első szakasza</span><span class="sxs-lookup"><span data-stu-id="6a49d-121">**Click Next** advance past hello first stage of hello installer</span></span>
1. <span data-ttu-id="6a49d-122">Ha mindenképpen olvassa el hello hello feltételeinek **EULA**, **hello jelölőnégyzetet** tooaccept hello feltételeit és **kattintson a Tovább gombra** tooadvance.</span><span class="sxs-lookup"><span data-stu-id="6a49d-122">Once you have read hello terms of hello **EULA**, **check hello box** tooaccept hello terms and **click Next** tooadvance.</span></span>
1. <span data-ttu-id="6a49d-123">Most válassza hello szerepkörök szeretné-e ezen a gépen tooinstall **funkciók szerepkör** és/vagy **funkciók feldolgozói szerepkör** és **kattintson a Tovább gombra**</span><span class="sxs-lookup"><span data-stu-id="6a49d-123">Now select hello roles you want tooinstall on this machine **Functions Management Role** and/or **Functions Worker Role** and **Click Next**</span></span>

    ![Az Azure Functions futásidejű Preview Installer - szerepkör kiválasztása][3]

    > [!NOTE]
    > <span data-ttu-id="6a49d-125">Hello telepítése **funkciók feldolgozói szerepkör** a sok más gépek toodo Igen, kövesse ezeket az utasításokat, és csak válassza **funkciók feldolgozói szerepkör** hello telepítője.</span><span class="sxs-lookup"><span data-stu-id="6a49d-125">You can install hello **Functions Worker Role** on many other machines toodo so, follow these instructions, and only select **Functions Worker Role** in hello installer.</span></span>

1. <span data-ttu-id="6a49d-126">**Kattintson a Tovább gombra** toohave hello **Azure Functions futásidejű telepítő** telepítése a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="6a49d-126">**Click Next** toohave hello **Azure Functions Runtime Installer** install on your machine.</span></span>
1. <span data-ttu-id="6a49d-127">Ha teljes hello telepítő indulnak hello **Azure Functions futásidejű konfigurációs eszköz**.</span><span class="sxs-lookup"><span data-stu-id="6a49d-127">Once complete hello installer will launch hello **Azure Functions Runtime Configuration tool**.</span></span>

    ![Az Azure Functions futásidejű Preview telepítő befejezése][5]

    > [!NOTE]
    > <span data-ttu-id="6a49d-129">Ha telepíti az **Windows 10** és hello **tároló** funkció nem korábban engedélyezve van, hello **Azure Functions Futtatókörnyezettel** Installer tooreboot kéri a gép toocomplete hello telepítése.</span><span class="sxs-lookup"><span data-stu-id="6a49d-129">If you are installing on **Windows 10** and hello **Container** feature has not been previously enabled, hello **Azure Functions Runtime** Installer prompts you tooreboot your machine toocomplete hello install.</span></span>

## <a name="configure-hello-azure-functions-runtime"></a><span data-ttu-id="6a49d-130">Hello Azure Functions Futtatókörnyezettel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6a49d-130">Configure hello Azure Functions Runtime</span></span>

<span data-ttu-id="6a49d-131">toocomplete hello Azure Functions Futtatókörnyezettel telepítési hello konfigurációs el kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="6a49d-131">toocomplete hello Azure Functions Runtime installation you must complete hello configuration.</span></span>

1. <span data-ttu-id="6a49d-132">Hello **Azure Functions futásidejű konfigurációs eszköz** jeleníti meg, mely szerepkörök sincs telepítve a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6a49d-132">hello **Azure Functions Runtime Configuration Tool** shows which roles are installed on your machine.</span></span>

    ![Az Azure Functions futásidejű Preview konfigurációs eszköz][6]

1. <span data-ttu-id="6a49d-134">Kattintson a hello **adatbázis** lapra, adja meg a hello **kapcsolódási adatait. az SQL Server-példány** és **kattintson az alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="6a49d-134">Click hello **Database** tab, enter hello **connection details for your SQL Server Instance** and **click Apply**.</span></span>  <span data-ttu-id="6a49d-135">Ez a sorrend toohello Azure Functions Futtatókörnyezettel toocreate egy adatbázis toosupport hello futásidejű szükséges.</span><span class="sxs-lookup"><span data-stu-id="6a49d-135">This is required in order toohello Azure Functions Runtime toocreate a database toosupport hello Runtime.</span></span>
    
    ![Az Azure Functions futásidejű Preview adatbázis konfigurációja][7]

1. <span data-ttu-id="6a49d-137">Kattintson a hello **hitelesítő adatok** fülre.  Ezen a képernyőn létre kell hoznia két új hitelesítő adatokat a fájlmegosztás az Azure Functions üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a49d-137">Click hello **Credentials** tab.  On this screen you must create two new credentials for use with a FileShare for hosting all your Azure Functions.</span></span>  <span data-ttu-id="6a49d-138">**Adja meg a felhasználónevet és jelszót** hello kombinációk **megosztás fájltulajdonos** és hello **fájl megosztási felhasználói** kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="6a49d-138">**Specify Username and Password** combinations for hello **File Share Owner** and for hello **File Share User** and click **Apply**.</span></span>

    ![Az Azure Functions futásidejű Preview hitelesítő adatok][8]

1. <span data-ttu-id="6a49d-140">Kattintson a hello **fájlmegosztás** fülre.  Ez a képernyő meg kell adnia hello hello részleteit **fájlmegosztás helye**.</span><span class="sxs-lookup"><span data-stu-id="6a49d-140">Click hello **File Share** tab.  In this screen you must specify hello details of hello **File Share location**.</span></span>  <span data-ttu-id="6a49d-141">Ez az Ön hozható létre, vagy egy meglévő fájlmegosztást, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="6a49d-141">This can be created for you or you can use an existing File Share and click **Apply**.</span></span>  <span data-ttu-id="6a49d-142">Ha egy új fájlmegosztási helyet választja, meg kell hello Azure Functions Futtatókörnyezettel egy könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="6a49d-142">If you select a new File Share location, you must specify a directory for use by hello Azure Functions Runtime.</span></span>
    
    ![Az Azure Functions futásidejű Preview fájlmegosztás][9]

1. <span data-ttu-id="6a49d-144">Kattintson a hello **IIS** fülre.  Ezen a lapon látható hello webhelyek hello részleteit az IIS-ben, hogy hello Azure Functions Runtime telepítésének hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6a49d-144">Click hello **IIS** tab.  This tab shows hello details of hello websites in IIS that hello Azure Functions Runtime Installation will create.</span></span>  <span data-ttu-id="6a49d-145">**Kattintson az alkalmaz** toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6a49d-145">**Click Apply** toocomplete.</span></span>

    ![Az Azure Functions futásidejű előzetes IIS][10]

1. <span data-ttu-id="6a49d-147">Kattintson a hello **szolgáltatások** fülre.  Ezen a lapon az Azure Functions Futtatókörnyezettel telepítés hello szolgáltatások hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6a49d-147">Click hello **Services** tab.  This tab shows hello status of hello services in your Azure Functions Runtime installation.</span></span>  <span data-ttu-id="6a49d-148">Ha hello kezdeti konfigurálása után **Azure Functions gazdaszolgáltatás aktiválási** nem fut kattintson **szolgáltatás indítása**</span><span class="sxs-lookup"><span data-stu-id="6a49d-148">If after initial configuration hello **Azure Functions Host Activation Service** is not running click **Start Service**</span></span>

    ![Az Azure Functions futásidejű Preview konfigurációs befejezése][11]

1. <span data-ttu-id="6a49d-150">Végül Tallózás toohello **Azure Functions futásidejű portálon** ,`https://<machinename>/`</span><span class="sxs-lookup"><span data-stu-id="6a49d-150">Finally browse toohello **Azure Functions Runtime Portal** as `https://<machinename>/`</span></span>

    ![Az Azure Functions futásidejű a betekintő portálon][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png