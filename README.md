## Centro Histórico de Juiz de Fora
### Delimitação espacial

O **Jornal Tribuna de Minas**
[noticiou](https://tribunademinas.com.br/noticias/cidade/24-01-2025/centro-historico-jf.html)
a publicação de um
[decreto](https://www.pjf.mg.gov.br/e_atos/e_atos_vis.php?id=126353),
pela Prefeitura Municipal de Juiz de Fora, que delimita o Centro
Histórico da cidade.
[Anexo](https://www.pjf.mg.gov.br/e_atos/anexos/anexo_centro_175327.pdf)
ao decreto, consta um memorial descritivo do polígono de delimitação da
área.

A partir das coordenadas UTM do polígono fornecidas, vamos criar um mapa
interativo do Centro Histórico, utilizando as bibliotecas do Python.

<br>
**Importamos as bibliotecas necessárias:**

``` python
import folium
from folium import plugins
from pyproj import Proj, Transformer
from shapely.geometry import Polygon
```

**Definimos a área de interesse:**

``` python
# Coordenadas UTM do polígono
vertices_utm = [
    (671631.01298, 7592562.9258),
    (670483.76674, 7593577.0383),
    (670398.10899, 7593532.9851),
    (670322.59534, 7593692.0525),
    (670290.67635, 7593669.7837),
    (670377.32049, 7593520.1438),
    (670302.54290, 7593484.0129),
    (670391.01186, 7593079.2177),
    (670353.52763, 7593071.3745),
    (670291.86401, 7593099.9004),
    (670188.72948, 7593021.7110),
    (670392.36957, 7593068.9978),
    (670471.79272, 7592740.6347),
    (670401.69082, 7592724.5514),
    (670423.71811, 7592633.6294),
    (670231.47881, 7592584.9345),
    (670234.18470, 7592574.6061),
    (670494.86684, 7592637.8945),
    (670565.63917, 7592323.5994)
]
```

**Definimos o Sistema de Referência Espacial**

Convertemos as coordenadas UTM fornecidas do Sistema SIRGAS 2000 para
EPSG4326, que é o padrão adotado pelo *folium*.

``` python
# Inicializar transformador UTM para WGS84
proj_utm = Proj(proj='utm', zone=23, south=True, ellps='GRS80', datum='WGS84')
transformer = Transformer.from_proj(proj_utm, 'epsg:4326')

# Converter coordenadas UTM para latitude e longitude
vertices_wgs84 = [transformer.transform(easting, northing) for easting, northing in vertices_utm]
```

*Exibimos as coordenadas obtidas*

``` python
print(vertices_wgs84)
```

::: {.output .stream .stdout}
    [(-21.762281043001835, -43.34016652775217), (-21.753233459476547, -43.351362572075494), (-21.753639557293926, -43.352186110828214), (-21.75221027520308, -43.35293251378737), (-21.752414458261512, -43.353238788656014), (-21.753757529707904, -43.35238575673133), (-21.754091028762748, -43.35310493520417), (-21.75773825072305, -43.352207947099004), (-21.757812692768333, -43.35256952148563), (-21.75756100908489, -43.353168600600966), (-21.758277067755575, -43.354157612890276), (-21.75783041668798, -43.35219376769888), (-21.760788237237197, -43.35139206518308), (-21.760940240650374, -43.35206813839776), (-21.761759241939266, -43.35184580552671), (-21.762217519769386, -43.35369933505048), (-21.76231053597393, -43.35367211067517), (-21.76171386749516, -43.351158388857264), (-21.764545464282058, -43.350441727160124)]

**Definimos os parâmetros do polígono**

``` python
## Inverter para a ordem correta (latitude, longitude)
location = [(lat, lon) for lon, lat in vertices_wgs84]

# Criar polígono no Folium
polygon = Polygon(location)

# Obter os limites do polígono (bounding box)
bounds = polygon.bounds  # (min_lon, min_lat, max_lon, max_lat)
```

**Definimos os parâmetros do mapa**

``` python
# Criar mapa centralizado nos limites do polígono
mapa = folium.Map()

# Adicionar polígono ao mapa
folium.Polygon(
    locations=vertices_wgs84,
    zoom_start=15,
    color="blue",
    weight=2,
    fill=True,
    fill_opacity=0.5
).add_to(mapa)
folium.plugins.Fullscreen(
    position="bottomright",
    title="Tela cheia",
    title_cancel="Restaurar",
    force_separete_button=True,
).add_to(mapa)

# Ajustar o mapa aos limites do polígono
mapa.fit_bounds([[bounds[1], bounds[0]], [bounds[3], bounds[2]]])
```

**Visualizamos o mapa**

``` python
# Salvar e exibir mapa
mapa.save("mapa_com_poligono.html")
mapa
```

::: {.output .execute_result execution_count="13"}
```{=html}
<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    
    &lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;
    
        &lt;script&gt;
            L_NO_TOUCH = false;
            L_DISABLE_3D = false;
        &lt;/script&gt;
    
    &lt;style&gt;html, body {width: 100%;height: 100%;margin: 0;padding: 0;}&lt;/style&gt;
    &lt;style&gt;#map {position:absolute;top:0;bottom:0;right:0;left:0;}&lt;/style&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://code.jquery.com/jquery-3.7.1.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap-glyphicons.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.2.0/css/all.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css&quot;/&gt;
    
            &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,
                initial-scale=1.0, maximum-scale=1.0, user-scalable=no&quot; /&gt;
            &lt;style&gt;
                #map_89e3d5b8a98b0b2e76f4a01cab988003 {
                    position: relative;
                    width: 100.0%;
                    height: 100.0%;
                    left: 0.0%;
                    top: 0.0%;
                }
                .leaflet-container { font-size: 1rem; }
            &lt;/style&gt;
        
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet.fullscreen@3.0.0/Control.FullScreen.min.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet.fullscreen@3.0.0/Control.FullScreen.css&quot;/&gt;
&lt;/head&gt;
&lt;body&gt;
    
    
            &lt;div class=&quot;folium-map&quot; id=&quot;map_89e3d5b8a98b0b2e76f4a01cab988003&quot; &gt;&lt;/div&gt;
        
&lt;/body&gt;
&lt;script&gt;
    
    
            var map_89e3d5b8a98b0b2e76f4a01cab988003 = L.map(
                &quot;map_89e3d5b8a98b0b2e76f4a01cab988003&quot;,
                {
                    center: [0.0, 0.0],
                    crs: L.CRS.EPSG3857,
                    zoom: 1,
                    zoomControl: true,
                    preferCanvas: false,
                }
            );

            

        
    
            var tile_layer_f282fb16ddc1aa016b7c7353a4130dcc = L.tileLayer(
                &quot;https://tile.openstreetmap.org/{z}/{x}/{y}.png&quot;,
                {&quot;attribution&quot;: &quot;\u0026copy; \u003ca href=\&quot;https://www.openstreetmap.org/copyright\&quot;\u003eOpenStreetMap\u003c/a\u003e contributors&quot;, &quot;detectRetina&quot;: false, &quot;maxNativeZoom&quot;: 19, &quot;maxZoom&quot;: 19, &quot;minZoom&quot;: 0, &quot;noWrap&quot;: false, &quot;opacity&quot;: 1, &quot;subdomains&quot;: &quot;abc&quot;, &quot;tms&quot;: false}
            );
        
    
            tile_layer_f282fb16ddc1aa016b7c7353a4130dcc.addTo(map_89e3d5b8a98b0b2e76f4a01cab988003);
        
    
            var polygon_ec4e62d5abfff48a098f3f70c022f252 = L.polygon(
                [[-21.762281043001835, -43.34016652775217], [-21.753233459476547, -43.351362572075494], [-21.753639557293926, -43.352186110828214], [-21.75221027520308, -43.35293251378737], [-21.752414458261512, -43.353238788656014], [-21.753757529707904, -43.35238575673133], [-21.754091028762748, -43.35310493520417], [-21.75773825072305, -43.352207947099004], [-21.757812692768333, -43.35256952148563], [-21.75756100908489, -43.353168600600966], [-21.758277067755575, -43.354157612890276], [-21.75783041668798, -43.35219376769888], [-21.760788237237197, -43.35139206518308], [-21.760940240650374, -43.35206813839776], [-21.761759241939266, -43.35184580552671], [-21.762217519769386, -43.35369933505048], [-21.76231053597393, -43.35367211067517], [-21.76171386749516, -43.351158388857264], [-21.764545464282058, -43.350441727160124]],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;blue&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;blue&quot;, &quot;fillOpacity&quot;: 0.5, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;noClip&quot;: false, &quot;opacity&quot;: 1.0, &quot;smoothFactor&quot;: 1.0, &quot;stroke&quot;: true, &quot;weight&quot;: 2}
            ).addTo(map_89e3d5b8a98b0b2e76f4a01cab988003);
        
    
            L.control.fullscreen(
                {&quot;forceSeparateButton&quot;: false, &quot;forceSepareteButton&quot;: true, &quot;position&quot;: &quot;bottomright&quot;, &quot;title&quot;: &quot;Tela cheia&quot;, &quot;titleCancel&quot;: &quot;Restaurar&quot;}
            ).addTo(map_89e3d5b8a98b0b2e76f4a01cab988003);
        
    
            map_89e3d5b8a98b0b2e76f4a01cab988003.fitBounds(
                [[-21.764545464282058, -43.354157612890276], [-21.75221027520308, -43.34016652775217]],
                {}
            );
        
    
            tile_layer_f282fb16ddc1aa016b7c7353a4130dcc.addTo(map_89e3d5b8a98b0b2e76f4a01cab988003);
        
&lt;/script&gt;
&lt;/html&gt;" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>
```

``` python
```

