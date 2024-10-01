Class: CS530 - Systems Programming (Spring 2024)
Assignment: Project #2 (SIC/XE Two-Pass Assembler)
Filename: README.txt (README file)


File manifest: Name and purpose of each file

- Makefile => Compiles and runs the asxe program
- asxe.cpp => The main source file of the program
- optab.cpp => A separate class made to implement an operation code table
- optab.h => The header file for optab.cpp
- P2sample.sic => The example test file
- testFile1.sic => My first own test file (Example in page 74 of the textbook)
- testFile2.sic => My second own test file (Figure 2.5 in page 55 of the textbook)
- testFile3.sic => My third own test file (Figure 2.9 in page 67 in the textbook)
- README.txt => (This file) Gives some information about the project

Compile instructions: In directory ~/a2 of cssc4004@edoras, type "make" command

Operating instructions: More instructions to run the program along with other functions of the Makefile should be output upon running the "make" command. Here are the instructions

- Input your sic source files with "asxe" preceeding them (Ex: "asxe example1.sic example2.sic"). After running the code
	- make open: print *.st *.I file(s)
	- make clean: remove asxe file
	- make remove: remove *.int file
	- make purge: remove asxe *.int *.st *.I file(s)

Novel/significant design decisions:

- I made a function called writeHex () to write any type of value (using template<typename T>) in string-hex representation and return it as a string value. In this function, I utilized string manipulators such as std::setw(), std::setfill(), std::right, and std::hex. And I designed the parameters to include arguments for these ios flags
- A few data structures I used: A hash table (std::unordered_map), a vector-map (std::vector<std::map>), a struct and a vector to contain each element comprised from it
- I used some string manipulation techniques to extract the name of the input file without the extension, concatenate other extensions to it, and then create write files with the concatenated string as the name of the file

Extra features/algorithms/functionality include that were not required:

- I made a bunch of outputs to display to make the program a lot easier for the user to follow after just inputting the make command
- I also included the contents of the source and intermediate file to the output from the program running
- Though the Makefile also lists various options of how to operate from there. Besides "make clean" which cleans the executable, "make open" outputs both the symbol table and listing file(s). There are also options to remove the intermediate file or even the other writing files
- I even figured out some algorithms in various make functions to state the actual name of each file being printed out

A few deficiencies:

- I did not make a header file for the asxe.cpp source file. Only have one for the optab class I made to include in the asxe program
- Symtab only includes R flags as opposed to the program determining whether the symbol location uses relative (R) or absolute/direct (A) addressing
- I did not implement program blocks (Section 2.3.4 of the textbook)
- In retrospect, I was not the most organized with my getComponents() method. I was creating this method while doing the pass 1 logic. So I only used this method to extract the prefix from the opcode (called "prefix 1") as opposed to the operand as well, given I did not need the operand prefix for anything in pass 1. However, given I used this method for both pass 1 and 2, I could have done so. Instead, I used a separate method to extract the prefix from the operand (called "prefix2" in the program)

What I learned from doing the assignment:

- To preface this section, I did use Chat GPT a lot. I did not copy and paste code. Though rather, it gave me a lot of good examples and explanations for all the implementations I needed for this assignment. Definitely a lot faster than looking through a C++ or Makefile manual, as far as acquiring knowledge goes. Although I do get that Chat GPT is not 100% perfect or reliable, hence why I still Google-searched some things to get full clarification
- In general, this assignment really bettered my understanding of a lot of even the more basic concepts of programming, at least with C++. Before this semester, I had barely any idea how to navigate the terminal or Edoras. Plus honestly, my general foundation with a good portion of lower-division Computer Science concepts is pretty rusty (most notably stuff like Object-oriented programming, or writing to files). Figuring out Assignment #1 was definitely tricky for those reasons. But doing so gave me a good foundation for carrying this huge assignment out. I am also happy to say that I think I have a way more solid grasp on input and output stream objects (whether it be files or strings) as well as various string manipulation functions or algorithms
- Besides this, I also have a very solid grasp on the two-pass assembler logic
- I also now have a more solid grasp on how Makefiles work and how to craft one than I did with Assignment #1
- Referencing a data variable in a parameter argument (i.e. "void firstPass(const str::string& fileName, Optab& optab)"). The referencing operator is in order to point to the original variable from its original memory location instead of creating a copy of it. I also generally developed a more solid understanding of pointers, especially with implementing various data structures (struct, map and unordered_map). Now for some more specific things I learned
- The difference between "if (!file)" and "if (!file.is_open())". Both can be used to check the state of the file or whether it is open or not. Only difference is that the former is "implicit" whereas the latter is "explicit" with the method. The former is more concise and simple. However, it relies on an implicit boolean conversion operator, which is inherited from the file stream class ("include <fstream>")
- C-style string vs. str::string and using c_str(). The one case I had to use c_str() was with the fileName string while using it in the ifstream command, to create a file stream object
- str.at(i) vs. str[0]. The former performs bound-checking whereas the latter does not
- Difference between scanning through each line and through each word
- std::istringstream and std::ifstream
- Utilizing composite/hierarchical data structures. In this program, I used a vector-map for SYMTAB. As it turns out, neither map or unordered_map (hash table) alone allow you to iterate through elements in the exact same order you inserted them. Even map, as I found out down the line, just orders string elements by their alphabetical values. However, vector keeps its elements in the same order that you input it, making you able to iterate through them in the same order. I will go into more detail about the decision to use a vector-map in the Software Document. But I learned multiple new functions and implementation methods that come from composite data structures. A most noteworthy example would be find_if and the concept of lambda functions
- Tab vs. setw(); had some problems with the output not aligning completely, as some strings are bigger than a tab. So not all lines should have an equal amount of tab characters. This is solved by the setw() method where I put a custom amount of space that should be between each word
- I also learned that getline concatenates its string parameter variable with a non-printable carriage return character ('^M'). While not seen, it can throw the standard output off if we implement the string having input written into, with anything else in the std::cout function. I learned this while I was trying to implement a boolean function that determines if the line being read is blank or not...only for it to keep saying even the blank lines are not blank. Though thankfully part of my program happens to output each line to a separate file. So I was able to check that file and figure out what was wrong before I got to the stage of wanting to pull my hair out. I also had a small feeling that implementing a string variable straight from getline would be a little 'trippy'
- I initially did both a hash table and a separate vector to address the format variable for each instruction. But then down the line, I realized I can just do a hash table with the value of the key-pair element being an int pair that contains two different int values, the opcode and the format number. And doing so far as far more efficient runtime complexity than iterating through a vector just to get the format number. Searching through a hash-table has a way faster best-case runtime complexity than with a vector, plus it is all being kept in just one variable now
- I also learned that you can actually type hex values in code, although they will be stored as the decimal value of it (for example, you can put 0x50 as a variable, although it gets stored as 80)
- Probably the three most useful VI editor shortcuts I used by far were [shift] + up/down (scroll through the file more efficiently), "dd" (delete line, or multiple if you type a number preceeding "dd") and [ctrl] + left/right (traverse each line more efficiently)
- Something else useful I learned is backing up my files to my host via the scp command (specifically using the -p flag). On a side note, I think it is more efficient than using Filezilla. I learned that Edoras VI is not always the easiest to navigate and can even be a little bit 'trippy' at times. I also accidentally inserted my source file as the executable argument during a g++ compile command at least two different times, which altered the file in a very less-than-favorable way. Though thankfully, I was able to back them up through the files I did a secure copy over to my host system. In addition, opening the files up on Visual Studio Code also helped as navigating the VSC IDE is more efficient than navigating the Edoras VI terminal
