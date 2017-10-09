---
title: "aaaReliable szolgáltatások értesítések |} Microsoft Docs"
description: "Service Fabric Reliable Services értesítések fogalmi dokumentációja"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: 8c43190d31dbe82d1dc7fa1c228128bdcc3684f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-notifications"></a>Megbízható szolgáltatások értesítések
Értesítések engedélyezése az ügyfelek tootrack hello kíváncsiak vagyunk tooan objektum alatt végzett módosításokat. Kétféle típusú objektumok támogatja értesítéseket: *megbízható állapotkezelője* és *megbízható szótár*.

Értesítések használata gyakori okai a következők:

* Épület materializált nézetek, például a másodlagos indexek vagy szűrt nézeteinek hello replika állapotát összesíti. Példa: egy rendezett index összes kulcsok megbízható szótárban.
* Küldő figyelési adatok, például az elmúlt egy órában hello hozzáadott felhasználók hello száma.

Értesítések elindulása részeként műveleteket alkalmazása esetén. Miatt, amely értesítések gyors lehetséges és szinkron események nem tartalmazhatja a költséges műveleteket kell kezelni.

## <a name="reliable-state-manager-notifications"></a>Megbízható állapot Manager értesítéseire
Megbízható állapotkezelője események hello értesítéseket biztosít:

* Tranzakció
  * Véglegesítés
* Állapot manager
  * Építse újra
  * A megbízható állapot hozzáadása
  * A megbízható állapot törlése

Megbízható állapotkezelője hello aktuális aktív tranzakciók követi nyomon. hello mindössze annyi a változás egy értesítési toobe indította okozó tranzakció állapotban egy tranzakció véglegesítés alatt áll.

Megbízható állapotkezelője tart fenn például megbízható szótár és megbízható várólista megbízható állapotok gyűjteménye. Megbízható állapotkezelője értesítések következik be, amikor megváltozik a gyűjtemény: olyan megbízható állapotban van hozzáadásakor vagy eltávolításakor, vagy újraépítésekor hello teljes gyűjteményt.
hello megbízható állapotkezelője gyűjtemény újraépítésekor három esetben:

* Helyreállítás: Replika indításakor azt anélkül állíthatóak vissza korábbi állapotba hello lemezről. Helyreállítási hello végén, használja **NotifyStateManagerChangedEventArgs** toofire helyreállított megbízható állapotok hello készletét tartalmazó esemény.
* Másolás teljes: előtt a replika hello konfigurációkészlet csatlakozhat, a beépített toobe rendelkezik. Egyes esetekben ehhez hello elsődleges replika toobe alkalmazott toohello tétlen másodlagos replika megbízható állapotkezelője állapot teljes másolata. Hello másodlagos replika használja a megbízható állapotkezelője **NotifyStateManagerChangedEventArgs** toofire azért szerzett hello elsődleges replikából megbízható állapotok hello készletét tartalmazó esemény.
* Visszaállítás: A vész-helyreállítási helyzetekben hello replika visszaállítható egy biztonsági másolatból keresztül **RestoreAsync**. Ilyen esetekben megbízható állapotkezelője hello elsődleges replikán használ **NotifyStateManagerChangedEventArgs** toofire egy eseményt, amely megbízható állapotok azt hello biztonsági másolatból visszaállított hello készletét tartalmazza.

tranzakció értesítések és/vagy állapot manager értesítéseinek tooregister, van szüksége a hello tooregister **TransactionChanged** vagy **StateManagerChanged** megbízható állapotkezelője eseményeit. Gyakori hely a ezek eseménykezelők tooregister az állapotalapú szolgáltatás hello konstruktor. Amikor regisztrál a hello konstruktor, teljesíti-e nem hello élettartama során változás által okozott értesítésekhez **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Hello **TransactionChanged** eseménykezelő használ **NotifyTransactionChangedEventArgs** hello esemény tooprovide adatait. Hello művelet tulajdonságot tartalmaz (például **NotifyTransactionChangedAction.Commit**), amely megadja, hogy a módosítás hello típusát. Hello transaction tulajdonság, amely biztosítja, hogy módosultak a hivatkozás toohello tranzakció is tartalmaz.

> [!NOTE]
> Napjainkban **TransactionChanged** események aktiválódnak csak akkor, ha hello tranzakció. hello művelet megegyezik túl**NotifyTransactionChangedAction.Commit**. De a jövőbeli hello események előfordulhat, hogy kiváltott tranzakció állapotváltozások más típusú. Javasoljuk, hogy hello művelet ellenőrzése és a hello esemény feldolgozása csak akkor, ha egy, a várt.
> 
> 

Példa **TransactionChanged** eseménykezelő.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Hello **StateManagerChanged** eseménykezelő használ **NotifyStateManagerChangedEventArgs** hello esemény tooprovide adatait.
**NotifyStateManagerChangedEventArgs** két alosztályok rendelkezik: **NotifyStateManagerRebuildEventArgs** és **NotifyStateManagerSingleEntityChangedEventArgs**.
Hello művelet tulajdonságával a **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello helytelen alosztályának:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** és **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Példa **StateManagerChanged** értesítéskezelő.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Megbízható szótár értesítések
Megbízható szótár események hello értesítéseket biztosít:

* Újraépítés: Meghívva **ReliableDictionary** helyreállt állapotában a helyreállított vagy másolt helyi állapot vagy a biztonsági mentés.
* Törlés: Meghívva állapotának hello **ReliableDictionary** keresztül hello törölve lett **ClearAsync** metódust.
* Adja hozzá: Hívható meg, ha egy elem túl hozzá lett adva**ReliableDictionary**.
* Frissítés: Hívható meg, ha az elem **IReliableDictionary** frissítve lett.
* Eltávolítás: Hívható meg, ha az elem **IReliableDictionary** törölve lett.

tooget megbízható szótár értesítések, van szüksége a hello tooregister **DictionaryChanged** eseménykezelő a **IReliableDictionary**. A közös helyen hello van az e eseménykezelők tooregister **ReliableStateManager.StateManagerChanged** értesítési hozzáadása.
Ha regisztrálja **IReliableDictionary** túl szerepel-e**IReliableStateManager** biztosítja, hogy nem hagyott ki belőle értesítéseket.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification** hello minta módszer van a fenti hello **OnStateManagerChangedHandler** példa hívások.
> 
> 

hello előző kód beállítja hello **IReliableNotificationAsyncCallback** kommunikáljanak, jelszavat **DictionaryChanged**. Mivel **NotifyDictionaryRebuildEventArgs** tartalmaz egy **IAsyncEnumerable** illesztő – amely toobe aszinkron módon--számba kell belüli értesítések elindulása esetén keresztül  **RebuildNotificationAsyncCallback** helyett **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> A kód, megelőző feldolgozási hello rebuild értesítési részeként hello első hello megmarad az összesített állapota nincs bejelölve. Hello megbízható gyűjtemény újraépíti az új állapot, mert az összes korábbi értesítés irrelevánsak.
> 
> 

Hello **DictionaryChanged** eseménykezelő használ **NotifyDictionaryChangedEventArgs** hello esemény tooprovide adatait.
**NotifyDictionaryChangedEventArgs** öt alosztályok rendelkezik. A hello művelet tulajdonsággal **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello helytelen alosztályának:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add** és **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Javaslatok
* *Tegye* értesítési események lehető legnagyobb befejezéséhez.
* *Ne* hajtható végre a költséges műveleteket (például i/o-műveletek) szinkron események részeként.
* *Tegye* hello művelet típusának ellenőrzése előtt hello esemény feldolgozása. Új művelettípusok hello jövőbeli lehet hozzáadni.

Íme néhány dolgot tookeep figyelembe vételével:

* Értesítések elindulása esetén hello művelet végrehajtásának részeként. Például egy visszaállítási értesítést a visszaállítási művelet hello utolsó lépésként lép működésbe. A visszaállítás nem fejeződik be, amíg hello értesítési eseményt dolgoz fel.
* Értesítések elindulása műveletek alkalmazása hello részeként esetén, mert az ügyfelek csak a helyileg véglegesített műveletek értesítések:. És mivel a műveletek csak a helyileg véglegesített toobe garantáltan (más szóval naplózása), előfordulhat, hogy, vagy előfordulhat, hogy nem vonható vissza a jövőbeli hello.
* Hello visszaállítási elérési úton egy értesítés minden alkalmazott művelethez lép működésbe. Ez azt jelenti, hogy ha tranzakció T1 Create(X) Delete(X) és Create(X) tartalmaz, kaphat egy értesítési X hello létrehozását, egy hello törlésre és hello létrehozásához ebben az esetben egy abban a sorrendben.
* Az egyes tranzakciókra vonatkozóan, több műveletet tartalmazó műveleteket a rendszer, amelyben beérkezett hello felhasználótól hello elsődleges replikán hello sorrendben alkalmazza.
* Feldolgozási hamis folyamat részeként néhány művelet lehet, hogy vonható vissza. Értesítések a visszavonási műveletek, működés közbeni hello replika hátsó tooa stabil pontjának állapota hello aktiválódnak. Egy fontos visszavonási értesítések különbség, hogy eseményeket, amelyek ismétlődő kulcsok összesítése. Például ha tranzakció T1 visszavonja, egy egyetlen értesítési tooDelete(X) láthatja.

## <a name="next-steps"></a>Következő lépések
* [Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
* [Megbízható szolgáltatások biztonsági mentése és visszaállítása (katasztrófa utáni helyreállítás)](service-fabric-reliable-services-backup-restore.md)
* [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

