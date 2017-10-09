## <a name="what-is-queue-storage"></a>Mi a Queue Storage?
Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához. Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.

A Queue Storage gyakori használati módjai:

* A munkahelyi tooprocess várakozó aszinkron módon létrehozása
* Üzenetek átadása egy Azure webes szerepkör tooan Azure feldolgozói szerepkörnek

## <a name="queue-service-concepts"></a>A Queue szolgáltatás alapfogalmai
Queue szolgáltatás hello hello a következő összetevőket tartalmazza:

![Queue1](./media/storage-queue-concepts-include/queue1.png)

* **URL-formátum:** várólisták, amelyek megcímezhető hello URL-cím formátuma a következő használatával:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    a következő URL-cím hello hello ábra egyik üzenetsora címek:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Az Azure Storage méretezhetőségi és teljesítménycéljai).
* **Üzenetsor:** Az üzenetsorok üzenetek készleteit tartalmazzák. Az összes üzenetnek üzenetsorban kell lennie. Vegye figyelembe, hogy hello várólista neve csak kisbetűket. Az üzenetsorok elnevezésével kapcsolatos információkat lásd: [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Üzenetsorok és metaadatok elnevezése).
* **Üzenet:** egy üzenet jelenik meg, tetszőleges méretű a too64 KB fel. hello maximális időtartam, egy üzenet maradhat hello sorban 7 nap.

