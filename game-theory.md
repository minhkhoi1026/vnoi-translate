# Lý thuyết trò chơi

Người viết: Nguyễn Nhật Minh Khôi

Trong thực tế, có rất nhiều loại trò chơi khác nhau, tuy nhiên trong bài viết này, chúng ta sẽ chỉ giới hạn trong các **trò chơi tổ hợp cân bằng** vì độ phổ biến của nó trong lập trình thi đấu.

## Trò chơi tổ hợp cân bằng tổng quát

### Định nghĩa
**Trò chơi tổ hợp** là trò chơi gồm: *hai người chơi (ở đây tạm gọi người chơi trước là $A$ và người chơi sau là $B$)*, một *tập **hữu hạn** các trạng thái* $S$ có thể đạt được của trò chơi và một *tập các bước di chuyển hợp lệ* $Q$ để di chuyển từ trạng thái này sang trạng thái khác (gọi là luật chơi). Một số trạng thái sẽ là trạng thái kết thúc, tập các trạng thái kết thúc gọi là $A$. Hai người chơi sẽ luân phiên di chuyển từ trạng thái này sang trạng thái khác. Người đến được trạng thái kết thúc trước sẽ là người chiến thắng.

Khi hai người chơi có tập luật chơi và tập trạng thái kết thúc giống nhau thì trò chơi được gọi là **trò chơi tổ hợp cân bằng**. Nói cách khác, hai người chơi chỉ khác nhau ở đúng một điểm là người này được đi trước, người kia đi sau.

Một ví dụ dễ thấy của trò chơi tổ hợp cân bằng chính là [Notakto](https://en.wikipedia.org/wiki/Notakto) (một biến thể của cờ caro). Trong đó:
- Hai người chơi lần lượt ghi X bàn cờ hình vuông $3 \times 3$.
- Một trạng thái của trò chơi chính là một trạng thái của bàn cờ.
- Một bước di chuyển hợp lệ là ghi ký tự X lên một ô trống trên bàn cờ.
- Trạng thái kết thúc là khi người chơi đạt tới trạng thái bàn cờ có ba ký tự X liên tiếp theo hàng dọc/hàng ngang/đường chéo.
> ![](https://i.imgur.com/ln0Od48.png)

### Chiến thuật thắng
Mục tiêu của chúng ta khi giải các bài toán với trò chơi tổ hợp cân bằng là tìm ra chiến thuật mà ở đó một trong hai người chơi luôn có thể ép người còn lại thua. Chiến thuật như vậy gọi là một **chiến thuật thắng**.

Để phục vụ cho việc đưa ra chiến thuật thắng, người ta phân loại các trạng thái cùa trò chơi thành 2 tập $N$ và $P$ thỏa ba tính chất sau:
1. Tập $P$ phải chứa toàn bộ trạng thái kết thúc
2. Mỗi trạng thái thuộc $N$ **luôn tồn tại** một cách di chuyển trực tiếp đến trạng thái thuộc $P$.
3. Mỗi trạng thái thuộc tập $P$ **chỉ** có thể di chuyển đến trạng thái thuộc tập $N$.

Tới đây, có thể bạn sẽ đặt ra câu hỏi: Liệu mọi trạng thái của trò chơi đều thuộc một trong hai tập $N$ hay $P$? Ta có thể chứng minh dễ dàng bằng quy nạp mạnh theo số bước tối đa để đạt tới trạng thái kết thúc. Nếu muốn, bạn đọc có thể tham khảo thêm ở phần [1.1. Impartial game - Game Theory, Alive](https://homes.cs.washington.edu/~karlin/GameTheoryBook.pdf).

Từ cách định nghĩa tập $P$ và $N$ như vậy, ta có thể dễ dàng xây dựng chiến thuật thắng cho người chơi $A$ như sau (do đây là trò chơi tổ hợp cân bằng nên chiến thuật thắng của $B$ cũng sẽ tương tự):
1. Nếu $A$ **bắt đầu ở trạng thái thuộc $N$**, **luôn đi tới trạng thái thuộc $P$** để ép $B$ đi vào trạng thái thuộc $N$. Do người thắng là người đi vào trạng thái kết thúc, mà trạng thái kết thúc lại thuộc $P$ nên chắc chắn $A$ sẽ thắng.
2. Nếu $A$ bắt đầu ở trạng thái thuộc $P$, $A$ chỉ có thể kéo dài ván đấu và đợi sơ hở của $B$ ($B$ đi vào vị trí thuộc $N$) và dùng chiến thuật thắng ở trường hợp đầu. Tuy nhiên nếu $B$ cũng chơi tối ưu thì chắc chắn $A$ sẽ thua.

### Thuật toán xác định tập P và N
Ta có ý tưởng thuật toán để tìm tập $P$ và $N$ như sau:
```C++
bool isTerminateState(int u) {
    // cài đặt logic hàm để xác định u có phải trạng thái kết thúc
}

// Hàm kiểm tra một trạng thái thuộc P (true) hay N (false)
int isInP(int u) {
    if (isTerminateState(u)) // nếu là trạng thái kết thúc thì thuộc P
        return 1;
    
    // kiểm tra xem từ u có thể đi tới trạng thái P nào không
    for (int v)
        if (approachable(u, v) && isInP(v))
            return 0; // nếu có v thuộc P thì u thuộc N
    
    return 1; // nếu không thì u thuộc P
}
```
Ý tưởng ở đây chính là đệ quy:
1. Nếu trạng thái $u$ đang xét là trạng thái kết thúc thì thêm $u$ vào tập $P$. Đây chính là trường hợp cơ bản.
2. Nếu $u$ không là trạng thái kết thúc thì đệ quy đến những trạng thái $v$ mà từ $u$ có thể di chuyển trực tiếp đến. Nếu có bất kỳ $v$ nào thuộc tập $P$ thì $u$ thuộc tập $N$, ngược lại (tất cả $v$ đều thuộc $N$) thì $u$ thuộc $P$.

Ở đây có hai chú ý về cách cài đặt trên:
1. Ở đây giả sử trạng thái được biểu diễn bằng một số nguyên int, tuy nhiên nhiều trường hợp trạng thái của chúng ta không ở dạng số nguyên, khi đó có một số cách các bạn có thể xem xét để lưu trạng thái như: bitmask, hashmap (unordered_map trong C++).
2. Không nhất thiết phải cài đặt hàm kiểm tra `approachable(u, v)` mà có thể lưu vào mảng/vector sẵn các vị trí $v$ mà từ $u$ có thể đến.

Tuy nhiên, thuật toán này có một nhược điểm, đó là **gọi lại cùng một giá trị quá nhiều lần** do không lưu lại kết quả trong quá trình đệ quy. Cách giải quyết chính là quy hoạch động top-down (hay còn gọi là đệ quy có nhớ), trong đó ta có một mảng $dp$ lưu lại kết quả của những trạng thái đã từng xét. Thuật toán sẽ thay đổi như sau
```C++
bool isTerminateState(int u) {
    // cài đặt logic hàm để xác định u có phải trạng thái kết thúc
}

const MAX_N = 1000; // số trạng thái tối đa
int dp[MAX_N];

// gọi hàm này trước để khởi tạo giá trị mảng dp toàn -1
void init() {
    memset(dp, -1, sizeof(dp));
}

// Hàm kiểm tra một trạng thái thuộc P (true) hay N (false)
int isInP(int u) {
    // Nếu đã tính u trước đó thì trả về kết quả đã lưu
    if (dp[u] != -1)
        return dp[u];
    
    // nếu là trạng thái kết thúc thì u thuộc P
    if (isTerminateState(u)) {
        dp[u] = 1;
        return 1;
    } 
        
    // kiểm tra xem từ u có thể đi tới trạng thái P nào không
    for (int v)
        if (approachable(u, v) && isInP(v)) {
            dp[u] = 0;
            return 0; // nếu có v thuộc P thì u thuộc N
        }
            
    dp[u] = 1;
    return 1; // nếu không thì u thuộc P
}
```

Trông thì có vẻ dài, nhưng khác biệt cốt lõi của cài đặt mới này chỉ nằm ở hai chỗ: (1) khi tính xong, trước khi trả về giá trị thì ta lưu giá trị lại vào một mảng $dp$ và ở đầu hàm $isInP(u)$ và (2) ta trả về $dp[u]$ nếu $dp[u] != -1$ (tức $u$ đã được tính trước đó). Lưu ý ở đây ta đặt $dp[u] = -1$ để đánh dấu $u$ chưa được tính.

### Ví dụ: bốc sỏi
Để có cái nhìn thực tế hơn về cách cài đặt, ta sẽ lấy ví dụ sau.

Hai bạn An và Bình đang chơi bốc sỏi, trò chơi được phát biểu như sau: cho một đống sỏi có $n$ viên sỏi, hai bạn sẽ lần lượt bốc sỏi từ đống sỏi, mỗi lượt chỉ được bốc $1$, $2$ hoặc $3$ viên sỏi, người lấy được viên sỏi cuối cùng sẽ là người chiến thắng. Biết rằng An đi trước và Bình đi sau và đều chơi tối ưu, hãy cho biết ai là người chiến thắng?

Ta có thể xác định được các thông tin của trò chơi như sau:
- Mỗi trạng thái của trò chơi là một số nguyên $x$ cho biết đống sỏi còn lại $x$ viên.
- Tập các trạng thái của trò chơi $S = \{0, 1, 2, \ldots, n\}$
- Tập trang thái kết thúc $A = \{0\}$. Có thể giải thích là vì trò chơi kết thúc khi một trong hai bạn lấy được viên sỏi cuối cùng, lúc đó trạng thái của trò chơi là $0$ (không còn viên sỏi nào).
- Ở trạng thái $x$, một bước di chuyển hợp lệ sẽ tới trạng thái $x - c$ sao cho $c \in \{1, 2, 3\}$ và $x - c \geq 0$.

Với nhận xét như vậy, ta có thuật toán sau:
```C++
const MAX_N = 100000; // số trạng thái tối đa
int dp[MAX_N];

void init() {
    memset(dp, -1, sizeof(dp));
}

int isInP(int u) {
    if (dp[u] != -1)
        return dp[u];
    
    if (u == 0) {
        dp[u] = 1;
        return 1;
    } 
        
    for (int v = u - 1; v >= max(u - 3, 0); v--)
        if (isInP(v)) {
            dp[u] = 0;
            return 0; 
        }
            
    dp[u] = 1;
    return 1;
}

int main() {
    int n;
    
    cin >> n;
    
    init();
    if (isInP(n) == false) cout << "A win";
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

Tuy nhiên, khi bắt tay vào làm, bạn sẽ gặp ngay một rào cản, đó là trạng thái của trò chơi đang được biểu diễn dưới dạng **một bộ các số nguyên** chứ không phải một số nguyên, điều đó gây khó khăn cho chúng ta khi tổng quát hóa lời giải và lập trình, một khó khăn rất dễ thấy đó là nếu chúng ta xài mảng $dp$ nhiều chiều để lưu trạng thái đã tính thì có khả năng khi $n$ thay đổi đoạn code cũ sẽ không dùng được nữa do số chiều của mảng đã thay đổi. Theo suy nghĩ tự nhiên, chúng ta sẽ cố gắng tìm cách kết hợp thông tin của bộ trạng thái $n$ số lại thành một số nguyên duy nhất. Số nguyên này được gọi là *giá trị Nim*, ký hiệu là $g$.

Đầu tiên, hãy giải quyết trường hợp đơn giản nhất - chỉ có một đống sỏi duy nhất với $p$ viên sỏi.
- Theo trực giác, vì thông tin duy nhất ta có là số lượng sỏi trong đống, ta có thể giả sử rằng giá trị Nim của trường hợp này là một số nguyên $p$ cho biết số sỏi còn lại.
- Chúng ta thấy rằng nếu $p > 0$ thì trạng thái trò chơi hiện tại thuộc $N$, ngược lại nếu $p = 0$ thì trạng thái trò chơi hiện tại thuộc $P$. Điều này có thể giải thích bằng việc nếu số sỏi là số dương, người đi trước luôn có thể lấy hết sỏi nên người đi sau luôn thua. Từ trường hợp đơn giản này, ta mường tượng được điều mà giá trị Nim $g$ phản ánh: *trạng thái trò chơi hiện tại thuộc $N$ khi giá trị Nim dương và thuộc $P$ khi giá trị Nim bằng $0$*.

Tiếp theo, chúng ta sẽ tìm cách ghép các đống sỏi đơn lại thành một trò chơi hoàn chỉnh gồm $n$ đống sỏi. Cụ thể hơn, ta sẽ tìm ra một phép toán $A \oplus B$ để biết được giá trị Nim của trò chơi mới khi kết hợp hai trò chơi có giá trị Nim lần lượt là $A$ và $B$ lại. Ta sẽ liệt kê một số tính chất của phép toán này:
- $(A \oplus B) \oplus C = A \oplus (B \oplus C)$, và $A \oplus B = B \oplus A$, nghĩa là $\oplus$ có tính chất kết hợp và giao hoán. Tính chất này có thể giải thích bằng lập luận rằng thứ tự các cọc không thực sự quan trọng nên khi đổi thứ tự kết hợp thì giá trị Nim không thay đổi.
- $A \oplus 0 = A$, nghĩa là, phần tử trung hòa của toán tử này là $0$. Điều này rất hiển nhiên vì nếu bạn kết hợp thêm một đống sỏi trống vào trò chơi, thì trò chơi vẫn không thay đổi, do đó giá trị Nim vẫn như cũ.
- $A \oplus A = 0$, nghĩa là phần tử đối của mỗi trạng thái trò chơi là chính nó. Tại sao điều này lại đúng?  Giả sử chúng ta có hai đống sỏi giống nhau, mỗi đống có $p$ viên sỏi. Lúc đó, người đi sau luôn có một chiến lược chiến thắng, đó là chỉ sao chép bước đi của người đi trước! Điều tương tự xảy ra với trường hợp tổng quát. Do đó, người đi trước sẽ luôn là người thua, tức trạng thái trò chơi thuộc $P$, như ta đã lập luận ở ví dụ trước, trạng thái thuộc $P$ tương ứng với $g = 0$, do đó $A \oplus A = 0$.

Ba thuộc tính này, đặc biệt nhất là tính chất $A \oplus A = 0$ giúp chúng ta xác định được phép toán cần tìm, không gì khác chính là **phép [bitwise XOR](https://vi.wikipedia.org/wiki/Ph%C3%A9p_to%C3%A1n_thao_t%C3%A1c_bit#XOR)**.

Tuyệt vời! Vậy ta đã giải quyết được vấn đề biểu diễn trạng thái cho trò chơi Nim. Và vì thực chất phép XOR chỉ là phép cộng modulo $2$, chúng ta hay gọi giá trị Nim là *tổng Nim*.

Ví dụ: với trò chơi Nim có ba đống sỏi với số sỏi lần lượt là $5$, $7$, $3$ thì tổng Nim là $0101 \oplus 0111 \oplus 0011 = 0001$.

### Định lý Bouton
Tuy nhiên, trong phần trình bày ở phía trên có một giả thuyết rất quan trọng mà ta chỉ mới thừa nhận chứ chưa chứng minh, đó là *trạng thái trò chơi hiện tại thuộc $N$ khi giá trị Nim dương và thuộc $P$ khi giá trị Nim bằng $0$*. Thực ra, nhận xét đó là một định lý đã được chứng minh, gọi là định lý Bouton.

**Định lý Bouton**: trạng thái của trò chơi Nim $(p_1, p_2, \ldots, p_n)$ thuộc $P$ khi và chỉ khi tổng Nim của nó $p_1 \oplus p_2 \oplus \ldots \oplus p_n$ bằng $0$.

**Chứng minh**

Gọi $\hat{P}$ là tập gồm các trạng thái có tổng Nim bằng $0$ và $\hat{N}$ là tập gồm các trạng thái có tổng Nim lớn hơn $0$. Ta nhận xét hai tập này có các tính chất sau:
1. Trạng thái kết thúc là $(0,\ldots,0)$ thuộc $\hat{P}$ do có tổng Nim là $0$
2. Với một trạng thái thuộc $\hat{N}$ ($g > 0), ta luôn có thể đi tới một trạng thái thuộc $\hat{P}$ ($g = 0). Để chứng minh, chọn một đống sỏi thứ $i$ có $p_i$ sỏi và biến nó thành $p'_i$ sao cho $p'_i = g \oplus p_i < p_i$, ta có được tổng Nim mới $g'$ như sau:
\begin{align*}
    g' &= p_1 \oplus p_2 \oplus \ldots \oplus p'_i \oplus \ldots \oplus p_n \\
        &=  p_1 \oplus p_2 \oplus \ldots \oplus [p_i \oplus g] \oplus \ldots \oplus p_n \\
        &=  (p_1 \oplus p_2 \oplus \ldots \oplus p_i \oplus \ldots \oplus p_n) \oplus g \\
        &= g \oplus g = 0
\end{align*}
Ta có thể đảm bảo luôn tồn tại đống sỏi $i$ thỏa mãn yêu cầu $p'_i = g \oplus p_i < p_i$. Vì $g > 0$ nên  biểu diễn nhị phân của $g$ luôn tồn tại bit trái nhất bằng $1$ (tạm gọi là $d$), mà ta thấy để tạo được giá trị $1$ chỉ có hai trường hợp $0 \oplus 1$ và $1 \oplus 0$, do đó chắc chắn trong các đống sỏi phải tồn tại đống $i$ sao cho bit tại vị ví $d$ bằng $1$. Chọn đống sỏi đó để thực hiện bốc sỏi, ta thấy $p_i \oplus g < p_i$ bởi vì khi XOR bit tại vị trí $d$ bằng $1 \oplus 1 = 0$, do đó $p'_i$ luôn mất một bit $1$ tại vị trí $d$ so với $p_i$.
3. Với một trạng thái thuộc $\hat{P}$ ($g = 0$), mọi cách đi đều dẫn tới trạng thái thuộc $P$ ($g > 0$). Ta có thể chứng minh dễ dàng bằng phương pháp phản chứng. Giả sử trạng thái trò chơi hiện tại là $(p_1, \ldots, p_n)$ có tổng Nim $g = 0$ và tồn tại một đống sỏi $i$ sao cho khi lấy bớt sỏi từ $i$ ra trạng thái trò chơi mới có tổng Nim $g' = 0$. Khi đó
\begin{align*}
    g' &= 0 = g
    \\ \Leftrightarrow
    p_1 \oplus p_2 \oplus \ldots \oplus p'_i \oplus \ldots \oplus p_n \\
    &=  p_1 \oplus p_2 \oplus \ldots \oplus p_i \oplus \ldots \oplus p_n \\
    \\ \Leftrightarrow
    p'_i = p_i
\end{align*}
Điều này có nghĩa là ta không bốc viên sỏi nào từ đống $p_i$ ra cả, mà theo giả thuyết ta phải bốc ít nhất một viên ($p'_i < p_i$), vì vậy không thể tồn tại đống sỏi $i$ nào thỏa mãn yêu cầu.

Rõ ràng $\hat{P}$ và $\hat{N}$ thỏa mãn ba điều kiện theo định nghĩa của tập $P$ và $N$ trong trò chơi tổng quát, vì vậy $P = \hat{P}$ và $N = \hat{N}$.

Qua định lý Bouton, chúng ta có một cách xác định tập $P$ và $N$ dựa trên tổng Nim, hơn thế nữa với việc chứng minh định lý Bouton, ta không chỉ biết được trạng thái thắng/thua của trò chơi mà còn có thể xây dựng được một chiến thuật thắng cụ thể.

## Cài đặt
Hàm `isInP` trong trường hợp này rất đơn giản, nếu lưu số lượng sỏi mỗi đống vào mảng một chiều thì thuật toán chỉ đơn giản là XOR của các phần tử với nhau, độ phức tạp thuật toán là $O(n)$.
```C++
int isInP(int a[], int n) {
    int g = 0;
    for (int i = 0; i < n; i++)
        g = g ^ a[i];
    return (g == 0); // highlight-line
}
```

# Định lý Sprague-Grundy

## Mở rộng trò chơi Nim
Định lý Bouton đã cho chúng ta một cách giải rất đẹp cho trò chơi Nim chuẩn, nhưng thực tế cuộc đời không dễ dàng vậy, hầu hết các bài lý thuyết trò chơi trong lập trình thi đấu sẽ không là trò chơi Nim chuẩn mà sẽ được thay đổi luật chơi ở một số điểm. Lúc ấy, định lý Bouton sẽ bó tay với các bài này.

Để giải bài toán, ta quay lại chiến lược ban đầu của mình - phân tách trạng thái trò chơi thành nhiều trò chơi con có cùng cấu trúc, tìm một số tính chất đặc biệt của mỗi trò chơi con, sau đó kết hợp chúng lại với nhau để có câu trả lời cho trò chơi gốc.

Hãy lấy ví dụ với một biến thể của trò chơi Nim chuẩn, trong đó chúng ta có $n$ đống sỏi, luật chơi vẫn như cũ, chỉ khác là số sỏi phải bốc ở mỗi lượt phải là *số chính phương* ($1, 4, \ldots$).

Đầu tiên, ta cũng xét trò chơi ở dạng đơn giản nhất: chỉ có một đống sỏi duy nhất với $p$ viên. Vậy làm thế nào để bạn biết đó là trạng thái thuộc $P$ hay trạng thái thuộc $N$?

Hãy nhìn trò chơi dưới dạng một đồ thị có hướng. Đồ thị này có $p + 1$ đỉnh có nhãn lần lượt là các số nguyên từ $0$ đến $p$. Mỗi đỉnh đồ thị tương ứng với một trạng thái trò chơi, trong đó nhãn của nó cho biết có bao nhiêu sỏi còn lại trong đống hiện tại. Nếu có thể di chuyển hợp lệ từ trạng thái $u$ sang $v$ thì tương ứng trong đồ thị cũng có cạnh có hướng $<u,v>$. Hình dưới là ví dụ trò chơi với một đống sỏi có số sỏi $p = 5$.
![custom-nim-example](https://i.imgur.com/gzCVF0K.png)

Theo định nghĩa, các đỉnh không có cạnh ra (bậc ra bằng $0$) chính là đỉnh của trạng thái kết thúc (trạng thái thuộc $P$). Sau đó, nếu đỉnh $u$ tồn tại một cạnh đi tới một đỉnh của trạng thái thuộc $P$, thì đó là đỉnh của trạng thái thuộc $N$. Ngược lại, nếu đỉnh $u$ luôn đi tới đỉnh của trạng thái thuộc $N$. Giả sử không có chu trình nào trong đồ thị, tức trò chơi kết thúc với một số lần di chuyển hữu hạn.
![custom-nim-example-PN](https://i.imgur.com/oYiVGVY.png)


Cách tiếp cận ở đây sẽ là từ việc tổng quát hóa trò chơi Nim về đồ thị. 

Sau đó liên hệ bất cứ combinational game nào mà đồ thị là 1 DAG đều tương đương với trò chơi Nim 1 cột.

- Định nghĩa số Grundy, nêu sự tương tự với Nim-value.

- Nêu định nghĩa đồ thị tổng

- Phát biểu định lý Grundy, chứng minh bằng cách nêu sự tương đương của $g=0$ và $g>0$ với tập P và tập N.

## Ví dụ
- Ví dụ với trò chơi con ngựa trong

## Tham khảo
- Tài liệu chuyên tin tập 3
- 