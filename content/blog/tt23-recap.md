---
title: "Thách Thức 2023: jalsol's recap"
date: 2023-03-25
extra:
    add_toc: true
---

# Lời nói đầu

[Cuộc thi Học thuật Thách thức](https://www.facebook.com/ThachThuc) là một cuộc thi thường niên, được tổ chức từ năm 2001 bởi Khoa Công nghệ Thông tin, Trường Đại học Khoa học Tự nhiên - ĐHQG TP HCM. Các thí sinh đến từ các trường trong khối ĐHQG HCM, từ năm 1 đến năm 4.

Khác với các bài blog mình từng viết trước đây bằng tiếng Anh (vốn là ngôn ngữ mình hay dùng trên internet và độc giả chủ yếu là các bạn APCS và CLC), lần này chắc mình sẽ viết bằng tiếng Việt, vì cuộc thi chủ yếu sử dụng tiếng Việt.

**NOTE: Mình cũng sẽ chỉ viết về phần thi cuối cùng - Đối đầu Trực tiếp**, vì mình chả có cách nào viết writeup cho Khởi động và Nối mạng Toàn cầu, và mình quá gà ở Lập trình Tiếp sức. Mình cũng chỉ viết những câu nào mình thấy thú vị. **Chú ý là các câu hỏi bằng tiếng Anh thì cuộc thi yêu cầu phải trả lời bằng tiếng Anh.**

Mình sẽ tiếp nhận ý kiến của mọi người! Nếu có thiếu sót gì, hãy [nhắn tin](https://www.facebook.com/jalsol.page)/[email](mailto:jalsol@protonmail.com) mình. Cảm ơn các bạn rất nhiều!

# Vòng loại bảng A

## Xàm xí 😮

Trận này có sự xuất hiện của Seno, một đội rất mạnh và đã chinh chiến nhiều năm. Ở vòng loại thì Seno đã hủy diệt hai đối thủ còn lại, và chỉ "troll" xí nên mới không được 500 điểm (nếu không có sự cố kỹ thuật của phần Lập trình Tiếp sức không lỗi thì con số có thể lên tới 600).

Mình vẫn ấn tượng với anh Quang Long của Seno, nhìn khá đụt nhưng đến Nối mạng Toàn cầu thì múa cực kì dẻo và mãn nhãn, chưa kể việc truyền tin ngược để thuận chiều đọc của đồng đội. Đội cũng đã nhấn chuông cược 60 điểm tối đa ở phần chơi đó.

## Bao thư số 6

**Q: For the "little-endian" architecture, which bit represents the LSB (Least Significant Bit) in the binary number?**

Đây là câu hỏi rất lú.

Thứ tự lưu trữ một giá trị dưới dạng little-endian hay big-endian *thường* là thứ tự các **byte,** không phải các bit. Mặc dù [bit endianness tồn tại](https://en.wikipedia.org/wiki/Endianness#Bit_endianness), hiếm có kiến trúc nào có từng địa chỉ riêng cho từng bit. Trong khi đó, đa số các kiến trúc bây giờ đều có từng địa chỉ riêng cho từng byte.

Nếu câu hỏi là về các **byte** thay vì các **bit** thì đáp án là "The smallest byte". Câu trả lời của Seno cũng chỉ cần thay đổi từ ngữ như đề bài thì mọi thứ là hợp lệ. Câu trả lời của ClosedAI cũng hợp lý nếu hiểu câu hỏi thành "index of the bit at the lowest position" hay mấy thứ đại loại vậy: bit thứ 8 (1-indexed) sẽ là đáp án.

Vì đây là câu hỏi chiếm thủ đô, nên câu này là câu tự luận. Các bạn có thể lý luận đủ thứ và ~~cosplay Bruno Fernandes để pressing~~ ý kiến với giám khảo.

## Bao thư số 7

**Q: Trong C++, so sánh tốc độ giữa 2 câu lệnh `i++` và `++i`.**

Đây là một câu rất cơ bản. `++i` sẽ nhanh hơn `i++` ở *hầu hết* các trường hợp.

`++i` sẽ thực hiện cộng trước khi tính. Vì vậy, chỉ cần một phép toán (cụ thể là lệnh `add` trong Assembly) vào biến `i` là mọi thứ đã hoàn tất.

`i++` sẽ thực hiện tính trước khi cộng. Vì vậy, chương trình sẽ tạo ra một bản sao của `i` (hay `mov` nó vào một địa chỉ khác), thực hiện phép tính, gán lại giá trị của `i` về vị trí ban đầu và thực hiện phép cộng.

Tuy nhiên, trong các trường hợp mà `i++` và `++i` có thể thay thế cho nhau, khi bật **optimization** lúc compile thì 2 lệnh này sẽ giống hệt nhau:

```cpp
// i++ hay ++i đều nhanh như nhau
for (int i = 0; i < n; ++i) {
    // ...
}

int x = 5;
x++; // ++x hay x++ đều nhanh như nhau
std::cout << x << '\n';
```

## Bao thư số 3

**Q: Cho biết kết quả in ra màn hình của đoạn code C sau:**

```c
#include <stdio.h>
int main() {
    int array[3] = {5};
    int i;
    for (i = 0; i <= 2; i++)
        printf("%d ", array[i]);
    return 0;
}
```

Seno làm sai câu này. Mình phải hỏi anh Hiệp (member mình nghĩ là mạnh nhất của Seno) để xem có phải họ cố tình sai để trận đấu thêm hấp dẫn không. Câu trả lời là không =)))) Chỉ là trong một giây phút nào đó, các thành viên bị xao nhãng nên viết sai đáp án thôi.

Một số bạn mới học, chưa hiểu bản chất thì sẽ làm sai câu này. Đây là một ví dụ phản biện: nếu `int array[3] = {5};` mà cho ra mảng 3 số 5, vậy thì `int array[3] = {5, 2};` sẽ cho ra gì?

Ta có quy luật kiểu như sau:

- `{}` thì output là `0 0 0`
- `{5}` thì output là `5 0 0`
- `{5, 2}` thì output là `5 2 0`
- `{5, 2, 3}` thì output là `5 2 3`

# Vòng loại bảng B

## Xàm xí 😩

Thực ra thì trận này mình không kịp xem vì phải về điểm danh môn Kinh tế Chính trị của thầy Ngô Tuấn Phương (nhưng vẫn bị đánh vắng chỉ vì giao tiếp lỗi với mấy đứa ở trong lớp).

Team Infinitum lúc đầu có mời mình tham gia, cơ mà mình thấy team có vẻ train rất nghiêm túc, trong khi mình chỉ muốn thi chơi nên mình không muốn vào. Team cũng rất cố gắng vì vẫn thi đấu với 4 người (vắng 1 vì lý do xàm xí).

# Vòng loại bảng C

## Xàm xí 😡

Lúc đầu mình được bạn Hải Minh của HCMUS-eggs hỏi: nếu hai đồng đội ICPC của mình tham gia thì mình có tham gia không? Mình trả lời là có thể. Bằng một lý do thần kì nào đó, bạn ấy lại hiểu thành "chỉ khi hai người kia tham gia thì mình mới tham gia". Và đúng là một trong hai người đồng đội kia không tham gia thật. Mình đang chờ đến ngày được liên lạc để đăng ký thi, nhưng chẳng có thông tin gì cả. Và vậy là họ lắp thêm 2 chị nữa từ APCS, tham gia thi với đội hình chắp vá. Ít nhất thì bạn Hải Minh cũng cho mình con fitbot để xin lỗi rồi.

Ngoài ra, rất xui cho HCMUS-eggs khi lọt vào bảng đấu có hai đội ứng cử viên vô địch: Disgraduates và Renamed. Cả hai đội này đều từng là Á quân và Quý quân Thách thức các mùa trước. Team Renamed là team duy nhất tại vòng loại đạt 100 điểm vòng Khởi động. Team Disgraduates, với slogan "Seno vô địch", lật kèo rất mạnh tại phần Lập trình Tiếp sức với 100 điểm tuyệt đối.

Chưa hết, đố các bạn biết tại sao tên đội là HCMUS-eggs? Bật mí là ý nghĩa mà Việt Hoàng giải thích cho MC chỉ là ý nghĩa tự chế, chứ chả phải ý nghĩa thật đâu 😉

## Bao thư số 5

**Q: Trong ngôn ngữ C, cho đoạn chương trình (với các thư viện đã được include đầy đủ), kết quả xuất ra màn hình là gì?**

```c
int main() {
    char *Ten;
    Ten = new char[256];
    Ten = "Thach thuc 2023";
    Ten[strlen(Ten) - 1] = '4';
    printf("%s", Ten);
}
```

Đội Disgraduates bị lừa. Các bạn có khi cũng bị lừa. Mình cũng đã từng bị lừa trước kia.

Đáp án là chương trình vẫn dịch bình thường nhưng sẽ chạy lỗi. Mình có nói chi tiết cách mình mò ra được tại sao nó lỗi [tại đây](/blog/cs162-notes/#char-x-alice).

Tóm tắt thì `"Thach thuc 2023"` là một xâu hằng, và `Ten` chỉ là một con trỏ đang trỏ vào một xâu hằng mà thôi. Việc thay đổi giá trị của một xâu hằng là không được phép, và sẽ gây ra lỗi.

Nếu các bạn khai báo `char Ten[256] = "Thach thuc 2023";` thì các bạn sẽ thay đổi được bình thường. Lý do là vì với cú pháp này, chương trình sẽ sao chép nội dung xâu hằng vào xâu mới (xâu `Ten` là xâu mới, không phải xâu hằng, nhưng nội dung là sao chép từ xâu hằng), nên các bạn chỉnh sửa được.

Đây là một câu xuất hiện trong bài thực hành môn CS162 (tương đương môn Kỹ thuật Lập trình) của tụi mình. Người ra đề thực hành là thầy Hồ Tuấn Thanh. Tuy nhiên, hồi đó thầy không để ý đến lỗi này, cho đến khi sinh viên báo lại với thầy. Và trong bảng đấu này, thầy cũng chính là thành viên ban giám khảo luôn 😀

# Vòng loại bảng D

## X…xàm xí 😳

Trong trận này, [mình có dành lời khen cho bạn MC trước mọi người](https://www.facebook.com/plugins/video.php?height=314&href=https%3A%2F%2Fwww.facebook.com%2FThachThuc%2Fvideos%2F742914470951799%2F&t=1210) rằng bạn là **MC xinh nhất trong cả 4 trận đấu.** Cơ mà, ý kiến của mình giờ có hơi khác một chút: bạn là **MC xinh nhất trong cả 6 trận đấu** *(và có lẽ là cả mùa Thách thức 2023).* Thậm chí mình còn nhớ là tầm tháng 07/2022, bạn có kết bạn Facebook mình (thực ra là kết bạn với tất cả mọi người) lúc mình đang còn núp trong group chat K22 CLC. À mà hình như bạn ngoài đời xinh hơn trong ảnh 😊

Tuy nhiên, vì mình cũng từng mấy lần phát biểu trước đám đông nên mình vẫn nghĩ là bạn cần phải cải thiện nhiều hơn trong cách điều khiển giọng điệu, ngắt nhịp và xử lý tình huống sao cho tự nhiên hơn. Cũng khó mà có thể mong chờ bạn làm tốt như chị Ngọc Thảo 👰 nếu bạn chưa quen, và mình cũng chân thành xin lỗi nếu mình làm bạn ngượng hay khó chịu trên sân khấu.

MC nam có vẻ không am hiểu âm nhạc lắm: tên của đội Jazzy đến từ sự ngẫu hứng, và sự ngẫu hứng đó là một yếu tố đặc trưng của dòng nhạc Jazz. Mà chẳng sao cả, cuộc thi học thuật chứ có phải cuộc thi về âm nhạc đâu :>

## Bao thư số 4

**Q: Để kết nối một ổ cứng SSD vào máy tính, ta cần sử dụng loại cổng nào?**

**A. PCI-E; B. Thunderbolt 3; C. SATA; D. AAUI**

Câu hỏi này có hai đáp án: A và C.

Với các loại SSD ngày trước (và bây giờ vẫn còn dùng rất nhiều), cổng được sử dụng là cổng SATA. Cổng SATA III vừa có thể dùng để kết nối rất rất nhiều các HDD và SSD ngày nay. Đây cũng là đáp án của ban giám khảo và hai đội chơi, nên tất nhiên là chả ai ý kiến gì cả.

Tuy nhiên, với những loại SSD NVMe đời mới, gọn nhẹ, tốc độ cao và ít dùng điện thì sẽ được kết nối vào cổng PCIe. Với những bạn có kiến thức về linh kiện máy tính, NVMe không còn quá xa lạ gì nữa.

## Bao thư số 2

**Q: Đâu là đầu ra khi chạy chương trình sau (biết mọi thư viện cần thiết đã được thêm vào).**

```c
struct Animal {
    Animal() { cout << "Hi animal "; }
    ~Animal() { cout << "Bye animal "; }
};
struct Dog : public Animal {
    Dog() { cout << "Hi dog "; }
    ~Dog() { cout << "Bye dog " }
};
int main() {
    Dog* a = new Dog;
    delete a;
}
```

**A. Hi animal Hi dog Bye animal Bye dog**

**B. Hi animal Hi dog Bye dog Bye animal**

**C. Hi dog Bye dog**

**D. Không đáp án nào đúng**

Đây là câu hỏi gây tranh cãi. Một vài tình huống trao đổi giữa thí sinh, ban giám khảo và mình (ừ, đúng rồi đấy, là mình) đã diễn ra.

Đội Jazzy chọn đáp án B. Nếu các bạn học OOP rồi thì đáp án B không có gì lạ:

- Base construct → Derived construct
- Derived destruct → Base destruct

Đội CSE chọn đáp án D. Đội ý kiến rằng biến được khởi tạo là `new Dog`, không phải `new Dog()` nên constructor sẽ không được gọi. Sau khi ban giám khảo hội ý, đáp án của đội CSE được chấp nhận. Sau khi trận đấu kết thúc, anh* Đỗ Trọng Lễ - thành viên ban giám khảo, phát biểu rằng *hành vi này là tùy vào compiler*.

Tuy nhiên, mình đã xin ý kiến ngay lập tức. Cảm ơn bạn Hải Minh (là bạn ở đội HCMUS-eggs ấy) đã gọi anh Lễ giúp mình.

Constructor sẽ luôn được gọi, dù cho là `new Dog` hay là `new Dog()`. Đây là điểm ưu việt của từ khóa `new` trong C++ so với `malloc` của C. Các bạn có thể đọc từ [C++ Core Guidelines](https://isocpp.org/wiki/faq/ctors#arrays-call-default-ctor) (bởi các thành viên Hội đồng C++) và [Technical FAQ của Bjarne Stroustrup](https://stroustrup.com/bs_faq2.html#malloc) (người tạo ra C++).

> The C++ operators new and delete guarantee proper construction and destruction; where constructors or destructors need to be invoked, they are.
> 
- Mấy câu chuyện bên lề của cái vụ này
    
    Anh Phát (team Culmination) hoàn toàn đồng ý với mình. Anh Hiệp (team Seno) lại bảo là cứ code đi rồi biết, tuy nhiên cách này sẽ không đảm bảo được là hành vi có tùy vào compiler hay không.
    
    Dẫu vậy, D vẫn là đáp án chính xác, vì dòng `cout` trong destructor của `Dog` bị thiếu một dấu `;` ở sau cùng =))))))))) Điều này được anh Hiệp chỉ ra khi đang code lại.
    
    Với việc chọn đúng câu trả lời nhưng giải thích sai, có hai khả năng xử lý. Một trong hai sẽ ảnh hưởng đến suất của đội nhì bảng có điểm cao thứ ba, nên tình hình rất căng thẳng (cơ mà đằng nào thì đội nhì bảng E sắp tới cũng đá được cả hai đội còn lại nên kết quả chả ảnh hưởng lắm).
    
    Mình biết ban giám khảo không chắc chắn về điều này, họ cũng thử tìm kiếm trên mạng. Mình cũng thử kiếm trên Google, nhưng đúng là hơi khó để tìm ra một câu trả lời cứng trên mạng. Mình phải cố gắng nhớ xem mình đã đọc điều này ở đâu, lúc đó thì mình mới dám phát biểu sửa sai cho ban giám khảo ở một cuộc thi học thuật được. Thật ra, ban giám khảo lại chủ yếu tập trung vào nghiên cứu AI, ít khi phát triển bằng C++. Mình hiểu điều này, nhưng vì tính chất học thuật của cuộc thi nên mình có hơi thất vọng chút.
    

*(\*): Mình gọi là anh vì anh Lễ là trợ giảng môn CM101 (Kỹ năng giao tiếp) của mình, và anh Lễ cũng tự xưng là "anh". Một số đàn anh của mình thì gọi là "thầy Lễ", vì anh Lễ là trợ giảng môn SC203 (Phương pháp nghiên cứu khoa học) của họ.*

# Vòng loại bảng E

## Xàm xí 😎

Trận này cũng không có câu nào thú vị ._.

Trận này có sự xuất hiện của team PatternDesigners - team gồm toàn người thân với mình. PatternDesigners thọt ở phần Nối mạng Toàn cầu, nhưng sự xuất sắc ở Đối đầu Trực tiếp đã cứu họ vào suất cuối cùng vào Bán kết - đội nhì bảng có điểm cao thứ ba. Mặc dù anh Thiên Phúc 🤵 hơi bóp khi chọn sai thành phố để tấn công ở câu cuối và đánh mất quyền tự quyết vào bán kết, song các đội ở bảng F đều thọt nên các anh tôi vẫn có slot.

# Vòng loại bảng F

## Xàm xí 😈

Lần nữa, mình chả thấy câu nào vui.

Tuy nhiên, trận này là trận quyết định xem mấy ông anh team PatternDesigners có vào được bán kết không. Lần này mình hơi mạnh mồm khi ở phần minigame, mình tuyên bố rằng mình không ủng hộ đội nào vì mình muốn cả ba đội đều thọt (thực ra cả ba đội đều đang thọt sẵn rồi). Tuy nhiên vì thời gian gấp gáp (ban tổ chức làm bảng C hơi lâu, các bạn hậu cần cũng phải ăn trưa khá trễ) nên mình đếch được chơi. Ít nhất thì truyền đi thông điệp là OK rồi. Mình là kẻ phản diện mà 😈

Trận này có team Megakill của Trí Phan từ UIT với khẩu hiệu "[Chọn UIT – Chọn để dẫn đầu](https://www.uit.edu.vn/chon-uit-chon-de-dan-dau)", ngay trên lãnh địa HCMUS. Megakill tự bắn vào chân ở câu hỏi cuối cùng rất đáng tiếc, mất đi suất nhất bảng (và không thể cạnh tranh suất nhì bảng vì điểm thọt). Ngoài ra, team Unity cũng là team được mong chờ sẽ thắng trận (và hình như cũng là team duy nhất thi nghiêm túc trong 3 đội, mình phỏng đoán theo lời anh Vinh Hiển của team PatternDesigners), và họ thắng thật. King Penguins có sự xuất hiện của Trần Minh Nam - thủ khoa bảng vàng APCS20 năm vừa rồi (còn lại 4 người kia thì mình chịu).

Sau khi cả 3 team đều thọt phần Nối mạng Toàn cầu, mình đã chúc mừng PatternDesigners luôn. Kết thúc trận, mình được chứng kiến màn phucthao cực cuti và lãng mạn.
