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
# <a name="displaying-javadoc-content-in-eclipse-for-hello-azure-libraries-package-for-java"></a>Az eclipse-ben Javadoc tartalom megjelenítés hello Azure szalagtárak csomag Java
hello hello Azure-könyvtárakban Java Javadoc tartalmának hello Javadoc tartalom toohello Java Azure-könyvtárakban társításával megtekinthetők az Eclipse-környezetben. hello következő lépések bemutatják, hogyan toouse belül eclipse-ben ezt a funkciót.

Ez az eljárás azt feltételezi, hogy már hozzáadott hello Azure könyvtár tooyour build Java az elérési úthoz.

## <a name="toodisplay-javadoc-content-in-eclipse-for-hello-azure-libraries-for-java"></a>toodisplay Javadoc tartalmának hello Azure-könyvtárakban Java az eclipse-ben
* Belül meg Eclipse Project Explorer, a hello **hivatkozott szalagtárak** szakasz a projekt, nyissa meg hello helyi menüjére hello Azure könyvtár a Java JAR. Például **microsoft-windowsazure-api-0.1.0.jar** (hello verziószám lehet különböző, mely telepített verzió függ).

* Kattintson a **Tulajdonságok** elemre.

* Hello belül **tulajdonságok** párbeszédpanelen hello bal oldali ablaktáblában kattintson a **Javadoc hely**. Hello **Javadoc hely** párbeszédpanel jelenik meg.

* Megadhat egy **Javadoc URL-cím**, vagy egy **az archívumban Javadoc**.

   * Ha úgy dönt, hogy toospecify egy **Javadoc URL-cím**, használjon, mint a hello URL-címeket **http://dl.windowsazure.com/javadoc** vagy **http://dl.windowsazure.com/storage/javadoc**.

   * Ha úgy dönt, hogy toouse **az archívumban Javadoc**, megadhat egy külső fájlban, vagy a munkaterületen.

   A választás, és tallózással keresse meg vagy ellenőrzése igény szerint. hello alábbi példa társítja hello Azure-könyvtárakban Java megfelelő Javadoc JAR töltött tölt le helyileg tooa mappájában hello **c:\MyAzureJARs**.

   ![][ic553487]

* *Nem kötelező lépés*: kattintson a **érvényesítése**. Itt Javadoc JAR hello potenciális problémákat sikerült megjeleníteni.

* Kattintson az **OK** gombra.

Hello könyvtárhoz kapcsolódó, miután hello Javadoc tartalma megjelenjen-e az Eclipse IDE belül. Például ha `blob` van definiálva típus `CloudBlockBlob` a kód hello következő az Javadoc tartalom, amely akkor jelenik meg, amikor beírja `blob.acquireLease` kódban:

![][ic553488]

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->
