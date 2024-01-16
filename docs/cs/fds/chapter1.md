# Agorithm Analysis

## Definition of Algorithm

All algorithms must satisfy the following criteria:

- Input: zero or more externally supplied values 外部提供的输入
- Output: at least one value 一个或多个输出
- Definiteness: each instruction must be clear and unambiguous 每条指令都必须清晰明确
- Finiteness: if we trace out the instructions of an algorithm, then for all cases the algorithm will terminate after a finite number of steps 对于所有情况，算法将在有限步骤后终止
- Effectiveness: every instruction must be basic enough to be carried out 每条指令都必须足够基本，能够被计算机执行

!!! note "program"
    A program is written in some programming language, and does not have to be finite.

    程序是用某种编程语言编写的，不一定有限。

## Algorithm Analysis

### 分析内容

- run times：运行时间
- time and space complexity：时间和空间复杂度

一般分析 \(T_{avg}(N)\) 和 \(T_{worst}(N)\) ，即平均时间复杂度和最坏时间复杂度。

### Asymptotic Notation

- \(T(N) = O(f(N)) \Rightarrow T(N) \leq cf(N)\)
- \(T(N) = \Omega(f(N)) \Rightarrow T(N) \geq cf(N)\)
- \(T(N) = \Theta(f(N)) \Rightarrow T(N) = O(f(N)) \land T(N) = \Omega(f(N))\)
- \(T(N) = o(f(N)) \Rightarrow T(N) = O(f(N)) \land T(N) \neq \Omega(f(N))\)

Rules:

- 如果 \(T_1(N) = O(f(N))\) 且 \(T_2(N) = O(g(N))\) ，则
    - \(T_1(N) + T_2(N) = \max(O(f(N)), O(g(N)))\)
    - \(T_1(N) \cdot T_2(N) = O(f(N) \cdot g(N))\)
- 如果 \(T(N)\) 是一个 \(k\) 次多项式，则 \(T(N) = \Theta(N^k)\)
- \(\log_k N = O(N)\)，logarithms grow very slowly 对数增长非常缓慢

### Logarithms

!!! note "Eucild's Algorithm"

    ```c
    int gcd(int a, int b) {
        if (b == 0) return a;
        else return gcd(b, a%b);
    }
    ```

!!! note "Expontentiation 快速幂"

    递归版本（无记忆化）：

    ```c
    int mypow(int a, int b) {
        if (b == 0) return 1;
        else if (b & 1) return a * mypow(a, b-1);
        else return mypow(a, b/2) * mypow(a, b/2);
    }
    ```

    迭代版本：

    ```c
    int mypow(int a, int b) {
        int res = 1;
        while (b) {
            if (b & 1) res *= a;
            a *= a;
            b >>= 1;
        }
        return res;
    }
    ```

### Checking Analysis

- 若 \(T(N) = O(N^k)\) ，则有

    \[\frac{T(2N)}{T(N)} \approx 2^k\]

- 若 \(T(N) = O(f(N))\) ，则有

    \[\lim_{N \to \infty} \frac{T(N)}{f(N)} = constant\]

## 错题整理

!!! question "Question 1"

    The recurrent equations for the time complexities of programs P1 and P2 are:

    - P1: \(T(1)=1,T(N)=T(N/3)+1\)
    - P2: \(T(1)=1,T(N)=3T(N/3)+1\)

    Then the correct conclusion about their time complexities is ______

    !!! note "Answer"

        - P1: 

            \[
            \begin{aligned}
            T(N) &= T(N/3) + 1 \\
            &= T(N/9) + 2 \\
            &= T(1) + \log_3 N \\
            &= O(\log_3 N)
            \end{aligned}
            \]

        - P2:

            \[
            \begin{aligned}
            T(N) &= 3T(N/3) + 1 \\
            &= 9T(N/9) + 3 + 1 \\
            &= 3^{\left\lfloor \log_3 N \right\rfloor} T(1) + 3^{\left\lfloor \log_3 N \right\rfloor - 1} + \cdots + 3^0 \\
            &= O(N) 
            \end{aligned}   
            \]