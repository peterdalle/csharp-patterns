# Simple patterns to make C# easier to read and write

Consider these suggestions as helpful ways you can refactor C# code to make is easier to read and easier to work with.

## Control program flow

### 1. Exit methods early with `return` before the real work

Make the method flatter (with fewer indents) by exiting early:

```csharp
public void Good✅(string username, int age)
{
    if (age < 18) return;
    if (username != "admin") return;

    try 
    {
        something.Save();
    }
    catch (Exception ex)
    {
        ...
    }
}
```

Instead of:

```csharp
public void Bad❌(string username, int age)
{
    if (age >= 18) 
    {
        if (username == "admin")
        {
            try 
            {
                something.Save();
            }
            catch (Exception ex)
            {
                ...
            }
        }
    }
}
```

### 2. Move complex `if` evaluations to variables

Make it easier to read by moving complex evaluations to its own variable:

```csharp
public void Good✅(string username, int age, bool isSuperUser, bool canReadMatters)
{
    bool userCanSave = (age >= 18 & username == "admin" & isSuperUser & canReadMatters);

    if (userCanSave) 
    {
        something.Save();
    }
}
```

Instead of:

```csharp
public void Bad❌(string username, int age, bool isSuperUser, bool canReadMatters)
{
    if (age >= 18 & username == "admin" & isSuperUser & canReadMatters)
    {
        something.Save();
    }
}
```

### 3. Simplify `if` statements by dropping `else`

Sometimes it can be simpler to skip the `else` statement altogether:

```csharp
public void Good✅()
{
    if (userCanSave) 
    {
        something.Save();
        return true;
    } 
    return false;
}
```

Instead of:

```csharp
public void Bad❌()
{
    if (userCanSave) 
    {
        something.Save();
        return true;
    } 
    else 
    {
        return false;
    }
}
```

## Literals and constants

### 4. Use `nameof` instead of hardcoded literal strings

Don't hardcode names as strings &mdash; use `nameof` to get the benefits of automatic renaming:

```csharp
public void Good✅(string name)
{
    throw new ArgumentException("Name cannot be empty.", paramName: nameof(name));
}
```

Instead of:

```csharp
public void Bad❌(string name)
{
    throw new ArgumentException("Name cannot be empty.", paramName: "name");
}
```
## Documentation

### 5. Use `<inheritdoc />` instead of duplicating text

Don't write more documentation than you have to. Use [tags for documentation](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/documentation-comments#d3-recommended-tags):

```csharp
/// <summary>
/// This documentation is used by both Good and Gooder.
/// </summary>
public void Good✅()
{
}

/// <inheritdoc cref="Good()"/>
public void Gooder✅()
{
}
```

Instead of:

```csharp
/// <summary>
/// This documentation is only used here.
/// </summary>
public void Bad❌()
{
}

/// <summary>
/// This documentation is only used here.
/// </summary>
public void Bader❌()
{
}
```
