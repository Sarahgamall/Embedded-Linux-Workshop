# Libraries

Libraries in programming languages are collections of prewritten code, that delivers ready to use functions each with a specific purpose and functionality, Libraries typically are grouped together to be more reachable and increase their reusability.

In order to be able to use these libraries, the object file produced after compiling your source code have to be linked with the used libraries either statically or dynamically.

Without the linking stage any functions used from the libraries will not be resolved and will cause a linker error

  
  
## Static Library

Static libraries are present locally and their location is known before runtime , they are compiled and copied into the executable file to resolve dependencies

Static Linking creates larger binary files, and need more space on disk and main memory ,however it ensures exclusivity 

Examples of static libraries are, **.a** files in Linux and **.lib** files in Windows.

  

## Create static library 

1- Create A directory named StaticLibrary_Lab

    mkdir StaticLibrary_Lab

2- change directory into the previously created directory and make another two directories 

    cd StaticLibrary_Lab
    mkdir Include 
    mkdir Lib 
    

 3- change directory into the Include directory and create a header file as an example lets implment an addition function

     vim add.h
    press i to insert 
     insert : int add(int x , int y);
  press esc :wq to save and exit 
    

4- change directory into the Lib directory and create a source file that implments the add function

     cd ../Lib
     vim add.c 
    press i to insert
     insert : 
      #include "add.h"
     int add(int x , int y)
     {
       int result = x+y ;
       return result ;
      }
     press esc :wq to save and exit 

     

5- compile add.c to get the object file , libraries on their own dont have an entry point therefore cant be executed

    gcc -c add.c -o add.o

6- combine all libraries object files into one archive , archiving the object files will compress them in order to take up less space, in our case there's only one library

    ar -rcs libadd.a add.o

 7- change directory back to StaticLibrary_Lab and create your app source file

    cd ..
    vim main.c 
    press i to insert
    insert :
    #include "add.h"
         int main(void)
         {
    	        int  add_Result ;
           
          add_Result  = int add(5,2);
         
           return 0 ;
        }
         press esc :wq to save and exit 

8- compile your app source code to get main.o , make sure you include the path to your header file

    gcc –c main.c –I ./Include
 9- generate an executable file by linking together your main.o and the archive created previously

     gcc –o main.exe main.o lib/libadd.a
10- Run the executable main.exe

    ./main.exe

## Dynamic Library 
dynamic libraries are external or shared libraries that are unresolved during compile time , and the operating system specifically the system loader during runtime searches for the unresolved symbols and loads then in the RAM memory in order to be used by the executable file, only one copy of the library is present in the memory and many programs can use it
Examples of Dynamic libraries are, **_.so_** in Linux and **_.dll_** in Windows.
## Create dynamic library 

1- Create A directory named DynamicLibrary_Lab

    mkdir DynamicLibrary_Lab

2- change directory into the previously created directory and make another two directories 

    cd DynamicLibrary_Lab
    mkdir Include 
    mkdir Lib 
3- change directory into the Include directory and create a header file as an example lets implement an addition function

     vim add.h
    press i to insert 
     insert : int add(int x , int y);
  press esc :wq to save and exit 
    

4- change directory into the Lib directory and create a source file that implments the add function

     cd ../Lib
     vim add.c 
    press i to insert
     insert :
      #include "add.h"
      int add(int x , int y)
     {
       int result = x+y ;
       return result ;
      }
     press esc :wq to save and exit 

   5-  Generate the object file of the library add.c file

       gcc -c -g -Wall –fPIC add.c

 


   6- Combine all your library object files into one shared library 
  

     gcc -shared -o libadd.so add.o 
   7- we need to specify the path of the shared library to the operating system , there are three ways to do so 
   

 1. using **LD_LIBRARY_PATH**
   Upon setting the **LD_LIBRARY_PATH** variable, the System loader automatically search for the library in the specified path, overriding the defualt search paths which are /usr/lib, /lib32, /usr/lib32

>    `Export LD_LIBRARY_PATH= /DynamicLibrary_Lab/Lib`

 
 2. **System path**
 Previously mentioned the default search paths for shared library are /usr/lib, /lib32, /usr/lib32.
you can copy your shared library in the system path

>  `sudo cp ./lib*.so /usr/lib`

then change the execution mode of the shared library

> `sudo chmod 0755 /usr/lib/lib*.so`

   then update the linker configuration
 
>  `sudo ldconfig`

 3. **Passing rpath**
 delete LD_LIBRARY_PATH variable
> `unset LD_LIBRARY_PATH`


path r-path option during compilation(this is done after step 8 below instead of step 9)

    > gcc -L./Lib -Wl,-rpath=./Lib -Wall -o main.exe main.c -l libadd.so -I./include

  8- change directory back to DynamicLibrary_Lab and create your app source file

    cd ..
    vim main.c 
    press i to insert
    insert :
    #include "add.h"
         int main(void)
         {
    	        int  add_Result ;
           
          add_Result  = int add(5,2);
         
           return 0 ;
        }
         press esc :wq to save and exit 

 
9- Compile and generate the final executable file (if you are using the first two ways) 

     gcc main.c -L./lib -l libadd.so -o main.exe -I./include

10 - Run the Executable

    ./main.exe



