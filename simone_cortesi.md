query overpass non ancora organizzate per argomento
============================================


 1. Per estrarre tutte le fontanelle (amenity=drinking_water) a Pavia:
 
 ~~~xml
<osm-script output="json" timeout="25">
  <!-- fetch area “Pavia” to search in -->
  <id-query type="area" ref="3600044383" into="area"/>
  <!-- gather results -->
   <union>
     <!-- query part for: “amenity=drinking_water” \-->
     <query type="node">
      <has-kv k="amenity" v="drinking_water"/>
      <area-query from="area"/>
     </query>
     <query type="way">
      <has-kv k="amenity" v="drinking_water"/>
      <area-query from="area"/>
     </query>
     <query type="relation">
      <has-kv k="amenity" v="drinking_water"/>
      <area-query from="area"/>
     </query>
   </union>
  <!-- print results -->
  <print mode="body"/>
  <recurse type="down"/>
  <print mode="skeleton" order="quadtile"/>
</osm-script>
 ~~~
 Pavia è definita da 3600044383 ovvero 3600000000 + 44383. Dove 44383 è l'ID della [relation](https://www.openstreetmap.org/relation/44383) che racchiude il Comune.

 2. Metodo alternativo con sintassi compatta (in questo caso estrae i civici nel comune di Magherno
 
 ~~~xml
 [out:json][timeout:25];
 area(3600044451)->.searchArea;
 (node["addr:housenumber"](area.searchArea););
 out body;
 >;
 out skel qt;
 ~~~

 3. il confine amministrativo del Comune di Campospinoso

 ~~~xml
 area(3600044191)->.searchArea;
(relation["boundary"="administrative"][admin_level=8](area.searchArea););
out meta;
>;
out meta qt; 
~~~

 4. tutti gli oggetti node all'interno del confine della relation Italia che abbiano chiave wikipedia

 ~~~xml
 [out:json][timeout:250];
area(3600365331)->.searchArea;
(
  node["wikipedia"](area.searchArea)
);
out body;
>;
out skel qt;
~~~
