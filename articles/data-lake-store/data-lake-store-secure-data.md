---
title: "Azure Data Lake Store-ban tárolt aaaSecuring adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan szabályozza a toosecure adatokat az Azure Data Lake Store csoportok és a hozzáférés a listák"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 2b4ed7e322e1843ca47d6968ec8801ac19ea6399
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure Data Lake Store-ban tárolt adatok védelme
Adatok védelme az Azure Data Lake Store egy három lépéses megközelítés.

1. Először hozzon létre biztonsági csoportokat az Azure Active Directory (AAD). A biztonsági csoportok használhatók tooimplement szerepköralapú hozzáférés-vezérlés (RBAC) az Azure portálon. További információ: [szerepköralapú hozzáférés-vezérlés a Microsoft Azure-ban](../active-directory/role-based-access-control-configure.md).
2. Rendelje hozzá a hello AAD biztonsági csoportok toohello Azure Data Lake Store-fiókba. Ez meghatározza, hogy a hálózatelérési toohello Data Lake Store fiókjával hello portal és felügyeleti műveletek hello portál és API-k.
3. Hozzáférés-vezérlési lista (ACL) a Data Lake Store-fájlrendszer hello rendeljen hello AAD biztonsági csoportokat.
4. Az ügyfelek által elérhető hello adatok a Data Lake Store továbbá az IP-címtartományok is beállíthat.

Ez a cikk útmutatás hogyan toouse hello Azure portál tooperform hello feladatok felett. Hogyan valósítja meg a Data Lake Store hello fiókot és az adatok szintű biztonsági részletes információkért lásd: [az Azure Data Lake Store biztonsági](data-lake-store-security-overview.md). A hozzáférés-vezérlési listákat az Azure Data Lake Store megvalósított hogyan részletesen információkért lásd: [áttekintése a hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Az Azure Active Directory biztonsági csoportok létrehozása
Útmutatást toocreate AAD biztonsági csoportok és hogyan tooadd felhasználók toohello csoport, lásd: [az Azure Active Directory biztonsági csoportok kezelése](../active-directory/active-directory-accessmanagement-manage-groups.md).

> [!NOTE] 
> Felhasználók és csoportok tooa olyan másik csoportnak is adhat hozzá az Azure AD-hello Azure-portál használatával. Azonban a rendezés tooadd egy egyszerű tooa szolgáltatáscsoport, használja a [az Azure AD PowerShell modul](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get hello desired group and service principal and identify hello correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add hello service principal toohello group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-tooazure-data-lake-store-accounts"></a>Felhasználók vagy biztonsági csoport hozzárendelése tooAzure Data Lake Store-fiók
Felhasználók hozzárendelése vagy a biztonsági csoportok tooAzure Data Lake Store-fiók hozzáférési toohello felügyeleti műveleteket hello fiókját hello Azure-portál és az Azure Resource Manager API-k szabályozhatja. 

1. Egy Azure Data Lake Store-fiók megnyitása. Hello bal oldali ablaktáblában kattintson **Tallózás**, kattintson a **Data Lake Store**, és hello Data Lake Store panelen, kattintson a hello fiók neve toowhich kívánt tooassign egy felhasználót vagy biztonsági csoportot.

2. A Data Lake Store-fiók beállítások panelen kattintson a **hozzáférés-vezérlés (IAM)**. alapértelmezett listák hello panelre **előfizetés rendszergazdái** tulajdonos csoportot.
   
    ![Rendelje hozzá a biztonsági csoport tooAzure Data Lake Store-fiók](./media/data-lake-store-secure-data/adl.select.user.icon.png "rendelje hozzá a biztonsági csoport tooAzure Data Lake Store-fiók")

    Két módon tooadd egy csoport, illetve rendelje hozzá kapcsolódó szerepkör is.
   
    * Adja hozzá a felhasználó vagy csoport toohello fiókot, és hozzárendelheti a szerepkör, vagy
    * Adja hozzá a szerepkört, és hozzárendelheti a felhasználók/csoportok toorole.
     
    Ebben a szakaszban úgy tekintünk hello első módszer, majd a szerepkörök hozzárendelése és a csoport hozzáadása. Hajtsa végre a hasonló lépéseket toofirst válassza ki a szerepkört, és hozzárendelheti a csoportok toothat szerepkör.
4. A hello **felhasználók** panelen kattintson a **Hozzáadás** tooopen hello **hozzáférés hozzáadása** panelen. A hello **hozzáférés hozzáadása** panelen kattintson a **Szerepkörválasztás**, majd válassza ki a szerepkört hello felhasználó/csoport.
   
    ![Adja hozzá a szerepkört hello felhasználó](./media/data-lake-store-secure-data/adl.add.user.1.png "hello felhasználó szerepkör hozzáadása")
   
    Hello **tulajdonos** és **közreműködő** szerepkör biztosítanak hozzáférést tooa számos felügyeleti feladatot hello data lake-fiók. A felhasználók számára, akik hello data Lake-adatokkal együtt fog működni, később is hozzáadhatja toohello ** olvasó ** szerepkör. hello ezek a szerepkörök hatóköre korlátozott toohello felügyeleti műveletek kapcsolódó toohello Azure Data Lake Store-fiók.
   
    Az adatok műveletek egyedi fájlrendszerre vonatkozó engedélyek megadása hello végezhető műveletek. Ezért egy olvasó szerepkört rendelkező felhasználói csak felügyeleti beállítások megjelenítése hello fiókhoz társított is, de is potenciálisan olvasási és írási adatok toothem hozzárendelt fájlrendszerbeli engedélyek alapján. Data Lake Store fájlrendszerre vonatkozó engedélyek ismerteti a következő [hozzárendelése biztonsági csoportot az Azure Data Lake Store-fájlrendszer hozzáférés-vezérlési listák toohello](#filepermissions).
5. A hello **hozzáférés hozzáadása** panelen kattintson a **felhasználók hozzáadása az** tooopen hello **felhasználók hozzáadása az** panelen. Ezen a panelen keresse meg az Azure Active Directoryban korábban létrehozott hello biztonsági csoport. Ha a csoportok toosearch számos, használhatja hello szövegmező hello felső toofilter a hello csoport nevére. Kattintson a **Kiválasztás** gombra.
   
    ![Biztonsági csoport hozzáadása](./media/data-lake-store-secure-data/adl.add.user.2.png "biztonsági csoport hozzáadása")
   
    Ha azt szeretné, hogy a csoport vagy felhasználó nem szerepel a tooadd, felajánlhatja őket hello segítségével **meghívása** ikonra, és e-mail címének hello hello felhasználó vagy csoport megadása.
6. Kattintson az **OK** gombra. Meg kell jelennie hello biztonsági csoportot hozzáadni a lent látható módon.
   
    ![Biztonsági csoport hozzáadott](./media/data-lake-store-secure-data/adl.add.user.3.png "hozzáadott biztonsági csoport")

7. A felhasználók vagy biztonsági csoport rendelkezik hozzáférési toohello Azure Data Lake Store-fiók. Ha azt szeretné, hogy tooprovide hozzáférés toospecific felhasználók, később is hozzáadhatja toohello biztonsági csoport. Hasonlóképpen ha azt szeretné, hogy toorevoke hozzáférést egy felhasználó számára, eltávolíthatja őket hello biztonsági csoportból. Több biztonsági csoportok tooan fiók is hozzárendelhetők. 

## <a name="filepermissions"></a>Azure Data Lake Store-fájlrendszer hozzáférés-vezérlési listák toohello rendeljen felhasználók vagy biztonsági csoport
Felhasználók vagy biztonsági csoportok hozzárendelése toohello Azure Data Lake-fájlrendszer, állítsa a hozzáférés-vezérlés az Azure Data Lake Store-ban tárolt hello adatok.

1. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Könyvtárak létrehozása a Data Lake Store-fiók](./media/data-lake-store-secure-data/adl.start.data.explorer.png "könyvtárak létrehozása a Data Lake-fiókban")
2. A hello **adatkezelő** paneljén kattintson hello fájlra vagy mappára, amelyhez szeretné, hogy tooconfigure hello hozzáférés-vezérlési lista, és kattintson **hozzáférés**. tooassign ACL tooa fájl, meg kell nyomnia **hozzáférés** a hello **fájljának előnézete** panel.
   
    ![Hozzáférés-vezérlési listák beállítása a Data Lake fájlrendszerben](./media/data-lake-store-secure-data/adl.acl.1.png "beállítása ACL-ek Data Lake fájlrendszer")
3. Hello **hozzáférés** panel hello szokásos hozzáférés sorolja fel, és egyéni hozzáférési már hozzá van rendelve toohello legfelső szintű. Kattintson a hello **Hozzáadás** ikon tooadd egyéni szintű hozzáférés-vezérlési listák.
   
    ![Normál és egyéni hozzáférési listában](./media/data-lake-store-secure-data/adl.acl.2.png "normál és egyéni hozzáférési listában")
   
   * **Szabványos hozzáférési** van hello UNIX típusú hozzáférés, amelyben meg kell határoznia olvasási, írási, (rwx) toothree különböző felhasználói osztályok hajtható végre: tulajdonos, csoport és mások számára.
   * **Egyéni hozzáférési** toohello POSIX hozzáférés-vezérlési listákat, amely lehetővé teszi az adott nevű felhasználók vagy csoportok, és nem csak hello fájl tulajdonosa vagy csoport tooset engedélyek felel meg. 
     
     További információkért lásd: [HDFS hozzáférés-vezérlési listák](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). A hozzáférés-vezérlési listákat a Data Lake Store megvalósított hogyan további információkért lásd: [hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).
4. Kattintson a hello **hozzáadása** ikon tooopen hello **egyéni hozzáférés hozzáadása** panelen. Ezen a panelen kattintson a **felhasználó vagy csoport kiválasztása**, majd a **felhasználó vagy csoport kiválasztása** panelen keresse meg az Azure Active Directory korábban létrehozott hello biztonsági csoport. Ha a csoportok toosearch számos, használhatja hello szövegmező hello felső toofilter a hello csoport nevére. Hello csoport tooadd szeretné, majd kattintson **válasszon**.
   
    ![Csoport hozzáadása](./media/data-lake-store-secure-data/adl.acl.3.png "csoport hozzáadása")
5. Kattintson a **Select engedélyeket**, hello engedélyként válassza, és hogy tooassign hello engedélyek valóban ACL alapértelmezés szerint, elérhetők a hozzáférés-vezérlési lista, vagy mindkettőt. Kattintson az **OK** gombra.
   
    ![Rendelje hozzá az engedélyek toogroup](./media/data-lake-store-secure-data/adl.acl.4.png "toogroup engedélyek hozzárendelése")
   
    A Data Lake Store és a hozzáférés-vezérlési listákat alapértelmezett/hozzáférési engedélyekkel kapcsolatos további információkért lásd: [hozzáférés-vezérlés a Data Lake Store](data-lake-store-access-control.md).
6. A hello **egyéni hozzáférés hozzáadása** panelen kattintson a **OK**. hello újonnan hozzáadott csoporthoz tartozó hello engedélyeivel mostantól szerepel hello **hozzáférés** panelen.
   
    ![Rendelje hozzá az engedélyek toogroup](./media/data-lake-store-secure-data/adl.acl.5.png "toogroup engedélyek hozzárendelése")
   
   > [!IMPORTANT]
   > Hello jelenlegi kiadásban, csak akkor 9 bejegyzések **egyéni hozzáférési**. Ha azt szeretné, tooadd legfeljebb 9-felhasználók, készítsen biztonsági csoportokat, vegye fel a felhasználók toosecurity csoportokat, vegye fel toothose biztonsági csoportok hozzáférést biztosítson a hello Data Lake Store-fiók.
   > 
   > 
7. Szükség esetén módosíthatja is hello hozzáférési engedélyek hello csoporthoz való hozzáadását követően. Törölje vagy jelölje be hello jelölőnégyzetet az egyes engedélyeket (olvasás, írás, végrehajtás) alapján, hogy szeretné-e tooremove, vagy rendeljen hozzá engedély toohello biztonsági csoportba. Kattintson a **mentése** toosave hello módosításokat, vagy **elvetése** tooundo hello módosításokat.

## <a name="set-ip-address-range-for-data-access"></a>Állítsa be az IP-címtartomány a eléréséhez
Azure Data Lake Store lehetővé teszi toofurther zár le a hozzáférési tooyour adattár szintjén. Tűzfal engedélyezése, IP-cím vagy IP-címtartomány megadása a megbízható ügyfeleket. Miután engedélyezte őket, csak a definiált tartományon belüli hello IP-címmel rendelkező ügyfelek kapcsolódhatnak toohello tároló.

![Tűzfalbeállítások és IP-hozzáférési](./media/data-lake-store-secure-data/firewall-ip-access.png "tűzfalbeállítások és IP-cím")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Távolítsa el az Azure Data Lake Store-fiók biztonsági csoportok
Biztonsági csoportok az Azure Data Lake Store-fiókból való eltávolításakor hozzáférési toohello felügyeleti műveleteket hello fiókját hello Azure portál és az Azure Resource Manager API-kat csak módosítani.

1. A Data Lake Store-fiók panelen kattintson a **beállítások**. A hello **beállítások** panelen kattintson a **felhasználók**.
   
    ![Rendelje hozzá a biztonsági csoport tooAzure Data Lake-fiók](./media/data-lake-store-secure-data/adl.select.user.icon.png "rendelje hozzá a biztonsági csoport tooAzure Data Lake-fiók")
2. A hello **felhasználók** panelen kattintson a kívánt tooremove hello biztonsági csoport.
   
    ![Biztonsági csoport tooremove](./media/data-lake-store-secure-data/adl.add.user.3.png "biztonsági csoport tooremove")
3. Hello biztonsági csoport hello paneljén kattintson **eltávolítása**.
   
    ![Biztonsági csoport eltávolítása](./media/data-lake-store-secure-data/adl.remove.group.png "biztonsági csoport eltávolítása")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Azure Data Lake Store-fájlrendszer hozzáférés-vezérlési listák biztonsági csoport eltávolítása
Biztonsági csoportok hozzáférés-vezérlési listák eltávolítja az Azure Data Lake Store-fájlrendszer, amikor hozzáférési toohello adat hello Data Lake Store módosítása.

1. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Könyvtárak létrehozása a Data Lake-fiókban](./media/data-lake-store-secure-data/adl.start.data.explorer.png "könyvtárak létrehozása a Data Lake-fiókban")
2. A hello **adatkezelő** paneljén kattintson hello fájl vagy mappa legyen tooremove hello ACL, majd a fiók panelen hello **hozzáférés** ikonra. tooremove hozzáférés-vezérlési lista a fájl, meg kell nyomnia **hozzáférés** a hello **fájljának előnézete** panelen.
   
    ![Hozzáférés-vezérlési listák beállítása a Data Lake fájlrendszerben](./media/data-lake-store-secure-data/adl.acl.1.png "beállítása ACL-ek Data Lake fájlrendszer")
3. A hello **hozzáférés** paneljén hello **egyéni hozzáférési** területen kattintson a kívánt tooremove hello biztonsági csoport. A hello **egyéni hozzáférési** panelen kattintson a **eltávolítása** , majd **OK**.
   
    ![Rendelje hozzá az engedélyek toogroup](./media/data-lake-store-secure-data/adl.remove.acl.png "toogroup engedélyek hozzárendelése")

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Adatok másolása az Azure Storage Blobs tooData Lake Store-ból](data-lake-store-copy-data-azure-storage-blob.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Ismerkedés a Data Lake Store PowerShell használatával](data-lake-store-get-started-powershell.md)
* [Ismerkedés a Data Lake Store .NET SDK használatával](data-lake-store-get-started-net-sdk.md)
* [A Data Lake Store diagnosztikai naplóinak elérése](data-lake-store-diagnostic-logs.md)

