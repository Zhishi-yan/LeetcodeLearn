1. 入门题目
    1. [输入处理：HJ5进制转换](#hj5)
    2. [排列组合：NC61两数之和](#nc61)
    3. [快速排序：HJ3](#hj3)
    4. [字符个数的统计 HJ10 哈希表 treeMap](#hj10)
    5. [递归：NC68 跳台阶 动态规划](#nc68)
2. 字符串操作
    1. [HJ17 坐标移动](#hj17)
    2. [HJ20 密码验证合格程序](#hj20)
    3. [HJ23 删除字符串中出现次数最少的字符](#hj23)
    4. [HJ33 整数与IP地址间的转换](#hj33)
    5. [HJ101 输入整型数组和排序标识,归并排序](#hj101)
    6. [HJ106 字符串逆序](#hj106)
3. 排序
    1. [HJ8 合并表记录](#hj8)
    2. [HJ14 字符串排序](#hj14)
    3. [HJ27 查找兄弟单词](#hj27)
    4. [NC37 合并区间](#nc37)
    5. [HJ68 成绩排序](#hj68)
4. 栈
    1. [NC52 括号序列](#nc52)
    2. [leetcode 1614 括号的最大嵌套深度](#1614)
5. 排列组合
    1. [leetcode 08.08 有重复字符串的排列组合](#0808)
    2. [leetcode 77 组合](#77)
6. 双指针
    1. [leetcode 674 最长连续递增序列](#674)
    2. [NC17 最长回文子串](#nc17)
    3. [NC28 最小覆盖子串](#nc28)
7. 其他
   1. [HJ41 称砝码](#hj41)
   2. [BM95 分糖果问题 动态规划](#bm95)
   3. [BM96 主持人调度（二）](#bm96)
   4. [csv](#csv)
   5. [全排列](#46)



##### HJ5

```java linenums
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

//进制转换
/*
 * 写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。
 * 输入一个十六进制的数值字符串。
 * 输出该数值的十进制字符串。不同组的测试用例用\n隔开。
 * 0xAA
 */
public class test1Hj5 {
    public static void main(String[] args) throws IOException {
        /*System.in 标准输入流，字节流
        InputStreamReader 将字节流转换为字符流
        BufferReader 提供缓冲以提高性能，也是一个字符流
        readLine()方法用于读取一行数据*/
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String input;
        while ((input = bf.readLine()) != null) {
            //从字符串中提取子字符串，从第二个索引到结尾
            String temp = input.substring(2, input.length());
            //创建一个和字符串temp长度一致的数组，并将字符串中的字符存入数组中
            int[] arr = new int[temp.length()];
            for (int i = arr.length - 1; i >= 0; i--) {
                if (temp.charAt(i) >= 'A') {
                    arr[i] = temp.charAt(i) - 'A' + 10;
                } else {
                    arr[i] = temp.charAt(i) - '0';
                }
            }

            //进制转换
            int sum = 0;
            int flag = 0;
            for (int i = arr.length - 1; i >= 0; i--) {
                sum += arr[i] * Math.pow(16, flag++);
            }
            System.out.println(sum);

        }
    }
}

```

##### NC61

```java

public class Main {
    public static int[] twoSum(int[] numbers, int target) {
        int[] sum = new int[2];

        for (int i = 0; i < numbers.length; i++) {
            if (numbers[i] > target) {
                //若当前元素大于或等于目标值，则跳过当前循环
                continue;
            }
            for (int i1 = i + 1; i1 < numbers.length; i1++) {
                if (numbers[i] + numbers[i1] == target) {
                    sum[0] = i + 1;
                    sum[1] = i1 + 1;
                    return sum;
                }
            }
        }
        return sum;

    }


    //    [20,70,110,150],90
    public static void main(String[] args) {
        int[] numbers = {20, 70, 110, 150};
        int target = 90;
        int[] ints = twoSum(numbers, target);
        for (int i = 0; i < ints.length; i++) {
            System.out.print(ints[i]);
        }
    }
}

```

##### HJ3

```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Map;
import java.util.TreeMap;

public class Main {
    public static void main(String[] args) throws IOException {
        Map<Integer, Integer> hashMap = new TreeMap<>();
        String str = null;
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int i = -1;
        int num = 0;

        while ((str = bf.readLine()) != null) {
            i++;
            if (i == 0) {
                num = Integer.parseInt(str);
            } else {
                hashMap.put(Integer.valueOf(str), i);
                if (i >= num) {
                    break;
                }
            }
        }

        for (int key : hashMap.keySet()) {
            System.out.println(key);
        }
    }
}
```

##### HJ10

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Map;
import java.util.TreeMap;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str;
        Map<String, Integer> hashMap = new TreeMap<>();
        while ((str = bf.readLine()) != null) {
            for (int i = 0; i < str.length(); i++) {
                hashMap.put(String.valueOf(str.charAt(i)), 1);
            }
            System.out.println(hashMap.size());
        }
    }
}
```

##### NC68

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。斐波那契数列。

```java
// F[n] = F[n-1] + F[n-2]
// F[3] = F[2] + F[1]
// F[1] = 1
// F[2] = 2

import java.util.Scanner;

public class Main {

    public static int jumpFloor(int number) {
        int[] F = new int[number];//定义数组
        //1≤n≤500
        if (number == 1) {
            return 1;
        }
        if (number == 2) {
            return 2;
        }

        for (int i = 0; i < number; i++) {
            if (i < 2) {
                F[i] = i + 1;
            } else {
                F[i] = F[i - 1] + F[i - 2];
            }
        }
        return F[number - 1];
    }


    public static void main(String[] args) {
        // 使用Scanner类从标准输入中读取整型数据
        Scanner sc = new Scanner(System.in);
        // 使用nextInt()方法获取数据
        int num = sc.nextInt();
        sc.close();//关闭sc
        int output = jumpFloor(num);
        System.out.println(output);
    }
}
```

##### HJ17

```java
//字符串分割split的用法

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        //测试用例  输入     A10;S20;W10;D30;X;A1A;B10A11;;A10;
        //输出            10,-10
        //
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str;
        int num = 0;
        int x = 0;
        int y = 0;
        while ((str = bf.readLine()) != null) {
            String[] input = str.split(";");
            for (String key : input) {
                num = 0;
                if (key.length() <= 1 || key.length() >= 4) {
                    continue;
                }
                if (key.length() == 2 && (key.charAt(1) >= '0' && key.charAt(1) <= '9')) {
                    num = (key.charAt(1) - '0');
                }
                if (key.length() == 3 && (key.charAt(1) >= '0' && key.charAt(1) <= '9') && (key.charAt(2) >= '0' && key.charAt(2) <= '9')) {
                    num = (key.charAt(1) - '0') * 10 + (key.charAt(2) - '0');
                }
                switch (key.charAt(0)) {
                    case 'A':
                        x -= num;
                        break;
                    case 'D':
                        x += num;
                        break;
                    case 'W':
                        y += num;
                        break;
                    case 'S':
                        y -= num;
                        break;
                    default:
                        break;
                }

            }
            System.out.println(x + "," + y);
        }
    }
}

```

##### [HJ20](https://www.nowcoder.com/practice/184edec193864f0985ad2684fbc86841?tpId=37&ru=%2Fexam%2Foj&difficulty=&judgeStatus=&tags=&title=HJ20&sourceUrl=&gioEnter=menu)

```java
// 长度超过8位
// 包括大小写字母，数字，其他符号，至少3种
// 不能有长度大于2的包含公共元素的子串重复
// 最长重复子串不得大于等于3

//  测试用例
//  021Abc9000
//  021Abc9Abc1
//  021ABC9000
//  021$bc9000


import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) throws IOException {
        // 长度超过8位
        // 包括大小写字母，数字，其他符号，至少3种
        // 不能有长度大于2的包含公共元素的子串重复
        // 最长重复子串不得大于等于3
        String str;
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));


        while ((str = bf.readLine()) != null) {

            int isA = 0;//是否有大写字母
            int isa = 0;//是否有小写字母
            int isNum = 0;//是否有数字
            int isOther = 0;//是否有其他字符，其他符号不含空格或换行

            //如果长度小于等于8位，则输出NG
            if (str.length() <= 8) {
                System.out.println("NG");
                continue;
            }

            //遍历字符串的每个元素，查找字符类型是否大于等于3种
            for (int i = 0; i < str.length(); i++) {
                //是否有大写字母
                if (str.charAt(i) >= 'A' && str.charAt(i) <= 'Z') {
                    isA = 1;
                }
                //是否有小写字母
                else if (str.charAt(i) >= 'a' && str.charAt(i) <= 'z') {
                    isa = 1;
                }
                //是否有数字
                else if (str.charAt(i) >= '0' && str.charAt(i) <= '9') {
                    isNum = 1;
                }
                //是否有其他字符
                else if (str.charAt(i) != ' ' && str.charAt(i) != '\n') {
                    isOther = 1;
                }
            }
            //如果符号类型小于3种，则打印NG
            if (isA + isa + isNum + isOther < 3) {
                System.out.println("NG");
                continue;
            }

            //使用TreeSet容器来存放子串，子串大小为3
            TreeSet<String> mySet = new TreeSet<>();
            for (int i = 0; i < str.length() - 2; i++) {
                mySet.add(str.substring(i, i + 3));
                //System.out.println(str.substring(i, i + 3));
            }
            //代表有重复的子串
            if (mySet.size() < str.length() - 2) {
                System.out.println("NG");
            } else {
                System.out.println("OK");
            }

        }
    }
}
```

##### [HJ23](https://www.nowcoder.com/practice/05182d328eb848dda7fdd5e029a56da9?tpId=37&ru=%2Fexam%2Foj&difficulty=&judgeStatus=&tags=&title=HJ23&sourceUrl=&gioEnter=menu)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;


public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str;
        LinkedHashMap<Character, Integer> myMap = new LinkedHashMap<>();
        List<Character> myList = new ArrayList<>();
        while ((str = bf.readLine()) != null) {

            for (int i = 0; i < str.length(); i++) {
                //使用getOrDefault方法获取键对应的值，不存在为0，存在则返回key对应的值
                int val = myMap.getOrDefault(str.charAt(i), 0);
                myMap.put(str.charAt(i), val + 1);
                myList.add(str.charAt(i));
            }

            int minVal = 50;
            //遍历每个键值对 求最小键值
            for (Character key : myMap.keySet()) {
                minVal = myMap.getOrDefault(key, 0) < minVal ? myMap.getOrDefault(key, 0) : minVal;
            }

            for (Character key : myMap.keySet()) {
                if (myMap.getOrDefault(key, 0) == minVal) {
                    while (myList.remove(key)) {
                    }
                }
            }
            for (Character c : myList) {
                System.out.print(c);
            }
        }
    }
}


```

##### [HJ33](https://www.nowcoder.com/practice/66ca0e28f90c42a196afd78cc9c496ea?tpId=37&tqId=21256&ru=/exam/oj)

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String IP;//第一行ip
        String num;//第二行数字
        IP = sc.nextLine();
        num = sc.nextLine();

        String[] ip = IP.split("\\.");
        String ip2num = "";
        for (int i = 0; i < ip.length; i++) {
            //把数字转换成2进制字符串
            ip[i] = Integer.toBinaryString(Integer.parseInt(ip[i]));
            //格式化成8位,不足8位的，会在前面补充空格
            ip[i] = String.format("%8s", ip[i]);
            //把空格变为0
            ip[i] = ip[i].replace(' ', '0');
            ip2num += ip[i];//字符串拼接在一起
        }
        //把拼接后的2进制字符串转换为10进制
        long idOutput = Long.parseLong(ip2num, 2);//这里也要使用Long.parseLong 而不能使用Integer.parseInt
        System.out.println(idOutput);

        //处理第二行数字，先转换成2进制，补足32位
        num = Long.toBinaryString(Long.parseLong(num));//这里要使用Long 不能使用integer
        num = String.format("%32s", num);//在前面补空格，补足32位
        num = num.replace(' ', '0');//把空格变成0
        String subStr = "";//定义子串，存放8位2进制
        String numOutput = "";
        for (int i = 0; i < num.length() / 8; i++) {
            subStr = num.substring(i * 8, i * 8 + 8);
            //把子串转换为10进制
            subStr = String.valueOf(Integer.parseInt(subStr, 2));
            if (i < 3) {
                numOutput += subStr + '.';
            } else {
                numOutput += subStr;
            }
        }
        System.out.println(numOutput);
    }
}

```

##### [HJ101](https://www.nowcoder.com/practice/dd0c6b26c9e541f5b935047ff4156309?tpId=37&tqId=21324&ru=/exam/oj)

```java

import java.util.Scanner;

public class Main {
    public static void mergeSort(int[] arr, int start, int end) {
        if (start < end) {
            //每个子数组至少有2个元素
            int j = start - 1;
            for (int i = start; i < end; i++) {
                if (arr[i] < arr[end]) {
                    //若当前值小于末尾的基准元素，则把它放在左边
                    //每次交换后，j要加1
                    int temp = arr[i];
                    arr[i] = arr[++j];
                    arr[j] = temp;
                }
            }
            int temp = arr[end];
            arr[end] = arr[++j];
            arr[j] = temp;
            mergeSort(arr, start, j - 1);//左边的部分
            mergeSort(arr, j + 1, end);//右边的部分
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int num = Integer.parseInt(sc.nextLine());
        String str = sc.nextLine();
        String[] strArr = str.split(" ");//分隔符为空格
        int isUP = Integer.parseInt(sc.nextLine());//0升序，1降序

        int[] arr = new int[num];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = Integer.parseInt(strArr[i]);
        }
        mergeSort(arr, 0, arr.length - 1);
        //使用归并排序,传入数组，起始点，终点，基准元素,一般是末尾元素
        if (isUP == 0) {

            for (int i : arr) {
                System.out.print(i + " ");
            }
        } else {

            for (int i = arr.length - 1; i >= 0; i--) {
                System.out.print(arr[i] + " ");
            }
        }
    }
}
```

##### [HJ106](https://www.nowcoder.com/practice/cc57022cb4194697ac30bcb566aeb47b?tpId=37&ru=%2Fexam%2Foj&difficulty=&judgeStatus=&tags=&title=HJ106&sourceUrl=&gioEnter=menu)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.List;

public class Main {
    public static void main(String[] args) throws IOException {
        //将一个字符串str的内容颠倒过来，并输出。
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str;
        //创建一个list容器，存储每个字符，包括空格，使用.forr方法，倒序打印
        List<String> myList = new LinkedList<>();
        while ((str = bf.readLine()) != null) {
            for (int i = 0; i < str.length(); i++) {
                myList.add(String.valueOf(str.charAt(i)));
            }
            for (int i = myList.size() - 1; i >= 0; i--) {
                System.out.print(myList.get(i));
            }
        }
    }
}
```

##### [HJ8](https://www.nowcoder.com/practice/de044e89123f4a7482bd2b214a685201?tpId=37&ru=%2Fexam%2Foj&difficulty=&judgeStatus=&tags=&title=HJ8&sourceUrl=&gioEnter=menu)

```java
import java.util.Map;
import java.util.Scanner;
import java.util.TreeMap;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int num = Integer.parseInt(sc.nextLine());
        String str = "";
        String[] strArr = new String[num];
        //LinkedHashMap是按照插入顺序排列的，保留了插入顺序
        //TreeMap是按照key升序排列的
        //HashMap是不排列顺序的，无序的
        Map<Integer, Integer> myMap = new TreeMap<>();
        for (int i = 0; i < num; i++) {
            str = sc.nextLine();
            strArr = str.split(" ");
            //查询当前的key在Map中有没有，没有返回0，有返回键值
            int val = myMap.getOrDefault(Integer.parseInt(strArr[0]), 0);
            //把当前键值和查询到的val相加作为新键值
            myMap.put(Integer.parseInt(strArr[0]), val + Integer.parseInt(strArr[1]));
        }
        for (int key : myMap.keySet()) {
            System.out.print(key + " ");
            int val = myMap.get(key);
            System.out.println(val);
        }
    }
}
```

##### [HJ14](https://www.nowcoder.com/practice/5af18ba2eb45443aa91a11e848aa6723?tpId=37&tqId=21237&ru=/exam/oj)

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        //对于字符串元素，TreeMap 和 TreeMap 都是按照字典序排序
        Scanner sc = new Scanner(System.in);
        int num = Integer.parseInt(sc.nextLine());
//        Set<String> mySet = new TreeSet<>(); // 重复元素无法处理
        Map<String, Integer> myMap = new TreeMap<>();
        for (int i = 0; i < num; i++) {
            //查询当前键，有返回其键值，无则为0
            String key = sc.nextLine();
            int val = myMap.getOrDefault(key, 0);
            myMap.put(key, val + 1);
        }
        for (String key : myMap.keySet()) {
            num = myMap.get(key);
            for (int i = 0; i < num; i++) {
                System.out.println(key);
            }
        }
    }
}
```

##### [HJ27](https://www.nowcoder.com/practice/03ba8aeeef73400ca7a37a5f3370fe68?tpId=37&tqId=21250&ru=/exam/oj)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str;
        String[] strArr;
        //字典序，不能使用TreeSet，因为会存在重复的，使用List 再使用Collection.sort
        List<String> myList = new ArrayList<>();
        while ((str = bf.readLine()) != null) {
            strArr = str.split(" ");
            int num = Integer.parseInt(strArr[0]);//第一个是数字
            //把字典中的单词存储到myList中
            for (int i = 0; i < num; i++) {
                myList.add(strArr[i + 1]);
            }
            //最后一个字符串存到x中
            String x = strArr[strArr.length - 2];
            int k = Integer.parseInt(strArr[strArr.length - 1]);
            int m = 0;//兄弟的个数
            List<String> outputStr = new ArrayList<>();//输出的兄弟单词
            //对List字典序排序
            Collections.sort(myList);

            //遍历每一个myList中的元素
            for (int i = 0; i < myList.size(); i++) {
                //x和myList中的元素不能相等，且长度必须一致
                if (!x.equals(myList.get(i)) && x.length() == myList.get(i).length()) {
                    //对x和myList当前的字符串 排序，若相等则是兄弟单词
                    char[] x_new = x.toCharArray();
                    char[] s_new = myList.get(i).toCharArray();
                    Arrays.sort(x_new);
                    Arrays.sort(s_new);
                    //排序后如果相同，是兄弟单词
                    if (Arrays.equals(x_new, s_new)) {
                        m += 1;
                        outputStr.add(myList.get(i));
                    }
                }
            }
            System.out.println(m);
            if (outputStr.size() > k - 1) {
                System.out.println(outputStr.get(k - 1));
            }
        }
    }
}
```

##### [NC37](https://www.nowcoder.com/practice/69f4e5b7ad284a478777cb2a17fb5e6a?tpId=196&tqId=37071&ru=/exam/oj)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;

public class Main {
    public static class Interval {
        int start;
        int end;

        public Interval(int start, int end) {
            this.start = start;
            this.end = end;
        }
    }


    public static ArrayList<Interval> merge(ArrayList<Interval> intervals) {
        // write code here
        ArrayList<Interval> ret = new ArrayList<>();
        if (intervals.isEmpty()) {
            return ret;
        }
        intervals.sort((a, b) -> {
            return (a.start - b.start);
        });
        while (intervals.size() > 1) {
            //至少有两个
            //当前区间的头，在上一个区间的头尾之间，则合并
            if (intervals.get(1).start >= intervals.get(0).start && intervals.get(1).start <= intervals.get(0).end) {
                Interval temp = new Interval(Math.min(intervals.get(0).start, intervals.get(1).start), Math.max(intervals.get(0).end, intervals.get(1).end));
                intervals.remove(0);
                intervals.remove(0);
                intervals.add(0, temp);
            } else {
                Interval temp = new Interval(intervals.get(0).start, intervals.get(0).end);
                ret.add(temp);
                intervals.remove(0);
            }
        }
        ret.add(intervals.get(0));
        return ret;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str;
        while ((str = bf.readLine()) != null) {
            str = str.replace("[", "");
            str = str.replace("]", "");
            String[] strArr = str.split(",");
            ArrayList<Interval> inputInterval = new ArrayList<Interval>();
            for (int i = 0; i < strArr.length / 2; i++) {
                inputInterval.add(new Interval(Integer.parseInt(strArr[2 * i]), Integer.parseInt(strArr[2 * i + 1])));
            }

            ArrayList<Interval> mergeRet = merge(inputInterval);


        }
    }
}
```

##### [HJ68](https://www.nowcoder.com/practice/8e400fd9905747e4acc2aeed7240978b?tpId=37&tqId=21291&ru=/exam/oj)

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    public static class Grade {
        String name;
        int grade;

        public Grade(String name, int grade) {
            this.grade = grade;
            this.name = name;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = Integer.parseInt(sc.nextLine());//要排序的人的个数
        int sortType = Integer.parseInt(sc.nextLine());//0代表降序，1代表升序
        String strTemp;
        String[] strArr;
        ArrayList<Grade> myList = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            strTemp = sc.nextLine();
            strArr = strTemp.split(" ");
            myList.add(new Grade(strArr[0], Integer.parseInt(strArr[1])));
        }
        if (sortType == 0) {
            myList.sort((a, b) -> Integer.compare(b.grade, a.grade));//降序排列
            //myList.sort((a, b) -> (b.grade - a.grade));//降序排列
        }
        if (sortType == 1) {
            myList.sort((a, b) -> Integer.compare(a.grade, b.grade));//升序排列
            //myList.sort((a, b) -> (a.grade - b.grade));//升序排列

        }
        for (Grade g : myList) {
            System.out.println(g.name + " " + g.grade);
        }
    }
}
```

##### [NC52](https://www.nowcoder.com/practice/37548e94a270412c8b9fb85643c8ccc2?tpId=196&tqId=37083&ru=/exam/oj)

```java
import java.util.*;


public class Main {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * @param s string字符串
     * @return bool布尔型
     */
    public static boolean isValid(String s) {
        ArrayDeque<Character> deque = new ArrayDeque<>();
        if (s.length() <= 1) {
            //如果长度为0或者1
            return false;
        }
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '[' || s.charAt(i) == '{' || s.charAt(i) == '(') {
                deque.push(s.charAt(i));
            } else {
                //检查是否双端队列是否为空
                if (deque.isEmpty()) {
                    return false;
                }
                if (Math.abs(deque.peek() - s.charAt(i)) <= 2) {
                    //括号匹配,弹栈
                    deque.pop();
                }
            }

        }
        if (deque.isEmpty()) {
            return true;
        } else {
            return false;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        s = s.substring(1, s.length() - 1);
        isValid(s);
    }
}
```

##### [1614](https://leetcode.cn/problems/maximum-nesting-depth-of-the-parentheses/)

```java
import java.util.ArrayDeque;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        //(1)+((2))+(((3))) 测试用例
        Solution solution = new Solution();
        System.out.println(solution.maxDepth(s));
    }
}

class Solution {
    public int maxDepth(String s) {
        ArrayDeque<Character> deque = new ArrayDeque<>();
        int depth = 0;
        if (s.isEmpty()) {
            return 0;
        }
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                deque.push(s.charAt(i));
                depth = Math.max(depth, deque.size());
            }
            if (!deque.isEmpty() && s.charAt(i) == ')') {
                deque.pop();
            }
        }
        return depth;
    }
}
```

##### [08.08.](https://leetcode.cn/problems/permutation-ii-lcci/)

```java
import java.util.*;

class Solution {
    int maxDepth = 0;
    List<String> L = new ArrayList<>();

    public String[] permutation(String S) {
        Map<String, Integer> M = new HashMap<>();
        for (int i = 0; i < S.length(); i++) {
            int val = M.getOrDefault(String.valueOf(S.charAt(i)), 0);
            M.put(String.valueOf(S.charAt(i)), val + 1);
        }
        maxDepth = S.length();
        DFS(M, 0, "");
        String[] ret = new String[L.size()];
        for (int i = 0; i < L.size(); i++) {
            ret[i] = L.get(i);
        }
        return ret;
    }

    private void DFS(Map<String, Integer> m, int Depth, String s) {
        if (Depth == maxDepth) {
            //执行
            L.add(s);
            return;
        }
        for (String key : m.keySet()) {
            int val = m.getOrDefault(key, 0);
            if (val > 0) {
                //若当前的val大于0则加入到字符串s中,并进入下一层
                m.replace(key, val - 1);//可使用次数减1
                DFS(m, Depth + 1, s += key);
                s = s.substring(0, s.length() - 1);
                val = m.getOrDefault(key, 0);//恢复可用次数
                m.replace(key, val + 1);//恢复可以用次数
            }
        }
    }
}


public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        Solution solution = new Solution();
        solution.permutation(str);

    }
}

// 
class Solution {
    int maxDepth = 0;
    List<String> list = new ArrayList<>();

    public String[] permutation(String S) {
        maxDepth = S.length();
        HashMap<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < S.length(); i++) {
            map.put(S.charAt(i), map.getOrDefault(S.charAt(i), 0) + 1);
        }
        DFS(map, 0);
        String[] ret = new String[list.size()];
        for (int i = 0; i < list.size(); i++) {
            ret[i] = list.get(i);
        }
        return ret;
    }

    String temp = "";

    private void DFS(HashMap<Character, Integer> map, int depth) {
        if (depth == maxDepth) {
            list.add(temp);
            return;
        }
        for (Character key : map.keySet()) {
            if (map.getOrDefault(key, 0) > 0) {
                map.replace(key, map.getOrDefault(key, 0) - 1);
                temp += key;
                DFS(map, depth + 1);
                map.replace(key, map.getOrDefault(key, 0) + 1);
                temp = temp.substring(0, depth);
            }
        }
    }
}

```

##### [77](https://leetcode.cn/problems/combinations/)

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    int maxDepth = 0;
    List<List<Integer>> ret = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    int[] used;

    public List<List<Integer>> combine(int n, int k) {
        int[] arr = new int[n];
        used = new int[n];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i + 1;
        }
        maxDepth = k;
        DFS(arr, 0, 0);
        return ret;
    }

    private void DFS(int[] arr, int depth, int begin) {
        if (depth == maxDepth) {
            ret.add(new ArrayList<>(temp));
            return;
        }
        for (int i = begin; i < arr.length; i++) {
            if (used[i] == 0) {
                used[i] = 1;
                temp.add(arr[i]);
                DFS(arr, depth + 1, i + 1);
                used[i] = 0;
                temp.remove(temp.size() - 1);
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        solution.combine(4, 2);
    }
}
```

##### [674](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

```java
import java.util.Arrays;
import java.util.OptionalInt;

class Solution {
    public int findLengthOfLCIS(int[] nums) {
        //动态规划
        int[] dp = new int[nums.length];
        dp[0] = 1;
        for (int i = 1; i < dp.length; i++) {
            dp[i] = nums[i] > nums[i - 1] ? dp[i - 1] + 1 : 1;
        }
        OptionalInt maxL = Arrays.stream(dp).max();
        return maxL.orElse(0);
    }
}

public class Main {
    public static void main(String[] args) {
        int[] nums = {1, 3, 5, 4, 7};

        Solution solution = new Solution();
        solution.findLengthOfLCIS(nums);
    }
}
```

##### [NC17](https://www.nowcoder.com/practice/b4525d1d84934cf280439aeecc36f4af?tpId=182&tqId=34752&ru=/exam/oj)

```java
class Solution {

    public int getLongestPalindrome(String A) {
        int maxl = 0;
        int[][] dp = new int[A.length()][A.length()];
        for (int l = 0; l < A.length(); l++) {
            for (int i = 0; i + l < A.length(); i++) {
                if (l == 0) {
                    dp[i][i + l] = 1;
                }
                if (l > 0 && l <= 2) {
                    dp[i][i + l] = A.charAt(i) == A.charAt(i + l) ? l + 1 : 0;
                }
                if (l > 2) {
                    dp[i][i + l] = (A.charAt(i) == A.charAt(i + l)) && (dp[i + 1][i + l - 1] != 0) ? l + 1 : 0;
                }
                if (dp[i][i + l] > 0) {
                    maxl = Math.max(maxl, l + 1);
                }
            }
        }

        return maxl;
    }
}

public class Main {
    public static void main(String[] args) {
        String A = "ababc";
        Solution solution = new Solution();
        solution.getLongestPalindrome(A);
    }
}
```

##### [NC28](https://www.nowcoder.com/practice/c466d480d20c4c7c9d322d12ca7955ac?tpId=196&tqId=37066&ru=/exam/oj)

```java
import java.util.*;


class Solution {

    public String minWindow(String S, String T) {
        if (T.length() == 1 && S.equals(T)) {
            return T;
        }
        int left = 0;
        int minl = S.length();
        String output = "";
        Map<Character, Integer> map = new HashMap<>();
        boolean isAll = false;
        for (int i = 0; i < T.length(); i++) {
            int keyVal = map.getOrDefault(T.charAt(i), 0);
            map.put(T.charAt(i), keyVal + 1);
        }
        for (int i = 0; i < S.length(); i++) {
            for (Character key : map.keySet()) {
                if (S.charAt(i) == key) {
                    map.replace(key, map.getOrDefault(key, 0) - 1);
                }
            }
            isAll = isAllPositive(map);
            while (isAll) {
                for (Character key : map.keySet()) {
                    if (S.charAt(left) == key) {
                        map.replace(key, map.getOrDefault(key, 0) + 1);
                    }
                }
                isAll = isAllPositive(map);
                if (!isAll) {
                    if (minl >= (i - left + 1)) {
                        output = S.substring(left, i + 1);
                    }
                    minl = Math.min(minl, i - left + 1);
                }
                left++;
            }
        }
        return output;
    }

    private boolean isAllPositive(Map<Character, Integer> map) {

        for (Character key : map.keySet()) {
            if (map.get(key) > 0) {
                //大于0代表T还没有被用完
                return false;
            }
        }
        return true;
    }

}

public class Main {
    public static void main(String[] args) {
//        String S = new String("XDOYEZODEYXNZ");
//        String T = new String("XYZ");
        String S = new String("aa");
        String T = new String("aa");
        Solution solution = new Solution();
        solution.minWindow(S, T);
    }
}
```

##### [HJ41](https://www.nowcoder.com/practice/f9a4c19050fc477e9e27eb75f3bfd49c?tpId=37&tqId=21264&ru=/exam/oj)

```java
import java.util.Scanner;
import java.util.TreeMap;
import java.util.TreeSet;


public class Main {
    static TreeSet<Integer> set = new TreeSet<>();
    static int maxDepth = 0;
    static int sum = 0;

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String[] m = in.nextLine().split(" ");
        String[] x = in.nextLine().split(" ");
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for (int i = 0; i < n; i++) {
            map.put(Integer.parseInt(m[i]), Integer.parseInt(x[i]));
            maxDepth += Integer.parseInt(x[i]);
        }
        DFS(map, 0);
        set.add(0);
        System.out.println(set.size());
    }

    private static void DFS(TreeMap<Integer, Integer> map, int depth) {
        if (depth == maxDepth) {
            return;
        }

        for (Integer key : map.keySet()) {
            if (map.get(key) > 0) {
                map.replace(key, map.getOrDefault(key, 0) - 1);
                sum += key;
                set.add(sum);
                DFS(map, depth + 1);
                map.replace(key, map.getOrDefault(key, 0) + 1);
                sum -= key;
            }
        }
    }
}
```

##### [HJ41](https://www.nowcoder.com/practice/f9a4c19050fc477e9e27eb75f3bfd49c?tpId=37&tqId=21264&ru=/exam/oj)

```java
import java.util.*;
//称砝码

public class Main {

    public static void main(String[] args) {
        Set<Integer> set = new LinkedHashSet<>();
        Deque<Integer> deque = new ArrayDeque<>();
        set.add(0);
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String[] m = in.nextLine().split(" ");
        String[] x = in.nextLine().split(" ");
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for (int i = 0; i < n; i++) {
            map.put(Integer.parseInt(m[i]), Integer.parseInt(x[i]));
        }
        for (Integer key : map.keySet()) {
            while (map.getOrDefault(key, 0) > 0) {
                map.replace(key, map.getOrDefault(key, 0) - 1);
                for (Integer i : set) {
                    deque.push(i + key);
                }
                set.add(key);
                while (!deque.isEmpty()) {
                    set.add(deque.peek());
                    deque.pop();
                }
            }
        }
        System.out.println(set.size());
    }
}
```

##### [BM95](https://www.nowcoder.com/practice/76039109dd0b47e994c08d8319faa352?tpId=295&tqId=1008104&ru=/exam/oj&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Foj)

```java
import java.util.*;

//分糖果问题
class Solution {

    public int candy(int[] arr) {
        // write code here
        int[] dp = new int[arr.length];
        Arrays.fill(dp, 1);
        for (int i = 1; i < arr.length; i++) {
            dp[i] = arr[i] > arr[i - 1] ? dp[i - 1] + 1 : dp[i];
        }
        for (int i = arr.length - 1; i >= 1; i--) {
            dp[i - 1] = (arr[i - 1] > arr[i]) && (dp[i - 1] <= dp[i]) ? dp[i] + 1 : dp[i - 1];
        }
        return Arrays.stream(dp).sum();
    }
}

public class Main {
    public static void main(String[] args) {
        int[] arr = {50, 49, 48, 47, 46, 45, 44, 43, 42, 41, 40, 39, 38, 37, 36, 35, 34, 33, 32, 31, 30, 29, 28, 27, 26, 25, 24, 23, 22, 21, 20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1};
//        int[] arr = {1, 4, 2, 7, 9};
        Solution solution = new Solution();

        System.out.println(solution.candy(arr));
    }
}
```

##### [#BM96](https://www.nowcoder.com/practice/4edf6e6d01554870a12f218c94e8a299?tpId=295&tqId=1267319&ru=%2Fexam%2Foj&qru=%2Fta%2Fformat-top101%2Fquestion-ranking&sourceUrl=%2Fexam%2Foj&dayCountBigMember=30%E5%A4%A9)
```java
import java.util.*;
//BM96 主持人调度（二）

class Solution {
    public int minmumNumberOfHost(int n, int[][] startEnd) {
        int[] start = new int[n];
        int[] end = new int[n];
        //分别得到活动起始时间
        for (int i = 0; i < n; i++) {
            start[i] = startEnd[i][0];
            end[i] = startEnd[i][1];
        }
        //单独排序
        Arrays.sort(start, 0, start.length);
        Arrays.sort(end, 0, end.length);
        int res = 0;
        int j = 0;
        for (int i = 0; i < n; i++) {
            //新开始的节目大于上一轮结束的时间，主持人不变
            if (start[i] >= end[j])
                j++;
            else
                //主持人增加
                res++;
        }
        return res;
    }
}

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        int n = 3;
        int[][] startEnd = {{1, 3}, {3, 5}, {2, 4}};

        solution.minmumNumberOfHost(n, startEnd);
    }
}
```

##### csv
```java
import java.util.HashMap;
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        String[] arr = str.split(",");
        //使用map,创建A~Z的键值对
//        System.out.println((int) 'A');//65
//        System.out.println((int) 'Z');//90
        HashMap<Character, String> map = new HashMap<>();
        int flag = 65;//A
        for (String string : arr) {
            map.put((char) flag++, string);
        }
        boolean isError = true;
        for (Character key : map.keySet()) {
            for (int i = 65; i < 65 + map.size(); i++) {
                String temp = "<" + (char) i + ">";
                int idx = map.get(key).indexOf(temp);
                if (idx != -1) {
                    temp = map.get(key).replace(temp, map.get((char) i));
                    map.replace(key, temp);
                    isError = false;
                    break;
                }
            }
            if ((map.get(key).indexOf('<') != -1 || map.get(key).indexOf('>') != -1) && isError) {
                System.out.println("-1");
                return;
            }
        }

        flag = 1;

        for (Character key : map.keySet()) {
            if (flag++ < map.size()) {
                System.out.print(map.get(key) + ",");
            } else {
                System.out.print(map.get(key));
            }

        }
    }
}


```


##### [46](https://leetcode.cn/problems/permutations/)
```java
import java.util.ArrayList;
import java.util.List;

class Solution {

    int[] used;
    int maxDepth = 0;
    List<Integer> temp = new ArrayList<>();
    List<List<Integer>> ret = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        used = new int[nums.length];
        maxDepth = nums.length;
        DFS(nums, 0);
        return ret;
    }

    private void DFS(int[] nums, int depth) {
        //如果当前的深度等于最大深度，则回溯
        if (depth == maxDepth) {
            ret.add(new ArrayList<>(temp));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i] == 0) {
                used[i] = 1;
                temp.add(nums[i]);//把当前的数字添加的temp中
                DFS(nums, depth + 1);
                used[i] = 0;
                temp.remove(temp.size() - 1);
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        //  测试用例   [1,2,3]
        int[] nums = {1, 2, 3};
        Solution solution = new Solution();
        List<List<Integer>> ret = solution.permute(nums);
        System.out.println(ret);
    }
}
```