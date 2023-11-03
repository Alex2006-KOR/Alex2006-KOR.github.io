---

layout:   post
title:    Recursion
date:     2020-09-26
image:    algorithm/algorithm.png
category: Algorithm
tags:     Recursion
author:   Alex2006

---

## Recursion
* 자기 자신을 호출(재귀호출)
{% highlight cpp %}
void func(...) {
    func(...); // Infinite loop에 빠지지 않도록 최소 1개의 Base case(기저)가 있어야 한다.
}
{% endhighlight %}
 
## Recursion의 구성
* Base case: 적어도 하나의 Recursion에 빠지지 않는 Case가 존재해야 한다.
* Recursion case: Recursion을 수행하다 보면 결국 Base case로 수렴 해야한다.
{% highlight c %}
void func(int k) {
    if (k <= 0) {
        return;      // Base case
    }
    else {
        printf("%d", k);
        func(k - 1); // Recursion
    }
}
{% endhighlight %}    

* Example) 1~n까지의 수 더하기
  * n = 0 인 경우 0을 return한다.
  * 임의의 양의 정수 k에 대해서 n < k 인 경우 0~n 까지의 합을 올바르게 계산한다고 가정.
  * n = k 인 경우 func(k-1)을 호출 → 2.의 가정에 의해 0~k-1까지 합을 올바르게 구한다.
  * func는 이 값에 n을 더해 반환하므로 OK.
{% highlight cpp %}
int func(int n) {               // 이 함수의 Missuion은 1~n 까지의 합을 구하는 것이다.
   if(n == 0) return 0;         // n이 0이라면 합은 0이다.
   else return n + func(n - 1); // n이 0보다 크면 0~n까지의 합은 0~n-1까지의 합 + n 이다.
}
{% endhighlight %}

* Example) Factorial
  * 0! = 1
  * n! = n*(n - 1)! (n > 0)
{% highlight cpp %}
int factorial(int n) {
   if(n == 0) return 1;
   else return n * factorial(n - 1);
}
{% endhighlight %}

* Example) Fibonachi
  * f<sub>0</sub> = 0
  * f<sub>1</sub> = 1
  * f<sub>2</sub> = f<sub>n-1</sub> + f<sub>n-2</sub>
{% highlight cpp %}
int fibonachi(int n) {
   if(n < 2) return n;
   else return fibonachi(n - 1) + fibonachi(n - 2);
}
{% endhighlight %}

* Example) G.C.D
  * m >= n 인 두 양수 m, n에 대하여 m이 n의 배수이면 GCD(m, n) = n
  * 아니라면 GCD(m, n) = GCD(n, m%n) 이다.
  * p (q == 0)
  * GCD(q, p%q) otherwise
{% highlight c %}
   if(q == 2) return p;
   else return GCD(q, p%q);
   
   if(m < n) swap(m, n);
   if(m%n == 0) return n;
   else return GCD(n, m%n);
{% endhighlight %}

## Recursive thinking
* Example) 문자열 계산하기
  * "[0] + [1]~~[n]" == "1  + 문자열 길이" == "총 문자열의 길이"
  * Idea : 첫번째 글자부터 하나씩 차례대로 세어 나간다. == 총 문자열의 길이는 첫번째 문자를 제외한 문자열 길이 + 1
{% highlight c %}
int StringLength(string str) {
   if(str == "") return 0;
   else return StringLength(Substr(str, 1)) + 1;
}
{% endhighlight %}

## Recursion ↔ Iteration
* 모든 Recursion은 Iteration으로 변경 가능
* 그 역도 성립하므로 즉 Iteration은 Recursion으로 변경 가능
* <span style="color:blue">알고리즘을 단순하고 명쾌하게 표현가능하나 함수 호출에 대한 Overhead가 발생.</span>
* Example) 배열탐색 - iteration case
  * 데이터의 범위에 대해 시작 index 0은 생략(n만 존재) → 암시적 매개변수
{% highlight c %}
int search(int[] data, int n, int target) {
   for(int i=0; i<n; i++)
      if(data[i] == target) return i;
   return -1;
}
{% endhighlight %}

* 배열탐색 - recursion case
  * 데이터의 범위에 대해 시작(begin)과 끝(end)을 명시 → 명시적 매개변수
{% highlight c %}
int search(int[] data, int begin, int end, int target) {
   if(begin > end) return -1;
   else if(target == data[begin]) return begin;
   else return search(data, begin + 1, end, ratget);
}
{% endhighlight %}

## Designing recursive
* <span style="color:red">적어도 하나의 Base case,</span> 즉 순환되지 않고 종료되는 Case가 존재해야 한다.
* 모든 Case는 결국 Base case로 <span style="color:red">수렴</span>해야 한다.
* <span style="color:blue">암시적(Implicit) 매개변수를</span><span style="color:red"> 명시적(explicit)으로 바꾸어 사용한다.</span>
* Example) Binary search
{% highlight c %}
int BinarySearch(int[] items, int begin, int end, int target) {
   if(begin > end) return -1;
   else {
      int mid = (begin + end) / 2, cmpRes = target.CompareTo(items[mid]);
      
      if(cmpRes ==0) return mid;
      else if(cmpRes < 0) return binarySearch(items, begin, mid-1, target);
      else return bianrySearch(items, mid+1, end, target);
   }
}
{% endhighlight %}

## Example
### Maze
<center><img src="/images/algorithm/20200926/maze.jpg"></center>
* Yellow: Start, Green: End, Gray: Wall, White: Pathway
* Start에서 End까지 Path가 존재하는지를 Return

#### Recursive Thinking
* 현재 위치가 출구 이거나
* 이웃한 셀들 중 하나에서 현재 위치를 지나지 않고 출구까지 가는 경로가 있거나

#### Pseudo Code
{% highlight c++ %}
//잘못된 코드 버전 : 두가지가 결여되어 있다 (1)  Basecase, (2) Basecase로의 수렴 
bool findPath(x, y)
    if (x, y) is the exit then
        return TRUE
    else
        foreach neighboring cell (x_a, y_a) of (x, y) do
            if (x_a, y_a) is on the pathway
                if findPath (x_a, y_a) then
                    return TRUE
    return FALSE
//수정된 버전
bool findPath(x, y)
    if (x, y) is either on the wall or visited cell then
        return FALSE
    else if (x, y)is exit then
        ... ...
    else
        mark(x, y) as a visited cell
        foreach neighboring cell (x_a, y_a) of (x, y) do
        ... ...
{% endhighlight %}

### Counting cells in a blob
<center><img src="/images/algorithm/20200926/blob_1.jpg"></center>

* Binary image에 대해 각 픽셀은 background color이거나 image color이다.
* 서로 연결된 image 픽셀들의 집합을 Blob이라고 하며 상하좌우 및 대각방향으로도 연결된 경우도 Blob에 해당
* 입력 - N * N 크기의 이차원 Grid, 하나의 좌표(x, y)
* 출력 - 픽셀(x, y)가 포함된 blob의 크기, 픽셀이 어떠한 blob에도 속하지 않으면 0
<center><img src="/images/algorithm/20200926/blob_2.jpg"></center>

#### Recursive Thinking
* 현재 픽셀이 image color가 아니라면 return 0
* 현재 픽셀이 image color라면
* 먼저 현재 픽셀을 카운트 한다.
* 현재 픽셀에 대한 중복 카운트를 방지하기 위해 다른 색으로 칠한다.
* 현재 픽셀에 이웃한 8개 픽셀들에 대해 <span style="color:red">그 픽셀이 속한 blob의 크기를 카운트 하여 카운터에 더한다.</span>

#### Pseudo Code
{% highlight c++ %}
algorithm_for_counting_blob(x, y)
    if the pixel(x, y) is outside the grid then the result is 0
    else if pixel(x, y) is not an image pixel or already counted then the result is 0
    else
        set the color of the pixel(x, y) to a red color
        the result is 1 plus the number of cells in each place of the blob includes a nearst neighbor
{% endhighlight %}

### N-Queens
<center><img src="/images/algorithm/20200926/queen_1.jpg"></center>

* Problem - N x N 체스 보드 위에 1행 마다 1개씩 N개의 말을 놓는데, 각 말에서 Queen 이동범위 내에 다른 말이 없도록 한다.

#### Recursive(Backtracking) Thinking
<center><img src="/images/algorithm/20200926/queen_2.jpg"></center>
* 3번째 행에 말을 놓을 수 없으므로 가장 최근 결정인 2행을 변경

<center><img src="/images/algorithm/20200926/queen_3.jpg"></center>
* 4번째 행에 말을 놓을 수 없으므로 가장 최근 결정인 3행을 변경  
  2번째 취소 → 1번째 취소

<center><img src="/images/algorithm/20200926/queen_4.jpg"></center>
* N-Queen 완성
  * 각 선택에서 결정을 내리며 진행하다가 막히게 되면 가장 최근의 결정부터 새로운 선택을 시도/반복
  * 상태 공간 트리로 표현이 가능
  * non-promising / infeasible : 모든 노드를 탐색하지 않아도 답을 구 할 수 있다.

<center><img src="/images/algorithm/20200926/queen_5.jpg"></center>

#### Recursion Design
{% highlight c++ %}
return_type Queens (Arguments) //
{
    if non-promising
        report failure or return FALSE // 조건에 맞지 않는 노드라면
    else if success
        report answer or return TRUE // 이 노드가(도달한) 정답이라면
    else
        visit child recursively                 // recursive 하게 자식 방문
}
{% endhighlight %}

#### Pseudo Code
{% highlight c++ %}
// Global variable : Chess board
int [] cols = new int[N+1]
{% endhighlight %}
{% highlight c++ %}
boolean  Queens (int level) // Arguments : level → cols[level] == level 행에 놓인 말 위치
    if (!Promising(level))
        return FALSE
    else if  (level == N)
        return TRUE

    for (int i = 1; i <= N; i++)
        cols[level+1] = i
        if (Queens(level + 1))
            return TRUE

    return FALSE
{% endhighlight %}
{% highlight c++ %}
boolean  promising (int level)
    for (int i = 1; i < level; i++)
        if (cols[i] == cols[level])
            return FALSE
        else if (level-i == |cols[level] - cols[i]|)
            return FALSE

    return TRUE
{% endhighlight %}

### Power set
## Problem
 * [ACMICPC : 하노이의 탑 이동순서](https://www.acmicpc.net/problem/11729)
 * [ACMICPC : N-Queen](https://www.acmicpc.net/problem/3344)
 * [ACMICPC : 자리배정](https://www.acmicpc.net/problem/10157)
 * [LEETCODE : Add two number](https://leetcode.com/problems/add-two-numbers/description/)
 * [LEETCODE : N-th Tribonacci](https://leetcode.com/problems/n-th-tribonacci-number/description/)
**<center><span style="color:navy">Recursion</span></center>**
