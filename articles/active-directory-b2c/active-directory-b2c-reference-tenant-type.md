---
title: "Az Azure Active Directory B2C: Régió rendelkezésre állási & adatok rezidens |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C-bérlők hello típusai"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: d382921fe9d62183b6d52c47d62f952dabd116c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Az Azure Active Directory B2C: Régió rendelkezésre állási & adatok rezidens
Régiónkénti elérhetőség és adatok rezidens két nagyon különböző fogalom alkalmazó másképp tooAzure AD B2C el hello többi Azure. Ez a cikk fogja a két módszer hello különbségei ismertetik, és hasonlítsa össze, hogyan alkalmazhatók az Azure AD B2C és tooAzure.

## <a name="summary"></a>Összefoglalás
Az Azure AD B2C jelenleg **általánosan elérhető világszerte** hello beállítással **adatok rezidens Amerikai Egyesült Államokban vagy Európa**.

## <a name="concepts"></a>Alapelvek
* **Régiónkénti elérhetőség** toowhere hivatkozik a "szolgáltatás" használható.
* **Adatok rezidens** toowhere felhasználói adat hivatkozik.

## <a name="region-availability"></a>Régiónkénti elérhetőség
Az Azure AD B2C érhető világszerte hello Azure nyilvános felhőjében keresztül. 

Ez eltér hello modell legtöbb más Azure-szolgáltatások hajtsa végre, amely az adatok rezidens gömbcsatlakozók a rendelkezésre állási. Erre példa látható, két Azure-ban [termékek elérhető területek szerint](https://azure.microsoft.com/regions/services/) lap és hello [Active Directory B2C díjszabási Számológép](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Adattárolás helye
Az Azure AD B2C az Amerikai Egyesült Államokban vagy Európa felhasználói adatait tárolja.

Adatok rezidens határozza meg, melyik ország vagy régió van kiválasztva alapján amikor [létrehozása az Azure AD B2C-bérlő](active-directory-b2c-get-started.md).

![A kép bérlő képernyőfelvétele](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Az adatok találhatók a következő országokban és régiókban hello hello Amerikai Egyesült Államokban:

> Az Amerikai Egyesült Államok, Kanada, hondurasi, Dominikai Köztársaság, El Salvador, Guatemala, mexikói, Panama, Puerto Rico és Trinidad és Tobago

Az adatok találhatók az európai országokban és régiókban következő hello a:

> Algéria, Ausztria, Azerbajdzsán, Bahreini Királyság, Fehéroroszországból, Belgium, bolgár, horvát, Ciprus, Cseh Köztársaság, Dánia, Egyiptom, észt, Finnország, Franciaország, Németország, Görögország, Magyarország, Izland, Írországban, izraeli, Olaszország, Jordánia, Kazahsztán, Kenya, kuvaiti, Lativa, Libanon, Liechtenstein, Lituania, Luxembourgban, Macedónia FYRO, Málta, Montenegró, Marokkó, holland, Nigéria, Norvégia, Omán, pakisztáni, Lengyelország, Portugália, Katar, román, orosz, Szaúd-Arábia, szerb, Szlovákia, szlovén, Dél-afrikai, Spanyolország, Svédország, Svájc, Tunézia, Törökország, ukrán, Egyesült Arab emírségekbeli és Egyesült Királyság.

hello fennmaradó országokban és régiókban hello folyamata toohello lista hozzáadott szerepelnek.  Most, továbbra is használhatja az Azure AD B2C ki bármelyik hello országokban és régiókban fenti.

> Afganisztáni, argentin, Ausztrália, Brazília, chilei, Kolumbia, Ecuadori, Hongkong KKT, India, indonéziai, Irak, japán, koreai, malajziai, Új-Zéland, paraguayi, Perui, Fülöp-szigeteki, szingapúri, Srí Lanka, Tajvan, Thaiföld, Uruguayi és Bolivári.

## <a name="preview-tenant"></a>Előzetes bérlői
A B2C-bérlő során az Azure AD B2C előzetes verzió idejének hozna létre, ha valószínű, amely a **típus bérlői** szerint **Preview bérlői**. Ha ez helyzet hello, csak a fejlesztési és tesztelési célra, és nem éles alkalmazások esetén a bérlő kell használnia.

> [!IMPORTANT]
> Nincs a B2C bérlő tooa-éles környezetben való méretezésre B2C-bérlő Preview áttelepítési útvonal. Vegye figyelembe, hogy vannak ismert problémák a B2C előzetes bérlő törlésekor, és hozza létre újból egy éles környezetben való méretezésre B2C-bérlőben hello ugyanazon a néven. Toocreate rendelkezik egy éles környezetben való méretezésre B2C-bérlő egy másik tartomány nevét.


![A kép bérlő képernyőfelvétele](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
