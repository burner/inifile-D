inifile-D
=========

[![Build Status](https://travis-ci.org/burner/inifiled.svg?branch=master)](https://travis-ci.org/burner/inifiled)

A compile time ini file parser and writter generator for D.
inifile.d takes annotated structs and create ini file parser and writer.
The ini file format always comments and section and to some degree nesting.

Example
-------

```d
import initest;

import inifiled;

@INI("A child must have a parent") 
struct Child {
	@INI("The firstname of the child") string firstname;

	@INI("The age of the child") int age;
}

@INI("A Spose") 
struct Spose {
	@INI("The age of the spose") int age;

	// Nesting
	@INI("The House of the spose") House house;
}

@INI("A Dog") 
struct Dog {
	@INI("The name of the Dog") string name;
}

@INI("A Person") 
struct Person {
	@INI("The lastname of the Person") string lastname;

	@INI("The height of the Person") float height;

	@INI("Some strings with a very long long INI description that is longer" ~
		" than eigthy lines hopefully."
	) string[] someStrings;

	int dontShowThis;

	// REGARD the nesting
	@INI("A Spose") Spose spose;

	// REGARD the nesting
	@INI("The family dog") Dog dog;
}

@INI("A House")
struct House {
	@INI("Number of Rooms") uint rooms;

	@INI("Number of Floors") uint floors;
}
```

```d
import initest;
import inifiled;

import std.string;

void main() {
	Person p;
	p.lastname = "Mike";
	p.height = 181.7;

	p.someStrings ~= "Hello";
	p.someStrings ~= "World";

	p.spose.firstname = "Molly";
	p.spose.age = 72;

	p.spose.house.rooms = 5;
	p.spose.house.floors = 2;

	p.dog.name = "Wuff";

	// Here the ini file is written
	writeINIFile(p, "filename.ini");

	// Here the ini file is read
	Person p2;
	readINIFile(p2, "filename.ini");

	// They should be the same
	assert(p == p2, format("%s\n%s", p, p2));
}
```
