# Coding Questions and Solutions in Java

### 1. Given an array of integers where every element appears even number of times except one element which appears odd number of times, write a program to find that odd occurring element in O(log n) time. The equal elements must appear in pairs in the array but there cannot be more than two consecutive occurrences of an element

#### Sample Input

> 5

> 2 2 3 1 1

#### Sample Output

> 3

#### Code Solution

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i = 0; i < n; i++){
            arr[i] = sc.nextInt();
        }

        int result = findOdd(arr);

        System.out.println(result);
        sc.close();
    }

    private static int findOdd(int[] arr){
        int left = 0, right = arr.length-1;

        while(left <= right){
            if(left == right) return arr[left];

            int mid = left + (right - left) / 2;

            if(mid % 2 == 0){
                if(arr[mid] == arr[mid+1]) left = mid + 2;
                else right = mid;
            } else {
                if(arr[mid] == arr[mid-1]) left = mid + 1;
                else right = mid - 1;
            }
        }

        return -1;
    }
}
```

---

### 2. Given an array of integers and a sum, the task is to count all subsets of given array with sum equal to given sum.

#### Input :

> The first line of input contains an integer T denoting the number of test cases. Then T test cases follow. Each test case contains an integer n denoting the size of the array. The next line contains n space separated integers forming the array. The last line contains the sum.

#### Output :

> Count all the subsets of given array with sum equal to given sum.

_NOTE_: Since result can be very large, print the value modulo 109+7.

#### Constraints :

```
1<=T<=100

1<=n<=103

1<=a[i]<=103

1<=sum<=103
```

#### Input :

```
2

6

2 3 5 6 8 10

10

5

1 2 3 4 5

10
```

Output :

```
3

3
```

#### Code Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        int mod = (int)1e9+7;

        while(t-- > 0){
            int n = sc.nextInt();
            int[] arr = new int[n];
            for(int i = 0; i < n; i++) arr[i] = sc.nextInt();
            int sum = sc.nextInt();

            int[] dp = new  int[sum + 1];
            dp[0] = 1;

            for(int i = 0; i < n; i++){
                for(int j = sum; j >= arr[i]; j--){
                    dp[j] = (dp[j] + dp[j - arr[i]]) % mod;
                }
            }

            System.out.println(dp[sum]);
        }
        sc.close();
    }
}
```

---

### 3. Before the outbreak of corona virus to the world, a meeting happened in a room in Wuhan. A person who attended that meeting had COVID-19 and no one in the room knew about it! So everyone started shaking hands with everyone else in the room as a gesture of respect and after meeting unfortunately every one got infected! Given the fact that any two persons shake hand exactly once, Can you tell the total count of handshakes happened in that meeting?

#### Input Format :

> The first line contains the number of test cases T, T lines follow.  
> Each line then contains an integer N, the total number of people attended that meeting.

#### Output Format :

> Print the number of handshakes for each test-case in a new line.

#### Constraints :

```
1 <= T <= 1000

0 < N < 106
```

#### Sample Input :

> 2

> 1

> 2

Output :

> 0

> 1

#### Code Solution :

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while(t-- > 0){
            int n = sc.nextInt();

            int res = n * (n - 1) / 2;

            System.out.println(res);
        }
    }
}
```

---

### 4. For enhancing the book reading, the school distributed story books to students as part of the Children’s Day celebrations. To increase the reading habit, the class teacher decided to exchange the books every week so that everyone will have a different book to read. She wants to know how many possible exchanges are possible. Find the number of possible exchanges, if the books are exchanged so that every student will receive a different book.

#### Constraints

> 1<= N <= 1000000

#### Input Format

> Input contains one line with N, indicates the number of books and number of students.

#### Output Format

> Output the answer modulo 100000007.  
> Refer the sample output for formatting

#### Sample Input :

> 4

#### Sample Output :

> 9

#### Code Solution :

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        if(n <= 2) {
            System.out.println(n - 1);
            return;
        }

        int mod = (int)1e8 + 7;

        long prev1 = 1, prev2 = 0, cur = 0;

        for(int i = 3; i <= n; i++){
            cur = ((i-1) * (prev1 + prev2)) % mod;
            prev2 = prev1;
            prev1 = cur;
        }

        System.out.println(cur);
        sc.close();
    }
}
```

---

### 5. A cybersecurity firm is developing a new encryption algorithm. To ensure maximum security, the system only accepts valid "Secure Keys." A Secure Key is defined strictly as a number that is greater than 1 and has no positive divisors other than 1 and itself. Given an integer $N$ representing a submitted key, write a program to determine if the key is valid.

#### **Input Format:**

> A single integer $N$ denoting the submitted key.

#### **Output Format:**

> Print `"Prime"` if the key is valid.  
> Print `"Not Prime"` if the key is invalid.

#### **Constraints:**

> $1 \le N \le 10^9$

#### **Sample Input 1:**

`29`

#### **Sample Output 1:**

`Prime`

#### **Sample Input 2:**

`35`

#### **Sample Output 2:**

`Not Prime`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        boolean prime = true;

        if(n <= 1) {
            System.out.println("Not Prime");
            return;
        }

        for(int i = 2; i * i <= n; i++){
            if(n % i == 0) {
                prime = false;
                break;
            }
        }

        System.out.println(prime ? "Prime" : "Not Prime");
    }
}
```

---

### 6. A network administrator is assigning secure communication ports to servers. A port is considered secure only if its port number is a prime number. Given a maximum port limit $N$, write a program to generate and print all secure port numbers from $1$ up to $N$ (inclusive).

#### **Input Format:**

> A single integer $N$ denoting the maximum port limit.

#### **Output Format:**

> Space-separated integers representing all the prime numbers up to $N$.

#### **Constraints:**

> $1 \le N \le 10^6$

#### **Sample Input 1:**

`20`

#### **Sample Output 1:**

`2 3 5 7 11 13 17 19`

#### **Sample Input 2:**

`10`

#### **Sample Output 2:**

`2 3 5 7`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        if(n <= 1){
            System.out.println("[]");
            return;
        } else {
            boolean[] isPrime = new boolean[n+1];
            Arrays.fill(isPrime, true);
            isPrime[0] = isPrime[1] = false;

            for(int i = 2; i * i <= n; i++){
                if(isPrime[i]){
                    for(int j = i * i; j <= n; j += i) isPrime[j] = false;
                }
            }

            System.out.print("[");
            for(int i = 2; i <= n; i++){
                if(isPrime[i]){
                    System.out.print(i == 2 ? i : " "+i);
                }
            }
            System.out.print("]");
        }
        System.out.println();
        sc.close();
    }
}
```

---

### 7. A logistics company needs to calculate the total number of unique routes a delivery truck can take to visit $N$ cities exactly once. The total number of unique permutations is mathematically represented as $N!$ (N factorial). Given an integer $N$, write a program to compute $N!$.

#### **Input Format:**

> A single integer $N$ denoting the number of cities.

#### **Output Format:**

> A single integer representing the total number of unique routes ($N!$).

#### **Constraints:**

> $0 \le N \le 20$

#### **Sample Input 1:**

`5`

#### **Sample Output 1:**

`120`

#### **Sample Input 2:**

`0`

#### **Sample Output 2:**

`1`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        long res = 1;

        for(int i = 2; i <= n; i++) res = res * i;

        System.out.println(res);

        sc.close();
    }
}
```

---

### 8. A financial analytics software is modeling stock market trends using a famous mathematical sequence. In this sequence, every new trend value is exactly the sum of the two previous trend values. The sequence always starts with $0$ and $1$. Given an integer $N$, write a program to generate the first $N$ terms of this trend sequence.

#### **Input Format:**

> A single integer $N$ denoting the number of terms to generate.

#### **Output Format:**

> Space-separated integers representing the first $N$ terms of the sequence.

#### **Constraints:**

> $1 \le N \le 50$

#### **Sample Input 1:**

`5`

#### **Sample Output 1:**

`0 1 1 2 3`

#### **Sample Input 2:**

`1`

#### **Sample Output 2:**

`0`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        if(n >= 1) System.out.print("0");
        if(n >= 2) System.out.print(" " + 1);

        long a = 0, b = 1;

        for(int i = 2; i < n; i++){
            long next = a + b;
            System.out.print(" " + next);
            a = b;
            b = next;
        }

        System.out.println();
        sc.close();
    }
}
```

---

### 9. A banking application generates a unique numerical transaction ID for every payment. For security audits, the system needs to process these IDs backwards to verify the checksum. Given an integer $N$ representing a transaction ID, write a program to mathematically reverse the digits of the number.

#### **Input Format:**

> A single integer $N$ denoting the transaction ID.

#### **Output Format:**

> A single integer representing the reversed transaction ID. (Note: Leading zeros in the reversed number should be ignored, e.g., reversing 100 becomes 1).

#### **Constraints:**

> $1 \le N \le 10^8$

#### **Sample Input 1:**

`12345`

#### **Sample Output 1:**

`54321`

#### **Sample Input 2:**

`1200`

#### **Sample Output 2:**

`21`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int res = 0;

        while(n > 0){
            res = res * 10 + n % 10;
            n /= 10;
        }

        System.out.println(res);
        sc.close();
    }
}
```

---

### 10. A data analytics company is filtering through massive streams of sensor data. They need to identify specific "symmetric" data packets to calibrate their equipment. A packet is symmetric if its numerical ID reads the exact same forwards and backwards. Given an integer $N$ representing a data packet ID, write a program to determine if the packet is a Palindrome.

#### **Input Format:**

> A single integer $N$ denoting the data packet ID.

#### **Output Format:**

> Print `"Palindrome"` if the ID is symmetric.
> Print `"Not Palindrome"` if the ID is not symmetric.

#### **Constraints:**

> $0 \le N \le 10^8$

#### **Sample Input 1:**

`12321`

#### **Sample Output 1:**

`Palindrome`

#### **Sample Input 2:**

`1045`

#### **Sample Output 2:**

`Not Palindrome`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int res = 0, copy = n;

        while(n > 0){
            res = res * 10 + n % 10;
            n /= 10;
        }

        System.out.println(res == copy ? "Palindrome" : "Not Palindrome");
        sc.close();
    }
}
```

---

### 11. A high-security banking vault uses a mathematical lock to verify administrative access. The system only grants access if the entered passcode is an "Armstrong Code." A number is considered an Armstrong Code if the sum of its own digits, each raised to the power of the total number of digits, equals the original number. Given an integer N, write a program to determine if the vault should open.

#### **Input Format:**

> A single integer N denoting the submitted passcode.

#### **Output Format:**

> Print `"Armstrong"` if the code is valid.
> Print `"Not Armstrong"` if the code is invalid.

#### **Constraints:**

> 1 <= N <= 10^8

#### **Sample Input 1:**

`153`

#### **Sample Output 1:**

`Armstrong`

#### **Explanation 1:**

`153 has 3 digits. 1^3 + 5^3 + 3^3 = 1 + 125 + 27 = 153.`

#### **Sample Input 2:**

`1634`

#### **Sample Output 2:**

`Armstrong`

#### **Explanation 2:**

`1634 has 4 digits. 1^4 + 6^4 + 3^4 + 4^4 = 1 + 1296 + 81 + 256 = 1634.`

#### **Sample Input 3:**

`120`

#### **Sample Output 3:**

`Not Armstrong`

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        // int count = Math.log10(n) + 1;

        int counts = 0, temp = n;
        while(temp > 0){
            counts++;
            temp /= 10;
        }

        temp = n;
        int sum = 0;
        while(temp > 0){
            sum += Math.pow(temp % 10, counts);
            temp /= 10;
        }

        System.out.println(sum == n ? "Armstrong" : "Not Armstrong");
        sc.close();
    }
}
```

---

### 12. A hardware manufacturing company is programming an LED matrix to display a staircase warning sign. The sign must be shaped like a right-angled triangle. Given an integer $N$ representing the height of the staircase, write a program to generate this star pattern.

#### **Input Format:**

> A single integer $N$ denoting the height of the triangle.

#### **Output Format:**

> A pattern of stars `*` separated by a single space, forming a right-angled triangle of height $N$.

#### **Constraints:**

> $1 \le N \le 100$

#### **Sample Input 1:**

`4`

#### **Sample Output 1:**

`* `
`* * `
`* * * `
`* * * * `

#### **Sample Input 2:**

`2`

#### **Sample Output 2:**

`* `
`* * `

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        for(int i = 0; i < n; i++){
            for(int j = 0; j <= i; j++){
                System.out.print("* ");
            }
            System.out.println();
        }

        sc.close();
    }
}
```

---

### 13. A network router assigns sequence numbers to fragmented data packets. The packets are transmitted in increasing batches, where each batch starts its sequence from 1 up to the current batch number. Given an integer $N$ representing the total number of batches, write a program to generate this transmission sequence pattern.

#### **Input Format:**

> A single integer $N$ denoting the number of batches.

#### **Output Format:**

> A right-angled triangle pattern of numbers, where each row $i$ prints numbers from $1$ to $i$, separated by spaces.

#### **Constraints:**

> $1 \le N \le 100$

#### **Sample Input 1:**

`4`

#### **Sample Output 1:**

`1 `
`1 2 `
`1 2 3 `
`1 2 3 4 `

#### **Sample Input 2:**

`3`

#### **Sample Output 2:**

`1 `
`1 2 `
`1 2 3 `

#### Coding Solution:

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        for(int i = 0; i < n; i++){
            for(int j = 0; j <= i; j++){
                System.out.print(j+1 + " ");
            }
            System.out.println();
        }

        sc.close();
    }
}
```

---

### 14. An architectural firm is designing a digital blueprint for a new monument. The design requires a perfectly centered, symmetrical pyramid made of structural nodes. Given an integer $N$ representing the total number of levels, write a program to generate this pyramid pattern.

#### **Input Format:**

> A single integer $N$ denoting the number of levels in the pyramid.

#### **Output Format:**

> A perfectly centered pyramid made of `*` characters.

#### **Constraints:**

> $1 \le N \le 100$

#### **Sample Input 1:**

`4`

#### **Sample Output 1:**

`   *`
`  ***`
` *****`
`*******`

#### **Sample Input 2:**

`3`

#### **Sample Output 2:**

`  *`
` ***`
`*****`

#### Coding Solution:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n - i - 1; j++) System.out.print(" ");
            for (int j = 0; j <= 2 * i; j++) {
                System.out.print("*");
            }
            System.out.println();
        }

        sc.close();
    }
}
```

---

### 15. A data engineering team is building a visual interface for a data funnel. As data is processed and filtered, the volume decreases, creating an inverted pyramid shape. Given an integer $N$ representing the initial number of data layers, write a program to generate this inverted funnel pattern.

#### **Input Format:**

> A single integer $N$ denoting the number of layers in the funnel.

#### **Output Format:**

> An inverted, perfectly centered pyramid made of `*` characters.

#### **Constraints:**

> $1 \le N \le 100$

#### **Sample Input 1:**

`4`

#### **Sample Output 1:**

`*******`
` *****`
`  ***`
`   *`

#### **Sample Input 2:**

`3`

#### **Sample Output 2:**

`*****`
` ***`
`  *`

#### Coding Solution:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        for (int i = n-1; i >= 0; i--) {
            for (int j = 0; j < n - i - 1; j++) System.out.print(" ");
            for (int j = 0; j <= 2 * i; j++) {
                System.out.print("*");
            }
            System.out.println();
        }

        sc.close();
    }
}
```

---

### 16. A luxury jewelry brand is programming a digital storefront display. The display must flash a perfectly symmetrical diamond logo made of LED nodes. Given an integer $N$ representing the half-height of the diamond, write a program to generate this pattern.

#### **Input Format:**

> A single integer $N$ denoting the number of rows in the top half of the diamond.

#### **Output Format:**

> A perfectly centered diamond shape made of `*` characters.

#### **Constraints:**

> $1 \le N \le 100$

#### **Sample Input 1:**

`3`

#### **Sample Output 1:**

`  *`
` ***`
`*****`
` ***`
`  *`

#### **Sample Input 2:**

`2`

#### **Sample Output 2:**

` *`
`***`
` *`

#### Coding Solution:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n - i - 1; j++) System.out.print(" ");
            for (int j = 0; j <= 2 * i; j++) {
                System.out.print("*");
            }
            System.out.println();
        }

        for (int i = n-2; i >= 0; i--) {
            for (int j = 0; j < n - i - 1; j++) System.out.print(" ");
            for (int j = 0; j <= 2 * i; j++) {
                System.out.print("*");
            }
            System.out.println();
        }

        sc.close();
    }
}
```

---

### 17. A financial analytics firm is analyzing the daily closing prices of a specific stock over $N$ days. The system needs to identify the peak value the stock reached during this period to trigger alert notifications. Given an integer $N$ and an array of $N$ daily prices, write a program to find the largest element in the array.

#### **Input Format:**

> The first line contains an integer $N$ denoting the number of days.
> The second line contains $N$ space-separated integers representing the stock prices.

#### **Output Format:**

> A single integer representing the highest stock price.

#### **Constraints:**

> $1 \le N \le 10^5$
> $-10^9 \le \text{Price}[i] \le 10^9$

#### **Sample Input 1:**

`5`
`120 340 210 500 190`

#### **Sample Output 1:**

`500`

#### **Sample Input 2:**

`4`
`-10 -5 -20 -3`

#### **Sample Output 2:**

`-3`

#### Coding Solution:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextInt();

        int max = Integer.MIN_VALUE;
        for(int i : arr) max = Math.max(max, i);

        System.out.println(max);

        sc.close();
    }
}
```

---

### 18. A cloud computing architecture uses a primary server to handle maximum traffic and a secondary backup server to handle the overflow. To allocate resources, the system needs to find the second-highest processing load recorded across $N$ virtual machines. Given an integer $N$ and an array of processing loads, write a program to find the second largest unique element. If no second largest element exists (e.g., all loads are identical), output -1.

#### **Input Format:**

> The first line contains an integer $N$ denoting the number of virtual machines.
> The second line contains $N$ space-separated integers representing the loads.

#### **Constraints:**

> $2 \le N \le 10^5$
> $-10^9 \le \text{Load}[i] \le 10^9$

#### **Sample Input 1:**

`6`
`12 35 1 10 34 1`

#### **Sample Output 1:**

`34`

#### **Sample Input 2:**

`3`
`10 10 10`

#### **Sample Output 2:**

`-1`

#### Coding Solution:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextInt();

        int max = Integer.MIN_VALUE, secondMax = Integer.MIN_VALUE;
        for(int i : arr) {
            if(i > max) {
                secondMax = max;
                max = i;
            } else if(i > secondMax && i != max) secondMax = i;
        }

        System.out.println(secondMax);

        sc.close();
    }
}
```

---

### 19. A logging server receives error codes in chronological order. However, the debugging dashboard needs to display these codes in LIFO (Last-In-First-Out) order, meaning the most recent errors appear first. Given an integer $N$ and an array of error codes, write a program to reverse the array in-place and print the result.

#### **Input Format:**

> The first line contains an integer $N$ denoting the number of error codes.
> The second line contains $N$ space-separated integers representing the chronological error codes.

#### **Output Format:**

> Space-separated integers representing the reversed array.

#### **Constraints:**

> $1 \le N \le 10^5$
> $0 \le \text{Code}[i] \le 10^5$

#### **Sample Input 1:**

`5`
`101 204 500 404 302`

#### **Sample Output 1:**

`302 404 500 204 101`

#### **Sample Input 2:**

`4`
`10 20 30 40`

#### **Sample Output 2:**

`40 30 20 10`

#### Coding Solution:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextInt();

        int left = 0, right = n-1;

        while(left<right){
            int tmp = arr[left];
            arr[left++] = arr[right];
            arr[right--] = tmp;
        }

        printArray(arr);

        sc.close();
    }

    public static void printArray(int[] array){
        System.out.print("[");
        for(int i = 0; i < array.length; i++){
            if(i == array.length - 1) System.out.print(array[i]+"]");
            else System.out.print(array[i]+", ");
        }
        System.out.println();
    }
}
```

---

### 20. A global e-commerce platform needs to calculate the total daily revenue generated from $N$ individual transactions. Because the platform processes high-value orders, the final sum can grow exceptionally large. Given an integer $N$ and an array representing the value of each transaction, write a program to calculate the exact total revenue.

#### **Input Format:**

> The first line contains an integer $N$ denoting the number of transactions.
> The second line contains $N$ space-separated integers representing the transaction values.

#### **Output Format:**

> A single integer representing the total sum of all elements in the array.

#### **Constraints:**

> $1 \le N \le 10^5$
> $0 \le \text{Value}[i] \le 10^5$

#### **Sample Input 1:**

`5`
`1000 2000 3000 4000 5000`

#### **Sample Output 1:**

`15000`

#### **Sample Input 2:**

`3`
`100000 100000 100000`

#### **Sample Output 2:**

`300000`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextInt();

        long sum = 0;

        for(int i : arr) sum += i;

        System.out.println(sum);

        sc.close();
    }
}
```

---

### 21. A secure communications network uses a cyclic shift protocol to obscure data packets before transmission. The protocol requires the array of packets to be shifted to the right by exactly $K$ positions. Given an integer $N$, an array of data packets, and an integer $K$, write a program to perform this cyclic right rotation in-place.

#### **Input Format:**

> The first line contains an integer $N$ denoting the number of data packets.
> The second line contains $N$ space-separated integers representing the packets.
> The third line contains an integer $K$ denoting the number of right shifts.

#### **Output Format:**

> Space-separated integers representing the rotated array.

#### **Constraints:**

> $1 \le N \le 10^5$
> $0 \le \text{Packet}[i] \le 10^5$
> $0 \le K \le 10^5$

#### **Sample Input 1:**

`5`
`1 2 3 4 5`
`2`

#### **Sample Output 1:**

`4 5 1 2 3`

#### **Sample Input 2:**

`5`
`1 2 3 4 5`
`7`

#### **Sample Output 2:**

`4 5 1 2 3`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        int[] arr = new int[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextInt();

        int k = sc.nextInt();
        k %= n;

        // int[] copy = arr.clone();
        // int j = 0;
        // for(int i = n-k; i < n; i++) copy[j++] = arr[i];
        // for(int i = 0; i < n-k; i++) copy[j++] = arr[i];

        reverseArray(arr, 0, n-1);
        reverseArray(arr, 0, k-1);
        reverseArray(arr, k, n-1);

        printArray(arr);
        sc.close();
    }

    private static void reverseArray(int[] arr, int i, int j){
        while(i < j){
            int tmp = arr[i];
            arr[i++] = arr[j];
            arr[j--]= tmp;
        }
    }
}
```

---

### 22. A secure messaging application encrypts text messages by completely reversing the sequence of characters before transmission. Given a string $S$ representing the original message, write a program to generate the reversed encrypted text.

#### **Input Format:**
> A single string $S$ denoting the message. The string may contain spaces.

#### **Output Format:**
> A single string representing the reversed message.

#### **Constraints:**
> $1 \le \text{Length of } S \le 10^5$

#### **Sample Input 1:**
`TCS NQT`
#### **Sample Output 1:**
`TQN SCT`

#### **Sample Input 2:**
`hello`
#### **Sample Output 2:**
`olleh`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        // char[] ch = s.toCharArray();
        // reverseCharArray(ch, 0, ch.length-1);
        // String rev = new String(ch);
        // System.out.println(rev);
        
        String rev = new StringBuilder(s).reverse().toString();
        System.out.println(rev);

        sc.close();
    }
}
```
____

### 23. A cybersecurity firewall validates incoming access tokens. A token is only considered valid and "symmetric" if it reads the exact same forwards and backwards (a palindrome). Given a string $S$ representing the access token, write a program to determine if the token is a valid palindrome.

#### **Input Format:**
> A single string $S$ denoting the access token.

#### **Output Format:**
> Print `"Palindrome"` if the token is symmetric.
> Print `"Not Palindrome"` if it is not symmetric.

#### **Constraints:**
> $1 \le \text{Length of } S \le 10^5$
> The string is case-sensitive (e.g., "Racecar" is not a palindrome, but "racecar" is).

#### **Sample Input 1:**
`radar`
#### **Sample Output 1:**
`Palindrome`

#### **Sample Input 2:**
`system`
#### **Sample Output 2:**
`Not Palindrome`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        int left = 0, right  = s.length() - 1;
        boolean isPalindrome = true;
        while(left < right){
            if(s.charAt(left) != s.charAt(right)){
                isPalindrome = false;
                break;
            }
            left++; right--;
        }
        System.out.println(isPalindrome ? "Palindrome" : "Not Palindrome");

        sc.close();
    }
}
```
____

### 24. A Natural Language Processing (NLP) module is analyzing text data to determine the phonetic complexity of sentences. The system needs to calculate the exact frequency of vowel characters (A, E, I, O, U) present in the text, regardless of their case. Given a string $S$, write a program to count the total number of vowels.

#### **Input Format:**
> A single string $S$ denoting the text data. The string may contain spaces and special characters.

#### **Output Format:**
> A single integer representing the total count of vowels.

#### **Constraints:**
> $1 \le \text{Length of } S \le 10^5$

#### **Sample Input 1:**
`TCS NQT Advanced 2026`
#### **Sample Output 1:**
`3`

#### **Sample Input 2:**
`Hello World!`
#### **Sample Output 2:**
`3`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        s = s.toLowerCase();
        int res = 0;
        for(char c : s.toCharArray()){
            if(!Character.isLetter(c)) continue;
            if(isVowel(c)) res++;
        }
        System.out.println(res);

        sc.close();
    }
    
    private static boolean isVowel(char c){
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
}
```
____

### 25. A database migration script is parsing raw user input from a legacy system. The old system allowed users to enter account IDs with random spaces, which breaks the new database schema. Given a string $S$ representing a corrupted account ID, write a program to remove all whitespace characters and output the sanitized ID.

#### **Input Format:**
> A single string $S$ denoting the corrupted account ID.

#### **Output Format:**
> A single string representing the sanitized account ID with no spaces.

#### **Constraints:**
> $1 \le \text{Length of } S \le 10^5$

#### **Sample Input 1:**
`TCS NQT 2026`
#### **Sample Output 1:**
`TCSNQT2026`

#### **Sample Input 2:**
`   data   base   `
#### **Sample Output 2:**
`database`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        StringBuilder res = new StringBuilder();
        for(char c : s.toCharArray()){
            if(c == ' ') continue;
            res.append(c);
        }
        System.out.println(res.toString());

        sc.close();
    }
}
```
____

### 26. A cybersecurity team is trying to crack an encrypted file using frequency analysis. They need to know exactly how many times each specific character appears in the intercepted text to find the encryption key. Given a string S representing the intercepted text, write a program to count and print the frequency of every character that appears in the text.

#### **Input Format:**
> A single string S denoting the intercepted text.

#### **Output Format:**
> Print each unique character followed by " - " and its total count, on separate lines. Print them in standard alphabetical/ASCII order.

#### **Constraints:**
> 1 <= Length of S <= 100,000

#### **Sample Input 1:**
`tcs nqt`
#### **Sample Output 1:**
`  - 1`
`c - 1`
`n - 1`
`q - 1`
`s - 1`
`t - 2`

#### **Sample Input 2:**
`hello`
#### **Sample Output 2:**
`e - 1`
`h - 1`
`l - 2`
`o - 1`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        int[] freq = new int[256];
        for(char c : s.toCharArray()) freq[c]++;
        for(int i = 0; i < 256; i++){
            if(freq[i] > 0) System.out.println((char) i + " - " + freq[i]);
        }

        sc.close();
    }
}
```
____

### 27. A cloud computing network has two automated backup servers that trigger at different minute intervals, $A$ and $B$. The network engineers need to know two things: the largest common time block they share to run simultaneous diagnostics (HCF), and the exact minute when both servers will next trigger a backup at the exact same time (LCM). Given two integers $A$ and $B$, write a program to compute their HCF and LCM.

#### **Input Format:**
> Two space-separated integers, $A$ and $B$, denoting the interval minutes.

#### **Output Format:**
> Two space-separated integers representing the HCF and LCM.

#### **Constraints:**
> $1 \le A, B \le 10^5$

#### **Sample Input 1:**
`12 15`
#### **Sample Output 1:**
`3 60`

#### **Sample Input 2:**
`14 21`
#### **Sample Output 2:**
`7 42`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        long a = sc.nextLong();
        long b = sc.nextLong();
        
        long aa = a, bb = b;
        
        while(b != 0){
            long tmp = b;
            b = a % b;
            a = tmp;
        }
        
        long hcf = a;
        long lcm = aa * bb / hcf;
        System.out.println(hcf + " " + lcm);

        sc.close();
    }
}
```
____

### 28. A global logistics company receives shipments from $N$ different suppliers. Each supplier delivers on a strict, repeating daily schedule (e.g., Supplier A every 4 days, Supplier B every 6 days). The warehouse manager needs to know the largest common delivery cycle (HCF) and the exact day when all $N$ suppliers will arrive on the exact same day (LCM). Given an integer $N$ and an array of delivery schedules, compute the overall HCF and LCM.

#### **Input Format:**
> The first line contains an integer $N$ denoting the number of suppliers.
> The second line contains $N$ space-separated integers representing the delivery intervals.

#### **Output Format:**
> Two space-separated integers representing the HCF and LCM of the entire array.

#### **Constraints:**
> $2 \le N \le 50$
> $1 \le \text{Interval}[i] \le 100$

#### **Sample Input 1:**
`3`
`4 6 8`
#### **Sample Output 1:**
`2 24`

#### **Sample Input 2:**
`4`
`12 15 75 120`
#### **Sample Output 2:**
`3 600`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        long[] arr = new long[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextLong();
        
        long hcf = arr[0], lcm = arr[0];
        
        for(int i = 1; i < n; i++){
            hcf = findHCF(hcf, arr[i]);
            long tempHCF = findHCF(lcm, arr[i]);
            lcm = lcm * arr[i] / tempHCF;
        }
        
        System.out.println(hcf + " " + lcm);
        sc.close();
    }
    
    private static long findHCF(long a, long b){
        while(b != 0){
            long tmp = b;
            b = a % b;
            a = tmp;
        }
        return a;
    }
}
```
____

### 30. A cybersecurity system logs the ID of every user who accesses a restricted server. Because users log in and out randomly, the IDs are completely unsorted, and some users log in multiple times. The security team needs a list of unique users who accessed the server, strictly maintaining the chronological order of their very first login. Given an integer $N$ and an unsorted array of user IDs, write a program to remove the duplicates.

#### **Input Format:**
> The first line contains an integer $N$ denoting the number of login events.
> The second line contains $N$ space-separated integers representing the unsorted user IDs.

#### **Output Format:**
> Space-separated integers representing the unique user IDs in their original sequence.

#### **Constraints:**
> $1 \le N \le 10^5$
> $1 \le \text{ID}[i] \le 10^5$

#### **Sample Input 1:**
`7`
`104 201 104 305 201 408 104`
#### **Sample Output 1:**
`104 201 305 408`

#### **Sample Input 2:**
`5`
`50 40 50 40 50`
#### **Sample Output 2:**
`50 40`

#### Coding Solution:

```java
import java.util.Scanner;
import java.util.LinkedHashSet;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();

        // for sorted array
        int idx = 0;
        for (int i = 1; i < n; i++)
            if (arr[i] != arr[idx]) arr[++idx] = arr[i];
        printArray(arr, 0, idx);
        
        //for unsorted array
        LinkedHashSet<Integer> set = new LinkedHashSet<>();
        for(int i : arr) set.add(i);
        int[] res = new int[set.size()];
        int i = 0;
        for(int j : set){
            res[i++] = j;
        }
        printArray(res, 0, set.size()-1);

        sc.close();
    }
}
```
____

### 31. A secure portal uses a unique password recovery system. Instead of answering a security question, the user must provide a new word that is an exact anagram of their original backup phrase. Given two strings, $S1$ and $S2$, write a program to determine if they are valid anagrams of each other. 

#### **Input Format:**
> The first line contains the string $S1$.
> The second line contains the string $S2$.

#### **Output Format:**
> Print `"Anagram"` if the strings are valid anagrams.
> Print `"Not Anagram"` if they are not.

#### **Constraints:**
> $1 \le \text{Length of } S1, S2 \le 10^5$
> Both strings contain only lowercase English letters.

#### **Sample Input 1:**
`listen`
`silent`
#### **Sample Output 1:**
`Anagram`

#### **Sample Input 2:**
`hello`
`world`
#### **Sample Output 2:**
`Not Anagram`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String a = sc.next();
        String b = sc.next();
        
        if(a.length() != b.length()) {
            System.out.println("Not Anagram");
            return;
        }
        
        int[] freq = new int[26];
        for(int i = 0; i < a.length(); i++){
            freq[a.charAt(i)-'a']++;
            freq[b.charAt(i)-'a']--;
        }
        for(int i : freq) if(i != 0) {
            System.out.println("Not Anagram");
            return;
        }
        System.out.println("Anagram");
        
        sc.close();
    }

}
```
____

### 32. A satellite communications receiver is processing a text stream. Atmospheric interference has caused some characters to echo and repeat themselves out of order. The system needs to extract only the first unique appearance of every character, perfectly maintaining their original chronological sequence. Given a string $S$, write a program to remove all duplicate characters.

#### **Input Format:**
> A single string $S$ denoting the corrupted text stream.

#### **Output Format:**
> A single string representing the sanitized text with no duplicate characters.

#### **Constraints:**
> $1 \le \text{Length of } S \le 10^5$

#### **Sample Input 1:**
`programming`
#### **Sample Output 1:**
`progamin`

#### **Sample Input 2:**
`tcs nqt 2026`
#### **Sample Output 2:**
`tcs nq206`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        StringBuilder res = new StringBuilder();
        boolean[] freq = new boolean[256];
        for(char c : s.toCharArray()){
            if(!freq[c]){
                freq[c] = true;
                res.append(c);
            }
        }
        System.out.println(res.toString());
        
        sc.close();
    }
}
```
____
### 33. A legacy keyboard firmware is misfiring its shift-key signals, causing text streams to have completely inverted capitalization. The engineering team needs a script to restore the text without using any built-in string case-conversion libraries. Given a string S, write a program to toggle the case of every English letter (uppercase to lowercase, and vice versa) using pure ASCII mathematics. Special characters and numbers should remain unchanged.

#### **Input Format:**
> A single string S denoting the corrupted text stream.

#### **Output Format:**
> A single string representing the restored text.

#### **Constraints:**
> 1 <= Length of S <= 100,000

#### **Sample Input 1:**
`tcs NQT 2026!`
#### **Sample Output 1:**
`TCS nqt 2026!`

#### **Sample Input 2:**
`HeLlO wOrLd`
#### **Sample Output 2:**
`hElLo WoRlD`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        StringBuilder res = new StringBuilder();
        for(char c : s.toCharArray()){
            if(c >= 'A' && c <= 'Z') res.append((char)(c + 32));
            else if(c >= 'a' && c <= 'z') res.append((char)(c - 32));
            else res.append(c);
        }
        System.out.println(res.toString());
        
        sc.close();
    }
}
```
____

### 34. A cloud storage provider is running out of server space and needs to implement a basic compression algorithm for their text-based log files. The algorithm must convert sequences of repeating characters into the character followed by its exact count (e.g., "aaa" becomes "a3"). Given a string S, write a program to generate the compressed version of the string.

#### **Input Format:**
> A single string S denoting the uncompressed log file.

#### **Output Format:**
> A single string representing the compressed data.

#### **Constraints:**
> $1 \le \text{Length of } S \le 10^5$
> The string contains only lowercase and uppercase English letters.

#### **Sample Input 1:**
`aaabbc`
#### **Sample Output 1:**
`a3b2c1`

#### **Sample Input 2:**
`TCS`
#### **Sample Output 2:**
`T1C1S1`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        
        StringBuilder res = new StringBuilder();
        int count = 1;
        for(int i = 0; i < s.length() - 1; i++){
            if(s.charAt(i) == s.charAt(i+1)) count++;
            else {
                res.append(s.charAt(i)).append(count);
                count = 1;
            }
        }
        res.append(s.charAt(s.length()-1)).append(count);
        System.out.println(res.toString());
        
        sc.close();
    }
}
```
____

### 35. (TCS PYQ) A database system stores data packets in contiguous memory blocks. Due to deletions, several memory blocks now contain null values (represented by 0). To defragment the database and optimize read speeds, the system must shift all valid data packets to the front of the memory array, while pushing all the 0s to the end. The original chronological order of the valid packets must be perfectly maintained. Given an integer $N$ and an array, write a program to perform this operation in-place.

#### **Input Format:**
> The first line contains an integer $N$ denoting the size of the memory array.
> The second line contains $N$ space-separated integers representing the data packets (including 0s).

#### **Output Format:**
> Space-separated integers representing the defragmented array.

#### **Constraints:**
> $1 \le N \le 10^5$
> $0 \le \text{Packet}[i] \le 10^5$

#### **Sample Input 1:**
`6`
`4 5 0 1 9 0`
#### **Sample Output 1:**
`4 5 1 9 0 0`

#### **Sample Input 2:**
`5`
`0 0 0 2 3`
#### **Sample Output 2:**
`2 3 0 0 0`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i = 0; i < n; i++){
            arr[i] = sc.nextInt();
        }
        
        int count = 0;
        for(int i = 0; i < n; i++){
            if(arr[i] != 0){
                arr[count++] = arr[i];
            }
        }
        while(count < n){
            arr[count++] = 0;
        }
        
        for(int i = 0; i < n; i++){
            if(i == n-1) System.out.print(arr[i]);
            else System.out.print(arr[i] + " ");
        }
        sc.close();
    }
}
```
____

### 36. (TCS PYQ) A cryptographic key generation system relies on a mixed mathematical sequence. The odd positions of the sequence follow the Fibonacci series (1, 1, 2, 3, 5...), while the even positions follow the Prime number series (2, 3, 5, 7, 11...). Given an integer $N$, write a program to find the exact $N$th term of this mixed sequence.

#### **Input Format:**
> A single integer $N$ denoting the position in the mixed sequence.

#### **Output Format:**
> A single integer representing the sequence value at position $N$.

#### **Constraints:**
> $1 \le N \le 500$

#### **Sample Input 1:**
`6`
#### **Sample Output 1:**
`5`
*(Explanation: The 6th position is an even position. The 3rd prime number is 5).*

#### **Sample Input 2:**
`7`
#### **Sample Output 2:**
`3`
*(Explanation: The 7th position is an odd position. The 4th Fibonacci number is 3).*

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        if(n % 2 != 0){
            System.out.println(getFib(n/2+1));
        } else System.out.println(getPrime(n/2));
        
        sc.close();
    }
    
    private static int getFib(int n){
        if(n == 1) return 0;
        if(n == 3 || n == 2) return 1;
        long prev = 1, prev2 = 1;
        for(int i = 4; i <= n; i++){
            long c = prev + prev2;
            prev2 = prev;
            prev = c;
        }
        return (int) prev;
    }
    
    private static int getPrime(int n){
        int count = 0, num = 2;
        
        while(count < n){
            if(isPrime(num)) count++;
            num++;
        }
        
        return num - 1;
    }
    
    private static boolean isPrime(int n){
        for(int i = 2; i * i <= n; i++){
            if(n % i == 0) return false;
        }
        return true;
    }
}
```
____

### 37. A cybersecurity firewall analyzes incoming data streams. To detect a specific type of encrypted malware payload, the firewall must count how many contiguous data segments (substrings) can be rearranged into a perfect palindrome. A segment that can be rearranged into a palindrome is flagged as a "Fake Palindrome." Given a string $S$ representing the data stream, write a highly optimized program to output the total number of fake palindrome substrings.

#### **Input Format:**
> A single string $S$ containing only uppercase English letters.

#### **Output Format:**
> A single integer representing the total number of fake palindrome substrings.

#### **Constraints:**
> $1 \le \text{Length of } S \le 2 \times 10^5$
> Time Limit: 1.0 second (Requires $O(N)$ optimization)

#### **Sample Input 1:**
`ABAB`
#### **Sample Output 1:**
`7`

#### **Sample Input 2:**
`AAA`
#### **Sample Output 2:**
`6`

#### Coding Solution:

```java
import java.util.Scanner;
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        
        Map<Integer, Integer> map = new HashMap<>();
        int mask = 0;
        long res = 0;
        map.put(0, 1);
        
        for(char c : s.toCharArray()){
            mask ^= (1 << (c - 'A'));
            res += map.getOrDefault(mask, 0);
            for(int i = 0; i < 26; i++){
                int newMask = mask ^ (1 << i);
                res += map.getOrDefault(newMask, 0);
            }
            map.put(mask, map.getOrDefault(mask, 0) + 1);
        }
        
        System.out.println(res);
        
        sc.close();
    }
}
```
____

### 39. (TCS Advanced) A telecommunications company is routing data packets through a network tree of $N$ servers. Each server adds a specific latency value to the packet. The goal is to find the maximum possible latency path from the root server (Node 1) to any endpoint (leaf node), with a strict security rule: the total latency must NOT be perfectly divisible by a specific frequency key $K$. Given the tree structure and values, find the maximum valid path sum.

#### **Input Format:**
> Line 1: An integer $N$ (number of nodes).
> Line 2: $N$ space-separated integers representing the node values.
> Next $N-1$ lines: Pairs of integers $u$ and $v$ representing the edges.
> Final line: An integer $K$ (the divisor).

#### **Output Format:**
> A single integer representing the maximum valid sum. Print `-1` if no valid path exists.

#### **Constraints:**
> $1 \le N \le 100,000$
> $1 \le \text{Value}[i], K \le 10^4$

#### **Sample Input:**
`7`
`3 4 8 2 1 6 10`
`1 2`
`1 3`
`2 4`
`2 5`
`3 6`
`3 7`
`5`

#### **Sample Output:**
`21`

#### Coding Solution:

```java
import java.util.Scanner;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;
import java.util.LinkedList;

public class Main {
    static class Node{
        int id;
        int parent;
        long sum;
        
        Node(int id, int parent, long sum){
            this.id = id;
            this.parent = parent;
            this.sum = sum;
        }
    }
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if(sc.hasNext()){
            int n = sc.nextInt();
            
            long[] values = new long[n+1];
            for(int i = 1; i <= n; i++) values[i] = sc.nextLong();
            
            List<Integer>[] adj = new ArrayList[n+1];
            for(int i = 1; i <= n; i++) adj[i] = new ArrayList<>();
            
            for(int i = 0; i < n - 1; i++){
                int a = sc.nextInt();
                int b = sc.nextInt();
                adj[a].add(b);
                adj[b].add(a);
            }
            
            long k = sc.nextLong();
            
            Queue<Node> queue = new LinkedList<>();
            queue.add(new Node(1, 0, values[1]));
            
            long res = -1;
            
            while(!queue.isEmpty()){
                Node cur = queue.poll();
                
                boolean leaf = true;
                for(int neighbour : adj[cur.id]){
                    if(neighbour != cur.parent){
                        leaf = false;
                        queue.add(new Node(neighbour, cur.id, cur.sum + values[neighbour]));
                    }
                }
                
                if(leaf && cur.sum % k != 0) res = Math.max(res, cur.sum);
            }
            
            System.out.println(res);
        }
        
        sc.close();
    }
}
```
____

### 40. (TCS PYQ) Alice and her friends are playing a game of verbal Kho-Kho. Alice is acting as a mediator, and the rest of the $N$ friends are seated on $N$ chairs. Alice provides a paper with a single-digit number to Friend 1. Friend 1 enacts the digit to Friend 2 without speaking, who passes it to Friend 3, and so on. Finally, the last person writes the digit on a paper. Alice checks everyone's understood digit against the original digit she gave Friend 1. She will only give a T-shirt to the friends who understood the exact original digit. Given the number of friends $N$ and an array $D$ denoting the digit understood by each friend, find out exactly how many friends failed to understand the original digit.

#### **Input Format:**
> The first line contains an integer $N$ denoting the number of friends.
> The second line contains $N$ space-separated integers representing the array $D$ (the digit each friend understood).

#### **Output Format:**
> A single integer representing the number of friends who will not receive a T-shirt.

#### **Constraints:**
> $1 \le N \le 10^5$
> $0 \le D[i] \le 9$

#### **Sample Input 1:**
`3`
`4 4 4`
#### **Sample Output 1:**
`0`

#### **Sample Input 2:**
`5`
`1 2 3 2 2`
#### **Sample Output 2:**
`4`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if(!sc.hasNext()) return;
        int n = sc.nextInt();
        int[] arr = new int[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextInt();
        
        int res = 0;
        for(int i = 1; i < n; i++) 
            if(arr[0] != arr[i]) res++;
        System.out.println(res);
        
        sc.close();
    }
}
```
____

### 41. (TCS PYQ) Bob wants to bet on a continuous sequence of horses to maximize his chances of winning. He has $N$ horses to choose from, each with a specific betting price. He will win $K$ units of money if any of his chosen horses win. To make a profit, the total sum of his bets must be strictly less than $K$. Given the prices of the $N$ horses in order, write a program to find the maximum length of a continuous sequence of horses Bob can bet on.

#### **Input Format:**
> The first line contains two space-separated integers, $N$ (number of horses) and $K$ (reward money).
> The second line contains $N$ space-separated integers representing the price to bet on each horse.

#### **Output Format:**
> A single integer representing the maximum length of the valid continuous sequence.

#### **Constraints:**
> $2 \le N \le 10^5$
> $1 \le K \le 10^9$
> $1 \le \text{Price}[i] \le 10^9$

#### **Sample Input 1:**
`10 100`
`30 40 50 20 20 10 90 10 10 10`
#### **Sample Output 1:**
`3`

#### **Sample Input 2:**
`10 100`
`10 90 80 20 90 60 40 60 70 75`
#### **Sample Output 2:**
`1`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if(!sc.hasNext()) return;
        int n = sc.nextInt();
        long k = sc.nextLong();
        
        long[] arr = new long[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextLong();
        
        int left = 0, res = 0;
        long sum = 0;
        
        for(int right = 0; right < n; right++){
            sum += arr[right];
            
            while(sum >= k && left <= right){
                sum -= arr[left++];
            }
            
            res = Math.max(res, right - left + 1);
        }
        System.out.println(res);
        
        sc.close();
    }
}
```
____

### 42. (TCS PYQ) The Golden House Puzzle. A mansion owner needs a manager and has created a test: find a continuous sequence of rooms such that the total number of gold coins collected perfectly equals $K$. You must enter a room, continuously collect coins from adjacent rooms, and exit. Given $N$ rooms and the coins in each, output the 1-based starting and ending room numbers. If multiple solutions exist, output the one with the smallest starting room number.

#### **Input Format:**
> The first line contains two integers, $N$ (number of rooms) and $K$ (target coins).
> The second line contains $N$ space-separated integers representing the coins in each room.

#### **Output Format:**
> Two space-separated integers representing the 1-based start and end room numbers.

#### **Constraints:**
> $1 \le N \le 10^5$
> $1 \le K \le 10^9$
> $0 \le \text{Coins}[i] \le 10^4$

#### **Sample Input 1:**
`10 15`
`5 3 7 14 18 1 18 4 8 3`
#### **Sample Output 1:**
`1 3`

#### Coding Solution:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if(!sc.hasNext()) return;
        int n = sc.nextInt();
        long k = sc.nextLong();
        
        long[] arr = new long[n];
        for(int i = 0; i < n; i++) arr[i] = sc.nextLong();
        
        int left = 0;
        long sum = 0;
        
        for(int right = 0; right < n; right++){
            sum += arr[right];

            while(sum > k && left <= right) sum -= arr[left++];
            
            if(sum == k){
                System.out.println((left+1) + " " + (right+1));
                break;
            }
        }

        sc.close();
    }
}
```
____

### 45. (TCS Advanced) Tina has a piece of silk cloth with $N$ specific coordinates marked on it. She needs to cut out a perfect square. She wants to use as many of the existing marked points as possible, minimizing the number of new coordinates she has to draw herself. If multiple squares require the same minimum number of new points, she prefers the larger square. Given the $N$ coordinates, find the absolute minimum number of additional points Tina must draw to form a valid square.

#### **Input Format:**
> The first line contains an integer $N$ (number of given points).
> The next $N$ lines each contain two space-separated integers, $x$ and $y$, representing a coordinate.

#### **Output Format:**
> A single integer representing the minimum number of additional points needed.

#### **Constraints:**
> $0 \le N \le 1000$
> $-10^4 \le x, y \le 10^4$

#### **Sample Input 1:**
`5`
`0 0`
`100 100`
`200 200`
`100 0`
`0 100`
#### **Sample Output 1:**
`0`

#### **Sample Input 2:**
`3`
`0 0`
`2 2`
`3 3`
#### **Sample Output 2:**
`2`

#### Coding Solution:

```java
import java.util.Scanner;
import java.util.HashSet;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        if(!sc.hasNext()) return;
        int n = sc.nextInt();
        
        if(n == 0) {System.out.println(4); return;}
        if(n == 1) {System.out.println(3); return;}
        
        int[] x = new int[n];
        int[] y = new int[n];
        
        HashSet<String> set = new HashSet<>();
        
        for(int i = 0; i < n; i++){
            x[i] = sc.nextInt();
            y[i] = sc.nextInt();
            set.add(x[i]+","+y[i]);
        }
        
        int minNeeded = 2;
        
        for(int i = 0; i < n; i++){
            for(int j = i + 1; j < n; j++){
                int p1x = x[i], p1y = y[i];
                int p2x = x[j], p2y = y[j];
                
                int dx = p2x - p1x;
                int dy = p2y  - p1y;
                

                int p3x = p1x - dy, p3y = p1y + dx;
                int p4x = p2x - dy, p4y = p2y + dx;
                
                boolean existp3 = set.contains(p3x+","+p3y);
                boolean existp4 = set.contains(p4x+","+p4y);
                
                int curNeeded = 2;
                if(existp3 && existp4) curNeeded = 0;
                else if(existp3 || existp4) curNeeded = 1;
                
                if(curNeeded < minNeeded){
                    minNeeded = curNeeded;
                    if(minNeeded == 0) {
                        print(minNeeded);
                        sc.close();
                        return;
                    }
                } 
            }
        }
        print(minNeeded);

        sc.close();
    }
}
```
____
