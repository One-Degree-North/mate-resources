# ODN MATE Arduino/C/C++ Style Guide
(Current Version: Oct 2021)

**Disclaimer**:
Please, don't bother with this until you actually know how to code in Arduino-C/C/C++. The style guide is for production code mostly by the R&D Team. 

This styel guide is still somewhat incomplete.

Based partially on [CS50's style guide](https://cs50.readthedocs.io/style/c/).

## Code Layout
### General
- Maximum length of a single line
	- Soft cap: 80 characters
	- Hard cap: 119 characters
- File Encoding
	- UTF-8
- Tabs
	- Prefer 4 space characters over `\t`

### Comments
- Whole-line comments:
	- `// this is a comment.`
	- notice the space between the ``//`` and the rest of the comment
- Side comments:
	- `shape(8, 5);  // creates an octagon with side length 5`
	- notice the two spaces between the code and the comment
- Comments always go **above** or **to the right** of code, never below.

### Conditions
Just do this, Java style.
```c
if (x > 0) {
	Serial.println("x is positive");
} else {
	Serial.println("x is zero or negative");
}
```
Notice the space between the `(x > 0)` and `{`.
Creating a new line for the opening bracket (`{`) is also acceptable.

### Loops
Do this:
```c
for (int i = 0; i < 5; i++) {
	// Do something
}
```

### Declarations
Do this:
```c
char a, b;
int c = 0;
float values[50];
float *valuePointer = &values;
```

### Operations
The style should look like this:
```c
int i = (3 + 4) % 5;
i++;
int j = i << 2;
j *= 15;
```

### Functions
Do this:
```c
int i = 0;

void loop() {
	Serial.println(getData(i));
	i++;
	i %= sizeof(uintArray) / sizeof(uintArray[0]);  // gets the actual size of the array
}

uint8_t getData(int pos) {
	return uintArray[pos];
}
```
### Macros
Do this:
```c
#define VALUE 8
// Aliased methods should be Capitalized
#define Debug(value) Serial.println(value)
```
Avoid non-syntactic macros
```c
#define elif else if
#define forUpTo(i, n) for((i) = 0; (i) < (n); (i)++)
```

### Structs and Classes
#### Structs
`typedef` format is better:
```c
typedef struct {
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
} Book;
```
Java style is OK:
```c
struct Book {
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}
```
#### Classes
```cpp
class Rectangle {
    int width, height;
  public:
    // if possible, create these methods in the .cpp rather than the .h
    // with, for example, int Rectangle::getDiagonals() {......}
    int getDiagonals() {
	    return sqrt(width * width + height * height);
    }
    void setWidth(int w) {
	    width = w;
    }
    void setHeight(int h) {
	    height = h;
    }
}
```
#### Enums
```c
enum Color {red = 0, green = 120, blue = 240};
enum ExtendedColor {
	red = 0,
	yellow = 60,
	green = 120,
	cyan = 180,
	blue = 240,
	magenta = 300
}
```

## Naming
```cpp
// UPPER_SNAKE_CASE_CONSTANTS
#define FUNNY_CONSTANT 17
const int FUNNY_CONSTANT_SQUARED = FUNNY_CONSTANT * FUNNY_CONSTANT;  // c++ only

// camelCaseVariables
int thisVariable = 35

// camelCaseFunction and camelCaseParameter
void doSomething(int someParameter):

// UpperCamelCaseClassStructEnum
class SomeClass {...}
struct SomeStruct {...}
enum AnEnumerator {...}
```
Of course, make your variable/declaration names meaningful.

### Files
```
projectName/
	main.cpp
	database.cpp
	database.h
	utils.cpp
	utils.h
	iconRenderer.cpp
	iconRenderer.h
	databaseHelpers/
		mysql.cpp
		mysql.h
		postgres.cpp
		postgres.h
```
We prefer a single term `lowercase` filename, but in general, use `camelCase`.

## Other things
- ALWAYS make sure your code can be compiled with both `gcc` and `clang`
- Use `C++11`/`C++14`/`C++17` or `C11`/`C17` (`C17` brought no new language features)
	- `C++20`'s module system is quite weird and nowhere near standard yet
