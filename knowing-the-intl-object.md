---
keywords: 
- **Intl**
- **Intl** Javascript
- Internationalization
- Javascript Internationalization
- JS Internationalization
- i18n
- Javascript i18n
- JS i18n
- **Intl** Date formatting
- **Intl** currency
- Pluralization
summary: A medida tu aplicación crece es posible quieras mostrar tu contenido en diferentes formatos e incluso diferentes idiomas. Javascript ofrece una API orientada a este objetivo, el objeto **Intl**.
---
# Knowing the **Intl** Javascript API
Personalizing the user experience is becoming more important every day, even more so if your application or content is consumed by users in different parts of the world who most likely use different languages ​​or date formats, currency, etc.

There are multiple solutions to make your content adapt to the location or language of your users, but many of these methods have become outdated or are very complex or dependent on a particular framework.

Javascript also offers an internationalization solution, the **Intl** object.

## What is Internationalization?


The concept of internationalization or i18n is the process of supporting different languages, languages ​​and countries in your application.



i18n is commonly confused with Localization or even translation, but i18n refers to the process of developing a product, a process focused on supporting different languages ​​and formats based on locality. A common code base.

Providing internationalization support is critical for many products but often overlooked.

> Localization refers to the generation of a specific product to target a market or region, including translation of content and even modification of the interface and terminology.

Typically i18n implementation may include:

- Develop software that is independent of specific language or cultural conventions. (eg: display of times and dates).
- Use of localization frameworks.
- Removal of "hard-coded" text in the code.
- Support for bi-directional languages.
- Support for different number formats.

## The **Intl** object

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/**Intl**), _"The **Intl** object is the namespace for the ECMAScript Internationalization API, which provides language sensitive string comparison, number formatting, and date and time formatting. The **Intl** object provides access to several constructors as well as functionality common to the internationalization constructors and other language sensitive functions."_

In other words, through the global **Intl** object you can access a set of tools aimed at working with content that is sensitive to the user's language.

These methods are currently well supported by most browsers based on information available at [caniuse.com](https://caniuse.com/?search=Internationalization%20API)

![Can I Use: **Intl**](https://res.cloudinary.com/matiasfha/image/upload/v1662665832/Screen_Shot_2022-09-07_at_16.45.50_anek8r.png)

TLDR; A quick example of what this API can achieve

```js 
const value = 1234.39;
const today = new Date("2022-09-07");

/**
 * @param local un string que representa un codigo de localización
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

In this code you can briefly see some of the things that **Intl** can do.

- Gives you static access to different constructors, in this case `DateTimeFormat` and `NumberFormat`
- Allows you to "transform" content from one format to another based on the value of `locale`

It is important to note the `locale` argument received by all constructors exposed by **Intl**. This is the value that you will ideally capture dynamically so that you can modify the content based on its value.


Browsers offer a method of getting the value of `locale` based on the user's preferences or the user's location.

```js
const locale = navigator.language
console.log(locale); // "es-CL"
```

## What is `locale` and how does it work?

When we refer to `locale` we refer to a string that represents a group of user preferences like:

- Date and Time
- numbers and currency
- Time zones, languages ​​and countries
- Measurement units

This `locale` string must follow a particular format to be used, this consists of:

* A subtag or language code.
* (optional) a region or country subtag.
* (optional) a subtag script.
* (optional) one or more variation subtags.
* (optional) one or more [BCP-47](https://datatracker.ietf.org/doc/html/rfc4647#section-3.4) extension sequences.

![locale example](https://res.cloudinary.com/matiasfha/image/upload/v1662665973/Screen_Shot_2022-09-08_at_15.39.23_mvtmlf.png)

Each subtag or sequence used to create the `locale` string is separated by a hyphen. Also, identifiers identify case.

Some examples of `locale` strings
* "es": Spanish (language)
* "es-CL": Spanish (language) as it is used in Chile (region).
* "zh-Hans-CN": Simplified Chinese (language) (script) as used in China (region).


When a `locale` string is passed as an argument to one of **Intl**'s provided constructors it is compared to a list of available `locales` for the best fit. This process is done using one of two possible algorithms: `lookup` or `best-fit`.

The `lookup` algorithm checks if the runtime environment has the `locale` used by searching from the most specific to the least specific result. If the exact value of `locale` is not available, it will return the closest one, for example if you search for `de-DE-u-co-phonebk` if not found, you can return `de-DE` and in case of not finding `de` reference will return a default value.

The `best-fit` algorithm is an improvement of the previous algorithm where a default value is not returned if the search is not found, if not, the one that best "fits" is returned. IF `es-CL` is searched for but not found, `es-AR` will be returned instead of sjust `is`.

You can learn more about this process by [reviewing the documentation on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/**Intl**#locale_identification_and_negotiation).


#### What does each subtag of the string mean?

As described above, the string locale is made up of different parts separated by a hyphen, the first section being the language identifier, what are the rest used for?

##### Script code

This code is used to identify in what "format" a certain language is written, for example in Asian languages ​​`Hans` means that Simplified Chinese will be used vs `Hant` which indicates that Traditional Chinese will be used.

##### Variant code

These represent the different dialect options for a certain language.

##### Extensions

It includes identifiers for different calendars, numeric or ordering systems, in the example of the previous image `phonebk` identifies the variant indicating the ordering used for the letters, in this case phone book style.

## Time to code

### Formatting dates and time 

Let's start by formatting dates and times based on different values ​​of `locale`.

For this you need access to the `DateTimeFormat` constructor.
```js
const date = new Date(); //la fecha de hoy

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

That is, you have many options to define the best way to display a date and time in your application.

Let's review some examples

```js
// Utilizando las opciones

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

// Fecha en formato USA
const usaDate2 = new Intl.DateTimeFormat('en-US', options).format(date)
// Thursday, September 8, 22 at 10:20:54 AM

const clDate2 = new Intl.DateTimeFormat('es-CL', options).format(date)
// jueves, 8 de septiembre de 22, 10:20:54 a. m.

// Fecha en formato Aleman
const deDate2 = new Intl.DateTimeFormat('de', options).format(date)
//Donnerstag, 8. September 22 um 10:20:54 AM

// Fecha en formato Arabico egipto
const arDate2 = new Intl.DateTimeFormat('ar-eg', options).format(date)
// الخميس، ٨ سبتمبر ٢٢ في ١٠:٢٠:٥٤ ص
```

### Formatting Numbers

Another constructor offered by **Intl** is `NumberFormat`, you can use this constructor to change the way numbers are represented in the screen.

The only difference between `DateTimeFormat` and `NumberFormat` constructors is the set of options that this contructors receives.

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
Using this big list of options you can format a number to probably every possible use case.

Check this quick example

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

In this example you can see the following
- A function was created to hold the formatter.
- The formatter is always using the `en-US` locale.
- There are 3 options defining what style of number formatting will be using `style: currency`.

Even that we kept the same locale the values returned by the formating function are different.

> What would happen if you also change the locale for each currency conversion?

In case this options don't cover your use case you can use another method that is provided by the formatter: `formatToParts`. This method will return an array of objects that represent the number as string in parts so you can use them to customize and solve your particular situation.

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


What if you want to show a nunber like the social networks? like `1.2K`?

The `NumberFormat` can solve that in a very simple way.

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

In this case the solution involves the use of the `notation` option 

You can find a demo for the number formatting [in this playground.](https://jsitor.com/YX91hj0gX)

