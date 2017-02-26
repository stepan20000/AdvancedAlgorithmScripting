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
###Symmetric Difference 
Create a function that takes two or more arrays and returns an array of the symmetric difference (△ or ⊕) of the provided arrays.
Given two sets (for example set A = {1, 2, 3} and set B = {2, 3, 4}), the mathematical term "symmetric difference" of two sets is the set of elements which are in either of the two sets, but not in both (A △ B = C = {1, 4}). For every additional symmetric difference you take (say on a set D = {2, 3}), you should get the set with elements which are in either of the two the sets but not both (C △ D = {1, 4} △ {2, 3} = {1, 2, 3, 4}).
```javascript
function sym() {
// A condition for exiting from a recursion
	if(!arguments[1]) {
		return arguments[0];
	}
//Convert the given arguments into the array args
	var args = Array.prototype.slice.call(arguments);
//First find the Symmetric Difference of first two arrays 
	var setA = args[0], setB = args[1];
	
//For optimization and efficiency is better to remove extra elements to make the arrays unique. But we do it only with second arrays setB
// because of recursion call later. If the function will call recursively the first array will be always Symmetric Difference so no need to
// change first array.
	setB = setB.reduce(function (acc, val) {
// if current value from setA is not found in setB (~x equivalent to the -(x+1) so if indexOf returns -1 i.e x = -1 ~x = -0 and !~x becomes true)
// push this value to the acc if acc doesn't contain equal value
		if(!~acc.indexOf(val)){
				acc.push(val);
		}
		return acc;
	}, []);
//Now setB contains only unique values

//Go through first setA and if the value is not found in setB push it to symDiffAB and if the value is found in setB remove it from setB
	var symDiffAB = setA.reduce(function (acc, val) {
		var indexInSetB = setB.indexOf(val);				
		if (!~indexInSetB) {
			if(!~acc.indexOf(val)){
				acc.push(val);
			}			
		}	
		else {
			if(~indexInSetB){			
				setB.splice(indexInSetB, 1);
			}
		}		
		return acc;
	}, []);
	symDiffAB = symDiffAB.concat(setB);
// remove the first element from args (first array from arguments)
	args.shift();
// replace the first element(the second in original arguments) with the symmetric difference of two first arguments
	args[0] = symDiffAB;
// Recursive call the function sym with the new shorter set of arguments
	return sym.apply(null, args);
}

```
###Exact Change
Design a cash register drawer function checkCashRegister() that accepts purchase price as the first argument (price), payment as the second argument (cash), and cash-in-drawer (cid) as the third argument.
cid is a 2D array listing available currency.
Return the string "Insufficient Funds" if cash-in-drawer is less than the change due. Return the string "Closed" if cash-in-drawer is equal to the change due.
Otherwise, return change in coin and bills, sorted in highest to lowest order.
```javascript
function checkCashRegister(price, cash, cid) {
//Will calculate all values in cents because of float accuracy
	var nominals = { "PENNY": 1, "NICKEL": 5, "DIME": 10, "QUARTER": 25, "ONE": 100, "FIVE": 500, "TEN": 1000, "TWENTY": 2000, "ONE HUNDRED": 10000};
	var change = [];
	function findCidSumInCents(cid) {
		var cidSumInner = 0;
		cid.forEach(function(val) {
			val[1] = val[1] * 100;
			cidSumInner = cidSumInner + val[1];
		});
		return cidSumInner;
	}
	price = price * 100;
	cash = cash * 100;
	var changeDue = cash - price;
	var cidSum = findCidSumInCents(cid);
	if (changeDue > cidSum) {
		return "Insufficient Funds";
	}
	else {
		for(var i = cid.length - 1; i >= 0; i--){
			if(changeDue > nominals[cid[i][0]] && cid[i][1] > 0 ){
				if (changeDue  <= cid[i][1]) {
					let givenNominal = Math.floor(changeDue / nominals[cid[i][0]]) * nominals[cid[i][0]];
					changeDue = changeDue - givenNominal;
					cid[i][1] = cid[i][1] - givenNominal;
					change.push([cid[i][0], givenNominal / 100]); 
				}
				else {
					changeDue = changeDue - cid[i][1];
					change.push([cid[i][0], cid[i][1] / 100]);
					cid[i][1] = 0;
				}
			}
		}
		if (changeDue > 0) {
			return "Insufficient Funds";
		}
		else if (findCidSumInCents(cid) == 0) {
			return "Closed";
			}
			else {
				return change;
			}
	}
}
```
###Inventory Update 
Compare and update the inventory stored in a 2D array against a second 2D array of a fresh delivery. Update the current existing inventory item quantities (in arr1). If an item cannot be found, add the new item and quantity into the inventory array. The returned inventory array should be in alphabetical order by item.
```javascript
function updateInventory(arr1, arr2) {
	var found;
    for(var i = 0, n = arr2.length; i < n; i++){
    	found = false;
    	for(var j = 0, m = arr1.length; j < m; j++) {
  			if (arr1[j][1] < arr2[i][1]) {
  				continue;
  			}
  			else if (arr1[j][1] == arr2[i][1]) {
  				found = true;
                arr1[j][0] += arr2[i][0];
  				break;
  			}
  			else {
  				break;
  			}	  
    	}
    	if(!found) {
    		arr1.splice(j, 0, arr2[i]);
    	}
    }
    return arr1;
}
```
###No repeats please
Return the number of total permutations of the provided string that don't have repeated consecutive letters. Assume that all characters in the provided string are each unique.
For example, aab should return 2 because it has 6 total permutations (aab, aab, aba, aba, baa, baa), but only 2 of them (aba and aba) don't have the same letter (in this case a) repeating.
```javascript
function permAlone(str){
// Create a regex to match repeated consecutive characters.
	var re = /(.)\1+/g;
// Split the string into an array of characters.
  	var arr = str.split('');
	var count = 0;
// function for swapping two elements of an array
	function swap(index1, index2, arr){
		var temp = arr[index2];
		arr[index2] = arr[index1];
		arr[index1] = temp;	
	}
// function for creating all permutation according to the Heap's algorithm
	function heap(n, arr) {
		for(var i = 0; ; i++){
			if (n > 2) {
				heap(n-1, arr);
			}
			if(i == n - 1){
				break;
			}
			if (n % 2 == 0) {                        
				swap(i, n -1, arr);
				if (!arr.join("").match(re)) {
					count++;
				}
			}
			else {
				swap(0, n - 1, arr);
				if (!arr.join("").match(re)) {
					count++;
				}
			}
		}
	}
//First checking for repeating letters given string 
	if (!str.match(re)) {
		count++;
	}
	heap(arr.length, arr);
	return count;
}
```
###Friendly Date Ranges 
Convert a date range consisting of two dates formatted as YYYY-MM-DD into a more readable format.
The friendly display should use month names instead of numbers and ordinal dates instead of cardinal (1st instead of 1).
Do not display information that is redundant or that can be inferred by the user: if the date range ends in less than a year from when it begins, do not display the ending year.
Additionally, if the date range begins in the current year (i.e. it is currently the year 2016) and ends within one year, the year should not be displayed at the beginning of the friendly range.
If the range ends in the same month that it begins, do not display the ending year or month.
Examples:
makeFriendlyDates(["2016-07-01", "2016-07-04"]) should return ["July 1st","4th"]
makeFriendlyDates(["2016-07-01", "2018-07-04"]) should return ["July 1st, 2016", "July 4th, 2018"].
```javascript
function makeFriendlyDates(arr) {
	function addMonth(line) {
		var months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
		reply[line] = reply[line].concat(months[initial[line].getMonth()] + " ");
	}
	function addDate(line) {
		if (initial[line].getDate() % 10 == 1)  {
			reply[line] = reply[line].concat(initial[line].getDate() + "st");
		}
		else if(initial[line].getDate() % 10 == 2) {
			reply[line] = reply[line].concat(initial[line].getDate() + "nd");
		}
		else if(initial[line].getDate()  == 3 || initial[line].getDate()  == 23){
			reply[line] = reply[line].concat(initial[line].getDate() + "rd");
		}
		else {
			reply[line] = reply[line].concat(initial[line].getDate() + "th");
		}
	}
	function addYear(line) {
		reply[line] = reply[line].concat(", " + initial[line].getFullYear());
	}
	
	var reply = ["", ""];
	var today = 2016 /* replace 2016 with new Date(). 2016 is because of the current year on FreeCoreCamp task verifier was 2016 */;
	var start = 0, finish = 1;
	var initial = [new Date(arr[0]), new Date(arr[1])];
	var milSecInYear = 365 * 24 * 60 * 60 * 1000;  
	
	addMonth(start);
	addDate(start);
	if (initial[start].valueOf() == initial[finish].valueOf()) {
		addYear(start);
		reply.pop();
		return reply;
	} 
	if (initial[finish] - initial[start] < milSecInYear){
		if (initial[start].getFullYear() == today) {
			if (initial[start].getMonth() == initial[finish].getMonth()) {
				addDate(finish);
				
			}
			else {
				addMonth(finish);
				addDate(finish);
			}		
		}
		else {
			addYear(start);
			addMonth(finish);
			addDate(finish);
			
		}
	}
	else {
		addYear(start);
		addMonth(finish);
		addDate(finish);
		addYear(finish);
	}
  	return reply;
}
```
###Make a Person 
Fill in the object constructor with the following methods below:

    getFirstName()
    getLastName()
    getFullName()
    setFirstName(first)
    setLastName(last)
    setFullName(firstAndLast)

Run the tests to see the expected output for each method.
The methods that take an argument must accept only one argument and it has to be a string.
These methods must be the only available means of interacting with the object.
```javascript
var Person = function(firstAndLast) {
    var re = /([A-Za-z]+)/g;
    
    this.getFirstName = function() {
		return firstAndLast.match(re)[0];
    };
    this.getLastName = function() {
		return firstAndLast.match(re)[1];  
    };
    this.getFullName = function() {
		return  this.getFirstName() + " " + this.getLastName();   
    };
    this.setFirstName = function(first) {
		this.getFirstName = function (){
			return first;
        };    
    };
    this.setLastName = function(last) {
		this.getLastName = function(){
			return last;
        };    
    };
    this.setFullName = function(firstAndLast) {
      let first = firstAndLast.match(re)[0];
      let last = firstAndLast.match(re)[1];
      this.setFirstName(first);
      this.setLastName(last);
    };

};
```
###Map the Debris Incomplete
```javascript

```
###Pairwise
```javascript

```
