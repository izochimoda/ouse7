<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- Titre de la page web  -->
    <title>Cartographie web</title>

    <!-- Insertion de la CSS librairie Leaflet -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />

    <!--Inserer CSS Librairie Bootstrap-->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">

    <!--Inserer la  JS librairie Leaflet -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
        integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

    <!--Inserer la  JS librairie Bootstrap -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
        crossorigin="anonymous"></script>

    <!--Inserer la js Jquery-->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"
        integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>

    <!--Ajouter le Js d'imprimante de leaflet-->
    <script type="text/javascript" src="js/leaflet.browser.print.min.js"></script>

    <!-- leaflet geojson  -->
    <script type="text/javascript" src="C:\xampp\tomcat\webapps\geosn\data/departement.geojson"></script>
    <script type="text/javascript" src="C:\xampp\tomcat\webapps\geosn\data/Region.geojson"></script>
	<script type="text/javascript" src="C:\xampp\tomcat\webapps\geosn\data/chemin_de_fer.geojson"></script>

    <!--Style de dimension de la page-->
    <style>
        #map {
            block-size: 90vh;
        }
    </style>


</head>

<body>
    <div class="container-fluid">
        <nav class="navbar navbar-expand-lg bg-body-tertiary" data-bs-theme="dark">
            <a class="navbar-brand" href="#" style="padding: 0px"> <img src="img/sn.jpg" style="block-size: 40px;"></a>
            <a class="navbar-brand" href="#">Projet SIG Web</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#">Acceuil</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#">Lien</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link disabled" aria-disabled="true">Desactiver</a>
                    </li>
                </ul>
                <form class="d-flex" role="search">
                    <input class="form-control me-2" type="search" placeholder="Rechercher" aria-label="Rechercher">
                    <button class="btn btn-outline-success" type="submit">Rechercher</button>
                </form>
            </div>
    </div>
    </nav>
    </div>


    <!-- Ici on va mettre notre carte -->
    <div id="map"></div>

    <!--Parametrer la carte web-->
    <script>
        //inserer un fond de carte OSM
        var osm = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: '© OpenStreetMap'
        });

        var osmHOT = L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: '© OpenStreetMap contributors, Tiles style by Humanitarian OpenStreetMap Team hosted by OpenStreetMap France'
        });

        var satellite = L.tileLayer('https://mt1.google.com/vt/lyrs=y&x={x}&y={y}&z={z}', {
            maxZoom: 20,
            attribution: '© Google'
        });

        var Stadia_Dark = L.tileLayer('https://tiles.stadiamaps.com/tiles/alidade_smooth_dark/{z}/{x}/{y}{r}.{ext}', {
            minZoom: 0,
            maxZoom: 20,
            attribution: '&copy; <a href="https://www.stadiamaps.com/" target="_blank">Stadia Maps</a> &copy; <a href="https://openmaptiles.org/" target="_blank">OpenMapTiles</a> &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
            ext: 'png'
        });

        //Inserer etendue centre de la carte
        var map = L.map('map', {
            center: [14.370834, -14.831543],
            zoom: 7,
            layers: [osm]
        });

        //Inserer les fonds de carte dans le controle de couche
        var baseMaps = {
            "OpenStreetMap": osm,
            "OpenStreetMap.HOT": osmHOT,
            "Satellite": satellite,
            "Dark": Stadia_Dark
        };


        //Inserer un fichier GeoJson

        var departement = L.geoJson(departement).addTo(map);
        var Region = L.geoJson(Region).addTo(map);
        var chemin_de_fer = L.geoJson(chemin_de_fer).addTo(map);




        //Inserer les fonds de carte dans le controle de couche
        var Layer = {
            "departement": departement,
            "Region": Region,
			"chemin_de_fer" :chemin_de_fer
        };

        //Inserer le controle des couches
        var layerControl = L.control.layers(baseMaps, Layer).addTo(map);

        // Ajouter l'impression des cartes
        L.control.browserPrint({ position: 'topleft' }).addTo(map);

        //Inserer l'echelle
        L.control.scale().addTo(map);

    </script>
</body>

</html>
