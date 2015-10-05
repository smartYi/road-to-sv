# Facebook

> 输入是两个字符串（第一个字符串是自定义的字母顺序比如zafdbeg，第二个字符是任意输入的字符串），输出是按照第一个字符串的规则排好序的字符串。

bucket sort解

```
public String outputAsOrder(String order, String input) {
     Map<Character, Integer> map = new HashMap<>();
    int[] arr = new int[input.length()];
        StringBuffer output = new StringBuffer();
        for(int i = 0; i < order.length() - 1; i++) {
            map.put(order.charAt(i), i);
        }
        for(int i = 0; i < input.length(); i++) {
            arr[i] = map.get(input.charAt(i));
    }
    Arrays.sort(arr);. 1point3acres.com/bbs
    for(int i = 0; i < arr.length; i++) {
            output.append(order.charAt(i));
    }
    return output.toString();
}
```

> 输入一串字符串比如 1 2 2 3 4 6 7，如果是nondecreasing 或者 nonincreasing 就返回true