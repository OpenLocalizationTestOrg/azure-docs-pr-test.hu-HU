---
title: "Azure-előfizetés tulajdonjogának átruházása másik fiókra |} Microsoft Docs"
description: "Ismerteti, hogyan lehet egy Azure-előfizetés átvitele egy másik felhasználó, és a folyamat kapcsolatos néhány gyakran ismételt kérdések (GYIK)"
keywords: "azure-előfizetésre, az azure átviteli előfizetés átvitele, azure-előfizetés áthelyezése egy másik fiókot, azure új előfizetés tulajdonos, azure-előfizetés átvitele egy másik fiókkal"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8a856c39eb11546f35cb4e8c21e6bdcce98857a8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Egy másik fiókot az Azure-előfizetés tulajdonjogának átruházása

Használatalapú fizetés, Visual Studio, a művelet, vagy az Account Center BizSpark előfizetések vihetők át az előfizetés egy másik felhasználóhoz. Azure-előfizetés típusaival kapcsolatban, valamint a külső szolgáltatások átruházása támogatjuk. 

Azure-előfizetés tulajdonjogának átruházása, ha a kívánt meg:

* Számlázási tulajdonjogát, hogy valaki más Azure-előfizetése felett oldalon kell.
* Regisztráció az Azure használt fiók módosítani szeretne. Lehet, hogy a Microsoft-Account használja, de azt jelentette, hogy használja a munkahelyi vagy iskolai fiókját.
* Az Azure-előfizetéshez áthelyezése egy címtárban másik szeretné.
* Azure és az Office 365 a különböző bérlők, és szeretnék vonják össze.

Az előfizetés egy másik ajánlatra módosításához lásd [az Azure-előfizetéshez Váltás másik ajánlatra](billing-how-to-switch-azure-offer.md). 

## <a name="transfer-ownership-of-an-azure-subscription"></a>Azure-előfizetés tulajdonjogának átruházása
> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Jelentkezzen be a(z) <https://account.windowsazure.com/Subscriptions>. A fiók rendszergazdájához, és egy tulajdonosi átvitelt végezni, hogy rendelkezik. Ki-e a Fiókadminisztrátor az előfizetés, lásd: a [gyakran ismételt kérdések](#faq).

2. Válassza ki az előfizetés átvitele.

3. Kattintson a **előfizetés átviteli** lehetőséget. Lásd: [gyakran ismételt kérdések](#no-button) Ha nem látja a gombra.

   ![Azure-fiók előfizetések lap](./media/billing-subscription-transfer/image1.png)
4. Adja meg a címzettet.

   ![Átviteli előfizetés párbeszédpanel](./media/billing-subscription-transfer/image2.PNG)
5. A címzett automatikusan kapni fog egy elfogadási hivatkozást tartalmazó e-mailt.

   ![Előfizetés átviteli e-mail címzett](./media/billing-subscription-transfer/image3.png)
6. A címzett a hivatkozásra való kattintás után követnie kell az utasításokat, többek között a fizetési információk megadásával.

   ![Első előfizetési átviteli weblapot](./media/billing-subscription-transfer/image4.png)

   ![Második előfizetési átviteli weblapot](./media/billing-subscription-transfer/image5.png)
7. Sikerült! Az Előfizetés most át.

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>A nagyvállalati szerződés (EA) ügyfelek előfizetés tulajdonjogának átruházása
A vállalati rendszergazda vihetők át a beléptetési belül előfizetések tulajdonjogát. Első lépésként tekintse meg a [átviteli fiók tulajdonjogának](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) a EA portálon.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Az előfizetés tulajdonosa elfogadása követő lépések
1. A Fiókadminisztrátor is. Tekintse át, és frissítse a szolgáltatás-rendszergazda és a Társadminisztrátorok. A rendszergazdák kezelése a [a klasszikus Azure portálon](https://manage.windowsazure.com) beállításainak megnyitásával. [További tudnivalók a rendszergazdai szerepkörökről](billing-add-change-azure-subscription-administrator.md).

2. Szerepköralapú hozzáférés-vezérlést (RBAC) használata az előfizetés és -szolgáltatásokra. Látogasson el az [Azure Portalra](https://portal.azure.com). [További tudnivalók az RBAC](../active-directory/role-based-access-control-configure.md)

3. Frissítse a hitelesítő adatokat, beleértve az előfizetés szolgáltatásokkal:
   
   * A felhasználó előfizetéshez kapcsolódó erőforrásokat rendszergazdai jogosultságokat adjon felügyeleti tanúsítványok. További információkért lásd: [létrehozása és felügyeleti az Azure-tanúsítvány feltöltése](../cloud-services/cloud-services-certs-create.md)
   
   * Tárelérési kulcsok szolgáltatások, például tároló. További információkért lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md)
   
   * Távoli hozzáférési hitelesítő adatok, szolgáltatások, például Azure virtuális gépeken. 

4. [Az előfizetés számlázási riasztások frissítése](billing-set-up-alerts.md) , a [Azure Account Center](https://account.windowsazure.com/Subscriptions). 

5. Ha a partnerrel dolgozik, fontolja meg a Partnerazonosítót. Ebben az előfizetésben. A partner Partnerazonosítóját, a frissítéshez a [Azure Account Center](https://account.windowsazure.com/Subscriptions).

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Gyakori kérdések (GYIK)
* <a name="whoisaa"></a>**Ki-e a fiókadminisztrátor az előfizetés?**

  A fiókadminisztrátor az a személy, aki regisztrált a vagy vásárolt Azure-előfizetést. Engedélyezett-e hozzáférni a [Account Center](https://account.windowsazure.com/Home/Index) és -előfizetések létrehozása, szakítsa meg a előfizetések, az előfizetés számlázási módosítása, vagy módosítsa a szolgáltatás-rendszergazda például különböző felügyeleti feladatok elvégzésére. Ha nem biztos, aki a fiókadminisztrátor az előfizetéshez, tegye a következőket megállapítása.

  1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
  2. A központ menüben válassza ki a **előfizetés**.
  3. Válassza ki az előfizetést, és ellenőrizze, majd keresse meg a **beállítások**.
  4. Válassza ki **tulajdonságok**. A fiókadminisztrátor az előfizetés jelenik meg a **Fiókadminisztrátor** mezőbe.  

* **Nem minden átviszi a? Többek között az erőforráscsoportok, a virtuális gépek, a lemezek és a többi futó szolgáltatások?**

  Igen, az erőforrások, mint például virtuális gépek, a lemezek és a webhelyek átvitelét az új tulajdonos. Azonban bármely [rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md) és [szerepköralapú hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-configure.md) létrehozott házirendeket nem veszi át más címtárak között.

* <a id="no-button"></a>**Miért nem látom az előfizetés átvitele gomb?**

  Ha nem látja a **átviteli előfizetés** gombra, majd az előfizetéshez nem támogatjuk az előfizetés átvitele. [Forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

* **Az előfizetés átadása szolgáltatás leállási eredményez?**

  Nincs hatással a szolgáltatás. Az előfizetés átvitele visszavonja az előfizetés aktuális rendszergazdai fiók alatt, és a címzett fiók alatt egy előfizetést hoz létre. Az új előfizetés az alapul szolgáló Azure-szolgáltatásokhoz való hozzá rendelve. Az előfizetés-azonosító változatlan marad.

* **Hogyan használjam ezt a folyamatot előfizetéshez tartozó könyvtárat szeretne váltani?**

  Azure-előfizetéssel, amely a Fiókadminisztrátor tartozik a címtárban jön létre. Ha könyvtárat szeretne váltani, vigye át az előfizetést a célkönyvtárat egy felhasználói fiókhoz. Átadás elfogadásához a lépéseket, hogy a felhasználó befejeződésekor az előfizetés automatikusan átkerül a célkönyvtárat.

* **Ha I átveszi egy másik szervezet előfizetés számlázási tulajdonosi jogokat, tegye azok továbbra is az erőforrások eléréséhez?**

  Ha az előfizetés át egy másik bérlőnek, az előző tenanthoz társított felhasználók elveszíti a hozzáférést az előfizetéshez. Akkor is, ha a felhasználó nem egy szolgáltatás-rendszergazdai vagy társadminisztrátori többé, előfordulhat, hogy továbbra is hozzáférhetnek az előfizetéseket a más védelmi mechanizmus, beleértve:

  * A felhasználó előfizetéshez kapcsolódó erőforrásokat rendszergazdai jogosultságokat adjon felügyeleti tanúsítványok. További információkért lásd: [létrehozása és feltöltése az Azure felügyeleti tanúsítvánnyal](../cloud-services/cloud-services-certs-create.md).
  * Tárelérési kulcsok szolgáltatások, például tároló. További információkért lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).
  * Távoli hozzáférési hitelesítő adatok, szolgáltatások, például Azure virtuális gépeken.

 Ha a címzett kell korlátozni az erőforrásokhoz való hozzáférést, akkor vegye figyelembe bármely a szolgáltatáshoz társított titkos kulcsok frissítése. A legtöbb erőforrást frissíthetik az alábbi lépéseket követve:

    1. Nyissa meg az [Azure Portal](https://portal.azure.com).
    2. A központ menüben válassza ki a **összes erőforrás**.
    3. Válassza ki az erőforrást. 
    4. Az erőforrás panel kattintson **beállítások**. Itt megtekintheti, és frissítse a meglévő titkos kulcsok.

* **Ha az előfizetés számlázási ciklus közepén átvinni, nem a címzett fizetési – a teljes számlázási ciklus?**

  A küldő bármilyen, hogy befejeződött-e az átvitel pontig jelentett használati felelős. A címzett nem az újabb verziók esetében az átviteli időt jelentett használati felelős. Előfordulhat, hogy néhány használati átvitel előtt történt, de ezek után történt. A használati a címzett számlázási tartalmazza.

* **A címzett rendelkezik használati és számlázási előzmények elérésére?**

  A címzett számára elérhető összes információ mennyisége az utolsó számlázási, vagy ha az előfizetés áthelyezése történt, az első számlázási előtt létrehozott, az aktuális egyenleg. A többi a használati és számlázási előzmények nem viszi át az előfizetéshez.

* **Az ajánlat módosítható egy átvitel közben?**

  Az ajánlat azonosnak kell maradnia. Ha módosítani szeretné az ajánlatot, lásd: [az Azure-előfizetéshez Váltás másik ajánlatra](billing-how-to-switch-azure-offer.md).

* **Átvihető előfizetés egy másik országban egy felhasználói fiókot?**

  Egy előfizetés átvitele egy felhasználói fiókhoz, egy másik országban nem, nem támogatott. A címzett felhasználói fióknak ugyanabban az országban kell lennie.

* **A címzett használhatja egy másik fizetési módot?**

  Igen. De az előfizetés számlázási előzmények be van-e osztani két fiókot.  

* **A fizetési módot feladatátvétele után az Azure-előfizetés átvitele?**

  Fogadja el az előfizetés átadása, hitelkártyával vagy hasonló fizetési metódust meg kell adni, az előfizetés fizetési. Például ha Bálint Jane előfizetés továbbítja, és Jane fogadja el az átvitelt, Jane biztosítania kell a fizetési módot, az előfizetés fizetési. Az átvitel befejezése után, Jane az előfizetés nem Bob lesz számlázva.

* **Hogyan adatait és szolgáltatásait saját Azure-előfizetés át az új előfizetés?**

  Ha az előfizetés tulajdonosa nem tudja áthelyezni, manuálisan áttelepítheti az erőforrások. Lásd: [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](../azure-resource-manager/resource-group-move-resources.md).



## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez. 


