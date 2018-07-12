
# Hướng dẫn về Trie và một số ví dụ về vấn đề gặp phải - Chủ đề @ IIIT Hyderabad 

Tôi sẽ viết trong bài viết này về Tries và khái niệm được sử dụng rộng rãi trong các loại thao tác nhỏ của vấn đề. Chúng ta sẽ thấy 2-3 vấn đề mà trie rất hữu ích.

Đầu tiên chúng ta sẽ tìm hiểu xem trie là cái gì. Trie có thể lưu trữ thông tin về các khóa/số/chuỗi nhỏ gọn trong 1 cây.
Các trie bao gồm các node , nơi mà mỗi nốt lưu trữ một ký tự/ bit. Chúng ta có thẻ chèn các chuỗi/số mới một cách phù hợp.
Đây là một ví dụ về trie của chuỗi:

![][1]

  
Nguồn: Wikipedia.

Nhưng chúng ta sẽ xử lý những con số ở đây và đặc biệt trong các bit nhị phân.Chúng ta sẽ thấy khi chúng ta giải quyết vấn đề.
**Vấn đề 1**: Cho một mảng số nguyên,  Chúng ta phải tìm 2 phần tử mà XOR là lớn nhất.
**Giải pháp:**  
Giả sử chúng ta có một cấu trúc dữ liệu mà có thẻ đáp ứng 2 kiểu truy vấn:

1\. Chèn số X
2\. Cho một biến Y, Tìm XOR lớn nhất của Y  với tất cả số được lưu đến tận bây giờ.

Nếu chúng ta có cấu trúc dữ liệu, chúng ta sẽ chèn  số nguyên khi chúng ta đi,và với truy vấn loại 2 thì chúng ta sẽ tìm được XOR lớn nhất.
Trie là cấu trúc dữ liệu chúng ta sẽ sử dụng.Đầu tiên hãy xem cách mà chúng ta chèn phần tử vào trie.


![][2]

Vì vậy , chúng ta theo dõi đường dẫn của số mà chúng ta cần chèn , chúng ta không phải vẽ lại đường dẫn hiện có.

Chèn một khóa độ dài N lấy O (N) là log2 (MAX) trong đó MAX là số lớn nhất được chèn vào trong trie, bởi vì có số bit nhị phân log2 (MAX) tối đa trong một số.
Bằng cách này, chúng ta lưu trữ tất cả dữ liệu về tất cả các số được chèn vào trie cho đến bây giờ.
Bây giờ, đối với truy vấn loại 2: 
Giả sử số Y của chúng ta là b1, b2… bn, trong đó b1, b2… là các bit nhị phân. Chúng ta bắt đầu từ b1. Bây giờ cho XOR là tối đa, chúng ta sẽ cố gắng tạo ra bit quan trọng nhất 1 sau khi dùng XOR. Vì vậy, nếu b1 bằng 0, chúng ta sẽ cần 1 và ngược lại. Trong trie, chúng ta đi đến phần bit cần thiết. Nếu tùy chọn thuận lợi không có ở đó, chúng ta sẽ đi bên kia. Làm điều này tất cả cho i = 1 đến n, chúng tôi sẽ nhận được tối đa XOR có thể.

![][3]

Quá nhiều truy vấn là log2(MAX).

**Vấn đề 2**: Cho một mảng các số nguyên, tìm mảng con với XOR tối đa.
**Giải pháp:**  
Giả sử F (L, R) là XOR của mảng con từ L đến R.
Ở đây chúng ta sử dụng đặc tính F (L, R) = F (1, R) XOR F (1, L-1). Làm như thế nào? Giả sử mảng con của chúng ta với XOR tối đa kết thúc ở vị trí i. Bây giờ, chúng ta cần tối đa hóa F (L, i). F (1, i) XOR F (1, L-1) trong đó L <= i. Giả sử, chúng ta đã chèn F (1, L-1) vào trie của chúng ta cho tất cả L <= i, thì đó chỉ là vấn đề1.
    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thể thử vấn đề này ở đây: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Cho một mảng các số nguyên dương bạn phải in số lượng các mảng con có XOR nhỏ hơn K.

**Giải pháp:**  
Điều này một lần nữa sử dụng các khái niệm chúng ta đã thấy cho đến bây giờ. Chúng ta sẽ đi như câu hỏi trước.
Đối với mỗi chỉ số i = 1 đến N, chúng ta có thể đếm số lượng các mảng con kết thúc tại vị trí thứ i thỏa mãn điều kiện đã cho. 

    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  
truy vấn (q, k) trả về số lượng các số nguyên đã tồn tại thành cấu trúc mà khi lấy xor với q trả về một số nguyên nhỏ hơn k.
Chúng ta so sánh các bit tương ứng của q và k, bắt đầu từ các bit quan trọng nhất. Giả sử p và q là các bit tương ứng mà chúng ta đang xem xét.

Nếu q là 1, và p là 0, thì chúng ta thực hiện điều này:

![][5]

Tương tự như vậy chúng ta có thể dễ dàng làm việc ra 3 trường hợp khác tức là. (q = 1, p = 1), (q = 0, p = 1) và (q = 0, p = 1).

Vì vậy, chúng ta cần phải thay đổi cấu trúc của chúng ta ở đây, chúng ta cũng giữ số lượng các nút lá có thể truy cập từ nút hiện tại nếu ta đi sang bên trái và tương tự cho phía bên phải. Bởi vì, nếu không, sự phức tạp sẽ tăng lên, nếu chúng ta đi qua toàn bộ cây một lần nữa và một lần nữa. Chúng ta có thể làm điều này trong khi chèn số vào cây rất dễ dàng.

Vấn đề này đã được thiết lập trong CodeCraft'14. Bạn có thể thực hành ở đây: [SPOJ.com - Problem SUBXOR][6]

Bây giờ, chúng ta hãy nói về cách triển khai.
Để thực hiện một trie trong C / CPP, chúng ta có thể giữ các nút và các con trỏ trái và phải. Chúng ta có thể viết các hàm đệ quy. 

    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn cũng có, chúng ta dùng đệ quy đi qua cây.

Cập nhật:
Một vấn đề khác khi sử dụng Trie (yay!: P). 
[Vấn đề - B - Codeforces][7]

**Vấn đề phụ**: Cho một nhóm các chuỗi không rỗng. Trong trò chơi, hai người chơi xây dựng từ với nhau, ban đầu từ đó trống. Các cầu thủ di chuyển lần lượt. Trong lượt của anh ta phải thêm một chữ cái vào cuối từ, từ kết quả phải là tiền tố của ít nhất một chuỗi từ nhóm. Một người chơi thua nếu anh ta không thể di chuyển.

Chúng ta cần phải tìm người chơi nào (thứ nhất hoặc thứ hai) có chiến lược chiến thắng.

Vì vậy, ý tưởng ở đây là một lần nữa để xây dựng một trie của tất cả các chuỗi. Tại sao? Bởi vì một trie lưu trữ thông tin về tất cả các tiền tố.
Bây giờ, chúng ta sẽ cố gắng để đánh giá cho mỗi nút nếu người chơi đầu tiên có một chiến lược chiến thắng hay không. Chúng ta có thể làm điều này một cách đệ quy. Đối với nút v, đối với mỗi nút u sao cho u là con tức thời của v, nếu người chơi đầu tiên có chiến lược thua đối với u, thì đối với cầu thủ đầu tiên của nút v có chiến lược thắng.
Ví dụ: giả sử chúng ta có “abc”, “abd”, “acd”. 
Trie của chúng ta sẽ như sau: 

![][8]

  
Tất cả các nút lá có chiến lược chiến thắng.

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c

  
