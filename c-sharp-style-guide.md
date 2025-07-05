# C# style guide

## Bracing
Place open braces on a new line after the statement that begins the block. Indent the contents of the brace by 4 spaces.

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

Do not use a space after a parenthesis and function arguments.

Good:

```c#
CreateFoo(myChar, 0, 1)
```

Bad:

```c#
CreateFoo( myChar, 0, 1 )
```

---


