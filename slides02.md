**Writing a function with iteration**

- Think of calculating `n!` the *factorial* of an integer `n`.

```
n! = n x(n-1)x(n-2)x(n-3)x ... x4x3x2x1  if n > 0
0! = 1
```	
	
- No value is defined for negative `n`.
- A typical imperative program gives names `retVal` and `i` to two memory locations of type int and gives them the initial values 1 and 2 respectively and then loops until all the values 2, 3, 4,…, `n` have been generated and multiplied.
- An iterative computation:

```java
int retVal = 1;
//...code to reject negative values of n can go here
for(int i = 2; i <= n; i++) { // assert: retVal = (i-1)!
	retVal = i * retVal; 
}
// at the end i = n+1
```

- Hence the ending value of `retVal` is equal to `n!` provided `n` >= 0
- The code shown for factorial quickly computes the wrong value if int or long are used
- With int, factorial(13) is too small by the amount 2<sup>32</sup> Also, factorial(34) and all inputs larger than 34 are zero
`2x3x4x5x6x7x8x9x10x11x12x13x14x15x16`
`x17x18x19x20x21x22x23x24`
`x25x26x27x28x29x30x31x32x33x34`
` = `
2`x3x`2<sup>2</sup>`x5x`2`x3x7x`2<sup>3</sup>`x9x`2`x5x11x`2<sup>2</sup>`x3x13x`2`x7x15x`2<sup>4</sup>` `
`x17x`2`x9x`2<sup>2</sup>`x5x21x`2`x11x23x`2<sup>3</sup>`x3`
`x25x`2`x13x27x`2<sup>2</sup>`x7x29x`2`x15x31x`2<sup>5</sup>`x33x`2`x17`
` = ` 2<sup>32</sup>`x3x5x3x7x9x5x11x3x13x7x15x17x9x5x21x11x23x3`
`x25x13x27x7x29x15x31x33x17`

- Multiplication by 2 is a shift left: `37 = 100101`, `37x2 = 1001010`, so multiplying by 2<sup>32</sup> clears all 32 bits.
- When calculating with long, factorial(21) is the wrong value since it is negative--in its binary representation it is the true factorial(21) – 2<sup>64</sup>
- With long, factorial(66) and all inputs larger than 66 are zero

**Accurate values using BigInteger**

```java
package notes;
import java.math.BigInteger;
import java.util.Scanner;

/**
 * FactorialFunctions computes and shows the factorials of the
 * command line arguments.
 * 
 * @author CS 140
 */
public class FactorialFunctions 
{
	/**
	 * Compute the factorial of the input value using int if the
	 * input is positive or zero. If the input is negative the
	 * function returns the <em>erroneous</em> value 1.
	 * The code uses a loop with the invariant: retVal = (i-1)!
	 * @param n positive or 0 value to compute factorial
	 * @return the factorial of n
	 */
	public static int factorial1(int n) 
	{
		int retVal = 1;
		for (int i = 2; i <= n; i++) 
		{
			retVal *= i;
			// means retVal = retVal * i;
		}
		return retVal;
	}

	/**
	 * Compute the factorial of the input value using long if the
	 * input is positive or zero. If the input is negative the
	 * function returns the <em>erroneous</em> value 1.
	 * The code uses a loop with the invariant: retVal = (i-1)!
	 * @param n positive or 0 value to compute factorial
	 * @return the factorial of n
	 */
	public static long factorial2(int n) 
	{
		var retVal = 1L;
		for (int i = 2; i <= n; i++) 
		{
			retVal *= i;
		}
		return retVal;
	}

	/**
	 * Compute the factorial of the input value using BigInteger if the
	 * input is positive or zero. If the input is negative the 
	 * function returns the <em>erroneous</em> value 1.
	 * The code uses a loop with the invariant: retVal = (i-1)!
	 * @param n positive or 0 value to compute factorial
	 * @return the factorial of n
	 */
	public static BigInteger factorial3(int n) 
	{
		var retVal = BigInteger.ONE;
		for (int i = 2; i <= n; i++) 
		{
			retVal = retVal.multiply(BigInteger.valueOf(i));
		}
		return retVal;
	}

	/**
	 * The main method computes and prints the
	 * prints the factorial of the command line
	 * argument on the screen.
	 * @param args command-line argument should be
	 * a single non-negative integer
	 */
	public static void main(String[] args) 
	{
		if (args.length == 0) 
		{
			System.out.println("Usage error: you must " +
					"supply a non-negative integer argument");
		} 
		else 
		{
			for(int i = 0; i < args.length; i++) 
			{
				var input = new Scanner(args[i]);
				if (input.hasNextInt()) 
				{
					int testValue = input.nextInt();
					if (testValue < 0) 
					{
						System.out.println(
							"Usage error: you must " +
							"not provide a negative " + 
							"integer argument");
						System.out.println(args[i] + 
							" is negative");
					} 
					else 
					{
						System.out.println(
							factorial1(testValue));
						System.out.println(
							factorial2(testValue));
						System.out.println(
							factorial3(testValue));
					}
				} 
				else 
				{
					System.out.println("Usage error: you must " +
							"provide a non-negative " +
							"integer argument");
					System.out.println(args[i] + " is not an "
							"integer");
				}
				input.close();
			}
		}
	}
} 
```

- factorial3(50) is 30414093201713378043612608166064768844377641568960512000000000000

**For loops, see Section 6.3**

- If you know how many times the loop is likely execute, use

```java
for(initial; boolean-end-check; increment) 
{
	sequence-of-statements
}
```

- The braces can be omitted if there is a *single* statement but PLEASE USE BRACES even in that case for safety
- NOTE that the "boolean-end-check" can have extra tests to escape early from the loop

**While loops, see Section 6.1**

- If the number of iterations is unknown, use a while loop:

```java
while(boolean-condition) 
{
	sequence-of-statements
}
```

- Again, the braces can be omitted if there is a *single* statement but PLEASE USE BRACES even in that case for safety
- Minor Detail: You can even have zero statements in very sneaky code that does useful work while evaluating the *boolean-condition*:

```java
public static void main(String[] args) throws FileNotFoundException 
{
	int count = 0;
	var input = new Scanner(new File("out.txt"));
	while(++count > 0 && input.hasNextLine() 
			&& input.nextLine() != null) {}
	// we could use simply ; instead of  {}
	// nextLine() reads the line and moves to next line.
	// It must be part of the boolean, hence != null

	System.out.println("Number of lines in file = " + (count-1));
	input.close( );
}
```

**Common errors (infinite loops)**

- The following is sadly common and wastes everyone’s time since it is hard to spot

```java
int x = 10;
while(x > 0);	//loops forever, no output
{
	System.out.println(x);
	x--;
}
```

- Also

```java
int x = 10;
while(x > 0)
	System.out.println(x);	//prints forever
	x--;
```

**The value of a variable is changed by assignment**

- As in many compiled languages, Java variables need have a type (possibly inferred if they are local variables), which defines the operations allowed on that variable. 
- The declared type cannot be changed (although variables that have a reference type
can be assigned to subtype instances if the declared type has subtypes--see
explanations later.
- Variables can be assigned different values.

```java
// Assume these lines are inside a method
var ps = new PrintStream(new File("xyz.txt"));
...
ps = System.out;
int k = 7; 
...
k = -20;
```

**Constants cannot be changed (Section 4.1.2)**

- There is a concept of constant using the word final

```java
public final double TAX_RATE = 0.28;
```

- The Java convention is to use upper-case letters for constants
- We will see later that the word *final* is used in some other ways in Java. It always has the meaning "cannot be modified" but that signifies different things in different places

**Boolean Operators (Section 5.7)**

- The operators are simply *and*, *or*, *not*
- Java uses the same symbols as C:

| meaning |--  |symbol in the code |
|:-------:|----|:-----------------:|
|	*and* |--  | `&&`  |
|	*or*  |--  | `\|\|` |
|	*not* |--  | `!`  |

- Hence we can write

```java
if((x%4 != 0 || x%100 == 0) && x%400 != 0) 
```

- and we see code such as (Programming Project P5-1) 

```java
import java.time.LocalDate;
public class Slides02Ex2 
{
	/**
	* Allows the user to input year, month, day values on the command line
	* when running this class
	* @param args command line arguments: first (if present) is the year,
	* second (if present) is the month (1-12), the third (if present) is the 
	* day (1-31)
	*/
	public static void main(String[] args) 
	{
		int y = 2020;
		int m = 3;
		int d = 22;
		if(args.length > 0) 
		{
			y = Integer.parseInt(args[0]);
		}
		if(args.length > 1) 
		{
			m = Integer.parseInt(args[1]);
		}
		if(args.length > 2) 
		{
			d = Integer.parseInt(args[2]);
		}		
		var test = LocalDate.of(y, m, d);
		int month = test.getMonthValue();
		var season = "Fall";
		if(month >= 1 && month <=3) 
		{
			season = "Winter";
		} 
		else if (month >= 4 && month <= 6) 
		{
			season = "Spring";
		} 
		else if (month >= 7 && month <= 9) 
		{
			season = "Summer";
		} 
		if(month%3 == 0 && test.getDayOfMonth() >= 21) 
		{
			if(season.equals("Winter")) 
			{
				season = "Spring";
			} 
			else if (season.equals("Spring")) 
			{
				season = "Summer";
			} 
			else if (season.equals("Summer")) 
			{
				season = "Fall";
			} 
			else 
			{
				season = "Winter";
			}
		}
		System.out.println(season);
	}
} // USE equals(...) FOR REFERENCE VARIABLES
```

-----------------------------
**Arrays, Lists and ArrayLists**

-----------------------------

**This starts in Chapter 7**

- Now we are discussing built-in data types and API-defined data structures

**Array: Section 7.1**

- This is an easy section of the textbook to reads
- We will see various ways to instantiate/initialize arrays.
- Many students are not able to remember these different ways if they do not code
enough in a plain text editor.
- Here is a complete declaration and instantiation:

```java
String[] monthNames = {"January", "February", 
	"March", "April", "May", "June", "July", 
	"August", "September", "October", "November", 
	"December"};
int[] thisDecade = {2020, 2021, 2022, 2023,
	2024, 2025, 2026, 2027, 2028, 2029};
double[] averages = {3.4, 3.7, 4.1, 2.9, 3.95};
Loan[] loans = { new Loan(200, 5.0), 
	new Loan(1500, 6.0), new Loan(400, 4.5)};

// NOTE we can be more verbose:
String[] monthNames1 = new String[]{"January", "February", 
	"March", "April", "May", "June", "July", 
	"August", "September", "October", "November", 
	"December"};
int[] thisDecade1 = new int[]{2020, 2021, 2022, 2023,
	2024, 2025, 2026, 2027, 2028, 2029};
double[] averages1 = new double[]{3.4, 3.7, 4.1, 2.9, 3.95};
Loan[] loans1 = new Loan[]{new Loan(200, 5.0), 
	new Loan(1500, 6.0), new Loan(400, 4.5)};
```

- There are some small restrictions on using var for local variables with
arrays:

```java
void method()
{
	// these two give errors
	var monthNames = {"January", "February", 
		"March", "April", "May", "June", "July", 
		"August", "September", "October", "November", 
		"December"};
	var thisDecade = {2020, 2021, 2022, 2023,
		2024, 2025, 2026, 2027, 2028, 2029};

	// these two do work
	var monthNames1 = new String[]{"January", "February", 
		"March", "April", "May", "June", "July", 
		"August", "September", "October", "November", 
		"December"};
	var thisDecade1 = new int[]{2020, 2021, 2022, 2023,
		2024, 2025, 2026, 2027, 2028, 2029};
}
```

- Objects in memory

![](https://www.cs.binghamton.edu/~lander/cs140/arrays1.JPG)

- The names `thisDecade` and `averages` are *not stored* in memory, they are in the diagram to explain what the storage areas represent.
- Similarly the name `length` and the indices 0, 1, 2, ... are not stored in the array objects, they indicate where things are stored at the offset locations (the length needs to be stored somewhere but the diagrams in the textbook do not agree that it is at the first position). (<a href="https://docs.oracle.com/javase/specs/jls/se11/html/jls-10.html" target="blank"> this link</a> states: "The array's length is available as a final instance variable length.")

![](https://www.cs.binghamton.edu/~lander/cs140/arrays2.JPG)

- Arrays of elements that are not a primitive types, will only store references to objects stored elsewhere in the heap

![](https://www.cs.binghamton.edu/~lander/cs140/arrays3.JPG)

**Accessing array elements**

- `monthNames[5]` is "June"
- `thisDecade[0]` is 2020
- `averages[averages.length – 1]` is 3.95
- `loans[2].getAmountDue(3)` will return the amount due on `loans[2]`
- Note arrays are objects that have a field "length" and can only contain that quantity of different values
- An array variable can be declared without instantiation
- An array variable can be declared and instantiated without explicit initialization
- An array variable can be reassigned to completely new storage (for example to change its length)

**Arrays can simply be declared**

```java
Loan[] moreLoans;
// Java will also accept Loan moreLoans[];
```

![](https://www.cs.binghamton.edu/~lander/cs140/arrays4.JPG)

- The memory storage is undefined if `moreLoans` is a local variable 
- The value is null if `moreLoans` is a field of some object 
- We could give `moreLoans` a value using assignment 

```java
moreLoans = loans;
```

- Then loans and moreLoans become two names for the same thing
- Or by calling for a completely new array to be constructed (NOTE this 
must use the verbose form new Loan[]{...})

```java
moreLoans = loans;
// complete instantiation:
moreLoans = new Loan[]{new Loan(100, 3.0), new Loan(450, 2.5)};
```

![](https://www.cs.binghamton.edu/~lander/cs140/arrays5.JPG)

**An array can be instantiated with only its specific length**

```java
String[] example1 = new String[10];
// inside a method, you can have
	var example2 = new String[10];
```

- each gives an array with 10 openings for Strings, indexed from 0 through 9 but each position is null until you put a value there

![](https://www.cs.binghamton.edu/~lander/cs140/arrays6.JPG)

```java
long[] timeMillis = new long[15];
```

- gives an array with 15 openings to store long values but the values are all 0L until you put values in

**Inserting a value, for example:**

![](https://www.cs.binghamton.edu/~lander/cs140/arrays7.JPG)

```java
timeMillis[9] = System.currentTimeMillis();
```

- Note: the number of milliseconds since the beginning of January 1, 1970; upper limit is in year 292,278,994

**Strings are stored using 2 objects**

![](https://www.cs.binghamton.edu/~lander/cs140/arrays8.JPG)

- We shall see later in the course the need for the hash code value of an object
- In this example it is `31*31*'J' + 31*'a' + 'n'  = 74231`
- Java provides numeric type promotion:	

 byte►short►int►long►float►double
 
 (also char►int►long►float►double)

**Note on variable parameters**

- This feature was introduced in Java 5, and is familiar from C, C++, Python.
- A constructor or a method might be 

```java
public Example(String... numbers) 
{
	...
}

public int sum(int... numbers) 
{
	int sum = 0;
	if (numbers != null && numbers.length > 0) 
	{
		for(int i = 0; i < numbers.length; i++) 
		{
			sum += numbers[i];
		}
	}
	return sum;
}
```

- This usage is called *vararg* parameters. There can be only one vararg parameter and it must be the last if there are multiple parameters.
- They are imlemented using arrays and the parameter name can be used as an array.
- Hence

```java
var obj = new Example("one", "two", "three");
var val = sum(2, 3, 4, 5);
```

- is implemented as 

```java
String[] temp1 = {"one", "two", "three"};
var obj = new Example(temp1);
int[] temp2 = {2,3,4,5};
var val = sum(temp2);
// NOTES
sum(); 
means 
int[] temp3 = {}; // empty array
sum(temp3);
// to test a null input, you need
int[] temp4 = null;
sum(temp4);
// or
sum(null);
```

-----------------------------
**Enhanced for loop for arrays (Section 7.2)**

-----------------------------

- The enhanced for loop is Java's version of the "foreach" construct in many languages:
- Given any type `T` and array `T[] arr`, the notation:

```java
for(var t : arr) 
{ 
	...t...		// ASSIGNING t = ... DOES NOT CHANGE arr
}
```

is equivalent to 

```java
for(int i = 0; i < arr.length; i++) 
{
	...arr[i]... // EXCEPT HERE arr[i] = ... CHANGES arr
}
```

**You CANNOT use enhanced-for to assign new values to elements**

```java
double[] arr = new double[10];
for(int i = 0; i < arr.length; i++) 
{
	arr[i] = 150.0*i;
}
```

- *CANNOT be replaced with*

```java
for(var d : arr) 
{
	d = 150.0*i;
}
```

- Think of the enhanced for loop as 

```java
for(int i = 0; i < arr.length; i++) 
{
	int d = arr[i];
	d = 150.0*i;
}
```

- In this form, arr is obviously not changed.
- Enhanced for loops can examine the elements of an array (or other collections--see later) and perhaps change the content of the elements if they are non-null references, but not reassignment to a different value.

**Looping through arrays (see Section 6.3)**

- If we have reason to examine each element of an array, we usually use a for loop:
- Will need to import `java.time.format.DateTimeFormatter`
- See <a href="https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/time/format/DateTimeFormatter.html" target="blank">DateTimeFormatter javadoc as HTML</a>

```java
var formatter = DateTimeFormatter.ofPattern(
					"MMMM dd, yyyy; k:m:s.SSS");
for(int i = 0; i < timeMillis.length; i++) 
{
	var inst = Instant.ofEpochMilli(timeMillis[i]);
	var time = LocalDateTime.ofInstant(inst, ZoneId.of("-5"));
	System.out.println(time.format(formatter));
}
```

- Enhanced loops

```java
var formatter = DateTimeFormatter.ofPattern(
					"MMMM dd, yyyy; k:m:s.SSS");
for(var millis: timeMillis) {
	var inst = Instant.ofEpochMilli(millis);
	var time = LocalDateTime.ofInstant(inst, ZoneId.of("-5"));
	System.out.println(time.format(formatter));
}
```

- Note -4 is for EDT and -5 is for EST
- The following code gives August 17, +292278994; 3:12:55.807

```java
var formatter = DateTimeFormatter.ofPattern(
			"MMMM dd, yyyy; k:m:s.SSS");
var inst = Instant.ofEpochMilli(Long.MAX_VALUE);
var time = LocalDateTime.ofInstant(inst, ZoneId.of("-4"));
System.out.println(time.format(formatter));
```

**Examples**

- Assume these are utility methods in the class `Sample`. They do not use any instance fields of `Sample` and so can be made static

```java
public static int sum(int[] array) 
{
	int value = 0;
	if(array != null) 
	{
		for (int i = 0; i < array.length; ++i) 
		{
			value += array[i];
		}
	}
	return value;
}
```

- using enhanced for loop

```java
public static int sum(int[] array) {
	int value = 0;
	if(array != null) 
	{
		for (int elem: array) 
		{
			value += elem;
		}
	}
	return value;
}
```

- In another method of another class we can call a static method using the class name `Sample`

```java
int[] test = {1, 3, 5, -4, 7, -10, 6, -3};
int total = Sample.sum(test);  // the value is 5
total = Sample.sum(null); // the value is 0
test = new int[]{1, -1, 2, -2, 3, -3, 4, -4};
// note this makes test reference a different array
total = Sample.sum(test); // the value is 0
```

- Java 8 has introduced an Optional type than can give a bit more precision to the return value
- Here are some warnings: https://dzone.com/articles/using-optional-correctly-is-not-optional
- Has the quote from Brian Goetz, Java language architect:
- "Optional is intended to provide a limited mechanism for library method return types where there needed to be a clear way to represent "no result," and using null for such was overwhelmingly likely to cause errors."
- Need to import `java.util.Optional`
- See Chapter 19 Section 19.6 of the textbook

```java
package notes;
import java.util.Optional;
public class NotesEx3 
{
	/**
	 * The method sum returns the sum of the elements in an array.
	 * It returns 0 if the array is null or empty
	 * @param array an array of int
	 * @return the sum of the ints in array or 0 is the array is null or empty
	 */
	public static int sum(int[] array) 
	{
		int value = 0;
		if(array != null) 
		{
			for (int i = 0; i < array.length; ++i) 
			{
				value += array[i];
			}
		}
		return value;
	}

	/**
	 * The method sumOpt returns an Optional<Integer>, which has the value
	 * of the sum of the elements in the array, if array is not null and 
	 * not empty. Otherwise it is Optional.empty().
	 * @param array an array of int
	 * @return an Optional<Integer> with value equal to the sume of the
	 * ints in array or Optional.empty() if the array is null or empty 
	 */
	public static Optional<Integer> sumOpt(int[] array) 
	{
		if(array != null) 
		{
			int value = 0;
			for (int i = 0; i < array.length; ++i) 
			{
				value += array[ i ];
			}
			return Optional.of(value);
		}
		return Optional.empty();

	}

	/**
	 * The main method tests the methods above with many input combinations
	 * @param args command line arguments are not used in this example
	 */
	public static void main(String[] args) 
	{
		int result = 0;
		result = sum(new int[]{1,2,3,4,5,6}); 
		System.out.println("Expected 21");
		System.out.println(result + "\n");

		result = sum(new int[]{-1,1,-1,1,-1,1});
		System.out.println("Expected 0 (because the " + 
					"elements cancel one another)");
		System.out.println(result + "\n");

		result = sum(null);
		System.out.println("Expected 0 (because the input is null)");
		System.out.println(result + "\n");

		result = sum(new int[]{});
		System.out.println("Expected 0 (because the input is empty)");
		System.out.println(result + "\n");

		var optint = sumOpt(new int[]{1,2,3,4,5,6});
		System.out.println("Expected 21");
		if(optint.isPresent()) 
		{
			System.out.println(optint.get() + "\n");
		} 
		else 
		{
			System.out.println("The input was null\n");
		}

		optint = sumOpt(new int[]{-1,1,-1,1,-1,1});
		System.out.println("Expected 0 (because the " + 
					"elements cancel one another)");
		if(optint.isPresent()) 
		{
			System.out.println(optint.get() + "\n");
		} 
		else 
		{
			System.out.println("The input was null\n");
		}

		optint = sumOpt(null);
		System.out.println("Expected \"The input was null or "
					"empty\" (because the input is null)");
		if(optint.isPresent()) 
		{
			System.out.println(optint.get() + "\n");
		} 
		else 
		{
			System.out.println("The input was null or empty\n");
		}

		optint = sumOpt(new int[]{});
		System.out.println("Expected \"The input was null or " +
					"empty\" (because the input is empty)");
		if(optint.isPresent()) 
		{
			System.out.println(optint.get() + "\n");
		} 
		else 
		{
			System.out.println("The input was null or empty\n");
		}
	}
}
```

**Finding the minimum in an array (or 0)**

- This one gives 0 if the array is null or empty (primitive types cannot be null).

```java
public static double minElement(double[] array) 
{
	var minVal = 0.0;
	if (array != null && array.length > 0)
	{
		minVal = array[0];	// NOTE 
		for (int i = 1; i < array.length; i++) 
		{
			if (array[i] < minVal) 
			{
				minVal = array[i];
			}
		}
	}
	return minVal;
}
```

- NOTE you cannot use 0 as the default value if the array is not empty, all the elements may be greater than 0.
- Enhanced for version

```java
public static double minElement(double[] array) 
{
	if (array != null && array.length > 0)
	{
		minVal = array[0]; 	// NOTE
		for (var x: array) 
		{
			if (x < minVal) 
			{
				var minVal = 0.0;
				minVal = x;
			}
		}
	}
	return minVal;
}
```

- We could use `Option<Double>`

```java
public static Optional<Double> minElement(double[] array) 
{
	if (array != null && array.length > 0) 
	{
		minVal = array[0];
		for (int i = 1; i < array.length; i++) 
		{
			if (array[i] < minVal) 
			{
				minVal = array[i];
			}
		}
		return Optional.of(minVal);
	}
	return Optional.empty();
}
```

**Reference types can give null elements**

- Find the first BankAccount in an array that has the least balance
- First think of the test cases:

```java
BankAccount[] test1 = null;	// null array
BankAccount[] test2 = {};	// empty array
BankAccount[] test3 = {null, null, null};
BankAccount[] test4 = {new BankAccount(200),
	new BankAccount(150), 
	new BankAccount(300)};
BankAccount[] test5 = {null, null, 
	new BankAccount(200), null, 
	new BankAccount(150), null,
	new BankAccount(300)};
```

- In `test3` and `test5`, it will be no use setting `minVal = array[0]` since that is null
- First we will look at the code for the tests `test1`, `test2`, `test4`, where there are no null elements

```java
public static BankAccount minBal(BankAccount[] arr) 
{
	BankAccount minAcc = null;
	if (arr != null && arr.length > 0) 
	{
		//FIRST ASSUME ALL ELEMENTS ARE NOT null
		minAcc = arr[0];
		for (int i = 1; i < arr.length; i++) 
		{
			if (arr[i].getBalance() < minAcc.getBalance()) 
			{
				minAcc = arr[i];
			} 
		}
	}
	return minAcc;
} 		// gives null if the array is null or empty
```

- We cannot call `array[i].getBalance()` on every element without possibly getting a `NullPointerException`.
- for `test3`, `minBal(test3)` should be `null`
- for `test3`, minBal(test5) should be `test5[4]`
- in the following code `minAcc == null` is used as an indicator that the first non-null BankAccount has yet to be found

```java
public static BankAccount minBal(BankAccount[] arr) {
	BankAccount minAcc = null;
	if (arr != null) 
	{ // don't care if array is empty
		for (int i = 0; i < arr.length; i++) 
		{
			if (array[i] != null) 
			{
				if (minAcc == null)
				{
					minAcc = array[i];
				} 
				else if (arr[i].getBalance() < 
						minAcc.getBalance()) 
				{
					minAcc = arr[i];
				}
			} 
		}
	}
	return minAcc;
} 	// gives null if the array is null, empty, or all null elements
```

- Here is a version using an enhanced for loop and `Optional`, with testing

```java
package notes;
import java.util.Optional;
import sec04.BankAccount;
public class NotesEx4 
{
	/**
	 * The method returns an Option<BankAccount>, which has the value
	 * that is an element with smallest balance from an
	 * array of BankAccount objects (the first if there are ties).
	 * If the array is null or empty Optional.empty() is returned or
	 * if all the elements in the array are null. The array is permitted
	 * to contain null and non-null elements.
	 * @param arr an array of BankAccount objects
	 * @return an Optional<BankAccount> with value that is the first
	 * non-null element in arr with smallest balance or Optional.empty() if
	 * arr is null, empty, or only contains null elements.
	 */
	public static Optional<BankAccount> minBal(BankAccount[] arr) 
	{
		Optional<BankAccount> retVal = Optional.empty();
		if (arr != null && arr.length > 0) 
		{
			BankAccount minAcc = null;
			for (BankAccount acc : arr)
				if (acc != null)
					if (minAcc == null) minAcc = acc;
					else if(acc.getBalance() < minAcc.getBalance())
						minAcc = acc;
			if(minAcc != null) retVal = Optional.of(minAcc);	 
		}
		return retVal;
	}

	/**
	 * The main method tests the methods above with many input combinations
	 * @param args command line arguments are not used in this example
	 */
	public static void main(String[] args) 
	{
		BankAccount[] test1 = null;	// null array
		BankAccount[] test2 = {};	// empty array   
		BankAccount[] test3 = {null, null, null};
		BankAccount[] test4 = {new BankAccount(200), 
				new BankAccount(150), new BankAccount(300)};	
		BankAccount[] test5 = {null, null, new BankAccount(200), null, 
				new BankAccount(150), null, new BankAccount(300)};

		Optional<BankAccount> optaccount = minBal(test1);
		System.out.println("Expected \"The input was null, empty, or an "
				+ "array of null accounts\" (because the input "
				+ "is null)");
		if(optaccount.isPresent()) {
			System.out.println(optaccount.get().getBalance() + "\n");
		} 
		else 
		{
			System.out.println("The input was null, empty, "
					+ "or an array of null accounts\n");
		}

		optaccount = minBal(test2);
		System.out.println("Expected \"The input was null, empty, or an "
				+ "array of null accounts\" (because the input "
				+ "is empty)");
		if(optaccount.isPresent()) 
		{
			System.out.println(optaccount.get().getBalance() + "\n");
		} 
		else 
		{
			System.out.println("The input was null, empty, "
					+ "or an array of null accounts\n");
		}

		optaccount = minBal(test3);
		System.out.println("Expected \"The input was null, empty, or an "
				+ "array of null accounts\" (because the input "
				+ "is array if nulls)");
		if(optaccount.isPresent()) 
		{
			System.out.println(optaccount.get().getBalance() + "\n");
		} 
		else 
		{
			System.out.println("The input was null, empty, "
					+ "or an array of null accounts\n");
		}

		optaccount = minBal(test4);
		System.out.println("Expected 150.0");
		if(optaccount.isPresent()) 
		{
			System.out.println(optaccount.get().getBalance() + "\n");
		} 
		else 
		{
			System.out.println("The input was null, empty, "
					+ "or an array of null accounts\n");
		}

		optaccount = minBal(test5);
		System.out.println("Expected 150.0");
		if(optaccount.isPresent()) 
		{
			System.out.println(optaccount.get().getBalance() + "\n");
		} 
		else 
		{
			System.out.println("The input was null, empty, "
					+ "or an array of null accounts\n");
		}
	}
}
```

**Finding positions in an array**

- Here it is simpler to have a counter in the search loop
- Example: find the position of smallest element in an array

```java
public static int indexFirstMin(double[] array) 
{
	int minIndex = -1;
	if (array != null && array.length > 0) 
	{
		minIndex = 0;
		for (int i = 1; i < array.length; i++) 
		{
			if (array[i] < array[minIndex]) 
			{
				minIndex = i;
			} 
		}
	}
	return minIndex;
}
```

- Note that the return value -1 indicated the array is null or empty
- Note that the smallest element may be the MOST negative

```java
double[] test = {7.1, 3.4, -10.0, 1.5, 
	5, - 4, 7.6, -10, 6.1, -3};
System.out.println(indexFirstMin(test));	// prints 2
```

- Remember type promotion: byte►short►int►long►float►double
- Suppose we want the position of the smallest in absolute value 
(3 is the index of the 1.5)
- That only involves a small change in the test used in the code, replaced

```java
if (array[i] < array[minIndex])
```

by

```java
if (Math.abs(array[i]) < Math.abs(array[minIndex]))
```

**Documentation and Source Code**

- Put a link in your browser directly to the API for Java at Oracle
<a href="https://docs.oracle.com/en/java/javase/11/docs/api/index.html" target="blank">Java 11 API</a> or 
<a href="https://docs.oracle.com/en/java/javase/12/docs/api/index.html" target="blank">Java 12 API</a>
- If you are trying our Java 13, then 
<a href="https://docs.oracle.com/en/java/javase/13/docs/api/index.html" target="blank">Java 13 API</a>
- The other things you should know how to find is `src.zip` file 
(maybe it is a `tar` file for Mac and Linux). In the Corretto and Oracle distribution,
it is in the `lib` folder of the `jdk` installation. If you download the current tar or zip of the current "Build" at https://jdk.java.net/13/, you can locate the source files.

**List and ArrayList (see Section 7.7)**

- It is possible (but a lot of work) to write code that allows for arrays to be expanded as needed by an application, even though Arrays.copyOf and Arrays.copyOfRange make it easy to copy all or part of one array into a new one of a different length. 
- Consider the <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html" target="blank">java.util.Arrays</a> class.
- Since Java 6, there are several versions of the `Arrays.copyOf` and `Arrays.copyOfRange`​ method:
 - `public static <T> T[] copyOf​(T[] original, int newLength)` Copies the specified array, truncating or padding with nulls 
 (if necessary) so the copy has the specified length.
 - `public static <T> T[] copyOfRange​(T[] original, int from, int to)` Copies the specified range of the specified array into a new array.
- Also in the <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html" target="blank">java.lang.System</a> class,
since Java 1, there has been the `System.arraycopy` method:
 - `public static void arraycopy​(Object src, int srcPos, Object dest, int destPos, int length)` Copies an array from 
 the specified source array, beginning at the specified position, to the specified position of the destination array. 
 A subsequence of array elements are copied from the source array referenced by src to the destination array 
 referenced by dest. The number of components copied is equal to the length argument. The elemnts at positions 
 srcPos through srcPos+length-1 in the source array are copied into positions destPos through destPos+length-1, 
 respectively, of the destination array. 
- However, Java does all the work of resizing arrays for you automatically using an `ArrayList` 
(or a `Vector` in multi-threaded applications)
- C++ STL (Standard Template Library) provides vector
- `ArrayList` is one the classes that give an implementation of the abstract concept of `List`
- A `List` is a Java *interface* (Chapter 10)

- Comparing the declaration of an *array* and an `ArrayList`:

```java
int[] numbers;
List<Integer> numList;
// or sometimes:
ArrayList<Integer> numList;
BankAccount[] accts;
List<BankAccount> acctList;
// or sometimes:
ArrayList<BankAccount> acctList;
```

- It is preferable to declare variables at the more general `List` level, `ArrayList` is specific kind of `List`

**Note that List<T> and ArrayList<T> are generic**

```java
List<Double> grades = new ArrayList<>(); 
// use of <> since Java 7, compared to 
double[] gds = new double[20];
// or
Double[] gds = new Double[20];
```

- Integer is the wrapper class for int, Double for double, etc.,

|primitive type|wrapper class|
|:-------:|:-----------------:|
|	`byte` | `Byte`  |
|	`short`  | `Short` |
|	`char` | `Character`  |
|	`int` | `Integer`  |
|	`float` | `Float`  |
|	`double` | `Double`  |
|	`boolean` | `Boolean`  |


- Note that working with local variables, if we write

```java
var grades = new ArrayList<Double>();
```

- then grades will be an ArrayList<Double>, not the more general List<Double>,
though we can write

```java
var grades1 = (List<Double>)new ArrayList<Double>();
// making a List of Double, even
var grades2 = (List<?>)new ArrayList<Double>();
// making a List of Object
```

**autoboxing**

- There is a feature called autoboxing (Section 7.7.4) where we are allowed to put primitive double values in the List of elements from the class Double (Java inserts `new Double(d)`)
- Similarly we can put `int` values in `List<Integer>`, `char` values in `List<Character>`, and so on for each primitive-to-wrapper correspondence.
- `List<int>`, `List<double>` are *illegal* in Java (in C++, you can have `vector<int>`)

**Inserting into lists**

- Arrays use assignment to a location `gds[5] = 25.0;`
- Lists have 

```java
grades.add(25.0);	// put at the end of list,
			// extends list by 1
grades.set(5, 25.0); // overwrite the index 5
			// provided there is an element already 
			// at that index
grades.add(5, 25.0); // insert at the index 5
			// provided there is an element already or
			// the last element is at index 4 when it
			// is equivalent to grades.add(25.0); 
			// extends the list by 1
```

**Reading data from a List (get)**

- What is the method call that corresponds to reading `gds[5]`?

```java
double d;
...
d = grades.get(5);
```

- You will get `IndexOutOfBoundsException` if the `List` does not have at least 6 elements (index 0 through index 5 and possibly larger)
- You can write the following to check that the access will work:

```java
if(grades.size() > 5) {
	d = grades.get(5);
}
```

**List, ArrayList example (using Java 7 & later)**

```java
List<Double> grades = new ArrayList<>();
grades.add(17.5); 	// index 0 
grades.add(15.5); 	// index 1
grades.add(12.75); 	// index 2
grades.add(19.25); 	// index 3
grades.add(13.5); 	// index 4
grades.add(11.25); 	// index 5
grades.add(17.0); 	// index 6
System.out.println(grades); 
// gives
[17.5, 15.5, 12.75, 19.25, 13.5, 11.25, 17.0]
```

- An `ArrayList` object is an adapter for an array of Objects, see Section 16.2 

![](https://www.cs.binghamton.edu/~lander/cs140/arraylist1.JPG)

- Insertion at index value 3

- `grades.add(3, 16.5);` puts 16.5 at index 3 and the following elements move to the right: 

```java
System.out.println(grades); 
// gives
[17.5, 15.5, 12.75, 19.25, 13.5, 11.25, 17.0]
grades.add(3, 16.5);
System.out.println(grades); 
// gives
[17.5, 15.5, 12.75, 16.5, 19.25, 13.5, 11.25, 17.0]
```

![](https://www.cs.binghamton.edu/~lander/cs140/arraylist2.JPG)

**Reading files**

- Now we can read any number of Strings from a file:

```java
	List<String> text = new ArrayList<>();
	try(Scanner inputFile = new Scanner(new File("words.txt"))) {
		while (inputFile.hasNextLine()) {
			text.add(inputFile.nextLine());
		}
	} catch (FileNotFoundException e) {
		e.printStackTrace();
	}
	System.out.println(text.size());

	// NOTE, there has been a problem in the past that may continue
	// that the Scanner version fails to read all the file. In that case 
	// use:
	List<String> text1 = new ArrayList<>();
	try (var br = new BufferedReader(new FileReader("words.txt"))) {
		var line = "";
		while((line = br.readLine())!=null) {
			text1.add(line);
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
	System.out.println(text1.size());

	// NOTE that Java 8 introduced Streams and a new way to read a file.
	// I have not seen comments about whether it works on large files
	List<String> text2 = new ArrayList<>();
	try(Stream<String> lines = Files.lines(Paths.get("words.txt"))){
		text2 = lines.collect(Collectors.toList());
	} catch (IOException e) {
		e.printStackTrace();
	}
	System.out.println(text2.size());
```

- All three print the same value (99171 for the "words.txt" file)
- The `ArrayList` text will keep growing to accommodate the lines read in
- The code inside `ArrayList` increases the available space of the list by a factor each time, e.g. 1.5 times 
- Chapter 19 shows how Java 8 `Stream`works.

- The imports needed for the code above:

```java
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;
```

- When you use the `List` text, *do not forget* to check if there are any lines in the list: `text.size()` tells you how many lines were read (if `text` is not null in the second case)
- If the size was 0 and you did not check, you could easily get an error in later code
- If the size of a List is the number of elements added to it. The backing array may have more capacity

**Converting a List to an array**

```java
String[] lines = new String[text.size( )];
for(int i = 0; i < text.size( ); i++) {
	lines[i] = text.get(i);
}

// or

String[] lines = new String[text.size()];
text.toArray(lines);

// or

String[] lines = {};
lines = text.toArray(lines);
```

**Converting an array to a List**

```java
String[] arr = {"abc", "acd", ...};
List<String> list = new ArrayList<>();
```

- Then we can do the following:

```java
list.addAll(Arrays.asList(arr));
```

- Also, you can do this in the constructor

```java
List<String> list = new ArrayList<>(Arrays.asList(arr));
```

- You can find these tricks by searching, you are likely to finish up at sourceforge, e.g. https://stackoverflow.com/questions/1005073/initialization-of-an-arraylist-in-one-line

- None of these save much space over:

```java
List<String> list = new ArrayList<>();
for(String str : arr) list.add(str);
```

**Autoboxing will not work in more complex situations**

```java
int[] arr = {1, 2, 3};
List<Integer> list = new ArrayList<>(Arrays.asList(arr));
// that does not compile

List<int[]> list1 = new ArrayList<>(Arrays.asList(arr));
// this compiles but gives 1 element List (contains one array)

// The thing you are trying to do ONLY works with 
Integer[] arr = {1, 2, 3}; // autoboxing on each element
List<Integer> list2 = new ArrayList<>(Arrays.asList(arr));
// list2 has 3 elements, as does list3 below:
List<Integer> list3 = new ArrayList<>(Arrays.asList(1,2,3));
```

- Another example of trying to combine autoboxing and type promotion fails

```java
double[] dbs2 = {1, 2, 3, 4}; // OK
Double[] dbs = {1, 2, 3, 4}; // is illegal
// The compiler says it cannot convert from int to Double
Double[] dbs1 = {1.0, 2.0, 3.0, 4.0}; // OK
// it is legal because it can use autoboxing
```

- Please memorize `size()`, `add()`, `get()`, `set()`, `remove()`
- Let `myList` be a `List`, we normally use `myList.add(e)` to add the element `e` at the end of the list
- For values of `n` in the range of the `List`, we can use `myList.add(n, e)` to insert `e` at index `n` (where the index starts at 0)
- `set(n,e)` replaces an element in a `List` and `get(n)` returns the reference at index `n`.
- `size()` is a method called on a `List` to know the number of elements currently stored (it is not the total capacity of the list, which is not intended to be available to the programmer)
- There are `remove(index)` and `remove(Object)` and Java will *often* give a run-time error if you execute `remove` inside a for loop that is traversing the `List`.
- There are other methods: `addAll`, `clear`, `contains`, `indexOf`, `removeAll`, `sort`and several others, see
https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/ArrayList.html

**Range checking**

- You *cannot* insert (add) at `n` using `add(n,e)` if `n > myList.size()` or if `n < 0` (it is OK if `n` equals `myList.size()`, in which case it is the same as `add(e)`)
- `myList.get(n)` returns the element at location `n` if `0 <= n && n < myList.size()`
- If you try to use an `n` outside those limitations, you will get `IndexOutOfBoundsException`
- If `0 <= n && n < myList.size( )`, `mylist.set(n, e1)` replaces the element at `n` (the return value is the element that had been stored at index `n`) and if `n` is outside those limits the same exception is thrown as before.
- Enhanced for loops are similar to the case of arrays

```java
for(int i = 0; i < text.size(); i++) {
	System.out.println(text.get(i).length());
}
// gives same result as
for(String line : text) {
	System.out.println(line.length());
}
```

**Collection classes**

- An ArrayList is one of several "Collection classes" and the enhanced for loop works with all of them
- See <a href="https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Collection.html" target="blank">Collection.java</a>
- The Java documentation points you to `Set`, `List`, `SortedSet`, `HashSet`, `TreeSet`, `ArrayList`, `LinkedList`, `Vector` and many others
- There are special things for small constant lists, where the best (*immutable*) storage is chosen automatically
- See <a href="https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/List.html#of()" target="blank">`List.of(...)`</a> 
- There are similar `Set.of()` and `Map.of()` that we will meet later.

**Finding the BankAccount with smallest balance**

- Consider

```java
public static BankAccount minBal(List<BankAccount> list) 
{
	BankAccount minAcc = null;
	if (list != null) 
	{
		for (int i = 0; i < list.size(); i++) 
		{
			if (list.get(i) != null) 
			{
				if (minAcc == null)
				{
					minAcc = list.get(i);
				} 
				else if(list.get(i).getBalance() 
					< minAcc.getBalance( )) 
				{
					minAcc = list.get(i);
				}
			} 
		}
	}
	return minAcc;
}
```

- This returns `null` if `list` is `null`, empty, or completely filled with `null`. 
- The tests can be:

```java
List<BankAccount> list1 = null;
List<BankAccount> list2 = List.of();
List<BankAccount> list3 = List.of(null, null, null); 
List<BankAccount> list4 = List.of(new BankAccount(200), 
	new BankAccount(150), new BankAccount(300)); 
List<BankAccount> list5 = List.of(null, null, new BankAccount(200), 
	null, new BankAccount(150), null, new BankAccount(300)); 
```

