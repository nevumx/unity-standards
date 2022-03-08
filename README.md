Unity C# Coding Standards and Processes
=======================================
by Nicholaos Mouzourakis

# Table of Contents
* [I: Preamble](#i-preamble)
	* [I.A: Purpose of this Document](#ia-purpose-of-this-document)
	* [I.B: Who should Read this Document](#ib-who-should-read-this-document)
	* [I.C: Priorites](#ic-priorites)
		* [I.C.1: A Note on Priorities](#ic1-a-note-on-priorities)
	* [I.D: Mission Statement](#id-mission-statement)
* [II: Introduction](#ii-introduction)
	* [II.A: Ground Rules](#iia-ground-rules)
		* [II.A.1: Environment](#iia1-environment)
		* [II.A.2: Submitted Code is Production Code](#iia2-submitted-code-is-production-code)
		* [II.A.3: Use of Third Party Code is to be Avoided](#iia3-use-of-third-party-code-is-to-be-avoided)
	* [II.B: Definitions](#iib-definitions)
		* [II.B.1: Phases of Development](#iib1-phases-of-development)
		* [II.B.2: State](#iib2-state)
			* [II.B.2.a: Everything Stems from State](#iib2a-everything-stems-from-state)
		* [II.B.3: Scope/Access Level](#iib3-scopeaccess-level)
		* [II.B.4: Multiplicity](#iib4-multiplicity)
		* [II.B.5: Permissions](#iib5-permissions)
		* [II.B.6: Attributes](#iib6-attributes)
* [III: State Considerations](#iii-state-considerations)
	* [III.A: State Minimization](#iiia-state-minimization)
		* [III.A.1: Functionize State Derivation](#iiia1-functionize-state-derivation)
		* [III.A.2: Eliminate Redundant State](#iiia2-eliminate-redundant-state)
		* [III.A.3: Requirement Changes Lead First to Structural State Changes](#iiia3-requirement-changes-lead-first-to-structural-state-changes)
* [IV: Stylistic Considerations](#iv-stylistic-considerations)
	* [IV.A: Tabs vs. Spaces](#iva-tabs-vs-spaces)
	* [IV.B: Horizontal Whitespace](#ivb-horizontal-whitespace)
	* [IV.C: Member Order](#ivc-member-order)
	* [IV.D: Capitalization](#ivd-capitalization)
	* [IV.E: State Initialization](#ive-state-initialization)
	* [IV.F: `var` vs Explicit Types in Declarations](#ivf-var-vs-explicit-types-in-declarations)
	* [IV.G: No Comments](#ivg-no-comments)
	* [IV.H: File and Function Size Preferences](#ivh-file-and-function-size-preferences)
	* [IV.I: Using `=` in `if` Statements](#ivi-using--in-if-statements)
	* [IV.J: No Magic Numbers](#ivj-no-magic-numbers)
	* [IV.K: `float` Literals Get an `f`](#ivk-float-literals-get-an-f)
	* [IV.L: Do Not Let Warnings Dictate Coding Style](#ivl-do-not-let-warnings-dictate-coding-style)
	* [IV.M: Indentation of Long Statements/Expressions](#ivm-indentation-of-long-statementsexpressions)
	* [IV.N: Always Specify Access Level](#ivn-always-specify-access-level)
	* [IV.O: Environment-specific Newlines](#ivo-environment-specific-newlines)
	* [IV.P: Use Defined Constants over Literals when Available](#ivp-use-defined-constants-over-literals-when-available)
	* [IV.Q: String Interpolation over Concatenation](#ivq-string-interpolation-over-concatenation)
	* [IV.R: `System.Action<>`s or `System.Func<>`s over `delegate` Declarations](#ivr-systemactions-or-systemfuncs-over-delegate-declarations)
		* [IV.R.1: `delegate`s vs `event`s](#ivr1-delegates-vs-events)
	* [IV.S: `for` vs `foreach` vs `ForEach(...)`](#ivs-for-vs-foreach-vs-foreach)
	* [IV.T: Prefer Iterated Lists to Repeated Function Calls](#ivt-prefer-iterated-lists-to-repeated-function-calls)
	* [IV.U: Iterate over Shallow Copies of Collections for Safety](#ivu-iterate-over-shallow-copies-of-collections-for-safety)
	* [IV.V: Add a Trailing Comma to All Comma-Separated Lists](#ivv-add-a-trailing-comma-to-all-comma-separated-lists)

# I: Preamble
## I.A: Purpose of this Document
The main purpose of this document is to detail the best standards and practices of writing Unity C# code. Unlike other coding standards, however, this one also describes the process by which that code should be written. It describes the intentions behind coding decisions that are made at each phase of development, from Pre-Alpha to Alpha to Beta to Gold Master. These principles should apply to all games made with the platform, and also should contain principles that can be reused in other types of software projects in general, but games will be the main focus here. 

## I.B: Who should Read this Document
The intended audience for this document are intermediate to advanced (or brave beginner) Unity C# programmers wishing to better organize the code of larger projects, especially nearer to the beginning of said projects, but can also be used as a basis for evaluating options for refactoring the code of existing projects. It is not intended to be a tutorial on C# or its subtleties, and assumes intermediate to advanced knowlege of the C# language, where gaps in knowledge can typically be searched for and read directly on [Microsoft's online C# documentation](https://docs.microsoft.com/en-us/dotnet/csharp/).

## I.C: Priorites
1. **Robustness / Ease of Debugging:** Technical or locigal errors experienced by the end user are unacceptable, and so therefore this is the highest priority. The best way to achieve code robustness is to make errors both as difficult to write as possible, and as easy to debug as possible.
2. **Maintainability / Changeability:** This is the next priority because changes in design and therefore requirements happen so often in games, and are usually the cause for more serious problems further along in development. This is done by writing the most generic code possible, and driving behavior from data instead of code wherever possible.
3. **Code Beauty / Ease of Visual Parsing:** Code that is easy to read and understand is easy to change in a robust manner. This is accomplished by respecting the space, design, and formatting of the code as it is written.
4. **Writeability / Ease of Writing:** Good code is hard to write, unfortunately. With practice though, less effective habits can be beaten, and code can be written cleanly for the programmers in its future.
5. **Performance:** Performance is not a priority until **Beta**, (defined below) at which point, code is profiled aggressively to achieve the desired performance through targeted optimization. Exceptions include code that is so grossly unoptimized that it is unplayable, thus hindering developement.

### I.C.1: A Note on Priorities
In creating these priorities, the author is *well* aware that some of the most sucessful games in production today are actually full of [technical debt](https://technology.riotgames.com/news/taxonomy-tech-debt). This is an unfortunate reality of game development: often times, iteration is so crucial to creating a fun game that code quality is sacrificed to iterate rapidly. The purpose of this document is not to enforce perfection, but instead to instill habits that do not add overburdensome development time when adopted, yet still keep the code quality at a manageable level for those who maintain the code in the future.

## I.D: Mission Statement
As one looks at the different programming constructs made available by the C# standard Unity provides, one must actively assess why they exist, and how they can be used to make one's code cleaner, according to the values above. This is where the standards and practices below come from. They are developed and curated according to lessons learned by the author, and are certainly open to discussion and change through this repository, making this a living document.

# II: Introduction
## II.A: Ground Rules
### II.A.1: Environment
- Use source control, preferably one that has a GUI available. Whether Perforce or Git or another solution is used, a GUI should be available to view local changes, previous diffs, merges, etc.
- Use a modern IDE with refactoring support.

### II.A.2: Submitted Code is Production Code
Similar to the foundational saying, "always treat a gun as if it is loaded," all code that is submitted to source control should be assumed to be production code. Throughout development, a conscious choice should be made to avoid "prototype code" from making it into source control, as when it makes it there, it rarely makes it out. Often times, this is for good reason: the "prototype code" becomes more understood by programmers, and this often creates a "technical debt Stockholm Syndrome" where the fear of new unknown potential bugs, (which *can* indeed happen) along with deadlines, results in the code being given fixes that do not address the root of the problem. Instead, a refactor is often avoided until some "substantial motivating event" occurs to prompt it. (This usually occurs at a time in development when it is not optimal to do so) The reason for this rule is that, while iteration may take longer, it forces designers to be deliberate with their decisions and what they want to try out; no change made to the source code is free, and this approach incurs the development cost upfront instead of later on, which is a more accurate representation of said cost, and makes for better timeline assessment and predictability.

### II.A.3: Use of Third Party Code is to be Avoided
Although the use of code written by a third party is sometimes necessary, its inclusion should not be considered unless all other options have been exhausted. The obvious benefit is not needing to write the code, however in addition to bringing in extra, possibly unwanted functionality (which then essentially becomes "dead code," see section V.D below) this also means that one is at the mercy of whoever wrote the third-party code, meaning that any potential debugging may end up inside the third party code, if the source code is even available. Even if the source code is provided, it is not likely to follow the same standards as one's own code, and as a result, debugging will likely be more difficult. Instead, it is preferable to take the third party source code, (where legally permissable) study it, understand it, modify/rewrite it to conform it to one's own standards (preferably these standards) and then integrate it into one's codebase manually. Even though this is more difficult, it makes the use of the third party code both safer and easier to debug.

## II.B: Definitions
### II.B.1: Phases of Development
- **Pre-Alpha:** This is the phase for all of the pre-production work to create a proof-of-concept for a game. Bits and pieces may be usable/playable, but there is not yet a crtical path from the start to the end of the game.
- **Alpha:** This is when there exists a crtical path from the start to the end (or repeat point) of the game, but some features/mechanics remain to be created. Performance bugs which do not substantially hinder the ability to test the game should *not* yet be addressed.
- **Beta:** At this point, all critical paths and features/mechanics are complete, but bugs still remain to be fixed, possibly including performance bugs, which should be addressed towards the end of this phase. This phase should be kept as short as possible though, to avoid delays in release.
- **Gold Master:** This is the point where the game is feature-complete, and is free of all known bugs.

### II.B.2: State
Before continuing, it is important to define "state," since it plays a crucial role in this document. "State" is any data that is stored for any amount of time. Any variables, data members, fields or data that are stored in memory *or* on permanent storage are "state."

#### II.B.2.a: Everything Stems from State
State forms the structure of the data which programs manipulate, and so before writing any code, it is important to organize state in such a way that it is as difficult as possible for code to put your program in an invalid state. State is also ubiquitous, and so it is important to organize it rigorously by its intended organizational aspects, which this document defines as its scope/access level, nesting, multiplicity, permissions and when applicable, attributes. Debugging often amounts to a certain piece of state having an incorrect value at some point of execution in the program. Since debugging involves first finding the incorrect state, then finding where/how it is being set incorrectly, using the most restrictive of each of the above organizational aspects will make it much easier to track down invalid state when debugging, and harder to write code that that can result in invalid state in the first place. Next, the organizational aspects of state will be examined:

### II.B.3: Scope/Access Level
The most useful aspect of scope and access is that they, along with permissions, quickly inform the programmer about the amount and location of code that could possibly touch the specified state. For this reason, state should be scoped as restrictively as possible, meaning local variables > `private` > `protected` > `public`. This way, the programmer can look at the access level of any piece of state and know at a glance all of the places that will have the potential to access or mutate that state. The potential search area can then be limited to the current class, sub-classes, or the whole program via a reference lookup. If all state was `public` in such a scenario, the debugging has the potential to be much harder, especially if multiple states are being debugged.

### II.B.4: Multiplicity
Knowing whether a member is `static` or not also helps greatly with debugging, since one does not have to worry about multiple instances of a `static` data member, so `static` should always be preferred when possible. All `readonly` state should be `static` and, if possible, `private` or `protected`. `const` is prefereable to all of the previous modifiers, but is more restricted by the compiler, and so can not always be used.

NOTE: The previous two organizational aspects (besides `const`) apply to functions as well as fields. The following two do not.

### II.B.5: Permissions
Permissions refer to the read/write (or `get`/`set`) accessors (not to be confused with access modifiers) on properties and auto-implemented properties. Whenever possible, the most restrictive specifiers should be used to describe to a potentially debugging programmer where a particular property is read from or written to to limit the scope of their debugging. Here once again, `private` > `protected` > `public` for both `get`ters and `set`ters. Auto-implemented properties should also be preferred over data members to exploit this ability. Permissions _also_ refer to whether or not a data member (as opposed to a property) is `readonly`. Remember that a `readonly` reference to an object still allows that object's members to be mutated!

### II.B.6: Attributes
With Unity, some restrictions can be placed on state with attributes as well. `[SerializeField]` is a common attribute which allows one to set references in the editor while keeping the serialized field `private` or `protected`. An auto-implemented property would be preferable here, but unfortunately, those are not serializeable. Instead, a normal `get`ter property should be used to expose such non-`public` data members if necessary, like so:

```csharp
[SerializeField]	private	MonoBehaviour	_cube;
			public	MonoBehaviour	Cube	{ get { return _cube: } }
```

Note how this utilizes the `public` and `private` access modifiers to achieve the goal that is pursued: the `_cube` field is only be avalable to the class and the Unity serializer.

Following from this, a declaration in the form `public MonoBehaviour Cube;` should *only* be used if all of the following hold true:
1. `Cube` needs to be set/serialized in the editor.
2. `Cube` needs to be read by code outside its class.
3. `Cube` needs to be set to another value by code outside of its class.

i.e. since these are all possible with the declaration, they should all happen somehere in the code. Consequently, if only the last two are necessary, but not the first, `[NonSerialized]` should be favored over auto-implemented properties, like so: `[NonSerialized] public MonoBehaviour Cube;` instead of `public MonoBehaviour Cube { get; set; }`

Another attribute that can usefully restrict the values of primitive numerical types (when serialized in the editor at least) is the Unity `Range` attribute. Use it when applicable, like so: `[Range(1, 100)] private int _volume;` or `[Range(0.0f, 90.0f)] private float _angle;`

# III: State Considerations
## III.A: State Minimization
In order to avoid ending up in an invalid state, use the absolute least amount of state necessary for the task. This refers to both the number of variables/members, their complexity _and_ the amount of bytes each takes up. For instance, `bool`s should be preferred to `enums`, which should be preferred to `byte`s, which should be preferred to `sbytes`s, which should be preferred to `short`s etc. And `int`s or `long`s should be preferred to `float`s, especially for discrete (as opposed to continuous) state. `enums` should also be used over `string`s when possible. Furthermore, if two pieces of state can actually be represented with one, this should done. One example of a way to do this is to...

### III.A.1: Functionize State Derivation
Under most circumstances, state should not exist if it can be expressed as a function of another state. In such cases, it should really be a function or property instead. For example:

```csharp
// Method A:
private	float[]	_lapTimes;
private	float	_averageLapTime;
```

Should instead look like:

```csharp
// Method B:
private	static	float[]	_lapTimes;
private	static	float	_averageLapTime
{
	get
	{
		return _lapTimes.Aggregate(0.0f, (a, c) => a + c) / Mathf.Max(1, _lapTimes.Length);
	}
}
```

The tradeoff here gives up some performance for rubustness, as per the priorities above. An exception to this rule does exist, though, when optimizing the game code durng **Beta**. Caching (Method A above) is often more performant than computing on the fly, (Method B above) especially when computing it often. If profiling deems it to be worthwhile, a caching solution (Method A above) can be used, as long as it is updated alongside its source state. An inner class can help encapsulate the data properly and adhere to the other state principles, like so:

```csharp
public class CachedLapTimes
{
	private	float[]	_lapTimes;
	public	float	AverageLapTime	{ get; private set; }

	public	float[]	LapTimes
	{
		get
		{
			return _lapTimes.ToArray(); // NOTE: See section IV.U to see why we copy this array here.
		}
		set
		{
			_lapTimes = value;
			AverageLapTime = _lapTimes == null ? 0.0f :
				_lapTimes.Aggregate(0.0f, (a, c) => a + c) / Mathf.Max(1, _lapTimes.Length);
		}
	}
}
```

**WARNING:** Using collections in the above manner means that they must be assigned instead of mutated by reference or indexing. For example, instead of `LapTimes[0] = 5.0f;`, something similar to `float[] lapTimes = LapTimes; lapTimes[0] = 5.0f; LapTimes = lapTimes;` must be used. To avoid this, a cleaner approach would be to equip the above class with an [indexer](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/indexers/), which would then enable the use of code like `LapTimes[0] = 5.0f;`. This is left as an exercise for the reader.

### III.A.2: Eliminate Redundant State
Redundant state is when something is represented my more state than it should be, causing the possibility of invalid state to occur. This should be avoided whenever possible, and the proper state should be used for the job. For example, if there are two `bool`s like so:

```csharp
public class Character : MonoBehaviour
{
	private	bool	_isCrouched;
	private	bool	_isLyingDown;

	// ...
}
```

This creates the possibility for an invalid state, which would be if both `_isCrouched` and `_isLyingDown` are `true`, what is the `Character` doing? Crouching, or lying down? To fix this, use an `enum`, like so:

```csharp
public enum Stance
{
	STANDING,
	CROUCHING,
	LYING_DOWN,
}

public class Character : MonoBehaviour
{
	private	Stance	_stance;

	// ...
}
```

### III.A.3: Requirement Changes Lead First to Structural State Changes
As soon as code is written and executed, the potential for requirements to change is present. In this case, as it was written, so it should be changed. The addition or subtraction of state, or the change in its type, scope, multiplicity, permissions, and/or attributes should be the first consideration when a change in requirements needs to be implemented in the code. The beauty of this approach is the compiler will then naturally let one know what code has to be changed as a result of this change of state, making it _much_ easier to find the code that needs to change to properly implement the new requirement(s). Further code can then be changed as necessary. This is why the delacartion and organziation of state needs to be kept neat, so that is it easy to find and change. This is the topic of the next section. For an example of this, one could imagine that in first code snippet of the previous example, `_isLyingDown` had not been added yet, but a requirement chanegs to allow for a character to lie down. Adding the `_isLyingDown` field would be incorrect for the reason stated in the previous section. It is the `_isCrouched` field that should be examined first.

# IV: Stylistic Considerations
## IV.A: Tabs vs. Spaces
Tabs should be used for indentation everywhere with no exceptions. They should appear 4 spaces wide on one's IDE, and no whitespace should appear after the last non-whitespace character on any given line. IDEs which can show whitespace are preferred for this reason.

## IV.B: Horizontal Whitespace
Whenever more than one data member is declared at the beginning of a class, all the different decorators should be on distict vertical lines in the text, aligned by tabs. Attributes (except for the last or only attribute) should be listed on their own lines before the declaration. Exceptionally long types may be broken into multiple lines, and default value assignments should be aligned with auto-implemented properties with like so:

```csharp
public class Example
{
	public	static	readonly		int	ExampleStaticReadOnlyInt	= 5;
	public			readonly	int	ExampleReadOnlyInt		= 5;
	public	static				int	ExampleStaticInt;		= 5;
	public					float	ExampleFloat;			= 5.0f;

	[NonSerialized]
	public					LinkedList<LinkedList<int>>
							ExampleIntListList;		{ get; private set; }
}
```

## IV.C: Member Order
When defining `class`es, `enum`s should come first, followed by inner `class`es, followed by serialized data members, followed by nonserialized data members, followed by non-auto-implemented properties, and then functions. Like so:

```csharp
public class Example
{
	public enum ExampleEnum
	{
		SOME,
		EXAMPLE,
		ENUM_VALUES,
	}

	private class Inner
	{
		private int _number;

		// ...
	}

	[SerializeField]	private	int	_otherNumber;

				private	bool	_nonSerializedBool;

	public int PropertyInt
	{
		get
		{
			return _otherNumber;
		}
		set
		{
			_otherNumber = value;
		}
	}

	public void SomeFunction()
	{
		// ... 
	}
}
```

## IV.D: Capitalization
With one exception, all identifiers should be camel cased, with captial letters (not underscores) separating words. The initial capitalization is dependent on the type of identifier and scope of the state where applicable:
- Function, `class`, `enum`, and struct names should begin with an uppercase letter, like so: `class CardHolder` or `private void DealCardsToPlayers(...)`
- `private` data members should start with an underscore and lowercase letter, like so: `private int _numberOfCards;`
- `protected` data members should start with an underscore and uppercase letter, like so: `protected float _WaterLevel;`
- `public` data members should start with an uppercase letter, like so: `public bool IsReadyForAnimation;`
- Local variables should start with a lowercase letter, like so: `int numberOfLevels;`
- `const`s, `static readonly` or `enum` values are to be expressed in all caps, like so: `ALL_CAPS_SEPARATED_BY_UNDERSCORES`

## IV.E: State Initialization
State should be left uninitialized (automatically initialized to the default value) unless a defferent value is necessary. `bool`s should always be worded such that `false` is the default value, and left uninitialized. For example, instead of `private int _score = 0;`, use `private int _score;`.

## IV.F: `var` vs Explicit Types in Declarations
The `var` keyword should be used when possible, *only if* the type it represents eixsts in the same statement, like so: `var scores = new int[5];` or like so: `var newInstance = new Instance();` or even like so: `var newObject = Instantiate<Prefab>(_objectPrefab);`

## IV.G: No Comments
Comments have one possible advantage, and several downsides. While they do "self-document" code to a certain extent, they require more development time to write, and often become quickly out-dated, becoming either irrelevant, or even worse, false. For these reasons, comments are not recommended except in very specific circumstances. Two of which are labelled "*COMMENT EXCEPTION*" the in the next two sections, and the third being very complcated algorithms that would otherwise possibly warrant academic consideration, or for labelling dangerous code to encourage caution to future programmers. `// TODO: (...)` comments are also encouraged to ensure that smaller tasks are not forgotten. Under *no* circumstances should commented-out code be submitted to source control. Deleted code that has the potential to be resurrected should be retrieved from source control via the history it provides if necessary. 

## IV.H: File and Function Size Preferences
Individual source files are the preferred way to group similar functions. If they are all related, and a part of the same class, in general, they should be placed together in that class, in that file. `partial` classes are the exception; if a class gets too big, and there is a case for splitting like members into different files, `partial` classes may be used to do so, but this is not required. It is not adviseable to break up a large class when it does not make sense architecurally.
That being said, the only reason to extract a block of code into its own function is if it is reused elsewhere, optionally with parameters it might need. (Remember that even code can be passed as a parameter via a delegate, which is encouraged where appropriate) If there is a function or a file that is getting too big, it may be separated/organized by `#region`s or comments (*COMMENT EXCEPTION 1*) denoting the purpose of each code block or file.
In general, code should be kept as localized as possible, (meaning that related code is kept together) unless there is an achitectural reason to split it up. (Of which there could be many) The reason for this is that when debugging, having too many functions in different locations or too many `class`es/`struct`s in too many files can very easily turn the debugging of one block of code into a complex web of searching disparate functions and files. This is to a certain exent unavoidable, as `class`es/`struct`s and functions indeed have their place, but it is simply to be minimized where possible.

## IV.I: Using `=` in `if` Statements
If one wishes to make an assignment, and then test the evaluation of that assignment in an if statement, utilizing a single `=`, it is perfectly okay to do so, as long as the following comment is included at the end of the line: `// Careful (=)` (*COMMENT EXCEPTION 2*) This acts as a subtle warning to code readers that a `==` not being utilized.

## IV.J: No Magic Numbers
Literals are the enemy of flexible constants. The only literals that should be allowed in code are `0`, `1`, `2` their negatives, and their `float`/`double` equivalents. Instead, a `const` data member is to be used with the capitalization specified above. If the value is not compile-time constant, use a `static readonly` instead. If there are too many `const`s, then a custom `ScriptableObject` makes a nice Flyweight pattern, and should then be used. Also, it is to be noted here that `ScriptableObject`s do not require recompilation to have their values changed.

## IV.K: `float` Literals Get an `f`
All literal float numbers must have a decimal, and at least one digit after that decimal, even if it is only a `0`, and then have an `f` at the end so that one can tell at a glance the type of that literal, like so: `1.0f`, `5.6f`, `0.0f`. `double` literals omit the `f`, like so: `2.0`, `4.7`.

## IV.L: Do Not Let Warnings Dictate Coding Style
During the course of development, it is likely that one's IDE will attempt to impose warnings on coding style. In this case, use a `#pragma warning disable (...)` directive after the `using` statements, and a `#pragma warning restore (...)` directive at the end of the file. (In reverse order of the first directives)

## IV.M: Indentation of Long Statements/Expressions
In general, when a line of code gets too long, (no specific number of columns dictates what is "too long," one's best judgement is to be used) a newline shoud be inserted such that an operator appears as the first non-whitespace character on the next line. At this point, in general, the number of *extra* tabs to use for the next line should be 1 plus the level of parentheses or brackets the newline occerred at. For example:

```csharp
int aLongVariableName = _anotherLongVariableName
	+ (SomeOtherLongVariableName - yetAnotherLongVariableName
		/ _thisOtherLongVariableName)
	- TheLastLongVariableName;
```

## IV.N: Always Specify Access Level
Access levels such as `private`, `protected`, or `public` should never be omitted when available, since default access levels differ across `class`es/`struct`s in C#, these keywords should always be present to make access levels clear.

## IV.O: Environment-specific Newlines
`Environment.NewLine` should be used instead of `\n` at all times to avoid newline ambiguity across platforms, unless not possible for multi-platform data.

## IV.P: Use Defined Constants over Literals when Available
For better readability, defined constants like `string.Empty` should be used over their literal counterparts like `""` in all cases. Unity also provides constants like `Vector2.zero` which should be used instead of `new Vector2(0.0f, 0.0f)`, and `Color.green` which should be used instead of `new Color(0.0f, 1.0f, 0.0f, 1.0f)`.

## IV.Q: String Interpolation over Concatenation
String interpolation should be used in place of string concatenation at all times. For example, `$"Score: {numPoints}"` instead of `"Score: " + numPoints`.
For newlines, or to break a string in code, use the verbatim identifier (`@`) like so:

```csharp
$@"Line one,{Environment.NewLine
}Line Two."
```

or

```csharp
$@"Line one, {string.Empty
}still line one."
```

## IV.R: `System.Action<>`s or `System.Func<>`s over `delegate` Declarations
`System.Action<>` and `System.Func<>` provide all required functionality of `delegate` declarations, and therefore should be used instead for simplicity and uniformity.

### IV.R.1: `delegate`s vs `event`s
`System.Action<>` or `System.Func<>` `delegate`s should be used where zero or one functions need to be called, or when a return value is needed. If there is the possibility of needing more than one function to be called, *and* order of execution is not important, then `event`s should be used. For Example:

```csharp
public class Example : MonoBehaviour
{
	private event System.Action _exampleEvent;

	private void Start()
	{
		_exampleEvent += EventFunctionA;
		_exampleEvent += EventFunctionB;
		StartCoroutine(AsyncFunction(_exampleEvent));
	}

	private void EventFunctionA()
	{
		_exampleEvent -= EventFunctionA;
		// ...
	}

	private void EventFunctionB()
	{
		_exampleEvent -= EventFunctionB;
		// ...
	}

	private IEnumerator AsyncFunction(System.Action onFinished)
	{
		yield return new WaitForSeconds(1.0f);
		onFinished();
	}
}
```

## IV.S: `for` vs `foreach` vs `ForEach(...)`
Classic `for` loops should be used when the iteration index's value is required in the loop, which should always be `i`, or if `i` is not available, then `j`, then `k`, etc. Whenever possible, the value the iteration index counts towards should be cached in the initialization statement of the `for` loop, named `iMax` (or `jMax`, or `kMax`, etc, as the case may be) and prefix increment operators should be preferred to postfix operators, like so: `for (int i = 0, iMax = numbers.Length; i < iMax; ++i) { /* ... */ }`. `foreach` loops should be replaced with `ForEach(...)` functions wherever possible. If one does not yet exist for the collection a programmer wants to use it on, a programmer can easily make an extension function for this purpose, and add it to the `static` utility `class` mentioned in section V.H. If a *copy* is needed to be iterated on, then `.ToList().ForEach(...)` (or `.ToArray().ForEach(...)` if it was written in the aforementioned utility class) is preferable. An example of such a `ForEach(...)` function is:

```csharp
public static void ForEach<T>(this T[] array, Action<T> action)
{
	Array.ForEach(a, action);
}
```

## IV.T: Prefer Iterated Lists to Repeated Function Calls
If the same function is to be called several times in a row on several distinct identifiers, like so:

```csharp
someObject.SomeMethod();
anotherObject.SomeMethod();
lastObject.SomeMethod();
```

Instead, an array literal with a `ForEach(...)` function and a lambda should be used, like so: `new SomeType[] { someObject, anotherObject, lastObject }.ToList().ForEach(o => o.SomeMethod());`

## IV.U: Iterate over Shallow Copies of Collections for Safety
Recall that read/write permissions refer only to object references themselves; the state they reference is still modifiable even if the reference ifself is `readonly` to a given scope. This should be kept in mind when writing state declatations and especially so with collection/enumberable types such as arrays. A relatively common, yet sometimes extremely difficult to track down, (or worse, discover) bug is when an array is modified inadvertently within a `for` loop, especially through several function calls. To prevent this from happening, consider (or require) only iterating over shallow copies of a collection using either `System.Linq`'s `ToArray()` or `ToList()`, or even better, a `ReadOnlyCollection<T>` object from a `ToReadOnly()` method, like so:

```csharp
public class CollectionExample
{
	private	List<int>	_scoreList	= new List<int>();
	public	List<int>	ScoreList	{ get { return _scoreList.ToList(); } }

	public void RemoveScore(int score)
	{
		_scoreList.Remove(score);
	}
}

// ...

foreach (int score in collectionExample.ScoreList)
{
	if (score < 0)
	{
		collectionExample.RemoveScore(score);
	}
}
```

## IV.V: Add a Trailing Comma to All Comma-Separated Lists
Add a trailing comma to all `enum`s and other identifiers separated by commas, such as array literals. Doing so makes it easier to add and remove options from the list, and makes source conrtol diffs look better, for example,

```csharp
public enum TrafficLightState
{
	GREEN,
	YELLOW,
	RED,
}

var trafficLightStates = new TrafficLightState[]
{
	GREEN,
	GREEN,
	GREEN,
	RED,
	YELLOW,
	YELLOW,
};
```

# V: Structural Considerations
## V.A: Do not check for `null` unless you *expect* `null`
If something is left `null`, either because it was not serialized by the editor, or because it was not set in code, a `NullReferenceException` makes this common issue much easier to debug than code simply not running because a `null` check failed without raising an exception. This sort of behavior is much more typical of a more complicated logic bug than an unexpected `null` reference.

## V.B: Use Functions *Only* for Reusable Code
Following from IV.H, functions have only one purpose: to organize code that would otherwise be repeated, or nearly repeated, into a reusable block of code with its own local scope, possibly with parameters to alter its behavior. Large functions should _not_ be broken up into smaller ones simply for the purpose of trying to break up a large, but contiguous, block of code. Doing so detroys the locality of the code, (or at least has the potential to do so, if another programmer decides to put any unrelated function(s) between any of the functions resulting from the larger broken-up function) making it harder to debug an issue across several code blocks.

## V.C: Minimize the Total Number of Files in the Project
This goes hand-in-hand with not breaking up functions into smaller ones unless they are to be reused: a file which contains simply contains many lines of code should not be broken up into more files unless it actually makes sense to do so architecturally.

## V.D: No Dead Code
Dead code, meaning code that not reachable from any entry point, (Unity has several entry points into C# such as the `Start()` and `Update()` functions) are never to be submitted to source control, and should be removed locally and in source control immediately upon detection.

## V.E: Coroutines vs. Threads vs. Jobs
The choice between Coroutines, Threads, and the newer Unity Jobs system for asynchronous operations should be made based on the attributes that best suit the operation. Coroutines should be used for operations where multithreaded performance is not needed, as they still occur on the main thread over multiple frames (if they `yield` control). This has the added benefit of also not requiring manual thread synchronization via Mutexes and/or Semaphores. If multithreaded performance is needed, the Unity Jobs system is preferable to C# Threads, as the former is better integrated with Unity, as described [here](https://docs.unity3d.com/Manual/JobSystemOverview.html), however there are some operations it can't do, like prematurely ending an asynchronous operation like a thread can, albeit through [archaic means](https://docs.microsoft.com/en-us/dotnet/standard/threading/cancellation-in-managed-threads). In such scenarios, threads are acceptable, but should be limited to doing work that requires limited synchronization with the main thread, typically that which involves much raw computation that can be easily parallelized.

## V.F: Permanent Storage
If it is necessary to write state to permanent storage, it should be done via a special purpose Singleton class or classes. This (these) class(es) will follow a common pattern of lazy evaluation as soon as their singleton(s) is (are) first accessed during execution: the state will be loaded (or created as a default object if not present) from the disk on lasy evaluation, and will be saved to the disk on every modification of its (their) state via `get` and/or `set` properties. WARNING: While primitives will work as normal, Rrferences to data structures (such as `Array<>`s or `Dictionary<>`s) need to be either reassigned on every modification via `set` property, or with the use of a helper function, AND they must also be checked for `null` when they are loaded from the disk in order to prevent `null` data from being loaded in from an obselete file (perhaps from an earlier version of the saved data) as demonstrated below...

```csharp
[Serializable]
public class GameData
{
	private static string SavedSettingsFilePath
	{
		get
		{
			return System.IO.Path.Combine(Application.persistentDataPath, "YourSaveFileName.dat");
		}
	}

	private static GameData _cachedInstance;
	public static GameData Get
	{
		get
		{
			if (_cachedInstance == null)
			{
				ReadFromDisk();
			}
			return _cachedInstance;
		}
	}

	private string[] _stringArray;
	public string[] StringArray
	{
		get
		{
			return _stringArray;
		}
		set
		{
			_stringArray = value;
			WriteToDisk();
		}
	}

	private bool _documentaryButtons;
	public bool DocumentaryButtons
	{
		set
		{
			_documentaryButtons = value;
			WriteToDisk();
		}
		get
		{
			return _documentaryButtons;
		}
	}

	public Dictionary<string, int> StringIntDict { get; private set; }

	public void SetLevelPositionData(string name, int number)
	{
		StringIntDict.Add(name, number);
		WriteToDisk();
	}

	private void WriteToDisk()
	{
		var formatter = new System.Runtime.Serialization.Formatters.Binary.BinaryFormatter();
		using (var stream = new System.IO.FileStream(SavedSettingsFilePath, System.IO.FileMode.Create, System.IO.FileAccess.Write, System.IO.FileShare.None))
		{
			formatter.Serialize(stream, this);
		}
	}

	private static void ReadFromDisk()
	{
		try
		{
			var formatter = new System.Runtime.Serialization.Formatters.Binary.BinaryFormatter();
			using (var stream = new System.IO.FileStream(SavedSettingsFilePath, System.IO.FileMode.Create, System.IO.FileAccess.Write, System.IO.FileShare.None))
			{
				_cachedInstance = (GameData)formatter.Deserialize(stream);
			}
		}
		catch
		{
			_cachedInstance = new GameData();
			_cachedInstance.WriteToDisk();
		}

		if (_cachedInstance._stringArray == null)
		{
			_cachedInstance._stringArray = new string[0];
		}
		if (_cachedInstance.StringIntDict == null)
		{
			_cachedInstance.StringIntDict = new Dictionary<string, int>();
		}
	}
}
```

This permanent state can then be used in code like so:

```
if (GameData.Get.DocumentaryButtons)
{
	// ...
}

GameData.Get.StringArray = new string[] { "String1, "String2" };
```

*WARNING:* Reference types like `Array<T>`s have to be assigned via the = operator, because assigning to a particular index of an array, or using a method like `.Add(...)` on a `Dictionary<K, V>` will not write the state to the disk through the `set` property. They *also* need to be initialized to empty collections if `null`, in order to protect against deserialization of older versions of the data, like at the end of the `ReadFromDisk()` function in the above example. As with III.A.1, bespoke collections can be used to get around this limitation.

*WARNING:* `BinaryFormatter` has been found by Microsoft to be [unsafe](https://docs.microsoft.com/en-us/dotnet/standard/serialization/binaryformatter-security-guide). Be sure to consider all options when it comes to serialization/deserialization, potentially with encryption.

## V.G: Love Lambdas
Lambda functions are a very useful tool for writing cleaner code. In addition to their ability to act as more verbose (and yet cleaner) inline function pointers for things like sorting algorithms, they also make for a great callback mechanism and should definitely be used for functions that need to be called at the end of a long processs such as an animation, for a user interaction such as a button, or anywhere else where more than one behavior is necessary as part of a function's execution. They should also be used instead of anonymous methods, and their parameters (if present) should be single letters, surrounded by parentheses only if more than one is present, like so: `numbers.All(n => n > 5)` or `numbers.Sort((a, b) => a > b);` If a lambda function is used more than once, or is recursive, use a local function instead. For example, instead of

```csharp
Func<int, int> fibonacci = null; // Recursive lambdas must be initailized and then reassigned.
fibonacci = n => n <= 1 ? n : fibonacci(n - 1) + fibonacci(n - 2);
PrintToFile(fibonacci(5));
PrintToScreen(fibonacci(5));
```

Use

```csharp
int fibonacci(int n) => n <= 1 ? n : fibonacci(n - 1) + fibonacci(n - 2); // Local functions can be recursive.
PrintToFile(fibonacci(5));
PrintToScreen(fibonacci(5));
```

### V.G.1: Closures avoid `private`s
In accordance with II.B.3, local variables are preferred over `private` fields. In the context of asynchronous operations, lambdas/local functions allow `private` fields to be replaced with local variables. For example, instead of

```csharp
public class AsyncExample : MonoBehaviour
{
	private GameObject _objectToDeactivate;

	private void AnimateOutOfScreen(AnimatableGameObject animatable)
	{
		// Assume OnScreenOutFinished is an event in this example.
		animatable.OnScreenOutFinished += AnimationFinished;
		_objectToDeactivate = animatable;
		animatable.AnimateOutOfScreen();
	}

	private void AnimationFinished()
	{
		_objectToDeactivate.OnScreenOutFinished -= AnimationFinished;
		_objectToDeactivate.SetActive(false);
		_objectToDeactivate = null;
	}
}
```

Which, in addition to using an `event` instead of a `System.Action onFinished` parameter in the `AnimateOutOfScreen()` function, keeps that unnecessary `_objectToDestroy` reference in a `private` field. This results in needing to remember to write *both* `_objectToDestroy.OnScreenOutFinished -= AnimationFinished;` *and* `_objectToDestroy = null;` for *every* such asynchronous operation in a class, which can quickly result in not only bloated, but also error-prone code. Instead, the following will take care of cleaning up any references that are needed simply by allowing them to fall out of scope when executing via a closure:

```csharp
public class AsyncExample : MonoBehaviour
{
	private void AnimateOutOfScreen(AnimatableGameObject animatable)
	{
		// Assume AnimateOutOfScreen has a `System.Action onFinished` parameter in this example.
		animatable.AnimateOutOfScreen(onFinished: () => animatable.SetActive(false));
	}
}
```

It's quite remarkable how many lines and potential errors are saved in second example. This is a good illustration of the benefits that functional style programming features can have on the imperative/object-oriented programming paradigm.

### V.G.2: Functional Style
Another great use for lambdas is that of the functional-style LINQ methods like [.Any(...)](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.any), [.All(...)](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.all), [.Aggregate(...)](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.aggregate), [.Select(...)](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.select), [.Where(...)](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.where), etc. Use these wherever possible.

## V.H: Extract Utilities
Generic, reusable code should be identified and rewritten as a function (preferably as an extenstion function) whenever possible, and put into a `static` utility `class` file. (Or several files if it is deemed structurally necessary)

## V.I: Conditional Compilation for Multiplatform Code
When writing a game that is to be run on multiple platforms, if a signficant amount of the code is to be shared between said platforms, and if the code is similar enough that separate source control branches/forks are considered to be too much of a hassle to deal with in regards to constantly merging code, conditional compilation with the directives defined [here](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html) should be used to distinguish code of one platform from code of another.

## V.J: Proper Use of `HideInInspector`
Note that the `HideInInspector` attribute has a very specific use: it is to be used only when data *is* to be serialized and *is not* to be made visible in the inspector. An example of this is data that needs to be serialized automatically in a script run before a build, and not by hand in the inspector. This behavior is different than the `NonSerialized` attribute, which both specifies that a field is not to be serialized, and therefore also should not appear in the inspector.
