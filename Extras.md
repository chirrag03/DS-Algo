## Extras


### To keep a track of visited cells in a 2D array:
```
Approach 1: Use a separate boolean 2D array to store if the cell is visited or not for each location  
Approach 2: Use a Set to store the id of only the visited cells in row major form i.e.  visited.add(xCoord*n + yCoord)
```

 ### HashSet:

```java
//Initialise a HashSet with some value
Set<Integer> visited = new HashSet<>(Arrays.asList(i*n + j));
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

//Concat a list of strings
stringArrayList.stream().collect(Collectors.joining(""))
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
