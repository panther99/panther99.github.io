---
layout: post
title:  "Mental gymnastics with C# – Part I"
date:   2020-02-02 22:01:33 +0200
tags: csharp
categories: coding-challenges
---

What's the better way to spend a part of the beautiful day than to do some code challenges? Long ago I saw the [Daily Challenge series](https://dev.to/thepracticaldev/daily-challenge-1-string-peeler-4nep) on dev.to where it's staff would post a new problem each day. As I'm progressing with my transition towards .NET ecosystem, I thought this would be the great chance to polish my C# skills.

Instead of just throwing solutions for the challenges, I will go through the *mental flow* for each problem and explain why I wrote what I wrote. In my opinion this is the most important thing behind solving code challenges. It will hopefully be helpful to those who are also solving them. I will also write unit tests for each solution.

The code probably won't be the most idiomatic C# nor it would be the most performant (even though I will try my best). I truly recommend you to try to solve each challenge on your own first. There is no better way to push limits of your knowledge than to tackle with challenging problems on your own.

Without further ado, let's get started with the actual challenges, shall we? As you will see, the first part will pretty much be the warm-up.

## Challenge #1 - String Peeler

> Your goal is to create a function that removes the first and last letters of a string. Strings with two characters or less are considered invalid. You can choose to have your function return null or simply ignore.

There is no much going on here. If the text has less than three characters, we will return `null`. In case it doesn't, we will ensure that our text won't have surrounding white space characters with `String.Trim` and use `String.Substring` to leave only the characters we want.

```cs
using System;

public class StringPeeler
{
    public static string RemoveFirstAndLastLetter(string text)
    {
        string trimmedText = text.Trim();
        return trimmedText.Length < 3
            ? null
            : trimmedText.Substring(1, trimmedText.Length - 2);
    }
    
    public static void Main()
    {
        string sentence = "I am just playing my best move";
        Console.WriteLine(Program.RemoveFirstAndLastLetter(sentence));
    }
}
```

## Challenge #2 - String Diamond

> The shape that the print method will return should resemble a diamond. A number provided as input will represent the number of asterisks printed on the middle line. The line above and below will be centered and will have two less asterisks than the middle line. This reduction will continue for each line until a line with a single asterisk is printed at the top and bottom of the figure.
>
> Return `null` if input is an even number or a negative number.

When I stumble upon a problem which initially seems a bit trickier, I like to observe a sample output and see if I can find any patterns. Sometimes, this may be misleading because not all solutions follow the same patterns. But sometimes, it could be helpful, like you will see in this case.

Observing string diamonds, we can see that they contain only spaces and stars. What I tried to do first is to find some ratio between number of spaces and number of stars on the same line. As you can see for yourself, number of spaces is always even, and number of stars is always odd.

```
      *           8 spaces, 1 star
     ***          6 spaces, 3 stars
    *****         4 spaces, 5 stars
   *******        2 spaces, 7 stars
  *********       0 spaces, 9 stars
   *******        2 spaces, 7 stars
    *****         4 spaces, 5 stars
     ***          6 spaces, 3 stars
      *           8 spaces, 1 star

```

Take a look at numbers - you see how they repeat in a different order once you pass the horizontal center line of the diamond? This is very important because it will allow us to find how many spaces each line has.

```cs
for (int i = startingPoint; i >= -startingPoint; i--)
{
    int spacesOnTheLeftSide = Math.Abs(i);
    int spacesOnTheRightSide = Math.Abs(i);
}
```

As we can't just move towards zero and move back upwards without changing value of the iterator in the loop itself, we will move past zero and use `Math.Abs` to preserve positive numbers. It doesn't make much sense to know only the total number of spaces; we need number of spaces on left side and number of spaces on the right side of the line.

You probably wonder where did `startingPoint` suddenly came from. That's just a placeholder - we will calculate this value right now. In the example above, `startingPoint` will be four, because we have four spaces on the each side. Four is also a number of rows below and above horizontal center line. We will get this number by dividing width of the diamond (which in this case is nine).

```cs
for (int i = width / 2; i >= -(width / 2); i--)
{
    int spacesOnTheLeftSide = Math.Abs(i);
    int spacesOnTheRightSide = Math.Abs(i);
    int numberOfStars = width - Math.Abs(i * 2);
}
```

Great! Now we have all the numbers we need to actually draw the string diamond. But how are we going to do that without adding new `for` loops for drawing multiple spaces and stars? We will use the particular `System.String` constructor overload which will allow us to repeat a character multiple times.

```cs
for (int i = width / 2; i >= -(width / 2); i--)
{
    string spaces = new String(' ', Math.Abs(i));
    string stars = new String('*', width - Math.Abs(i * 2));
}
```

Now we're able to write the whole solution:

```cs
using System;

public class StringDiamond
{
    public static string DrawDiamond(int width)
    {
        if (width % 2 == 0 || width < 0)
        {
            return null;
        }

        string diamond = "";

        for (int i = width / 2; i >= -(width / 2); i--)
        {
            string spaces = new String(' ', Math.Abs(i));
            string stars = new String('*', width - Math.Abs(i * 2));
            diamond += spaces + stars + spaces + "\n";
        }

        return diamond;
    }
    
    public static void Main()
    {
        string asterixDiamond = StringDiamond.DrawDiamond(9);
        Console.Write(asterixDiamond);
    }
}
```

## Challenge #3 - Vowel Counter

> Write a function that returns the number (count) of vowels in a given string. Letters considered as vowels are: a, i, e, o, and u. The function should be able to take all types of characters as input, including lower case letters, upper case letters, symbols, and numbers.

The most obvious solution for this challenge would be to have a counter, loop through each character of the given string and increment the counter if character is a vowel. Thankfully, C# has a killer feature called [LINQ](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/) (Language Integrated Query), which will make solution even simpler. I'll present solution to you first and then we'll discuss what it actually does.

```cs
using System;
using System.Linq;

public class VowelCounter
{
    public static int CountVowels(string text)
    {
        char[] vowels = { 'a', 'e', 'i', 'o', 'u' };
        return text.ToLower().Where(c => vowels.Contains(c)).Count();
    }
    
    public static void Main()
    {
        int vowelCount = VowelCounter.CountVowels("I made my move. You make yours.");
        Console.WriteLine("Vowel count: {0}", vowelCount);
    }
}
```

First, we use `String.ToLower` to convert whole string to lowercase letters. After that, we filter out consonants by using `Enumerable.Where` method - we provide the array of vowels and check if it contains the current character. When filtering is finished, we use `Enumerable.Count` method to get the number of results.

## Challenge #4 - Checkbox Balancing

> You are given a small checkbook to balance that is given to you as a string. Sometimes, this checkbook will be cluttered by non-alphanumeric characters.
>
> The first line shows the original balance. Each other (not blank) line gives information: check number, category, and check amount.
>
> You need to clean the lines first, keeping only letters, digits, dots, and spaces. Next, return the report as a string. On each line of the report, you have to add the new balance. In the last two lines, return the total expenses and average expense. Round your results to two decimal places.
>
> **Example Checkbook**
>
> * 1000.00
> * 125 Market 125.45
> * 126 Hardware 34.95
> * 127 Video 7.45
> * 128 Book 14.32
> * 129 Gasoline 16.10
>
> **Example Solution**
>
> * Original_Balance: 1000.00
> * 125 Market 125.45 Balance 874.55
> * 126 Hardware 34.95 Balance 839.60
> * 127 Video 7.45 Balance 832.15
> * 128 Book 14.32 Balance 817.83
> * 129 Gasoline 16.10 Balance 801.73
> * Total expense 198.27
> * Average expense 39.65
>
> **Challenge Checkbook**
>
> * 1233.00
> * 125 Hardware;! 24.8?;
> * 123 Flowers 93.5
> * 127 Meat 120.90
> * 120 Picture 34.00
> * 124 Gasoline 11.00
> * 123 Photos;! 71.4?;
> * 122 Picture 93.5
> * 132 Tires;! 19.00,?;
> * 129 Stamps 13.6
> * 129 Fruits{} 17.6
> * 129 Market;! 128.00?;
> * 121 Gasoline;! 13.6?;

This was *somewhat* boring challenge because I spent more time thinking about structure of the code than the problem itself. Anyway, here is the list of things we need to do here:

* Clean the whole text, leaving only letters, numbers, spaces, dots and new lines.
* Take the line and try to parse it to `double`. It should succeed on the first try because the first line should have only the current balance.
* If the previous step didn't succeed, try to parse the line and take the check number, the category and the expense.
* Form a new line with the data we gathered and add it to the string we're gonna return at the end of the method and save the expense for later calculations.
* If there are no expenses it means our checkbox had no data (or at least not valid data).
* Calculate total and average expense and add them to the string which is returned at the end of the method.

Let's write a method for cleaning up the text first. We'll use regular expression to keep only allowed characters (I'm pretty sure there is a shorter way to write this).

```cs
public static string CleanUpCheckbox(string checkbox)
{
    var regex = new Regex(@"^[A-Za-z0-9. \n]$");
    return new String(checkbox.Where(c => regex.IsMatch(c.ToString())).ToArray());
}
```

Next, we will have to parse each checkbox line which has a check number, a category and an expense. For this purpose we will use [tuples](https://docs.microsoft.com/en-us/dotnet/csharp/tuples). We need to ensure that each line which violates this format won't break our program, so we'll wrap the parsing process in a `try...catch` block.

```cs
public static (int? CheckNumber, string Category, double? Expense) ParseCheckboxLine(string line)
{
    try
    {
        string[] fields = line.Split(' ');
        int checkNumber = int.Parse(fields[0]);
        string category = fields[1];
        double expense = double.Parse(fields[2]);

        return (CheckNumber: checkNumber, Category: category, Expense: expense);
    }
    catch
    {
        return (CheckNumber: null, Category: null, Expense: null);
    }
}
```

Finally, we need to ensure that the line we have parsed is valid:

```cs
public static bool ValidCheckboxLine((int? CheckNumber, string Category, double? Expense) tuple)
{
    var (CheckNumber, Category, Expense) = tuple;
    return new object[] { CheckNumber, Category, Expense }
                .Where(value => value == null).Count() == 0;
}
```

Now we have everything we need to write the solution. We will use `List<double>` to store expenses and `Enumerable.Sum` and `Enumerable.Average` to calculate the total expense and the average expense.

```cs
using System;
using System.Linq;
using System.Text.RegularExpressions;
using System.Collections.Generic;

public class CheckboxBalancer
{
    public static string CleanUpCheckbox(string checkbox)
    {
        var regex = new Regex(@"^[A-Za-z0-9. \n]$");
        return new String(checkbox.Where(c => regex.IsMatch(c.ToString())).ToArray());
    }

    public static (int? CheckNumber, string Category, double? Expense) ParseCheckboxLine(string line)
    {
        try
        {
            string[] fields = line.Split(' ');
            int checkNumber = int.Parse(fields[0]);
            string category = fields[1];
            double expense = double.Parse(fields[2]);

            return (CheckNumber: checkNumber, Category: category, Expense: expense);
        }
        catch
        {
            return (CheckNumber: null, Category: null, Expense: null);
        }
    }

    public static bool ValidCheckboxLine((int? CheckNumber, string Category, double? Expense) tuple)
    {
        var (CheckNumber, Category, Expense) = tuple;
        return new object[] { CheckNumber, Category, Expense }
                    .Where(value => value == null).Count() == 0;
    }

    public static string BalanceCheckbox(string checkbox)
    {
        double balance = 0;
        var expenses = new List<double>();
        string cleanedUpCheckbox = CheckboxBalancer.CleanUpCheckbox(checkbox);
        string balancedCheckbox = "";

        foreach (var line in cleanedUpCheckbox.Split('\n'))
        {
            try
            {
                balance = double.Parse(line);
                balancedCheckbox += string.Format("Current balance: {0:F2}\n", balance);
            }
            catch (FormatException)
            {
                var tuple = CheckboxBalancer.ParseCheckboxLine(line);
                var (CheckNumber, Category, Expense) = tuple;

                if (!CheckboxBalancer.ValidCheckboxLine(tuple))
                {
                    continue;
                }

                balance -= (double)Expense;
                expenses.Add((double)Expense);
                balancedCheckbox += string.Format(
                    "{0} {1} {2:F2} Balance {3:F2}\n",
                    CheckNumber, Category, Expense, balance
                );
            }
        }

        if (expenses.Count == 0)
        {
            return null;
        }

        balancedCheckbox += string.Format("Total expense: {0:F2}\n", expenses.Sum());
        balancedCheckbox += string.Format("Average expense: {0:F2}\n", expenses.Average());

        return balancedCheckbox;
    }
    
    public static void Main()
    {
        var checkbox = @"1233.00
125 Hardware;! 24.8?;
123 Flowers 93.5
127 Meat 120.90
120 Picture 34.00
124 Gasoline 11.00
123 Photos;! 71.4?;
122 Picture 93.5
132 Tires;! 19.00,?;
129 Stamps 13.6
129 Fruits{} 17.6
129 Market;! 128.00?;
121 Gasoline;! 13.6?;	
";

        Console.Write(CheckboxBalancer.BalanceCheckbox(checkbox));
    }
}
```

## Unit tests

Remember I have said that I will write unit tests for each solution? You can find them on [my GitHub repository](https://github.com/panther99/CodeChallenges), along with the source code for each solution. Feel free to ask me any question or give your suggestion on solution improvement.
