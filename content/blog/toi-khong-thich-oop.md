---
title: "[vi] tôi không thích OOP và sự \"ảo\" OOP"
date: 2025-04-16
extra:
description: "Tôi không thích OOP. Lạm dụng OOP có thể hại nhiều hơn lợi. Tôi ghét những kẻ sùng bái OOP và design pattern."
taxonomies:
    tags: ["vietnamese", "oop", "programming", "software engineering"]
---

[Read the English version here.](/blog/i-dont-like-oop/)

Hôm nay có dịp tôi đào lại một bài nói của anh Đinh Ngọc Thành - Chief Architect tại Holistics, công ty (cũ) của tôi. Vì là một bài tôi rất thích nên tôi chia sẻ ở đây, và góp thêm vài lời của bản thân.

TL;DR: Tôi không thích OOP. Lạm dụng OOP có thể hại nhiều hơn lợi. Tôi ghét những kẻ sùng bái OOP và design pattern. Anh Thành có phân tích một góc nhìn khác của OOP trong video. Tôi thích góc nhìn đấy và tôi nghĩ mọi người cũng nên như vậy.

<div style="display: flex;">
  <iframe style="aspect-ratio: 16 / 9; width: 100% !important;" src="https://www.youtube.com/embed/7PGCvpJl_0o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

{{hr()}}

OOP cực kì phổ biến. OOP được dạy ngay từ năm 1, năm 2 trên đại học. Các ngôn ngữ phổ biến, các framework thông dụng trong mấy chục năm đều có sự xuất hiện của OOP. Mọi người nói dùng OOP thì code gọn hơn, linh hoạt hơn, phát triển tốt hơn. Mọi người học OOP, học design pattern, thậm chí có người còn sùng bái cứng nhắc.

Tôi không thích OOP (hoặc, ít nhất, là cái kiểu OOP mà mọi người đã quen). Tôi ghét việc có những người tranh cãi trên mạng (rất ngu, tôi biết) và thậm chí là các thầy coi OOP là nhất (tôi đã thất vọng về đào tạo CNTT ở VN).

Có một nghịch lý rằng: mặc dù OOP đã tồn tại rất, rất lâu cùng với sự phát triển của software engineering, nhưng OOP lại không có nền tảng lý thuyết vững chắc. Functional Programming (paradigm tôi thích nhất hiện tại) có nền từ lambda calculus, còn Imperative Programming thì có nền từ Turing machine. OOP thì ngược lại: người ta sinh ra OOP cả mấy chục năm, mà người ta mới chỉ chế [một cái lý thuyết cho nó cách đây vài năm](https://arxiv.org/abs/2111.13384).

Một vấn đề lớn trong OOP nói riêng và trong software engineering nói chung là quá nhiều abstraction. Môn CM101 dạy tôi rằng abstraction là một tính chất quan trọng. Tuy nhiên, quá nhiều abstraction lại tăng complexity của hệ thống: hiệu năng kém, phát triển khó, debug khó, control flow phức tạp... Đôi khi abstraction không giảm complexity xuống, mà chỉ chia nhỏ rồi rải ra hoặc ẩn đi. Trong khi, nếu bọc raw data bằng struct và xử lý trực tiếp thì mọi chuyện lại đơn giản hơn. Không phải ngẫu nhiên mà những ngôn ngữ hiện đại như Go và Rust không có class hay inheritance mà chỉ có struct.

Có những design pattern trong OOP sinh ra chỉ vì người ta muốn "lách" những thứ mà đúng ra làm được rất dễ trong các paradigm khác, như Singleton hay Facade. Hoặc, việc bundle state và behavior với nhau thành một class sẽ gây khó khăn trong việc test các behavior (vì phải đạt đúng state, đúng dependency mới replicate được behavior).

Tôi lại thích cách nhìn nhận rằng OOP chỉ là một phương thức để các object truyền dữ liệu cho nhau, thông qua một contract/interface. Góc nhìn này về OOP không bị ràng buộc về class và inheritance, thoáng hơn, trực diện hơn, và có nhiều phương án xử lý hay tích hợp các paradigm khác vào (như Functional Programming). Mọi người có thể xem video trên để hiểu thêm.
