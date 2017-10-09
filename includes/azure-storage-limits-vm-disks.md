Az Azure virtuális gépek több adatlemez csatolását támogatták. Az optimális teljesítmény érdekében érdemes erősen túlterhelt lemezek számát illetően toolimit hello csatolt toohello virtuális gép tooavoid lehetséges szabályozás. Ha az összes lemez nem kívánt magas használhatók hello, egy időben hello tárfiók egy nagyobb számú lemezek is támogatja.

* **Az Azure Managed lemezek:** felügyelt lemezek maximális száma regionális és hello tárolótípus is függ. alapértelmezett hello és is hello maximális száma 10 000 előfizetésenként, régiónként és egy tárolási típust. Például be too10 hozhat létre, 000 szabványos felügyelt lemezeket, és 10 000 premium lemezek egy előfizetésben és régióban felügyelt. 

    Felügyelt pillanatképek és a képeket elleni felügyelt lemezek korlátozása hello bájtjai számítanak.

* **Standard szintű tárfiókok esetén:** A Standard szintű tárfiókok összesen legfeljebb 20 000 IOPS kérelemaránnyal rendelkeznek. hello összes egy standard szintű tárfiók a virtuális gép lemezeinek teljes iops-érték nem haladhatja meg ezt a határt.
  
    Nagyjából kiszámításához hello erősen túlterhelt lemezek hello kérelem sávszélesség-korlátjának alapján egységes tárfiók által támogatott. Például egy virtuális gép, az alapszintű csomag hello maximális száma a magas be a lemezek készül 66 (20 000/300 iops-érték lemezenként), pedig a Standard szint virtuális gépek készül 40 (20 000 500 iops-érték lemezenként), hello az alábbi táblázatban ismertetett módon. 
* **Prémium szintű tárfiókok esetén:** A prémium szintű tárfiókok összesen legfeljebb 50 Gbps átviteli sebességgel rendelkeznek. hello összes, a virtuális lemezek teljes átviteli sebesség nem haladhatja meg ezt a határt.

