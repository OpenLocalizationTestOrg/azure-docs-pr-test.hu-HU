---
title: "aaaHow toocreate, kezelése vagy törlése a klasszikus Azure portálon hello |} Microsoft Docs"
description: "Hozzon létre egy új tárfiókot, a tárelérési kulcsok kezelése vagy törlése a hello Azure-portálon. Ismerje meg a standard és a prémium szintű tárfiókokat."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 5e4f4360-3f81-4d63-a0b1-e7771b67af11
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: 6ee2359e02c7c9e9c111e1fc87a6160bb8b785b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-storage-accounts"></a>Tudnivalók az Azure Storage-fiókokról
[!INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Áttekintés
Egy Azure storage biztosít hozzáférést az Azure Storage Azure Blob, a várólista, a tábla és a fájl szolgáltatások toohello. A tárfiók biztosítja az Azure Storage-adatobjektumok hello egyedi névteret. Alapértelmezés szerint hello a fiók adatai elérhető csak tooyou, hello fiók tulajdonosának.

Kétféle tárfióktípus létezik:

* A standard szintű tárfiók a Blob, Table, Queue és a File tárolási szolgáltatást foglalja magában.
* A prémium szintű tárfiók jelenleg csak az Azure virtuális gépek lemezeit támogatja. A Premium Storage részletesebb áttekintéséért lásd: [Premium Storage: High-performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md) (Premium Storage: Nagy teljesítményű tárterület az Azure virtuális gépek számítási feladataihoz).

## <a name="storage-account-billing"></a>Tárfiókok számlázása
Az Azure Storage használatáért a tárfiók alapján kell fizetni. A tárolási költségeket négy tényező határozza meg: a tárolási kapacitás, a replikálási séma, a tárolási tranzakciók és a kimenő adatforgalom.

* Tárolási kapacitás azt toohow sokkal tárfiókja, toostore adatokat használ. az adatok tárolásának költségét hello határozza meg, hogy mennyi adatot tárolja, és azok replikálása hogyan történik.
* A replikálás határozza meg, hogy adatait egyszerre hány példányban és hol tárolja.
* Tranzakciók tekintse meg a tooall olvasási és írási műveletek tooAzure tárolási.
* Kimenő adatforgalom egy Azure-régiót kivitt toodata hivatkozik. Ha a tárfiókban lévő hello adatokhoz férhetnek hozzá a nem futó alkalmazás hello ugyanabban a régióban, hogy, hogy az alkalmazás egy felhőalapú szolgáltatás, vagy egyéb típusú alkalmazásról, majd van szó, a kimenő adatforgalom. (Az Azure szolgáltatások esetében létrehozhat lépéseket toogroup adatait és szolgáltatásait hello ugyanazokat az adatokat tooreduce adatközpontok vagy kiküszöbölheti a kimenő adatforgalom díját.)  

Hello [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage) lap a tárolási kapacitás, a replikáció és a tranzakciókért részletes információkat biztosít. Hello [adatforgalmi díjszabás](https://azure.microsoft.com/pricing/details/data-transfers/) lap adatforgalommal kapcsolatos részletes információkat biztosít.

A tárfiókok kapacitási és teljesítménycéljaival kapcsolatos további információk: [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) (Az Azure Storage méretezhetőségi és teljesítménycéljai).

> [!NOTE]
> Azure virtuális gép létrehozásakor a storage-fiók automatikusan létrejön, meg hello telepítési helyen Ha még nem rendelkezik egy tárfiókot az adott helyre. Így nem szükséges toofollow hello lépéseket alatti toocreate egy tárfiók a virtuális gépek lemezeit. a tárfiók neve hello hello virtuális gép neve alapul. Lásd: hello [Azure Virtual Machines – dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machines/) további részleteket.
> 
> 

## <a name="create-a-storage-account"></a>Create a storage account
1. Jelentkezzen be toohello [klasszikus Azure portál](https://manage.windowsazure.com).
2. Kattintson a **új** alján hello hello hello tálcán. Válassza a **Data Services** | **Tárolás** lehetőséget, majd kattintson a **Gyorslétrehozás** elemre.
   
    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)
3. Az **URL-cím** mezőben adjon meg egy nevet a tárfiók számára.
   
   > [!NOTE]
   > A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.
   > 
   > A tárfiók nevének egyedinek kell lennie az Azure rendszerben. hello klasszikus Azure portál jelzi, ha választja hello tárfióknév már használatban van.
   > 
   > 
   
    Lásd: [tárfiókvégpontok](#storage-account-endpoints) alatt a vonatkozó részletek hogyan hello tárfióknév használt tooaddress lesz-e az Azure Storage az objektumok.
4. A **hely/Affinitáscsoport**, jelöljön ki egy helyet a tárfiók, amely Bezárás tooyou vagy tooyour ügyfelek. Ha a tárfiókban lévő adatokat fogják elérni egy másik Azure szolgáltatás, például egy Azure virtuális gép vagy felhőszolgáltatás, érdemes lehet tooselect hello lista toogroup az affinitáscsoport a storage-fiókot hello ugyanabban az adatközpontban más Azure-szolgáltatásokkal használt tooimprove teljesítmény és az alacsonyabb költségek.
   
    Ügyeljen arra, hogy az affinitáscsoportot a tárfiók létrehozásakor kell kiválasztania. Egy meglévő fiókot tooan affinitáscsoport nem helyezhető át. Az affinitáscsoportokkal kapcsolatos további információkért lásd az alábbiakban a [Szolgáltatások közös elhelyezése affinitáscsoporttal](#service-co-location-with-an-affinity-group) című szakaszt.
   
   > [!IMPORTANT]
   > melyik helyek elérhetőek az előfizetéshez tartozó toodetermine, hívása hello [listában az összes erőforrás-szolgáltató](https://msdn.microsoft.com/library/azure/dn790524.aspx) műveletet. toolist szolgáltatók a Powershellből hívás [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). A .NET-keretrendszerből használja a hello [lista](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) hello ProviderOperationsExtensions osztály metódusában.
   > 
   > Továbbá tekintse meg [Az Azure régiói](https://azure.microsoft.com/regions/#services) című lapot azzal kapcsolatban, hogy melyik régióban mely szolgáltatások érhetőek el.
   > 
   > 
5. Ha több Azure-előfizetéssel rendelkezik, majd hello **előfizetés** mezőben jelenik meg. A **előfizetés**, adja meg a hello Azure-előfizetést, amelyet toouse hello tárfiók.
6. A **replikációs**, válassza ki a tárfiók replikálásának kívánt szintjét hello. hello ajánlott Replikációs beállítás georedundáns replikáció, amely maximális tartósságot biztosít az adatok. Az Azure Storage replikálási beállításaival kapcsolatos további részletek: [Azure Storage replication](storage-redundancy.md) (Az Azure Storage replikációja).
7. Kattintson a **Tárfiók létrehozása** lehetőségre.
   
    Eltarthat néhány percig toocreate a tárfiók. toocheck hello állapotát, a figyelheti hello értesítéseket a klasszikus Azure portál hello hello alján. Hello storage-fiók létrehozása után az új tárolási fiók rendelkezik-e **Online** állapotát és használatra kész.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)

### <a name="storage-account-endpoints"></a>Tárfiókvégpontok
Az Azure Storage szolgáltatásban tárolt minden egyes objektum egyedi URL-címmel rendelkezik. hello tárolási fiók neve űrlapok hello cím altartományát. hello altartomány és a tartomány nevét, amely egy adott tooeach szolgáltatás, kombinációja képezi egy *végpont* a tárfiók.

Például, ha a tárfiók neve *mystorageaccount*, akkor a tárfiók hello alapértelmezett végpontok a következők:

* Blob szolgáltatás: http://*mystorageaccount*.blob.core.windows.net
* Table Service: http://*mystorageaccount*.table.core.windows.net
* Queue szolgáltatás: http://*mystorageaccount*.queue.core.windows.net
* File szolgáltatás: http://*mystorageaccount*.file.core.windows.net

A tárfiók a tárterület irányítópultján hello hello hello végpontok látható [klasszikus Azure portál](https://manage.windowsazure.com) hello fiók létrehozása után.

a tárfiók eléréséhez szükséges hello URL hozzáfűzésével épül fel hello tárolási fiók toohello végpont hello objektum helyét. Például egy blobcím formátuma lehet a következő: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

A tárfiók egy egyéni tartomány nevét toouse is konfigurálható. További részletek: [Configure a custom domain name for your blob storage endpoint](storage-custom-domain-name.md) (Egyéni tartományév konfigurálása a Blob Storage-végponthoz).

### <a name="service-co-location-with-an-affinity-group"></a>Szolgáltatások közös elhelyezése affinitáscsoporttal
Az *affinitáscsoport* az Azure szolgáltatásainak és virtuális gépeinek földrajzi csoportosítása Azure tárfiókjával. Az affinitáscsoport javíthatja a szolgáltatás teljesítményének ha a számítási feladatok a hello ugyanazokat az adatokat center vagy hello célközönség közelébe közelében. Is, adatforgalmi díjak sem merülnek fel a kimenő forgalom a tárfiókban lévő adatokat egy olyan másik szolgáltatás, amely része hello elérésekor ugyanabban az affinitáscsoportban.

> [!NOTE]
> toocreate affinitáscsoporthoz, nyissa meg hello <b>beállítások</b> hello területe [klasszikus Azure portál](https://manage.windowsazure.com), kattintson a <b>Affinitáscsoportok</b>, ezután kattintson <b>hozzáadása egy affinitáscsoport</b> vagy hello <b>Hozzáadás</b> gombra. Is létrehozása és kezelése az affinitáscsoportok hello Azure szolgáltatásfelügyeleti API használatával. További információk: <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Operations on affinity groups</a> (Műveletek affinitáscsoportokkal).
> 
> 

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Tárelérési kulcsok megtekintése, másolása és újragenerálása
Amikor létrehoz egy tárfiókot, az Azure létrehoz két 512 bites tárelérési kulcsot, a hitelesítéshez használt hello tárfiók elérésekor. Két tárelérési kulcsot biztosít, Azure lehetővé teszi tooregenerate hello kulcsok megszakítás tooyour társzolgáltatás és toothat szolgáltatás nélkül.

> [!NOTE]
> Javasoljuk, hogy a tárelérési kulcsokat ne ossza meg senkivel. toopermit toostorage erőforrások eléréséhez a tárelérési kulcsok kiadása nélkül, használhatja a *közös hozzáférésű jogosultságkódot*. A közös hozzáférésű jogosultságkód hozzáférést tooa erőforrás fiókjában biztosít, Ön által meghatározott időtartamra és hello jogosultságokkal rendelkező. További információk: [Using Shared Access Signatures (SAS) (Közös hozzáférésű jogosultságkód (SAS) használata)](storage-dotnet-shared-access-signature-part-1.md)
> 
> 

A hello [klasszikus Azure portál](https://manage.windowsazure.com), használjon **kulcsok kezelése** hello irányítópult vagy hello **tárolási** tooview, másolása és újragenerálása hello tárelérési kulcsok használt lapon tooaccess hello Blob, Table és Queue szolgáltatások.

### <a name="copy-a-storage-access-key"></a>Tárelérési kulcs másolása
Használhat **kulcsok kezelése** toocopy egy tárolási hozzáférési kulcs toouse a kapcsolati karakterláncban. hello kapcsolati karakterlánc hello a tárfiók neve vagy a kulcs toouse hitelesítést igényel. Kapcsolat konfigurálásával kapcsolatos információkat az Azure storage szolgáltatások tooaccess karakterláncok, lásd: [konfigurálása Azure Storage kapcsolati karakterláncok](storage-configure-connection-string.md).

1. A hello [klasszikus Azure portál](https://manage.windowsazure.com), kattintson a **tárolási**, és kattintson a hello tárolási fiók tooopen hello irányítópult hello nevét.
2. Kattintson a **Kulcsok kezelése** elemre.
   
     Megnyílik az **Elérési kulcsok kezelése** felület.
   
    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)
3. toocopy a tárelérési kulcs, jelölje be hello kulcs szövegét. Ezután kattintson jobb gombbal, majd kattintson a **Másolás** lehetőségre.

### <a name="regenerate-storage-access-keys"></a>Tárelérési kulcsok újragenerálása
Azt javasoljuk, hogy módosítsa a hello hívóbetűk tooyour tárolási fiók rendszeresen toohelp garantálják a tárolási kapcsolatok biztonságát. Két tárelérési kulcsok vannak hozzárendelve, így akkor is fenntartható a kapcsolatok toohello tárfiók hello újragenerálja egyik kulcs segítségével más hozzáférési kulcsot.

> [!WARNING]
> A tárelérési kulcsok újragenerálása hatással lehet az Azure, valamint a saját alkalmazásai hello tárfiók függő szolgáltatások. Hello hozzáférési kulcs tooaccess hello tárfiókot használó összes ügyfelet frissített toouse hello új kulcsot kell lennie.
> 
> 

**A Media services** -Ha a tárfiók függő media services, újra kell szinkronizálnia hello hívóbetűk a médiaszolgáltatással után hello kulcsok újragenerálása.

**Alkalmazások** – Ha a webes alkalmazások cloud services – hello tárolási fiók használatát, vagy megszakad a hello kapcsolatok Ha újragenerálja a kulcsokat, kivéve, ha rotálja a kulcsokat. 

**Tártallózók** – Ha bármilyen [Tártallózó alkalmazást](storage-explorers.md), valószínűleg szüksége lesz az alkalmazások által használt kulcs tooupdate hello.

Hello folyamat végrehajtásával rotálhatóak a tárelérési kulcsok itt található:

1. Hello kapcsolati karakterláncokat az alkalmazás kódja tooreference hello másodlagos elérési kulcsát hello storage-fiókjának frissítése.
2. A tárfiók hello elsődleges elérési kulcs újragenerálása. A hello [klasszikus Azure portál](https://manage.windowsazure.com), hello irányítópult vagy hello **konfigurálása** kattintson **kulcsok kezelése**. Kattintson a **újragenerálja** hello elsődleges elérési kulcsát, és kattintson a **Igen** , amelyet az új kulcs toogenerate tooconfirm.
3. Hello kapcsolati karakterláncok a kód tooreference hello új elsődleges elérési kulcs frissítése.
4. Hello másodlagos elérési kulcs újragenerálása.

## <a name="delete-a-storage-account"></a>Tárfiók törlése
a storage-fiók, amely már nem használ, használjon tooremove **törlése** hello irányítópult vagy hello **konfigurálása** lap. **Törlés** törlések hello teljes tárfiókot, beleértve az összes hello blobot, táblát és üzenetsort hello fiók.

> [!WARNING]
> Nem lehetséges toorestore törölt tárfiókokat, vagy a törlés előtt abban tárolt hello tartalmakat bármelyikét beolvasása. Bármi is meg arról, hogy tooback kívánt toosave hello fiók törlése előtt. Ez is igaznak minden olyan erőforrásnál hello fiók – Ha töröl egy blob, table, várólista vagy fájlt, az véglegesen törlődni fog.
> 
> Ha a tárfiók VHD-fájlokat egy Azure virtuális géphez, majd törölnie kell összes lemezképet és lemezt, amely hello tárfiók törlése előtt ezeket a VHD-fájlokat használja. Először állítsa le hello virtuális gépet, ha fut, és törölje azt. toodelete lemezek, keresse meg a toohello **lemezek** lapra, és töröljön minden ott található lemezt. toodelete képek, keresse meg a toohello **képek** lapra, és törölje az összes, hello fiókban tárolt lemezképet.
> 
> 

1. A hello [klasszikus Azure portál](https://manage.windowsazure.com), kattintson a **tárolási**.
2. Kattintson bárhova hello tárfiókbejegyzésben hello nevét, és kattintson a **törlése**.
   
     – Vagy –
   
    Kattintson a hello tárolási fiók tooopen hello irányítópult hello nevét, majd **törlése**.
3. Kattintson a **Igen** tooconfirm, amelyet az toodelete hello tárfiók.

## <a name="next-steps"></a>Következő lépések
* További információ az Azure Storage toolearn lásd: hello [Azure Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).
* A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/).
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)

