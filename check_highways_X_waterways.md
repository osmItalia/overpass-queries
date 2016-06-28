### query Overpass-Turbo

Serve per verificare la presenza dei nodi all'incrocio tra una
strada e un corso d'acqua.
Un buffer di 15 metri permette di selezionare tutti i corsi d'acqua in 
prossimita' delle tipologie di strade selezionate e salva il tutto nel set ".water"
Successivamente all'interno di questo set vengono cercati tutti i culvert, i ford 
e i bridge salvandoli in un set specifico per ciascuna tipologia (.culvert, .ford, .bridge).
Infine viene effettuata una differenza tra i corsi d'acqua presenti in prossimita di una strada 
e tutti i corsi d'acqua che presentano un culvert, un ford o un bridge.
Quello che ne dovrebbe risultare sono i corsi d'acqua che non presentano nodi di intersezione 
con le strade e quelli che pur presentando un nodo senza tags specifici che 
indichino il tipo di intersezione.
  

 ~~~xml
/*  
    -----------------------------------------
    -->  check highway crossing waterway  <--
    -----------------------------------------
    estract waterway near highway (buffer 15 meters)
    check presence of culvert in waterway
    check presence of ford in waterway
    check presence of bridge near waterway (buffer 5 meters)
    make difference between the waterways and the union of 
    the objects with tunnel:culvert, ford:yes and bridge:yes

*/
[out:xml][timeout:25][bbox:{{bbox}}];

way["highway"~"primary|secondary|tertiary|unclassified"];
way(around:15)["waterway"];
(._;>;)->.water;

way["tunnel"="culvert"];
way(around:0).water;
(._;>;)->.tunnel;

node["ford"="yes"];
way(around:0).water;
(._;>;)->.ford;

way["bridge"="yes"];
way(around:5).water;
(._;>;)->.bridge;
  
(.water; - (.tunnel; .ford; .bridge);)->.xroad;

.xroad out skel meta;

 ~~~
 
 