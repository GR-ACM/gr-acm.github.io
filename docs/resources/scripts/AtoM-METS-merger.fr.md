# Intégration des métadonnées d'Archivematica à AtoM

[:material-account: Seibt, Yonathan Jérôme](https://infoscience.epfl.ch/entities/person/54236519-4a6b-461d-a338-60e3627f13d0)  
:material-calendar: Décembre 2024  
:material-office-building: Archives de la construction moderne – EPFL

*Ce package est distribué sous la licence GNU General Public License v3. Vous pouvez le redistribuer et/ou le modifier selon les termes de la GNU GPL tels que publiés par la Free Software Foundation, soit la version 3 de la licence, soit (à votre choix) toute version ultérieure.*  
*Ce programme est distribué dans l’espoir qu’il sera utile, mais SANS AUCUNE GARANTIE ; sans même la garantie implicite de QUALITÉ MARCHANDE ou D’ADÉQUATION À UN USAGE PARTICULIER. Voir la GNU General Public License pour plus de détails.*  
*Pour obtenir une copie de la GNU General Public License, consultez [www.gnu.org/licenses/](https://www.gnu.org/licenses/).*

---

## Objectif

Ce package automatise l’intégration des métadonnées générées par Archivematica (1.17.0) dans AtoM (2.8.2), en garantissant une synchronisation fluide et des mises à jour cohérentes entre les deux systèmes. Il fournit un flux de travail structuré pour :

1.	Extraction et transformation des données : Fusion des métadonnées des fichiers METS (XML) générés par Archivematica avec les notices exportées d’AtoM (CSV).
2.	Mise à jour automatisée des notices dans AtoM : Injection des métadonnées enrichies via des requêtes HTTP.

---

## Contenu

**Scripts Python**

- `AtoM_METS_Data_Merger.py` : Analyse et fusionne les métadonnées provenant de METS (Archivematica) et CSV (AtoM).
- `AtoM_Record_Updater.py` : Met à jour les notices dans AtoM avec les métadonnées enrichies via l’interface web.

**Fichiers de données**

- `isad_0000000001.csv` : Fichier d’export AtoM contenant les notices existantes.
- `METS_123456789.xml` : Fichier METS produit par Archivematica contenant les métadonnées structurées.
- `login.csv` : Identifiants nécessaires pour accéder à l’interface web d’AtoM.
- `urls.csv` : Liste des URLs ou des UUID nécessaires pour localiser ou synchroniser les données.

---

## Prérequis techniques

- **Systèmes requis**

    - Archivematica : Version 1.17.0, pour produire des fichiers METS.
    - AtoM : Version 2.8.2

- **Environnement Python, version 3.8+ avec les bibliothèques suivantes**

    - pandas : Manipulation des fichiers CSV.
    - xml.etree.ElementTree : Analyse des fichiers XML.
    - requests : Gestion des requêtes HTTP.
    - BeautifulSoup (bs4) : Navigation et interaction avec les formulaires web.

## Pourquoi ce script est nécessaire ?

Lorsqu'un DIP généré par Archivematica est poussé vers AtoM, la structure hiérarchique des dossiers disparaît. Tous les fichiers se retrouvent regroupés sans distinction de leur organisation d'origine. Cette limitation est documentée dans les communautés Archivematica/AtoM.

Ce script permet de recréer une correspondance logique entre :

- Les notices AtoM existantes (exportées via le Clipboard en CSV)
- Les fichiers du DIP (et leurs métadonnées, extraites du fichier METS)

Le fichier résultant `urls.csv` peut être utilisé par un second script pour injecter automatiquement les métadonnées enrichies dans AtoM.

---

## Étapes d'utilisation

### Vérifier le nom du fichier METS

Ouvrez le script et vérifiez que la variable contenant le nom du fichier METS correspond bien à votre fichier. Par défaut, cela pourrait ressembler à :

```py
mets_filename = "METS_123456789.xml"
```

Modifiez cette valeur si nécessaire.

### Ouvrir l’invite de commande et se placer dans le bon dossier

```bash
cd "C:\Users\votre.nom\Chemin\Vers\Le\Dossier"
```

Ce dossier doit contenir :

- le fichier METS
- le fichier CSV AtoM
- le script Python

### Exécutez le script

```bash
python AtoM_METS_Data_Merger.py
```

Si tout s’est bien passé, le message suivant s'affiche :

```bash
Le fichier 'urls.csv' a été créé avec succès.
```

### Résultat attendu

Colonnes incluses dans `urls.csv` :

| Colonne               | Description                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------- |
| `urls`                | Deux liens vers chaque notice AtoM (version FR et EN), utilisés pour appliquer les modifications. |
| `titre`               | Titre reconstruit à partir du nom du fichier.                                                     |
| `editEvents_0_type`   | Type d’événement à enregistrer, ex : `"creation"`.                                                |
| `dateCreation`        | Date de création du fichier, extraite du METS.                                                    |
| `levelOfDescription`  | Niveau de description ISAD-G, ex : `"item"`.                                                      |
| `locationOfOriginals` | Chemin d’accès original au fichier dans le DIP.                                                   |
| `extentAndMedium`     | Type et taille du fichier (ex. : PDF, 512 Ko).                                                    |
| `identifier`          | Cote ISAD-G générée pour le fichier.                                                              |

---

## Étapes suivantes

Vous pouvez maintenant utiliser le fichier urls.csv avec le script `AtoM_Record_Updater` pour injecter automatiquement les métadonnées dans les notices AtoM via leur interface web.

---

## Script `AtoM_METS_Data_Merger.py`

```py
# Script: AtoM_METS_Data_Merger.py
# Description: This script extracts and merges metadata from AtoM (CSV) and METS (XML) files. It standardizes, parses, and combines data
# into a unified, enriched CSV file ready for further processing or import into AtoM.
#
# Author: Yonathan Seibt, Archives de la construction moderne – EPFL
# Date: November 2024
#
# License: This script is distributed under the GNU General Public License v3. You may redistribute and/or modify it under the terms 
#          of the GNU GPL as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
#
#          This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty 
#          of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
#
#          You should have received a copy of the GNU General Public License along with this program. If not, see <https://www.gnu.org/licenses/>.

import pandas as pd
import re
import xml.etree.ElementTree as ET
from datetime import datetime

# ---- Partie 1 : Extraire les UUID du fichier CSV ----

# Lire le fichier CSV
df_csv = pd.read_csv('isad_0000000001.csv')  # Remplacez par le nom de votre fichier CSV

# Ajouter une colonne d'index pour conserver l'ordre
df_csv['index'] = df_csv.index

# Fonction pour extraire l'UUID
def extract_uuid(url):
    if isinstance(url, str):
        match = re.search(r'/([0-9a-fA-F-]{36})-', url)
        if match:
            return match.group(1)
    return None

# Appliquer la fonction à la colonne contenant les URLs
df_csv['UUID'] = df_csv['digitalObjectURI'].apply(extract_uuid)  # Remplacez par le nom de votre colonne

# ---- Partie 2 : Extraire les données du fichier XML ----

# Charger le fichier XML
xml_file = 'METS.07fdd110-6ae2-49c4-989d-6394c152be9c.xml'
tree = ET.parse(xml_file)
root = tree.getroot()

# Définir l'espace de noms utilisé dans le fichier XML
ns = {'mets': 'http://www.loc.gov/METS/'}

# Fonction pour extraire toutes les données des balises <mets:amdSec> et les structurer
def extract_all_amdSec_data(root, ns):
    amdSec_data = []
    
    # Trouver toutes les balises <mets:amdSec>
    for amdSec in root.findall('.//mets:amdSec', ns):
        data = {}
        
        # Extraire l'attribut 'ID' de la balise <mets:amdSec>
        data['ID'] = amdSec.attrib.get('ID', None)

        # Extraire toutes les sous-balises pertinentes
        for element in amdSec.iter():
            # Si l'élément est une balise avec un texte (et non l'attribut)
            if element.tag != '{http://www.loc.gov/METS/}amdSec' and element.text:
                # Enlever l'espace de noms de la balise
                tag = element.tag.split('}')[1] if '}' in element.tag else element.tag
                data[tag] = element.text.strip()
        
        # Ajouter les données extraites à la liste
        amdSec_data.append(data)
    
    return amdSec_data

# Extraire toutes les données des balises <mets:amdSec>
amdSec_data = extract_all_amdSec_data(root, ns)

# Créer un DataFrame avec les données extraites
df_xml = pd.DataFrame(amdSec_data)

# Filtrer les colonnes non pertinentes
columns_to_keep = [
    'ID', 'objectIdentifierValue', 'size', 'messageDigestAlgorithm', 'messageDigest', 'formatName', 'formatVersion', 
    'formatRegistryKey', 'dateCreatedByApplication', 'created', 
    'creatingApplicationName', 'FileName', 'FileType', 'FileTypeExtension', 'MIMEType', 'originalName'
]

# Garder uniquement les colonnes pertinentes
df_filtered = df_xml[columns_to_keep]

# ---- Partie 3 : Fusionner les données des deux DataFrames ----

# Fusionner les deux DataFrames sur les colonnes correspondantes
# Ici, on suppose que les noms des colonnes à fusionner sont respectivement 'objectIdentifierValue' et 'UUID'
df_merged = pd.merge(df_filtered, df_csv, left_on='objectIdentifierValue', right_on='UUID', how='inner')

# Réordonner le DataFrame fusionné selon l'index original
df_merged = df_merged.sort_values(by='index')

# ---- Partie 4 : Transformation et duplication des données ----

# Initialiser les nouvelles colonnes
df_merged['urls'] = 'https://morphe-test.epfl.ch/index.php/' + df_merged['slug'] + '/edit'
df_merged['titre'] = df_merged['title'] + ' (fichier numérique)'
df_merged['editEvents_0_type'] = 'creation'

# Corriger le format de date
def format_date(date_str):
    try:
        return datetime.strptime(date_str, '%Y-%m-%dT%H:%M:%SZ').strftime('%Y-%m-%d')
    except ValueError:
        return ''

df_merged['dateCreation'] = df_merged['dateCreatedByApplication'].apply(format_date)
df_merged['levelOfDescription'] = 'item'
df_merged['scopeAndContent'] = 'Chemin d’accès du fichier sur le support original : ' + df_merged['originalName'].str.replace('%transferDirectory%objects/', '')
df_merged['extentAndMedium'] = '1 fichier numérique ' + df_merged['formatName'] + ' de ' + df_merged['size'] + ' octets'
df_merged['identifier'] = ['0219.01.0130/04.01.01.' + str(i+1).zfill(2) for i in range(len(df_merged))]

# Dupliquer chaque enregistrement avec modification de la colonne urls
df_duplicated = df_merged.copy()
df_duplicated['urls'] = 'https://morphe-test.epfl.ch/index.php/' + df_duplicated['slug'] + '/edit?sf_culture=fr&template=isad'

# Combiner les DataFrames
df_final = pd.concat([df_merged, df_duplicated], ignore_index=True)

# Réordonner le DataFrame final selon l'index original
df_final = df_final.sort_values(by='index')

# Sélectionner les colonnes nécessaires
columns_to_keep = ['urls', 'titre', 'editEvents_0_type', 'dateCreation', 'levelOfDescription', 'scopeAndContent', 'extentAndMedium', 'identifier']
df_final = df_final[columns_to_keep]

# Sauvegarder dans le fichier CSV final
output_file = 'urls.csv'
df_final.to_csv(output_file, index=False)

print(f"Le fichier '{output_file}' a été créé avec succès.")
```