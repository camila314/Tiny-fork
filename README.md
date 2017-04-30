Tiny is an implementation of the Tiny programming language as specified by this wikipedia page (but very much extended):
http://en.wikipedia.org/wiki/Tiny_programming_language

It compiles a program to it's own bytecode and then executes this bytecode.

The following is an example program:

```

read x y z end
write x + y + z end

```

This program will write the sum of 3 numbers which it reads from stdin to stdout.

Another example program:

```

func fact(x) {						# compute the factorial of a number
	if $x == 0 {
		return 1
	}
	
	return $x * fact($x - 1)
}

write fact(5) end					# compute factorial of 5 and write to stdout

```

This program willl write the factorial of 5 (120) to stdout

The language also has support for local variables and the included interpreter (tinystd.c) 
supports arrays:

```

func test()
	local arr = array()
	
	array_push($arr, "hello world")
	write array_get($arr, 0) end
end

# writes "hello world" (excluding quotes) to stdout
test()

```

You can bind your own functions and structures to the language (see tinystd.c, as this is how arrays are implemented).

Here is an example of the binding api:

```

// C Code

// write this function in tinystd.c
void MyNativeLibrary_add() {
	double num2 = DoPop().number;
	double num1 = DoPop().number;
	
	DoPush(NewNumber(num1 + num2));
}

// and then in the BindStandardLibrary function (in tinystd.c) add this code
void BindStandardLibrary()
{
	...
	BindForeignFunction(MyNativeLibrary_add, "add");
}

# Tiny code

write add(10, 20) end # will output 30 (but it will compute it in the cpu, not in the virtual machine)

```

You can bind any C library to Tiny, but there is no support for structures/classes in the language itself.
