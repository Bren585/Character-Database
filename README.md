# Overview

This software is a set of two classes who's purpose is to be used in an interactive visual novel (an upcoming project) as a save/load system for character data. 

The classes are Database, a class used to read and write to and from an sqlite database, and Character, a compact class used for storing volatile character data during runtime. The Database class is capable of "preparing" instances of the Character class to be used for runtime processing. Character classes are capable of using their linked Database to "save" the changes made to them before destruction.

[Software Demo Video](https://youtu.be/XHD3B0O4Xi8)

# Classes

## Database

### Methods

|Method|Paramaters|Functionality|
|-|-|-|
|\_\_init\_\_|None|Connects to the sqlite database, or creates it if it does not exist. If necessary, populates the "gender" table.|
|\_\_del\_\_|None|Close the connection and commit changes.|
|execute|None|Executes an SQL command.|
|create|String name|Creates a character with defualt values with the given name.|
|delete|String name|Deletes all characters with the given name.|
|getCharacterData|String name|Returns a list of character data for a character with the given name to be used for construction of the Character class. If no character is found, returns an empty list.|
|characterExists|String name|Returns "true" if a character with name "name" is in the database. Else, false.|
|getGenderData|String gender|Collects pronouns for the given gender (of "male," "female," "none") for Character construction.|
|loadCharacters|None|Returns a list of Character objects built from the data stored in the database.|

### Members

|Member|Description|
|-|-|
|connection|An sqllite3 connection object.|
|cursor|An sqllite3 cursor object on connection.|

## Character

### Methods

|Method|Paramaters|Functionality|
|-|-|-|
|\_\_init\_\_|Database db, String name| Initialize a character from the data stored in db with the name "name," or create one if it does not exist.|
|\_\_repr\_\_|N/A|Called during the global print function.|
|save|None|Update db's values regarding this character to reflect the data stored in this object|
|refreshPronouns|None|Update this object's pronouns to reflect it's gender.|

### Members

|Member|Description|
|-|-|
|db|A reference to the main Database object.|
|name|The character's name.|
|mood|See the "characters" table.| 
|love|See the "characters" table.| 
|gender|The character's gender.| 
|pronouns|A list of the character's pronouns in the order they appear in the "genders" table.|

# Relational Database

The database is consistent of two tables which collectively describe the characteristics of characters in an interactive visual novel. 

## genders
Table "genders" is a static table prefilled with static information regarding pronouns and related grammatical structures.
|Column | Data Type | Comments |
|-|-|-|
|id|int|Primary Key|
|gender|varchar(7)|The name of the gender, i.e. "male," "female," or "none."|
|personal|varchar(7)|The Personal Pronoun, i.e. "He."|
|indirect|varchar(7)|The Indirect Pronoun, i.e. "Him"|
|dirPossesive|varchar(7)|The Direct Possesive Pronoun, i.e. "Her" in "This is her bag."|
|indPossesive|varchar(7)|The Indirect Possesive Pronoun, i.e. "Hers" in "The bag is hers."|
|article|varchar(7)|The proper article used in reference to the pronoun, i.e. "is" or "are."|
|contraction|varchar(7)|The combination of the direct pronoun and the article, i.e. "He's" or "They're."|

## Characters
Table "characters" is a nonstatic table to store character information, such as mood and gender.
|Column | Data Type | Comments |
|-|-|-|
|id|int|Primary Key|
|name|varchar(45)|The character's name. Must be unique.|
|gender|int|Foreign key connecting to the "genders" table.|
|mood|int|The mood of the character, on scale from 0-99, where 0 is a bad mood and 99 is a good mood.|
|love|int|A numerical representation of the opinion the character holds of the player character, from 0-99, where 0 is a bad opinion, 50 is nuetral, and 99 is a good opinion.|

# Development Environment

Python3 with sqlite3.

# Useful Websites

* [SQLite Documentation](http://sqlite.org/docs.html)
* [W3 Schools](http://w3schools.com)

# Future Work

* Change Character.pronouns into a dictionary
* Incorporate Database and Character classes into a future project.
