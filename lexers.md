[Home](index.md)

A Lexer is a system or program that converts a string into a list of tokens. 
 
Lexer: String -> List<Token> 
 
The easiest way to write a Lexer is to write it by hand. Most languages have a hand-written Lexer. Tools exist for generating Lexers, and regex libraries can help, but in practice the performance and error-handling benefits of a handwritten Lexer far outweigh the cost. 
 
# Lexing Json 
We can continue using Json as an example language. A simple Json lexer might look like this: 
 

```C#
define NextToken(): Token 
{ 
   c = LookAhead(0) 
   if(IsNumber(c)) 
      return LexNumber() 
   else if(IsQuote(c)) 
      return LexString() 
   else if(IsLetter(c)) 
      return LexKeyword() 
   else if(IsPunctuation(c)) 
      return LexPunctuation() 
   else 
      return LexWhitespace() 
} 
 
define LexJson(): List<Token> 
{ 
   result: List<Token> = [] 
   while(HasMore()) { 
      next = NextToken() 
      if(!IsWhitespace(next)) 
         result.Add(next) 
   } 
   return result 
} 
```

Lexing a keyword causes the lexer to consume all letters until it hits the first non-letter character. It then checks to see if the resulting word exists in a dictionary. If not, it throws an exception. 
 
Whitespace is typically ignored, either within the Lexer or filtered at a later stage. 
 
# Lexer Grammars 
Note that the Lexer looks similar to a recognizer in its structure. Indeed a Lexer does have its own grammar. 
 
The Lexer applies its grammar to concrete characters whereas the recognizer applies its grammar to abstract tokens. It is completely possible to combine the two, but in practice this is not done. 
 
The split between character grammars and higher-level token grammars has withstood the test of time. The reason why is explained in Chapter 3. Lexers are concerned with constructing a List while Parsers are concerned with constructing a Tree. 
 
# Look Ahead 
Note that the example Lexer above only looks ahead to the single character that is the next character in the buffer. In some rules the Lexer may need to look ahead multiple characters. For example, when matching the >= operator, the Lexer needs to combine the two characters into a single token. In general, a Lexer will always return the longest possible token, i.e. the Lexer is very greedy. 
 
Most languages are designed to minimize the amount of look ahead that is required. A language that has at most K lookahead (where K is a small integer) can be implemented by hand using very straightforward code. Languages that require a great deal of look ahead are uncommon, they have not withstood the test of time. 
 
# The Subset Construction 
In computer science school the topic of Lexical analysis will often include a description of the dreaded Subset Construction. It is used to convert a non-deterministic finite-state automata (NFA) into a deterministic finite-state automata (DFA). 
 
The subset construction and related techniques are only useful when implementing a regex library or a Lexer generator. They are not used in the industry to make compilers. 

[Home](index.md)