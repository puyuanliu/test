# Description of the Word Puzzle Problem
This file contains the description of the word puzzle problem of the assignment2.

## Problem Description

Bob and Alice are competitive word-search puzzle solvers. However, even to the best of Bob's abilities, he was never able to find words in the puzzles faster
than Alice. As Bob's friend and amazing programmer, you have decided to help him out by creating a solver for word-search puzzles.
A word-search puzzle is solved when, given a grid of random letters and a set of words, the program finds the location of the input words in the grid (if they
exist). The orientation of the words can be horizontal, vertical, left diagonal, or right diagonal. The words can be read left to right, right to left, top to bottom
(whether vertically or diagonally), and bottom to up (whether vertically or diagonally). Note that a diagonal can start at any index, not necessarily on a central
diagonal.

You have decided to approach the problem using the Rabin-Karp algorithm. The Rabin-Karp algorithm uses what is called a hash function to find a substring of
size x in a string of size y >= x.

The hash function is applied as follows:

- Decide on a prime number p.
- For a substring of size x, the hash function multiplies the ASCII value of the first letter by p^(x-1), adds it to the ASCII value of the second letter multiplied
by p^(x-2), and so on until the ASCII code of the last letter is multiplied by p^(0). For example, hash("hello") = 80976 with p=5 . This is
calculated as

hash("hello") = h*(5^4) + e*(5^3) + l*(5^2) + l*(5^1) + o*(5^0) = 104*625 + 101*125 + 108*25 + 108*5 + 111*1 = 80976

The Rabin-Karp algorithm applies this hash function to all substrings of a specific size in a given string, storing the calculated hash value for each applicable 
index of the given string. The hash value is stored at the index corresponding to the start of the substring being hashed. The number of stored hashes depends
on the length of the given string and the length of the substring being searched for. If the algorithm cannot make a hash value with the specified length of the
word, it counts those positions as 0 . For example, assume the algorithm is searching for the word oworl in the string helloworld . Since the input
word is of length 5, it will calculate the hash values of all substrings of length 5. In this case, it will calculate 10 hash values: the hash value of the word
hello at index 0, the hash value for the word ellow at index 1, the hash value for the word llowo at index 2, the hash value for the word lowor
at index 3, the hash value of the word oworl at index 4, the hash value of the word world at index 5, and the value 0 in indices 6-9. While searching for
a word, if any of the values match the hash value of the given word, the algorithm has found the position of the start of the word.

The nice thing about Rabin-Karp is that it constructs consecutive hashes in O(1) time. This is done by taking the previous hash value, subtracting from it the
hash value of the first letter of the previous hash, multiplying by the prime p, and adding the value of the new letter. For example, the hash value for the word
hello with a prime p=5 is 80976. To get the hash value for the next substring of length x (x=5 in this case) ellow , we can simply remove the ASCII
value for h (104) multiplied by p^(x-1), then multiply by p, then add the ASCII value of w (119). So the hash( ellow ) = ( 80976 - 104 * 5^(4) ) * 5 + 119 =
79999

For the purposes of this assignment, you must fix p to 101.

## Task

Write a program that uses the Rabin-Karp algorithm to find the location of individual words in a given word-search grid and outputs their starting position and
direction, if they exist.

Enumerate the directions as follows:

 1. **horizontal right (→)**
 2. **horizontal left (←)**
 3. **vertical down (↓)**
 4. **vertical up (↑)**
 5. **top left to bottom right diagonal (↘)**
 6. **bottom right backwards to top left diagonal (↖)**
 7. **bottom left to top right (↗)**
 8. **top right backwards to bottom left (↙)**


## How to run your program
 ./wordSearch2D -p <puzzle_file> -l <word_length> -w <wordlist_file> [-o <solution_file>]

where:

- puzzle_file is a required argument. It is preceded by the -p flag. It is a text input file that contains the puzzle grid. The provided puzzle file will
have n lines of single strings of length n, providing an n by n square grid of letters. Each line in the file represents a row, and each line will be of same size
n. The positions go from 0 to n - 1 on each axis, with indices read from left to right and top to bottom. Note that 0 <= n <= 100. The puzzle file can contain
any ASCII character with values [32, 126], not necessarily English letters only.

- word_length is a required argument. It is preceded by the -l flag. It is a number in the range [1, n-1] that represents the fixed length of the words
the program will search for.

- solution_file is an optional argument. It is the name of the text output file to which your program will write its solution. Note that the
solution_file may not necessarily exist before your program runs. If this argument is not provided, the output should be written to a file
called output.txt . For each word in the input wordlist_file , a corresponding line will be outputted in the solution_file with the
following format word;(y,x);d , where word is the input word, (y,x) are the coordinates of the first letter of the word in the puzzle (y is the row index 
and x is the column index) and d is the direction of the word according to the enumerated list above. If the input word is not found in the puzzle, the output
would be word;(0,0);0 . Note that the top left corner of the puzzle corresponds to coordinate (0,0) , but since the direction is 0, this indicates that
the word is not found. Note that you must output the solution of each input word in the same order they were provided in the wordlist_file .

You must account for any edge cases for command-line arguments. We may run with too many arguments, not enough arguments, invalid arguments, etc.
Also, note that, similar to assignment 1, the order of the arguments is not fixed. For example,
./wordSearch2D -w wordList.txt -p puzzleFile.txt -l 20 is valid.


## Error Checking

Your program must report and print an error to stderr and quit in the following cases. Note the return code that needs to be returned in each case. The
return code is the integer that your program returns upon exit (e.g., return 0; in main or exit(2) from anywhere in the program). The error
message should be the exact error message provided:

- If the input puzzle_file is not found (angle brackets are placeholders here, replace with appropriate file name)
  - Error Message: "Error: Puzzle file <Name of input file> does not exist"
  - Return Code 4

- If the input wordlist_file is not found (angle brackets are placeholders here, replace with appropriate file name):
  - Error Message: "Error: Wordlist file <Name of input file> does not exist"
  - Return Code 5

- For any other incorrect program usage (e.g., forgetting a mandatory argument), your program should report (angle brackets need to appear as is here
since they indicate the placeholders needed for correct usage):
   - Error Message: "Usage: ./wordSearch2D -p <puzzle_file> -l <word_length> -w <wordlist_file> [-o <solution_file>]"
   - Return Code: 6

- Unless explicitly stated in the assignment description or in the assumptions below, there are no simplifying assumptions in this assignment. You should
handle all corner cases you can think of. If there is a case of incorrect input that prevents the program from proceeding correctly, your program should
report:
   - Error message: "Encountered error"
   - Return code: 7

## Assumptions

- Only characters with ASCII values between 32 and 126 will appear in the puzzle and word files
- a is not the same as A . This is already the case, given their ASCII codes
- The input word file can have any number of words, but each line will contain a single word. Each word will have a maximum size of 100 characters
- A word exists at most once in the puzzle

## Example with Expected Output

Here is a full example.
Given the following 5*5 puzzle file, puzzle.txt :

ahelw
hello
gblfk
cikml
jdgod

and the following word list, wordlist.txt :

wok
dog
bat
elk

then running the program with ./wordSearch2D -l 3 -w wordlist.txt -p puzzle.txt -o soln.txt would write the following to a file called
soln.txt:

wok;(0,4);3
dog;(4,4);2
bat;(0,0);0
elk;(0,2);5

Note that the directions correspond to the enumerated list provided above.

## Program Organization
Your program must have the following files

- Only your main function and any helper methods for reading arguments should be in a file called wordSearch2D.c
- All functionality related to solving the word puzzle should be in file puzzle2D.c and its corresponding header file puzzle2D.h . You must divide the
logic of solving the word puzzle into multiple functions.
- Feel free to add additional files and functions to further divide the functionality in your program.
- You must not use any global variables! You can define macros for predefined constants such as the maximum puzzle size or the prime number, but
you cannot use any global variables.

## Submission Guidelines

- You must use gcc -Wall -std=c99 ... to compile your program
- You must create a Makefile to compile and link the files in your program. Your Makefile must also contain the clean target. Your program must compile
into an executable called wordSearch2D . If your TA cannot run ./wordSearch2D after running make , your assignment is considered
incomplete as we cannot test your program. You will get a 0 for the assignment.

- Create a README text file that lists your references (e.g., any online resources you used) and any students you have talked to. Please note that
assignments should be strictly solved on an individual basis. Coding side by side and looking at each other's screens is not allowed. Exchanging
written code is not allowed. Someone writing out the solution for you on a piece of paper is not allowed. You get the idea. The only thing you are allowed
to do is to verbally talk to each other about ideas you might be stuck with. See collaboration policy on eClass. Please take this seriously as plagariasm
cases will be reported to the Faculty of Science.

- Submit 1 test case in the following form:
 - puzzle.txt contains a given puzzle
 - wordlist.txt contains a word list to find in the given puzzle
 - expected_soln.txt contains the solution to the given word list and puzzle
 - sample_test.sh is an executable shell script that runs your test case using the appropriate parameters. DO NOT SPECIFY AN OUTPUT FILE.
We will run your test case as ./sample_test.sh and compare the produced output.txt file to your provided expected_soln.txt .

- Your Makefile and all your code files must contain commented text at the top of your file that specifies your information, as indicated in the general
assignment instructions on eClass. You must also have the License information as a comment on the top of all your code files (i.e., .h and .c files).

- Summary of files that need to be on Github for submission:
 - wordSearch2D.c
 - puzzle2D.c
 - puzzle2D.h
 - Makefile
 - README
 - Your test case files
  - puzzle.txt
  - wordlist.txt
  - expected_soln.txt
  - sample_test.sh

- Any additional .h or .c files that are necessary to compile your code

## Important Notes
- You must be able to handle any puzzle size upto 100 * 100
- You must implement and use the Rabin-Karp algorithm as described above. Your program must run sufficiently quickly, even when tested with large
puzzles. If your program takes more than 0.3 seconds to solve a given puzzle on the lab machines, you will fail that test case. To test how long your
program takes to run, use the time command (e.g., time ./wordSearch2D ... ) and look at the Real value displayed.
- Your program must compile without any warnings or errors (using gcc -Wall -std=c99 ). If compiling your program results in errors or warnings, you
will receive a 0 for the assignment.
- Any missing files will result in 0 in the assignment. If you forget the ReadMe, Makefile, one of the source files etc., you will get a 0. Please make use of
the scripts we provide you to check the format of your submission.
- Please follow the instructions carefully. Any extra spaces you put after any word will result in a failed diff between the expected output and your solution.
Your GitHub repo already has sample test cases and a script to show you how we will compare the program output to the expected output. PLEASE USE
THIS SCRIPT! It will tell you if your output differs from what we expect. The continuous integration status will also let you know if there is anything wrong.
Please do not push every commit you make right away because this overloads the Travis CI build server. We advise you to commit locally as you work,
then test locally using the LocalTestScripts . Once you verify that things work, then push your commits. In general, try to push at least once a day
but not every 10 minutes.

## Grading
Total: 50 marks

- README file contains necessary information according to the general assignment instructions posted on eClass (1 mark) 
- running make compiles files into an executable called wordSearch2D without warnings or errors (1 mark) -- 0 for whole assignment if this step does
not work
- make clean should delete all object and executable files that are generated in the compilation process (1 mark)
- Proper structuring of your code into puzzle2D.h , puzzle2D.c , and wordSearch2D.c (3 marks)
- Solution is implemented using the Rabin Karp algorithm described (7 marks)
- Pass your own test case. Obviously, if your test case is missing or has the incorrect format, you will not get the mark (2 marks)
- Argument checking. Ensure you cover all cases for command line parameters (10 marks)
- Passing of word-search test cases (23 marks)
- Proper code formatting and commenting where necessary (2 marks)
- 10 marks will be deducted from your grade if global variables are used

Credits: This assignment has been developed with the help of Imtihan Ahmed

