
# OlyGraph and Olympics Web Application
This project is about converting a plain CSV data into a linked data where all the entities are linked to each other and then creating a web application which will use that linked data .
OlyGraph is the ontology which has been used in this project to map different entities and relation . The knowledge Graph consist triples in the form of subject , predicate and object  where the predicate define the relationship between the subject and the object . After creation of the knowledge graph , we use the following knowledge graph into our django based web application as database using SPARQLWrapper and use sparql queries to retrieve data from the knowledge graph . 
## URL

* [https://olympicsapplication.herokuapp.com/](https://olympicsapplication.herokuapp.com/)
## Authors
***
* Sarika Jain 
* Nandana Mihindukulasooriya
* Pooja Harde 
* Ankush Bisht 

## Data Source
***
*   [Olympics Data in CSV](https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results)
*   [RDF Turtle File](https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results)


##  Libraries Used
****
* [Django](https://www.djangoproject.com/) - It is an free and Open-Source Web Development Framework in python which is used to create the backend of any application .
* [SPARQLWrapper](https://libraries.io/pypi/SPARQLWrapper) - It is an Open-Source python based library which is  used to query rdf data in python .
* [Tailwind CSS](https://tailwindcss.com/) - This is a CSS based library used to beautify your html document using pre-defined CSS classes .
* [YASQE](https://github.com/pkleef/YASGUI.YASQE) - YASQE (Yet Another SPARQL Query Editor) is part of the the YASGUI suite of SPARQL tools. .

## SPARQLWrapper
****
It is the python based library used to connect your turtle file with django backend so that we can query the data inside the triple using python .

### Sample Code
```python 
from SPARQLWrapper import SPARQLWrapper, JSON

sparql = SPARQLWrapper(
    "http://vocabs.ardc.edu.au/repository/api/sparql/"
    "csiro_international-chronostratigraphic-chart_geologic-time-scale-2020"
)
sparql.setReturnFormat(JSON)

# gets the first 3 geological ages
# from a Geological Timescale database,
# via a SPARQL endpoint
sparql.setQuery("""
    PREFIX gts: <http://resource.geosciml.org/ontology/timescale/gts#>

    SELECT *
    WHERE {
        ?a a gts:Age .
    }
    ORDER BY ?a
    LIMIT 3
    """
)

try:
    ret = sparql.queryAndConvert()

    for r in ret["results"]["bindings"]:
        print(r)
except Exception as e:
    print(e)

```




## Sparql 
Few examples of the sparql queries that we have used in our project are - 
1. Top 10 Oldest athlete 
![App Screenshot](https://i.ibb.co/ggkj2w0/1.png)

## Environment Variables
To run this project, you will need to add the following environment variables to your .env file
- SECRET_KEY= Secret key of your django project
- DEBUG = "False"
- ALLOWED_HOSTS = ""

## Run The Project Locally 
Clone the project

```bash
  git clone https://github.com/ankushbisht01/120-years-olympics
```

Go to the project directory

```bash
  cd 120-years-olympics
```

Install dependencies

```bash
  pip install -r requirements.txt
```

Start the server

```bash
  python manage.py runserver
```


## Acknowledgements
This work is supported by the IHUB-ANUBHUTI-IIITD FOUNDATION set up under the NM-ICPS scheme of the Department of Science and Technology, India.


## Contacts
### Dr. Sarika Jain  (National Institute of Technology, Kurukshetra, India ) 
-  Email - jasarika@nitkkr.ac.in

### Nandana Mihindukulasooriya (IBM Research, Dublin, Ireland) 
-  Email - nandana@ibm.com

### Pooja Harde (National Institute of Technology, Kurukshetra, India ) 
-  Email - pmharde29@gmail.com

### Ankush Bisht (University of Delhi, India ) 
-  Email - ankushbisht72@gmail.com


