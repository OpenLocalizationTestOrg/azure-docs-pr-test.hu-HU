Az Azure Storage összes blobjának egy tárolóban kell lennie. hello tároló részét képezi hello blob neve. Például `mycontainer` hello név hello tároló ezekben a BLOB URI-azonosítók:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

A tároló nevének kell lennie egy érvényes DNS-nevet, a következő elnevezési szabályoknak megfelelő toohello:

1. A tárolónévnek betűvel vagy számmal kell kezdődnie, és csak betűket, számokat és kötőjelet (-) karakter hello tartalmazhat.
2. Minden kötőjel előtt és után közvetlenül egy betűnek vagy számnak kell állnia. A tárolók nevében nem szerepelhetnek egymást követő kötőjelek.
3. A tároló nevében szereplő összes betűnek kisbetűnek kell lennie.
4. A tároló nevének 3–63 karakter hosszúságúnak kell lennie.

> [!IMPORTANT]
> Vegye figyelembe, hogy hello a tároló nevének mindig kisbetűnek kell lennie. Ha egy nagybetűt szerepeljenek a tároló neve, vagy más módon megsérti a hello tároló elnevezési szabályait, előfordulhat, hogy hibaüzenet 400 (hibás kérés). 
> 
> 

