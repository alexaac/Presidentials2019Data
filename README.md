<h1>Alegerile Prezidențiale din 2019 în România - Date geografice la nivel de UAT</h1>

<h2>Surse de date</h2>
<ul>
    <li>
        Geometriile au fost create de către ANCPI și publicate pe <a href="http://geoportal.gov.ro" target="_blank">Geoportalul INSPIRE al României</a> și pe <a href="http://geoportal.gov.ro" target="_blank">Portalul de date deschise al Guvernului României</a>.
    </li>
    <li>
        Tabelul de atribute a fost completat cu date publicate liber de Biroul Electoral Central pe <a href="http://prezenta.bec.ro" target="_blank">Prezenta BEC</a>.
    </li>
    <li>
        Datele se pot reutiliza sub termenii <a href="http://data.gov.ro/base/images/logoinst/OGL-ROU-1.0.pdf" target="_blank">Licenței pentru o Guvernare Deschisă</a>.
    </li>
</ul>

<h2>Fișiere</h2>
    <ul>
        <li>
            Unitățile administrative tip poligon, din layer-ul Unitate_administrativa_UAT de pe <a href="http://geoportal.ancpi.ro/portal/apps/webappviewer/index.html?id=faeba2d173374445b1f13512bd477bb2" target="_blank">Geoportalul ANCPI</a>.
        </li>
        <li>
            Rezultate finale alegeri din turul 1, 10 noiembrie 2019, din fișierul pv_RO_PRSD_FINAL.csv, de pe <a href="https://prezenta.bec.ro/prezidentiale10112019/romania-pv-final" target="_blank">Prezenta BEC 10-11-2019</a>.
        </li>
        <li>
            Rezultate finale alegeri din turul 2, 24 noiembrie 2019, din fișierul pv_RO_PRSD_FINAL.csv, de pe <a href="https://prezenta.bec.ro/prezidentiale24112019/romania-pv-final" target="_blank">Prezenta BEC 24-11-2019</a>.
        </li>
    </ul>

<h2>Sistem de Coordonate</h2>
    <ul>
        <li>
            EPSG:3844 - Pulkovo 1942(58) / Stereo70 - Projected
        </li>
        <li>
            EPSG:4326 - WGS 84 - Geographic
        </li>
    </ul>

<h2>Metode</h2>
    <p>
        Procesarea datelor a fost făcută în QGIS 3.8.3-Zanzibar și în Spatialite (tot prin QGIS).
    </p>
    <p>
        Au fost descărcate datele geografice și cele electorale, apoi s-a trecut la agregarea rezultatelor electorale la nivel de UAT (deoarece erau la nivel de secție de votare), pentru a putea face legătura între cele două seturi de date.
    </p>
    <p>
        Gruparea datelor din tabelul pv_RO_PRSD_FINAL s-a făcut pe câmpul Siruta, în Spatialite, și a fost creat un view temporar pv_RO_UAT_VIEW.
    </p>
    <p>
        A urmat Join-ul între fișierul Unitate_administrativa_UAT și pv_RO_UAT_VIEW, pe Siruta = natCode.
    </p>
    <p>
        Au rezultat înregistrări fără date electorale în urma Join-ului, a urmat inspectarea cauzelor. S-a constatat că:
        <ul>
            În pv_RO_PRSD_FINAL:
            <li>
                sectoarele Bucureștiului au Siruta 179132
            </li>
            <li>
                orașul Băneasa din Constanța are Siruta 63171
            </li>
        </ul>
        <ul>
            În Unitate_administrativa_UAT:
            <li>
                BUCUREŞTI SECTORUL 1: 179141
            </li>
            <li>
                BUCUREŞTI SECTORUL 2: 179150
            </li>
            <li>
                BUCUREŞTI SECTORUL 3: 179169
            </li>
            <li>
                BUCUREŞTI SECTORUL 4: 179178
            </li>
            <li>
                BUCUREŞTI SECTORUL 5: 179187
            </li>
            <li>
                BUCUREŞTI SECTORUL 6: 179196
            </li>
            <li>
                BĂNEASA: 61069
            </li>
        </ul>
    </p>
    <p>
        Problema a fost remediată prin folosirea unui câmp intermediar, Siruta_Mod. A fost creat un tabel intermediar din pv_RO_UAT_VIEW, pv_RO_UAT_MOD, a fost adăugat un câmp Siruta_Mod și updatat din Siruta, dar pentru București și Băneasa au fost inserate codurile de mai sus.
    </p>
    <p>
        S-a refacut Join-ul cu Unitate_administrativa_UAT, de data asta cu pv_RO_UAT_MOD, și au rezultat 3186 rânduri populate complet.
    </p>

<h2>Rezultate</h2>
    <p>
        Fișierul rezultat în urma Join-ului a fost exportat ca ESRI shapefile, cu numele UAT_elections_round_1(respectiv UAT_elections_round_2), EPSG 3844, și encoding UTF8.
    </p>
    <p>
        În final, a fost adăugat un nou câmp pv_siruta în care sunt trecute pentru București și Băneasa codurile Siruta originale din pv_RO_PRSD_FINAL.
    </p>
    <p>
        Rezultatele au fost verificate prin query-uri în Spatialite și compararea cu rezultatele finale și cele la nivel de UAT de pe <a href="http://prezenta.bec.ro" target="_blank">Prezenta BEC</a>.
    </p>

<h2>Alte considerente</h2>
    <p>
        S-a ales agregarea datelor electorale la nivel de Siruta (natCode) deoarece s-a urmat structura din fișierul geografic de pe ANCPI, și s-a dorit folosirea unui nivel de detalii mai mare, de unde se pot generaliza datele, dacă este nevoie.
    </p>
    <p>
        Se putea folosi și agregarea la nivel de 'Siruta' ca în fișierul cu date electorale, folosind sectoarele Bucureștiului nediferențiate, prin dizolvarea geometriilor corespunzătoare din fișierul geografic.
    </p>
    <p>
        De asemenea, se putea folosi și agregarea la nivel de 'Cod birou electoral', iar atunci am fi dizolvat geometriile la nivel de județ, păstrând sectoarele Bucureștiului nedizolvate.
    </p>
    <p>
        Putem ajunge la cele două nivele de generalizare de mai sus și pornind de la fișierul UAT_elections_round_1(respectiv UAT_elections_round_2).
    </p>

<h2>Detalii despre proces</h2>
    <p>
        <a href="https://blog.maptheclouds.com/ro/tutoriale/perspectiva-spatiala-alegeri">Datele Electorale – Perspective spațiale în QGIS 3.8.3</a>
    </p>

<h2>Autor</h2>
    <p>Cristina Alexa</p>

<h2>Data</h2>
    <p>2019-12-07</p>

<hr></br>

<h1>Romania 2019 Presidential Elections - Geographic data at UAT level</h1>

<h2>Data sources</h2>
    <ul>
        <li>
            The geometries were created by ANCPI and published on <a href="http://geoportal.gov.ro" target="_blank">Geoportalul INSPIRE al României</a> and on <a href="http://geoportal.gov.ro" target="_blank">Portalul de date deschise al Guvernului României</a>.
        </li>
        <li>
            The attribute table was complemented with open data from Biroul Electoral Central on <a href="http://prezenta.bec.ro" target="_blank">Prezenta BEC</a>.
        </li>
        <li>
            The data can be reused under the <a href="http://data.gov.ro/base/images/logoinst/OGL-ROU-1.0.pdf" target="_blank">Open Government License</a>.
        </li>
    </ul>

<h2>Files</h2>
    <ul>
        <li>
            Administrative units of polygon type, from the Unitate_administrativa_UAT layer, from <a href="http://geoportal.ancpi.ro/portal/apps/webappviewer/index.html?id=faeba2d173374445b1f13512bd477bb2" target="_blank">Geoportalul ANCPI</a>.
        </li>
        <li>
            Final elections results from the first round, 10th of November 2019, from the file pv_RO_PRSD_FINAL.csv, from <a href="https://prezenta.bec.ro/prezidentiale10112019/romania-pv-final" target="_blank">Prezenta BEC 10-11-2019</a>.
        </li>
        <li>
            Final elections results from the second round, 24th of November 2019, from the file pv_RO_PRSD_FINAL.csv, from <a href="https://prezenta.bec.ro/prezidentiale10112019/romania-pv-final" target="_blank">Prezenta BEC 24-11-2019</a>.
        </li>
    </ul>

<h2>Coordinate Systems</h2>
    <ul>
        <li>
            EPSG:3844 - Pulkovo 1942(58) / Stereo70 - Projected
        </li>
        <li>
            EPSG:4326 - WGS 84 - Geographic
        </li>

<h2>Methods</h2>
    <p>
        Data processing was made in QGIS 3.8.3-Zanzibar and Spatialite (in QGIS).
    </p>
    <p>
        Geographic data and elections data were downloaded, then we proceeded to the aggregation of the elections results at UAT level (because they were at precinct level), in order to join the two data sets.
    </p>
    <p>
        The data grouping from pv_RO_PRSD_FINAL was made on the Siruta field, in Spatialite, and a temporary view was created, pv_RO_UAT_VIEW.
    </p>
    <p>
        Next, it was made the Join between Unitate_administrativa_UAT and pv_RO_UAT_VIEW, on Siruta = natCode.
    </p>
    <p>
        There have resulted records without electoral data after the Join, the inspecion of the causes followed up. It was found that:
        <ul>
            In pv_RO_PRSD_FINAL:
            <li>
                Bucharest sectors have Siruta 179132
            </li>
            <li>
                Băneasa city from Constanța has Siruta 63171
            </li>
        </ul>
        <ul>
            In Unitate_administrativa_UAT:
            <li>
                BUCUREŞTI SECTORUL 1: 179141
            </li>
            <li>
                BUCUREŞTI SECTORUL 2: 179150
            </li>
            <li>
                BUCUREŞTI SECTORUL 3: 179169
            </li>
            <li>
                BUCUREŞTI SECTORUL 4: 179178
            </li>
            <li>
                BUCUREŞTI SECTORUL 5: 179187
            </li>
            <li>
                BUCUREŞTI SECTORUL 6: 179196
            </li>
            <li>
                BĂNEASA: 61069
            </li>
        </ul>
    </p>
    <p>
        The issue was solved by using an intermediate field, Siruta_Mod. An intermediary table was created from pv_RO_UAT_VIEW, pv_RO_UAT_MOD, a field Siruta_Mod was added and updated from Siruta, but for Bucharest and Băneasa the codes above were inserted.
    </p>
    <p>
        The Join with Unitate_administrativa_UAT was made again, this time with pv_RO_UAT_MOD, and there have resulted 3186 completely populated rows.
    </p>


<h2>Results</h2>
    <p>
        The resulting file from the Join was exported as ESRI shapefile, named UAT_elections_round_1 (respectively UAT_elections_round_2), with EPSG 3844 and UTF8 encoding.
    </p>
    <p>
        Finally, a new field, pv_siruta was added, in which there are recorded the original Siruta codes for București and Băneasa from pv_RO_PRSD_FINAL.
    </p>
    <p>
        The results were checked by queries in Spatialite and cross-checking with the final results and the UAT level results from <a href="http://prezenta.bec.ro" target="_blank">Prezenta BEC</a>.
    </p>

<h2>Other considerations</h2>
    <p>
        The aggregation of the electoral data at Siruta (natCode) level was chosen because it followed the structure from the geographic file from ANCPI, and the use of a greater detail level was intended, from where the data could be generalized, if needed.
    </p>
    <p>
        The aggregation at 'Siruta' level could be used in a similar way like in the electoral data file, using the Bucharest sectors together, dissolving the corresponding geometries from the geographic file.
    </p>
    <p>
        The aggregation at 'Cod birou electoral' level could also be used, and them we would have dissolved the geometries at county level, and keep the Bucharest sectors undissolved.
    </p>
    <p>
        We could get to the two generalization levels from above starting from the UAT_elections_round_1 (respectively UAT_elections_round_2) file, too.
    </p>

<h2>Details about the process</h2>
    <p>
        <a href="https://blog.maptheclouds.com/tutorials/spatial-perspective-elections">Elections Data – Spatial Perspectives in QGIS 3.8.3</a>
    </p>

<h2>Autor</h2>
    <p>Cristina Alexa</p>

<h2>Data</h2>
    <p>2019-12-07</p>
