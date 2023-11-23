---
author: JB
title: JavaScript array equivalents to C# LINQ methods
date: 2023-03-24
description: Great reference for translating from C# LINQ to JS
image: "logos/js-logo.png"
categories: [ "javascript", "dotnet" ]
tags: [ "c-sharp", "javascript", "js" ]
---

Ref: https://gist.github.com/DanDiplo/30528387da41332ff22b

[Dan Booth](https://www.diplo.co.uk/) has made this great reference containing JavaScript array equivalents to C# LINQ methods. \
Really useful for us .NET devs when we're not writing our native language!

```javascript
// JS array equivalents to C# LINQ methods - by Dan B.

// First: This version using older JavaScript notation for universal browser support (scroll down for ES6 version):

// Here's a simple array of "person" objects
var people = [ 
	{ name: "John", age: 20 }, 
	{ name: "Mary", age: 35 }, 
	{ name: "Arthur", age: 78 }, 
	{ name: "Mike", age: 27 }, 
	{ name: "Judy", age: 42 }, 
	{ name: "Tim", age: 8 } 
];

// filter is equivalent to Where

var youngsters = people.filter(function (item) {
	return item.age < 30;
});

console.log("People younger than 30:", youngsters);


// map is equivalent to Select

var names = people.map(function (item) {
	return item.name;
});

console.log("Just the names of people:", names);


// every is equivalent to All

var allUnder40 = people.every(function (item) {
	return  item.age < 40;
});

console.log("Are all people under 40?", allUnder40); // false


// some is equivalent to Any

var anyUnder30 = people.some(function (item) {
	return  item.age < 30;
});

console.log("Are any people under 30?", anyUnder30); // true


// reduce is "kinda" equivalent to Aggregate (and also can be used to Sum)

var aggregate = people.reduce(function (item1, item2) {
	return  { name: '', age: item1.age + item2.age };
});

console.log("Aggregate age", aggregate.age);  // { age: 210 }


// sort is "kinda" like OrderBy (but it sorts the array in place - eek!)

var orderedByName = people.sort(function (a, b) {
	return  a.name < b.name ? 1 : -1;
})

console.log("Ordered by name:", orderedByName); 


// and, of course, you can chain function calls 

var namesOfPeopleOver30OrderedDesc = people.filter(function (person) {
	return person.age > 30;
}).
map(function (person) {
	return person.name;
}).
sort(function (a, b) {
	return a > b ? 1 : -1;
});

console.log("And now.. the names of all people over 30 ordered by name descending:", namesOfPeopleOver30OrderedDesc);



// Second: And now the more modern ES6 way of doing this using arrow functions (lambdas!)...

const peoples = [ 
	{ name: "John", age: 20 }, 
	{ name: "Mary", age: 35 }, 
	{ name: "Arthur", age: 78 }, 
	{ name: "Mike", age: 27 }, 
	{ name: "Judy", age: 42 }, 
	{ name: "Tim", age: 8 } 
];

// filter is equivalent to Where

const youngPeople = peoples.filter(p => p.age < 30);

console.log("People younger than 30:", youngPeople);


// map is equivalent to Select

const justNames = peoples.map(p => p.name);

console.log("Just the names of people:", justNames);

// every is equivalent to All

const peopleUnder40 = peoples.every(p => p.age < 40);

console.log("Are all people under 40?", peopleUnder40); // false


// some is equivalent to Any

const areAnyUnder30 = peoples.some(p => p.age < 30);

console.log("Are any people under 30?", areAnyUnder30); // true


// reduce is "kinda" equivalent to Aggregate (and also can be used to Sum)

const aggregatedAge = peoples.reduce((p1, p2) => {
	return  { name: '', age: p1.age + p2.age }
});

console.log("Aggregate age:", aggregatedAge.age);  // { age: 210 }


// sort is "kinda" like OrderBy (but it sorts the array in place - eek!)

const peopleOrderedByName = peoples.sort((p1, p2) => p1.name < p2.name ? 1 : -1);

console.log("Ordered by name:", peopleOrderedByName); 


// and, of course, you can chain function calls 

const peepsOver30OrderedDesc = peoples.filter(p => p.age > 30).map(p => p.name).sort((p1, p2) => p1 > p2 ? 1 : -1);

console.log("Chained", peepsOver30OrderedDesc);
```
