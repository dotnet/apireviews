# Usage of System.Text.Encodings.Web

This file contains some sample usage of the APIs in `System.Text.Encodings.Web`.

## Basic scenarios

### Encoding to a string

```CSharp
// prints: &lt;hello&gt; &lt;world&gt;
var encoder = HtmlEncoder.Default;
string encoded = encoder.Encode("<hello> <world>");
Console.Write(encoded);
```

### Encoding to a TextWriter

```CSharp
// prints: &lt;hello&gt; &lt;world&gt;
var encoder = HtmlEncoder.Default;
encoder.Encode(Console.Out, "<hello> <world>");
```

### Encoding from substrings

```CSharp
// prints: &lt;hello&gt;
var encoder = HtmlEncoder.Default;
encoder.Encode(Console.Out, "<hello> <world>", 0, 7);
```

### Encoding from arrays

```CSharp
// prints: &lt;hello&gt;
var encoder = HtmlEncoder.Default;
encoder.Encode(Console.Out, "<hello> <world>".ToCharArray(), 0, 7);
```

### Other encodings

#### URLs

```CSharp
// prints: What%20is%202%20%2B%202%3F
var encoder = UrlEncoder.Default;
string encoded = encoder.Encode("What is 2 + 2?");
Console.Write(encoded);
```
#### JavaScript

```CSharp
// prints: Seattle\u0027s nickname is the \u0022Emerald City\u0022.
var encoder = JavaScriptEncoder.Default;
string encoded = encoder.Encode("Seattle's nickname is the \"Emerald City\".");
Console.Write(encoded);
```

## Advanced scenarios

### Filtering

```CSharp
var filter = new CodePointFilter();
filter.AllowRange(UnicodeRanges.SupplementalMathematicalOperators);
var encoder = new DefaultHtmlEncoder(filter);
string encoded = encoder.Encode("<hello> <world>");
```

### Customizing encoders

You can also tweek the encoders:

```CSharp
// encodes all characters as '@'
class MyEncoder : HtmlEncoder
{
    public override int MaxOutputCharactersPerInputCharacter
    {
        get { return 1; }
    }

    public override bool Encodes(int unicodeScalar)
    {
        return true;
    }

    public override unsafe int FindFirstCharacterToEncode(
        char* text,
        int textLength)
    {
        return 0;
    }

    public override unsafe bool TryEncodeUnicodeScalar(
        int unicodeScalar,
        char* buffer,
        int bufferLength,
        out int numberOfCharactersWritten)
    {
        if (bufferLength < 1) {
            numberOfCharactersWritten = 0;
            return false;
        }

        numberOfCharactersWritten = 1;
        buffer[0] = '@';
        return true;
    }
}
```

Usage:

```CSharp
// prints: @@@@@@@
var encoder = new MyEncoder();
encoder.Encode(Console.Out, "<hello>");
```
