---
title: "Javadoc tartalom megjelenítése az eclipse-ben a Javához készült Azure szalagtárak csomag"
description: "Az eclipse-ben az Azure-tárak Javadoc tartalom megjelenítési módját."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="f8b80-103">Javadoc tartalom megjelenítése az eclipse-ben a Javához készült Azure szalagtárak csomag</span><span class="sxs-lookup"><span data-stu-id="f8b80-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>
<span data-ttu-id="f8b80-104">Az Azure-könyvtárakban Java Javadoc tartalmának tekintheti meg az Eclipse-környezetben a Javadoc tartalmat, hogy az Azure-könyvtárakban Java társításával.</span><span class="sxs-lookup"><span data-stu-id="f8b80-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="f8b80-105">A következő lépések bemutatják a Ez a funkció Eclipse belül.</span><span class="sxs-lookup"><span data-stu-id="f8b80-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="f8b80-106">Ez az eljárás azt feltételezi, hogy már hozzáadta a Javához készült Azure könyvtárban a build elérési útját.</span><span class="sxs-lookup"><span data-stu-id="f8b80-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="f8b80-107">A megjelenítendő Javadoc tartalom az eclipse-ben a Javához készült Azure-tárak</span><span class="sxs-lookup"><span data-stu-id="f8b80-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>
* <span data-ttu-id="f8b80-108">Belül meg Eclipse Project Explorer az a **hivatkozott szalagtárak** szakasz a projekt, nyissa meg a helyi menü az Azure-könyvtárhoz a Java JAR.</span><span class="sxs-lookup"><span data-stu-id="f8b80-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="f8b80-109">Például **microsoft-windowsazure-api-0.1.0.jar** (a verziószám lehet különböző, mely telepített verzió függ).</span><span class="sxs-lookup"><span data-stu-id="f8b80-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

* <span data-ttu-id="f8b80-110">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="f8b80-110">Click **Properties**.</span></span>

* <span data-ttu-id="f8b80-111">Belül a **tulajdonságok** párbeszédpanelen, a bal oldali ablaktáblán kattintson a **Javadoc hely**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="f8b80-112">A **Javadoc hely** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f8b80-112">The **Javadoc Location** dialog is displayed.</span></span>

* <span data-ttu-id="f8b80-113">Megadhat egy **Javadoc URL-cím**, vagy egy **az archívumban Javadoc**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="f8b80-114">Ha úgy dönt, hogy adjon meg egy **Javadoc URL-cím**, használjon, mint az URL-címeket **http://dl.windowsazure.com/javadoc** vagy **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="f8b80-115">Ha úgy dönt, hogy használjon **Javadoc az archívumban**, megadhat egy külső fájlban, vagy a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="f8b80-116">A választás, és tallózással keresse meg vagy ellenőrzése igény szerint.</span><span class="sxs-lookup"><span data-stu-id="f8b80-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="f8b80-117">A következő példa az Azure-könyvtárakban Java társítja a megfelelő Javadoc JAR helyileg letöltött nevű **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![][ic553487]

* <span data-ttu-id="f8b80-118">*Nem kötelező lépés*: kattintson a **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-118">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="f8b80-119">Itt a Javadoc JAR potenciális problémákat sikerült megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="f8b80-119">Potential issues with the Javadoc JAR could be displayed here.</span></span>

* <span data-ttu-id="f8b80-120">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f8b80-120">Click **OK**.</span></span>

<span data-ttu-id="f8b80-121">Miután a könyvtárhoz kapcsolódó, Javadoc tartalma megjelenjen-e az Eclipse IDE belül.</span><span class="sxs-lookup"><span data-stu-id="f8b80-121">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="f8b80-122">Például ha `blob` van definiálva típus `CloudBlockBlob` a kód a következő az Javadoc tartalom, amely akkor jelenik meg, amikor beírja `blob.acquireLease` kódban:</span><span class="sxs-lookup"><span data-stu-id="f8b80-122">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![][ic553488]

## <a name="see-also"></a><span data-ttu-id="f8b80-123">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f8b80-123">See Also</span></span>
<span data-ttu-id="f8b80-124">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f8b80-124">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="f8b80-125">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f8b80-125">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="f8b80-126">[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f8b80-126">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="f8b80-127">Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="f8b80-127">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
