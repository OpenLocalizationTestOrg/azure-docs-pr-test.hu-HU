---
title: "a felhasználói fiókok az Azure Batch aaaRun feladatok |} Microsoft Docs"
description: "Feladatok futtatása az Azure Batch felhasználói fiókok konfigurálása"
services: batch
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.openlocfilehash: 13d7d76451d89a3cca090c4ef24ed0ed781bbf09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a>Kötegelt a felhasználói fiókok feladatok futtatása

Mindig fusson a feladat, az Azure Batch egy felhasználói fiókkal. Alapértelmezés szerint a feladatok futtathatók általános jogú felhasználói fiókokat, a rendszergazdai engedélyekkel. Ezek az alapértelmezett felhasználói fiók beállítások általában elegendők. Bizonyos esetekben azonban esetén hasznos toobe képes tooconfigure hello felhasználói fiók alatt, amely egy feladat toorun. Ez a cikk ismerteti, amelyek hello típusú felhasználói fiókokat, és hogyan konfigurálhatók a forgatókönyvnek.

## <a name="types-of-user-accounts"></a>Felhasználói fiókok típusai

Az Azure Batch kétféle típusú felhasználói fiókokat biztosít az éppen futó feladatok:

- **Automatikus-felhasználói fiókokat.** Automatikus-felhasználói fiókok olyan beépített felhasználói fiókok, hello Batch szolgáltatás automatikusan létrehozza. Alapértelmezés szerint feladatok automatikus felhasználói fiókkal futtassa. Egy feladat tooindicate alatt automatikus-felhasználói fiókot a feladat futni hello automatikus felhasználói előírása konfigurálhatja. hello automatikus felhasználói megadását lehetővé teszi toospecify hello jogosultságszint-emelés szint és hello automatikus-felhasználói fiókot hello feladat hatókörében. 

- **Egy névvel ellátott felhasználói fiókot.** Megadhat egy vagy több elnevezett felhasználói fiókok készlet hello készlet létrehozásakor. Minden felhasználói fiókhoz hello készlet minden egyes csomóponton jön létre. Továbbá toohello fiók neve, megadhatja hello felhasználói fiók jelszavát, jogosultságszint-emelés szinten, valamint a Linux-készletek, hello titkos SSH-kulcsot. Ha hozzáad egy feladatot, nevű felhasználói fiók, amely alatt ez a feladat futni hello is megadhat.

> [!IMPORTANT] 
> hello Batch szolgáltatás 2017-01-01.4.0 tartalmazza, amely megköveteli, hogy a kód toocall adott verziók frissítése használhatatlanná tévő változást. Ha áttelepítése kód köteg egy korábbi verziójából származó, vegye figyelembe, hogy hello **runElevated** tulajdonság már nem támogatott a REST API vagy kötegelt hello klienskódtárak segítségével. Új használata hello **userIdentity** egy feladat toospecify jogosultságszint-emelés szint tulajdonság. Hello című témakörben talál [frissítse a kódot toohello legújabb kötegelt ügyféloldali kódtár](#update-your-code-to-the-latest-batch-client-library) kapcsolatos gyors útmutatást a kötegelt kód frissítése használata egyik hello klienskódtárak segítségével.
>
>

> [!NOTE] 
> Ebben a cikkben ismertetett hello felhasználói fiókok nem támogatják az Remote Desktop Protocol (RDP) vagy a Secure Shell (SSH), a biztonsági okokból. 
>
> tooconnect tooa csomóponton futó hello Linux virtuálisgép-konfiguráció ssh, lásd: [használata a távoli asztal tooa Linux virtuális gép az Azure-ban](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). Tekintse meg a Windows rendszert futtató RDP,-kapcsolaton keresztül tooconnect toonodes [csatlakozás a Windows Server virtuális gép tooa](../virtual-machines/windows/connect-logon.md).<br /><br />
> Tekintse meg a tooconnect tooa csomóponton futó hello felhőalapú szolgáltatás konfigurációja RDP,-kapcsolaton keresztül [engedélyezése a távoli asztali kapcsolat az Azure Cloud Services szerepkör](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>

## <a name="user-account-access-toofiles-and-directories"></a>Felhasználói fiók hozzáférési toofiles és könyvtárak

Az automatikus-felhasználói fiókkal, és egy névvel ellátott felhasználói fiókot is rendelkezik olvasási/írási hozzáférést toohello feladatütemezési munkakönyvtár, megosztott és többpéldányos feladatok könyvtárat. Mindkét típusú fiókok olyan könyvtárai olvasási hozzáférés toohello indítás és a feladat előkészítése.

Ha egy feladat fut. hello a kezdő tevékenységre, hello feladat futtatásához használt fióknak van olvasási és írási hozzáférése toohello kezdőkönyvtára feladat. Hasonlóképpen, ha egy feladat fut. hello ugyanaz a feladat előkészítése tevékenységet, hello feladat futtatásához használt fióknak olvasási és írási hozzáférése toohello feladat előkészítése tevékenység könyvtár. Ha egy feladat fut, mint hello kezdő tevékenység vagy a feladat előkészítése tevékenységet egy másik fiókból, hello tevékenység csak olvasási hozzáféréssel toohello megfelelő directory rendelkezik.

A fájlok és könyvtárak feladat eléréséhez további információkért lásd: [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Emelt szintű hozzáférés feladatokhoz 

hello felhasználói fiók jogosultságszint-emelés szintjét jelzi, hogy fut-e a feladat emelt szintű hozzáféréssel rendelkező. Automatikus-felhasználói fiókkal és a egy névvel ellátott felhasználói fiókot is futtassa emelt szintű hozzáférés. Jogosultságszint-emelés szint a hello két lehetőségek közül választhat:

- **NonAdmin:** hello feladat fut, az általános jogú felhasználó emelt szintű hozzáférés nélkül. hello alapértelmezett jogosultságszint-emelés kötegelt felhasználói fiók szintje mindig **NonAdmin**.
- **Felügyeleti:** hello feladat emelt szintű hozzáféréssel rendelkező felhasználóként fut, és teljes körű felügyeleti engedélyekkel működik. 

## <a name="auto-user-accounts"></a>Automatikus-felhasználói fiókok

Alapértelmezés szerint feladatokat futtató kötegben automatikus felhasználói fiókkal, az általános jogú felhasználó emelt szintű hozzáférés nélkül, és a feladat hatókörében. Hello automatikus felhasználói specification feladat hatókör konfigurálásakor hello Batch szolgáltatás ezt a feladatot csak egy automatikus-felhasználói fiókot hoz létre.

hello alternatív tootask hatóköre készlet hatókör. Hello automatikus felhasználói megadása egy feladatot az alkalmazáskészlet hatókör konfigurálásakor hello feladat elérhető tooany feladat hello készlet automatikus felhasználói fiók alatt fut. Készlet hatókör kapcsolatos további információkért lásd: hello című [feladat futtatása, automatikus felhasználói készlet hatókörű hello](#run-a-task-as-the-autouser-with-pool-scope).   

hello alapértelmezett hatókör nem azonos a Windows és Linux-csomópontok:

- Windows csomópontján feladatok futtathatók feladat hatókör alapértelmezés szerint.
- Linux-csomópontok a készlet hatókör mindig futnia.

Nincsenek hello automatikus felhasználói megadását, amelyek mindegyike megfelel tooa egyedi automatikus-felhasználói fiók négy lehetséges konfigurációkat:

- A nem rendszergazda hozzáférést feladat hatókörrel (hello alapértelmezett automatikus felhasználói specifikáció)
- A feladat hatókörű (emelt szintű) rendszergazdai hozzáférés
- A nem rendszergazda hozzáférést készlet hatókörű
- Rendszergazdai hozzáférés készlet hatókörű

> [!IMPORTANT] 
> Feladat hatókör alatt futó feladatok a csomópont nem rendelkeznek tényleges hozzáférési tooother feladatát. Azonban egy rosszindulatú felhasználó hozzáférési toohello fiókhoz sikerült megoldható, ez a korlátozás, amely rendszergazdai jogosultságokkal futtatja, és más tevékenység könyvtárak fér hozzá a feladat elküldése. Egy rosszindulatú felhasználó RDP- vagy SSH-tooconnect tooa csomópont is használhatja. Fontos tooprotect hozzáférés tooyour kötegelt fiók kulcsok tooprevent ilyen esetben. Amennyiben azt gyanítja, hogy a fiók sérült, lehet, hogy tooregenerate a kulcsokat.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Emelt szintű hozzáféréssel rendelkező automatikus-felhasználóként feladat futtatása

Rendszergazdai jogosultságokkal hello automatikus felhasználói előírása konfigurálhatja, ha a toorun kötegazonosítójú feladat emelt szintű hozzáférés szükséges. A kezdő tevékenység Előfordulhat például, emelt szintű hozzáférés tooinstall szoftver hello csomóponton.

> [!NOTE] 
> Általában érdemes toouse emelt szintű hozzáférés csak szükség esetén. A bevált gyakorlat része hello minimális jogosultság szükséges tooachieve hello kívánt eredmény biztosítása. Például ha egy kezdő tevékenység szoftvereket telepít hello aktuális felhasználó, ahelyett, hogy a felhasználók lehet képes tooavoid emelt szintű hozzáférés tootasks megadása. Hello automatikus-felhasználó megadása az alkalmazáskészlet hatókörrel és a nem rendszergazdai hozzáférést minden olyan feladatokat, amelyeket a fiókot használja, beleértve a hello kezdő tevékenység hello toorun konfigurálhatja. 
>
>

a következő kódrészletek hello megjelenítése hogyan tooconfigure hello automatikus felhasználói megadását. hello példák túl hello jogosultságszint-emelés szintjének beállítása`Admin` és hatókör túl hello`Task`. Feladat hatókör hello alapértelmezett beállítás, de a hello szakét példa.

#### <a name="batch-net"></a>Batch .NET

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a>Kötegelt Java

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a>Batch Python

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Készlet hatókörű automatikus-felhasználóként feladat futtatása

Ha egy csomópont ki van építve, két készlet kiterjedő automatikus-felhasználói fiókok létrejöttek hello készlet minden egyes csomópontja, egy emelt szintű hozzáféréssel rendelkező és egy emelt szintű hozzáférés nélkül. Ezeket a két készlet kiterjedő automatikus felhasználói fiókokat valamelyike hello feladatot hello automatikus-felhasználó hatókör toopool hatókör egy adott tevékenység beállítása futtatja. 

Hello automatikus-felhasználó készlet hatókör megadása esetén minden olyan feladat, amely rendszergazdai jogosultságokkal futtassa futni hello ugyanazt a készlet kiterjedő automatikus-felhasználói fiókot. Ehhez hasonlóan rendszergazdai jogosultságok nélküli futtatott feladatok is futtathatók egy alkalmazáskészlet kiterjedő automatikus felhasználói fiókkal. 

> [!NOTE] 
> hello két készlet kiterjedő automatikus-felhasználói fiókok külön fiók is. Hello készlet szintű rendszergazdai fiók alatt futó feladatok nem adatok megosztása a hello szabványos fiókkal, és ez fordítva is igaz futó feladatok. 
>
>

hello előny toorunning alapján ugyanazt a auto-felhasználói fiókot az, hogy feladatokat is képes tooshare adatok más hello futó feladatok hello ugyanahhoz a csomóponthoz.

Egy forgatókönyvet, ahol az éppen futó feladatok hello két készlet kiterjedő automatikus-felhasználói fiókok valamelyike akkor hasznos, titkos kulcsok tevékenységek közötti megosztása szolgáltatás. Tegyük fel, hogy a kezdő tevékenységre kell tooprovision hello csomópont, amellyel más feladatok, titkos kulcs. Hello Windows Data Protection API (DPAPI) használata, de rendszergazdai jogosultságokat igényel. Ehelyett hello titkos hello felhasználói szintű védelmet biztosíthat. Ugyanazzal a fiókkal hozzáférhet hello alatt futó feladatok hello titkos emelt szintű hozzáférés nélkül.

Ossza meg egy másik forgatókönyv, ahol érdemes lehet toorun feladatok készlet hatókörű egy automatikus-felhasználói fiók alatt egy olyan Message Passing Interface (MPI) fájl. Egy MPI fájlmegosztás akkor hasznos, ha hello MPI feladat kell toowork a hello csomópontjának hello azonos fájladatok. hello átjárócsomópont létrehoz egy fájlmegosztást, amely hello gyermekcsomópontok hozzáférhetnek, abban az esetben, ha a hello futnak ugyanazt a auto-felhasználói fiókot. 

a következő kódrészletet hello hello automatikus-felhasználó hatókör toopool hatókör meg olyan feladatra, a Batch .NET állítja be. hello jogosultságszint-emelés szint nincs megadva, ezért hello feladat hello szabványos készlet kiterjedő automatikus-felhasználói fiók alatt fut.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Elnevezett felhasználói fiókok

Itt megadhatja, hogy a program elnevezett felhasználói fiókokat, amikor a készletet hoz létre. Egy névvel ellátott felhasználói fiók rendelkezik, egy nevet és jelszót. Megadhatja, hogy hello jogosultságszint-emelés szintje egy névvel ellátott felhasználói fiókot. Linux-csomópontok titkos SSH-kulcsot is megadhatja.

Hello készlet összes csomópontján nevű felhasználói fiók létezik és elérhető tooall feladatok fut azokat a csomópontokat. Nevesített felhasználók készlet tetszőleges számú adhatók meg. Feladat vagy tevékenység gyűjtemény hozzáadásakor adja meg, hogy hello feladat fut. hello nevű hello készlet definiált felhasználói fiókok közül.

Egy névvel ellátott felhasználói fiókot akkor hasznos, ha azt szeretné, hogy toorun egy feladat összes tevékenységében a hello ugyanazzal a fiókkal, azonban elkülöníti azokat az egyéb feladatokat hello a futó feladatok ugyanannyi időt vesz igénybe. Például minden feladat nevű felhasználót kell létrehozni, és az adott nevű felhasználói fiók minden feladat feladatok futtatásához. Minden feladat dolgozhat ugyanazon a titkos kulcs a saját feladatokhoz, de nem váltanak futó feladatok.

Egy elnevezett felhasználói fiók toorun egy feladatot, amely olyan engedélyeket ad meg a külső erőforrások, például fájlmegosztásokat is használható. Elnevezett felhasználói fiókkal hello felhasználói identitás szabályozza, és használhatja a felhasználói identitás tooset engedélyeket.  

Nevesített felhasználó fiókok lehetővé teszik a jelszó nélküli SSH Linux-csomópontok között. Linux-csomópontok toorun többpéldányos feladatok igénylő használható egy névvel ellátott felhasználói fiókot. Minden csomópont hello készletben futtathatnak feladatokat hello teljes készletében egy felhasználói fiókkal. Többpéldányos feladatokkal kapcsolatos további információkért lásd: [használata többszörös\-példány feladatok toorun MPI alkalmazások](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Elnevezett felhasználói fiókok létrehozása

toocreate nevű kötegben, a felhasználói fiókok hozzáadása a felhasználói fiókok toohello készlet gyűjteménye. hello alábbi kódtöredékek bemutatják, hogyan toocreate nevű felhasználói fiókokat a .NET, Java és Python. Ezek az részletek megjelenítése hogyan code toocreate rendszergazda és a nem rendszergazdai fiókok, a készlet neve. hello példák létrehozása szolgáltatással hello felhőalapú szolgáltatás konfigurációja, de hello azonos közelítse hello virtuálisgép-konfiguráció használata Windows vagy Linux készlet létrehozásakor használható.

#### <a name="batch-net-example-windows"></a>Batch .NET típusú példát (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using hello cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                                         
    virtualMachineSize: "small",                                                
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit hello pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a>Batch .NET típusú példát (Linux)

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello virtual machine configuration toouse toocreate hello pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create hello unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit hello pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a>Kötegelt Java – példa

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a>Kötegelt Python – példa

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.nonadmin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Emelt szintű hozzáférés egy névvel ellátott felhasználói fiókhoz tartozó feladat futtatása

egy tevékenység egy emelt szintű felhasználó, set hello feladat toorun **UserIdentity** tulajdonság tooa nevű felhasználói fiók hoztak létre a **ElevationLevel** tulajdonsága túl`Admin`.

A kódrészletet megadja hello tevékenység egy elnevezett felhasználói fiókkal kell futtatni. Ez a nevesített felhasználói fiók hello készletében van definiálva, hello készlet létrehozásakor. Ebben az esetben hello nevű felhasználói fiók rendszergazdai jogosultságokkal rendelkező jött létre:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-toohello-latest-batch-client-library"></a>Frissítse a kódot toohello legújabb kötegelt ügyféloldali kódtár

2017-01-01.4.0 bevezeti használhatatlanná tévő változást, cseréje hello hello kötegelt verziójú **runElevated** korábbi verzióival rendelkező hello tulajdonság **userIdentity** tulajdonság. a következő táblák hello elérhető egy egyszerű leképezési, hogy így tooupdate hello klienskódtárak korábbi verzióiban a kódot.

### <a name="batch-net"></a>Batch .NET

| Ha a kódot használja...                  | Frissítse úgy, hogy...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated`Nincs megadva | Nincs frissítés szükséges                                                                                               |

### <a name="batch-java"></a>Kötegelt Java

| Ha a kódot használja...                      | Frissítse úgy, hogy...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated`Nincs megadva | Nincs frissítés szükséges                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Ha a kódot használja...                      | Frissítse úgy, hogy...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, ahol <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | `user_identity=user`, ahol <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| `run_elevated`Nincs megadva | Nincs frissítés szükséges                                                                                                                                  |


## <a name="next-steps"></a>Következő lépések

### <a name="batch-forum"></a>Batch fórum

Hello [Azure Batch fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése. Központi a keresztül a hasznos rögzített bejegyzések, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.
