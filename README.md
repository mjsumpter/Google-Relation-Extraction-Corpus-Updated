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

Each dataset is a **properly-formatted**  JSON file (was previously a .ndjson). **Each snippet was converted from unicode to ASCII** Each example has the following properties:

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

## Things to be done

* Some objects and subjects have not been matched to a string (the above methods failed). In this case, _sub_ and _obj_ will have the original Freebase MID, followed by "/ needs_entry"

* Some dbpedia URIs were not found. Date of Birth items do not have a DBPedia URI. All others should have a valid mapping, but currently _dbpedia_sub_ and _dbpedia_obj_ can take the value "Not Found"

* I would like to add the Wikidata QID for each subject and object
