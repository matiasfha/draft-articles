---
keywords: 
- Intl
- Intl Javascript
- Internationalization
- Javascript Internationalization
- JS Internationalization
- i18n
- Javascript i18n
- JS i18n
- Intl Date formatting
- Intl currency
- Pluralization
summary: As your app grows you may want to display your content in different formats and even different languages. Javascript offers an API for this purpose, the **Intl** object.
---
# Knowing the **Intl** Javascript API
Personalizing the user experience is becoming more critical every day, even more so if your application or content is consumed by users in different parts of the world who most likely use other languages, date formats, currency, etc.

There are multiple solutions to make your content adapt to the location or language of your users. Still, many of these methods have become outdated, complex, or dependent on a particular framework.

Javascript also offers an internationalization solution, the **Intl** object.

## What is Internationalization?


The concept of internationalization, or i18n, is the process of supporting different languages, and countries in your application.

i18n is commonly confused with Localization or even translation, but i18n refers to developing a product, a process focused on supporting different languages and formats based on locality — a common code base.

Providing internationalization support is critical for many products but often overlooked.

> Localization refers to the generation of a specific product to target a market or region, including translating content and even modifying the interface and terminology.

Typically i18n implementation may include:

- Development of software that is independent of a specific language or cultural convention. (e.g. display of times and dates)
- Use of localization frameworks
- Removal of "hard-coded" text in the code
- Support for bi-directional languages
- Support for different number formats

## The **Intl** object

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/**Intl**), _"The **Intl** object is the namespace for the ECMAScript Internationalization API, which provides language sensitive string comparison, number formatting, and the date and time formatting. The **Intl** object provides access to several constructors and functionality common to the internationalization constructors and other language sensitive functions."_

In other words, through the global **Intl** object, you can access a set of tools to work with content sensitive to the user's language.

Most browsers currently support these methods well based on information available at [caniuse.com](https://caniuse.com/?search=Internationalization%20API)

![Can I Use: **Intl**](https://res.cloudinary.com/matiasfha/image/upload/v1662665832/Screen_Shot_2022-09-07_at_16.45.50_anek8r.png)

TLDR; A quick example of what this API can achieve:

```js 
const value = 1234.39;
const today = new Date("2022-09-07");

/**
 * @param locale is a string that represents a location code
 */
function dateFormatter(locale) {
  const dateFormatter = new Intl.DateTimeFormat(locale)
  const currencyFormatter = new Intl.NumberFormat(locale, {
    style: 'currency',
    currency: 'USD'
  })

  const date = dateFormatter.format(today)
  const currency = currencyFormatter.format(value)
  console.log(locale, date, currency);
}

dateFormatter("en-US");
// expected output: en-US 9/7/2022 $1,234.39

dateFormatter("es-ES");
// expected output: es-ES 7/9/2022 1234,39 US$
```

<small>You can see a demo of this code in [the following link](https://jsitor.com/BtkNjWJNw)</small>

In this code, you can briefly see some methods and transformations that **Intl** can do.

- Gives you static access to different constructors, in this case, `DateTimeFormat` and `NumberFormat`
- Allows you to "transform" content from one format to another based on the value of `locale`

It is essential to note that the `locale` argument is received by all constructors exposed by **Intl**. This `locale` is the value you will ideally capture dynamically so you can modify content based on its value.

Browsers offer a method of getting the value of `locale` based on the user's preferences or location.

```js
const locale = navigator.language
console.log(locale); // "es-CL"
```

## What is `locale`, and how does it work?

When we refer to `locale` we refer to a string that represents a group of user preferences like:

- Date and Time
- numbers and currency
- Time zones, languages, and countries
- Measurement units

This `locale` string must follow a particular format to be used, this consists of:

* A subtag or language code
* (optional) a region or country subtag
* (optional) a subtag script
* (optional) one or more variation subtags
* (optional) one or more [BCP-47](https://datatracker.ietf.org/doc/html/rfc4647#section-3.4) extension sequences

![locale example](https://res.cloudinary.com/matiasfha/image/upload/v1662665973/Screen_Shot_2022-09-08_at_15.39.23_mvtmlf.png)

Each subtag or sequence used to create the `locale` string is separated by a hyphen. Also, identifiers identify the case.

Some examples of `locale` strings
* "es": Spanish (language)
* "es-CL": Spanish (language) as it is used in Chile (region)
* "zh-Hans-CN": Simplified Chinese (language) (script) as used in China (region)


When a `locale` string is passed as an argument to one of **Intl**'s provided constructors, it is compared to a list of available `locales` for the best fit. This process is done using one of two possible algorithms: `lookup` or `best-fit`.

The `lookup` algorithm checks if the runtime environment has the `locale` used by searching from the most specific to the least detailed result. If the exact value of `locale` is unavailable, it will return the closest one. For example, if you search for `de-DE-u-co-phonebk` if not found, you can return `de-DE`, and in case of not seeing `de` reference will return a default value.

The `best-fit` algorithm is an improvement of the previous algorithm where a default value is not returned if the search is not found. If not, the one that best "fits" is returned. IF `es-CL` is searched for but not found, `es-AR` will be returned instead of just `es`.

You can learn more about this process by [reviewing the documentation on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/**Intl**#locale_identification_and_negotiation).


#### What does each subtag of the string mean?

As described above, the string locale is made up of different parts separated by a hyphen, the first section being the language identifier. 

What are the rest used for?

##### Script code

This script code is used to identify in what "format" a particular language is written. For example, in Asian languages `Hans` means that Simplified Chinese will be used vs. `Hant`, which indicates that Traditional Chinese will be used.

##### Variant code

Variant codes represent the different dialect options for a specific language.

##### Extensions

Extensions include identifiers for different calendars, and numeric or ordering systems. In the example of the previous image, `phonebk` identifies the variant indicating the ordering used for the letters, in this case phone book style.

## Time to code

### Formatting dates and time 

Let's start by formatting dates and times based on different values of `locale`.

For this, you need access to the `DateTimeFormat` constructor.
```js
const date = new Date(); //Today's date

// Fecha en formato USA
const usaDate = new Intl.DateTimeFormat('en-US').format(date) // 9/8/2022

// Fecha en formato Chile
const clDate = new Intl.DateTimeFormat('es-CL').format(date) // 08-09-2022

// Fecha en formato Aleman
const deDate = new Intl.DateTimeFormat('de').format(date) // 8.9.2022

// Fecha en formato Arabico egipto
const arDate = new Intl.DateTimeFormat('ar-eg').format(date) // ٨‏/٩‏/٢٠٢٢
```

<small>Find a demo of this code [in this playground](https://jsitor.com/0XTIZA4QI)</small>

This is the basic way to format date and time, but each constructor provided by the **Intl** object accepts a second argument that allows you to modify the result.

For `DateTimeFormat` the options are:

```ts
type Options = {
  dateStyle: 'full' | 'long' | 'medium' | 'short',
  timeStyle: 'full' | 'long' | 'medium' | 'short',
  calendar: 'buddhist' | 'chinese' | 'coptic' | 'dangi' | 'ethioaa' | 'ethiopic' | 'gregory' | 'hebrew' | 'indian' | 'islamic' | 'islamic-umalqura' | 'islamic-tbla' | 'islamic-civil' | 'islamic-rgsa' | 'iso8601' | 'japanese' | 'persian' | 'roc' | 'islamicc',
  dayPeriod: 'narrow' | 'short' | 'long',
  numberingSystem: 'arab' | 'arabext' | 'bali' | 'beng' | 'deva' | 'fullwide' | ' gujr' | 'guru' | 'hanidec' | 'khmr' | ' knda' | 'laoo' | 'latn' | 'limb' | 'mlym' | 'mong' | 'mymr' | 'orya' | 'tamldec' | 'telu' | 'thai' | 'tibt',
  localeMatcher: 'lookup' | 'best fit',
  year: "numeric" | "2-digit",
  month: "numeric" | "2-digit" | "long" | "short" | "narrow",
  day: "numeric" | "2-digit",
  hour: "numeric" | "2-digit",
  minute: "numeric" | "2-digit",
  second: "numeric" | "2-digit",
  era: "long" | "short" | "narrow",
  weekday: "long" | "short" | "narrow",
  hourCycle: 'h11'|'h12'|'h23'|'h24',
  hour12: boolean,
  timeZone: string,
  formatMatcher: 'basic' |'best fit',
  timeZoneName: 'long' | 'short' |'shortOffset'|'longOffset'|'shortGeneric'| 'longGeneric'
}
```

You have many options to define the best way to display a date and time in your application.

Let's review some examples.

```js
const options = {
  year: "2-digit",
  month: "long",
  day: "numeric",
  hour: "numeric",
  minute: "numeric",
  second: "numeric",
  weekday: "long",
  hour12: true,
  timeZone: 'America/Santiago'
}

// Date in USA format
const usaDate2 = new Intl.DateTimeFormat('en-US', options).format(date)
// Thursday, September 8, 22 at 10:20:54 AM

const clDate2 = new Intl.DateTimeFormat('es-CL', options).format(date)
// jueves, 8 de septiembre de 22, 10:20:54 a. m.

// Date in German format
const deDate2 = new Intl.DateTimeFormat('de', options).format(date)
//Donnerstag, 8. September 22 um 10:20:54 AM

// Date in Arabic Egypt format
const arDate2 = new Intl.DateTimeFormat('ar-eg', options).format(date)
// الخميس، ٨ سبتمبر ٢٢ في ١٠:٢٠:٥٤ ص
```

### Formatting Numbers

Another constructor offered by **Intl** is `NumberFormat`. You can use this constructor to change the way numbers are represented on the screen.

The only difference between `DateTimeFormat` and `NumberFormat` constructors is the set of options that these contructors receives.

```ts
type Options = {
    compactDisplay?: "short" | "long"; // Only used when notation is "compact"
    currencyDisplay?: "symbol" | "narrowSymbol" | "code" | "name";
    currencySign?: "standard" | "accounting";
    localeMatcher?: "lookup" | "best fit";
    notation?: "standard" | "scientific" | "engineering" | "compact";
    numberingSystem?: 'arab' | 'arabext' | 'bali' | 'beng' | 'deva' | 'fullwide' | 'gujr' | 'guru' | 'hanidec' | 'khmr' | 'knda' | 'laoo' | 'latn' | 'limb' | 'mlym' | 'mong' | 'mymr' | 'orya' | 'tamldec' | 'telu' | 'thai' | 'tibt';
    signDisplay?: "auto" | "always" | "exceptZero" | "negative" | "never" ;
    style?: "decimal" | "currency" | "percent" | "unit";
    unit?: string;
    unitDisplay?: "long" | "short" | "narrow";
    useGrouping?: "always" | "auto" | boolean | "min2";
    roundingMode?: "ceil" | "floor" | "expand" | "trunc" | "halfCeil" | "halfFloor" | "halfExpand" | "halfTrunc" | halfEven";
    roundingPriority?: "auto" | "morePrecision" | "lessPrecision";
    roundingIncrement?: 1 | 2 | 5 | 10 | 20 | 25 | 50 | 100 | 200 | 250 | 500 | 1000 | 2000 | 2500 | 5000;
    trailingZeroDisplay?: "auto" | "stripIfInteger";
    minimumIntegerDigits?: number;
    minimumFractionDigits?: number;
    maximumFractionDigits?: number;
    minimumSignificantDigits?: number;
    maximumSignificantDigits?: number;
}
```
Using this extensive list of options, you can very likely format a number with every possible use case.

Check this quick example.

```js
function currencyFormatter({ currency, value}) {
  const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    minimumFractionDigits: 2,
    currency
  }) 
  return formatter.format(value)
}

const value = 123456
const dollar = currencyFormatter({
  currency: "USD",
  value
}) //$123,456.00

const pound = currencyFormatter({
  currency: "GBP",
  value
}) // £123,456.00


const peso =  currencyFormatter({
  currency: "CLP",
  value
}) // CLP 123,456.00

const dinar = currencyFormatter({
  currency: "DZD",
  value
}) // DZD 123,456.00
```

In this code snippet, you can see the following
- A function was created to hold the formatter.
- The formatter is always using the `en-US` locale.
- Three options define what style of number formatting will use `style: currency`.

Even though we kept the same locale, the values returned by the formatting function are different.

> What would happen if you also changed the locale for each currency conversion?

In case these options don't cover your use-case, you can use another method that is provided by the formatter: `formatToParts`. This method will return an array of objects representing the number as a string in parts so you can use them to customize and solve your particular situation.

```js
const formatToParts = new Intl.NumberFormat('en-US', {
  currency: 'USD',
  style: 'currency',
  minimumFractionDigits: 2
}).formatToParts(value) 
/*
[
  { type: "currency", value: "$"},
  { type: "integer", value: "123" },
  { type: "group", value: "," },
  { type: "integer", value: "456" },
  { type: "decimal", value: "." },
  { type: "fraction", value: "00" }
]
```


What if you want to show a number like the social networks? Like `1.2K`?

The `NumberFormat` can solve that.

```js
// Compact format 

function formatCompact(value) {
  const result = new Intl.NumberFormat(
    'en-US',
    { notation: "compact"}
  ).format(value)
  return result;
}

const res1 = formatCompact(123)     // 123
const res2 = formatCompact(1234)    // 1.2K
const res3 = formatCompact(12345)   // 12K
const res4 = formatCompact(123456)  // 123K
const res5 = formatCompact(1234567) // 1.2M
```

In this case, the solution involves the use of the `notation` option. 

You can find a demo for the number formatting [in this playground.](https://jsitor.com/YX91hj0gX)

### Relative Time

Sometimes, you want to display the dates in a relative form such as: `2 weeks ago`. You can do this by doing some math and string concatenation with the dates, but *Intl* helps you with this.

*Intl* expose a constructor named *RelativeTimeFormat* that, same as before, accepts an argument to tell it what language you want to use and a set of options

```ts
type Options = {
    localeMatches: 'best fit' | 'lookup',
    numeric: 'always' |'auto'  // The format of the output
    style: 'long' | 'short' | 'narrow'
}
```

For this constructor the `format` method accepts a numeric value (*NOT A DATE*) like this

```js
const formatter = new Intl.RelativeTimeFormat('en-US');
const monthAgo = formatter.format(-1, 'month') // 1 month ago
const futureMonth = formatter.format(1, 'month') // in 1 month
```

When would you use something like this? 

Imagine a list of articles, each article have a publication date, but you don't want to show just that, you want to show how much relative time is between the publication date and today (the day the user is reading the list). 

To accomplish this you'll need to do a little math to get the difference between the two dates and use that result as a relative time value.

```js
const publicationDate = new Date('2022/01/05')

const currentDate = new Date()

const msPerDay = 1000 * 60 * 60 * 24;

const diffTime = Math.abs(currentDate - publicationDate);
const diffDays = Math.ceil(diffTime / msPerDay);


const enRtf = new Intl.RelativeTimeFormat("en-US", {
  numeric: 'auto',
});

console.log(enRtf.format(-diffDays, "day")); // 251 days ago
console.log(enRtf.format(-diffDays/30, "month")); //8.367 months ago


const esRtf = new Intl.RelativeTimeFormat("es-ES", {
  numeric: 'auto',
});

console.log(esRtf.format(-diffDays, "day")); // hace 251 días
console.log(esRtf.format(-diffDays / 30, "month")); // hace 8.367 meses
```
<small> Check the demo playground [in this link](https://jsitor.com/P9XZ1ET9L)</small>


### Pluralization

The *Intl* object also supports a way to define plural-sensistive content with the *PluralRules* constructor.

One use case for this feature is to show the number of items that exist in your collection, ideally this should respect the grammatical numbering in each language you choose to use.

You can always get around this requirement by writing your copy avoiding the need of pluralization, but what happen is you need it?

Let's check an example

```js
function pluralize(locale, count, singular, plural) {
    const pluralRules = new Intl.PluralRules(locale);
    const numbering = pluralRules.select(count);
    switch (numbering) {
        case 'one':
        return count + ' ' + singular;
        case 'other':
        return count + ' ' + plural;
        default:
        throw new Error('Unknown: '+ numberig);
    }
}

function showItemsQuantity(count) {
    return pluralize('en-US', count, 'item', 'items')
}

const zeroItem = showItemsQuantity(0) // 0 items
const oneItem = showItemsQuantity(1) // 1 item
const manyItem = showItemsQuantity(12) // 12 items

// Spanish
function mostrarCantidadCajas(count) {
    return pluralize('es-ES', count, 'caja', 'cajas')
}
const ceroItem = mostrarCantidadCajas(0) // 0 cajas
const unoItem = mostrarCantidadCajas(1) // 1 caja
const muchosItem = mostrarCantidadCajas(12) // 12 cajas

```
<small>Check the demo [in the playground](https://jsitor.com/exTjrc0s_V)</small>
> Note: in a real-woprld sceneario, you wouldn't hardcode plurals 
> in this snippet; they'd be part of your translation files

Maybe this example is too naive, since English and Spanish have just two pluralization rules; however, not every language follow this rule, some have only a single plural form, while other have multiple forms. 

> This example has been borrowed from [v8 blog](https://v8.dev/features/intl-pluralrules)

```js
const suffixes = new Map([
  ['zero',  'cathod'],
  ['one',   'gath'],
  // Note: the `two` form happens to be the same as the `'one'`
  // form for this word specifically, but that is not true for
  // all words in Welsh.
  ['two',   'gath'],
  ['few',   'cath'],
  ['many',  'chath'],
  ['other', 'cath'],
]);
const pr = new Intl.PluralRules('cy');
const formatWelshCats = (n) => {
  const rule = pr.select(n);
  const suffix = suffixes.get(rule);
  return `${n} ${suffix}`;
};

formatWelshCats(0);   // '0 cathod'
formatWelshCats(1);   // '1 gath'
formatWelshCats(1.5); // '1.5 cath'
formatWelshCats(2);   // '2 gath'
formatWelshCats(3);   // '3 cath'
formatWelshCats(6);   // '6 chath'
formatWelshCats(42);  // '42 cath'
```

This constructor just like the others exposed by *Intl* accepts a second argument to define options. One option is the `type` that allows you to define the selection rule. By default it uses the `cardinal` option. If you want to get the `ordinal` indicator for a number (for example to create a list ) you can accomplish that as follows:

```js
const pr = new Intl.PluralRules('en-US',{
    type: 'ordinal'
})
const suffixes = {
    one: 'st',
    two: 'nd',
    few: 'rd',
    other: 'th'
}

const formatOrdinals = n => `${n}${suffixes[pr.select(n)]}`
formatOrdinals(0) // 0th
formatOrdinals(1) //1sst
formatOrdinals(2) // //2nd
formatOrdinals(40) //40th
formatOrdinals(63) // 63rd
formatOrdinals(100) // 100nd
```


### List Formatting

Displaying a list is one of the most used ways to showcase information in a web app. But since your users speaks different language you need a way to format the list based on that language convention. 

To avoid the hassle of implementing this formatting rules by hand - that could be really hard - *Intl* offers the *ListFormat* API.

```js
function getFormatter(locale = 'en-US') {
  const  lf  = new Intl.ListFormat(locale);
  return lf
}

const enLf = getFormatter()
const zfLf = getFormatter('zh')

enLf.format(['one']);                           // One
enLf.format(['one', 'two']);                    // One and two
enLf.format(['one', 'two', 'three']);           // One, two, and three
enLf.format(['one', 'two', 'three', 'four']);   // One, two, and three


zfLf.format(['one']);                           // One
zfLf.format(['one', 'two']);                    // One和two
zfLf.format(['one', 'two', 'three']);           // One、two和three
zfLf.format(['one', 'two', 'three', 'four']);   // One、two、three和four
```

What this formatter does is basically joining an array of string with the correct conjunction or disjunction to create a meaningful phrase.

The default way to format is by using a conjunction unless you pass the `type` options as `disjuntion`

```js
const dlf = new Intl.ListFormat('en-US', { type: 'disjunction'})
dlf.format(['One'])                         // One
dlf.format(['One', 'two'])                  // One or two
dlf.format(['One', 'two', 'three'])         // One, two, or three
dlf.format(['One', 'two', 'three', 'four']) // One, two, three or four
```

As always, you can check the playground [example in this link.](https://jsitor.com/wTiyKccEh)

### Segmentation

Segmentation refers to the requirement of splitting some text in segments, for example, split a paragraph in words.
The *Intl.Segmenter* constructor offers a locale-sensitive solution to split the text returning meaningful items for the source string.

The most naive and most often used solution to split a string is just using `String.prototype.split`, for example splitting a text by whitespaces `.split(' ')`. But, what happen if the language does not use whitespace between words? The result of the split action will be wrong, to avoid this let's use the *Segmenter* constructor.

```js
const str = "吾輩は猫である。名前はたぬき。";
const segmenterJa = new Intl.Segmenter('ja-JP', { granularity: 'word' });

const segments = segmenterJa.segment(str);
console.table(Array.from(segments));
// [{segment: '吾輩', index: 0, input: '吾輩は猫である。名前はたぬき。', isWordLike: true},
// etc.
// ]
```
<small>Example extracted directly from [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Segmenter#basic_usage_and_difference_from_string.prototype.split)</small>


### String comparison

The last constructor to review is the *Intl.Collator*, this constructor enables string comparison with locale sensitivity.

This constructor is very useful to sort strings that contain extra letters like German or Swedish. Different languages have different sorting rules, let's check a quick example of this

```js
const letters = ['ä', 'z']
const listDE = new Intl.Collator('de')
const listSV = new Intl.Collator('sv')
// in German, ä sorts with a
console.log(letters.sort(listDE.compare));
// → a negative value

// in Swedish, ä sorts after z
console.log(letters.sort(listSV.compare));
// → a positive value
```
<small>Check the code [in the playground](https://jsitor.com/XXRuoOAF1)</small>


## Conclusion
Internationalization is an important piece of an application or website that is meant to be used by a worldwide audience, but getting it right is a complex topic. Luckly there are many building blocks directly available from the Javascript engine that can help you implement a locale sensible UI.


That’s a wrap!
If you have any questions or feedback, [open an issue on  my AMA repo](https://github.com/matiasfha/ama/issues/new/choose) or [ping me on Twitter.](https://twitter.com/matiasfha)
