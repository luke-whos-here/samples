# C# style guide

## Bracing
Place open braces on a new line after the statement that begins the block. Indent the contents of the brace by 1 tab or 4 spaces.

```c#
if(someExpression)
{
	DoSomething();
}
else
{
	DoSomethingElse();
}
```

---

Single-line statements can have braces that begin and end on the same line.

```c#
public class Foo
{
	int m_bar;

	public int m_bar
	{
		get { return m_bar; }
		set {m_bar = value; }
	}
}
```

---

Always use braces, even for single-statement blocks.

```c#
for (int i=0; i<100; i++) { DoSomething(i); }
```

## Commenting
Use comments to describe your intention and approach. We recommend you add comments to most routines to help eveyone understand what's happening in your code.

In most cases, comments should be placed above the code.

```c#
// Required for Controller access for hit detection
FPSController controller = hit.GetComponent<FPSController>();
```

---

Very short comments can be placed at the end of a line.

```c#
public class Something
{
	private int m_itemHash; // instance member
	private static bool s_hasDoneSomething; // static member
}
```

## Spacing
Use a single space after a comma between function arguments.

Good:

```c#
Console.In.Read(myChar, 0, 1);
```

Bad:

```c#
Console.In.Read(myChar,0,1);
```

---

Do not use a space between a parenthesis and function arguments.

Good:

```c#
CreateFoo(myChar, 0, 1)
```

Bad:

```c#
CreateFoo( myChar, 0, 1 )
```

---

Do not use spaces between a function name and a parenthesis.

Good:

```c#
CreateFoo()
```

Bad:

```c#
CreateFoo ()
```

---

Do not use spaces inside brackets.

Good:

```c#
x = dataArray[index];
```

Bad:

```c#
x = dataArray[ index ];
```

---

Use a single space for flow control statements and comparison operators.

Good:

```c#
while (x == y)
```

Bad:

```c#
while(x==y)
```

### Tabs and spaces

The equivalent of 1 tab is 4 spaces.

Whichever you use, remember to format any new files for Unity. (Default for Unity is tabs.)

## Naming
Our naming styles are as follows:

- **Member variables:** camelCase
- **Parameters:** camelCase
- **Local variables:** camelCase
- **Functions, properties and classes:** PascalCase

Prefix **interface names**  with "I".

Do not prefix **enums**, **classes**, or **delegates** with any letter.

For **events**, use the C# standard of adding `On` to the event handling prefix. Do not prefix the event itself.

```c#
class Metronome
{
	public event EventHandler Tick;

	protected virtual void OnTick(EventArgs e)
	{
		// Raise the Tick event
		var tickEvent = Tick;
		if(tickEvent !=null)
		    tickEvent(this, e);
	}

	public void Go()
	{
		while(true)
		{
			Thread.Sleep(2000);
			OnTick(EventArgs.Empty); // Raises the Tick event
		}
	}
}
```

### File naming
Source files should contain only one `public` type. The filename should be the same as that `public` type.

## Further reading
- [C# coding standards by Dofactory](https://www.dofactory.com/csharp-coding-standards)
- [C# code conventions by Microsoft](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)