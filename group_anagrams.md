# Group Anagrams

Write a method to sort an array of strings so that all the anagrams are next to each other

## Solution

```java
// Use the modification of the bucket sort
public void sort(String[] array){
    HashMapList<String, String> mapList = new HashMapList<String, String>();

    for (String s : array){
        String key = sortChars(s);
        mapList.put(key, s);
    }

    int index = 0;
    for (String key : mapList.keySet()){
        ArrayList<String> list = mapList.get(key);
        for (String t : list){
            array[index] = t;
            index++;
        }
    }

    String sortChars(String s){
        char[] content = sortChars.toCharArray();
        Arrays.sort(content);
        return new String(content);
    }
}
```
