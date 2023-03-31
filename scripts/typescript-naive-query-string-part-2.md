Today's video will review the second part of building a naive query string parser using type-level programming in Typescript.

Remember to put your thumbs up and subscribe to keep up with this series and future content.

In the previous video, we create a simple query string extractor using conditional types, pattern matching, and type inference.
Scratching the surface of what combining that features can do to your types and code.

If you still need to watch the previous video, the link is in the description below.

After extracting the query string part from an URL string, the next step will be to transform that literal string 
into an object type like this one.

To fulfill that, first, we need to be able to extract the key-value pairs from it.


We already know, based on what we did in the previous video, that extracting information from some data in type-level programming means using pattern matching along with the `infer` keyword inside 
some conditional type, let's write that type here 

the name will be ExtractKeyValue, and since we want this to be reusable, it will accept a generic parameter named S
the condition will check if the S parameter is a string that matches this pattern of `string` & `string` and let's return true or false to complete the type 

We know that we want to extract information from this pattern, so let's change this `string` here to use the `infer` keyword and use the name
of Match and Rest, and let's return the Match part if the condition is true or the original S if now

Let's test it out by writing a new type that uses the utility type you created. 

Pass the query string type from here as the parameter. Remember that you can think of generics as functions in the type level, but remember, 
Typescript type-level is a pure functional language, so these "functions" cannot have side effects.

By testing it, you know that it works. It extracts the first key-value pair. Now how to get the Rest?

Here is where the syntax becomes tricky, but the way to do it's a known concept: Recursion. 

Since Typescript type-level doesn't include loops, you need to use recursion to loop over the data, in this case, to extract the key-value pairs from the `Rest` variable here.
let's apply the recursive utility type and check the result in our test type. 

Now you got a union of the key-value pairs, but there is a false. Why?

Since you're using recursion to loop over the data, you need to add some rules to stop the recursion. That rule returns false. Check this 

Let's review each part of this recursive loop,

in the first pass, the S type will hold the literal value of the entire query string, 
so the `Match` variable will have the first match found that is `query=123`
and `Rest` variable will have the Rest of the string literal.

Now, the second pass or use of the utility type is the recursive loop, where S will have the value of the previous Rest, that is user=2&test=some string 
Match will become the first key-value pair of that subset of the query string, that is `user=2` and now `Rest` will be test=some string 

There is a third pass, so now S is equal to the previous Rest, that is `test=some string`
The conditional type checks if S matches with the pattern, and since the new S literal value doesn't match, the condition returns false.

To fix this, let's return the original S now, the stop rule is when the generic S doesn't match with the pattern, and in that case, the type will "return."
the original generic.

Let's check the test here, and voila! all the key-value pairs are now extracted as a union type that you can trasform to achieve the goal of Creating
an object type from the query string.

In this second video, you review another powerful feature of Typescript: recursive types. This is "THE" way of looping over the data.

The following video will check how to transform that union into an object type and, at last, get the naive implementation of the query string parser at the type level.

Let me know in the comments if you use some of these features in your day to day basis, and since you're there, don't forget to like this video and subscribe to not miss the next 
video. 



