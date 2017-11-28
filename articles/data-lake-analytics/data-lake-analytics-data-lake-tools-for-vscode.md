---
title: "Az Azure Data Lake Tools: Használata Azure Data Lake Tools Visual Studio Code |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio Code toocreate tesztelése, és a U-SQL-parancsfájlok futtatása. "
Keywords: "VScode, az Azure Data Lake Tools, a helyi futtatáskor, helyi hibakeresése a helyi debug preview tárolófájl feltöltése toostorage elérési útja"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 77771c5d5dae3bfce4ad2df240ea6c6ef848f288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a><span data-ttu-id="3a7a1-104">Az Azure Data Lake Tools használja a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3a7a1-104">Use Azure Data Lake Tools for Visual Studio Code</span></span>

<span data-ttu-id="3a7a1-105">Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio (kód VS) toocreate, teszteléséhez, és a U-SQL-parancsfájlok futtatása.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-105">Learn how toouse Azure Data Lake Tools for Visual Studio Code (VS Code) toocreate, test, and run U-SQL scripts.</span></span> <span data-ttu-id="3a7a1-106">hello információkat is tartalmazza, az alábbi videó hello:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-106">hello information is also covered in hello following video:</span></span>

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a><span data-ttu-id="3a7a1-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3a7a1-107">Prerequisites</span></span>

<span data-ttu-id="3a7a1-108">A Data Lake Tools VS kód által támogatott hello platformokon telepíthető.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-108">Data Lake Tools can be installed on hello platforms supported by VS Code.</span></span> <span data-ttu-id="3a7a1-109">hello támogatott platformok a következők: a Windows, Linux és MacOS.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-109">hello supported platforms include Windows, Linux, and MacOS.</span></span> <span data-ttu-id="3a7a1-110">hello különböző platformokon a következő előfeltételek hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-110">hello different platforms have hello following prerequisites:</span></span>

- <span data-ttu-id="3a7a1-111">Windows</span><span class="sxs-lookup"><span data-stu-id="3a7a1-111">Windows</span></span>

    - <span data-ttu-id="3a7a1-112">[A Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-112">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span>
    - <span data-ttu-id="3a7a1-113">[Java-futtatókörnyezet Ánjon 8-as verzió frissítési 77 vagy újabb](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-113">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="3a7a1-114">Adja hozzá a hello java.exe elérési toohello környezeti változó elérési útja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-114">Add hello java.exe path toohello system environment variable path.</span></span> <span data-ttu-id="3a7a1-115">Konfigurációs útmutatásért lásd: [hogyan beállítása vagy módosítása hello elérési rendszerváltozó?]( https://www.java.com/download/help/path.xml).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-115">For configuration instructions, see [How do I set or change hello Path system variable?]( https://www.java.com/download/help/path.xml).</span></span> <span data-ttu-id="3a7a1-116">elérési út hello hasonló tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-116">hello path is similar tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.</span></span>
    - <span data-ttu-id="3a7a1-117">[A .NET core SDK 1.0.3 vagy a .NET Core 1.1 futásidejű](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-117">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>
    
- <span data-ttu-id="3a7a1-118">Linux (ajánlott Ubuntu 14.04 LTS)</span><span class="sxs-lookup"><span data-stu-id="3a7a1-118">Linux (We recommend Ubuntu 14.04 LTS)</span></span>

    - <span data-ttu-id="3a7a1-119">[A Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-119">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span> <span data-ttu-id="3a7a1-120">tooinstall hello kódja, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-120">tooinstall hello code, enter hello following command:</span></span>

              sudo dpkg -i code_<version_number>_amd64.deb

    - <span data-ttu-id="3a7a1-121">[Monó 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-121">[Mono 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/).</span></span> 

        - <span data-ttu-id="3a7a1-122">tooupdate hello deb csomag forrás-, adja meg a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-122">tooupdate hello deb package source, enter hello following commands:</span></span>

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - <span data-ttu-id="3a7a1-123">tooinstall Monó, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-123">tooinstall Mono, enter hello following command:</span></span>

                sudo apt-get install mono-complete

            > [!NOTE] 
            > <span data-ttu-id="3a7a1-124">Monó 4.6 nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-124">Mono 4.6 is not supported.</span></span> <span data-ttu-id="3a7a1-125">Távolítsa el a 4.6-os verzió, teljes mértékben 4.2.x telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-125">Uninstall version 4.6 entirely before you install 4.2.x.</span></span>  

        - <span data-ttu-id="3a7a1-126">[Java-futtatókörnyezet Ánjon 8-as verzió frissítési 77 vagy újabb](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-126">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="3a7a1-127">A telepítési utasításokért lásd: hello [Linux 64 bites telepítési utasításokat Java]( https://java.com/en/download/help/linux_x64_install.xml) lap.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-127">For instructions on installation, see hello [Linux 64-bit installation instructions for Java]( https://java.com/en/download/help/linux_x64_install.xml) page.</span></span>
        - <span data-ttu-id="3a7a1-128">[A .NET core SDK 1.0.3 vagy a .NET Core 1.1 futásidejű](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-128">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>
- <span data-ttu-id="3a7a1-129">MacOS</span><span class="sxs-lookup"><span data-stu-id="3a7a1-129">MacOS</span></span>

    - <span data-ttu-id="3a7a1-130">[A Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-130">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span>
    - <span data-ttu-id="3a7a1-131">[Monó 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-131">[Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/).</span></span> 
    - <span data-ttu-id="3a7a1-132">[Java-futtatókörnyezet Ánjon 8-as verzió frissítési 77 vagy újabb](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-132">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="3a7a1-133">A telepítési utasításokért lásd: hello [Linux 64 bites telepítési utasításokat Java](https://java.com/en/download/help/mac_install.xml) lap.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-133">For instructions on installation, see hello [Linux 64-bit installation instructions for Java](https://java.com/en/download/help/mac_install.xml) page.</span></span>
    - <span data-ttu-id="3a7a1-134">[A .NET core SDK 1.0.3 vagy a .NET Core 1.1 futásidejű](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-134">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>

## <a name="install-data-lake-tools"></a><span data-ttu-id="3a7a1-135">A Data Lake Tools telepítése</span><span class="sxs-lookup"><span data-stu-id="3a7a1-135">Install Data Lake Tools</span></span>

<span data-ttu-id="3a7a1-136">Hello előfeltételek telepítése után a Data Lake Tools for VS kód is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-136">After you install hello prerequisites, you can install Data Lake Tools for VS Code.</span></span>

<span data-ttu-id="3a7a1-137">**a Data Lake Tools tooinstall**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-137">**tooinstall Data Lake Tools**</span></span>

1. <span data-ttu-id="3a7a1-138">Nyissa meg a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-138">Open Visual Studio Code.</span></span>
2. <span data-ttu-id="3a7a1-139">Válassza ki a Ctrl + P, és írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-139">Select Ctrl+P, and then enter hello following command:</span></span>
```
ext install usql-vscode-ext
```
<span data-ttu-id="3a7a1-140">A Visual Studio code kiterjesztések listája látható.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-140">You can see a list of Visual Studio code extensions.</span></span> <span data-ttu-id="3a7a1-141">Az egyik **Azure Data Lake Tools**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-141">One of them is **Azure Data Lake Tools**.</span></span>

3. <span data-ttu-id="3a7a1-142">Válassza ki **telepítése** következő túl**Azure Data Lake Tools**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-142">Select **Install** next too**Azure Data Lake Tools**.</span></span> <span data-ttu-id="3a7a1-143">Néhány másodperc múlva hello **telepítése** túl gombra a módosítások**Újrabetöltés**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-143">After a few seconds, hello **Install** button changes too**Reload**.</span></span>
4. <span data-ttu-id="3a7a1-144">Válassza ki **Újrabetöltés** tooactivate hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-144">Select **Reload** tooactivate hello extension.</span></span>
5. <span data-ttu-id="3a7a1-145">Válassza ki **OK** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-145">Select **OK** tooconfirm.</span></span> <span data-ttu-id="3a7a1-146">Azure Data Lake Tools hello látható **bővítmények** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-146">You can see Azure Data Lake Tools in hello **Extensions** pane.</span></span>
    <span data-ttu-id="3a7a1-147">![Data Lake Tools for Visual Studio Code bővítmények ablaktábla](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)</span><span class="sxs-lookup"><span data-stu-id="3a7a1-147">![Data Lake Tools for Visual Studio Code Extensions pane](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)</span></span>

## <a name="activate-azure-data-lake-tools"></a><span data-ttu-id="3a7a1-148">Az Azure Data Lake Tools aktiválása</span><span class="sxs-lookup"><span data-stu-id="3a7a1-148">Activate Azure Data Lake Tools</span></span>
<span data-ttu-id="3a7a1-149">Hozzon létre egy új .usql fájlt, vagy nyisson meg egy meglévő .usql tooactivate hello fájlkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-149">Create a new .usql file or open an existing .usql file tooactivate hello extension.</span></span> 

## <a name="connect-tooazure"></a><span data-ttu-id="3a7a1-150">Csatlakozás tooAzure</span><span class="sxs-lookup"><span data-stu-id="3a7a1-150">Connect tooAzure</span></span>

<span data-ttu-id="3a7a1-151">Mielőtt fordítási, és a Data Lake Analytics U-SQL parancsfájlt, csatlakoznia kell a tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-151">Before you can compile and run U-SQL scripts in Data Lake Analytics, you must connect tooyour Azure account.</span></span>

<span data-ttu-id="3a7a1-152">**tooconnect tooAzure**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-152">**tooconnect tooAzure**</span></span>

1.  <span data-ttu-id="3a7a1-153">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-153">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2.  <span data-ttu-id="3a7a1-154">Adja meg **ADL: bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-154">Enter **ADL: Login**.</span></span> <span data-ttu-id="3a7a1-155">hello bejelentkezési adatok megjelennek-e hello **kimeneti** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-155">hello login information appears in hello **Output** pane.</span></span>

    <span data-ttu-id="3a7a1-156">![A Data Lake Visual Studio Code parancs paletta eszközei](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![a Data Lake Tools for Visual Studio Code eszköz bejelentkezési adatok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)</span><span class="sxs-lookup"><span data-stu-id="3a7a1-156">![Data Lake Tools for Visual Studio Code command palette](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
![Data Lake Tools for Visual Studio Code device login information](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)</span></span>
3. <span data-ttu-id="3a7a1-157">Válassza ki a Ctrl + kattintson a hello bejelentkezési URL-címe: https://aka.ms/devicelogin tooopen hello bejelentkezési képernyőn látható weblapon.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-157">Select Ctrl+click on hello login URL: https://aka.ms/devicelogin tooopen hello login webpage.</span></span> <span data-ttu-id="3a7a1-158">Írja be a kódot hello **G567LX42V** hello szövegmezőbe, majd válassza ki azt a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-158">Enter hello code **G567LX42V** into hello text box, and then select **Continue**.</span></span>

   ![A Data Lake Tools for Visual Studio Code bejelentkezési illessze be a kódot](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  <span data-ttu-id="3a7a1-160">Kövesse a hello utasításokat toosign hello weblap.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-160">Follow hello instructions toosign in from hello webpage.</span></span> <span data-ttu-id="3a7a1-161">Ha kapcsolódik, az Azure-fiók neve megjelenik hello állapotsor hello bal alsó sarkában hello **Visual STUDIO Code** ablak.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-161">When you're connected, your Azure account name appears on hello status bar in hello lower-left corner of hello **VS Code** window.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="3a7a1-162">A fiókjának van engedélyezve, a két tényező, azt javasoljuk, hogy használja-e PIN-kóddal helyett phone hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-162">If your account has two factors enabled, we recommend that you use phone authentication rather than using a PIN.</span></span>

<span data-ttu-id="3a7a1-163">toosign, adja meg a hello parancsot **ADL: kijelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-163">toosign out, enter hello command **ADL: Logout**.</span></span>

## <a name="list-your-data-lake-analytics-accounts"></a><span data-ttu-id="3a7a1-164">A Data Lake Analytics-fiókok listázása</span><span class="sxs-lookup"><span data-stu-id="3a7a1-164">List your Data Lake Analytics accounts</span></span>

<span data-ttu-id="3a7a1-165">tootest hello kapcsolat, a Data Lake Analytics-fiókok listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-165">tootest hello connection, get a list of your Data Lake Analytics accounts.</span></span>

<span data-ttu-id="3a7a1-166">**toolist hello Data Lake Analytics-fiókok alatt az Azure-előfizetéshez**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-166">**toolist hello Data Lake Analytics accounts under your Azure subscription**</span></span>

1. <span data-ttu-id="3a7a1-167">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-167">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2. <span data-ttu-id="3a7a1-168">Adja meg **ADL: fiókok listában**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-168">Enter **ADL: List Accounts**.</span></span> <span data-ttu-id="3a7a1-169">hello fiókok megjelennek-e hello **kimeneti** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-169">hello accounts appear in hello **Output** pane.</span></span>

## <a name="open-hello-sample-script"></a><span data-ttu-id="3a7a1-170">Nyissa meg hello parancsfájlpéldát</span><span class="sxs-lookup"><span data-stu-id="3a7a1-170">Open hello sample script</span></span>
<span data-ttu-id="3a7a1-171">Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és adja meg **ADL: Nyissa meg a minta-parancsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-171">Open hello command palette (Ctrl+Shift+P) and enter **ADL: Open Sample Script**.</span></span> <span data-ttu-id="3a7a1-172">Ekkor megnyílik a minta egy másik példánya.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-172">This opens another instance of this sample.</span></span> <span data-ttu-id="3a7a1-173">Szerkeszthető, konfigurálása, és küldje el a parancsfájl ezen a példányon.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-173">You can also edit, configure, and submit script on this instance.</span></span>

## <a name="work-with-u-sql"></a><span data-ttu-id="3a7a1-174">U-SQL műveletek</span><span class="sxs-lookup"><span data-stu-id="3a7a1-174">Work with U-SQL</span></span>

<span data-ttu-id="3a7a1-175">A U-SQL-fájl vagy egy mappa toowork U-SQL kell megnyitni.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-175">You need open either a U-SQL file or a folder toowork with U-SQL.</span></span>

<span data-ttu-id="3a7a1-176">**egy mappa a U-SQL projekt tooopen**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-176">**tooopen a folder for your U-SQL project**</span></span>

1. <span data-ttu-id="3a7a1-177">A Visual Studio Code, válassza ki a hello **fájl** menüben, majd válassza ki **mappa megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-177">From Visual Studio Code, select hello **File** menu, and then select **Open Folder**.</span></span>
2. <span data-ttu-id="3a7a1-178">Adjon meg egy mappát, majd válassza ki **Mappaválasztás**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-178">Specify a folder, and then select **Select Folder**.</span></span>
3. <span data-ttu-id="3a7a1-179">Jelölje be hello **fájl** menüben, majd válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-179">Select hello **File** menu, and then select **New**.</span></span> <span data-ttu-id="3a7a1-180">Egy névtelen-1 fájl toohello projekt kerül.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-180">An Untitled-1 file is added toohello project.</span></span>
4. <span data-ttu-id="3a7a1-181">Adja meg a kódot hello névtelen-1-fájlba a következő hello:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-181">Enter hello following code into hello Untitled-1 file:</span></span>

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO “/Output/departments.csv”

    <span data-ttu-id="3a7a1-182">hello parancsfájl fájlt hoz létre departments.csv néhány hello/output mappában szereplő adatokat.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-182">hello script creates a departments.csv file with some data included in hello /output folder.</span></span>

5. <span data-ttu-id="3a7a1-183">Hello fájl mentése másként **myUSQL.usql** hello a megnyitni a mappát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-183">Save hello file as **myUSQL.usql** in hello opened folder.</span></span> <span data-ttu-id="3a7a1-184">Adltools_settings.json konfigurációs fájlt is bővült toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-184">A adltools_settings.json configuration file is also added toohello project.</span></span>
4. <span data-ttu-id="3a7a1-185">Nyissa meg, majd konfigurálja adltools_settings.json hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-185">Open and configure adltools_settings.json with hello following properties:</span></span>

    - <span data-ttu-id="3a7a1-186">Fiók: Egy Data Lake Analytics-fiók alatt az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-186">Account:  A Data Lake Analytics account under your Azure subscription.</span></span>
    - <span data-ttu-id="3a7a1-187">Adatbázis: A fiók alatt adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-187">Database: A database under your account.</span></span> <span data-ttu-id="3a7a1-188">hello alapértelmezett érték a **fő**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-188">hello default is **master**.</span></span>
    - <span data-ttu-id="3a7a1-189">Séma: Az adatbázis a séma.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-189">Schema: A schema under your database.</span></span> <span data-ttu-id="3a7a1-190">hello alapértelmezett érték a **dbo**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-190">hello default is **dbo**.</span></span>
    - <span data-ttu-id="3a7a1-191">Választható beállítások:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-191">Optional settings:</span></span>
        - <span data-ttu-id="3a7a1-192">Prioritás: hello prioritás tartomány: 1 1 too1000 hello legmagasabb prioritás szerint.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-192">Priority: hello priority range is from 1 too1000 with 1 as hello highest priority.</span></span> <span data-ttu-id="3a7a1-193">hello alapértelmezett értéke **1000**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-193">hello default value is **1000**.</span></span>
        - <span data-ttu-id="3a7a1-194">Párhuzamossági: hello párhuzamossági tartomány: 1 too150.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-194">Parallelism: hello parallelism range is from 1 too150.</span></span> <span data-ttu-id="3a7a1-195">hello alapértelmezett értéke hello maximális párhuzamossági engedélyezett az Azure Data Lake Analytics-fiókja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-195">hello default value is hello maximum parallelism allowed in your Azure Data Lake Analytics account.</span></span> 
        
        > [!NOTE] 
        > <span data-ttu-id="3a7a1-196">Ha hello beállítások érvénytelenek, hello alapértelmezett értékeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-196">If hello settings are invalid, hello default values are used.</span></span>

    ![A Data Lake Tools for Visual Studio Code konfigurációs fájl](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    <span data-ttu-id="3a7a1-198">A Data Lake Analytics-fiók számítási toocompile és run U-SQL feladatok szükséges.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-198">A compute Data Lake Analytics account is needed toocompile and run U-SQL jobs.</span></span> <span data-ttu-id="3a7a1-199">Fordítsa le és U-SQL feladatok futtatása előtt konfigurálnia kell az hello számítógépfiókját.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-199">You must configure hello computer account before you can compile and run U-SQL jobs.</span></span>
    
<span data-ttu-id="3a7a1-200">Hello konfiguráció mentése után hello állapotsor hello megfelelő .usql fájlt hello bal alsó sarkában megjelenő hello fiók, adatbázis és séma információk.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-200">After hello configuration is saved, hello account, database, and schema information appears on hello status bar at hello bottom-left corner of hello corresponding .usql file.</span></span> 
 
 
<span data-ttu-id="3a7a1-201">Az összehasonlított tooopening egy fájlt, amikor megnyit egy mappát is:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-201">Compared tooopening a file, when you open a folder you can:</span></span>

- <span data-ttu-id="3a7a1-202">A háttérkód fájl használható.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-202">Use a code-behind file.</span></span> <span data-ttu-id="3a7a1-203">Háttérkód hello egyfájlos módban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-203">In hello single-file mode, code-behind is not supported.</span></span>
- <span data-ttu-id="3a7a1-204">A konfigurációs fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-204">Use a configuration file.</span></span> <span data-ttu-id="3a7a1-205">Ha a mappa megnyitásához munkamappa hello hello parancsfájlok osztják meg egyetlen konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-205">When you open a folder, hello scripts in hello working folder share a single configuration file.</span></span>


<span data-ttu-id="3a7a1-206">U-SQL parancsfájl hello távolról lefordítja a Data Lake Analytics szolgáltatás hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-206">hello U-SQL script compiles remotely through hello Data Lake Analytics service.</span></span> <span data-ttu-id="3a7a1-207">Hello küldésekor **fordítási** parancs hello U-SQL parancsfájl küldött tooyour Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-207">When you issue hello **compile** command, hello U-SQL script is sent tooyour Data Lake Analytics account.</span></span> <span data-ttu-id="3a7a1-208">Később a Visual Studio Code hello fordítási eredménye kap.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-208">Later, Visual Studio Code receives hello compilation result.</span></span> <span data-ttu-id="3a7a1-209">Miatt toohello távoli fordítási a Visual Studio Code van szükség, hogy Ön tooconnect tooyour Data Lake Analytics-fiók hello konfigurációs fájlban hello-információinak felsorolásához.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-209">Due toohello remote compilation, Visual Studio Code requires that you list hello information tooconnect tooyour Data Lake Analytics account in hello configuration file.</span></span>

<span data-ttu-id="3a7a1-210">**toocompile egy U-SQL parancsfájl**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-210">**toocompile a U-SQL script**</span></span>

1. <span data-ttu-id="3a7a1-211">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-211">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2. <span data-ttu-id="3a7a1-212">Adja meg **ADL: parancsfájl összeállításához**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-212">Enter **ADL: Compile Script**.</span></span> <span data-ttu-id="3a7a1-213">hello fordítási eredményei jelennek meg hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-213">hello compile results appear in hello **Output** window.</span></span> <span data-ttu-id="3a7a1-214">Jobb gombbal egy olyan parancsfájlt, és válassza **ADL: parancsfájl összeállításához** toocompile egy U-SQL-feladatot.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-214">You can also right-click a script file, and then select **ADL: Compile Script** toocompile a U-SQL job.</span></span> <span data-ttu-id="3a7a1-215">hello fordítási eredménye megjelenik hello **kimeneti** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-215">hello compilation result appears in hello **Output** pane.</span></span>
 

<span data-ttu-id="3a7a1-216">**toosubmit egy U-SQL parancsfájl**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-216">**toosubmit a U-SQL script**</span></span>

1. <span data-ttu-id="3a7a1-217">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-217">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2. <span data-ttu-id="3a7a1-218">Adja meg **ADL: elküldeni a feladatot**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-218">Enter **ADL: Submit Job**.</span></span>  <span data-ttu-id="3a7a1-219">Jobb gombbal egy olyan parancsfájlt, és válassza **ADL: feladat elküldése**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-219">You can also right-click a script file, and then select **ADL: Submit Job**.</span></span> 

<span data-ttu-id="3a7a1-220">A U-SQL-feladat elküldése után hello küldésének naplók megjelennek-e hello **kimeneti** ablak a Visual STUDIO Code.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-220">After you submit a U-SQL job, hello submission logs appear in hello **Output** window in VS Code.</span></span> <span data-ttu-id="3a7a1-221">Ha hello elküldése sikeres, hello feladat URL-címet is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-221">If hello submission is successful, hello job URL appears as well.</span></span> <span data-ttu-id="3a7a1-222">Hello feladat URL egy webes böngésző tootrack hello valós idejű feladat állapotát a nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-222">You can open hello job URL in a web browser tootrack hello real-time job status.</span></span>

<span data-ttu-id="3a7a1-223">tooenable hello kimenete hello feladat részleteit, állítsa be **jobInformationOutputPath** a hello **vs helykódja hello u-sql_settings.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-223">tooenable hello output of hello job details, set **jobInformationOutputPath** in hello **vs code for hello u-sql_settings.json** file.</span></span>
 
## <a name="use-a-code-behind-file"></a><span data-ttu-id="3a7a1-224">A háttérkód fájl használata</span><span class="sxs-lookup"><span data-stu-id="3a7a1-224">Use a code-behind file</span></span>

<span data-ttu-id="3a7a1-225">A háttérkód fájlt egy U-SQL parancsfájl társított C# fájlban.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-225">A code-behind file is a C# file associated with a single U-SQL script.</span></span> <span data-ttu-id="3a7a1-226">Hello háttérkód fájlban definiálhat egy dedikált parancsfájl tooUDO, uda-Értékeket, UDT és UDF.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-226">You can define a script dedicated tooUDO, UDA, UDT, and UDF in hello code-behind file.</span></span> <span data-ttu-id="3a7a1-227">hello UDO, uda-Értékeket, UDT és UDF használható közvetlenül a hello parancsfájl hello szerelvény először regisztrálása nélkül.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-227">hello UDO, UDA, UDT, and UDF can be used directly in hello script without registering hello assembly first.</span></span> <span data-ttu-id="3a7a1-228">hello háttérkód fájl kerül, hello azonos mappában található, mint a társviszony-létesítési U-SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-228">hello code-behind file is put in hello same folder as its peering U-SQL script file.</span></span> <span data-ttu-id="3a7a1-229">Ha hello parancsfájl neve xxx.usql, hello háttérkód, xxx.usql.cs neve.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-229">If hello script is named xxx.usql, hello code-behind is named as xxx.usql.cs.</span></span> <span data-ttu-id="3a7a1-230">Ha manuálisan törli hello háttérkód fájlt, hello háttérkód funkció le van tiltva, a társított U-SQL parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-230">If you manually delete hello code-behind file, hello code-behind feature is disabled for its associated U-SQL script.</span></span> <span data-ttu-id="3a7a1-231">További információ a felhasználói kódot a U-SQL parancsfájl: [írási és egyéni kód használata U-SQL: felhasználó által definiált függvényeket]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-231">For more information about writing customer code for U-SQL script, see [Writing and Using Custom Code in U-SQL: User-Defined Functions]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).</span></span>

<span data-ttu-id="3a7a1-232">toosupport háttérkód, meg kell nyitnia egy munkamappa.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-232">toosupport code-behind, you must open a working folder.</span></span> 

<span data-ttu-id="3a7a1-233">**a háttérkód fájl toogenerate**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-233">**toogenerate a code-behind file**</span></span>

1. <span data-ttu-id="3a7a1-234">A forrásfájl megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-234">Open a source file.</span></span> 
2. <span data-ttu-id="3a7a1-235">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-235">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
3. <span data-ttu-id="3a7a1-236">Adja meg **ADL: mögött kód generálása**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-236">Enter **ADL: Generate Code Behind**.</span></span> <span data-ttu-id="3a7a1-237">A háttérkód fájl jön létre hello azonos mappába.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-237">A code-behind file is created in hello same folder.</span></span> 

<span data-ttu-id="3a7a1-238">Jobb gombbal egy olyan parancsfájlt, és válassza **ADL: kód generálása mögött**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-238">You can also right-click a script file, and then select **ADL: Generate Code Behind**.</span></span> 

<span data-ttu-id="3a7a1-239">toocompile, és küldje el a háttérkód fájlt tartalmazó U-SQL parancsfájl van hello megegyeznek a hello önálló U-SQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-239">toocompile and submit a U-SQL script with a code-behind file is hello same as with hello standalone U-SQL script file.</span></span>

<span data-ttu-id="3a7a1-240">a következő két képernyőképeket hello megjelenítése a háttérkód fájl- és a kapcsolódó U-SQL-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-240">hello following two screenshots show a code-behind file and its associated U-SQL script file:</span></span>
 
![A Data Lake Tools for Visual Studio Code háttérkód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![A Data Lake Tools for Visual Studio Code háttérkód parancsfájl](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a><span data-ttu-id="3a7a1-243">Szerelvények használata</span><span class="sxs-lookup"><span data-stu-id="3a7a1-243">Use assemblies</span></span>

<span data-ttu-id="3a7a1-244">Szerelvények fejlesztéséről további információkért lásd: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-244">For information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>

<span data-ttu-id="3a7a1-245">A Data Lake Tools tooregister egyéni kód szerelvények hello Data Lake Analytics-katalógus használható.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-245">You can use Data Lake Tools tooregister custom code assemblies in hello Data Lake Analytics catalog.</span></span>

<span data-ttu-id="3a7a1-246">**egy szerelvény tooregister**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-246">**tooregister an assembly**</span></span>

<span data-ttu-id="3a7a1-247">Hello szerelvény hello segítségével regisztrálhatja **ADL: szerelvény regisztrálása** vagy **ADL: szerelvény regisztrálása konfigurálással** parancsok.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-247">You can register hello assembly through hello **ADL: Register Assembly** or **ADL: Register Assembly through Configuration** commands.</span></span>

<span data-ttu-id="3a7a1-248">**tooregister keresztül hello ADL: szerelvény regisztrálása parancs**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-248">**tooregister through hello ADL: Register Assembly command**</span></span>
1.  <span data-ttu-id="3a7a1-249">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-249">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2.  <span data-ttu-id="3a7a1-250">Adja meg **ADL: szerelvény regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-250">Enter **ADL: Register Assembly**.</span></span> 
3.  <span data-ttu-id="3a7a1-251">Adja meg a hello helyi szerelvény elérési útja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-251">Specify hello local assembly path.</span></span> 
4.  <span data-ttu-id="3a7a1-252">Válassza ki a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-252">Select a Data Lake Analytics account.</span></span>
5.  <span data-ttu-id="3a7a1-253">Válasszon ki egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-253">Select a database.</span></span>

<span data-ttu-id="3a7a1-254">Eredmények: hello portál egy böngészőben meg van nyitva, és hello szerelvény regisztrációs folyamat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-254">Results: hello portal is opened in a browser and displays hello assembly registration process.</span></span>  

<span data-ttu-id="3a7a1-255">Egy másik kényelmesen tootrigger hello **ADL: szerelvény regisztrálása** parancs a Fájlkezelőben tooright kattintással hello .dll fájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-255">Another convenient way tootrigger hello **ADL: Register Assembly** command is tooright-click hello .dll file in File Explorer.</span></span> 

<span data-ttu-id="3a7a1-256">**tooregister azonban hello ADL: szerelvény regisztrálása konfigurációs paranccsal**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-256">**tooregister though hello ADL: Register Assembly through Configuration command**</span></span>
1.  <span data-ttu-id="3a7a1-257">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-257">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2.  <span data-ttu-id="3a7a1-258">Adja meg **ADL: szerelvény konfigurálással regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-258">Enter **ADL: Register Assembly through Configuration**.</span></span> 
3.  <span data-ttu-id="3a7a1-259">Adja meg a hello helyi szerelvény elérési útja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-259">Specify hello local assembly path.</span></span> 
4.  <span data-ttu-id="3a7a1-260">hello JSON-fájl jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-260">hello JSON file is displayed.</span></span> <span data-ttu-id="3a7a1-261">Tekintse át, és szükség esetén szerkessze a hello szerelvény függőségeit és erőforrás-paraméterek.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-261">Review and edit hello assembly dependencies and resource parameters, if needed.</span></span> <span data-ttu-id="3a7a1-262">Utasítások jelennek meg hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-262">Instructions are displayed in hello **Output** window.</span></span> <span data-ttu-id="3a7a1-263">tooproceed toohello szerelvények regisztrálásakor, (Ctrl + S) hello JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-263">tooproceed toohello assembly registration, save (Ctrl+S) hello JSON file.</span></span>

![A Data Lake Tools for Visual Studio Code háttérkód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- <span data-ttu-id="3a7a1-265">Szerelvény függőségek: Azure Data Lake Tools egy hello dll-fájl rendelkezik-e függőségekről.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-265">Assembly dependencies: Azure Data Lake Tools autodetects whether hello DLL has any dependencies.</span></span> <span data-ttu-id="3a7a1-266">hello függőségek hello JSON-fájlban jelennek meg, miután azokat.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-266">hello dependencies are displayed in hello JSON file after they are detected.</span></span> 
>- <span data-ttu-id="3a7a1-267">Erőforrás: A DLL-erőforrások (például .txt, .png és .csv) feltöltheti hello szerelvények regisztrálásakor részeként.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-267">Resources: You can upload your DLL resources (for example, .txt, .png, and .csv) as part of hello assembly registration.</span></span> 

<span data-ttu-id="3a7a1-268">Egy másik módja tootrigger hello **ADL: szerelvény regisztrálása konfigurálással** parancs a Fájlkezelőben tooright kattintással hello .dll fájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-268">Another way tootrigger hello **ADL: Register Assembly through Configuration** command is tooright-click hello .dll file in File Explorer.</span></span> 

<span data-ttu-id="3a7a1-269">U-SQL-kódot a következő hello bemutatja, hogyan toocall szerelvényt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-269">hello following U-SQL code demonstrates how toocall an assembly.</span></span> <span data-ttu-id="3a7a1-270">Hello mintában hello szerelvény neve: *tesztelése*.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-270">In hello sample, hello assembly name is *test*.</span></span>

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    too@"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```


## <a name="access-hello-data-lake-analytics-catalog"></a><span data-ttu-id="3a7a1-271">Hello Data Lake Analytics-katalógus elérése</span><span class="sxs-lookup"><span data-stu-id="3a7a1-271">Access hello Data Lake Analytics catalog</span></span>

<span data-ttu-id="3a7a1-272">Miután csatlakozott a tooAzure, a következő lépéseket tooaccess hello U-SQL catalog hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-272">After you have connected tooAzure, you can use hello following steps tooaccess hello U-SQL catalog.</span></span>

<span data-ttu-id="3a7a1-273">**tooaccess hello Azure Data Lake Analytics metaadatok**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-273">**tooaccess hello Azure Data Lake Analytics metadata**</span></span>

1.  <span data-ttu-id="3a7a1-274">Válassza ki a Ctrl + Shift + P, és írja be **ADL: listáját táblákat**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-274">Select Ctrl+Shift+P, and then enter **ADL: List Tables**.</span></span>
2.  <span data-ttu-id="3a7a1-275">Válasszon ki egy hello Data Lake Analytics-fiókok.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-275">Select one of hello Data Lake Analytics accounts.</span></span>
3.  <span data-ttu-id="3a7a1-276">Válasszon ki egy hello Data Lake Analytics-adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-276">Select one of hello Data Lake Analytics databases.</span></span>
4.  <span data-ttu-id="3a7a1-277">Válasszon ki egy hello sémák.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-277">Select one of hello schemas.</span></span> <span data-ttu-id="3a7a1-278">Hello táblák listáját tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-278">You can see hello list of tables.</span></span>

## <a name="view-data-lake-analytics-jobs"></a><span data-ttu-id="3a7a1-279">Nézet Data Lake Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="3a7a1-279">View Data Lake Analytics jobs</span></span>

<span data-ttu-id="3a7a1-280">**tooview Data Lake Analytics-feladatok**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-280">**tooview Data Lake Analytics jobs**</span></span>
1.  <span data-ttu-id="3a7a1-281">Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és válassza ki **ADL: a feladat megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-281">Open hello command palette (Ctrl+Shift+P) and select **ADL: Show Job**.</span></span> 
2.  <span data-ttu-id="3a7a1-282">Válasszon egy Data Lake Analytics vagy helyi fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-282">Select a Data Lake Analytics or local account.</span></span> 
3.  <span data-ttu-id="3a7a1-283">Várjon, amíg hello fiók tooappear hello feladatok listája.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-283">Wait for hello jobs list for hello account tooappear.</span></span>
4.  <span data-ttu-id="3a7a1-284">Jelölje ki a feladatot feladatot a listából, Data Lake Tools hello feladat részletei megnyitja hello Azure-portálon, és hello Feladatinformáció fájl és a kód jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-284">Select a job from job list, Data Lake Tools opens hello job details in hello Azure portal and displays hello JobInfo file in VS Code.</span></span>

![A Data Lake Tools for Visual Studio Code IntelliSense objektumtípusok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a><span data-ttu-id="3a7a1-286">Azure Data Lake tároló integrációja</span><span class="sxs-lookup"><span data-stu-id="3a7a1-286">Azure Data Lake Storage integration</span></span>

<span data-ttu-id="3a7a1-287">Az Azure Data Lake tárolással kapcsolatos parancsokat használhatja:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-287">You can use Azure Data Lake Storage-related commands to:</span></span>
 - <span data-ttu-id="3a7a1-288">Tallózzon a hello Azure Data Lake tárolási erőforrások.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-288">Browse through hello Azure Data Lake Storage resources.</span></span> 
 - <span data-ttu-id="3a7a1-289">Előzetes hello Azure Data Lake-tárolási fájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-289">Preview hello Azure Data Lake Storage file.</span></span>  
 - <span data-ttu-id="3a7a1-290">Töltse fel a hello közvetlenül tooAzure Data Lake-tárolási fájl a Visual STUDIO Code.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-290">Upload hello file directly tooAzure Data Lake Storage in VS Code.</span></span> 

### <a name="list-hello-storage-path"></a><span data-ttu-id="3a7a1-291">Lista hello. tárolási elérési útja</span><span class="sxs-lookup"><span data-stu-id="3a7a1-291">List hello storage path</span></span> 
<span data-ttu-id="3a7a1-292">Hello. tárolási elérési útja vagy kattintson a jobb gombbal az hello parancs paletta szolgáltatással is listázhatja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-292">You can list hello storage path through hello command palette or through right-click.</span></span>

<span data-ttu-id="3a7a1-293">**hello parancs paletta keresztül toolist hello tárolási elérési útja**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-293">**toolist hello storage path through hello command palette**</span></span>

1.  <span data-ttu-id="3a7a1-294">Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és adja meg **ADL: lista. tárolási elérési útja**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-294">Open hello command palette (Ctrl+Shift+P) and enter **ADL: List Storage Path**.</span></span>

    ![A Data Lake Tools for Visual Studio Code lista. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  <span data-ttu-id="3a7a1-296">Válassza ki a kívánt módon felsoroló hello. tárolási elérési útja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-296">Select your preferred way for listing hello storage path.</span></span> <span data-ttu-id="3a7a1-297">Az átjáró használja **adjon meg egy elérési utat** példaként.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-297">This passage uses **Enter a path** as an example.</span></span>

    ![A Data Lake Tools for Visual Studio Code egyirányú toolist hello. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- <span data-ttu-id="3a7a1-299">Visual STUDIO Code tartja hello utoljára meglátogatott elérési út minden Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-299">VS Code keeps hello last-visited path in every Data Lake Analytics account.</span></span> <span data-ttu-id="3a7a1-300">Például: / tt/ss.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-300">For example: /tt/ss.</span></span>
    >- <span data-ttu-id="3a7a1-301">Legfelső szintű elérési útról böngésző: hello lista gyökérelérési útja a kijelölt Data Lake Analytics-fiók vagy helyi elérési utat.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-301">Browser from root path: hello list root path from your selected Data Lake Analytics account or a local path.</span></span>
    >- <span data-ttu-id="3a7a1-302">Adjon meg egy elérési utat: egy adott elérési úton, a kiválasztott Data Lake Analytics-fiókhoz tartozó vagy egy helyi elérési útját listában.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-302">Enter a path: List a specified path from your selected Data Lake Analytics account or a local path.</span></span>
    
3. <span data-ttu-id="3a7a1-303">Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-303">Select an account from hello local path or a Data Lake Analytics account.</span></span>

    ![A Data Lake Tools for Visual Studio Code több kiválasztása](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. <span data-ttu-id="3a7a1-305">Válassza ki **további** toolist további Data Lake Analytics-fiókok, és válassza a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-305">Select **more** toolist more Data Lake Analytics accounts, and then select a Data Lake Analytics account.</span></span>

    ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  <span data-ttu-id="3a7a1-307">Az Azure storage elérési útjának megadása</span><span class="sxs-lookup"><span data-stu-id="3a7a1-307">Enter an Azure storage path.</span></span> <span data-ttu-id="3a7a1-308">Például/kimeneti.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-308">For example, /output.</span></span>

    ![A Data Lake Tools for Visual Studio Code, adja meg. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  <span data-ttu-id="3a7a1-310">Eredmények: hello parancs paletta hello útvonal-információinak alapján sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-310">Results: hello command palette lists hello path information based on your entries.</span></span>

    ![A Data Lake Tools for Visual Studio Code tárolási elérési útja eredmények felsorolása](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

<span data-ttu-id="3a7a1-312">Toolist hello relatív elérési út keresztül hello kényelmesebb úgy kattintson a jobb gombbal a helyi menü.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-312">A more convenient way toolist hello relative path is through hello right-click context menu.</span></span>

<span data-ttu-id="3a7a1-313">**Kattintson jobb gombbal az toolist hello. tárolási elérési útja keresztül**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-313">**toolist hello storage path through right-click**</span></span>

1.  <span data-ttu-id="3a7a1-314">Kattintson a jobb gombbal a hello elérési út karakterlánc tooselect **lista. tárolási elérési útja**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-314">Right-click hello path string tooselect **List Storage Path**.</span></span>

       ![A Data Lake Tools for Visual Studio Code kattintson a jobb gombbal a helyi menü](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. <span data-ttu-id="3a7a1-316">hello kijelölt relatív elérési út hello parancs paletta jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-316">hello selected relative path appears in hello command palette.</span></span>

   ![A Data Lake Tools for Visual Studio Code kijelölt relatív elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  <span data-ttu-id="3a7a1-318">Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-318">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  <span data-ttu-id="3a7a1-320">Eredmények: hello parancs paletta hello mappák és fájlok hello aktuális útvonal sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-320">Results: hello command palette lists hello folders and files for hello current path.</span></span>

       ![Data Lake Tools for Visual Studio Code listáját a következőtől: hello aktuális elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-hello-storage-file"></a><span data-ttu-id="3a7a1-322">Előzetes hello tárolófájl</span><span class="sxs-lookup"><span data-stu-id="3a7a1-322">Preview hello storage file</span></span>
<span data-ttu-id="3a7a1-323">Megtekintheti a hello tárolófájl, vagy kattintson a jobb gombbal az hello parancs paletta szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-323">You can preview hello storage file through hello command palette or through right-click.</span></span>

<span data-ttu-id="3a7a1-324">**toopreview hello tárolófájl hello parancs paletta keresztül**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-324">**toopreview hello storage file through hello command palette**</span></span>

1.  <span data-ttu-id="3a7a1-325">Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és adja meg **ADL: Preview tárolófájl**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-325">Open hello command palette (Ctrl+Shift+P) and enter **ADL: Preview Storage File**.</span></span>

       ![A Data Lake Tools for Visual Studio Code preview tárolófájl](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  <span data-ttu-id="3a7a1-327">Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-327">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![A Data Lake Tools for Visual Studio Code listában fiók](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  <span data-ttu-id="3a7a1-329">Válassza ki **további** toolist további Data Lake Analytics-fiókok, és válassza a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-329">Select **more** toolist more Data Lake Analytics accounts, and then select a Data Lake Analytics account.</span></span>

       ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  <span data-ttu-id="3a7a1-331">Adjon meg egy Azure storage elérési út vagy fájlnév.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-331">Enter an Azure storage path or file.</span></span> <span data-ttu-id="3a7a1-332">Például /output/SearchLog.txt.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-332">For example, /output/SearchLog.txt.</span></span>

       ![A Data Lake Tools for Visual Studio Code adja meg a tároló elérési útja és fájlneve](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  <span data-ttu-id="3a7a1-334">Eredmények: hello parancs paletta hello útvonal-információinak alapján sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-334">Results: hello command palette lists hello path information based on your entries.</span></span>

       ![A Data Lake Tools for Visual Studio Code előzetes fájl eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

<span data-ttu-id="3a7a1-336">**Kattintson jobb gombbal az toolist hello. tárolási elérési útja keresztül**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-336">**toolist hello storage path through right-click**</span></span>

1.  <span data-ttu-id="3a7a1-337">toopreview egy fájlt, kattintson a jobb gombbal hello fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-337">toopreview a file, right-click hello file path.</span></span>

   ![A Data Lake Tools for Visual Studio Code kattintson a jobb gombbal a helyi menü](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  <span data-ttu-id="3a7a1-339">Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-339">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  <span data-ttu-id="3a7a1-341">Eredmények: Visual STUDIO Code hello fájl hello előnézeti eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-341">Results: VS Code displays hello preview results of hello file.</span></span>

       ![A Data Lake Tools for Visual Studio Code előzetes fájl eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a><span data-ttu-id="3a7a1-343">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="3a7a1-343">Upload a file</span></span> 

<span data-ttu-id="3a7a1-344">Fájlok feltöltheti hello parancsok beírásával **ADL: fájl feltöltése** vagy **ADL: a konfigurációs fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-344">You can upload files by entering hello commands **ADL: Upload File** or **ADL: Upload File through Configuration**.</span></span>

<span data-ttu-id="3a7a1-345">**tooupload fájlok azonban hello ADL: a fájl feltöltése parancs**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-345">**tooupload files though hello ADL: Upload File command**</span></span>
1. <span data-ttu-id="3a7a1-346">Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, vagy kattintson a jobb gombbal a hello parancsprogram-szerkesztő, és írja be **fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-346">Select Ctrl+Shift+P tooopen hello command palette or right-click hello script editor, and then enter **Upload File**.</span></span>
2.  <span data-ttu-id="3a7a1-347">tooupload hello adja meg a helyi elérési utat.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-347">tooupload hello file, enter a local path.</span></span>

    ![A Data Lake Tools for Visual Studio Code helyi elérési utat adjon meg.](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. <span data-ttu-id="3a7a1-349">Válasszon ki egy listát hello. tárolási elérési útja hello módjait.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-349">Select one of hello ways of listing hello storage path.</span></span> <span data-ttu-id="3a7a1-350">Az átjáró használja **adjon meg egy elérési utat** példaként.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-350">This passage uses **Enter a path** as an example.</span></span>

    ![A Data Lake Tools for Visual Studio Code lista. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- <span data-ttu-id="3a7a1-352">Visual STUDIO Code tartja hello utoljára meglátogatott elérési út minden Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-352">VS Code keeps hello last-visited path in every Data Lake Analytics account.</span></span> <span data-ttu-id="3a7a1-353">Például: / tt/ss.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-353">For example: /tt/ss.</span></span>
    >- <span data-ttu-id="3a7a1-354">Legfelső szintű elérési útról böngésző: hello lista gyökérelérési útja a kijelölt Data Lake Analytics-fiók vagy helyi elérési utat.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-354">Browser from root path: hello list root path from your selected Data Lake Analytics account or a local path.</span></span>
    >- <span data-ttu-id="3a7a1-355">Adjon meg egy elérési utat: egy adott elérési úton, a kiválasztott Data Lake Analytics-fiókhoz tartozó vagy egy helyi elérési útját listában.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-355">Enter a path: List a specified path from your selected Data Lake Analytics account or a local path.</span></span>

4. <span data-ttu-id="3a7a1-356">Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-356">Select an account from hello local path or a Data Lake Analytics account.</span></span>

    ![A Data Lake Tools for Visual Studio Code kattintson a jobb gombbal a tároló](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. <span data-ttu-id="3a7a1-358">Az Azure storage elérési útjának megadása</span><span class="sxs-lookup"><span data-stu-id="3a7a1-358">Enter an Azure storage path.</span></span> <span data-ttu-id="3a7a1-359">Például: / kimeneti.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-359">For example: /output.</span></span>

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. <span data-ttu-id="3a7a1-360">Az Azure storage elérési út található.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-360">Find your Azure storage path.</span></span> <span data-ttu-id="3a7a1-361">Válassza ki **válasszon aktuális mappa**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-361">Select **Choose current folder**.</span></span>

    ![A Data Lake Tools for Visual Studio Code, válasszon ki egy mappát](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  <span data-ttu-id="3a7a1-363">Eredmények: hello **kimeneti** ablak hello fájl feltöltési állapotot jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-363">Results: hello **Output** window displays hello file upload status.</span></span>

       ![A Data Lake Tools for Visual Studio Code feltöltési állapot](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

<span data-ttu-id="3a7a1-365">**tooupload fájlok azonban hello ADL: konfigurációs paranccsal fájl feltöltése**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-365">**tooupload files though hello ADL: Upload File through Configuration command**</span></span>
1.  <span data-ttu-id="3a7a1-366">Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, vagy kattintson a jobb gombbal a hello parancsprogram-szerkesztő, és írja be **fájl feltöltése konfigurálással**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-366">Select Ctrl+Shift+P tooopen hello command palette or right-click hello script editor, and then enter **Upload File through Configuration**.</span></span>
2.  <span data-ttu-id="3a7a1-367">Visual STUDIO Code jeleníti meg a JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-367">VS Code displays a JSON file.</span></span> <span data-ttu-id="3a7a1-368">Adja meg az elérési utat, és a hello több fájl feltöltése ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-368">You can enter file paths and upload multiple files at hello same time.</span></span> <span data-ttu-id="3a7a1-369">Utasítások jelennek meg hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-369">Instructions are displayed in hello **Output** window.</span></span> <span data-ttu-id="3a7a1-370">tooproceed tooupload hello fájl (Ctrl + S) hello JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-370">tooproceed tooupload hello file, save (Ctrl+S) hello JSON file.</span></span>

       ![A Data Lake Tools for Visual Studio Code-fájl elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  <span data-ttu-id="3a7a1-372">Eredmények: hello **kimeneti** ablak hello fájl feltöltési állapotot jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-372">Results: hello **Output** window displays hello file upload status.</span></span>

       ![A Data Lake Tools for Visual Studio Code feltöltési állapot](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

<span data-ttu-id="3a7a1-374">A fájl toostorage hello keresztül történik egy másik módja tooupload kattintson a jobb gombbal hello fájl teljes elérési útja vagy az hello fájl relatív elérési út az hello parancsprogram-szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-374">Another way tooupload a file toostorage is through hello right-click menu on hello file's full path or hello file's relative path in hello script editor.</span></span> <span data-ttu-id="3a7a1-375">Hello helyi fájl elérési útját adja meg, és válassza a hello fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-375">Enter hello local file path, and then select hello account.</span></span> <span data-ttu-id="3a7a1-376">Hello **kimeneti** ablak hello feltöltési állapotot jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-376">hello **Output** window displays hello upload status.</span></span> 

### <a name="open-azure-storage-explorer"></a><span data-ttu-id="3a7a1-377">Nyissa meg az Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="3a7a1-377">Open Azure Storage Explorer</span></span>
<span data-ttu-id="3a7a1-378">Megnyithatja **Azure Tártallózó** hello parancs megadásával **ADL: Nyissa meg a webalkalmazás Azure Tártallózó** vagy hello kattintson a jobb gombbal helyi menüből való kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-378">You can open **Azure Storage Explorer** by entering hello command **ADL: Open Web Azure Storage Explorer** or by selecting it from hello right-click context menu.</span></span>

<span data-ttu-id="3a7a1-379">**Azure Tártallózó tooopen**</span><span class="sxs-lookup"><span data-stu-id="3a7a1-379">**tooopen Azure Storage Explorer**</span></span>

1. <span data-ttu-id="3a7a1-380">Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-380">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2. <span data-ttu-id="3a7a1-381">Adja meg **megnyitása webes Azure Tártallózó** , vagy kattintson a jobb gombbal egy relatív elérési út vagy hello hello parancsprogram-szerkesztő a teljes elérési útja, és válassza **megnyitása webes Azure Tártallózó**.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-381">Enter **Open Web Azure Storage Explorer** or right-click on a relative path or hello full path in hello script editor, and then select **Open Web Azure Storage Explorer**.</span></span>
3. <span data-ttu-id="3a7a1-382">Válassza ki a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-382">Select a Data Lake Analytics account.</span></span>

<span data-ttu-id="3a7a1-383">A Data Lake Tools hello az Azure storage elérési hello Azure-portál megnyitása.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-383">Data Lake Tools opens hello Azure storage path in hello Azure portal.</span></span> <span data-ttu-id="3a7a1-384">Hello elérési útját és preview hello fájl hello webkiszolgálóról található.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-384">You can find hello path and preview hello file from hello web.</span></span>

### <a name="local-run-and-local-debug-for-windows-users"></a><span data-ttu-id="3a7a1-385">Helyi futtatáskor és a helyi hibakeresése Windows felhasználók</span><span class="sxs-lookup"><span data-stu-id="3a7a1-385">Local run and local debug for Windows users</span></span>
<span data-ttu-id="3a7a1-386">U-SQL helyi futtatásával teszteli a helyi adatok, és érvényesíti a parancsfájl helyi, a kódot csak közzétett tooData Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-386">U-SQL local run tests your local data and validates your script locally, before your code is published tooData Lake Analytics.</span></span> <span data-ttu-id="3a7a1-387">hello helyi hibakeresési funkció lehetővé teszi, hogy Ön toocomplete hello feladatok követően a kód benyújtott tooData Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-387">hello local debug feature enables you toocomplete hello following tasks before your code is submitted tooData Lake Analytics:</span></span> 
- <span data-ttu-id="3a7a1-388">A C# háttérkód hibakeresési.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-388">Debug your C# code-behind.</span></span> 
- <span data-ttu-id="3a7a1-389">Hello kód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-389">Step through hello code.</span></span> 
- <span data-ttu-id="3a7a1-390">A parancsfájl helyi ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-390">Validate your script locally.</span></span>

<span data-ttu-id="3a7a1-391">A helyi futtatás helyi hibakeresési útmutatásért lásd: [U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-391">For instructions on local run and local debug, see [U-SQL local run and local debug with Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span></span>

## <a name="additional-features"></a><span data-ttu-id="3a7a1-392">További funkciók</span><span class="sxs-lookup"><span data-stu-id="3a7a1-392">Additional features</span></span>

<span data-ttu-id="3a7a1-393">A Data Lake Tools for Visual STUDIO Code hello a következő szolgáltatásokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-393">Data Lake Tools for VS Code supports hello following features:</span></span>

-   <span data-ttu-id="3a7a1-394">Automatikus kitöltés IntelliSense: javaslatok elemek, például a kulcsszavak, a metódusok és a változók körüli előugró ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-394">IntelliSense auto-complete: Suggestions appear in pop-up windows around items, such as keywords, methods, and variables.</span></span> <span data-ttu-id="3a7a1-395">Különböző ikonjai különböző hello objektumokat:</span><span class="sxs-lookup"><span data-stu-id="3a7a1-395">Different icons represent different types of hello objects:</span></span>

    - <span data-ttu-id="3a7a1-396">Scala adattípus</span><span class="sxs-lookup"><span data-stu-id="3a7a1-396">Scala data type</span></span>
    - <span data-ttu-id="3a7a1-397">Összetett adattípusú</span><span class="sxs-lookup"><span data-stu-id="3a7a1-397">Complex data type</span></span>
    - <span data-ttu-id="3a7a1-398">Beépített UDTs</span><span class="sxs-lookup"><span data-stu-id="3a7a1-398">Built-in UDTs</span></span>
    - <span data-ttu-id="3a7a1-399">.NET adatgyűjtési és -osztályok</span><span class="sxs-lookup"><span data-stu-id="3a7a1-399">.NET collection and classes</span></span>
    - <span data-ttu-id="3a7a1-400">C# kifejezések</span><span class="sxs-lookup"><span data-stu-id="3a7a1-400">C# expressions</span></span>
    - <span data-ttu-id="3a7a1-401">Beépített C# felhasználó által megadott függvények, udo-k és UDAAGs</span><span class="sxs-lookup"><span data-stu-id="3a7a1-401">Built-in C# UDFs, UDOs, and UDAAGs</span></span> 
    - <span data-ttu-id="3a7a1-402">U-SQL-funkciók</span><span class="sxs-lookup"><span data-stu-id="3a7a1-402">U-SQL functions</span></span>
    - <span data-ttu-id="3a7a1-403">U-SQL Ablakozó függvény</span><span class="sxs-lookup"><span data-stu-id="3a7a1-403">U-SQL windowing function</span></span>
 
    ![A Data Lake Tools for Visual Studio Code IntelliSense objektumtípusok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   <span data-ttu-id="3a7a1-405">Automatikus kitöltés a Data Lake Analytics metaadatok IntelliSense: Data Lake Tools hello Data Lake Analytics metaadat-információ helyileg tölti le.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-405">IntelliSense auto-complete on Data Lake Analytics metadata: Data Lake Tools downloads hello Data Lake Analytics metadata information locally.</span></span> <span data-ttu-id="3a7a1-406">hello IntelliSense szolgáltatás automatikusan tölti fel az objektumok, például hello adatbázis, séma, tábla, nézet, táblázat értékű függvényben, eljárások és C#-szerelvények, hello Data Lake Analytics-metaadatok.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-406">hello IntelliSense feature automatically populates objects, including hello database, schema, table, view, table-valued function, procedures, and C# assemblies, from hello Data Lake Analytics metadata.</span></span>
 
    ![A Data Lake Tools for Visual Studio Code IntelliSense metaadatok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   <span data-ttu-id="3a7a1-408">IntelliSense hiba jelölő: Data Lake Tools kiemeli az U-SQL és C# hibák Szerkesztés hello.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-408">IntelliSense error marker: Data Lake Tools underlines hello editing errors for U-SQL and C#.</span></span> 
-   <span data-ttu-id="3a7a1-409">Szintaxis emeli ki: a Data Lake Tools különböző színek toodifferentiate elemeket, például a változók, kulcsszavakat, adattípus és funkciók használja.</span><span class="sxs-lookup"><span data-stu-id="3a7a1-409">Syntax highlights: Data Lake Tools uses different colors toodifferentiate items, such as variables, keywords, data type, and functions.</span></span> 

    ![A Data Lake Tools for Visual Studio Code szintaxis kiemelések](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a><span data-ttu-id="3a7a1-411">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a7a1-411">Next steps</span></span>

- <span data-ttu-id="3a7a1-412">U-SQL helyi futtatása és a Visual Studio Code helyi debug: [U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-412">For U-SQL local run and local debug with Visual Studio Code, see [U-SQL local run and local debug with Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span></span>
- <span data-ttu-id="3a7a1-413">A Data Lake Analytics-bevezető információkat lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-413">For getting-started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="3a7a1-414">A Data Lake Tools for Visual Studio információ: [oktatóanyag: fejlesztése U-SQL-parancsfájlok Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-414">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="3a7a1-415">Szerelvények fejlesztéséről további információkért lásd: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="3a7a1-415">For information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>



