## 递归算法题
```java
https://www.cnblogs.com/aismvy/p/6361636.html

import java.util.Scanner;

public class Main{
	public static int fn(int m,int n) {
		if (m<n)
		{
			return 0; //当借出数量<归还时 
		}
		if (n==0)
		{
			return 1;
		}
		return fn(m-1,n)+fn(m,n-1); //fun(m-1,n)和fun(m,n-1)表示下一次借鞋子和还鞋子的数量
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int m = sc.nextInt();
		int n = sc.nextInt();
		System.out.println(fn(m,n));
		
		sc.close();
	}

}
```
计算亲和数：
```
import java.util.Scanner;

public class Main{

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int num = sc.nextInt();
		int i=0,temp=1,sum =0,sum2=0;
		int[][] arr = new int[num][2]; 
		for(;i<num;i++) {
			arr[i][0] = sc.nextInt();
			arr[i][1] = sc.nextInt();
		}
		for(i= 0;i<num;i++) {
			while(temp <= arr[i][0] / 2) {
				if(arr[i][0] % temp == 0) {
					sum += temp;
				}
				temp ++;
			}
			temp =1;
			while(temp <= arr[i][1] / 2) {
				if(arr[i][1] % temp == 0) {
					sum2 += temp;
				}
				temp ++;
			}
			if(sum == arr[i][1] && sum2 == arr[i][0]) {
				System.out.println("YES");
			}else {
				System.out.println("NO");
			}
			temp =1;sum =0;
		}
		
		sc.close();
	}

}
```

最小公倍数
```
import java.util.Scanner;

public class Main{

	public static void main(String[] args) {
		int a,b,c,d,e;
		Scanner sc = new Scanner(System.in);
		a = sc.nextInt();
		b = sc.nextInt();
		c = a; d = b;
		while(c % d !=0) {
			e = c % d;
			c = d;
			d = e;
		}
		System.out.println(a*b/d);
		sc.close();
	}

}
```
斐波拉契数列
```
import java.util.Scanner;

public class Main{
	public static int fn(int x) {
		if(x ==0 || x == 1) {
			return 1;
		}
		return fn(x-1) + fn(x-2);
		
	}
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int num = sc.nextInt();
		for(int i=0;i<num;i++) {
			System.out.print(fn(i)+" ");
		}
		sc.close();
	}

}
```
质数判断：
```
import java.util.Scanner;

public class Main{
	public static void main(String[] args) {
		int sqr = 1;
		Scanner sc = new Scanner(System.in);
		int num = sc.nextInt();
		if(num < 2) {
			System.out.println("N");
			return ;
		}
		while(sqr * sqr < num) {
			sqr ++;
		}
		for(int i=2;i<=sqr;i++) {
			if(num % i == 0) {
				System.out.println("N");
				return ;
			}
		}
		System.out.println("Y");
		sc.close();
	}

}

```
计算t=1+1/2+...+1/n 
```
import java.util.Scanner;

public class Main{

	public static void main(String[] args) {
		double t=0;
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		for(int i=1;i<=n;i++) {
			t += 1.0/i;
		}
		System.out.printf("%.6f",t);
		sc.close();
	}

}
```
求对角线元素和
```
import java.util.Scanner;

public class Main{

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int[][] a = new int[n][n];
		int i,j;
		int sum = 0; 
		for(i=0;i<n;i++) {
			for(j=0;j<n;j++) {
				a[i][j] = sc.nextInt();
			}
		}
		for(i=0;i<n;i++) {
			sum += a[i][i]+a[i][n-i-1]; 
		}
		if(n%2 != 0) {
			sum -= a[n/2][n/2];
		}
		System.out.println(sum);
		sc.close();
	}

}
```

求多行数据的最大值https://oj.css.dgut.edu.cn/problem/23
```
```
上下车人数
```
import java.util.Scanner;

public class Main{
	public static void main(String[] args) {
		int a,n,m,x;
		Scanner sc = new Scanner(System.in);
		a = sc.nextInt();
		n = sc.nextInt();
		m = sc.nextInt();
		x = sc.nextInt();
		int[] m1 = new int[n + 2];// 上车
        int[] m2 = new int[n + 2];// 下车
        int[] m3 = new int[n + 2];// 车上人数
        m1[1] = a;
        m2[1] = 0;
        m3[1] = a;
        int i;
        for (i = 0;; i++) {
            m1[2] = i;
            m2[2] = i;
            m3[2] = m1[2] - m2[2];
            for (int j = 3; j < n; j++) {
                m1[j] = m1[j - 2] + m1[j - 1];
                m2[j] = m1[j - 1];
                m3[j] = m1[j] - m2[j];
            }
            int sum = 0;
            for (int k = 1; k < n; k++) {
                sum += m3[k];
            }
            if (sum == m)
                break;
        }
        int su = 0;
        for (int l = 1; l <= x; l++) {
            su += m3[l];
        }
        System.out.println(su);
		
		sc.close();
	}

}

```

水仙花数：
```
import java.util.Scanner;

public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int num = sc.nextInt();
		int sum = 0,temp=num,i;
		do{
			i = temp % 10;
			sum += i * i * i;
			temp = temp/10;
		}while(temp !=0);
		if(sum == num) {
			System.out.println(1);
		}else {
			System.out.println(0);
		}
		sc.close();
	}

}
```

