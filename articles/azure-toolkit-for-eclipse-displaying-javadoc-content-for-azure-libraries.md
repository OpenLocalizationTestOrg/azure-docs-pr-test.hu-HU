---
title: "a Javához készült Azure szalagtárak csomag hello Javadoc tartalom az eclipse-ben aaaDisplaying"
description: "Hogyan toodisplay Javadoc tartalom hello hello Azure szalagtárak az eclipse-ben."
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
ms.openlocfilehash: 8070023a24dc07eca8df906db5b8b662ceed6ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-hello-azure-libraries-package-for-java"></a><span data-ttu-id="c7ef8-103">Az eclipse-ben Javadoc tartalom megjelenítés hello Azure szalagtárak csomag Java</span><span class="sxs-lookup"><span data-stu-id="c7ef8-103">Displaying Javadoc Content in Eclipse for hello Azure Libraries Package for Java</span></span>
<span data-ttu-id="c7ef8-104">hello hello Azure-könyvtárakban Java Javadoc tartalmának hello Javadoc tartalom toohello Java Azure-könyvtárakban társításával megtekinthetők az Eclipse-környezetben.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-104">hello Javadoc content for hello Azure Libraries for Java can be viewed within your Eclipse environment by associating hello Javadoc content toohello Azure Libraries for Java.</span></span> <span data-ttu-id="c7ef8-105">hello következő lépések bemutatják, hogyan toouse belül eclipse-ben ezt a funkciót.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-105">hello following steps show you how toouse this functionality within Eclipse.</span></span>

<span data-ttu-id="c7ef8-106">Ez az eljárás azt feltételezi, hogy már hozzáadott hello Azure könyvtár tooyour build Java az elérési úthoz.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-106">This procedure assumes you have already added hello Azure Library for Java tooyour build path.</span></span>

## <a name="toodisplay-javadoc-content-in-eclipse-for-hello-azure-libraries-for-java"></a><span data-ttu-id="c7ef8-107">toodisplay Javadoc tartalmának hello Azure-könyvtárakban Java az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="c7ef8-107">toodisplay Javadoc content in Eclipse for hello Azure Libraries for Java</span></span>
* <span data-ttu-id="c7ef8-108">Belül meg Eclipse Project Explorer, a hello **hivatkozott szalagtárak** szakasz a projekt, nyissa meg hello helyi menüjére hello Azure könyvtár a Java JAR.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-108">Within Eclipse's Project Explorer, in hello **Referenced Libraries** section of your project, open hello context menu for hello Azure Library for Java JAR.</span></span> <span data-ttu-id="c7ef8-109">Például **microsoft-windowsazure-api-0.1.0.jar** (hello verziószám lehet különböző, mely telepített verzió függ).</span><span class="sxs-lookup"><span data-stu-id="c7ef8-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (hello version number may be different, dependent upon which version you have installed).</span></span>

* <span data-ttu-id="c7ef8-110">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-110">Click **Properties**.</span></span>

* <span data-ttu-id="c7ef8-111">Hello belül **tulajdonságok** párbeszédpanelen hello bal oldali ablaktáblában kattintson a **Javadoc hely**.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-111">Within hello **Properties** dialog, in hello left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="c7ef8-112">Hello **Javadoc hely** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-112">hello **Javadoc Location** dialog is displayed.</span></span>

* <span data-ttu-id="c7ef8-113">Megadhat egy **Javadoc URL-cím**, vagy egy **az archívumban Javadoc**.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="c7ef8-114">Ha úgy dönt, hogy toospecify egy **Javadoc URL-cím**, használjon, mint a hello URL-címeket **http://dl.windowsazure.com/javadoc** vagy **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-114">If you choose toospecify a **Javadoc URL**, use hello URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="c7ef8-115">Ha úgy dönt, hogy toouse **az archívumban Javadoc**, megadhat egy külső fájlban, vagy a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-115">If you choose toouse **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="c7ef8-116">A választás, és tallózással keresse meg vagy ellenőrzése igény szerint.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="c7ef8-117">hello alábbi példa társítja hello Azure-könyvtárakban Java megfelelő Javadoc JAR töltött tölt le helyileg tooa mappájában hello **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-117">hello following example associates hello Azure Libraries for Java with hello corresponding Javadoc JAR that has been downloaded locally tooa folder named **c:\MyAzureJARs**.</span></span>

   ![][ic553487]

* <span data-ttu-id="c7ef8-118">*Nem kötelező lépés*: kattintson a **érvényesítése**.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-118">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="c7ef8-119">Itt Javadoc JAR hello potenciális problémákat sikerült megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-119">Potential issues with hello Javadoc JAR could be displayed here.</span></span>

* <span data-ttu-id="c7ef8-120">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-120">Click **OK**.</span></span>

<span data-ttu-id="c7ef8-121">Hello könyvtárhoz kapcsolódó, miután hello Javadoc tartalma megjelenjen-e az Eclipse IDE belül.</span><span class="sxs-lookup"><span data-stu-id="c7ef8-121">Once associated with hello library, hello Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="c7ef8-122">Például ha `blob` van definiálva típus `CloudBlockBlob` a kód hello következő az Javadoc tartalom, amely akkor jelenik meg, amikor beírja `blob.acquireLease` kódban:</span><span class="sxs-lookup"><span data-stu-id="c7ef8-122">For example, if `blob` is defined of type `CloudBlockBlob` within your code, hello following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![][ic553488]

## <a name="see-also"></a><span data-ttu-id="c7ef8-123">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c7ef8-123">See Also</span></span>
<span data-ttu-id="c7ef8-124">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="c7ef8-124">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="c7ef8-125">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="c7ef8-125">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="c7ef8-126">[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="c7ef8-126">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="c7ef8-127">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="c7ef8-127">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
