# Advanced Algorithm Scripting
Collection of scripts from Free Code Camp's Advanced Algorithm Scripting Section

##Names of scripting challenges:

###Validate US Telephone Numbers Incomplete
Return true if the passed string is a valid US phone number.
The user may fill out the form field any way they choose as long as it is a valid US number. The following are examples of valid formats for US numbers (refer to the tests below for other variants):

    555-555-5555
    (555)555-5555
    (555) 555-5555
    555 555 5555
    5555555555
    1 555 555 5555

For this challenge you will be presented with a string such as 800-692-7753 or 8oo-six427676;laskdjf. Your job is to validate or reject the US phone number based on any combination of the formats provided above. The area code is required. If the country code is provided, you must confirm that the country code is 1. Return true if the string is a valid US phone number; otherwise return false.
```javascript
function telephoneCheck(str) {
	var re = new RegExp(/^1?\s?\d{3}\s?\d{3}[-\s]?\d{4}$|^1?\s?\(\d{3}\)\s?\d{3}[-\s]?\d{4}$|^1?\s?\d{3}\-\s?\d{3}[-\s]?\d{4}$/,"");	
	if (str.search(re) == -1) {
		return false;
	}
	else {
		return true;
	}
}
```
###Record Collection
You are given a JSON object representing a part of your musical album collection. Each album has several properties and a unique id number as its key. Not all albums have complete information.
Write a function which takes an album's id (like 2548), a property prop (like "artist" or "tracks"), and a value (like "Addicted to Love") to modify the data in this collection.
If prop isn't "tracks" and value isn't empty (""), update or set the value for that record album's property.
Your function must always return the entire collection object.
There are several rules for handling incomplete data:
If prop is "tracks" but the album doesn't have a "tracks" property, create an empty array before adding the new value to the album's corresponding property.
If prop is "tracks" and value isn't empty (""), push the value onto the end of the album's existing tracks array.
If value is empty (""), delete the given prop property from the album.
```javascript
var collection = {
    "2548": {
      "album": "Slippery When Wet",
      "artist": "Bon Jovi",
      "tracks": [ 
        "Let It Rock", 
        "You Give Love a Bad Name" 
      ]
    },
    "2468": {
      "album": "1999",
      "artist": "Prince",
      "tracks": [ 
        "1999", 
        "Little Red Corvette" 
      ]
    },
    "1245": {
      "artist": "Robert Palmer",
      "tracks": [ ]
    },
    "5439": {
      "album": "ABBA Gold"
    }
};
// Keep a copy of the collection for tests
var collectionCopy = JSON.parse(JSON.stringify(collection));

// Only change code below this line
function updateRecords(id, prop, value) {
  if(value === "") delete collection[id][prop];
  else if(prop === "tracks") 
       {
         if(collection[id].hasOwnProperty("tracks"))
         {
           collection[id].tracks.push(value);
         }
         else 
         {
           var ar = [value];
           collection[id].tracks = ar;
         }
       }
       else 
       {
         collection[id][prop] = value;
       }
  
  return collection;
}
```
###Symmetric Difference Incomplete
```javascript

```
###Exact Change Incomplete
```javascript

```
###Inventory Update Incomplete
```javascript

```
###No repeats please Incomplete
```javascript

```
###Friendly Date Ranges Incomplete
```javascript

```
###Make a Person Incomplete
```javascript

```
###Map the Debris Incomplete
```javascript

```
###Pairwise
```javascript

```
