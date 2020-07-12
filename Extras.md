## Extras


### To keep a track of visited cells in a 2D array:
```
Approach 1: Use a separate boolean 2D array to store if the cell is visited or not for each location  
Approach 2: Use a Set to store the id of only the visited cells in row major form i.e.  visited.add(xCoord*n + yCoord)
```

### Lexicographical order:
```
Compare corresponding characters of the two strings from left to right.  
The first character where the two strings differ determines which string comes first.  
All uppercase letters come before lower case letters. If two letters are the same case, then alphabetic order is used to compare them.  
If two strings contain the same characters in the same positions, then the shortest string comes first.
```

### GCD of a and b :
```java
// Function to return gcd of a and b 
int gcd(int a, int b) { 
    if (a == 0) 
        return b; 
    return gcd(b % a, a); 
} 
```

 ### HashSet:

```java
//Initialise a HashSet with some value
Set<Integer> visited = new HashSet<>(Arrays.asList(i*n + j));
```  

 ### Union & Intersection of Sets:

```java
//Let say we have 2 set: Set<Integer> setA and Set<Integer> setB

Set<Integer> union = new HashSet<>(setA); 
union.addAll(setB);

Set<Integer> intersection = new HashSet<>(setA); 
intersection.retainAll(setB);
```  

 ### String:
```java
//Substring from index i to the end
s.substring(i)

//Substring from index i (inclusive) to j (exclusive)
s.substring(i, j)
```  


 ### Arrays:
```java
//Initialise at time of declaration
int[] arr = new int[]{10,1};

//Sort a primitive type array Let's say int[] arr = new int[]{10,1};
Arrays.sort(arr);
Arrays.sort(arr, Comparator.reverseOrder());

//Sort a object type array Let's say Item[] arr = new Item[20];
Arrays.sort(arr, (i1, i2) -> i1.freq - i2.freq));
```  

 ### ArrayList:
```java
//Initialise at time of declaration
List<String> stringArrayList = new ArrayList<>(Arrays.asList("hot","dot","dog","lot","log","cog"));

//Sort a list
Collections.sort(arr);
Collections.sort(arr, Comparator.reverseOrder());

//Conversion of primitive array to list of objects
int[] arr = new int[]{10,1};
List<String> stringArrayList = Arrays.stream(arr)
        .mapToObj(String::valueOf).collect(Collectors.toList());

//Conversion of primitive char array to list of objects
String s = "ddefdp";
List<Character> charArrayList = s.chars().mapToObj(c -> (char) c).collect(
            Collectors.toList());
        
//Concat a list of strings
stringArrayList.stream().collect(Collectors.joining(""))


//Convert arraylist to 2d array
List<int[]> outputIntervals = new ArrayList<>();
outputIntervals.toArray(new int[outputIntervals.size()][]);
```  


 ### ASCII:
```java
//Integer ASCII value to ASCII char
char a = (char) (97 + 5);   //97 is for a ....so if we need the fifth alphabetical char
```  


 ### Generate a Random number:
```java
Random rand = new Random();
int randomIndex = rand.nextInt(x); //Genrates a random number between 0 (inclusive) and x (exclusive)
```  
