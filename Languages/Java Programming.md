### **`import java.util.*;`** `<-- To import all libraries` like =="_bits/stdc++.h_"==

```
class Testing {
    public static void main(String[] args){
        System.out.println("Sukuna Here!!!");
    }
}
//Println vs Print vs Printf --> println gives new line and printf can print formatted string i.e., we can use format specifer inside brackets
```

- Here **Testing**(caps) is entry point to code if multiple class is present then multiple files will be created   
- **public** is access specifier
- **void** because it dose not return anything
- **"String[] args"** is a parameter

> Important thing to note in java is, when we use "=" operator the compiler (JAVAC) will assign same address space to new val and any change to previous val will be reflected onto new val
> 
> To copy one array to another without providing reference we can use -
> 
> ```
> int vec2[] = Arrays.copyOf(vec1, vec1.length);
> ```

How to take input

```
import java.util.Scanner;

class Testing {
    public static void main(String[] arguments){
        Scanner scanner=new Scanner(System.in); // Object for input
        String name=scanner.nextLine(); // For input
        scanner.close();
    }
}
```

// Used for explicit conversion of higher data type to lower data type (double -> int)

```
double x=5.8;
int y=(int)x;
System.out.println(y);
```

- _Operators are same as C++ (+ - * / % ++ -- Bit wise)_
- _Variables are also same but while using special containers use Integer not int. same for rest_
- _Relational operators are also same (> < = !)_
- _All loops are same_
- _Break and continue are also same_

2 Ways to declare String

```
String s1="abc"; // Will assign same address space if previous declared in memory (For optimisation)
String s2=new String("xyz"); // Will give new address space in memory 
```

Some methods of string

```
String s1=String.format("My name is %s and I currently live in %s",name,city);// Gives clean code and saves timeto
.isempty()
.length()
.toUpperCase()
.toLowerCase()
.replace("Blue","Red")
.contains("xyz")// returns true/false
```

```
String name=scanner.next(); //takes imput without spaces
String name=scanner.nextLine(); // its like getline
String name=scanner.nextInt(); // Takes int as an input but need .nextLine() afterwards to ignore <enter> keypress
String name=scanner.nextBoolean(); //Takes Boolean

Example --
Scanner sc=new Scanner(System.in);

int age=sc.nextInt();
scanner.nextLine();  //<----- insert this
System.out.println(age);

OR we directly can use
int age = Integer.parseInt(sc.nextLine());
Double age = Double.parseDouble(sc.nextLine());
```

Simple Calculator using if else and switch case

```
System.out.print("Enter First Number -> ");
        double val1=Double.parseDouble(scanner.nextLine());
        System.out.println(val1);
        System.out.print("Enter Second Number -> ");
        double val2=Double.parseDouble(scanner.nextLine());
        System.out.println(val2);

        System.out.println("Enter any opertion form given below \n+\n-\n*\n/\n%");
        String operation=scanner.nextLine();

        // switch(operation){
        //     case "+": 
        //         System.out.println(val1+val2);
        //         break;
        //     case "-": 
        //         System.out.println(val1-val2);
        //         break;
        //     case "*": 
        //         System.out.println(val1*val2);
        //         break;
        //     case "/": 
        //     if(val2==0){
        //         System.out.print("Divide by zero exception");
        //         break;
        //     }
        //     else{
        //         System.out.println(val1/val2);
        //         break;
        //     }
        //     case "%": 
        //         System.out.println(val1%val2);
        //         break;
        //     default:
        //         System.out.println("Chutiya hai kya Bhosdike!!!");
        // }

        // if(operation.equals("+")){
        //     System.out.println(val1+val2);
        // }
        // else if(operation.equals("-")){
        //     System.out.println(val1-val2);
        // }
        // else if(operation.equals("*")){
        //     System.out.println(val1*val2);
        // }
        // else if(operation.equals("/")){
        //     if(val2==0){
        //         System.out.print("Divide by zero exception");
        //     }
        //     else{
        //         System.out.println(val1/val2);
        //     }
        // }
           for(auto &[x,y]:mp)
        // else if(operation.equals("%")){
        //     System.out.println(val1%val2);
        // }
        // else{
        //     System.out.println("Chutiya hai kya Bhosdike!!!");
        // }
```

Arrays

```
        String vec1[]=new String[10]; //Declaration
        // Methods -----------------------------------
        Arrays.binarySearch() // Val,start_index,end_index
        Arrays.fill()
        Arrays.sort()
        Arrays.tostring() // Used to print whole array 
        Arrays.sort(array, Collections.reverseOrder()); // To use this "import java.util.Collections;"
        Collections.reverse(Arrays.asList(array));
        Arrays.equals(vec1,vec2) // returns true/false
```

ArrayList (Vector of java)

```
import java.util.ArrayList;
ArrayList <Integer> al1=new ArrayList<>();
al1.add();
al1.get();
myList.remove(1); //removes element at index
myList.remove("Item 2"); //removes first occurrence of element
myList.remove(Integer.valueOf(5)); 
myList.clear();
```

Algorithms in Java

```
Collections.sort(numbers);
Collections.sort(numbers, Collections.reverseOrder());
Collections.reverse(numbers);
Integer maxElement = Collections.max(numbers);
Integer minElement = Collections.min(numbers);
```

Containers in Java

```
ArrayList<Integer> list = new ArrayList<>(); // Vector            (add(),get(),set(),remove())
TreeSet<Integer> treeSet = new TreeSet<>(); // Sets               (add(),remove(),contains() <--returns boolean)
HashSet<String> set = new HashSet<>(); // Unordered set           (add(),remove(),contains() <--returns boolean)
TreeMap<String, Integer> treeMap = new TreeMap<>(); // Maps       (put(),remove(),contains())
HashMap<String, Integer> map = new HashMap<>(); // Unordered map  (put(),remove(),contains()) 
PriorityQueue<Integer> pq = new PriorityQueue<>(); //PQ           (offer() <-- add element, poll() <--pop element)

To iterate over HashMaps -
    hm1.forEach((key,value)->System.out.println(key+" "+value));
```