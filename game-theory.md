# Lý thuyết trò chơi

Người viết: Nguyễn Nhật Minh Khôi

Trong thực tế, có rất nhiều loại trò chơi khác nhau, tuy nhiên trong bài viết này, chúng ta sẽ chỉ giới hạn trong các **trò chơi tổ hợp cân bằng** vì độ phổ biến của nó trong lập trình thi đấu.

## Giới thiệu: trò chơi bốc sỏi
Trước khi đi vào các định nghĩa, chúng ta sẽ đi qua một ví dụ để hiểu được vấn đề mà lý thuyết trò chơi với trò chơi tổ hợp cân bằng giải quyết.

### Phát biểu trò chơi
Hai bạn An và Bình đang chơi bốc sỏi, trò chơi được phát biểu như sau: cho một đống sỏi có $n$ viên sỏi, hai bạn sẽ luân phiên bốc sỏi từ đống sỏi, mỗi lượt chỉ được bốc $1$, $2$ hoặc $3$ viên sỏi, người lấy được viên sỏi cuối cùng sẽ là người chiến thắng. Biết rằng An đi trước, Bình đi sau. Hãy tìm một chiến thuật để An có thể chiến thắng trò chơi.

### Cách giải
Ta sẽ tiếp cận bài toán bằng cách giải những trường hợp đơn giản trước, sau đó rút ra quy luật chung cho trường hợp tổng quát.

Với $n = 1, 2$ hoặc $3$, An luôn chiến thắng bằng cách lấy hết sỏi trong lượt bốc đầu tiên.

Với $n = 4$, An không thể lấy hết sỏi trong lần bốc đầu tiên vì số sỏi được lấy tối đa mỗi lần chỉ là $3$. Nếu Bình là một người thông minh và luôn biết được những nước đi tối ưu, An sẽ *luôn thua* do:
- Nếu An chọn lấy $1$ viên, số sỏi còn lại là $n' = 3$, lúc đó Bình có thể dùng chiến thuật của An ở trên, đó là lấy hết sỏi để chiến thắng.
- Tương tự, nếu An chọn lấy $2$ hoặc $3$ viên thì bạn cũng sẽ thua.

Do đó, trong trường hợp này An nên cố gắng kéo dài trò chơi bằng cách chỉ bốc $1$ viên sỏi và hi vọng Bình sẽ phạm sai lầm. Tuy nhiên như đã nói, nếu Bình là một người chơi có chiến thuật tối ưu thì An sẽ luôn thua. Từ trường hợp này, ta thấy trường hợp $n = 4$ là một trường hợp "xấu" do nó đặt người ở lượt đi hiện tại vào thế bất lợi.

Từ nhận xét trên, ta thấy nếu $n = 5,6,7$, An luôn có thể thắng bằng cách lấy sỏi sao cho số sỏi còn lại là $n'=4$, mục đích là để ép Bình vào trường hợp bất lợi (cũng là có lợi cho An).

Ta rút ra được một quy luật của trò chơi: nếu ở lượt của ai mà số sỏi là $4, 8, 12,\ldots$ hay nói cách khác $n$ chia hết cho $4$ thì người đó sẽ ở vị trí bất lợi.

Vậy chiến thuật tối ưu của An dựa trên số sỏi hiện tại $m$ sẽ là:
- Nếu $m \bmod 4 \neq 0$ thì An sẽ bốc $m - (m \bmod 4)$ viên sỏi để ép Bình vào trường hợp bất lợi $m' \bmod 4 = 0$.
-  Nếu $m \bmod 4 = 0$ thì An sẽ bốc $1$ viên sỏi để kéo dài trò chơi và chờ sai lầm của Bình.

với $\bmod$ là phép lấy phần dư.

## Trò chơi tổ hợp cân bằng
Trò chơi ở trên chính là một ví dụ điển hình cho trò chơi tổ hợp cân bằng. Trong phần này, chúng ta sẽ tổng quát hóa bài toán này thành định nghĩa trò chơi tổ hợp cân bằng để thể giải được một lớp bài toán lớn hơn.

### Định nghĩa
**Trò chơi tổ hợp** là trò chơi gồm: *hai người chơi (ở đây gọi người chơi trước là $A$ và người chơi sau là $B$)*, một *tập **hữu hạn** các trạng thái* $S$ (viết tắt của State) có thể đạt được của trò chơi. Mỗi người chơi có một *tập các bước di chuyển hợp lệ* $Q$ để di chuyển từ trạng thái này sang trạng thái khác (gọi là luật chơi) và một tập các trạng thái kết thúc gọi là $T \subset S$ (viết tắt của Terminal). Hai người chơi sẽ luân phiên di chuyển từ trạng thái này sang trạng thái khác. Người đến được trạng thái kết thúc trước sẽ là người chiến thắng.

> Trong trò chơi ví dụ, giả sử $n = 8$ thì mỗi trạng thái sẽ là số sỏi còn lại hiện tại của trò chơi. Do đó tập trạng thái của trò chơi là $S = \{0,1,\ldots, 8\}$ (hình dưới).
![](https://i.imgur.com/4GZXYoh.png)
> Giả sử đang ở trạng thái $x = 7$, ta có thể di chuyển hợp lệ đến trạng thái $x' = 6$ (lấy ra $1$ viên sỏi), $x' = 5$ (lấy ra $2$ viên sỏi) hoặc $x' = 4$ (lấy ra $3$ viên sỏi). Do đó ta có các phần tử $(7, 6), (7,5), (7,4)$ thuộc tập di chuyển hợp lệ $Q$ (hình dưới).
![](https://i.imgur.com/q7k7TVn.png)
> Từ đó, ta nhận xét được tập các bước di chuyển hợp lệ $Q$ của cả hai người chơi sẽ là tất cả những cặp số nguyên $(x,x - c)$ ($0 \leq x, y \leq n$) sao cho $c \in \{1, 2, 3\}$ (từ trạng thái có số sỏi $x$ chỉ có thể lấy ra $1$, $2$, hoặc $3$ viên sỏi) và $x - c \geq 0$ (số sỏi lấy ra không được phép lớn hơn số sỏi đang có).
> 
> Trò chơi kết thúc khi không còn viên sỏi nào để bốc, do đó tập trạng thái kết thúc của cả hai người chơi là $T = \{0\}$. Khi đó người bốc viên sỏi cuối cùng sẽ là người thắng.

Khi hai người chơi có tập các bước di chuyển hợp lệ $Q$ và tập trạng thái kết thúc $T$ giống nhau thì trò chơi được gọi là **trò chơi tổ hợp cân bằng**. Nói cách khác, hai người chơi chỉ khác nhau ở đúng một điểm là người này được đi trước, người kia đi sau.
> Trò chơi bốc sỏi ở ví dụ chính là một trò chơi tổ hợp cân bằng, do cả An và Bình đều chỉ được cho phép bốc $1$, $2$, hoặc $3$ viên sỏi một lần và đều thắng khi bốc được viên sỏi cuối cùng.

### Chiến thuật thắng
Mục tiêu của chúng ta khi giải các bài toán với trò chơi tổ hợp cân bằng là tìm ra chiến thuật mà ở đó một trong hai người chơi luôn có thể ép người còn lại thua. Chiến thuật như vậy gọi là một **chiến thuật thắng**.

Rõ ràng khi phân tích trò chơi bốc sỏi, ta thấy rằng nếu cả An và Bình đều chơi tối ưu thì chỉ cần biết được trạng thái ban đầu của trò chơi, ta có thể xác định được người đi trước (An) sẽ thắng hay thua. Nếu số sỏi ban đầu là $n$ chia hết cho $4$, Bình chắc chắn sẽ thắng do bạn sẽ luôn ép An đi tới trạng thái có $n$ chia hết cho $4$ và cuối cùng là $n = 0$ (cũng chia hết cho $4$). Lập luận tương tự, nếu $n$ không chia hết cho $4$ thì chắc chắn An sẽ thắng.

Vậy từ đây, ta thấy với một trò chơi tổ hợp cân bằng, chỉ cần biết trạng thái ban đầu, ta có thể suy ra được ai sẽ là người chiến thắng. Để phục vụ cho việc đưa ra chiến thuật thắng, người ta phân loại các trạng thái cùa trò chơi thành 2 tập $N$ và $P$:
- $N$: tập các trạng thái $x \in S$ sao cho nếu trạng thái ban đầu của trò chơi là $x$ thì **người chơi trước** luôn chiến thắng.
- $P$: tập các trạng thái $x \in S$ sao cho nếu trạng thái ban đầu của trò chơi là $x$ thì **người chơi sau** luôn chiến thắng.
> Trong trò chơi ví dụ, nếu số sỏi ban đầu là $n = 8$ thì 
> - $N = \{1, 2, 3, 5, 6, 7\}$
> - $P = \{0, 4, 8\}$

Từ định nghĩa trên, $N$ và $P$ sẽ thỏa ba tính chất sau:
1. Tập $P$ phải chứa toàn bộ trạng thái kết thúc.
2. Với mỗi trạng thái $s$ thuộc tập $N$, **tồn tại** ít nhất một thái $s'$ đến được từ $S$ mà thuộc tập $P$.
3. Với mỗi trạng thái $s$ thuộc tập $P$, **tất cả** các trạng thái $s'$ đến được từ $s$ phải thuộc tập $N$. 

Tới đây, có thể bạn sẽ đặt ra câu hỏi: Liệu mọi trạng thái của trò chơi đều thuộc một trong hai tập $N$ hay $P$? Ta có thể chứng minh dễ dàng bằng quy nạp mạnh theo số bước tối đa để đạt tới trạng thái kết thúc. Nếu muốn, bạn đọc có thể tham khảo thêm ở phần [1.1. Impartial game - Game Theory, Alive](https://homes.cs.washington.edu/~karlin/GameTheoryBook.pdf).

Ràng buộc đầu tiên xác định trường hợp cơ bản nhất. Hai ràng buộc sau sẽ giúp chúng ta liên tục đệ quy từ trường hợp cơ bản để xây dựng được tập $P$ và $N$ hoàn chỉnh. Ta sẽ thấy rõ điều này ở phần **Thuật toán xác định tập $P$ và $N$**.

Khi xây dựng được tập $P$ và $N$ theo các ràng buộc trên, ta có thể dễ dàng xây dựng chiến thuật thắng cho người chơi $A$ như sau (do đây là trò chơi tổ hợp cân bằng nên chiến thuật thắng của $B$ cũng sẽ tương tự):
1. Nếu $A$ **bắt đầu ở trạng thái thuộc $N$**, **luôn đi tới trạng thái thuộc $P$** để ép $B$ đi vào trạng thái thuộc $N$. Do người thắng là người đi vào trạng thái kết thúc, mà trạng thái kết thúc lại thuộc $P$ nên chắc chắn $A$ sẽ thắng.
2. Nếu $A$ bắt đầu ở trạng thái thuộc $P$, $A$ chỉ có thể kéo dài ván đấu và đợi sơ hở của $B$ ($B$ đi vào vị trí thuộc $N$) và dùng chiến thuật thắng ở trường hợp đầu. Tuy nhiên nếu $B$ cũng chơi tối ưu thì chắc chắn $A$ sẽ thua.

Qua đó, ta thấy được ý nghĩa của việc đặt tên tập $P$ và $N$. $P$ là viết tắt của *positive*, đặt tên như vậy vì người nào đi đến trạng thái này sẽ có lợi, đó là lý do tại sao các trạng thái kết thúc lại thuộc $P$, vì người chơi nào đi tới được trạng thái kết thúc sẽ có lợi (thắng trò chơi). Tương tự, $N$ là viết tắt của *negative*, đặt tên như vậy vì người nào đi đến trạng thái thuộc tập $N$ sẽ bị bất lợi (vì người kia sẽ luôn tìm cách đi được vào trạng thái thuộc $P$).

### Thuật toán xác định tập P và N
Ta có ý tưởng thuật toán để tìm tập $P$ và $N$ như sau:
```
// Hàm kiểm tra một trạng thái thuộc P (true) hay N (false)
bool isInP(State u) {
    if (u in T) // nếu u là trạng thái kết thúc thì u thuộc P
        return true;
    
    // duyệt qua tất cả các trạng thái v trong tập hợp S
    for (State v in S) 
        if (u, v) in Q and isInP(v)
            return false; // nếu có v thuộc P thì u thuộc N
    
    return true; // nếu không thì u thuộc P
}
```
Ý tưởng ở đây chính là đệ quy:
1. Nếu trạng thái $u$ đang xét là trạng thái kết thúc thì thêm $u$ vào tập $P$. Đây chính là trường hợp cơ bản của quá trình đệ quy.
2. Nếu $u$ không là trạng thái kết thúc thì đệ quy đến những trạng thái $v$ mà từ $u$ có thể di chuyển trực tiếp đến. Nếu có bất kỳ $v$ nào thuộc tập $P$ thì $u$ thuộc tập $N$, ngược lại (tất cả $v$ đều thuộc $N$) thì $u$ thuộc $P$.

Ở đây có một chú ý về cách cài đặt trên, đó là kiểu dữ liệu `State` là một kiểu dữ liệu tượng trưng để lưu một trạng thái của trò chơi. Trong thực tế, trạng thái hay được biểu diễn bằng một số nguyên int, tuy nhiên nhiều trường hợp trạng thái của chúng ta không ở dạng số nguyên, khi đó có một số cách các bạn có thể xem xét để lưu trạng thái như: bitmask, hashmap (unordered_map trong C++).

Tuy nhiên, thuật toán này có một nhược điểm, đó là **gọi lại cùng một giá trị quá nhiều lần** do không lưu lại kết quả trong quá trình đệ quy. Cách giải quyết chính là quy hoạch động top-down (hay còn gọi là đệ quy có nhớ), trong đó ta có một mảng $dp$ lưu lại kết quả của những trạng thái đã từng xét. Để hiểu thêm, bạn hãy xem cài đặt tìm trò chơi bốc sỏi ở phần ví dụ sau.

### Ví dụ cài đặt thuật toán cho trò chơi bốc sỏi
Đầu tiên, ta cần thêm các thư viện cần thiết, cũng như khai báo mảng $dp$ để nhớ kết quả của những trạng thái đã đệ quy.
```C++
#include<bits/stdc++.h>

using namespace std;

const int MAX_N = 100000; // số trạng thái tối đa
int dp[MAX_N];
```

Sau đó, ta viết hàm xác định xem trạng thái $u$ có thuộc $P$ không như sau
```C++
bool isInP(int u) {
    if (dp[u] != -1) // nếu u đã được tính trước đó thì
        return dp[u];
    
    if (u == 0) { // u = 0 là trạng thái kết thúc nên thuộc P
        dp[u] = 1;
        return 1;
    } 
        
    // Từ u chỉ có thể đi tới các v hợp lệ
    for (int v = u - 1; v >= max(u - 3, 0); v--)
        if (isInP(v)) {
            dp[u] = 0;
            return false; 
        }
    
    // u không đi được trạng thái nào thuộc P
    dp[u] = 1;
    return true;
}
```

Ở đây quy ước giá trị của mảng $dp$ như sau:
- $dp[u] = 0$ nghĩa là trạng thái $u$ đã xét và $u$ không thuộc $P$, hay nói cách khác $u$ thuộc $N$.
- $dp[u] = 1$ nghĩa là trạng thái $u$ đã xét và $u$ thuộc $P$.
- $dp[u] = -1$ nghĩa là trạng thái $u$ chưa xét.

Sở dĩ ta quy ước như vậy vì C++ có cơ chế [implicit casting](https://www.cplusplus.com/doc/oldtutorial/typecasting/). Nói đơn giản, khi trả về $dp[u]$ trong hàm `isInP(u)`, $dp[u]$ sẽ được tự động ép kiểu về `bool` với quy tắc: $0$ là $\texttt{false}$, các giá trị khác là $\texttt{true}$.

Cài đặt này tối ưu thời gian đệ quy bằng hai ý sau: 
1. Khi tính xong, trước khi trả về giá trị thì ta lưu giá trị lại vào một mảng $dp$ và ở đầu hàm $isInP(u)$
2. Ta trả về $dp[u]$ nếu $dp[u] \neq -1$ (tức $u$ đã được tính trước đó).

Cuối cùng, ta viết hàm main để nhập xuất và kết luận nghiệm. Lưu ý trước khi thực hiện quá trình tìm tập $P$ và $N$ ta phải khởi tạo mảng $dp$ thành toàn $-1$ bằng hàm `fill` do thư viện chuẩn của C++ cung cấp.
```C++
int main() {
    int n;
    
    cin >> n;
    
    fill(dp, dp + n + 1, -1);
    if (!isInP(n)) cout << "A win";
    else cout << "B win";
    
    return 0;
}
```

## Trò chơi Nim chuẩn
Sau khi đã tìm hiểu lý thuyết trò chơi tổ hợp cân bằng tổng quát, phần tiếp theo ta sẽ áp dụng lý thuyết đó vào một bài toán kinh điển trong lý thuyết trò chơi - trò chơi Nim - đại diện tiêu biểu cho trò chơi tổ hợp cân bằng.

### Phát biểu trò chơi
Có $n$ đống sỏi, và mỗi đống lần lượt có $p_1, p_2, \ldots, p_n$ sỏi, trong đó mỗi số $p_i$ là một số nguyên không âm. Mỗi trạng thái trò chơi tương ứng với mỗi bộ $n$ cho biết số sỏi của từng đống này.

Có hai người chơi thay phiên nhau bỏ sỏi. Người chơi trong lượt hiện tại có thể loại bỏ bao nhiêu sỏi tùy thích, miễn là tất cả các sỏi đều cùng một đống. Hình thức hơn, người chơi sẽ chọn một đống sỏi $i$ và số lượng sỏi $s$ để loại bỏ khỏi đống ($0 < s \leq p_i$), sau đó thay $p_i$ bằng $p_i-s$. Chú ý rằng người chơi không thể bỏ qua lượt của mình mà không bỏ viên sỏi nào cả. Sau đó, đến lượt người chơi kia loại bỏ sỏi, lần lượt như vậy tới khi trò chơi kết thúc.

Trò chơi kết thúc khi không còn sỏi trên bàn chơi. Người chiến thắng là người lấy được viên sỏi cuối cùng. Nói cách khác, người thua cuộc là người đầu tiên không thể lấy sỏi trong lượt của mình.

Trò bốc sỏi ở ví dụ trên chính là biến thể của trò chơi Nim chuẩn, khác hai chỗ là ở đây chỉ có một đống sỏi ($n = 1$) và số sỏi lấy ra tối đa ở mỗi đống là $3$.

### Giá trị Nim
Mục tiêu lớn nhất của chúng ta vẫn là xác định xem ai là người thắng nếu cả hai người chơi tối ưu, hay nói cách khác là xác định được chiến thuật thắng.

Tuy nhiên, khi bắt tay vào làm, bạn sẽ gặp ngay một rào cản, đó là trạng thái của trò chơi đang được biểu diễn dưới dạng **một bộ các số nguyên** chứ không phải một số nguyên, điều đó gây khó khăn cho chúng ta khi tổng quát hóa lời giải và lập trình, một khó khăn rất dễ thấy đó là nếu chúng ta xài mảng $dp$ nhiều chiều để lưu trạng thái đã tính thì có khả năng khi $n$ thay đổi đoạn code cũ sẽ không dùng được nữa do số chiều của mảng đã thay đổi. Theo suy nghĩ tự nhiên, chúng ta sẽ cố gắng tìm cách kết hợp thông tin của bộ trạng thái $n$ số lại thành một số nguyên duy nhất. Một cách khả thi để làm điều này chính là *giá trị Nim*, ký hiệu là $g$.

Đầu tiên, hãy giải quyết trường hợp đơn giản nhất - chỉ có một đống sỏi duy nhất với $p$ viên sỏi.
- Theo trực giác, vì thông tin duy nhất ta có là số lượng sỏi trong đống, ta có thể giả sử rằng giá trị Nim của trường hợp này là một số nguyên $p$ cho biết số sỏi còn lại.
- Chúng ta thấy rằng nếu $p > 0$ thì trạng thái trò chơi hiện tại thuộc $N$, ngược lại nếu $p = 0$ thì trạng thái trò chơi hiện tại thuộc $P$. Điều này có thể giải thích bằng việc nếu số sỏi là số dương, người đi trước luôn có thể lấy hết sỏi nên người đi sau luôn thua. Từ trường hợp đơn giản này, ta mường tượng được điều mà giá trị Nim $g$ phản ánh: *trạng thái trò chơi hiện tại thuộc $N$ khi giá trị Nim dương và thuộc $P$ khi giá trị Nim bằng $0$*.

Tiếp theo, chúng ta sẽ tìm cách ghép các đống sỏi đơn lại thành một trò chơi hoàn chỉnh gồm $n$ đống sỏi. Cụ thể hơn, ta sẽ tìm ra một phép toán $A \oplus B$ để biết được giá trị Nim của trò chơi mới khi kết hợp hai trò chơi có giá trị Nim lần lượt là $A$ và $B$ lại. Ta sẽ liệt kê một số tính chất cần có của phép toán này để có thể gộp các giá trị của cột lại:
- $(A \oplus B) \oplus C = A \oplus (B \oplus C)$, và $A \oplus B = B \oplus A$, nghĩa là $\oplus$ có tính chất kết hợp và giao hoán. Tính chất này có thể giải thích bằng lập luận rằng thứ tự các cọc không thực sự quan trọng nên khi đổi thứ tự kết hợp thì giá trị Nim không thay đổi.
- $A \oplus 0 = A$, nghĩa là, phần tử trung hòa của toán tử này là $0$. Điều này rất hiển nhiên vì nếu bạn **thêm một đống sỏi trống** vào trò chơi, thì trò chơi vẫn không thay đổi, do đó giá trị Nim vẫn như cũ.
- $A \oplus A = 0$, nghĩa là phần tử đối của mỗi trạng thái trò chơi là chính nó. Tại sao điều này lại đúng?  Giả sử chúng ta có hai đống sỏi giống nhau, mỗi đống có $p$ viên sỏi. Lúc đó, người đi sau luôn có một chiến lược chiến thắng, đó là chỉ sao chép bước đi của người đi trước! Điều tương tự xảy ra với trường hợp tổng quát. Do đó, người đi trước sẽ luôn là người thua, tức trạng thái trò chơi thuộc $P$, như ta đã lập luận ở ví dụ trước, trạng thái thuộc $P$ tương ứng với $g = 0$, do đó $A \oplus A = 0$.

Ba thuộc tính này, đặc biệt nhất là tính chất $A \oplus A = 0$ giúp chúng ta tìm ra một ứng cử viên tìm năng, đó là **phép [bitwise XOR](https://vi.wikipedia.org/wiki/Ph%C3%A9p_to%C3%A1n_thao_t%C3%A1c_bit#XOR)**. Và quả thật phép toán này cũng chính là đáp án chúng ta cần tìm, định lý Bouton được trình bày ở phía dưới sẽ chứng minh tính đúng đắn của việc này.

Thực chất, phép bitwise XOR chỉ là phép cộng modulo $2$ trên từng bit, do đó chúng ta hay gọi giá trị Nim là *tổng Nim*. Tổng Nim của một trò chơi có trạng thái $(p_1, p_2, \ldots, p_n)$ thu được bằng cách thực hiện phép xor các $p_i$ lại với nhau.
$$g = p_1 \oplus p_2 \oplus \ldots \oplus p_n$$

Ví dụ: với trò chơi Nim có ba đống sỏi với số sỏi lần lượt là $5$, $7$, $3$ thì tổng Nim là $0101 \oplus 0111 \oplus 0011 = 0001$.

### Định lý Bouton
Tuy nhiên, trong phần trình bày ở phía trên có một giả thuyết rất quan trọng mà ta chỉ mới thừa nhận chứ chưa chứng minh, đó là *trạng thái trò chơi hiện tại thuộc $N$ khi giá trị Nim dương và thuộc $P$ khi giá trị Nim bằng $0$*. Thực ra, nhận xét đó là một định lý đã được chứng minh, gọi là định lý Bouton. Phần này khuyến khích bạn đọc hiểu về các tính chất của phép toán [bitwise XOR](https://vi.wikipedia.org/wiki/Ph%C3%A9p_to%C3%A1n_thao_t%C3%A1c_bit#XOR).

**Định lý Bouton**: trạng thái của trò chơi Nim $(p_1, p_2, \ldots, p_n)$ thuộc $P$ khi và chỉ khi tổng Nim $g$ của nó bằng $0$.

**Chứng minh**

Gọi $\hat{P}$ là tập gồm các trạng thái có tổng Nim bằng $0$ và $\hat{N}$ là tập gồm các trạng thái có tổng Nim lớn hơn $0$. Ta nhận xét hai tập này có các tính chất sau:

Đầu tiên, trạng thái kết thúc là $(0,\ldots,0)$ thuộc $\hat{P}$ do có tổng Nim là $0$

Thứ hai, với một trạng thái thuộc $\hat{N}$ ($g > 0$), ta luôn có thể đi tới một trạng thái thuộc $\hat{P}$ ($g = 0$). Để chứng minh, chọn một đống sỏi thứ $i$ có $p_i$ sỏi và biến nó thành $p'_i$ sao cho $p'_i = g \oplus p_i < p_i$, ta có được tổng Nim mới $g'$ như sau:
\begin{align*}
    g' &= p_1 \oplus p_2 \oplus \ldots \oplus p'_i \oplus \ldots \oplus p_n \\
        &=  p_1 \oplus p_2 \oplus \ldots \oplus [p_i \oplus g] \oplus \ldots \oplus p_n \\
        &=  (p_1 \oplus p_2 \oplus \ldots \oplus p_i \oplus \ldots \oplus p_n) \oplus g \\
        &= g \oplus g = 0
\end{align*}
        
Lưu ý rằng phép XOR không giống phép cộng thông thường. Ở phép cộng hai số nguyên dương, kết quả luôn lớn hơn các toán hạng ban đầu. Tuy nhiên, trong phép XOR điều này không xảy ra, kết quả có thể lớn hơn hoặc nhỏ hơn các toán hạng ban đầu. Do đó việc ta có thể đảm bảo luôn tồn tại đống sỏi $i$ thỏa mãn yêu cầu $p'_i = g \oplus p_i < p_i$ không phải là điều hiển nhiên và **cần được chứng minh**.

Vì $g > 0$ nên  biểu diễn nhị phân của $g$ luôn tồn tại bit trái nhất bằng $1$ (tạm gọi là $d$).Khi đó, xét bit thứ $d$ của tất cả các cột $p_i$, ta có số lượng $p_i$ có bit thứ $d$ bằng $1$ phải lẻ (theo tính chất của phép XOR), do đó luôn tồn tại một đống sỏi có bit thứ $d$ bằng $1$. Chọn đống sỏi có bit thứ $d$ bằng $1$ đó để thực hiện bốc sỏi, ta thấy $p'_i = p_i \oplus g < p_i$ bởi vì khi XOR bit tại vị trí $d$ bằng $1 \oplus 1 = 0$, do đó $p'_i$ luôn mất một bit $1$ tại vị trí $d$ so với $p_i$.

> Ví dụ, nếu trò chơi Nim hiện tại có $4$ cột có số sỏi lần lượt là $7$, $10$, $12$, $3$, thì thao tác tính tổng Nim và chọn cột để lấy sỏi ra sẽ diễn ra như hình dưới
> ![](https://i.imgur.com/oodUeSQ.png)

Cuối cùng, với một trạng thái thuộc $\hat{P}$ (tức $g = 0$), mọi cách đi đều dẫn tới trạng thái thuộc $\hat{N}$ (tức $g > 0$). Ta có thể chứng minh dễ dàng bằng phương pháp phản chứng. Giả sử trạng thái trò chơi hiện tại là $(p_1, \ldots, p_n)$ có tổng Nim $g = 0$ và tồn tại một đống sỏi $i$ sao cho khi lấy bớt sỏi từ $i$ ra trạng thái trò chơi mới có tổng Nim $g' = 0$. Khi đó
\begin{align*}
    g' &= 0 = g
    \\ \Leftrightarrow
    p_1 \oplus p_2 \oplus \ldots \oplus p'_i \oplus \ldots \oplus p_n &=  p_1 \oplus p_2 \oplus \ldots \oplus p_i \oplus \ldots \oplus p_n
    \\ \Leftrightarrow
    p'_i &= p_i
\end{align*}
Điều này có nghĩa là ta không bốc viên sỏi nào từ đống $p_i$ ra cả, mà theo giả thuyết ta phải bốc ít nhất một viên ($p'_i < p_i$), vì vậy không thể tồn tại đống sỏi $i$ nào thỏa mãn yêu cầu.
> Ví dụ, nếu trò chơi Nim hiện tại có $3$ đống có số sỏi lần lượt là $5$, $6$, $3$, thì tổng Nim $g = 0$. Xét bit đầu tiên từ phải qua, ta thấy được số lượng bit được bật tại vị trí này là số chẵn ($2$, tương ứng với bit đầu tiên của $5$ và $3$). Tương tự, số lượng các bit được bật tại các vị trị khác đều có tính chất này. Điều này không phải là trùng hợp mà do tính chất của phép XOR, nếu muốn bit thứ $i$ trong kết quả bằng $0$ thì số lượng bit thứ $i$ được bật trong các toán hạng phải là số chẵn. Từ đây ta nhận thấy, việc bỏ sỏi ở một đống sỏi chỉ có thể làm thay đổi số lượng bit được bật tại mỗi vị trí $i$ lên hoặc xuống 1 đơn vị, do đó dù cho lấy sỏi ở cột nào đi nữa thì vẫn sẽ xuất hiện một vị trí có số bit được bật là lẻ.
> ![](https://i.imgur.com/uFzRESc.png)

Rõ ràng $\hat{P}$ và $\hat{N}$ thỏa mãn ba điều kiện theo định nghĩa của tập $P$ và $N$ trong trò chơi tổng quát, vì vậy $P = \hat{P}$ và $N = \hat{N}$.

Qua định lý Bouton, chúng ta có một cách xác định tập $P$ và $N$ dựa trên tổng Nim, hơn thế nữa với việc chứng minh định lý Bouton, ta không chỉ biết được trạng thái thắng/thua của trò chơi mà còn có thể xây dựng được một chiến thuật thắng cụ thể.

## Cài đặt
Hàm `isInP` trong trường hợp này rất đơn giản, nếu lưu số lượng sỏi mỗi đống vào vector số nguyên thì thuật toán chỉ đơn giản là XOR của các phần tử với nhau, độ phức tạp thuật toán là $O(n)$.
```C++
bool isInP(vector<int> v) {
    int g = 0;
    for (auto p: v)
        g ^= p;
    return (g == 0);
}
```

# Định lý Sprague-Grundy

## Mở rộng trò chơi Nim
Định lý Bouton đã cho chúng ta một cách giải rất đẹp cho trò chơi Nim chuẩn, nhưng trong thực tế, hầu hết các bài lý thuyết trò chơi trong lập trình thi đấu sẽ không là trò chơi Nim chuẩn mà sẽ được thay đổi luật chơi ở một số điểm. Lúc ấy, định lý Bouton sẽ không thể giải quyết các dạng bài này.

Để giải quyết, ta quay lại chiến lược ban đầu của mình - phân tách trạng thái trò chơi thành nhiều trò chơi con có cùng cấu trúc, tìm một số tính chất đặc biệt của mỗi trò chơi con, sau đó kết hợp chúng lại với nhau để có câu trả lời cho trò chơi gốc.

Hãy lấy ví dụ với một biến thể của trò chơi Nim chuẩn, trong đó chúng ta có $n$ đống sỏi, luật chơi vẫn như cũ, chỉ khác là số sỏi phải bốc ở mỗi lượt phải là *số chính phương* ($1, 4, \ldots$).

Đầu tiên, ta cũng xét trò chơi ở dạng đơn giản nhất: chỉ có một đống sỏi duy nhất với $p$ viên. Vậy làm thế nào để bạn biết đó là trạng thái thuộc $P$ hay trạng thái thuộc $N$?

Hãy nhìn trò chơi dưới dạng một đồ thị có hướng. Đồ thị này có $p + 1$ đỉnh có nhãn lần lượt là các số nguyên từ $0$ đến $p$. Mỗi đỉnh đồ thị tương ứng với một trạng thái trò chơi, trong đó nhãn của nó cho biết có bao nhiêu sỏi còn lại trong đống hiện tại. Nếu có thể di chuyển hợp lệ từ trạng thái $u$ sang $v$ thì tương ứng trong đồ thị cũng có cạnh có hướng $<u,v>$. Hình dưới là ví dụ trò chơi với một đống sỏi có số sỏi $p = 5$.
![](https://i.imgur.com/IiBr0pB.png)


---------------UNDER CONSTRUCTION---------------

Cách tiếp cận ở đây sẽ là từ việc tổng quát hóa trò chơi Nim về đồ thị. 

Sau đó liên hệ bất cứ combinational game nào mà đồ thị là 1 DAG đều tương đương với trò chơi Nim 1 cột.

- Định nghĩa số Grundy, nêu sự tương tự với Nim-value.

- Nêu định nghĩa đồ thị tổng

- Phát biểu định lý Grundy, chứng minh bằng cách nêu sự tương đương của $g=0$ và $g>0$ với tập P và tập N.

## Ví dụ
[](https://www.topcoder.com/thrive/articles/Algorithm%20Games)