# FakeItEasy Documentation (Combined)

Generated from `FakeItEasy/FakeItEasy` (`master`)

Ordering source: `docs/mkdocs.yml` (when parseable)


## Contents

- [mkdocs.yml](#mkdocsyml)
- [index.md](#indexmd)
- [quickstart.md](#quickstartmd)
- [creating-fakes.md](#creating-fakesmd)
- [what-can-be-faked.md](#what-can-be-fakedmd)
- [default-fake-behavior.md](#default-fake-behaviormd)
- [how-to-fake-internal-types.md](#how-to-fake-internal-typesmd)
- [specifying-a-call-to-configure.md](#specifying-a-call-to-configuremd)
- [specifying-return-values.md](#specifying-return-valuesmd)
- [throwing-exceptions.md](#throwing-exceptionsmd)
- [doing-nothing.md](#doing-nothingmd)
- [assigning-out-and-ref-parameters.md](#assigning-out-and-ref-parametersmd)
- [invoking-custom-code.md](#invoking-custom-codemd)
- [calling-base-methods.md](#calling-base-methodsmd)
- [calling-wrapped-methods.md](#calling-wrapped-methodsmd)
- [changing-behavior-between-calls.md](#changing-behavior-between-callsmd)
- [argument-constraints.md](#argument-constraintsmd)
- [assertion.md](#assertionmd)
- [ordered-assertions.md](#ordered-assertionsmd)
- [capturing-arguments.md](#capturing-argumentsmd)
- [faking-async-methods.md](#faking-async-methodsmd)
- [raising-events.md](#raising-eventsmd)
- [strict-fakes.md](#strict-fakesmd)
- [dummies.md](#dummiesmd)
- [custom-dummy-creation.md](#custom-dummy-creationmd)
- [implicit-creation-options.md](#implicit-creation-optionsmd)
- [formatting-argument-values.md](#formatting-argument-valuesmd)
- [custom-argument-equality.md](#custom-argument-equalitymd)
- [scanning-for-extension-points.md](#scanning-for-extension-pointsmd)
- [bootstrapper.md](#bootstrappermd)
- [advanced-usage.md](#advanced-usagemd)
- [faking-http-client.md](#faking-http-clientmd)
- [platform-support.md](#platform-supportmd)
- [contributing.md](#contributingmd)
- [how-to-build.md](#how-to-buildmd)
- [why-was-fakeiteasy-created.md](#why-was-fakeiteasy-createdmd)
- [external-resources.md](#external-resourcesmd)


---


## mkdocs.yml

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/mkdocs.yml



site_name: FakeItEasy
site_dir: ../artifacts/docs
repo_url: https://github.com/FakeItEasy/FakeItEasy
extra_css: [css/fakeiteasy.css]
docs_dir: .
exclude_docs: |
    .python-version
    mkdocs.yml
    pyproject.toml
    uv.lock

extra:
  version:
    provider: mike

theme:
  name: material
  favicon: img/favicon.ico
  logo: img/icon.svg
  features:
    - navigation.footer

  palette:

    # Palette toggle for light mode
    - media: "(prefers-color-scheme: light)"
      scheme: default
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode

    # Palette toggle for dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      toggle:
        icon: material/brightness-4
        name: Switch to light mode

plugins:
  - mike:
      canonical_version: null
      css_dir: css
      javascript_dir: js
      version_selector: true
  - search

markdown_extensions:
  - footnotes
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.details
  - pymdownx.inlinehilite
  - pymdownx.snippets:
      base_path: !ENV [SNIPPETS_REPO_ROOT, '..']
      check_paths: true
      dedent_subsections: true
  - pymdownx.superfences
  - pymdownx.tabbed:
        alternate_style: true
  - pymdownx.tilde

nav:
- FakeItEasy: index.md
- Quickstart: quickstart.md
- Creating Fakes:
  - Creating Fakes: creating-fakes.md
  - What can be faked: what-can-be-faked.md
  - Default fake behavior: default-fake-behavior.md
  - How to fake internal (Friend in VB) types: how-to-fake-internal-types.md
- Specifying Fakes’ Behavior:
  - Specifying a Call to Configure: specifying-a-call-to-configure.md
  - Specifying Return Values: specifying-return-values.md
  - Throwing Exceptions: throwing-exceptions.md
  - Doing Nothing: doing-nothing.md
  - Assigning out and ref parameters: assigning-out-and-ref-parameters.md
  - Invoking Custom Code: invoking-custom-code.md
  - Calling base methods: calling-base-methods.md
  - Calling wrapped methods: calling-wrapped-methods.md
  - Changing behavior between calls: changing-behavior-between-calls.md
- Argument constraints: argument-constraints.md
- Assertion:
  - Assertion: assertion.md
  - Ordered assertions: ordered-assertions.md
- Capturing arguments: capturing-arguments.md
- Faking async methods: faking-async-methods.md
- Raising events: raising-events.md
- Strict fakes: strict-fakes.md
- Dummies:
  - Dummies: dummies.md
  - Custom Dummy Creation: custom-dummy-creation.md
- Implicit Creation Options: implicit-creation-options.md
- Formatting Argument Values: formatting-argument-values.md
- Custom Argument Equality: custom-argument-equality.md
- Scanning for Extension Points: scanning-for-extension-points.md
- Bootstrapper: bootstrapper.md
- Advanced usage: advanced-usage.md
- Analyzer Packages: https://fakeiteasy.github.io/docs/analyzers/
- Recipes:
  - Faking HttpClient: Recipes/faking-http-client.md
- Platform support: platform-support.md
- Contributing: contributing.md
- How to build: how-to-build.md
- Why was FakeItEasy created?: why-was-fakeiteasy-created.md
- External Resources: external-resources.md



---


## index.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/index.md



---
hide:
  - toc
---
<div id="logo"></div>

FakeItEasy is a .NET dynamic fake library for creating all types of fake objects, mocks, stubs etc.

* Easier semantics, all fake objects are just that - fakes - the use of the fakes determines whether they're mocks or stubs.
* Context-aware fluent interface guides the developer.
* Designed for ease of use.
* Full compatibility with both C# and VB.NET.

#### It's faking amazing!

* [Website](https://fakeiteasy.github.io/)
* [Quickstart](quickstart.md)
* [Chat](https://app.gitter.im/#/room/#FakeItEasy_FakeItEasy:gitter.im)
* [Package](https://www.nuget.org/packages/FakeItEasy "FakeItEasy on NuGet")

----
FakeItEasy logo designed by Vanja Pakaski.



---


## quickstart.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/quickstart.md



### Quickstart

Getting started with FakeItEasy is very simple:

* Open the Package Manager Console:  
Tools → Library Package Manager → Package Manager Console
* Execute `Install-Package FakeItEasy`
* Start faking dependencies in your tests. Here's a sample test class with fakes:

```csharp
namespace FakeItEasyQuickstart
{
    using FakeItEasy;
    using NUnit; // any test framework will do

    public class SweetToothTests
    {
        [Test]
        public void BuyTastiestCandy_should_buy_top_selling_candy_from_shop()
        {
            // make some fakes for the test
            var lollipop = A.Fake<ICandy>();
            var shop = A.Fake<ICandyShop>();

            // set up a call to return a value
            A.CallTo(() => shop.GetTopSellingCandy()).Returns(lollipop);

            // use the fake as an actual instance of the faked type
            var developer = new SweetTooth();
            developer.BuyTastiestCandy(shop);

            // asserting uses the exact same syntax as when configuring calls—
            // no need to learn another syntax
            A.CallTo(() => shop.BuyCandy(lollipop)).MustHaveHappened();
        }
    }
}
```

* Most FakeItEasy functionality is reached from a common entry point: the `A` class.
* In this example the `lollipop` instance is used as a stub and the `shop` instance is used as a mock but there's no need to know the difference, just fake it! Easy!
* Fluent, easy-to-use syntax guides you as you configure fakes.


---


## creating-fakes.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/creating-fakes.md



### Creating Fakes

#### Natural fakes
The common way to create a fake object is by using the `A.Fake` syntax, for example:

```csharp
var foo = A.Fake<IFoo>();
```
This will return a faked object that is an actual instance of the type specified (`IFoo` in this case).

You can create a fake delegate with the same syntax:
```csharp
var func = A.Fake<Func<string, int>>();
```

You can also create a collection of fakes by writing:
```csharp
var foos = A.CollectionOfFake<Foo>(10);
```

For cases where the type to fake isn't statically known, non-generic methods are also available. These are usually only required when writing extensions for FakeItEasy, so they live in the `FakeItEasy.Sdk` namespace:
```csharp
using FakeItEasy.Sdk;
...
var type = GetTypeOfFake();
object fake = Create.Fake(type);
IList<object> fakes = Create.CollectionOfFake(type, 10);
```

#### Explicit Creation Options
When creating fakes you can, through a fluent interface, specify options for how the fake should be created, depending on the type of fake being made:

| Option                                                                                            | Applies to    |
|---------------------------------------------------------------------------------------------------|---------------|
| Specify arguments for the constructor of the faked type                                           | non-delegates |
| Specify additional interfaces that the fake should implement                                      | non-delegates |
| Assign additional custom attributes to the faked type                                             | non-delegates |
| Cause a fake to have [strict mocking semantics](strict-fakes.md)                                  | any fake      |
| Configure all of a fake's methods to [use their original implementation](calling-base-methods.md) | classes       |
| Create a fake that wraps another object                                                           | any fake      |
| Create a named fake                                                                               | any fake      |

Examples:

```csharp
// Specifying arguments for constructor using an expression. This is refactoring friendly!
// Since the constructor call seen here is an expression, it is not invoked. Instead, the
// constructor arguments will be extracted from the expression and provided to
// the fake class's constructor. (Of course the fake class, being a subclass of `FooClass`,
// ultimately invokes `FooClass`'s constructor with these arguments.)
var foo = A.Fake<FooClass>(x => x.WithArgumentsForConstructor(() => new FooClass("foo", "bar")));

// Specifying arguments for constructor using IEnumerable<object>.
var foo = A.Fake<FooClass>(x => x.WithArgumentsForConstructor(new object[] { "foo", "bar" }));

// Specifying additional interfaces to be implemented. Among other uses,
// this can help when a fake skips members because they have been
// explicitly implemented on the class being faked.
var foo = A.Fake<FooClass>(x => x.Implements(typeof(IFoo)));
// or
var foo = A.Fake<FooClass>(x => x.Implements<IFoo>());

// Assigning custom attributes to the faked type.
// foo's type should have "FooAttribute"
var foo = A.Fake<IFoo>(x => x.WithAttributes(() => new FooAttribute()));

// Create wrapper - unconfigured calls will be forwarded to wrapped
var wrapped = new FooClass("foo", "bar");
var foo = A.Fake<IFoo>(x => x.Wrapping(wrapped));

// Create a named fake, for easier identification in error messages and using ToString()
// Note that for a faked delegate, ToString() won't return the name, because only the Invoke
// method of a delegate can be configured.
var foo = A.Fake<IFoo>(x => x.Named("Foo #1"));
```

#### Implicit Creation Options

[Implicit creation options](implicit-creation-options.md) are
available, equivalent in power to the explicit creation options
mentioned above.

#### Unnatural fakes

For those accustomed to [Moq](https://github.com/devlooped/moq/) there is an
alternative way of creating fakes through the `new Fake<T>`
syntax. The fake provides a fluent interface for configuring the faked
object:

```csharp
var fake = new Fake<IFoo>();
fake.CallsTo(x => x.Bar("some argument")).Returns("some return value");

var foo = fake.FakeObject;
```

For an alternative look at migrating from Moq to FakeItEasy, see Daniel Marbach's blog post that talks about [Migration from Moq to FakeItEasy with Resharper Search Patterns](https://www.planetgeek.ch/2013/07/18/migration-from-moq-to-fakeiteasy-with-resharper-search-patterns/).


---


## what-can-be-faked.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/what-can-be-faked.md



### What can be faked

#### What types can be faked?

FakeItEasy uses
[Castle DynamicProxy](https://www.castleproject.org/projects/dynamicproxy/)
to create fakes. Thus, it can fake just about anything that could
normally be overridden, extended, or implemented.  This means that the
following entities can be faked:

* interfaces
* classes that
    * are not sealed,
    * are not static, and
    * have at least one public or protected constructor whose arguments FakeItEasy can construct or obtain
* delegates

Note that special steps will need to be taken to
[fake internal interfaces and classes](how-to-fake-internal-types.md).

##### Types whose methods have `in` parameters

Due to deficiencies in earlier .NET framework releases, generic types that contain methods having
a parameter modified by the `in` keyword cannot be faked by FakeItEasy running on target frameworks
earlier than .NET 6.

##### Where do the constructor arguments come from?
  
* they can be supplied via `WithArgumentsForConstructor` as shown in
  [creating fakes](creating-fakes.md), or
* FakeItEasy will use [dummies](dummies.md) as arguments

#### What members can be overridden?

Once a fake has been constructed, its methods and properties can be
overridden if they are:

* virtual,
* abstract, or
* an interface method when an interface is being faked

Note that this means that static members, including extension methods,
**cannot** be overridden.

##### Methods that return values by reference

Methods that return values by reference (officially called "[reference return values](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/declarations#what-is-a-reference-return-value)") cannot be invoked on a Fake. Any attempt to do so will result in a `NullReferenceException` being thrown.


---


## default-fake-behavior.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/default-fake-behavior.md



### Default fake behavior

Fake objects come with useful default behavior as soon as
[they are created](creating-fakes.md). Knowing the default behavior
can make the fakes easier to work with and can lead to more concise
tests.

#### Non-overrideable members cannot be faked

Methods and properties can only be faked if they are declared on a
faked interface, or are declared abstract or virtual on a faked
class. If none of these conditions hold, then a member cannot be
faked, just as it could not be overridden in a derived class.

When such a member is invoked on the fake, the original behavior will be invoked.

#### Overrideable members are faked

When a method or property is declared on a faked interface
(this includes default interface members), or is
declared as abstract or virtual on a faked class, and the member is
invoked on the fake, no action will be taken by the fake. It is as if
the body of the member were empty. If the member has a return type (or
is a get property), the return value will depend on the type `T` of
the member:

* If `T` can be made into a [Dummy](dummies.md), then a Dummy `T` will
  be returned. Note that this may be a Fake or an instance of a
  concrete, pre-existing type;
* otherwise, `default(T)` will be returned.

##### Read/write properties

By default, any overrideable property that has both a `set` and `get` accessor,
and that has not been explicitly configured to behave otherwise, behaves like
you might expect. Setting a value and then getting the value returns the value
that was set.

```csharp
var fakeShop = A.Fake<ICandyShop>();

fakeShop.Address = "123 Fake Street";

System.Console.Out.Write(fakeShop.Address);
// prints "123 Fake Street"
```

This behavior can be used to

* supply values for the system under test to use (via the getter) or to
* verify that the system under test performed the `set` action on the Fake

##### Object members

Virtual methods inherited from `System.Object` are faked with a special default behavior:

* `Equals` uses reference equality: it returns `true` if the argument is the fake
  itself, `false` for any other argument.
* `GetHashCode` returns a hashcode consistent with the behavior of `Equals`.
* `ToString` returns a string of the form "Faked &lt;type of fake&gt;"

This behavior applies to all fakes, including fakes of types that override these
methods. Like any other method, the behavior can be explicitly configured.

##### Cancellation tokens

When a faked method that accepts a `CancellationToken` receives a canceled token
(i.e. with `IsCancellationRequested` set to `true`), it will either:

* return a canceled task, if it is asynchronous (i.e. if it returns a `Task`,
  `Task<T>`, `ValueTask` or `ValueTask<T>`);
* throw an `OperationCanceledException`, if it is synchronous.

#### Examples

Suppose we have the following interface definition

```csharp
public interface Interface
{
    bool BooleanFunction();
    int IntProperty { get; set; }
    string StringFunction();
    FakeableClass FakeableClassFunction();
    UnfakeableClass UnfakeableClassProperty { get; set; }
    Struct StructFunction();
}
```

Then the following test will pass

```csharp
public void Members_should_return_empty_string_default_or_fake_another_fake()
{
    var fakeLibrary = A.Fake<Interface>();

    Assert.AreEqual(default(bool), fakeLibrary.BooleanFunction());

    Assert.AreEqual(default(int), fakeLibrary.IntProperty);

    Assert.AreEqual(typeof(string), fakeLibrary.StringFunction().GetType());
    Assert.AreEqual(string.Empty, fakeLibrary.StringFunction());

    Assert.IsInstanceOfType(fakeLibrary.FakeableClassFunction(), typeof(FakeableClass));
    Assert.AreEqual("FakeableClassProxy",
                    fakeLibrary.FakeableClassFunction().GetType().Name); // to show it's a fake

    Assert.IsNull(fakeLibrary.UnfakeableClassProperty);

    Assert.AreEqual(default(Struct), fakeLibrary.StructFunction());
}
```


---


## how-to-fake-internal-types.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/how-to-fake-internal-types.md



### How to fake internal (Friend in VB) types

This guide will show you how to set up your project in order to be
able to fake internal types in your tested system.

#### Details

The assembly that generates the proxy instances must have access to
your internal types, therefore a `InternalsVisibleTo` attribute must
be added to your tested assembly. Note that it is the assembly under
test, not your test-assembly that needs this attribute.

##### Unsigned assemblies

If your assembly is not signed with a strong name, it's as easy as
adding the following to your project file (assuming you're using
.NET 5 SDK or higher):

```xml
<ItemGroup>
  <InternalsVisibleTo Include="DynamicProxyGenAssembly2" />
</ItemGroup>
```

If you're using an older SDK, you can add this to your
AssemblyInfo.cs/vb file instead:

=== "C#"
    ```csharp
    [assembly: InternalsVisibleTo("DynamicProxyGenAssembly2")]
    ```
=== "VB"
    ```vb
    <Assembly:InternalsVisibleTo("DynamicProxyGenAssembly2")>
    ```

##### Signed assemblies

For signed assemblies, you have to specify the public key of the
proxy-generating assembly in your project file:

```xml
<ItemGroup>
  <InternalsVisibleTo Include="DynamicProxyGenAssembly2" Key="0024000004800000940000000602000000240000525341310004000001000100c547cac37abd99c8db225ef2f6c8a3602f3b3606cc9891605d02baa56104f4cfc0734aa39b93bf7852f7d9266654753cc297e7d2edfe0bac1cdcf9f717241550e0a7b191195b7667bb4f64bcb8e2121380fd1d9d46ad2d92d2d15605093924cceaf74c4861eff62abf69b9291ed0a340e113be11e6a7d3113e92484cf7045cc7" />
</ItemGroup>
```

Or, if using an older SDK, in your AssemblyInfo.cs/vb file:
=== "C#"
    ```csharp
    [assembly: InternalsVisibleTo("DynamicProxyGenAssembly2, PublicKey=0024000004800000940000000602000000240000525341310004000001000100c547cac37abd99c8db225ef2f6c8a3602f3b3606cc9891605d02baa56104f4cfc0734aa39b93bf7852f7d9266654753cc297e7d2edfe0bac1cdcf9f717241550e0a7b191195b7667bb4f64bcb8e2121380fd1d9d46ad2d92d2d15605093924cceaf74c4861eff62abf69b9291ed0a340e113be11e6a7d3113e92484cf7045cc7")]
    ```
=== "VB"
    ```vb
    <Assembly:InternalsVisibleTo("DynamicProxyGenAssembly2, PublicKey=0024000004800000940000000602000000240000525341310004000001000100c547cac37abd99c8db225ef2f6c8a3602f3b3606cc9891605d02baa56104f4cfc0734aa39b93bf7852f7d9266654753cc297e7d2edfe0bac1cdcf9f717241550e0a7b191195b7667bb4f64bcb8e2121380fd1d9d46ad2d92d2d15605093924cceaf74c4861eff62abf69b9291ed0a340e113be11e6a7d3113e92484cf7045cc7")>
    ```


---


## specifying-a-call-to-configure.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/specifying-a-call-to-configure.md



### Specifying a Call to Configure

One of the first steps in configuring a fake object's behavior is to
specify which call to configure. Like most FakeItEasy actions, this is
done using a method on the `A` class: `A.CallTo`.

#### Specifying a method call or property `get` using an Expression

```csharp
A.CallTo(() => fakeShop.GetTopSellingCandy()).Returns(lollipop);
A.CallTo(() => fakeShop.Address).Returns("123 Fake Street");
```

The expressions in the above example are not evaluated by FakeItEasy:
no call to `GetTopSellingCandy` or `Address` is made. The expressions
are just used to identify which call to configure, after which
`A.CallTo` returns an object that can be used to specify how the fake
should behave when the call is made.

???+ note "Use an action to complete call configuration"
    Specifying the call via `A.CallTo` and related methods does not
    have any effect on the fake object. You must include an action
    for the call to perform to alter the behavior of the fake object.

    There are many types of actions that can be specified, including
    [returning various values](specifying-return-values.md),
    [throwing exceptions](throwing-exceptions.md), and more. Even
    [doing nothing](doing-nothing.md), which may be required for a
    void method.

#### Specifying a call to a property setter

Assignment operators can't be used in lambda expressions, so the
`A.CallTo` overloads described above cannot be used to configure calls
to property setters.
Use `A.CallToSet` to configure the `set` behavior of read/write properties:

```csharp
A.CallToSet(() => fakeShop.Address).To("123 Fake Street").CallsBaseMethod();
A.CallToSet(() => fakeShop.Address).To(() => A<string>.That.StartsWith("123")).DoesNothing();
A.CallToSet(() => fakeShop.Address).DoesNothing(); // ignores the value that's set
```

[Argument constraints](argument-constraints.md) can be used to
constrain the value that's set into the property, or the indexes that
must be supplied when invoking an indexer.

Note that any customization of a read/write property's behavior will break the
default behavior of having
[the getter return the last set value](default-fake-behavior.md#readwrite-properties).
To avoid this, a
[custom action](invoking-custom-code.md#case-study-customizing-a-readwrite-property)
may be used to preserve the behavior.

#### Specifying the invocation of a delegate

To specify the invocation of a delegate, just use `A.CallTo`, invoking the fake delegate as you normally would:

```csharp
var deepThought = A.Fake<Func<string, int>>();
A.CallTo(() => deepThought.Invoke("What is the answer to life, the universe, and everything?")).Returns(42);

// Note that the .Invoke part is optional:
A.CallTo(() => deepThought("What is the answer to life, the universe, and everything?")).Returns(42);
```

#### Specifying a call to an explicitly implemented interface member

An explicitly implemented member is not directly visible on the concrete class. Instead, it has to be called
(or overridden) via the interface. In addition, since fakes don't automatically intercept explicitly implemented
interfaces, you need to explicitly specify that the fake implements the interface:

```csharp
var fakeShop = A.Fake<CandyShop>(options => options.Implements<ICandyShop>());
A.CallTo(() => ((ICandyShop)fakeShop).GetTopSellingCandy()).Returns(lollipop);
```

#### Specifying a call to any method or property

Instead of supplying an expression to identify a specific method, pass
the fake to `A.CallTo` to refer to any method on the fake:

```csharp
A.CallTo(fakeShop).Throws(new Exception());

// Or limit the calls to void methods
A.CallTo(fakeShop).WithVoidReturnType().Throws("sugar overflow");

// Or limit the calls by return type
A.CallTo(fakeShop).WithReturnType<string>().Returns("sugar tastes good");

// Or limit the calls to methods that return a value. Note that it will throw at runtime
// if the configured return value doesn't match the called method's return type.
A.CallTo(fakeShop).WithNonVoidReturnType().Returns("sugar tastes good");

// Or create a sophisticated test with a predicate that acts on an IFakeObjectCall.
// One use case is when the call's arguments include an object of an anonymous type,
// since it's impossible to create argument constraints for anonymous types.
A.CallTo(fakeShop).Where(call => call.Method.Name == "AnonymousArgEater")
                  .WithReturnType<string>()
                  .Returns("no-name candy");
```

`A.CallTo(object)` can also be used to specify write-only properties and
`protected` members:

```csharp
A.CallTo(fakeShop).Where(call => call.Method.Name == "ProtectedCalculateSalesForToday")
                  .WithReturnType<double>()
                  .Returns(4741.71);

// Use the conventional .NET prefix "get_" to refer to a property's getter:
A.CallTo(fakeShop).Where(call => call.Method.Name == "get_Address")
                  .WithReturnType<string>()
                  .Returns("123 Fake Street");

// Use the conventional .NET prefix "set_" to refer to a property's setter:
A.CallTo(fakeShop).Where(call => call.Method.Name == "set_Address")
                  .Throws(new Exception("we can't move"));
```

#### Specifying a call to an event accessor

Although calls to event accessors can be specified using the approach described
in the previous section, FakeItEasy also provides helper methods to make this
easier:

```csharp
// Specifies a call to the add accessor of the MyEvent event of the fake
A.CallTo(fake, EventAction.Add("MyEvent")).Invokes((EventHandler h) => ...);
// Specifies a call to the remove accessor of the MyEvent event of the fake
A.CallTo(fake, EventAction.Remove("MyEvent")).Invokes((EventHandler h) => ...);
// Specifies a call to the add accessor of any event of the fake
A.CallTo(fake, EventAction.Add()).Invokes(...);
// Specifies a call to the remove accessor of any event of the fake
A.CallTo(fake, EventAction.Remove()).Invokes(...);
```

#### VB.Net
Special syntax is provided to specify `Func`s and `Sub`s in VB, using their respective keywords:

```
A.CallTo(Sub() fakeShop.SellSomething())
                       .DoesNothing()

A.CallTo(Func() fakeShop.GetTopSellingCandy())
                        .Returns(lollipop)
```


---


## specifying-return-values.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/specifying-return-values.md



### Specifying Return Values

One of the most common tasks on a
[newly-created Fake](creating-fakes.md) is to specify the return value
for some method or property that might be called on it. This is often
done by using the `Returns` method on the result of an `A.CallTo`:

```csharp
A.CallTo(() => fakeShop.GetTopSellingCandy()).Returns(lollipop);
```

Now, whenever the parameterless method `GetTopSellingCandy` is called
on the `fakeShop` Fake, it will return the `lollipop` object.

A `get` property on a Fake can be configured similarly:
```csharp
A.CallTo(() => fakeShop.Address).Returns("123 Fake Street");
```

#### Return Values Calculated at Call Time

Sometimes a desired return value won't be known at the time the call
is configured. `ReturnsNextFromSequence` and `ReturnsLazily` can help
with that. `ReturnsNextFromSequence` is the simpler of the two:

```csharp
A.CallTo(() => fakeShop.SellSweetFromShelf())
                       .ReturnsNextFromSequence(lollipop, smarties, wineGums);
```

will first return `lollipop`, then `smarties`, then `wineGums`. The
next call will not take an item from the sequence, but will rely on
other configured (or default) behavior.

On to the very powerful `ReturnsLazily`:

```csharp
// Returns the number of times the method has been called
int sweetsSold = 0;
A.CallTo(() => fakeShop.NumberOfSweetsSoldToday()).ReturnsLazily(() => ++sweetsSold);
```

If a return value depends on input to the method, those values can be
incorporated in the calculation. Convenient overloads exist for
methods of up to 8 parameters.

```csharp
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(A<DateTime>.Ignored)) 
                       .ReturnsLazily((DateTime theDate) => 
                                          theDate.DayOfWeek == DayOfWeek.Sunday ? 0 : 200);
```

The convenience methods may be used with methods that take `out` and
`ref` parameters. This means that the previous example would work even
if `NumberOfSweetsSoldOn` took an `out DateTime` or a `ref DateTime`.

Note that the type of the `Func` sent to `ReturnsLazily` isn't checked
at compile time, but any type mismatch will trigger a helpful error
message.

If more advanced decision-making is required, or the method has more
than 8 parameters, the convenience methods won't work. Use the variant
that takes an `IFakeObjectCall` instead:

```charp
A.CallTo(() => fakeShop.SomeCall(…))
                       .ReturnsLazily(objectCall => calculateReturnFrom(objectCall));
```

The `IFakeObjectCall` object provides access to

* information about the `Method` being called, as a `MethodInfo`,
* the `Arguments`, accessed by position or name, and
* the original `FakedObject`


---


## throwing-exceptions.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/throwing-exceptions.md



### Throwing Exceptions

When it's deployed, you may not want code to throw exceptions, but
often it's necessary to test what happens when libraries your code
interacts with throw them. You can configure a Fake to throw an
exception like this:

```csharp
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(DateTime.MaxValue))
 .Throws(new InvalidDateException("the date is in the future"));
```

If the exception type has a parameterless constructor, you can use it
like

```csharp
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(DateTime.MaxValue))
 .Throws<InvalidDateException>();
```

There are also more advanced methods that can throw exceptions based
on values calculated at runtime. These act similarly to how you
[specify return values that are calculated at call time](specifying-return-values.md#return-values-calculated-at-call-time). For
example

```csharp
// Generate the exception at call time.
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(A<DateTime>._))
 .Throws(() => new InvalidDateException(DateTime.UtcNow + " is in the future"));

// Pass up to 8 original call argument values into the method that creates the exception.
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(A<DateTime>._))
 .Throws((DateTime when)=>new InvalidDateException(when + " is in the future"));

// Pass an IFakeObjectCall into the creation method for more advanced scenarios,
// including throwing an exception from a method that has more than 8 parameters.
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(A<DateTime>._))
 .Throws(callObject => new InvalidDateException(callObject.FakedObject +
                                                " is closed on " +
                                                callObject.Arguments[0]));
```

#### Throwing exceptions from an async method

When a method returns a `Task` or `Task<T>`, there are two ways it can indicate
failure via an exception:

- throw the exception synchronously, i.e. not actually return a `Task`
- "throw asynchronously", i.e. return a failed task with the exception.

The former is supported by the `Throws` method described above, in the same way as if the
method was synchronous. The latter can be configured by using the `ThrowsAsync` method:

```csharp
A.CallTo(() => fakeShop.OrderSweetsAsync("cheeseburger"))
 .ThrowsAsync(new ArgumentException("'cheeseburger' isn't a valid sweet category"));
```

This will cause the configured method to return a failed `Task` whose `Exception` property
is set to the exception specified in `ThrowsAsync`.

As with `Throws` above, `ThrowsAsync` has several overloads, including those that take `Func`s of up to
8 parameters, and one that takes a `Func` that operates on an `IFakeObjectCall`. The latter is suitable
for examining, in detail, the call that triggers the exception, or for configuring a method that has
more than 8 parameters.

These overloads of `ThrowsAsync` also exist for `ValueTask` and `ValueTask<T>`. If your test project
targets a framework compatible with .NET Standard 2.1 or higher, they're built into FakeItEasy itself;
otherwise, they're in a separate package:
[`FakeItEasy.Extensions.ValueTask`](https://www.nuget.org/packages/FakeItEasy.Extensions.ValueTask).


---


## doing-nothing.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/doing-nothing.md



### Doing Nothing

Sometimes you want a call to be ignored. That can be configured like so:
```csharp
A.CallTo(() => aFake.SomeVoidMethodThatShouldDoNothing())
 .DoesNothing();
```

This is quite close to what a default Fake's
[unconfigured member will do](default-fake-behavior.md#overrideable-members-are-faked),
but there a few situations where you may need to make the `DoesNothing` call
explicitly. Here are some examples:

* To allow a call to a member on a [strict Fake](strict-fakes.md), where an otherwise
  unconfigured call will throw an exception.
* To change the behavior that an already-configured call is supposed to have.
  For example, if a call is [set to throw an exception](throwing-exceptions.md), that can be
  overridden. For more on this kind of thing, see how to
  [override the behavior for a call](changing-behavior-between-calls.md#overriding-the-behavior-for-a-call).
* To fulfill FakeItEasy's configuration API requirements by providing an action when applying 
  [capturing arguments](capturing-arguments.md) to a Fake's member. Aside from capturing, you may
  not want to change the behavior of the member, but you have to use some action to complete the
  chained calls that configure the method.

???+ note "Only applies to members that return nothing"
    Note that `DoesNothing` is only applicable when configuring members that have a
    `void` return (or `Task`, which is the async equivalent of `void`). To override
    behavior on members that return values, you must
    instead [configure a preferred return value](specifying-return-values.md).


---


## assigning-out-and-ref-parameters.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/assigning-out-and-ref-parameters.md



### Assigning out and ref parameters

Sometimes methods have `out` or `ref` parameters that need to be
filled in when the faked method is called. Use
`AssignsOutAndRefParameters`:

```csharp
A.CallTo(() => aFake.AMethod(anInt, ref aRef, out anOut))
 .Returns(true)
 .AssignsOutAndRefParameters("new aRef value", "new anOut value");
```

`AssignsOutAndRefParameters` takes a `params object[]`, with one
element (in order) for each of the `out` and `ref` parameters in the
call being faked - the other parameters are omitted.

While assigning `out` and `ref` parameters, the `Returns` method (or
[some variant](specifying-return-values.md)) is often used to specify
the return value for a non-void method - `AssignsOutAndRefParameters`
does not do this on its own. If `AssignsOutAndRefParameters` is used
without a `Returns`, the return value will be a [Dummy](dummies.md).
When both `Returns` and `AssignsOutAndRefParameters` are used,
`Returns` must be specified first.

#### Assigning Values Calculated at Call Time

When `out` or `ref` parameter values aren't known until the method is
called, `AssignsOutAndRefParametersLazily` can be used.

```csharp
string theValue;
A.CallTo(() => aFake.AMethod(anInt, ref aRef, out anOut))
 .Returns(true)
 .AssignsOutAndRefParametersLazily((int someInt, string someRef, string someOut) =>
     new[] { "new aRef value: " + someInt, "new anOut value" });
```

As shown above, the inputs to the method may be used to calculate the
values to assign. Convenient overloads exist for methods of up to 8
parameters.

The type of the `Func` sent to `AssignsOutAndRefParametersLazily`
isn't checked at compile time, but any _type_ mismatch will trigger
a helpful error message.

If more advanced decision-making is required, or the method has more
than 8 parameters, the convenience methods won't work. Use the variant
that takes an `IFakeObjectCall` instead:

```csharp
string theValue;
A.CallTo(() => aFake.AMethod(anInt, ref aRef, out anOut))
 .Returns(true)
 .AssignsOutAndRefParametersLazily(call => CalculateValuesFrom(call));
```
The `IFakeObjectCall` object provides access to

* information about the `Method` being called, as a `MethodInfo`,
* the `Arguments`, accessed by position or name, and
* the original `FakedObject`

#### Implicitly Assigning `out` Parameter Values

Any `Expression`-based `A.CallTo` configuration that's made on a
method that has an `out` parameter will cause the value of the variable
used in the `A.CallTo` to be assigned to the `out` parameter when the
method is actually called. For example:

```csharp
string configurationValue = "lollipop";
A.CallTo(() => aFakeDictionary.TryGetValue(theKey, out configurationValue))
 .Returns(true); 

string fetchedValue;
aFakeDictionary.TryGetValue(theKey, out fetchedValue);

// fetchedValue is now "lollipop";
```

If this behavior is not desired, `AssignsOutAndRefParameters` (or `…Lazily`) can be used to provide different behavior.


---


## invoking-custom-code.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/invoking-custom-code.md



### Invoking Custom Code

Sometimes a faked method's desired behavior can't be satisfactorily
defined just by
[specifying return values](specifying-return-values.md),
[throwing exceptions](throwing-exceptions.md),
[assigning out and ref parameters](assigning-out-and-ref-parameters.md)
or even [doing nothing](doing-nothing.md). Maybe you need to simulate
some kind of side effect, either for the benefit of the System Under
Test or to make writing a test easier (or possible). Let's see what
that's like.

```csharp
A.CallTo(() => fakeShop.SellSmarties())
 .Invokes(() => OrderMoreSmarties()) // simulate Smarties stock falling too low
 .Returns(20);
```

Now when the System Under Test calls `SellSmarties`, the Fake will
call `OrderMoreSmarties`.

If the method being configured has a return value, it will continue to return the
[default value for an unconfigured fake](default-fake-behavior.md#overrideable-members-are-faked)
unless you override it with `Returns` or `ReturnsLazily`.

There are also more advanced variants that can invoke actions based on
arguments supplied to the faked method. These act similarly to how you
specify return values that are calculated at call time. For example

```csharp
// Pass up to 8 original call argument values into the callback method.
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(A<DateTime>._))
 .Invokes((DateTime when) => System.Console.Out.WriteLine("showing sweet sales for " + when))
 .Returns(17);

// Pass an IFakeObjectCall into the callback for more advanced scenarios,
// including configuring methods that have more than 8 parameters.
A.CallTo(() => fakeShop.NumberOfSweetsSoldOn(A<DateTime>._))
 .Invokes(callObject => System.Console.Out.WriteLine(callObject.FakedObject +
                                                     " is closed on " +
                                                     callObject.Arguments[0]));
```

#### Case study - customizing a read/write property

Sometimes customizing a Fake's behavior interferes with the default Fake
behavior in undesired ways. For example, changing the setter behavior of a
read/write property (perhaps to [raise an event](raising-events.md)) can break
how the
[`set` and `get` share values](default-fake-behavior.md#readwrite-properties).
If the setter's behavior is changed, it's necessary to explicitly retain the
connection to the getter:

```c#
A.CallToSet(() => fakeShop.OpeningHours).Invokes(TimeRange newTimes) =>
{
    // have the getter return the new times when called
    A.CallTo(() => fakeShop.OpeningHours).Returns(newTimes);

    // custom action - notify listeners of the change
    fakeShop.OpeningHoursChanged += Raise.With(new HoursChangedEvent(newTimes));
}
```


---


## calling-base-methods.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/calling-base-methods.md



### Calling base methods

The `CallsBaseMethod` configuration method can be used to make a method execute the implementation of the faked class:

```csharp
A.CallTo(() => fakeShop.SellSmarties())
 .CallsBaseMethod();
```

Configuring a method to call its base method only makes sense if the method is actually implemented, so this technique cannot be used on an abstract class method or on any method from a faked interface—a faked abstract method told to call its base method will throw a `NotImplementedException`.

#### Configuring all methods at once

Perhaps you want to have all or nearly all of a fake's (fakeable) methods defer to the original implementation. Rather than using `CallsBaseMethod` a dozen times, the [fake creation option](creating-fakes.md#explicit-creation-options) `CallsBaseMethods` can do all the work at once:

```csharp
var fakeShop = A.Fake<CandyShop>(options => options.CallsBaseMethods());
```

And then [selectively override some of them](changing-behavior-between-calls.md#overriding-the-behavior-for-a-call)

```csharp
A.CallTo(() => fakeShop.SellRockets()).Throws<Exception>();
```

#### Default interface members

A fake may also be instructed to execute the
[default implementation for an interface member](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface#default-interface-members)
using `CallsBaseMethod` or `CallsBaseMethods` as above.

???+ warning "This will not work for explicitly-implemented interfaces that have default member implementations"
    [Explicit interface implementation](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/interfaces/explicit-interface-implementation)
    redeclares the implemented interface's member(s), making them effectively new methods on the type, so there is no
    "base member" implementation to call.


---


## calling-wrapped-methods.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/calling-wrapped-methods.md



### Calling wrapped methods

By default, calls to a wrapping fake that have not been explicitly configured will be forwarded to the wrapped object.
However, if this behavior has been overridden by another configuration, or if you need to invoke a callback before
calling the wrapped method, you can explicitly configure the call to be forwarded to the wrapped object with the
`CallsWrappedMethod` configuration method.

```csharp
var realShop = new CandyShop();
var fakeShop = A.Fake<ICandyShop>(o => o.Wrapping(realShop));
A.CallTo(() => fakeShop.SellSmarties())
    .Invokes(() => Console.WriteLine("Selling smarties!"))
    .CallsWrappedMethod();
```


---


## changing-behavior-between-calls.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/changing-behavior-between-calls.md



### Changing behavior between calls

#### Limited call specifications

When [specifying return values](specifying-return-values.md) or
configuring [exceptions to be thrown](throwing-exceptions.md) and so
on, it's possible to define the number of times the action can
occur. By default, omitting the number of repetitions is the same as
saying "forever", so after specifying
`A.CallTo(() =>fakeShop.Address).Returns("123 Fake Street")`,
`fakeShop.Address` will return the same value _every time it's
called. Forever._

This can be changed, though:
```csharp
A.CallTo(() => fakeShop.Address).Returns("123 Fake Street").Once();
A.CallTo(() => fakeShop.Address).Returns("123 Fake Street").Twice();
A.CallTo(() => fakeShop.Address).Returns("123 Fake Street").NumberOfTimes(17);
```

This could be useful if you want to allow a limited number of calls on
a [strict fake](strict-fakes.md), but there's a more useful application.

#### Specifying different behaviors for successive calls

In some cases, you might want to specify different behaviors for successive
calls to the same method. For instance, in order to test the System Under
Test's retry logic, a Fake service could be configured to fail once and then
function properly thereafter. This can be done by chaining behaviors like this:

```csharp
// Configure the method to throw an exception once, then succeed forever
A.CallTo(() => fakeService.DoSomething())
    .Throws<Exception>().Once()
    .Then
    .Returns("SUCCESS");
```

Note that you can only use `Then` after specifying that some behavior should
only occur a limited number of times.


#### Overriding the behavior for a call

Call specifications act kind of like a stack - they're pushed on the
Fake and then popped off once the number of repetitions defined for a
call have been exhausted.

Thus, it's possible to have a call to a Fake act one way, and then another. For
instance, the same effect as the previous sample can be achieved by overriding
the behavior of the fake:

```csharp
// set up an action that can run forever, unless superseded
A.CallTo(() => fakeService.DoSomething()).Returns("SUCCESS");

// set up a one-time exception which will be used for the first call
A.CallTo(() => fakeService.DoSomething()).Throws<Exception>().Once();
```

This can be useful when you are unable to use `Then` to specify a different
behavior for successive calls. For example, when you have a fake with a default
behavior (configured in a test `Setup` method or a
[`FakeOptionsBuilder`](implicit-creation-options.md)), and you need to override
this behavior for a specific test.


---


## argument-constraints.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/argument-constraints.md



### Argument constraints

When configuring and [asserting](assertion.md) calls in FakeItEasy,
the arguments of the call can be constrained so that only calls to the
configured method where the arguments matches the constraint are
selected.

#### Matching values exactly

Assume the following interface exists:
```csharp
public interface IFoo
{
    void Bar(string s, int i);
}
```

Then the arguments to Bar can be constrained to limit call matching:
```csharp
var foo = A.Fake<IFoo>();

A.CallTo(() => foo.Bar("hello", 17)).MustHaveHappened();
```

Then FakeItEasy will look _only_ for calls made with the arguments
`"hello"` and `17` - no other calls will match the rule.

When checking for argument equality, FakeItEasy uses the first applicable equality
test from this list:

* if both values are `null`, they are considered equal,
* if one value is `null` and the other isn't, they are not considered equal,
* the highest-priority [custom argument equality comparer](custom-argument-equality.md)
  that can compare the example object's type, or
* if both values are `string`, `string.Equals` (as an optimization),
* if the example object's type implements `System.Collections.IEnumerable`, the values
  are considered equal if and only if the actual object's type also implements
  `System.Collections.IEnumerable` and `System.Linq.Enumerable.SequenceEqual` evaluates
  to true, or
* `Object.Equals`

If this list is not satisfactory, you may have to use the `That.Matches`
method described in [Custom matching](#custom-matching). Be
particularly careful of types whose `Equals` methods perform reference
equality rather than value equality. In that case, the objects have to
be _the same object_ in order to match, and this sometimes produces
unexpected results. When in doubt, verify the type's `Equals`
behavior manually.

#### Other matchers
##### Ignoring arguments

Suppose the value of the integer in the `Bar` call wasn't important,
but the string was. Then the following constraint could be used:

```csharp
A.CallTo(() => foo.Bar("hello", A<int>.Ignored)).MustHaveHappened();
```

Then any call will match, so long as the string value was
`"hello"`. The `Ignored` property can be used on any type.

An underscore (`_`) can be used as a shorthand for `Ignored` as well:
```csharp
A.CallTo(() => foo.Bar("hello", A<int>._)).MustHaveHappened();
```

##### More convenience matchers

If more complicated constraints are needed, the `That` method can be
used. There are a few built-in matchers:

| Matcher                                | Tests for                                                                              |
|:---------------------------------------|:---------------------------------------------------------------------------------------|
| IsNull()                               | `null`                                                                                 |
| IsNotNull()                            | not `null`                                                                             |
| IsEqualTo(other)                       | object equality using `object.Equals`                                                  |
| IsEqualTo(other, equalityComparer)     | object equality using `equalityComparer.Equals`                                        |
| IsSameAs(other)                        | object identity - like `object.ReferenceEquals`                                        |
| IsInstanceOf(type)                     | an argument that can be assigned to a variable of type `type`                          |
| Contains(string)                       | substring match with ordinal string comparison                                         |
| Contains(string, comparisonType)       | substring match with the specified comparison type                                     |
| StartsWith(string)                     | substring at beginning of string with ordinal string comparison                        |
| StartsWith(string, comparisonType)     | substring at beginning of string with the specified comparison type                    |
| EndsWith(string)                       | substring at end of string with ordinal string comparison                              |
| EndsWith(string, comparisonType)       | substring at end of string with the specified comparison type                          |
| IsNullOrEmpty()                        | `null` or `""`                                                                         |
| IsEmpty()                              | empty enumerable                                                                       |
| Contains(element)                      | element's presence in an enumerable                                                    |
| IsSameSequenceAs(enumerable)           | sequence equality, like `System.Linq.Enumerable.SequenceEqual`                         |
| IsSameSequenceAs(value1, value2, ...)  | sequence equality, like `System.Linq.Enumerable.SequenceEqual`                         |
| HasSameElementsAs(enumerable)          | [multiset](https://en.wikipedia.org/wiki/Multiset) equality - same elements, any order |
| HasSameElementsAs(value1, value2, ...) | [multiset](https://en.wikipedia.org/wiki/Multiset) equality - same elements, any order |
| Not                                    | inverts the sense of the matcher                                                       |

##### Custom matching

If none of the canned matchers are sufficient, you can provide a
predicate to perform custom matching using `That.Matches`. Like in
this rather contrived example:

```csharp
A<string>.That.Matches(s => s.Length == 3 && s[1] == 'X');
```

FakeItEasy will evaluate the predicate against any supplied
argument. The predicate can be supplied as an `Expression<Func<T, bool>>`
or as a `Func<T, bool>`. FakeItEasy can generate a description
of the matcher when an `Expression` is supplied (although you may
supply your own as well), but you must supply a description when using
a `Func`.

##### Always place `Ignored` and `That` inside `A.CallTo`

The `Ignored` (and `_`) and `That` matchers must be placed within the
expression inside the `A.CallTo` call. This is because these special
constraint methods do not return an actual matcher object. They tell
FakeItEasy how to match the parameter via a special event that's fired
when the constraint method is invoked. FakeItEasy only listens to the
events in the context of an `A.CallTo`.

So, tempting as it might be to save one of the constraints away in a
handy variable, don't do it.

#### Using correct grammar

FakeItEasy's API attempts to imitate the English language, so that call
configurations and assertions read naturally. In that spirit, it's
possible to use `An<T>` instead of `A<T>` for types whose name starts
with a vowel sound. For instance:

```csharp
An<Apple>.That.Matches(a => a.Color == "Red")
```

`A<T>` and `An<T>` are exact synonyms and can be used exactly the same
way.

#### `out` parameters

The incoming argument value of `out` parameters is ignored when matching
calls. The incoming value of an `out` parameter can't be seen by the
method body anyhow, so there's no advantage to constraining by it.

For example, this test passes:

```csharp
string configurationValue = "lollipop";
A.CallTo(() => aFakeDictionary.TryGetValue(theKey, out configurationValue))
 .Returns(true);

string fetchedValue = "licorice";
var success = aFakeDictionary.TryGetValue(theKey, out fetchedValue);

Assert.That(success, Is.True);
```

See
[Implicitly Assigning `out` Parameter Values](assigning-out-and-ref-parameters.md#implicitly-assigning-out-parameter-values)
to learn how the initial `configurationValue` is used in this case.

#### `ref` parameters

Due to the limitations of working with `ref` parameters in C#, only exact-value matching is possible using argument constraints,
and the argument value must be compared against a local variable or a field:

```csharp
int someValue = 3;
A.CallTo(() => aFake.aMethod(ref someValue)).Returns(true);
```

To perform more sophisticated matching of `ref` parameter values in C#, use constraints that work on the entire call, such as `WithAnyArguments`
or `WhenArgumentsMatch`, described below.

In addition to constraining by `ref` argument values, calls can be explicitly configured to
[assign outgoing `ref` argument values](assigning-out-and-ref-parameters.md).


#### Arguments of anonymous types

Anonymous types are not shared between assemblies, so it's not possible to make an argument
constraint that matches an anonymous type from another assembly. Instead, you may use
`A.CallTo(fake).Where(...)` as described in
[Specifying a call to any method or property](specifying-a-call-to-configure.md#specifying-a-call-to-any-method-or-property).


#### Overriding argument matchers

Sometimes individually constraining arguments isn't sufficient. In
such a case, other methods may be used to determine which calls match
the fake's configuration.

_When using the following methods, any inline argument constraints are ignored, and only the
special method is used to match the call._ Some arguments have to be supplied in order to satisfy the compiler,
but the values will not be used, so you can supply whatever values make the code easiest for you to read.

`WithAnyArguments` ensures that no argument constraints will be applied when matching calls:

```csharp
A.CallTo(() => foo.Bar(null, 7)).WithAnyArguments().MustHaveHappened();
```

The example above will match any call to `foo.Bar`, regardless of the arguments. The
`Ignored` property performs the same task, and is more flexible, but
some people prefer the look of `WithAnyArguments`.


`WhenArgumentsMatch` accepts a predicate that operates on the entire
collection of call arguments.  For example, to have a Fake throw an
exception when a call is made to `Bar` where the first arguments is a
string representation of the second, use

```csharp
A.CallTo(() => fake.Bar(null, 0))
    .WhenArgumentsMatch(args =>
        args.Get<string>("theString")
            .Equals(args.Get<int>("theInt").ToString()))
    .Throws<Exception>();
```

Strongly typed overloads of `WhenArgumentsMatch` are also available for methods of up to 8
parameters (if a method has more parameters, use the variant described above):

```csharp
A.CallTo(() => fake.Bar(null, 0))
    .WhenArgumentsMatch((string theString, int theInt) =>
        theString.Equals(theInt.ToString()))
    .Throws<Exception>();
```

#### Capturing arguments

It is also possible to [capture arguments passed to a call](capturing-arguments.md), whether you
also want to constrain those arguments or not, and verify the passed values after the fact.

#### Nested argument constraints

Note that an argument constraint cannot be "nested" in an argument; the
constraint has to be the whole argument. For instance, the following call
configurations are invalid and will throw an exception:

```
A.CallTo(() => fake.Foo(new Bar(A<int>._))).Returns(42);
A.CallTo(() => fake.Foo(new Bar { X = A<string>.That.Contains("hello") })).Returns(42);
```

To achieve the desired effect, you can do this instead:

```
A.CallTo(() => fake.Foo(A<Bar>._)).Returns(42);
A.CallTo(() => fake.Foo(A<Bar>.That.Matches(bar => bar.X.Contains("hello")))).Returns(42);
```


---


## assertion.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/assertion.md



### Assertion

Assertion uses exactly the same syntax as configuration to specify the
call to be asserted, followed by a method call beginning with `.MustHaveHappened`.

The two most common forms of assertion are :

* `MustHaveHappened()` (no arguments) asserts that the call was made 1 or more times, and 
* `MustNotHaveHappened()` asserts that the specified call did not happen at all.

Arguments are constrained using
[Argument Constraints](argument-constraints.md) just like when
configuring calls.

#### Syntax

```csharp
A.CallTo(() => foo.Bar()).MustHaveHappened();
A.CallTo(() => foo.Bar()).MustNotHaveHappened();

A.CallTo(() => foo.Bar()).MustHaveHappenedOnceExactly();
A.CallTo(() => foo.Bar()).MustHaveHappenedOnceOrMore();
A.CallTo(() => foo.Bar()).MustHaveHappenedOnceOrLess();

A.CallTo(() => foo.Bar()).MustHaveHappenedTwiceExactly();
A.CallTo(() => foo.Bar()).MustHaveHappenedTwiceOrMore();
A.CallTo(() => foo.Bar()).MustHaveHappenedTwiceOrLess();

A.CallTo(() => foo.Bar()).MustHaveHappened(4, Times.Exactly);
A.CallTo(() => foo.Bar()).MustHaveHappened(6, Times.OrMore);
A.CallTo(() => foo.Bar()).MustHaveHappened(7, Times.OrLess);

A.CallTo(() => foo.Bar()).MustHaveHappenedANumberOfTimesMatching(n => n % 2 == 0);
```

#### Asserting Calls Made with Mutable Arguments

When FakeItEasy records a method (or property) call, it remembers
which objects were used as argument, but does not take a snapshot of
the objects' state. This means that if an object is changed after
being used as an argument, but before argument constraints are
checked, expected matches may not happen. For example,

```csharp
var aList = new List<int> {1, 2, 3};

A.CallTo(() => myFake.SaveList(A<List<int>>._))
    .Returns(true);

myFake.SaveList(aList);
aList.Add(4);

A.CallTo(() => myFake.SaveList(A<List<int>>.That.IsThisSequence(1, 2, 3)))
    .MustHaveHappened();
```

The `MustHaveHappened` will fail, because at the time the
`IsThisSequence` check is made, `aList` has 4 elements, not 3, and
`IsThisSequence` only has the reference to `aList` to use in its
check, not a deep copy or some other form of snapshot—it has to work
with the _current_ state.

If your test or production code must mutate call arguments between the
time of the call and the assertion time, you must look for some other
way to verify the call. Perhaps using `IsSameAs` will suffice, if the
correct behavior of the System Under Test can otherwise be
inferred. Or consider using [Invokes](invoking-custom-code.md) to
create a snapshot of the object and interrogate it later:

```csharp
var aList = new List<int> {1, 2, 3};

List<int> capturedList;
A.CallTo(() => myFake.SaveList(A<List<int>>._))
    .Invokes((List<int> list) => capturedList = new List<int>(list))
    .Returns(true);

myFake.SaveList(aList);
aList.Add(4);

Assert.That(capturedList, Is.EqualTo(new List<int> {1, 2, 3}));
```

#### More advanced assertions

If the built-in assertion API isn't sufficient, you can also examine the list of recorded calls directly, as described in [Getting the list of calls made on a fake](advanced-usage.md#getting-the-list-of-calls-made-on-a-fake).

#### VB.NET

```
' Functions and Subs can be asserted using their respective keywords
A.CallTo(Function() foo.Bar()).MustHaveHappened()
A.CallTo(Sub() foo.Baz(A(Of String).Ignored)).MustHaveHappened()
```


---


## ordered-assertions.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/ordered-assertions.md



### Ordered assertions

The concept of ordered assertions is somewhat complex and nothing that
should be used frequently but there are times when it's really needed.

Using FakeItEasy you can assert that calls happened in a specific order
on _one or more_ fake objects.

#### Details

One area where ordered asserts are useful is when you need to test
that a call to a fake has happened between two other calls, such as
when dealing with transactions or units of work.

```csharp
public interface IUnitOfWorkFactory
{
    IDisposable BeginWork();
}

public interface IDoSomethingPrettyUseful
{
    void JustDoIt();
}

public class Worker
{
    private IUnitOfWorkFactory unitOfWorkFactory;
    private IDoSomethingPrettyUseful usefulCollaborator;
        
    public Worker(IUnitOfWorkFactory unitOfWorkFactory, IDoSomethingPrettyUseful usefulCollaborator)
    {
        this.unitOfWorkFactory = unitOfWorkFactory;
        this.usefulCollaborator = usefulCollaborator;
    }

    public void JustDoIt()
    {
        using (this.unitOfWorkFactory.BeginWork())
        {
            this.usefulCollaborator.JustDoIt();
        }
    }
}
```

In the following example we'll assert that the call to `usefulCollaborator.JustDoIt()` happened between the calls to `BeginWork` and the `Dispose` method of the returned unit of work.

```csharp
[Test]
public void Should_start_work_within_unit_of_work()
{
    // Arrange
    var unitOfWork = A.Fake<IDisposable>();
            
    var unitOfWorkFactory = A.Fake<IUnitOfWorkFactory>();
    A.CallTo(() => unitOfWorkFactory.BeginWork()).Returns(unitOfWork);

    var usefulCollaborator = A.Fake<IDoSomethingPrettyUseful>();

    var worker = new Worker(unitOfWorkFactory, usefulCollaborator);

    // Act
    worker.JustDoIt();

    // Assert
    A.CallTo(() => unitOfWorkFactory.BeginWork()).MustHaveHappened()
        .Then(A.CallTo(() => usefulCollaborator.JustDoIt()).MustHaveHappened())
        .Then(A.CallTo(() => unitOfWork.Dispose()).MustHaveHappened());
}
```

The regular `MustHaveHappened` calls are made, but the results are chained together using `Then`, which verifies not only that that call happened, but that it occurred in the right order relative to other calls that have been asserted in the same chain.

With the current implementation of the `Worker` the test will pass. But let's change the order of the calls in `JustDoIt`:

```csharp
public void JustDoIt()
{ 
    using (this.unitOfWorkFactory.BeginWork())
    { 
        
    }
    this.usefulCollaborator.JustDoIt();
}
```

The test will now fail with the following exception message:

<pre>
 Assertion failed for the following calls:
    'OrderedAssertsDemo.IUnitOfWorkFactory.BeginWork()' once or more
    'OrderedAssertsDemo.IDoSomethingPrettyUseful.JustDoIt()' once or more
    'System.IDisposable.Dispose()' once or more
  The calls where found but not in the correct order among the calls:
    1.  'OrderedAssertsDemo.IUnitOfWorkFactory.BeginWork()'
    2.  'System.IDisposable.Dispose()'
    3.  'OrderedAssertsDemo.IDoSomethingPrettyUseful.JustDoIt()'
</pre>


---


## capturing-arguments.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/capturing-arguments.md



### Capturing arguments

Often tests will make [assertions against the calls made to a fake object](assertion.md)
to verify the behavior of the system under test, but you can also capture arguments
supplied in a call to a fake object and examine the values later.

#### Simple capture

Take this contrived example:

```csharp title='"production code"'
--8<--
recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:IListLogger

recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:Calculator
--8<--
```

Suppose we want to make sure that we send correct messages to the logger.
We can record the first argument to each `Log` call in a variable of type
`Captured` and verify the values in the "assert" phase of the test.

```csharp title="simple capture" linenums="1" hl_lines="2 6 8 17 22"
--8<--
recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:SimpleCapture
--8<--
```

This is a fairly standard test with fakes, except we:

* create a `Captured` object to store received argument values
* use the `Captured` object's `_` member to configure the call to capture any values for that argument
  (this is analogous to the `A<T>._` member, and just like it, there is a matching `Ignored` member in
  case you prefer that name)
* specify that the method will [do nothing](doing-nothing.md).
  As with any attempt to [configure a fake object's behavior](specifying-a-call-to-configure.md), it's the final
  action that completes the `A.CallTo` sequence. Without it, there's no change to the fake object's
  behavior, and no values will be captured on calls to the method. You don't need to pair `DoesNothing` with
  configuration that uses a capturing argument: you could return values, throw exceptions, or whatever;
  it was just appropriate here for a method that returns nothing.
* use `Values` to access all the captured values so they can be asserted
* (alternative flow) use `GetLastValue` to access the most recent captured value so it can be asserted.
  This will throw a `FakeItEasy.ExpectationException` if no values have been captured.

???+ note "Values are only captured if the call matches the configuration"
    When a call configuration intends to capture one or more arguments, the argument
    values are only captured if this specific call configuration is triggered. If an incoming call
    does not match that configured for the method or property, no arguments are captured.

#### Capturing and constraining at the same time

Even though you can interrogate captured values after the fact, you may want to configure
fake behavior to take effect only when incoming arguments meet a constraint. As you might have guessed
based on the references to `_` and `Ignored` above, you can do this using a `That` method just like the
one supplied by the [`A<T>` argument constraints](argument-constraints.md):

```csharp title="constrained capture" linenums="1" hl_lines="6"
--8<--
recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:ConstrainedCapture
--8<--
```

#### Capturing mutable arguments

##### The challenge

Argument capture is shallow; there's no copying of object state.
If a reference-based argument (e.g. a class instance, not a struct) is captured and
subsequently modified by the test or production code, the "assert" phase of the test
will see the updated state.

```csharp title="capturing mutating values" linenums="1" hl_lines="2 7 13 14 15 16 22 23"
--8<--
recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:NaivelyCaptureMutatedList
--8<--
```

Here a single list instance is passed to the production code twice, but an element is removed
between the calls. The `Captured` object captures a reference to the list each
time, but does not preserve the internals of the list. So by the time the `Values` are examined,
the list has had its first element removed, and this is reflected in the failing assertion.

It's the same effect as running

```c#
List<int> list = [1, 2, 3, 4];
var a = list;
list.RemoveAt(0);
var b = list;

// both a and b point to a list with elements { 2, 3, 4 }
```

##### Capturing frozen state

`Captured` objects can be created with a transforming function (or "freezer") that runs on the
argument value before saving it away, thus insulating the captured values
from subsequent mutations.

```csharp title="freezing state of captured values" linenums="1" hl_lines="2 3 20"
--8<--
recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:CaptureCopiedMutatedList
--8<--
```

You can even transform values into a different type:

```csharp title="freezing state of captured values as new type" linenums="1" hl_lines="2 3 20 21"
--8<--
recipes/FakeItEasy.Recipes.CSharp/CapturingArguments.cs:CaptureCopiedMutatedListToNewType
--8<--
```

In this case we supply an updated freezer function as well as the `Captured` typeparams
for both the argument value and the captured value types.


---


## faking-async-methods.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/faking-async-methods.md



### Faking async methods

The faking of `async` methods is fully supported in FakeItEasy.

```csharp
public class Foo
{
    public virtual async Task<string> Bar()
    {
        // await something...
    }
}
```

A call to a non-configured async method on a fake will return a
[Dummy](dummies.md#how-are-the-dummies-made) `Task` or `Task<T>`, just
as if it were any other method that returns a `Task` or `Task<T>`. For
example:

```csharp
var foo = A.Fake<Foo>();
var bar = await foo.Bar(); // will return immediately and return string.Empty
```

Of course, you can still configure calls to `async` methods as you would normally:

```csharp
A.CallTo(() => foo.Bar()).Returns(Task.FromResult("bar"));
```

There are also convenience overloads of `Returns` and `ReturnsLazily` that let you specify a value rather
than a task, and configure the method to return a completed task whose result is the specified value:

```csharp
A.CallTo(() => foo.Bar()).Returns("bar");
```

These overloads of `Returns` and `ReturnsLazily` also exist for `ValueTask<T>`. If your test project
targets a framework compatible with .NET Standard 2.1 or higher, they're built into FakeItEasy itself;
otherwise, they're in a separate package:
[`FakeItEasy.Extensions.ValueTask`](https://www.nuget.org/packages/FakeItEasy.Extensions.ValueTask).

#### Throwing exceptions

To configure an async method to throw an exception, see
[Throwing exceptions from an async method](throwing-exceptions.md#throwing-exceptions-from-an-async-method).


---


## raising-events.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/raising-events.md



### Raising events

FakeItEasy can be used to simulate the raising of an event from a Fake object, assuming the event is virtual or abstract, or defined on an interface.

#### `EventHandler`-based events

Suppose a standard `EventHandler`-based event such as this one:

```csharp
public interface IRobot
{
    event EventHandler FellInLove;
}
```

You can raise that event, specifying sender and event
arguments. You could also omit the sender and the Fake will be passed as
sender to the event handler, and there's also a convenience method for
raising with empty event arguments:

```csharp
var robot = A.Fake<IRobot>();

// Somehow use the fake from the code being tested

// Raise the event!
robot.FellInLove += Raise.With(someEventArgs); // the "sender" will be robot

// Use the overload for empty event args. Sender will be robot, args will be EventArgs.Empty
robot.FellInLove += Raise.WithEmpty();

// Specify sender and event args explicitly:
robot.FellInLove += Raise.With(sender: robot, e: someEventArgs);
```

##### VB.NET syntax

```vbnet
' Raise the event!
AddHandler robot.FellInLove, Raise.With(someEventArgs) ' the "sender" will be robot

' Use the overload for empty event args. Sender will be robot, args will be EventArgs.Empty
AddHandler robot.FellInLove, Raise.WithEmpty()

' Specify sender and event args explicitly:
AddHandler robot.FellInLove, Raise.With(sender, someEventArgs)
```

#### Raising `EventHandler<TEventArgs>`

Events of type `EventHandler<TEventArgs>` can be raised in exactly the same way.

#### "Free-form" events using arbitrary delegates

It is also possible to raise events defined using a **custom delegate** (a.k.a
"free-form delegate"), like these:

```csharp
public delegate void FreeformEventHandler(int count);
public delegate void CustomEventHandler(object sender, CustomEventArgs e);
…
event FreeformEventHandler FreeformEvent;
event CustomEventHandler CustomEvent;
```

##### From C&#x23;
To raise a free-form event from C#, use `Raise.FreeForm.With`, which automatically infers the correct delegate type:

```csharp
fake.FreeformEvent += Raise.FreeForm.With(7);
fake.CustomEvent += Raise.FreeForm.With(fake, sampleCustomEventArgs);
```

Due to language limitations, `Raise.Freeform.With` does not work in VB.NET, and it uses late binding, so you need a reference to the `Microsoft.CSharp` assembly in order to use it.

##### From VB.NET
To raise a free-form event from VB.NET, you must use `Raise.FreeForm(Of TEventHandler).With`:

```vbnet
AddHandler fake.FreeformEvent, Raise.FreeForm(Of FreeformEventHandler).With(7)
AddHandler fake.CustomEvent, Raise.FreeForm(Of CustomEventHandler).With(fake, sampleCustomEventArgs)
```

Specifying the type of the event handler gets around the language restrictions in VB.NET.
This method may also be used from C# if you don't want to rely on late binding.

#### Limitations

The approach described above for raising events doesn't work in some situations:

- Wrapping fakes
- Fakes configured to call base methods

This is because the calls (including event subscription and unsubscription) are
forwarded to another implementation (wrapped object or base class) that
FakeItEasy has no control over, so the fake doesn't know about the handlers and
cannot call them.

Similarly, strict fakes don't handle any call unless explicitly configured,
including event subscription or unsubscription, so FakeItEasy also can't raise
events on strict fakes.

To work around this limitation, you have three options:

- For a strict fake, you can [enable the default event behavior on the fake at
creation time](strict-fakes.md#events).

- You can explicitly enable the default event behavior on the fake, for a
specific event or for all events of the fake:

```csharp
Manage.Event("FellInLove").Of(robot);
Manage.AllEvents.Of(robot);
```

- If you need more control, you can explicitly configure the calls to the event
accessors to handle event subscription yourself, as in the example below:

```csharp
// Declare a delegate to store the event handlers
EventHandler handlers = null;

// Configure event subscription on the fake
A.CallTo(robot, EventAction.Add("FellInLove")).Invokes((EventHandler h) => handlers += h);
A.CallTo(robot, EventAction.Remove("FellInLove")).Invokes((EventHandler h) => handlers -= h);

// Raise the event
handlers?.Invoke(robot, EventArgs.Empty);
```


---


## strict-fakes.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/strict-fakes.md



### Strict fakes

[By default](default-fake-behavior.md), FakeItEasy's fakes support
what is sometimes called "loose mocking". This means that calls to any
of the fake's members are allowed, even if they haven't been
configured.

However, FakeItEasy also supports strict fakes, in which all calls to
unconfigured members are rejected, throwing an
`ExpectationException`. Strict fakes are created by supplying a
[creation option](creating-fakes.md#explicit-creation-options):

```csharp
var foo = A.Fake<IFoo>(x => x.Strict());
```

After you have configured your fake in this fashion you can configure
any "allowed" calls as usual, for example:

```csharp
A.CallTo(() => foo.Bar()).Returns("bar");
```

Strict fakes are useful when it is important to ensure that no calls
are made to your fake other than the ones you are expecting.

#### Object members

It can sometimes be inconvenient that *all* methods throw an exception
if not configured. You might want to allow calls to methods inherited
from `System.Object` (`Equals`, `GetHashCode` and `ToString`), because
they're used all the time, often implicitly, and in most cases there's
no real value in configuring them manually.

To achieve this, pass a `StrictFakeOptions` value to the `Strict`
method when you create the fake:

```csharp
// Allow calls to all object methods
var foo = A.Fake<IFoo>(x => x.Strict(StrictFakeOptions.AllowObjectMethods));

// Allow calls to ToString
var foo = A.Fake<IFoo>(x => x.Strict(StrictFakeOptions.AllowToString));

// Allow calls to Equals and GetHashCode
var foo = A.Fake<IFoo>(x => x.Strict(StrictFakeOptions.AllowEquals | StrictFakeOptions.AllowGetHashCode));
```

#### Events

By default, calls to event accessors of a strict fake will fail if the
calls are not configured. Although you can [manually handle event subscription
or unsubscription](raising-events.md#limitations), there's often not much value
in doing this manually. You can allow a strict fake to manage events
automatically by passing the `AllowEvents` flag to the `Strict` method:

```csharp
var foo = A.Fake<IFoo>(x => x.Strict(StrictFakeOptions.AllowEvents));
```


---


## dummies.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/dummies.md



### Dummies

A Dummy is an object that FakeItEasy can provide when an object of a
certain type is required, but the actual behavior of the object is not
important.

#### How to use them in your tests

Consider this example. Say that you want to test the following class:

```csharp
public class Library
{
    public bool Checkout(Card patronCard, Book someBook);
}
```

Maybe in one of your tests you want to invoke `Checkout` with an
expired library card. The checkout should fail, regardless of the book
being checked out&mdash;only the status of the card matters. Instead
of writing

```csharp
library.Checkout(MakeExpiredCard(),
                 new Book { Title = "The Ocean at the End of the Lane" } );
```

You can write:

```csharp
library.Checkout(MakeExpiredCard(), A.Dummy<Book>());
```

This signals that the actual value of the `Book` is really not
important. The code is intention-revealing.

You can also create a collection of dummies by writing:

```csharp
A.CollectionOfDummy<Book>(10);
```

This will return an `IList` containing 10 dummy `Book` instances.

For cases where the type of dummy isn't statically known, non-generic methods are also available. These are usually only required when writing extensions for FakeItEasy, so they live in the `FakeItEasy.Sdk` namespace:
```csharp
using FakeItEasy.Sdk;
...
var type = GetTypeOfDummy();
object dummy = Create.Dummy(type);
IList<object> dummies = Create.CollectionOfDummy(type, 10);
```

#### How FakeItEasy uses them

When [creating Fakes](creating-fakes.md) or Dummies of class types,
FakeItEasy needs to invoke the classes' constructors. If the
constructors take arguments, FakeItEasy needs to generate appropriate
argument values. It uses Dummies.

#### How are the Dummies made?

When FakeItEasy needs to access a Dummy of type `T`, it tries a number
of approaches in turn, until one succeeds:

1. If `T` is `void`, return `null`.
1. If there's a user-supplied
  [dummy factory](custom-dummy-creation.md) for `T`,
  return whatever it makes.
1. If `T` is `String`, return an empty string.
1. If `T` is `Task` or `ValueTask`, the returned Dummy will be an actual `Task` or `ValueTask`
   that is already completed.
1. If `T` is `Task<TResult>` or `ValueTask<TResult>`, the returned Dummy will be an actual
  `Task<TResult>` or `ValueTask<TResult>` that is already completed and whose `Result` is a
  Dummy of type `TResult`, or a default `TResult` if no  Dummy can be made for `TResult`.
1. If `T` is a `Lazy<TValue>` the returned Dummy will be an actual
  `Lazy<TValue>` whose `Value` is a Dummy of type
  `TValue`, or a default `TValue` if no Dummy can be made
  for `TValue`.
1. If `T` is a tuple type (`Tuple<>` or `ValueTuple<>`), the Dummy will
be a tuple whose elements are dummies, or default values when dummies
can't be made.
1. If `T` is a value type, the Dummy will be the default value for that type (i.e. `new T()`).
1. If `T` is [fakeable](what-can-be-faked.md), the Dummy will be a
  Fake `T`.
1. If nothing above matched, then `T` is a class. Loop over all its
  public constructors in _descending order of argument list length_.
  For each constructor, attempt to get Dummies to satisfy the argument
  list. If the Dummies can be found, create an instance using that constructor,
  supplying the Dummies as the argument list. If the argument list can't be satisfied,
  then try the next constructor.

If none of these strategies yield a viable Dummy, then FakeItEasy
can't make a Dummy of type `T`.


---


## custom-dummy-creation.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/custom-dummy-creation.md



### Custom Dummy Creation

FakeItEasy has built-in [Dummy](dummies.md) creation rules that
provide usable non-null values to be used in tests. However, if the
default dummy creation behavior isn't adequate, you can provide your
own. Here's an example:

```csharp
class DummyBookFactory : DummyFactory<Book>
{
    protected override Book Create()
    {
        return new Book { Title = "Some Book", PublishedOn = new DateTime(2000, 1, 1) };
    }
}
```

##### How it works

FakeItEasy uses classes that implement the following interface to create Dummies:

```csharp
public interface IDummyFactory
{
    bool CanCreate(Type type);
    object Create(Type type);
    Priority Priority { get; }
}
```

When FakeItEasy tries to create a Dummy, it looks at all known
`IDummyFactory` implementations for which `CanCreate` returns
`true`. If multiple implementations match, the one with the highest
`Priority`is used.

If all that's needed is a Dummy Factory that creates a single,
explicit type, extending `abstract class DummyFactory<T>:
IDummyFactory` is preferred. It provides default implementations of
`Priority` and `CanCreate` (although they can be overridden if
needed).

However, if you want to provide Dummies for a variety of types, you
may prefer to extend `IDummyFactory` directly. For example, if you
wanted all Dummy `IEnumerable<T>`s to be `SortedSet<T>`s, you might
write something like this:

```csharp
class DummyEnumerableFactory: IDummyFactory
{
    public bool CanCreate(Type type)
    {
        if (type.IsGenericType)
        {
            var enumerableContentType = type.GetGenericArguments()[0];
            var enumerableTypeDefinition = typeof (IEnumerable<>).MakeGenericType(enumerableContentType);
            return enumerableTypeDefinition.IsAssignableFrom(type);
        }
        return false;
    }

    public object Create(Type type)
    {
        var enumerableContentType = type.GetGenericArguments()[0];
        var enumerableType = typeof (SortedSet<>).MakeGenericType(enumerableContentType);
        return enumerableType.GetConstructor(new Type[0]).Invoke(null);
    }

    public Priority Priority
    {
        get { return Priority.Default; }
    }
}
```

##### How does FakeItEasy find the Dummy Factories?

On initialization, FakeItEasy
[looks for Discoverable Extension Points](scanning-for-extension-points.md),
including Dummy Factories.



---


## implicit-creation-options.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/implicit-creation-options.md



### Implicit Creation Options

While it's possible to provide
[explicit creation options](creating-fakes.md#explicit-creation-options),
this can sometimes be tedious. Sometimes you want to have every Fake
of a particular type start with some basic configuration, using a
`FakeOptionsBuilder`. Here's an example:

```csharp
public class RobotRunsAmokEventFakeOptionsBuilder : FakeOptionsBuilder<RobotRunsAmokEvent>
{
    protected override void BuildOptions(IFakeOptions<RobotRunsAmokEvent> options)
    {
        options.ConfigureFake(fake =>
        {
            A.CallTo(() => fake.CalculateTimestamp())
                .Returns(new DateTime(1997, 8, 29, 2, 14, 03));
            robotRunsAmokEvent.ID = Guid.NewGuid();
        });
    }
}
```

This will ensure that any new `RobotRunsAmokEventFakeOptionsBuilder`
will have an
[appropriate date](https://en.wikipedia.org/wiki/Skynet_(Terminator)#Before_Judgment_Day)
applied and will have a unique ID.

In addition to `ConfigureFake`, any
[explicit creation option](creating-fakes.md#explicit-creation-options)
can be used in `BuildOptions`, including implementing interfaces,
providing constructor arguments, and more.

##### How it works

FakeItEasy uses classes that implement the following interface to configure Fakes:

```csharp
public interface IFakeOptionsBuilder
{
    bool CanBuildOptionsForFakeOfType(Type type);
    void BuildOptions(Type typeOfFake, IFakeOptions options);
    Priority Priority { get; }
}
```

When FakeItEasy creates a Fake, it looks at all known
`IFakeOptionsBuilder` implementations for which
`CanBuildOptionsForFakeOfType` returns `true`. Then it passes an empty
`options` object to `BuildOptions`. If multiple implementations match,
the one with the highest `Priority`is used.

If all that's needed is a Fake Options Builder that configures a
single explicit type, extending `abstract class FakeOptionsBuilder<T>:
IFakeOptionsBuilder` is preferred, as was done above. This abstract
class provides default implementations of `Priority` and
`CanBuildOptionsForFakeOfType` (although the `Priority` can be
overridden if needed). If you want to configure a variety of Fake
types, you may prefer to extend `IFakeOptionsBuilder` directly. For
example, if you wanted all Fakes to be Strict, you might write
something like this:

```csharp
class MakeEverythingStrictOptionsBuilder : IFakeOptionsBuilder
{
    public bool CanBuildOptionsForFakeOfType(Type type)
    {
        return true;
    }

    public void BuildOptions(Type typeOfFake, IFakeOptions options)
    {
        options.Strict();
    }

    public Priority Priority
    {
        get { return Priority.Default; } // equivalent to value 0
    }
}
```

This method provides additional power, in that the Fake Options
Builder can be applied to more types, but it sacrifices compile-time
typesafety.  Of course, it's possible to perform more sophisticated
analysis on the types, perhaps having `CanBuildOptionsForFakeOfType`
accept only types whose name match a pattern. In this way,
conventions-based faking could be accomplished.

Note that once the type of Fake being created is identified, say as
`FakedType`, it's possible to cast `options` to a
`IFakeOptions<FakedType>` and operate on it, but the `FakedType` must
be the _exact_ type being faked, not just something in the inheritance
tree.

##### How does FakeItEasy find the Fake Options Builders?

On initialization, FakeItEasy
[looks for Discoverable Extension Points](scanning-for-extension-points.md),
including Fake Options Builders.


---


## formatting-argument-values.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/formatting-argument-values.md



### Formatting Argument Values

FakeItEasy tries to provide helpful error messages when an
[Assertion](assertion.md) isn't met. For example, when an expected call to a fake
method isn't made, or when an unexpected call _is_ made. Often these
messages are adequate, but sometimes there's a need to improve upon
them, which can be done by writing custom argument value formatters.

#### FakeItEasy's default formatter behavior

Unless custom formatters are provided, FakeItEasy formats argument
values like so:

- the `null` value is formatted as `NULL`,
- the empty `string` is formatted as `string.Empty`,
- other `string` values are formatted as `"the string value"`, including the quotation marks, and
- any other value is formatted as its `ToString()` result

There is no way to change FakeItEasy's behavior when formatting
`null`, but the other behavior can be overridden by user-defined
formatters.

#### Writing a custom argument value formatter
Just define a class that extends `FakeItEasy.ArgumentValueFormatter<T>`. Here's a sample that formats argument values of type `Book`:
```csharp
class BookArgumentValueFormatter : ArgumentValueFormatter<Book>
{
    protected override string GetStringValue(Book argumentValue)
    {
        return string.Format("'{0}' published on {1:yyyy-MM-dd}",
            argumentValue.Title, argumentValue.PublishedOn);
    }
}
```

This would help FakeItEasy display this error message:
<pre>
Assertion failed for the following call:
  SampleTests.ILibrary.Checkout(&lt;Ignored&gt;)
Expected to find it never but found it once among the calls:
  1: SampleTests.ILibrary.Checkout(<b>book: 'The Ocean at the End of the Lane', published on 2013-06-18</b>)
</pre>
which could make tracking down any failures a little easier.

Compare to the original behavior:
<pre>
Assertion failed for the following call:
  SampleTests.ILibrary.Checkout(&lt;Ignored&gt;)
Expected to find it never but found it once among the calls:
  1: SampleTests.ILibrary.Checkout(<b>book: SampleTests.Book</b>)
</pre>

In the original form of the message, the Book argument is just
formatted using `Book.ToString()` because FakeItEasy doesn't know any
better.

#### How it works

FakeItEasy uses classes that implement the following interface to format argument values:

```csharp
public interface IArgumentValueFormatter
{
    string GetArgumentValueAsString(object argumentValue);
    Type ForType { get; }
    Priority Priority { get; }
}
```

`GetArgumentValueAsString` does the work, transforming an argument into its formatted representation.  
`ForType` indicates what type of argument a formatter can format.  
`Priority` is discussed below.

Above, we wrote a formatter in the preferred way, by extending
`abstract class ArgumentValueFormatter<T>:
IArgumentValueFormatter`. `ArgumentValueFormatter<T>` defines a
`GetArgumentValueAsString` method that defers to `GetStringValue`, and
its `ForType` method simply returns `T`. The default implementation of
`Priority` returns `Priority.Default` (equivalent to value `0`), but
this can be overridden.  It's possible to write a formatter from
scratch, but there's no advantage to doing so over extending
`ArgumentValueFormatter<T>`.

It's possible to create formatters for any type, including concrete
types, abstract types, and interfaces. Formatters defined for base
types and interfaces will be used when formatting values whose types
extend or implement the formatter's type.

#### Resolving formatter collisions

It's possible for a solution to contain multiple formatters that would
apply to the same types of arguments. In fact, it's guaranteed to
happen, since FakeItEasy itself defines a formatter that applies to
`object`s and one that applies to `string`s. Any user-defined
formatter will conflict with at least the built-in object formatter,
and maybe others. When there is more than one candidate for formatting
an argument, FakeItEasy picks the best one based on two factors:

- the distance between the argument's type (hereafter _ArgType_) and the type each formatter knows about (hereafter _ForType_), and
- the value of each formatter's `Priority` property

##### Lowest distance

When an argument value needs to be formatted, FakeItEasy examines all
known formatters whose ForType is in ArgType's inheritance tree, or
whose ForType is an interface that ArgType implements. The _distance_
between ForType and ArgType is calculated as follows:

- 0 if ForType and ArgType are the same
- 1 if ForType is an interface that ArgType implements
- 2 if `ForType == ArgType.BaseType`, 
- 3 if `ForType == ArgType.BaseType.BaseType`, and so on, adding one for every step in the inheritance chain

The formatter whose ForType has the smallest distance to ArgType is used to format the argument.

##### Highest priority

Sometimes more than one formatter is found the same distance from
ArgType. Maybe two formatters actually specify the same `ForType`
property value, or there's a formatter defined for ArgType as well as
for an interface that ArgType implements.

When multiple formatters have the same distance from the argument,
FakeItEasy will select the one with the highest `Priority` property
value. If multiple formatters have the same distance _and_ the same
priority, the behavior is undefined.

All classes that extend `ArgumentValueFormatter<T>` have a `Priority`
property that returns `Priority.Default`, unless they explicitly
override it.  However, the formatters that FakeItEasy includes have a
`Priority` lower than `Priority.Default`, so unless two user-supplied
formatters apply to the same types, and yield the same distance when
applied to a type, there's no need to override the `Priority`
property.

##### How does FakeItEasy find Argument Value Formatters?

On initialization, FakeItEasy
[looks for Discoverable Extension Points](scanning-for-extension-points.md),
including Argument Value Formatters.


---


## custom-argument-equality.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/custom-argument-equality.md



### Custom argument equality

#### Default behavior when comparing argument values

By default, FakeItEasy compares argument values using `Object.Equals`. For
instance, consider this call configuration:

```csharp
A.CallTo(() => fake.DoSomething("hello")).Returns(42);
```

When comparing the argument value from the actual call with the configured
value `"hello"`, the values are compared with the default comparison rules
(using `String.Equals` in this case).

In most cases, that's what you want, but there are scenarios where it can be
inconvenient. For example, you might want to compare instances based on the
values of their properties, but by default the objects are compared using
reference equality. If you don't own the type, you can't override `Equals` to
implement the desired behavior. So you end up having to compare the properties
explicitly in an
[argument constraint](argument-constraints.md#custom-matching):

```csharp
Foo expectedFoo = ...;
A.CallTo(() =>
    fake.DoSomethingElse(A<Foo>.That.Matches(foo => foo.Bar == expectedFoo.Bar)))
    .Returns(42);
```

This is quite verbose, and not very readable. Ideally you would write it like
this:

```csharp
Foo expectedFoo = ...;
A.CallTo(() => fake.DoSomethingElse(expectedFoo)).Returns(42);
```

And it would just do the right thing.

FakeItEasy offers a way to override the default behavior by providing a custom
argument equality comparer.

#### Writing a custom argument equality comparer

Just define a class that inherits `ArgumentEqualityComparer<T>`, and override
the `AreEqual` method:

```csharp
public class FooComparer : ArgumentEqualityComparer<Foo>
{
    protected override bool AreEqual(Foo expectedValue, Foo argumentValue)
    {
        return expectedValue.Bar == argumentValue.Bar;
    }
}
```

FakeItEasy will automatically discover this class and use it to compare
instances of `Foo`.

#### How it works

FakeItEasy uses classes that implement the following interface to compare argument values:

```csharp
public interface IArgumentEqualityComparer
{
    bool CanCompare(Type type);
    bool AreEqual(object expectedValue, object argumentValue);
    Priority Priority { get; }
}
```

When FakeItEasy needs to compare a non-null argument value with a non-null expected value,
it looks at all known `IArgumentEqualityComparer` implementations for which
`CanCompare` returns true for the type of the expected value. If multiple implementations
match, the one with the highest `Priority` is used.

If all that's needed is an Argument Equality Comparer that specifies how to
compare two instances of a specific type, extending `abstract class
ArgumentEqualityComparer<T>: IArgumentEqualityComparer` is preferred. It
provides default implementations of `Priority` and `CanCompare` (although
they can be overridden if needed).

However, if you want to provide custom equality comparison for a variety of
types, you may prefer to implement `IArgumentEqualityComparer` directly. For
example, if you wanted all types in a specific namespace to be compared by
their string representation, you might write something like this:

```csharp
class ToStringArgumentEqualityComparer : IArgumentEqualityComparer
{
    public bool CanCompare(Type type) => type.Namespace == "MySpecialNamespace";

    public bool AreEqual(object expectedValue, object argumentValue)
    {
        return expectedValue.ToString() == argumentValue.ToString();
    }

    public Priority Priority => Priority.Default;
}
```

#### How does FakeItEasy find Argument Equality Comparers?

On initialization, FakeItEasy
[looks for Discoverable Extension Points](scanning-for-extension-points.md),
including Argument Equality Comparers.


---


## scanning-for-extension-points.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/scanning-for-extension-points.md



### Scanning for Extension Points

On initialization, essentially as soon as a FakeItEasy type is
accessed, FakeItEasy uses reflection to look for internal and
user-supplied extension points. In most cases, there is no _need_ for
users to define any extensions, but they may be used to enhance the
power and usability of FakeItEasy.

There are currently four kinds of extension points defined:

* [Custom Dummy Creation](custom-dummy-creation.md) rules,
* [Implicit Creation Options](implicit-creation-options.md),
* [Argument Value Formatters](formatting-argument-values.md), and
* [Argument Equality Comparers](custom-argument-equality.md)

Please see their individual documentation to learn how each of these is used.

#### The scanning process

On startup, FakeItEasy searches the following assemblies for classes that
implement the various extensions points:

* its own assembly,
* assemblies already loaded in the current AppDomain, if they reference
  FakeItEasy,
* assemblies referenced by assemblies from the previous bullet point, if they
  reference FakeItEasy,
* additional assemblies identified by the [Bootstrapper](bootstrapper.md)'s
  `GetAssemblyFileNamesToScanForExtensions` method, if they reference
  FakeItEasy.

Any such classes found are added to a catalogue and used at need.


---


## bootstrapper.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/bootstrapper.md



### Bootstrapper

Most of FakeItEasy's functionality is directly triggered by client
code: [creating a fake](creating-fakes.md),
[configuring a call](specifying-a-call-to-configure.md) and
[making assertions about calls](assertion.md) are all explicitly
invoked and are controllable by various input parameters.

Some behavior is triggered implicitly. FakeItEasy initializes itself
when its classes are first accessed. The Bootstrapper
allows users to customize the initialization process.

#### What does the Bootstrapper do?

At present, the Bootstrapper provides only one service:

* `GetAssemblyFileNamesToScanForExtensions` provides a list of
  absolute paths to assemblies that should be
  [scanned for extension points](scanning-for-extension-points.md).  
  The default behavior is to return an empty list.

#### How can the behavior be changed?

Provide an alternative bootstrapper class and ensure that it is loaded
in the current AppDomain before FakeItEasy is initialized (often
this means just including it in your test assembly).

The best way to provide an alternative implementation is to **extend
FakeItEasy.DefaultBootstrapper**. This class defines the default
FakeItEasy setup behavior, so using it as a base allows clients to
change only those aspects of the initialization that need to be
customized.

##### An example: returning a specific extra assembly scan for extensions

Most often, FakeItEasy extension points will be defined in assemblies
that are already loaded at the time that FakeItEasy is used. In some
cases, extensions may reside in assemblies that are not (yet)
loaded. Perhaps the extensions are distributed in a shared assembly
that does not need to be referenced by any other code. The following
bootstrapper can be used to force an additional assembly to be scanned
for extension points.

```csharp
public class ScanAnExternalAssemblyBootstrapper : FakeItEasy.DefaultBootstrapper
{
    public override IEnumerable<string> GetAssemblyFileNamesToScanForExtensions()
    {
        return new [] { @"c:\full\path\to\another\assembly.dll" };
    }
}
```

#### How does FakeItEasy find alternative bootstrappers?

Just before the first Bootstrapper function needs to be accessed,
FakeItEasy checks all the assemblies currently loaded in the
AppDomain. Each assembly is examined for exported types that implement
`FakeItEasy.IBootstrapper`. The first such type that is not
`FakeItEasy.DefaultBootstrapper` is instantiated and used. If no such
type is found, then `FakeItEasy.DefaultBootstrapper` is used.

**Note that there is no warning provided if FakeItEasy finds more than
one custom bootstrapper implementation. One will be chosen
non-deterministically.**


---


## advanced-usage.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/advanced-usage.md



### Advanced usage

FakeItEasy exposes a few APIs that aren't commonly needed, but can be useful in certain scenarios.

#### Resetting a fake to its initial state

In most cases, you should just discard the fake after use and create a new one; in other words,
don't reuse a fake between tests if you can avoid it.

However, in some situations this might not be practical. To actually reset a fake to its initial
state, use the `Fake.Reset` method:

```csharp
Fake.Reset(fake);
```

This method resets all changes made to the fake after it was created. This includes:

* call configurations
* values of [auto-faked read-write properties](default-fake-behavior.md#readwrite-properties)
* event handlers
* [recorded calls](#getting-the-list-of-calls-made-on-a-fake)
* [interception listeners](#intercepting-calls)

However, any changes made _during_ fake creation (via fake creation options) will be preserved.
This includes all the elements mentioned above, but also:

* [strictness](strict-fakes.md),
* [object wrapping](calling-wrapped-methods.md),
* [redirecting to base methods](calling-base-methods.md), and
* other [explicit](creating-fakes.md#explicit-creation-options) or
  [implicit](implicit-creation-options.md) creation options.

#### Clearing a fake's recorded calls

The `Fake.ClearRecordedCalls` method clears all recorded calls from a fake. Subsequent call assertions won't see these calls.

```csharp
var foo = A.Fake<IFoo>();
foo.Bar();
A.CallTo(() => foo.Bar()).MustHaveHappened();
Fake.ClearRecordedCalls(foo);
A.CallTo(() => foo.Bar()).MustNotHaveHappened();
```

#### Getting the list of calls made on a fake

Sometimes, the call assertion API offered by FakeItEasy isn't enough, and you need to manually check the calls made on a fake.
The `Fake.GetCalls` method returns a fake's recorded calls as a list of `ICompletedFakeObjectCall` objects that you can examine yourself:

```csharp
var foo = A.Fake<IFoo>();
foo.Bar();
foo.Baz();
var calls = Fake.GetCalls(foo).ToList();
Assert.Equal(2, calls.Count);
Assert.Equal("Bar", calls[0].Method.Name);
Assert.Equal("Baz", calls[1].Method.Name);
```

#### The `FakeManager` object

The `Fake.GetFakeManager` method returns a `FakeManager` object that can be used to get information on the fake and manipulate its call rules.

`Fake.GetFakeManager` throws an exception if the provided object is not a fake. To test if an object is a fake you can call `Fake.IsFake`
or try get the `FakeManager` with `Fake.TryGetFakeManager` which will return true if the provided object is a fake and also give you the `FakeManager`
for that object via an out parameter.

##### Getting the type of the fake

The `FakeManager.FakeObjectType` property returns the type of the fake, i.e. the type that was passed to `A.Fake`. This can be useful
if you're writing code that dynamically manipulates fakes.

```csharp
var foo = A.Fake<IFoo>();
var manager = Fake.GetFakeManager(foo);
Assert.Equal(typeof(IFoo), manager.FakeObjectType);
```

##### Getting the fake from the fake manager

The `FakeManager.Object` property returns the fake object managed by this `FakeManager`.

```csharp
var foo = A.Fake<IFoo>();
var manager = Fake.GetFakeManager(foo);
Assert.Equal(foo, manager.Object);
```

##### Manipulating a fake's call rules

The `FakeManager.Rules` property returns all the rules configured on a fake.

```csharp
var foo = A.Fake<IFoo>();
A.CallTo(() => foo.Bar()).Returns(42);
A.CallTo(() => foo.Baz()).DoesNothing();
var manager = Fake.GetFakeManager(foo);
var rules = manager.Rules.ToList();
Assert.Equal(2, rules.Count);
```

It is also possible to add custom rules for advanced scenarios, using `AddRuleFirst` and `AddRuleLast`. `AddRuleFirst` adds a rule at the beginning of the rule list, so that it's considered before any other rule (which is the normal behavior when configuring a fake with  `A.CallTo(...)`). `AddRuleLast` adds a rule at the end of the rule list, so that it's considered after any other rule.

```csharp
class MyRule : IFakeObjectCallRule
{
    public int? NumberOfTimesToCall => null;

    public void Apply(IInterceptedFakeObjectCall fakeObjectCall)
    {
        fakeObjectCall.SetReturnValue(42);
    }

    public bool IsApplicableTo(IFakeObjectCall fakeObjectCall)
    {
        return fakeObjectCall.Method.DeclaringType == typeof(IFoo)
            && fakeObjectCall.Method.Name == "Bar";
    }
}

...

var foo = A.Fake<IFoo>();
var manager = Fake.GetFakeManager(foo);
manager.AddRuleFirst(new MyRule());
Assert.Equal(42, foo.Bar());
```

You can also remove a rule using the `RemoveRule` method.

Note that if your custom rule is stateful, you should implement the
`IStatefulFakeObjectCallRule` interface to provide a way to get a snapshot of
the current state of your rule. This is necessary for `Fake.Reset` to work
correctly.

##### Intercepting calls

Using the `FakeManager.AddInterceptionListener` method, you can add a listener that is called every time a fake method is called.

```csharp
class MyListener : IInterceptionListener
{
    public void OnBeforeCallIntercepted(IFakeObjectCall interceptedCall)
    {
        Console.WriteLine($"A call to '{interceptedCall.Method}' is about to be processed");
    }

    public void OnAfterCallIntercepted(ICompletedFakeObjectCall interceptedCall)
    {
        Console.WriteLine($"A call to '{interceptedCall.Method}' has been processed");
    }
}

...

var foo = A.Fake<IFoo>();
A.CallTo(() => foo.Baz()).Invokes(() => Console.WriteLine("Hello world"));
var manager = Fake.GetFakeManager(foo);
manager.AddInterceptionListener(new MyListener());
foo.Baz();
```

The code above prints:

```
A call to 'Void Baz()' is about to be processed
Hello world
A call to 'Void Baz()' has been processed
```


---


## faking-http-client.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/Recipes/faking-http-client.md




Let's assume that you want to create a fake `HttpClient` so you can dictate the
behavior of the
[GetAsync(String)](https://learn.microsoft.com/en-us/dotnet/api/system.net.http.httpclient.getasync?view=net-7.0#system-net-http-httpclient-getasync(system-string))
method. This seems like it would be a
straightforward task, but it's complicated by the design of `HttpClient`, which
is not faking-friendly.

#### A working Fake

First off, let's look at the declaration of `GetAsync`:

```csharp
public Task<HttpResponseMessage>GetAsync(string? requestUri)
```

This method is neither virtual nor abstract, and so [can't be overridden by
FakeItEasy](../what-can-be-faked.md#what-members-can-be-overridden).

As a workaround, we can look at the [definition of
GetAsync](https://github.com/dotnet/runtime/blob/ab5e28c1cab305450897749daa7393bef30d7505/src/libraries/System.Net.Http/src/System/Net/Http/HttpClient.cs#L363-L364)
and see that the method eventually ends up calling
[HttpMessageHandler.SendAsync(HttpRequestMessage,
CancellationToken)](https://learn.microsoft.com/en-us/dotnet/api/system.net.http.httpmessagehandler.sendasync?view=net-7.0#system-net-http-httpmessagehandler-sendasync(system-net-http-httprequestmessage-system-threading-cancellationtoken))
on an `HttpMessageHandler` that can be supplied via the `HttpClient`
constructor.

`HttpMessageHandler.SendAsync` is protected, which makes it
less convenient to override than a public method. We need to specify the call by
name, and to give FakeItEasy a hint about the return type, as described in
[Specifying a call to any method or
property](../specifying-a-call-to-configure.md#specifying-a-call-to-any-method-or-property).

With this knowledge, we can write a passing test:

??? note "This is a simplified example"
    In the interest of brevity, we create a Fake, exercise it directly, and check
    its behavior. A more realistic example would create the Fake as a collaborator
    of some production class (the "system under test") and the Fake would not be
    called directly from the test code.

```csharp
--8<--
recipes/FakeItEasy.Recipes.CSharp/FakingHttpClient.cs:FakeAnyMethodWay
--8<--
```

#### Easier and safer call configuration

The above code works, but specifying the method name and return type is a little
awkward. A `FakeableHttpMessageHandler` class can be used to clean things up and
to also supply a little compile-time safety by ensuring we're configuring the
expected method.

```csharp
--8<--
recipes/FakeItEasy.Recipes.CSharp/FakingHttpClient.cs:FakeableHttpMessageHandler

recipes/FakeItEasy.Recipes.CSharp/FakingHttpClient.cs:FakeByMakingMessageHandlerFakeable
--8<--
```

#### Faking PostAsync

The techniques above can be used to intercept calls to  methods other than `GetAsync` as well.

Consider this test, which ensures that the correct content is passed to `HttpClient.PostAsync`.

??? bug "Does not work for .NET Framework"
    When run under .NET Framework, the request content is disposed
    as soon as the request is made, as explained in
    [HttpClient source code comments](https://github.com/dotnet/runtime/blob/b0b7aaefb88aa8d01b3d64fb40ac2f73a9d98c3e/src/libraries/System.Net.Http/src/System/Net/Http/HttpClient.cs#L665-L680).

```csharp
--8<--
recipes/FakeItEasy.Recipes.CSharp/FakingHttpClient.cs:FakePostAsync
--8<--
```


---


## platform-support.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/platform-support.md



### Platform support

#### Supported platforms for released versions

See the "Dependencies" section of the [FakeItEasy NuGet Gallery page](https://www.nuget.org/packages/FakeItEasy) to see each released package's supported platforms and package dependencies.

#### Platform support policy

Beginning with FakeItEasy 8.0.0, we intend to publish new packages that target

* .NET versions designated as Long Term Support (LTS) and currently supported by Microsoft
* .NET Framework 4.6.2[^1]

See the [.NET Support Policy](https://dotnet.microsoft.com/en-us/platform/support/policy) to learn which target frameworks
are categorized as "Long Term Support", and when support begins and ends.

We will add builds for new LTS .NET versions as they are released, and remove them from
new FakeItEasy builds once support is ended.

It's not clear that there's a global consensus on whether the removal of a supported target framework constitutes
a breaking change and therefore requires a new major version. We choose to limit surprises to FakeItEasy's clients
and issue a new major release when dropping a supported target framework.

[^1]:
    There are no explicit plans to remove support for .NET Framework, but we may increase the supported
    version or remove support if Microsoft ends support for .NET Framework, or if supporting it within
    FakeItEasy becomes too onerous.


---


## contributing.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/contributing.md



../contributing.md


---


## how-to-build.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/how-to-build.md



../how-to-build.md


---


## why-was-fakeiteasy-created.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/why-was-fakeiteasy-created.md



#### Introduction

There was a good question on Stack Overflow that asks what
distinguishes FakeItEasy from other libraries. Creator of FakeItEasy,
Patrik H&auml;gne, answered the question there but we reproduce the
answer here. Note that the text has been preserved, and particular
constructs referenced (such as `DummyDefinitions`) have changed or
been renamed in newer versions of FakeItEasy.

The question on Stack Overflow:
"[Are fakes better than Mocks?](https://stackoverflow.com/questions/4001101/are-fakes-better-than-mocks)"

#### Patrik H&auml;gne's answer

To be clear, I created FakeItEasy so I'll definitely not say whether
one framework is better than the other, what I can do is point out
some differences and motivate why I created FakeItEasy. Functionally
there are no major differences between Moq and FakeItEasy.

FakeItEasy has no "Verifiable" or "Expectations" it has assertions
however, these are always explicitly stated at the very end of a test,
I believe this makes tests easier to read and understand. It also
helps beginners to avoid multiple asserts (where they would set
expectations on many calls or mock objects).

I used Rhino Mocks before and I quite liked it, especially after the
AAA-syntax was introduced I did like the fluent API of Moq better
though. What I didn't like with Moq was the "mock object" where you
have to use mock.Object everywhere, I like the Rhino-approach with
"natural" mocks better. Every instance looks and feels like a normal
instance of the faked type. I wanted the best of both worlds and also
I wanted to see what I could do with the syntax when I had absolutely
free hands. Personally I (obviously) think I created something that is
a good mix with the best from both world, but that's quite easy when
you're standing on the shoulders of giants.

As has been mentioned here one of the main differences is in the
terminology, FakeItEasy was first created to introduce TDD and mocking
to beginners and having to worry about the differences between mocks
and stubs up front is not very useful.

I've put a lot of focus into the exception messages, it should be very
easy to tell what went wrong in a test just looking at an exception
message.

FakeItEasy has some extensibility features that the other frameworks
don't have but these aren't very well documented yet.

FakeItEasy is (hopefully) a little stronger in mocking classes that
has constructor arguments since it has a mechanism for resolving
dummy-values to use. You can even specify your own dummy value
definitions by implementing a DummyDefinition(Of T) class within your
test project, this will automatically be picked up by FakeItEasy.

The syntax is an obvious difference, which one is better is largely a
matter of taste.

I'm sure there are lots of other differences that I forget about now
(and to be fair I've never used Moq in production myself so my
knowledge of it is limited), I do think these are the most important
differences though.


---


## external-resources.md

Source: https://github.com/FakeItEasy/FakeItEasy/blob/master/docs/external-resources.md



### External Resources

* [FakeItEasy courses on Pluralsight](https://www.pluralsight.com/search?q=fakeiteasy&categories=all)
* [The "fakeiteasy" tag on StackOverflow](https://stackoverflow.com/questions/tagged/fakeiteasy)
* [Migration from Moq to FakeItEasy with Resharper Search Patterns](https://www.planetgeek.ch/2013/07/18/migration-from-moq-to-fakeiteasy-with-resharper-search-patterns/)


---
