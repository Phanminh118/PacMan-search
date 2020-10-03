## Mục lục
* [Cách chơi
* [Tìm kiếm không được thông tin]
    * [Vấn đề: Tìm một số thức ăn trong một không gian tìm kiếm nhỏ]
        * [Độ sâu-Tìm kiếm đầu tiên]
        * [Theo chiều rộng-Tìm kiếm đầu tiên]
        * [So sánh]
    * [Vấn đề: Tìm một số thức ăn trong không gian tìm kiếm rộng lớn]
        * [Độ sâu-Tìm kiếm đầu tiên]
        * [Theo chiều rộng-Tìm kiếm đầu tiên]
        * [So sánh]
* [Tìm kiếm thông tin]
    * [Vấn đề: Tìm tất cả thức ăn]
        * [Độ sâu-Tìm kiếm đầu tiên]
        * [Theo chiều rộng-Tìm kiếm đầu tiên]
        * [A * Tìm kiếm]
        

## Cách chơi
Sử dụng WASD hoặc phím mũi tên để điều khiển Pac-Man. Để bắt đầu một trò chơi tương tác, hãy nhập vào dòng lệnh:

~~~~
python pacman.py
~~~~

Để xem Pac-Man hoạt động như thế nào bằng các thuật toán tìm kiếm, chúng tôi có thể xác định một số biến


## Tìm kiếm Không được Thông tin
Với các đầu vào:
* trạng thái ban đầu - ô mê cung mà Pac-Man bắt đầu,
* chức năng kế thừa - các hành động khả thi mà anh ta có thể thực hiện (* ví dụ: *, * di chuyển về phía Đông *) và
* trạng thái mục tiêu - ô mà anh ấy muốn ở trong,

thuật toán tìm kiếm trả về một chuỗi các trạng thái dẫn Pac-Man từ trạng thái ban đầu đến mục tiêu. Chiến lược tìm kiếm là tạo lặp đi lặp lại một danh sách (được gọi là * biên giới *) của các trạng thái kế thừa cho đến khi tìm thấy một đường dẫn đến trạng thái mục tiêu.


### Sự cố: Tìm một số thức ăn trong một không gian tìm kiếm nhỏ
Chúng ta có thể so sánh một số thuật toán bằng cách quan sát cách Pac-Man biểu diễn trên `MAZE_TYPE` nhỏ xíu với một chấm thức ăn cố định.

Trạng thái ban đầu của Pac-Man là ở góc trên bên phải của mê cung. Trạng thái mục tiêu của anh ta là một ô chứa thức ăn, trong trường hợp này là ô ở góc dưới bên trái.

#### Độ sâu-Tìm kiếm Đầu tiên (DFS)

** Chi phí: ** Pac-Man tìm thức ăn trong 10 bước.

** Các nút tìm kiếm được mở rộng: ** DFS mở rộng 15 ô trong quá trình tìm giải pháp.

** Chiến lược: ** DFS sử dụng ngăn xếp LIFO (nhập sau cùng, xuất trước) để xây dựng biên giới. DFS thêm một người kế nhiệm vào biên giới và ngay lập tức mở rộng nó; nói cách khác, nó xây dựng một đường dẫn bằng cách khám phá một ô lân cận, sau đó khám phá ô bên cạnh ô đó, rồi ô bên cạnh ô đó, v.v. Ví dụ: nó xác định Pac-Man có thể đi về phía tây hoặc nam từ trạng thái ban đầu, nhưng chọn khám phá con đường đi về phía tây đến cuối trước khi xem xét con đường đi về phía nam.

Đường dẫn mà DFS khám phá đầu tiên được biểu thị bằng màu trắng. Ở cuối con đường này, DFS không có tế bào mới chưa được khám phá và cũng không tìm thấy thức ăn. Do đó, DFS chuyển sang trạng thái chưa được khám phá tiếp theo trên biên giới: nó "lùi" tới các đường giao nhau cuối cùng và tiếp tục đi xuống con đường xanh. DFS tìm thức ăn và đạt được trạng thái mục tiêu.

#### Breadth-First Search (BFS)
Pac-Man cố gắng tìm ra một giải pháp ngắn hạn và mang tính tiến bộ. Tuy nhiên, phải mất gần một phút để BFS tìm ra giải pháp này - BFS mở rộng hơn 16.000 nút trong quá trình tìm kiếm. (Để tham khảo, có 67 nút trong mê cung này.)

Nếu chúng ta muốn nhanh chóng tìm ra một con đường tiết kiệm chi phí, chúng ta cần xác định một phương pháp heuristic.

#### A * Tìm kiếm
Chúng tôi xác định phương pháp heuristic (foodHeuristic) có tính đến các vị trí của thực phẩm hiện có trong mê cung. Bằng cách tính toán khoảng cách từ vị trí hiện tại của Pac-Man đến các chấm thức ăn, chúng ta có thể khuyến khích Pac-Man ăn hết thức ăn nhanh nhất có thể. Thực hiện các món ăn



Pac-Man giờ đây nhanh chóng và tự tin ngấu nghiến tất cả thức ăn trong mê cung. A * chỉ mở rộng 2.748 nút so với 16.688 của BFS và vẫn cố gắng tìm ra một giải pháp ngắn gọn.


#### So sánh
Trong số ba thuật toán, chỉ có BFS tìm ra giải pháp tối ưu. Tuy nhiên, BFS tốn kém không gian vì nó mở rộng một số lượng lớn các nút. DFS tiết kiệm không gian nhưng mang lại một giải pháp dài và kém hấp dẫn. A * Tìm kiếm với heuristic tạo ra một giải pháp rất gần với độ dài tối ưu, nhưng thực hiện điều này mà không sử dụng quá nhiều dung lượng.

Câu 1 : Finding a Fixed Food Dot using Depth First Search
DFS on tinyMaze

Sử dụng giải thuật tìm kiếm theo chiều sâu(DFS):

Các biến sử dụng:

checkedNode : Khởi tạo set dùng để lưu trữ các node đã duyệt

stack: Ngăn xếp dùng để lưu trữ các node để duyệt DFS

state: Lưu trữ để thể hiện trạng thái hiện tại

action: Hành động của pacman

Thuật toán: Duyệt DFS tất cả các node cho đến khi đến trạng thái cuối cùng

Câu 2: Tìm kiếm theo chiều rộng

Thuật toán tương tự tìm kiếm theo chiều sâu nhưng thay vào đó ta lưu các đỉnh đang xét trong queue
Câu 3: Thay đổi hàm chi phí:

Thuật toán tương tự tìm kiếm theo chiều rộng nhưng ta sử dụng hàng đợi ưu tiên để lưu các đỉnh đang xét, các đỉnh có chi pjí thấp hơn sẽ được duyệt trước.
Khi duyệt các nút nếu thấy chi phí thấp hơn chi phí đã có thì gan lại chi phí cho nít này.
Câu 4: Tìm kiếm A*

Thuật toán giống với Câu 3, chỉ thay điều kiện ưu tiên của hàng đợi ưu tiên theo công thức F = H + G
Câu 5:Tìm tất cả các góc

Đầu tiên kiểm tra điểm đầu tiên bắt đầu đi có phải ở góc nào không.
Kiểm tra xem có thể đi đuọc hay không.
Viết hàm kiếm tra Pacman đã đến đủ 4 gọc hay chưa.
Tìm kiếm đường đi đến tất cả các góc bằng 1 trong các thuật toán tìm kiếm DFS,BFS,A*
Câu 6: Bài toán góc: Heurític

Từ điểm khởi đầu, ta liệt kê tất cả quãng đường từ điểm xuất phát cho đến n điểm kết nối với nó rồi chọn đi theo con đường ngắn nhất.
Khi đã đi đến một điểm kết nối với nó, chọn đi đến điểm kế tiếp cũng theo nguyên tắc trên. Nghĩa là liệt kê tất cả con đường từ điểm bên cạnh ta đang đứng đến những điểm chưa đi đến. Chọn con đường ngắn nhất.
Lặp lại quá trình này cho đến lúc không còn đại lý nào để đi.
Câu 7:Ăn hết dấu chấm

Cho tất cả các cạnh vào hàng đợi ưu tiên.
Lấy dần ra các cạnh, kiểm tra các cạnh có tạo chu trình hay không, nếu không thì thêm cạnh đó vào
Làm đến khi đủ n-1 cạnh ta được đường đi ăn tất cả cấc điểm ngắn nhất
Câu 8:

Bắt đầu từ 1 nút, duyệt tất cả các đỉnh liền kề của nút đó, lấy đường đi mà từ nút đng xét đếm nút liền kề có độ dài nhỏ nhất.
Tiếp tục lấy các nút cho đếm khi đủ tất cả các nút.
