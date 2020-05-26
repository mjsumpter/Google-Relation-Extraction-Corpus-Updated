# Augmented Google Relation Extraction Corpus
[Original Google Blogpost describing the dataset](https://ai.googleblog.com/2013/04/50000-lessons-on-how-to-read-relation.html)

[Link to the original dataset](https://code.google.com/archive/p/relation-extraction-corpus/)

This is a fairly robust dataset for building out relation extraction solutions, but was somewhat out of date. I've added (and will continue adding) additional properties to the .json objects to make future use of this dataset more useful. 


## Files

* **dob-augment-.json** : SUBJECT was born on OBJECT (date)

* **pob-augment-.json** : SUBJECT was born in OBJECT (location)

* **pod-augment-.json** : SUBJECT died in OBJECT (location)

* **education-augment-.json** : SUBJECT received OBJECT (Degree)

* **institution-augment-.json** : SUBJECT attended OBJECT (university)

* **augment_grec.py**: The script used to augment the original dataset

## Data Structure
**Changes from original dataset are bolded**

Each dataset is a **JSON** file (was previously a .ndjson). **Each snippet was converted from unicode to ASCII** Each example has the following properties:

* _pred_: Freebase label for predicate. Unaltered, as the files are divided this way, and every item will have the same tag

* ***sub***: A string representing the subject. These were generated in one of two ways: 1) finding a match of the Freebase MID in the Google Knowledge Graph API, 2) If not found with (1), then used the _evidence_ URL to pull the subject name

* ***obj***: A string representing the object. These were generated by finding a match of the Freebase MID in the Google Knowledge Graph

* _evidences_: The evidence for the relation

  * _url_: The URL the snippet was pulled from
  
  * _snippet_: The text snippet that was judged to contain the relation
  
* _judgements_: A list of the 5 human assessments of the snippet to contain the relation

  * _rater_: hashed id of the person providing the rating
  
  * _judgement_: The decision of the rater. Can be 'yes', 'no', or 'skip'
  
* ***UID***: A unique ID for that specific sample. Prefix found before underscore identifies the type of relation

* ***maj_vote***: The majority vote of the 5 raters. Can be 'yes', 'no', or 'skip'

* _sub_id_: The Freebase MID from the original dataset (originally _sub_)

* _obj_id_: The Freebase MID from the original dataset (originally _obj_)

* ***dbpedia_sub***: The DBPedia URI relating to the subject

* ***dbpedia_obj***: The DBPedia URI relation to the object

* ***wikidata_qid_sub***: The Wikidata QID relating to the subject

* ***wikidata_qid_obj***: The Wikidata QID relating to the object

## Things to be done

* Some object and subject MIDs have not been matched to a string (the above methods failed). In this case, _sub_ and _obj_ will have the original Freebase MID, followed by "/ needs_entry"

* Some dbpedia URIs were not found. Date of Birth items do not have a DBPedia URI. All others should have a valid mapping, but currently _dbpedia_sub_ and _dbpedia_obj_ can take the value "Not Found"

* Wikidata QIDs were also added but with the same stipulations as DBpedia above

### Before
```
{
    "pred":"/people/person/education./education/education/institution",
    "sub":"/m/056h7f",
    "obj":"/m/0lk0l",
    "evidences":[
            {
            "url":"http://en.wikipedia.org/wiki/Charles_Mantoux",
            "snippet":"Charles Mantoux graduated from the University of Paris where he studied under Broca. For health reasons he relocated to Cannes but continued to work in Paris during the long vacation periods granted to patients in sanatoriums."
            }
        ],
        "judgments":[
            {
                "rater":"16671464637895630418",
                "judgment":"yes"
            },
            {
                "rater":"18153321702395861849",
                "judgment":"yes"
            },
            {
                "rater":"6570687721363324718",
                "judgment":"yes"
            },
            {
                "rater":"1295059937994543754",
                "judgment":"yes"
            },
            {
                "rater":"12022408018620867151",
                "judgment":"yes"
            }
        ]
}
```

### After
```
{
    "pred": "/people/person/education./education/education/institution",
    "sub": "Charles Mantoux",
    "obj": "University of Paris",
    "evidences": [
      {
        "url": "http://en.wikipedia.org/wiki/Charles_Mantoux",
        "snippet": "Charles Mantoux graduated from the University of Paris where he studied under Broca. For health reasons he relocated to Cannes but continued to work in Paris during the long vacation periods granted to patients in sanatoriums."
      }
    ],
    "judgments": [
      {
        "rater": "16671464637895630418",
        "judgment": "yes"
      },
      {
        "rater": "18153321702395861849",
        "judgment": "yes"
      },
      {
        "rater": "6570687721363324718",
        "judgment": "yes"
      },
      {
        "rater": "1295059937994543754",
        "judgment": "yes"
      },
      {
        "rater": "12022408018620867151",
        "judgment": "yes"
      }
    ],
    "UID": "i_PX5AbGVlRD",
    "maj_vote": "yes",
    "sub_id": "/m/056h7f",
    "obj_id": "/m/0lk0l",
    "dbpedia_sub": "http://dbpedia.org/resource/Charles_Mantoux",
    "dbpedia_obj": "http://dbpedia.org/resource/University_of_Paris",
    "wikidata_qid_sub": "Q924088",
    "wikidata_qid_obj": "Q209842"
}
```

