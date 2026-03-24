
## Brute Force:

**Encoding**: Encode the strings with a delimiter like | or a different character. 
**Decoding**: Loop through each character and keep on appending it to StringBuilder and if the character is the delimiter. Create a string of existing set of characters and populate it into the ans.

**Brute Solution**:

Since an existing testcase had an input string with | in it. Due to this we were getting wrong output and so we used **\`** as the delimiter.

```java
class Solution {
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for(String str: strs) {
            sb.append(str + "`");
        }
        return sb.toString();
    }

    public List<String> decode(String str) {
        List<String> ans = new ArrayList<>();
        StringBuilder sb = new StringBuilder();

        for(char ch: str.toCharArray()) {
            if(ch == '`') {
                ans.add(sb.toString());
                sb = new StringBuilder();
                continue;
            }

            sb.append(ch);
        }

        return ans;
    }
}
```
## Optimal

As mentioned above, having an delimiter as something that doesn't exist in the testcase is more of a loophole than a solution. So as to fix it with proper solution we can use encode the string with size based delimiter. 

**For Example**: 
str: ["Hello", "World"]
Encoded String: 5#Hello5#World

This helps us to know the exact word length and simultaneously just fetch that part of substring while decoding

**Solution**

```java
public class Codec {

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        StringBuilder encodedString = new StringBuilder();
        for (String s : strs) {
            // Append the length, a delimiter, and the string itself.
            encodedString.append(s.length()).append("#").append(s);
        }
        return encodedString.toString();
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        List<String> decodedStrs = new ArrayList<>();
        int i = 0;
        while (i < s.length()) {
            // Find the index of the next delimiter.
            int j = i;
            while (j < s.length() && s.charAt(j) != '#') {
                j++;
            }

            // Extract the length substring and convert to an integer.
            int length = Integer.parseInt(s.substring(i, j));
            // Move the start pointer past the length and the delimiter.
            i = j + 1;
            // Extract the original string using the calculated length.
            String str = s.substring(i, i + length);
            decodedStrs.add(str);
            // Move the start pointer to the beginning of the next length segment.
            i += length;
        }
        return decodedStrs;
    }
}

```