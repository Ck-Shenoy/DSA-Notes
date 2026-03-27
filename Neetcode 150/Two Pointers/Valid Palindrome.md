## Brute Force

As the question suggests, just consider valid characters and build a new string with just these valid characters. Now convert this to lowercase so that the case becomes insensitive. Have 2 variables str and rev, where one is the reversed string and the other is a straight string. Compare both if both are exactly same. 

```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder sb = new StringBuilder();
        for(char ch: s.toCharArray()){
            if(
                (ch >= 'A' && ch <= 'Z')
                || (ch >= 'a' && ch <= 'z')
                || (ch >= '0' && ch <= '9')
            ) {
                sb.append(ch);
            }
        }

        String str = sb.toString().toLowerCase(), 
        rev = sb.reverse().toString().toLowerCase();
        
        return str.equals(rev);
    }
}
```

## Best

Here we are assigning a reversed string to another variable, is it really necessary?
We can have 2 pointers, one that travel from 0 to stringLength/2 and another one that travels from stringLength to stringLength/2. Both move at the exact same pace, so if the values related to the pointers are different then we would return false immediately.

```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder sb = new StringBuilder();
        
        for(char ch: s.toCharArray()){
            if(
                (ch >= 'A' && ch <= 'Z')
                || (ch >= 'a' && ch <= 'z')
            ) {
                sb.append(Character.toLowerCase(ch));
            } else if (ch >= '0' && ch <= '9') {
                sb.append(ch);
            }
        }
  
        String str = sb.toString();
        int n = sb.length();
  
        // Core two-pointer logic
        for(int i=0; i<(n/2); i++) {
            if(str.charAt(i) != str.charAt(n-i-1)) {
                return false;
            }
        }
  
        return true;
    }
}
```

## Optimal

There isn't much things that we can optimize here. It's just the space that we are avoiding. Instead of using stringbuilder we are trying this same approach as part of the main string itself. Since we are avoiding space, we need to use actual 2 pointers low and high so as to skip indexes where the value isn't alpha numeric

```java
class Solution {
    private boolean isAlphaNum(char ch) {
        if(
            (ch >= 'A' && ch <= 'Z')
            || (ch >= 'a' && ch <= 'z')
            || (ch >= '0' && ch <= '9')
        ) {
            return true;
        }
        return false;
    }

    public boolean isPalindrome(String s) {
        int n = s.length(), low = 0, high = n-1;
        
        while(low < high) {
            // Skip indexes which aren't alphanumeric
            while(low < high && !isAlphaNum(s.charAt(low))) {
                low++;
            }

			// Skip indexes which aren't alphanumeric
            while(low < high && !isAlphaNum(s.charAt(high))) {
                high--;
            }

            char first = Character.toLowerCase(s.charAt(low)), last =  Character.toLowerCase(s.charAt(high));

            if(first != last) {
                return false;
            }

            low++;
            high--;
        }

        return true;
    }
}
```