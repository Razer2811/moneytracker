TÀI LIỆU PHÂN TÍCH VÀ THIẾT KẾ HỆ THỐNG PHẦN MỀM
Đề tài: Xây dựng ứng dụng quản lý chi tiêu cá nhân đa nền tảng bằng Flutter (tham chiếu ứng dụng CapMoney v1.7.9)
Tháng 10 năm 2026

PHẦN ĐẦU
Trang bìa
[TÊN TRƯỜNG / TỔ CHỨC] [KHOA / BỘ MÔN]
TÀI LIỆU PHÂN TÍCH VÀ THIẾT KẾ HỆ THỐNG PHẦN MỀM
ĐỀ TÀI: ỨNG DỤNG QUẢN LÝ CHI TIÊU CÁ NHÂN "SNAPMONEY" (tên tạm) XÂY DỰNG BẰNG FLUTTER
	
Giảng viên hướng dẫn	[Họ tên]
Sinh viên / Nhóm thực hiện	[Họ tên – MSSV]
Lớp / Khóa	[Lớp – Khóa]
Thời gian thực hiện	[Bắt đầu – Kết thúc]
Thừa Thiên Huế, tháng 10 năm 2026
Lời mở đầu
Chi tiêu cá nhân là nhu cầu hằng ngày nhưng việc ghi chép thường bị bỏ dở vì thao tác nhập liệu mất thời gian. Các ứng dụng hiện đại giải quyết bằng cách giảm số bước ghi chép: chụp ảnh khoản chi, gõ một dòng "cà phê 35k", hoặc nói bằng giọng nói. Ứng dụng CapMoney (phiên bản 1.7.9, giao diện tiếng Việt, tối ưu cho iOS) là một ví dụ điển hình của hướng tiếp cận "ghi nhanh trong vài giây".
Tài liệu này phân tích và thiết kế một hệ thống tương tự bằng Flutter, theo quy trình công nghệ phần mềm: khảo sát, phân tích yêu cầu, phân tích hệ thống, thiết kế kiến trúc, cơ sở dữ liệu, giao diện, API, bảo mật, kiểm thử, triển khai và đánh giá.
Lưu ý về nguồn thông tin. Phần yêu cầu và giao diện được suy ra từ 20 ảnh chụp màn hình của ứng dụng tham chiếu (onboarding, màn hình chính, thêm giao dịch, nhập hàng loạt, thống kê, tài khoản, ngân sách, hồ sơ cá nhân). Các phần kiến trúc, cơ sở dữ liệu, API, bảo mật là thiết kế đề xuất độc lập của tài liệu này, không phải mã nguồn hay kiến trúc thực của CapMoney. Những điểm không thể xác định từ ảnh được đánh dấu "(giả định)".
Lời cảm ơn
Nhóm thực hiện xin gửi lời cảm ơn đến giảng viên hướng dẫn [Họ tên], các thầy cô trong khoa và những người bạn đã tham gia khảo sát, dùng thử và góp ý trong quá trình xây dựng tài liệu. (Phần này có thể xóa nếu không cần.)
Mục lục
•Phần đầu: Trang bìa, Lời mở đầu, Lời cảm ơn, Mục lục, Danh mục từ viết tắt, Danh mục hình ảnh, Danh mục bảng biểu
•Chương 1. Tổng quan dự án
•Chương 2. Khảo sát và phân tích yêu cầu
•Chương 3. Phân tích hệ thống
•Chương 4. Thiết kế kiến trúc và cấu trúc hệ thống
•Chương 5. Thiết kế cơ sở dữ liệu
•Chương 6. Thiết kế giao diện người dùng
•Chương 7. Thiết kế API và tích hợp hệ thống
•Chương 8. Thiết kế bảo mật và xử lý lỗi
•Chương 9. Kiểm thử hệ thống
•Chương 10. Triển khai và vận hành hệ thống
•Chương 11. Đánh giá và hướng phát triển
•Kết luận, Tài liệu tham khảo
•Phụ lục A – H
(Mục lục có số trang có thể được sinh tự động khi xuất sang Word/PDF.)
Danh mục từ viết tắt và thuật ngữ
Viết tắt / Thuật ngữ	Ý nghĩa
API	Application Programming Interface – giao diện lập trình ứng dụng
ARB	Application Resource Bundle – tệp tài nguyên đa ngôn ngữ của Flutter
BR	Business Rule – quy tắc nghiệp vụ
CI/CD	Continuous Integration / Continuous Delivery
CRUD	Create, Read, Update, Delete
DAO	Data Access Object
DTO	Data Transfer Object
ERD	Entity Relationship Diagram – sơ đồ thực thể liên kết
FAB	Floating Action Button – nút nổi (+) trên màn hình chính
FR / NFR	Functional / Non-functional Requirement
FCM / APNs	Firebase Cloud Messaging / Apple Push Notification service
IAP	In-App Purchase – mua trong ứng dụng
JWT	JSON Web Token
MVP	Minimum Viable Product
OCR	Optical Character Recognition – nhận dạng ký tự quang học
Offline-first	Ứng dụng hoạt động đầy đủ khi không có mạng, đồng bộ sau
Onboarding	Chuỗi màn hình giới thiệu và thiết lập khi mở app lần đầu
Coachmark	Hướng dẫn nổi bật từng phần tử giao diện ("Bước 1/9")
Pro	Gói trả phí mở khóa các tính năng nâng cao
Soft delete	Xóa mềm – đánh dấu đã xóa thay vì xóa vật lý
UC	Use Case – ca sử dụng
UI / UX	User Interface / User Experience
VND	Việt Nam Đồng
Danh mục hình ảnh
Số hiệu	Tên hình
Hình 2.1	Sơ đồ Use Case tổng quát
Hình 3.1	Sơ đồ hoạt động – Ghi nhận khoản chi bằng ảnh
Hình 3.2	Sơ đồ tuần tự – Quét hóa đơn và lưu giao dịch
Hình 3.3	Sơ đồ tuần tự – Nhập hàng loạt bằng văn bản/giọng nói
Hình 3.4	Mô hình miền nghiệp vụ
Hình 3.5	Sơ đồ trạng thái – Giao dịch
Hình 3.6	Sơ đồ trạng thái – Gói đăng ký Pro
Hình 4.1	Sơ đồ kiến trúc tổng thể
Hình 4.2	Cấu trúc thư mục dự án
Hình 4.3	Sơ đồ lớp (Class Diagram)
Hình 4.4	Luồng tương tác khi lưu một giao dịch
Hình 5.1	Sơ đồ ERD
Hình 6.1	Sơ đồ điều hướng hệ thống
Hình 10.1	Sơ đồ triển khai và CI/CD
Danh mục bảng biểu
Nhóm	Bảng
Chương 1	1.1 Các bên liên quan; 1.2 Công nghệ và công cụ
Chương 2	2.1 So sánh giải pháp hiện có; 2.2 Tác nhân; 2.3 Yêu cầu chức năng; 2.4 Yêu cầu phi chức năng; 2.5 Quy tắc nghiệp vụ; 2.6 Ràng buộc; 2.7 Danh sách Use Case; 2.8–2.11 Đặc tả UC02, UC04, UC11, UC18
Chương 3	3.1 Quy trình nghiệp vụ; 3.2 Đối tượng nghiệp vụ; 3.3 Luồng ngoại lệ
Chương 4	4.1 So sánh kiến trúc; 4.2 Các tầng; 4.3 Danh sách module; 4.4 Trách nhiệm module; 4.5 Các lớp chính; 4.6 Quyết định thiết kế
Chương 5	5.1 Thực thể; 5.2 Danh sách bảng; 5.3–5.7 Từ điển dữ liệu; 5.8 Ràng buộc toàn vẹn; 5.9 Chỉ mục
Chương 6	6.1 Nguyên tắc giao diện; 6.2 Danh sách màn hình; 6.3 Thành phần giao diện; 6.4 Thông báo và xử lý lỗi
Chương 7	7.1 Danh sách API; 7.2 Mã trạng thái; 7.3 Tích hợp bên ngoài
Chương 8	8.1 Phân quyền; 8.2 Dữ liệu nhạy cảm; 8.3 Lỗ hổng và biện pháp
Chương 9	9.1 Phạm vi kiểm thử; 9.2 Test case; 9.3 Ma trận truy vết
Chương 10	10.1 Yêu cầu phần cứng/phần mềm; 10.2 Môi trường; 10.3 Biến môi trường
Chương 11	11.1 Mức độ đáp ứng yêu cầu

CHƯƠNG 1. TỔNG QUAN DỰ ÁN
1.1. Giới thiệu đề tài
Đề tài xây dựng SnapMoney, một ứng dụng di động quản lý tài chính cá nhân viết bằng Flutter, theo định hướng "ghi nhanh – xem rõ – kiểm soát được". Từ các ảnh chụp màn hình tham chiếu, ứng dụng gồm 5 khu vực chính trên thanh điều hướng dưới:
1.Trang chủ: lời chào, tổng chi/thu, bộ lọc ví, lịch chi tiêu theo tháng/ngày, nút (+) thêm giao dịch.
2.Thống kê: tuần/tháng/năm, thu nhập, chi tiêu, số dư, biểu đồ danh mục và bản đồ chi tiêu.
3.Tài khoản: ví, ngân hàng, sổ tiết kiệm, khoản vay, đầu tư, chuyến du lịch.
4.Ngân sách: tạo nhiều ngân sách theo mẫu.
5.Cá nhân: hồ sơ, nâng cấp Pro, đăng nhập Apple để sao lưu, bạn bè, nhóm, chia tiền, giao dịch định kỳ, danh mục, shortcut, quy đổi giờ làm, cài đặt.
1.2. Lý do chọn đề tài
•Nhu cầu thực tế: quản lý chi tiêu là bài toán gần gũi, dễ lấy yêu cầu và dễ kiểm chứng.
•Phù hợp học tập: chạm đủ các mảng của một dự án di động hoàn chỉnh: giao diện, cơ sở dữ liệu cục bộ, đồng bộ đám mây, camera/OCR/giọng nói, thanh toán trong ứng dụng, thông báo, widget, bảo mật.
•Có mẫu tham chiếu rõ ràng: CapMoney cho thấy một tập tính năng và trải nghiệm đã được thị trường chấp nhận.
•Thế mạnh Flutter: một mã nguồn cho iOS và Android, giao diện nhất quán, hiệu năng tốt.
1.3. Bối cảnh và vấn đề cần giải quyết
Bối cảnh. Thanh toán không tiền mặt tăng nhanh nhưng chi tiêu của người dùng nằm rải rác ở tiền mặt, nhiều ngân hàng, ví điện tử, thẻ tín dụng, các khoản vay và đầu tư.
Vấn đề cần giải quyết:
•Ghi chép tốn công nên dễ bỏ giữa chừng → cần nhập liệu dưới 10 giây.
•Không có cái nhìn gộp về tài sản/nợ → cần mô hình đa tài khoản.
•Biết mình chi quá tay khi đã muộn → cần ngân sách và cảnh báo sớm.
•Chia tiền nhóm, tiền chung gia đình dễ gây nhầm lẫn → cần chia bill và theo dõi ai đã trả.
•Lo ngại mất dữ liệu khi đổi máy → cần sao lưu/đồng bộ nhưng vẫn dùng được khi không có mạng.
1.4. Mục tiêu xây dựng hệ thống
Mục tiêu tổng quát: xây dựng ứng dụng quản lý chi tiêu cá nhân chạy trên iOS và Android, ghi chép nhanh, thống kê trực quan, đồng bộ an toàn.
Mục tiêu cụ thể:
1.Ghi một khoản chi bằng ảnh trong tối đa 3 thao tác chạm sau khi mở app.
2.Hỗ trợ nhập hàng loạt bằng văn bản tự nhiên và giọng nói, tự tách số tiền và đoán danh mục.
3.Quản lý đa tài khoản, ngân sách, giao dịch định kỳ, khoản vay, tiết kiệm, đầu tư.
4.Thống kê theo tuần/tháng/năm, theo danh mục và theo bản đồ; xuất PDF/Excel (Pro).
5.Hỗ trợ bạn bè, nhóm, chia bill; hỗ trợ widget, Siri Shortcut, nhắc nhở.
6.Offline-first, đồng bộ đám mây, bảo vệ dữ liệu bằng khóa sinh trắc học.
7.Giao diện 4 ngôn ngữ: Tiếng Việt, English, 日本語, 繁體中文.
1.5. Phạm vi của hệ thống
Trong phạm vi: ứng dụng di động Flutter (iOS là nền tảng ưu tiên, Android bản song song); dịch vụ backend đám mây (xác thực, đồng bộ, lưu media, xã hội, kiểm tra đăng ký); trang quản trị nhẹ.
Ngoài phạm vi (giai đoạn này): kết nối trực tiếp API ngân hàng/Open Banking; giao dịch chứng khoán thực; cung cấp tư vấn đầu tư; ứng dụng desktop/web cho người dùng cuối.
1.6. Đối tượng sử dụng và các bên liên quan
Bảng 1.1. Các bên liên quan
Bên liên quan	Vai trò / Kỳ vọng
Người dùng cá nhân (học sinh, sinh viên, nhân viên văn phòng)	Ghi chi tiêu nhanh, xem thống kê, kiểm soát ngân sách
Người dùng Pro	Dùng thẻ tín dụng, tiết kiệm, đầu tư, vay, xuất báo cáo, chia sẻ tài khoản
Gia đình / bạn bè / nhóm	Chia bill, quản lý chi tiêu chung
Đội phát triển	Mã nguồn dễ bảo trì, kiểm thử được, phát hành đều đặn
Chủ sản phẩm	Tăng tỷ lệ giữ chân và chuyển đổi sang Pro
Quản trị / hỗ trợ	Xử lý góp ý, hỗ trợ đăng ký, cập nhật danh mục mẫu
Apple / Google	Quy định cửa hàng ứng dụng, thanh toán, quyền riêng tư
1.7. Phương pháp thực hiện
•Quy trình: Agile – Scrum, sprint 2 tuần. Sprint 0 phân tích và thiết kế; các sprint sau làm theo MVP rồi mở rộng.
•Thu thập yêu cầu: phân tích ngược từ ảnh chụp màn hình ứng dụng tham chiếu, khảo sát nhanh người dùng, so sánh sản phẩm tương tự.
•Mô hình hóa: UML (Use Case, Activity, Sequence, Class, State) và ERD; thiết kế giao diện bằng Figma.
•Kiến trúc: Clean Architecture theo hướng feature-first, quản lý trạng thái bằng Riverpod.
•Kiểm thử: kiểm thử đơn vị, tích hợp, hệ thống và chấp nhận; CI chạy tự động.
1.8. Công nghệ và công cụ sử dụng
Bảng 1.2. Công nghệ và công cụ
Nhóm	Lựa chọn	Mục đích
Ngôn ngữ / Framework	Dart 3, Flutter 3.x (stable)	Xây dựng ứng dụng đa nền tảng
Quản lý trạng thái	flutter_riverpod	Trạng thái, DI, bất đồng bộ
Điều hướng	go_router	Điều hướng khai báo, deep link
CSDL cục bộ	Drift (SQLite)	Lưu dữ liệu offline, truy vấn thống kê
Backend	Firebase Auth, Cloud Firestore, Cloud Storage, Cloud Functions (TypeScript), FCM	Xác thực, đồng bộ, media, API, thông báo
Camera / ảnh	camera, image_picker	Chụp và chọn ảnh/clip
OCR	google_mlkit_text_recognition (trên thiết bị)	Đọc hóa đơn, trích số tiền
Giọng nói	speech_to_text	Nhập liệu bằng giọng nói theo "ngôn ngữ nói"
Vị trí / bản đồ	geolocator, geocoding, google_maps_flutter	Gắn vị trí, bản đồ chi tiêu
Biểu đồ	fl_chart	Biểu đồ danh mục, xu hướng
Thanh toán	purchases_flutter (RevenueCat) hoặc in_app_purchase	Gói Pro
Đăng nhập	sign_in_with_apple, google_sign_in	Liên kết tài khoản để sao lưu
Widget / Shortcut	home_widget, App Intents (kênh nền tảng)	Widget màn hình chính, Siri
Thông báo	flutter_local_notifications, FCM	Nhắc nhở hóa đơn, khoản vay, sao kê
Bảo mật	local_auth, flutter_secure_storage	Khóa sinh trắc học, lưu khóa an toàn
Xuất báo cáo	pdf, excel, share_plus	Xuất PDF/Excel
Đa ngôn ngữ	flutter_localizations, intl (ARB)	4 ngôn ngữ
Kiểm thử	flutter_test, mocktail, integration_test	Unit, widget, integration
CI/CD	GitHub Actions, Fastlane, TestFlight, Firebase App Distribution	Build, kiểm thử, phát hành
Thiết kế / Tài liệu	Figma, draw.io / PlantUML, Git	Giao diện, sơ đồ, quản lý mã nguồn
1.9. Cấu trúc tài liệu
Tài liệu gồm 11 chương: Chương 1 (tổng quan), Chương 2 (khảo sát và yêu cầu), Chương 3 (phân tích), Chương 4 (kiến trúc), Chương 5 (cơ sở dữ liệu), Chương 6 (giao diện), Chương 7 (API), Chương 8 (bảo mật), Chương 9 (kiểm thử), Chương 10 (triển khai), Chương 11 (đánh giá); kèm phụ lục A–H.

CHƯƠNG 2. KHẢO SÁT VÀ PHÂN TÍCH YÊU CẦU
2.1. Khảo sát hiện trạng
Bảng 2.1. So sánh giải pháp hiện có (đánh giá chung, mang tính định tính)
Tiêu chí	Sổ tay / Excel	Ứng dụng ngân hàng	Ứng dụng quản lý chi tiêu phổ thông	Ứng dụng tham chiếu (CapMoney)
Tốc độ ghi chép	Chậm	Tự động nhưng chỉ trong 1 ngân hàng	Trung bình (nhiều bước)	Nhanh: chụp ảnh, gõ 1 dòng, giọng nói
Gộp nhiều nguồn tiền	Thủ công	Không	Có	Có: ví, ngân hàng, thẻ, tiết kiệm, vay, đầu tư
Ngân sách / cảnh báo	Tự tính	Hạn chế	Có	Có: tuần/tháng/kỳ tùy chọn, quỹ theo mục tiêu
Chia bill, chi tiêu nhóm	Không	Không	Tùy ứng dụng	Có: bạn bè, nhóm, chia tiền, nhắn tin
Hoạt động offline	Có	Cần mạng	Tùy ứng dụng	Có (giả định: dữ liệu cục bộ, sao lưu khi đăng nhập Apple)
Tiện ích hệ điều hành	Không	Có hạn	Có hạn	Widget, Siri Shortcut, lưu từ ảnh chụp màn hình
2.2. Quy trình nghiệp vụ hiện tại
Quy trình thủ công phổ biến:
1.Phát sinh khoản chi (mua hàng, ăn uống, di chuyển) hoặc khoản thu (lương, thưởng).
2.Giữ hóa đơn hoặc tự nhớ; cuối ngày/cuối tuần mới nhớ lại.
3.Mở sổ/Excel/ứng dụng, nhập số tiền, chọn danh mục, ghi chú.
4.Cuối tháng cộng tổng, so với dự định và nhận ra đã chi vượt.
5.Với khoản chung: nhắn tin đòi tiền từng người và tự đối chiếu ai đã trả.
2.3. Những hạn chế của hệ thống hiện tại
•Quên hoặc nhớ sai số tiền vì ghi muộn; mất hóa đơn giấy.
•Số liệu rời rạc, khó có bức tranh tổng thể giữa tiền mặt, ngân hàng, thẻ và nợ.
•Cảnh báo ngân sách đến muộn; không có nhắc nhở hóa đơn, sao kê thẻ, hạn trả nợ.
•Chia tiền nhóm thiếu minh bạch, tốn thời gian đối chiếu.
•Dữ liệu dễ mất khi mất/đổi điện thoại hoặc hỏng tệp.
2.4. Đề xuất giải pháp
Xây dựng SnapMoney với các nguyên tắc: (1) mọi thao tác ghi chép có đường tắt (ảnh, văn bản, giọng nói, widget, Siri); (2) dữ liệu lưu cục bộ trước, đồng bộ sau (offline-first); (3) cấu trúc đa tài khoản và ngân sách linh hoạt; (4) xã hội hóa vừa đủ (bạn bè, nhóm, chia bill); (5) mô hình Freemium: tính năng cốt lõi miễn phí, tính năng nâng cao thuộc gói Pro.
2.5. Xác định các tác nhân (Actors)
Bảng 2.2. Danh sách tác nhân
Mã	Tác nhân	Mô tả
A1	Người dùng mới (ẩn danh)	Mới cài app, dùng ngay không cần đăng ký (tài khoản ẩn danh)
A2	Người dùng thường (Free)	Dùng các tính năng miễn phí
A3	Người dùng Pro	Có quyền truy cập tính năng Pro
A4	Bạn bè / thành viên nhóm	Người dùng khác tham gia chia bill, nhóm, chia sẻ
A5	Apple/Google	Đăng nhập, thanh toán và xác thực biên lai
A6	Dịch vụ OCR/AI	Đọc hóa đơn, phân loại danh mục (tác nhân hệ thống)
A7	Dịch vụ thông báo	APNs/FCM và bộ lập lịch cục bộ
A8	Quản trị viên	Quản lý danh mục mẫu, góp ý, hỗ trợ đăng ký
2.6. Yêu cầu chức năng (Functional Requirements)
Độ ưu tiên theo MoSCoW: M (Must), S (Should), C (Could).
Bảng 2.3. Yêu cầu chức năng
Mã	Yêu cầu	Ưu tiên	Pro
FR01	Tạo hồ sơ lần đầu: tên, ảnh đại diện, tiền tệ (tự động theo vùng), ngôn ngữ giao diện, ngôn ngữ nói	M	
FR02	Onboarding 6 màn hình giới thiệu và coachmark hướng dẫn 9 bước	S	
FR03	Đăng nhập với Apple (và Google trên Android) để liên kết tài khoản, sao lưu	M	
FR04	Khóa bảo mật ứng dụng (Face ID/Touch ID/vân tay/PIN)	S	
FR05	Đổi ngôn ngữ, giao diện sáng/tối, tiền tệ	M	
FR06	Thêm giao dịch chi/thu: số tiền, danh mục, tài khoản, ngày, ghi chú	M	
FR07	Đính kèm ảnh từ camera hoặc thư viện; có thể bỏ qua ảnh	M	
FR08	Quét hóa đơn thông minh, tự điền số tiền	S	
FR09	Quay clip 3 giây cho khoảnh khắc đáng nhớ	C	✔
FR10	Thêm ghi chú, cảm xúc và vị trí cho giao dịch	S	
FR11	Nhập hàng loạt bằng văn bản: mỗi dòng một giao dịch, hiểu "35k", "1tr2", dòng bắt đầu "+" là thu	M	
FR12	Nhập hàng loạt bằng giọng nói theo ngôn ngữ nói đã chọn	S	
FR13	Sửa, xóa, khôi phục giao dịch	M	
FR14	Tìm kiếm và lọc giao dịch (từ khóa, danh mục, tài khoản, ngày, số tiền)	M	
FR15	Xem lịch chi tiêu theo tháng và danh sách theo ngày	M	
FR16	Ẩn/hiện số dư và số tiền trên màn hình	S	
FR17	Quản lý tài khoản/ví (thêm, sửa, lưu trữ, đặt mặc định), chuyển tiền nội bộ	M	
FR18	Thẻ tín dụng có hạn mức và ngày sao kê	S	✔
FR19	Sổ tiết kiệm: lãi suất, kỳ hạn, ngày đáo hạn, lãi tích lũy	S	✔
FR20	Đầu tư vàng và chứng khoán	C	✔
FR21	Theo dõi khoản cho vay và đi vay	S	✔
FR22	Chuyến du lịch: gom các chi tiêu theo chuyến	C	
FR23	Ngân sách theo tuần, tháng hoặc kỳ tùy chọn; tạo từ mẫu "Chi tiêu hàng tháng"	M	
FR24	Cảnh báo khi chi tiêu đạt 80% và 100% ngân sách	S	
FR25	Quỹ tiết kiệm riêng cho từng mục tiêu	S	
FR26	Giao dịch định kỳ tự động ghi mỗi kỳ	M	
FR27	Thống kê theo danh mục và xu hướng theo thời gian (tuần/tháng/năm)	M	
FR28	Bản đồ chi tiêu theo vị trí	C	
FR29	Xuất báo cáo PDF/Excel	S	✔
FR30	Quy đổi giờ làm: số tiền tương ứng bao nhiêu giờ lao động	C	
FR31	Quản lý bạn bè (mời, chấp nhận, xóa)	S	
FR32	Quản lý nhóm	S	
FR33	Chia bill và theo dõi ai đã trả	S	
FR34	Gửi khoảnh khắc chi tiêu cho bạn bè và nhắn tin trong ứng dụng	C	
FR35	Chia sẻ tài khoản, ngân sách với người thân	C	✔
FR36	Widget màn hình chính xem số dư, ngân sách; mở nhanh vào camera	S	
FR37	Siri Shortcut và lưu nhanh từ ảnh chụp màn hình	C	
FR38	Nhắc nhở hóa đơn, khoản vay, sao kê thẻ	S	
FR39	Quản lý danh mục (thêm, sửa, sắp xếp, ẩn)	M	
FR40	Mua, khôi phục và kiểm tra quyền gói Pro	M	
FR41	Đồng bộ đám mây, sao lưu và khôi phục dữ liệu	M	
FR42	Bộ đếm lượt ghi nhanh "3/3" trên màn hình chính và màn hình ghi (giả định: hạn mức lượt dùng tính năng thông minh mỗi ngày cho gói Free)	C	
2.7. Yêu cầu phi chức năng (Non-functional Requirements)
Bảng 2.4. Yêu cầu phi chức năng
Mã	Nhóm	Yêu cầu	Tiêu chí đo
NFR01	Hiệu năng	Khởi động nguội và mở màn hình ghi nhanh	< 2,5 giây trên máy tầm trung; mở màn hình ghi < 1 giây
NFR02	Hiệu năng	Cuộn lịch/danh sách mượt	≥ 55 FPS; truy vấn thống kê tháng < 300 ms với 100.000 giao dịch
NFR03	Khả dụng	Ghi nhanh một khoản chi	≤ 3 chạm và ≤ 10 giây (sau khi mở app)
NFR04	Offline-first	Mọi chức năng cốt lõi dùng được khi mất mạng	Tự đồng bộ trong 30 giây sau khi có mạng; không mất giao dịch khi app bị tắt đột ngột
NFR05	Tin cậy	Ổn định	Crash-free sessions ≥ 99,5%
NFR06	Bảo mật	Truyền và lưu trữ an toàn	TLS 1.2+; khóa/token lưu ở Keychain/Keystore; khóa sinh trắc học
NFR07	Riêng tư	Tuân thủ quy định	Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân, chính sách quyền riêng tư của Apple/Google; từ chối quyền camera/mic/vị trí vẫn dùng được app
NFR08	Đa ngôn ngữ	Giao diện và định dạng theo vùng	4 ngôn ngữ; tiền VND không phần thập phân; hỗ trợ đa tiền tệ
NFR09	Tiếp cận	Hỗ trợ người dùng khó khăn	Dynamic Type, VoiceOver/TalkBack, tương phản WCAG AA
NFR10	Tương thích	Thiết bị hỗ trợ (giả định)	iOS 15+; Android 8.0+ (API 26); màn hình 4,7"–6,9"
NFR11	Bảo trì	Mã dễ kiểm thử, dễ mở rộng	Kiến trúc phân tầng; độ phủ test tầng domain ≥ 80%, tổng ≥ 60%; lint không cảnh báo
NFR12	Tài nguyên	Dung lượng và pin	Gói cài < 80 MB; ảnh nén ≤ 1 MB; đồng bộ theo lô; tải clip tùy chọn chỉ qua Wi-Fi
NFR13	Giao diện	Chủ đề	Tối (mặc định), sáng, theo hệ thống
2.8. Quy tắc nghiệp vụ (Business Rules)
Bảng 2.5. Quy tắc nghiệp vụ
Mã	Quy tắc
BR01	Số tiền giao dịch là số nguyên dương, lưu theo đơn vị nhỏ nhất của tiền tệ (VND: đồng). Loại giao dịch (chi/thu) quyết định dấu.
BR02	Giao dịch chi làm giảm số dư tài khoản; giao dịch thu làm tăng số dư. Số dư luôn được tính từ số dư đầu kỳ cộng tổng các giao dịch, không lưu trùng lặp ở nhiều nơi.
BR03	Chuyển tiền giữa hai tài khoản của cùng người dùng không được tính là thu hay chi trong thống kê.
BR04	Cú pháp rút gọn khi nhập số tiền: k = ×1.000; tr = ×1.000.000; tr2 = 1.200.000 (chữ số sau tr là phần thập phân của triệu); dòng bắt đầu bằng + là khoản thu, ngược lại là khoản chi.
BR05	Không cho ghi giao dịch thủ công vào ngày trong tương lai (lịch hiển thị mờ các ngày sau hôm nay); giao dịch định kỳ là ngoại lệ vì được tạo đúng kỳ.
BR06	Mỗi người dùng có đúng một tài khoản mặc định (hiển thị dấu sao). Mặc định khi tạo mới: "Wallet" (tiền mặt) và "Bank".
BR07	Xóa giao dịch/tài khoản/danh mục là xóa mềm; dữ liệu khôi phục được trong 30 ngày.
BR08	Không xóa cứng danh mục đang có giao dịch; chỉ ẩn hoặc gộp sang danh mục khác.
BR09	Ngân sách đạt 80% hạn mức thì cảnh báo vàng, đạt/vượt 100% thì cảnh báo đỏ. Ngân sách chung tính mọi khoản chi; ngân sách theo danh mục chỉ tính khoản chi thuộc danh mục đó.
BR10	Thẻ tín dụng: dư nợ = tổng chi chưa thanh toán; khi dư nợ vượt hạn mức thì cảnh báo; nhắc trước ngày sao kê và ngày đến hạn thanh toán.
BR11	Sổ tiết kiệm: lãi tích lũy (lãi đơn) = gốc × lãi suất năm × số ngày đã gửi / 365; ngày đáo hạn = ngày gửi + kỳ hạn.
BR12	Khoản vay/cho vay: dư nợ = gốc − tổng đã trả; khi dư nợ = 0 chuyển sang trạng thái "Đã tất toán".
BR13	Chia bill: tổng các phần chia phải bằng đúng tổng hóa đơn; phần dư do làm tròn được gán cho người trả tiền.
BR14	Giao dịch định kỳ: nếu app không mở đúng kỳ thì khi mở lại hệ thống tạo bù các kỳ bị lỡ (tối đa 12 kỳ mỗi lần).
BR15	Tính năng Pro chỉ mở khi quyền (entitlement) còn hiệu lực; khi hết hạn dữ liệu cũ vẫn xem được nhưng không tạo mới.
BR16	Quy đổi giờ làm: số giờ = số tiền ÷ mức lương theo giờ của người dùng.
BR17	(Giả định) Gói Free có hạn mức 3 lượt dùng tính năng thông minh mỗi ngày, đặt lại lúc 00:00 giờ địa phương.
2.9. Các ràng buộc của hệ thống
Bảng 2.6. Ràng buộc hệ thống
Loại	Ràng buộc
Công nghệ	Dùng Flutter/Dart cho toàn bộ giao diện; mã nền tảng (widget, Siri, App Intents) viết bằng Swift/Kotlin qua platform channel.
Nền tảng	iOS là ưu tiên phát hành đầu; phải đạt chuẩn App Store Review (đăng nhập Apple khi có đăng nhập mạng xã hội, mô tả quyền truy cập, dùng IAP của Apple cho gói Pro).
Pháp lý	Tuân thủ Nghị định 13/2023/NĐ-CP; có chính sách quyền riêng tư và chức năng xóa tài khoản/dữ liệu.
Dữ liệu	Không lưu thông tin thẻ thật/mật khẩu ngân hàng; ứng dụng chỉ ghi nhận thủ công.
Hiệu năng	Phải chạy tốt trên thiết bị tầm trung, có chế độ tiết kiệm khi pin yếu.
Chi phí	Dùng gói miễn phí của Firebase ở giai đoạn đầu; OCR chạy trên thiết bị để giảm chi phí.
Thời gian	MVP trong 10–12 tuần (xem 1.7).
2.10. Mô hình Use Case tổng quát
Hình 2.1. Sơ đồ Use Case tổng quát (dạng văn bản)
                    +-------------------- SnapMoney ---------------------+
 Người dùng         |  [Ghi chép]   UC02 Ghi bằng ảnh  <<extend>> UC03 Clip|
 (Free/Pro) ------- |               UC04 Nhập hàng loạt, UC06 Sửa/xóa      |
                    |  [Xem]        UC05 Lịch, UC07 Tìm kiếm, UC08 Thống kê|
                    |  [Tài sản]    UC09 Tài khoản, UC10 Thẻ/Tiết kiệm/ĐT  |
                    |               UC14 Khoản vay, UC15 Chuyến du lịch    |
                    |  [Kế hoạch]   UC11 Ngân sách, UC12 Quỹ, UC13 Định kỳ |
                    |  [Xã hội]     UC17 Bạn bè/nhóm, UC18 Chia bill       |
                    |               UC19 Chia sẻ GD, UC20 Chia sẻ tài khoản|
                    |  [Tiện ích]   UC21 Xuất báo cáo, UC22 Widget/Siri    |
                    |               UC23 Nhắc nhở, UC24 Quy đổi giờ làm    |
                    |  [Hệ thống]   UC01 Hồ sơ, UC16 Danh mục, UC25 Sao lưu|
                    |               UC26 Cài đặt/khóa, UC27 Nâng cấp Pro   |
                    +------------------------------------------------------+
 Apple/Google ---- UC25 (Đăng nhập), UC27 (IAP)
 Dịch vụ OCR/AI -- UC02 (<<include>> Quét hóa đơn), UC04 (đoán danh mục)
 Dịch vụ thông báo - UC23
 Bạn bè/Thành viên - UC17, UC18, UC19, UC20
 Quản trị viên ---- Console quản trị (danh mục mẫu, góp ý, hỗ trợ đăng ký)
2.11. Danh sách các Use Case
Bảng 2.7. Danh sách Use Case
Mã	Tên Use Case	Tác nhân chính	FR liên quan	Pro
UC01	Thiết lập hồ sơ và onboarding	A1	FR01, FR02	
UC02	Ghi nhận giao dịch bằng ảnh (quét hóa đơn)	A2, A3, A6	FR06–FR08, FR10	
UC03	Quay clip 3 giây	A3	FR09	✔
UC04	Nhập hàng loạt bằng văn bản/giọng nói	A2, A3	FR11, FR12	
UC05	Xem lịch và danh sách giao dịch	A2, A3	FR15, FR16	
UC06	Sửa, xóa, khôi phục giao dịch	A2, A3	FR13	
UC07	Tìm kiếm và lọc giao dịch	A2, A3	FR14	
UC08	Xem thống kê và bản đồ chi tiêu	A2, A3	FR27, FR28	
UC09	Quản lý tài khoản/ví và chuyển tiền	A2, A3	FR17	
UC10	Quản lý thẻ tín dụng, sổ tiết kiệm, đầu tư	A3	FR18–FR20	✔
UC11	Quản lý ngân sách và cảnh báo	A2, A3	FR23, FR24	
UC12	Quản lý quỹ tiết kiệm theo mục tiêu	A2, A3	FR25	
UC13	Quản lý giao dịch định kỳ	A2, A3	FR26	
UC14	Theo dõi khoản vay/cho vay	A3	FR21	✔
UC15	Quản lý chuyến du lịch	A2, A3	FR22	
UC16	Quản lý danh mục	A2, A3	FR39	
UC17	Quản lý bạn bè và nhóm	A2, A3, A4	FR31, FR32	
UC18	Chia hóa đơn	A2, A3, A4	FR33	
UC19	Chia sẻ giao dịch và nhắn tin	A2, A3, A4	FR34	
UC20	Chia sẻ tài khoản/ngân sách	A3, A4	FR35	✔
UC21	Xuất báo cáo PDF/Excel	A3	FR29	✔
UC22	Widget, Siri Shortcut, lưu từ ảnh chụp màn hình	A2, A3	FR36, FR37	
UC23	Nhắc nhở hóa đơn, khoản vay, sao kê	A7	FR38	
UC24	Quy đổi giờ làm	A2, A3	FR30	
UC25	Đăng nhập Apple, đồng bộ và sao lưu	A2, A5	FR03, FR41	
UC26	Khóa bảo mật và cài đặt (ngôn ngữ, giao diện, tiền tệ)	A2, A3	FR04, FR05, FR42	
UC27	Nâng cấp và quản lý gói Pro	A2, A5	FR40	
2.12. Đặc tả chi tiết các Use Case
Bốn Use Case cốt lõi được đặc tả tại đây; các Use Case còn lại được tóm tắt tại Phụ lục B.
Bảng 2.8. Đặc tả UC02 – Ghi nhận giao dịch bằng ảnh
Mục	Nội dung
Mô tả	Người dùng chụp ảnh khoản chi, hệ thống tạo giao dịch nháp; nếu là hóa đơn thì tự điền số tiền
Tác nhân	Người dùng (A2/A3); Dịch vụ OCR (A6)
Tiền điều kiện	Đã hoàn tất UC01; đã cấp quyền camera (hoặc chọn ảnh từ thư viện); còn lượt ghi (BR17)
Luồng chính	1) Nhấn nút (+) trên trang chủ. 2) Hệ thống mở camera (hoặc theo cài đặt "Mở từ widget"). 3) Người dùng chụp ảnh. 4) Hệ thống chạy OCR, nếu tìm được tổng tiền thì điền sẵn số tiền. 5) Hệ thống hiển thị màn hình xác nhận: số tiền, "Thêm chi tiết", danh mục, tài khoản, chuyển Chi/Thu, ngày. 6) Người dùng chỉnh sửa và nhấn dấu tick. 7) Hệ thống kiểm tra dữ liệu (BR01, BR05), lưu giao dịch cục bộ, cập nhật số dư, đóng màn hình và đưa về trang chủ.
Luồng thay thế	A1) "Chọn từ thư viện": chọn ảnh có sẵn. A2) "Bỏ qua ảnh": vào thẳng form nhập. A3) Nhấn "Share": chia sẻ giao dịch (UC19). A4) Nhấn "Thêm": lưu và mở form giao dịch mới. A5) Nhấn dấu X: gỡ ảnh và chụp lại.
Ngoại lệ	E1) Từ chối quyền camera: hiển thị hướng dẫn mở cài đặt, cho phép bỏ qua ảnh. E2) OCR thất bại: để số tiền 0 và yêu cầu nhập tay. E3) Số tiền = 0 hoặc rỗng: không cho lưu, hiển thị lỗi. E4) Hết lượt thông minh: mời nâng cấp Pro hoặc nhập tay. E5) Hết bộ nhớ: báo lỗi, không mất dữ liệu đã nhập.
Hậu điều kiện	Giao dịch lưu với trạng thái chờ đồng bộ; thống kê, ngân sách và widget được cập nhật
Quy tắc liên quan	BR01, BR02, BR05, BR09, BR17
Bảng 2.9. Đặc tả UC04 – Nhập hàng loạt
Mục	Nội dung
Mô tả	Nhập nhiều giao dịch trong một lần, mỗi dòng một giao dịch, hệ thống tự tách số tiền và đoán danh mục
Tác nhân	Người dùng; Dịch vụ AI/quy tắc phân loại (A6)
Tiền điều kiện	Có ít nhất một tài khoản; chọn được ngày và tài khoản áp dụng
Luồng chính	1) Mở "Nhập hàng loạt". 2) Chọn ngày (mặc định hôm nay) và tài khoản. 3) Gõ/dán văn bản hoặc nhấn "Nói". 4) Hệ thống phân tích từng dòng: tách tên, số tiền (BR04), loại thu/chi, đoán danh mục. 5) Hiển thị bản xem trước. 6) Người dùng sửa danh mục/số tiền nếu cần. 7) Nhấn "Lưu": hệ thống tạo toàn bộ giao dịch trong một giao dịch CSDL (atomic).
Luồng thay thế	A1) "Dán" lấy văn bản từ bộ nhớ tạm. A2) Chạm gợi ý "35k = 35.000" để xem quy ước. A3) Nói: dùng ngôn ngữ nói đã chọn ở UC01.
Ngoại lệ	E1) Dòng không có số: đánh dấu lỗi, bỏ qua khi lưu. E2) Từ chối quyền micro: ẩn nút "Nói". E3) Nhận dạng giọng nói thất bại: giữ lại văn bản đã nhận. E4) Lỗi ghi CSDL: hoàn tác toàn bộ (rollback).
Hậu điều kiện	N giao dịch được tạo; nút "Lưu" bị vô hiệu khi chưa có dòng hợp lệ
Quy tắc liên quan	BR01, BR04, BR05
Bảng 2.10. Đặc tả UC11 – Quản lý ngân sách
Mục	Nội dung
Mô tả	Tạo, theo dõi và nhận cảnh báo ngân sách chi tiêu
Tác nhân	Người dùng
Tiền điều kiện	Đang ở tab "Ngân sách"
Luồng chính	1) Nhấn (+) hoặc chọn mẫu "Chi tiêu hàng tháng". 2) Nhập tên, hạn mức, kỳ (tuần/tháng/tùy chọn), phạm vi (chung hoặc theo danh mục). 3) Lưu. 4) Mỗi khi có giao dịch chi mới, hệ thống cập nhật mức đã dùng. 5) Khi đạt 80%/100%, gửi cảnh báo (BR09).
Luồng thay thế	A1) Sửa hoặc lưu trữ ngân sách. A2) Nhấn biểu tượng người dùng ở góc: mời người thân dùng chung (UC20, Pro).
Ngoại lệ	E1) Hạn mức ≤ 0: báo lỗi. E2) Trùng kỳ và phạm vi với ngân sách có sẵn: hỏi hợp nhất. E3) Hết quyền Pro với ngân sách chia sẻ: chuyển về chế độ chỉ xem.
Hậu điều kiện	Ngân sách hiển thị tiến độ; có thông báo khi chạm ngưỡng
Quy tắc liên quan	BR09, BR15
Bảng 2.11. Đặc tả UC18 – Chia hóa đơn
Mục	Nội dung
Mô tả	Chia một hóa đơn cho nhiều người và theo dõi ai đã trả
Tác nhân	Người dùng (người trả tiền); Bạn bè/Thành viên nhóm
Tiền điều kiện	Có ít nhất một bạn bè hoặc nhóm (UC17)
Luồng chính	1) Vào Cá nhân → "Chia tiền". 2) Nhập tổng tiền, chọn người tham gia/nhóm. 3) Chọn cách chia: đều, theo số tiền, theo phần trăm. 4) Kiểm tra tổng các phần (BR13). 5) Xác nhận: hệ thống tạo giao dịch chi cho người trả và khoản phải thu cho từng người. 6) Gửi thông báo đến những người liên quan. 7) Khi một người trả, người dùng đánh dấu "Đã trả"; hệ thống tạo khoản thu tương ứng.
Luồng thay thế	A1) Chia từ một giao dịch có sẵn. A2) Gửi nhắc trả cho người chưa trả.
Ngoại lệ	E1) Tổng phần chia ≠ tổng hóa đơn: không cho xác nhận. E2) Bạn bè chưa dùng app: tạo "người chưa liên kết" chỉ lưu tên. E3) Mất mạng khi gửi thông báo: xếp hàng gửi lại.
Hậu điều kiện	Có bản ghi chia bill và trạng thái thanh toán từng người
Quy tắc liên quan	BR13

CHƯƠNG 3. PHÂN TÍCH HỆ THỐNG
3.1. Phân tích quy trình nghiệp vụ
Bảng 3.1. Các quy trình nghiệp vụ chính
Quy trình	Kích hoạt	Các bước chính	Kết quả
P1. Ghi nhận chi tiêu	Người dùng nhấn (+), widget hoặc Siri	Chụp/chọn ảnh → OCR → xác nhận → lưu	Giao dịch mới, cập nhật số dư
P2. Nhập hàng loạt	Mở "Nhập hàng loạt"	Gõ/nói → phân tích → xem trước → lưu	N giao dịch
P3. Quản lý tài khoản	Tab "Tài khoản"	Tạo/sửa tài khoản → ghi giao dịch → xem số dư	Số dư đúng theo từng tài khoản
P4. Ngân sách	Tạo ngân sách	Đặt hạn mức → mỗi giao dịch chi cập nhật mức dùng → cảnh báo	Kiểm soát chi tiêu
P5. Giao dịch định kỳ	Đến kỳ (bộ lập lịch)	Kiểm tra quy tắc → tạo giao dịch → thông báo	Giao dịch tự động
P6. Chia hóa đơn	Chọn "Chia tiền"	Chọn người → chia → xác nhận → theo dõi thanh toán	Khoản phải thu/trả
P7. Đồng bộ	Có mạng hoặc có thay đổi	Gom thay đổi → đẩy lên → kéo thay đổi mới → giải quyết xung đột	Dữ liệu nhất quán giữa thiết bị
P8. Nâng cấp Pro	Nhấn "Nâng cấp" hoặc chạm tính năng khóa	Hiển thị gói → thanh toán IAP → xác thực biên lai → mở quyền	Quyền Pro hiệu lực
3.2. Sơ đồ hoạt động (Activity Diagram)
Hình 3.1. Sơ đồ hoạt động – Ghi nhận khoản chi bằng ảnh
(●) Bắt đầu
  |
[Nhấn nút (+)]
  |
[Mở camera] --{Có quyền camera?}--Không--> [Hướng dẫn cấp quyền] --> [Bỏ qua ảnh]---+
  |Có                                                                               |
[Chụp ảnh / Chọn từ thư viện / Bỏ qua ảnh] -----------------------------------------+
  |
[Chạy OCR trên ảnh] --{Tìm thấy tổng tiền?}--Không--> [Số tiền = 0, chờ nhập tay]
  |Có                                                        |
[Điền sẵn số tiền] <---------------------------------------+
  |
[Người dùng sửa: số tiền, danh mục, tài khoản, ngày, chi tiết]
  |
{Hợp lệ? (số tiền>0, ngày ≤ hôm nay)} --Không--> [Hiện lỗi] --> (quay lại bước sửa)
  |Có
[Lưu cục bộ (trạng thái chờ đồng bộ)] --> [Cập nhật số dư, ngân sách, widget]
  |
{Nhấn "Thêm"?} --Có--> (quay lại bước mở camera)
  |Không
[Về trang chủ] --> [Đẩy đồng bộ nền khi có mạng]
  |
(◉) Kết thúc
3.3. Sơ đồ tuần tự (Sequence Diagram)
Hình 3.2. Sơ đồ tuần tự – Quét hóa đơn và lưu giao dịch
Người dùng   AddTxScreen   AddTxNotifier   OcrService   TxRepository   LocalDB   SyncEngine
   |--chụp ảnh-->|              |              |             |            |          |
   |             |--onPhoto---->|              |             |            |          |
   |             |              |--extract---->|             |            |          |
   |             |              |<--amount,text|             |            |          |
   |<--form điền sẵn------------|              |             |            |          |
   |--sửa + xác nhận----------->|              |             |            |          |
   |             |              |--save(draft)------------->|            |          |
   |             |              |              |             |--insert--->|          |
   |             |              |              |             |<--id-------|          |
   |             |              |              |             |--enqueue(tx)--------->|
   |<--đóng màn hình + cập nhật số dư-----------|             |            |          |
Hình 3.3. Sơ đồ tuần tự – Nhập hàng loạt bằng văn bản/giọng nói
Người dùng   BulkScreen   SpeechService   BulkParser   Categorizer   TxRepository
   |--nhấn "Nói"-->|            |              |             |             |
   |               |--listen--->|              |             |             |
   |<--văn bản-----|<--text-----|              |             |             |
   |--nhấn "Lưu"-->|--parse(text)------------->|             |             |
   |               |              |            |--guess(name)->|           |
   |               |<--List<ParsedTx>----------|<--category---|           |
   |<--bản xem trước|            |              |             |             |
   |--xác nhận---->|--saveAll(List<Tx>)------------------------------->|
   |<--thành công (hoặc rollback + lỗi)--------------------------------|
3.4. Mô hình miền nghiệp vụ (Domain Model)
Hình 3.4. Mô hình miền nghiệp vụ
User 1───* Account 1───* Transaction *───1 Category
  |            |                |  *───0..1 Trip
  |            |                |  *───0..1 RecurringRule
  |            |                +──0..1 Media (ảnh/clip)
  |            +── (Card | Savings | Investment | Loan) là các loại Account/mở rộng
  |
  +──* Budget *──* Category          Budget 0..*───* Member (chia sẻ)
  +──* SavingGoal
  +──* Friend ───* Group *───* SplitBill 1───* SplitShare
  +──1 Subscription     +──* Reminder      +──1 Settings (ngôn ngữ, tiền tệ, khóa)
3.5. Phân tích các đối tượng nghiệp vụ
Bảng 3.2. Các đối tượng nghiệp vụ
Đối tượng	Thuộc tính chính	Hành vi / ràng buộc
User	tên, ảnh, tiền tệ, ngôn ngữ UI, ngôn ngữ nói, ngày tham gia	Tạo, đổi thông tin, liên kết Apple
Account	tên, loại, số dư đầu, màu/biểu tượng, mặc định, lưu trữ	Tính số dư, chuyển tiền, đặt mặc định (BR06)
Transaction	loại, số tiền, ghi chú, ngày, ảnh/clip, cảm xúc, vị trí, nguồn	Tạo, sửa, xóa mềm, đồng bộ (BR01–BR05)
Category	tên, biểu tượng, màu, loại, thứ tự	Ẩn/gộp (BR08)
Budget	tên, hạn mức, kỳ, phạm vi, ngưỡng cảnh báo	Tính tiến độ, cảnh báo (BR09)
SavingGoal	tên, mục tiêu, đã góp, hạn	Nạp/rút quỹ
Savings / Investment / Loan	gốc, lãi suất, kỳ hạn, số lượng, đối tác	Tính lãi, dư nợ, tất toán (BR11, BR12)
RecurringRule	tần suất, khoảng, kỳ kế tiếp, mẫu giao dịch	Sinh giao dịch, tạo bù (BR14)
Friend / Group	trạng thái kết bạn, thành viên	Mời, chấp nhận, rời nhóm
SplitBill / SplitShare	tổng tiền, người trả, phần chia, trạng thái	Kiểm tổng (BR13), đánh dấu đã trả
Subscription	gói, trạng thái, ngày hết hạn	Mở/khóa tính năng (BR15)
3.6. Phân tích trạng thái đối tượng (State Machine Diagram)
Hình 3.5. Sơ đồ trạng thái – Giao dịch
[Nháp] --lưu--> [Đã lưu, chờ đồng bộ] --đẩy thành công--> [Đã đồng bộ]
   ^                    |                                      |
   |                    +--sửa (khi chưa đồng bộ)--> (giữ nguyên trạng thái)
   |                                                           |--sửa--> [Đã lưu, chờ đồng bộ]
[Hủy] <--bỏ--                                                  |--xóa--> [Đã xóa mềm]
                                          [Đã xóa mềm] --khôi phục (≤30 ngày)--> [Đã lưu, chờ đồng bộ]
                                          [Đã xóa mềm] --quá 30 ngày--> [Xóa vĩnh viễn]
Hình 3.6. Sơ đồ trạng thái – Gói đăng ký Pro
[Free] --mua--> [Pro đang hiệu lực] --hết hạn, gia hạn lỗi--> [Gia hạn chờ (grace)] --thanh toán lại--> [Pro đang hiệu lực]
   ^                  |  --hủy gia hạn--> [Pro, sẽ hết hạn]                |
   |                  |                                                     +--quá hạn--> [Hết hạn] --mua lại--> [Pro đang hiệu lực]
   +---------- [Hết hạn] --(giữ dữ liệu, khóa tạo mới)
3.7. Phân tích các luồng xử lý ngoại lệ
Bảng 3.3. Các luồng ngoại lệ
Tình huống	Cách xử lý
Mất mạng khi đồng bộ	Giữ giao dịch trong hàng đợi, thử lại theo lũy thừa (exponential backoff), hiển thị chấm trạng thái chờ đồng bộ
Xung đột chỉnh sửa trên 2 thiết bị	Dùng quy tắc "bản ghi có updated_at mới hơn thắng" theo từng bản ghi; giao dịch bị xóa thắng bản sửa cũ
Từ chối quyền camera/micro/vị trí	Ẩn hoặc vô hiệu tính năng liên quan, hướng dẫn mở cài đặt, các chức năng khác dùng bình thường
OCR không đọc được	Giữ ảnh, để người dùng nhập số tay
Biên lai IAP không hợp lệ	Không mở quyền Pro, thông báo lỗi, cho phép "Khôi phục giao dịch mua"
Dữ liệu cục bộ hỏng	Phát hiện qua kiểm tra toàn vẹn, thử khôi phục từ bản sao lưu gần nhất
Thiết bị đổi múi giờ	Lưu thời điểm theo UTC cùng múi giờ gốc; hiển thị theo múi giờ hiện tại
Hết dung lượng bộ nhớ	Báo lỗi rõ ràng, không lưu dở dang; gợi ý xóa clip/ảnh
3.8. Tổng kết kết quả phân tích
Kết quả phân tích xác định 8 tác nhân, 42 yêu cầu chức năng, 13 yêu cầu phi chức năng, 17 quy tắc nghiệp vụ và 27 Use Case. Có ba điểm thiết kế quan trọng được rút ra: (1) offline-first với hàng đợi đồng bộ; (2) số dư luôn tính từ giao dịch thay vì lưu trùng lặp; (3) mọi tính năng nâng cao được kiểm soát tập trung qua một dịch vụ kiểm tra quyền (entitlement). Ba điểm này là cơ sở cho thiết kế kiến trúc ở Chương 4.

CHƯƠNG 4. THIẾT KẾ KIẾN TRÚC VÀ CẤU TRÚC HỆ THỐNG
4.1. Tổng quan kiến trúc hệ thống
Hệ thống gồm ba khối: (1) ứng dụng Flutter chạy trên điện thoại, chứa toàn bộ logic và cơ sở dữ liệu cục bộ; (2) dịch vụ backend đám mây (Firebase) lo xác thực, đồng bộ, lưu media, API xã hội, kiểm tra đăng ký và thông báo đẩy; (3) các dịch vụ bên ngoài gồm Apple/Google (đăng nhập, IAP), bản đồ, APNs/FCM. Dữ liệu "nguồn sự thật" khi sử dụng nằm ở CSDL cục bộ (offline-first); đám mây là bản sao lưu và là kênh đồng bộ giữa các thiết bị/người dùng.
4.2. Lựa chọn mô hình kiến trúc
Bảng 4.1. So sánh các mô hình kiến trúc
Mô hình	Ưu điểm	Nhược điểm	Đánh giá
MVC / widget chứa logic	Đơn giản, nhanh làm	Khó kiểm thử, widget phình to	Không chọn
MVVM + Provider	Tách UI/logic, dễ học	Dễ trộn logic với truy cập dữ liệu khi dự án lớn	Chưa đủ
BLoC	Luồng sự kiện rõ ràng, kiểm thử tốt	Nhiều mã mẫu (boilerplate)	Có thể dùng
Clean Architecture (feature-first) + Riverpod	Phân tầng rõ, domain độc lập framework, kiểm thử dễ, ít boilerplate với code generation	Cần kỷ luật khi đặt tên và tách lớp	Chọn
Lý do chọn: nghiệp vụ tài chính (số dư, ngân sách, chia bill) cần được kiểm thử độc lập khỏi giao diện; đồng thời dự án có nhiều tính năng nên tổ chức theo từng tính năng (feature-first) giúp nhiều người làm song song.
4.3. Sơ đồ kiến trúc tổng thể
Hình 4.1. Sơ đồ kiến trúc tổng thể
+------------------------------- THIẾT BỊ (Flutter App) -------------------------------+
| PRESENTATION   Screens / Widgets  <--->  Notifiers (Riverpod)  [Home|Stats|Accounts |
|                                                              Budget|Profile|Social]|
|-------------------------------------------------------------------------------------|
| DOMAIN         Entities | UseCases (AddTransaction, ParseBulkInput, BudgetProgress,   |
|                SplitBill, CheckEntitlement) | Repository Interfaces                 |
|-------------------------------------------------------------------------------------|
| DATA           Repository Impl | DAO (Drift/SQLite) | RemoteDataSource | SyncEngine  |
|                OcrService | SpeechService | LocationService | SecureStorage          |
|-------------------------------------------------------------------------------------|
| PLATFORM       Camera | Mic | GPS | Widget/App Intents (Swift/Kotlin) | Biometrics   |
+------------------------------------|------------------------------------------------+
                                     | HTTPS (TLS)
+------------------------------- BACKEND (Firebase) ---------------------------------+
| Auth (Anonymous, Apple, Google) | Firestore (sync + social) | Storage (ảnh/clip)      |
| Cloud Functions (API: social, split, entitlement, export) | FCM (push)             |
+------------------------------------|------------------------------------------------+
                                     |
          Apple/Google (IAP, Sign-in) | Maps | APNs/FCM | RevenueCat (tùy chọn)
4.4. Phân chia các tầng và thành phần
Bảng 4.2. Các tầng
Tầng	Thành phần	Quy tắc phụ thuộc
Presentation	Screen, Widget, Notifier/Provider, theme, router	Chỉ phụ thuộc Domain
Domain	Entity, UseCase, Repository interface, Value Object (Money, DateRange)	Không phụ thuộc Flutter hay thư viện bên ngoài
Data	Repository implementation, DAO, DTO/mapper, remote/local data source, SyncEngine	Phụ thuộc Domain (cài đặt interface)
Platform	Mã native, plugin, quyền truy cập	Được Data gọi qua interface
Core	Lỗi chung, tiện ích, hằng số, định dạng tiền, i18n, log	Dùng được ở mọi tầng
4.5. Thiết kế các module chức năng
Bảng 4.3. Danh sách module (feature)
Module	Màn hình / chức năng	Use Case
onboarding	Hồ sơ, giới thiệu, coachmark	UC01
transactions	Thêm ảnh/clip, form, bulk, sửa, tìm kiếm, lịch	UC02–UC07
statistics	Tuần/tháng/năm, danh mục, bản đồ	UC08
accounts	Ví, thẻ, tiết kiệm, đầu tư, vay, chuyến đi	UC09, UC10, UC14, UC15
budgets	Ngân sách, quỹ tiết kiệm, định kỳ	UC11–UC13
categories	Danh mục	UC16
social	Bạn bè, nhóm, chia bill, chia sẻ	UC17–UC20
reports	Xuất PDF/Excel	UC21
integrations	Widget, Siri, nhắc nhở, quy đổi giờ làm	UC22–UC24
auth_sync	Đăng nhập Apple, đồng bộ, sao lưu	UC25
settings	Ngôn ngữ, giao diện, tiền tệ, khóa	UC26
premium	Gói Pro, paywall, entitlement	UC27
Hình 4.2. Cấu trúc thư mục dự án
lib/
 ├─ main.dart, app.dart              # khởi tạo, ProviderScope, MaterialApp.router
 ├─ core/   (errors, utils, money/, l10n/, theme/, router/, logger/)
 ├─ data/   (database/ drift tables+DAO, sync/, remote/, services/ ocr, speech, location)
 ├─ features/
 │   ├─ transactions/  { presentation/, domain/, data/ }
 │   ├─ budgets/       { presentation/, domain/, data/ }
 │   ├─ accounts/ statistics/ social/ premium/ settings/ ...
 └─ l10n/   (app_vi.arb, app_en.arb, app_ja.arb, app_zh_TW.arb)
ios/ android/ (native: widget, App Intents, Share Extension)
test/ integration_test/
4.6. Mô tả trách nhiệm của từng module
Bảng 4.4. Trách nhiệm module
Module	Trách nhiệm chính	Phụ thuộc
transactions	Tạo/sửa/xóa giao dịch, phân tích văn bản, OCR, media, tìm kiếm	accounts, categories, budgets, premium
statistics	Tổng hợp theo kỳ/danh mục/địa điểm; không ghi dữ liệu	transactions
accounts	Tính số dư, chuyển tiền, lãi tiết kiệm, dư nợ	transactions
budgets	Tính tiến độ, phát cảnh báo, sinh giao dịch định kỳ	transactions, integrations
social	Bạn bè, nhóm, chia bill, thông điệp	auth_sync
auth_sync	Định danh, hàng đợi đồng bộ, giải quyết xung đột	mọi module có dữ liệu
premium	Kiểm tra quyền, mua và khôi phục	auth_sync
integrations	Widget, Siri, thông báo cục bộ	transactions, budgets
4.7. Thiết kế Class Diagram
Hình 4.3. Sơ đồ lớp (Class Diagram) – rút gọn
<<entity>> Transaction                <<entity>> Account            <<entity>> Category
 id, accountId, toAccountId?          id, name, type, openingBal    id, name, icon, kind
 categoryId, type, amount: Money      isDefault, archived           parentId?
 note, occurredAt, emotion?           balance(): Money
 place?, mediaIds[], source

<<usecase>> AddTransaction            <<usecase>> ParseBulkInput     <<usecase>> BudgetProgress
 call(NewTx): Result<Transaction>      call(text, date): List<Parsed> call(budgetId): Progress

<<interface>> TransactionRepository   <<interface>> AccountRepository   <<interface>> EntitlementRepo
 add(tx) / update / softDelete          watchAll() / balanceOf(id)       isPro(): Stream<bool>
 watchByRange(range, filter)
        ^                                       ^
TransactionRepositoryImpl -> TransactionDao (Drift) + SyncQueue + MediaStore

<<notifier>> AddTransactionNotifier -> AddTransaction, OcrService, BudgetProgress
<<notifier>> HomeCalendarNotifier   -> TransactionRepository.watchByRange
<<service>>  SyncEngine  push(), pull(), resolve(conflict)
<<service>>  OcrService.extract(image) : OcrResult(amount?, merchant?, rawText)
4.8. Thiết kế các lớp và mối quan hệ giữa các lớp
Bảng 4.5. Các lớp chính
Lớp	Loại	Quan hệ	Ghi chú
Money	Value Object	Dùng bởi Transaction, Budget, Account	Lưu số nguyên + mã tiền tệ; phép cộng chỉ khi cùng tiền tệ
Transaction	Entity	Account *–1, Category *–1	Bất biến; sửa tạo bản sao với copyWith
AddTransaction	UseCase	Gọi TransactionRepository, BudgetProgress	Kiểm tra BR01, BR05 và hạn mức lượt (BR17)
ParseBulkInput	UseCase	Dùng Categorizer	Hàm thuần, không phụ thuộc nền tảng
Categorizer	Domain Service	Từ khóa → danh mục; có thể thay bằng mô hình AI	Học từ lựa chọn của người dùng
SyncEngine	Data Service	Đọc SyncQueue, gọi RemoteDataSource	Chạy nền khi có mạng
EntitlementRepository	Interface	Cài đặt bằng RevenueCat/Apple	Nguồn duy nhất cho BR15
NotificationScheduler	Platform Service	Lên lịch nhắc	Gọi bởi budgets và accounts
4.9. Thiết kế luồng tương tác giữa các thành phần
Hình 4.4. Luồng tương tác khi lưu một giao dịch
UI (nhấn ✓) -> AddTransactionNotifier.submit()
   -> AddTransaction.call()         [kiểm tra BR01, BR05, quota]
   -> TransactionRepository.add()   [1 transaction SQLite: insert tx + media + sync_queue]
   -> emit stream (Drift watch) --> Home, Statistics, Accounts, Widget tự cập nhật
   -> BudgetProgress.recalculate()  --> NotificationScheduler (nếu chạm 80%/100%)
   -> SyncEngine.push() (nền)       --> Firestore/Storage --> đánh dấu sync_state = synced
4.10. Các quyết định thiết kế và lý do lựa chọn
Bảng 4.6. Quyết định thiết kế
Quyết định	Lý do	Đánh đổi
Offline-first, SQLite là nguồn dữ liệu chính	Ghi nhanh, hoạt động không mạng, truy vấn thống kê hiệu quả	Cần cơ chế đồng bộ và giải quyết xung đột
Drift thay vì Hive/Isar	SQL quan hệ, truy vấn tổng hợp, migration có kiểm soát, stream phản ứng	Cần học SQL/cú pháp Drift
Số dư tính từ giao dịch	Tránh sai lệch dữ liệu	Truy vấn tổng hợp; giảm tải bằng chỉ mục và bộ nhớ đệm
Tiền lưu số nguyên	Tránh sai số số thực	Phải định dạng khi hiển thị
ID dạng UUID/ULID sinh ở máy	Tạo offline không trùng; sắp xếp theo thời gian (ULID)	Dài hơn khóa số nguyên
Đồng bộ theo bản ghi, "cập nhật mới nhất thắng"	Đơn giản, phù hợp dữ liệu cá nhân	Hiếm khi mất một chỉnh sửa đồng thời
OCR trên thiết bị (ML Kit)	Riêng tư, miễn phí, nhanh	Độ chính xác thấp hơn mô hình đám mây; có thể thêm tùy chọn đám mây
Firebase làm backend	Nhanh triển khai, có Auth/Storage/Push	Phụ thuộc nhà cung cấp; truy vấn quan hệ phức tạp hạn chế
Riverpod + code generation	An toàn kiểu, dễ kiểm thử, ít boilerplate	Cần chạy build_runner

CHƯƠNG 5. THIẾT KẾ CƠ SỞ DỮ LIỆU
5.1. Giới thiệu hệ quản trị cơ sở dữ liệu
•SQLite (qua Drift) làm CSDL cục bộ trên thiết bị: nhẹ, không cần máy chủ, hỗ trợ giao dịch ACID, phù hợp offline-first. Có thể bật mã hóa bằng SQLCipher.
•Cloud Firestore làm kho đồng bộ và dữ liệu xã hội: mỗi người dùng một nhánh users/{uid}/... chứa bản sao các bảng; nhánh xã hội (groups, split_bills, shared_transactions) có quy tắc bảo mật riêng.
•Cloud Storage lưu ảnh và clip; CSDL chỉ lưu đường dẫn.
5.2. Xác định các thực thể dữ liệu
Bảng 5.1. Thực thể dữ liệu
Nhóm	Thực thể
Người dùng	UserProfile, AppSetting, Subscription, UsageQuota
Giao dịch	Transaction, Media, Category, Trip, RecurringRule
Tài sản	Account, SavingsDeposit, Investment, Loan, LoanPayment
Kế hoạch	Budget, BudgetCategory, SavingGoal, SavingGoalEntry
Xã hội	Friend, Group, GroupMember, SplitBill, SplitShare, SharedTransaction, Message
Tiện ích	Reminder, WorkHourSetting
Hệ thống	SyncQueue
5.3. Sơ đồ thực thể – liên kết (ERD)
Hình 5.1. Sơ đồ ERD (rút gọn các quan hệ chính)
user_profile 1──* accounts 1──* transactions *──1 categories (1──* categories con)
                      |              |*──0..1 trips          |*──0..1 recurring_rules
                      |              |1──* media             |*──0..1 split_bills
                      +─ savings_deposits / investments / loans 1──* loan_payments
user_profile 1──* budgets *──* categories   (qua budget_categories)
user_profile 1──* saving_goals 1──* saving_goal_entries
user_profile 1──* friends ; groups 1──* group_members ; groups 1──* split_bills 1──* split_shares
user_profile 1──1 subscriptions ; user_profile 1──* reminders ; sync_queue (bảng kỹ thuật)
5.4. Mô hình cơ sở dữ liệu quan hệ
Mô hình quan hệ được suy ra từ ERD theo quy tắc: quan hệ 1–N đặt khóa ngoại ở phía "N"; quan hệ N–N tách thành bảng trung gian (budget_categories, group_members); thực thể có nhiều loại (Account) dùng cột type kèm bảng mở rộng cho loại phức tạp (tiết kiệm, đầu tư, vay).
5.5. Danh sách các bảng dữ liệu
Bảng 5.2. Danh sách bảng
STT	Bảng	Mô tả
1	user_profile	Hồ sơ, tiền tệ, ngôn ngữ UI, ngôn ngữ nói
2	accounts	Tài khoản/ví/thẻ
3	categories	Danh mục chi/thu
4	transactions	Giao dịch chi/thu/chuyển
5	media	Ảnh/clip của giao dịch
6	budgets	Ngân sách
7	budget_categories	Ngân sách – danh mục
8	saving_goals	Quỹ tiết kiệm theo mục tiêu
9	saving_goal_entries	Lịch sử nạp/rút quỹ
10	savings_deposits	Sổ tiết kiệm
11	investments	Vàng, chứng khoán
12	loans	Khoản vay/cho vay
13	loan_payments	Lần trả nợ
14	recurring_rules	Giao dịch định kỳ
15	trips	Chuyến du lịch
16	friends	Quan hệ bạn bè
17	groups	Nhóm
18	group_members	Thành viên nhóm
19	split_bills	Hóa đơn chia
20	split_shares	Phần chia của từng người
21	shared_transactions	Giao dịch gửi cho bạn
22	messages	Tin nhắn trong app
23	reminders	Nhắc nhở
24	work_hour_settings	Lương theo giờ
25	subscriptions	Gói Pro
26	app_settings	Cài đặt (khóa, giao diện…)
27	usage_quota	Bộ đếm lượt ghi nhanh theo ngày
28	sync_queue	Hàng đợi đồng bộ
5.6. Từ điển dữ liệu
Bảng 5.3. transactions
Cột	Kiểu	Ràng buộc	Mô tả
id	TEXT	PK	ULID sinh ở thiết bị
account_id	TEXT	FK → accounts, NOT NULL	Tài khoản phát sinh
to_account_id	TEXT	FK → accounts, NULL	Tài khoản nhận (khi chuyển)
category_id	TEXT	FK → categories, NULL	Bắt buộc nếu không phải chuyển
type	TEXT	CHECK IN ('expense','income','transfer')	Loại
amount	INTEGER	CHECK > 0	Số tiền (đơn vị nhỏ nhất)
currency	TEXT	NOT NULL	Mã ISO 4217, ví dụ VND
note	TEXT	NULL	Chi tiết
occurred_at	INTEGER	NOT NULL	Thời điểm (UTC, epoch ms)
tz	TEXT	NULL	Múi giờ gốc
emotion	TEXT	NULL	Cảm xúc
lat, lng, place_name	REAL/REAL/TEXT	NULL	Vị trí
trip_id	TEXT	FK → trips, NULL	Chuyến đi
recurring_id	TEXT	FK → recurring_rules, NULL	Quy tắc sinh ra
split_bill_id	TEXT	FK → split_bills, NULL	Hóa đơn chia liên quan
source	TEXT	CHECK IN ('manual','ocr','bulk','voice','siri','recurring')	Nguồn
created_at, updated_at	INTEGER	NOT NULL	Dấu thời gian
deleted_at	INTEGER	NULL	Xóa mềm
sync_state	TEXT	DEFAULT 'pending'	pending / synced / conflict
Bảng 5.4. accounts
Cột	Kiểu	Ràng buộc	Mô tả
id	TEXT	PK	
name	TEXT	NOT NULL	Tên (Wallet, Bank…)
type	TEXT	CHECK IN ('cash','bank','ewallet','credit_card')	Loại
currency	TEXT	NOT NULL	Tiền tệ
opening_balance	INTEGER	DEFAULT 0	Số dư đầu
credit_limit	INTEGER	NULL	Hạn mức (thẻ)
statement_day, due_day	INTEGER	NULL, 1–31	Ngày sao kê, ngày đến hạn
icon, color	TEXT	NULL	Hiển thị
is_default	INTEGER	0/1	Tài khoản mặc định (BR06)
archived	INTEGER	0/1	Lưu trữ
created_at, updated_at, deleted_at	INTEGER		
Bảng 5.5. categories
Cột	Kiểu	Ràng buộc	Mô tả
id	TEXT	PK	
name	TEXT	NOT NULL	Ví dụ: Ăn uống, Mua sắm, Di chuyển, Cà phê
kind	TEXT	CHECK IN ('expense','income')	
parent_id	TEXT	FK → categories, NULL	Danh mục cha
icon, color	TEXT		
sort_order	INTEGER	DEFAULT 0	
is_system	INTEGER	0/1	Danh mục có sẵn
hidden	INTEGER	0/1	Ẩn thay vì xóa (BR08)
Bảng 5.6. budgets
Cột	Kiểu	Ràng buộc	Mô tả
id	TEXT	PK	
name	TEXT	NOT NULL	
scope	TEXT	CHECK IN ('overall','category')	Phạm vi
amount	INTEGER	CHECK > 0	Hạn mức
period_type	TEXT	CHECK IN ('week','month','custom')	Kỳ
start_date, end_date	INTEGER	NULL	Với kỳ tùy chọn
warn_pct, limit_pct	INTEGER	DEFAULT 80, 100	Ngưỡng cảnh báo
is_shared	INTEGER	0/1	Chia sẻ (Pro)
archived, created_at, updated_at, deleted_at			
Bảng 5.7. Các bảng còn lại (tóm tắt cột chính)
Bảng	Cột chính
media	id PK, transaction_id FK, kind ('photo','clip'), local_path, remote_url, size_bytes, duration_ms, created_at
budget_categories	PK(budget_id, category_id), cả hai là FK
saving_goals	id, name, target_amount, deadline, icon, archived
saving_goal_entries	id, goal_id FK, amount (+nạp/−rút), occurred_at, account_id FK
savings_deposits	id, account_id FK, bank_name, principal, rate_pa, term_months, start_date, maturity_date, status
investments	id, kind ('gold','stock'), symbol, quantity, buy_price, buy_date, note
loans	id, direction ('lend','borrow'), counterparty, principal, rate_pa, due_date, status
loan_payments	id, loan_id FK, amount, paid_at
recurring_rules	id, template_json, frequency ('daily','weekly','monthly','yearly'), interval, next_run, end_date, active
trips	id, name, start_date, end_date, budget_amount
friends	id, user_id, friend_uid, status ('pending','accepted','blocked')
groups / group_members	groups(id, name, owner_uid); group_members(PK(group_id, uid), role)
split_bills / split_shares	split_bills(id, group_id, payer_uid, total, method, note); split_shares(id, bill_id FK, member_uid/name, amount, status)
shared_transactions / messages	id, from_uid, to_uid, tx_snapshot_json / body, sent_at, read_at
reminders	id, kind ('bill','loan','statement'), ref_id, remind_at, repeat, done
work_hour_settings	user_id, hourly_wage, hours_per_day
subscriptions	user_id, plan, status, expires_at, original_txn_id
app_settings	key PK, value
usage_quota	date PK, used, limit
sync_queue	id, entity, entity_id, op ('upsert','delete'), payload, attempts, created_at
5.7. Thiết kế khóa chính và khóa ngoại
•Khóa chính là chuỗi ULID (sinh ngay trên thiết bị, không đụng độ giữa các thiết bị, sắp xếp theo thời gian).
•Khóa ngoại bật PRAGMA foreign_keys = ON. Quy tắc xóa: ON DELETE RESTRICT với accounts/categories đang có giao dịch (dùng xóa mềm/ẩn); ON DELETE CASCADE với bảng con thuần túy (media, loan_payments, split_shares, budget_categories).
5.8. Thiết kế các mối quan hệ giữa các bảng
Quan hệ	Loại	Ghi chú
accounts – transactions	1–N	Một giao dịch thuộc một tài khoản; chuyển tiền dùng thêm to_account_id
categories – transactions	1–N	Danh mục cha–con tự tham chiếu
transactions – media	1–N	Tối đa 1 ảnh + 1 clip mỗi giao dịch (ràng buộc ứng dụng)
budgets – categories	N–N	Qua budget_categories
groups – users	N–N	Qua group_members
split_bills – split_shares	1–N	Tổng amount của shares = total (BR13)
loans – loan_payments	1–N	Dư nợ = gốc − tổng đã trả
5.9. Chuẩn hóa cơ sở dữ liệu
Lược đồ được chuẩn hóa đến dạng chuẩn 3 (3NF): không có nhóm lặp (1NF); mọi thuộc tính phụ thuộc đầy đủ vào khóa (2NF), ví dụ bảng budget_categories chỉ chứa khóa ghép; không có phụ thuộc bắc cầu (3NF), ví dụ tên danh mục không lưu trong transactions. Có hai chỗ phi chuẩn hóa có chủ đích: (1) tx_snapshot_json trong shared_transactions để người nhận vẫn thấy dữ liệu khi người gửi sửa/xóa; (2) bộ nhớ đệm tổng hợp theo ngày cho lịch, tính lại được từ bảng gốc.
5.10. Các ràng buộc toàn vẹn dữ liệu
Bảng 5.8. Ràng buộc toàn vẹn
Loại	Ràng buộc
Miền giá trị	amount > 0; type, source, status dùng CHECK với tập giá trị cố định
Thực thể	PK không rỗng, duy nhất
Tham chiếu	FK bật toàn cục; không để tham chiếu treo
Nghiệp vụ	Mỗi người dùng đúng một is_default = 1 (chỉ mục duy nhất từng phần); type='transfer' thì to_account_id bắt buộc và khác account_id; type<>'transfer' thì category_id bắt buộc
Thời gian	occurred_at ≤ hiện tại cho giao dịch thủ công (kiểm tra ở tầng domain, BR05)
Nhất quán	Ghi giao dịch, media và sync_queue trong cùng một transaction SQLite
5.11. Thiết kế chỉ mục và tối ưu truy vấn
Bảng 5.9. Chỉ mục
Chỉ mục	Mục đích
transactions(deleted_at, occurred_at DESC)	Danh sách/lịch theo thời gian
transactions(account_id, occurred_at)	Số dư và lọc theo tài khoản
transactions(category_id, occurred_at)	Thống kê theo danh mục
transactions(sync_state) WHERE sync_state<>'synced'	Lấy nhanh bản ghi chờ đồng bộ
transactions(lat, lng) WHERE lat IS NOT NULL	Bản đồ chi tiêu
accounts(is_default) UNIQUE WHERE is_default=1	Đảm bảo BR06
split_shares(bill_id)	Tải phần chia
Ví dụ truy vấn thống kê chi theo danh mục trong tháng:
SELECT c.name, SUM(t.amount) AS total
FROM transactions t JOIN categories c ON c.id = t.category_id
WHERE t.type = 'expense' AND t.deleted_at IS NULL
  AND t.occurred_at >= :start AND t.occurred_at < :end
GROUP BY c.id ORDER BY total DESC;
Tối ưu thêm: phân trang theo con trỏ thời gian, dùng watch của Drift để chỉ cập nhật phần giao diện thay đổi, tính số dư bằng truy vấn tổng hợp có chỉ mục.
5.12. Chiến lược sao lưu và phục hồi dữ liệu
•Sao lưu liên tục: SyncEngine đẩy mọi thay đổi lên Firestore/Storage khi có mạng (sau khi liên kết Apple/Google; chế độ ẩn danh chỉ có sao lưu cục bộ).
•Sao lưu thủ công: xuất tệp sao lưu JSON/SQLite lưu vào iCloud Drive/Tệp.
•Phục hồi: đăng nhập cùng tài khoản trên thiết bị mới → kéo toàn bộ dữ liệu; hoặc nhập tệp sao lưu. Quá trình phục hồi chạy trong một transaction, hỏi xác nhận trước khi ghi đè.
•Giữ dữ liệu đã xóa: xóa mềm 30 ngày; sau đó dọn bằng tác vụ nền.
•Di trú lược đồ: mỗi phiên bản CSDL có migration đánh số, kiểm thử tự động trước khi phát hành.

CHƯƠNG 6. THIẾT KẾ GIAO DIỆN NGƯỜI DÙNG
6.1. Nguyên tắc thiết kế giao diện
Bảng 6.1. Nguyên tắc thiết kế giao diện (rút ra từ ứng dụng tham chiếu)
Nguyên tắc	Biểu hiện trong thiết kế
Ghi nhanh là trung tâm	Nút (+) nổi luôn hiện ở trang chủ; camera mở ngay; có "Bỏ qua ảnh"
Giảm tải nhận thức	Mỗi màn hình một mục tiêu; dùng thẻ bo tròn, nhãn chữ hoa nhỏ cho từng nhóm (TÊN CỦA BẠN, TIỀN TỆ, NGÔN NGỮ)
Giao diện tối là mặc định	Nền đen/xám đậm, màu nhấn theo ngữ nghĩa: đỏ = chi, xanh lá = thu, xanh dương = hành động chính, tím–hồng = tính năng thông minh, vàng = Pro
Gợi ý tại chỗ	Chip "35k = 35.000", "1tr2 = 1.200.000" hướng dẫn cú pháp ngay cạnh ô nhập
Tự động có thể chỉnh	Nhãn "Tự động" (xanh) cho tiền tệ và ngôn ngữ nói; người dùng vẫn đổi được
Trạng thái rỗng hữu ích	Thông báo "Chưa có ngân sách", "Chưa có sổ tiết kiệm" kèm hướng dẫn và nút hành động
Riêng tư	Biểu tượng con mắt để ẩn số tiền ở trang chủ, thẻ Chi/Thu và hồ sơ
Nhận diện Pro rõ ràng	Huy hiệu vàng PRO cạnh tính năng khóa; không chặn tính năng cốt lõi
6.2. Sơ đồ điều hướng hệ thống
Hình 6.1. Sơ đồ điều hướng hệ thống
Khởi động ──> [Chưa có hồ sơ?] ──Có──> S01 Hồ sơ ──> S02..S07 Onboarding (có "Bỏ qua") ──> Trang chủ + Coachmark 9 bước
          └─Không──────────────────────────────────────────────────────────────────────────> Trang chủ

Thanh điều hướng dưới (5 tab):
 [Trang chủ] ── (+) ──> Camera ──> Xác nhận giao dịch ──> chọn Danh mục / Tài khoản / Ngày
      |                    └─ biểu tượng danh sách ──> Nhập hàng loạt
      |── Ngày/Tháng, Tìm kiếm, Ẩn số, Ghim ── Chi tiết ngày ──> Sửa giao dịch
 [Thống kê] ── Tuần/Tháng/Năm ── Danh mục | Bản đồ ── Chi tiết danh mục
 [Tài khoản] ── Tài khoản | Khoản vay | Đầu tư ── Chi tiết tài khoản; Sổ tiết kiệm; Chuyến du lịch
 [Ngân sách] ── Danh sách ── Tạo từ mẫu / Tạo mới ── Mời người thân
 [Cá nhân] ── Nâng cấp (Paywall) | Đăng nhập Apple | Bạn bè | Nhóm | Giao dịch được chia sẻ | Chia tiền
             ── Công cụ: Định kỳ | Danh mục | Shortcut & Tự động hóa | Quy đổi giờ làm
             ── Tùy chọn: Ngôn ngữ | Giao diện | Tiền tệ | Mở từ widget | Khóa bảo mật | Cài đặt
             ── Hỗ trợ: Đánh giá | Góp ý | Chia sẻ ứng dụng
6.3. Thiết kế giao diện đăng nhập và xác thực
Ứng dụng dùng tài khoản ẩn danh ngay từ lần đầu nên không có màn hình đăng nhập bắt buộc, giúp người dùng bắt đầu ngay.
•S01. Hồ sơ lần đầu: vòng tròn "Thêm ảnh" với nút (+); ô "Tên của bạn"; chọn Tiền tệ (nhãn "Tự động", mặc định VND theo vùng); chọn Ngôn ngữ bằng 4 ô (Tiếng Việt, English, 日本語, 繁體中文); chọn Ngôn ngữ nói (dùng khi nhập bằng giọng nói); nút Bắt đầu chỉ bật khi đã nhập tên.
•S02–S07. Onboarding 6 trang: mỗi trang gồm biểu tượng lớn, tiêu đề, mô tả ngắn và thẻ liệt kê 3 tính năng (có huy hiệu PRO nếu cần), chấm phân trang, nút "Tiếp tục" và nút "Bỏ qua" ở góc phải: (1) Chụp một tấm, ghi xong ngay; (2) Mọi ví tiền trong một chỗ; (3) Đặt ngân sách, giữ đúng kế hoạch; (4) Nhìn lại để tiêu tốt hơn; (5) Tiền chung, chia cho rõ ràng; (6) Ghi nhanh mọi lúc mọi nơi.
•Liên kết tài khoản: tại tab Cá nhân, thẻ "Đăng nhập với Apple – Liên kết tài khoản để sao lưu"; sau khi liên kết hiển thị email đã ẩn và trạng thái đồng bộ.
•Khóa bảo mật: màn hình khóa khi mở app nếu bật (Face ID/PIN).
6.4. Thiết kế giao diện trang chủ
Các thành phần từ trên xuống (theo ảnh chụp):
1.Tiêu đề: "Chào buổi tối" + biểu tượng thời điểm trong ngày, tên người dùng, ảnh đại diện (góc phải).
2.Hàng trạng thái: chip "Hôm nay chưa chi tiêu" (khuyến khích ghi) và chip bộ đếm lượt "3/3".
3.Công tắc Ngày | Tháng, các nút Tìm kiếm, Ẩn/hiện số, Ghim/tiện ích.
4.Hai thẻ tổng: Chi (đỏ, mũi tên lên) và Thu (xanh lá, mũi tên xuống), mỗi thẻ có nút ẩn số.
5.Chip lọc tài khoản: Tất cả | Wallet | Bank.
6.Bộ chọn tháng ("tháng 10 2026") với mũi tên chuyển tháng.
7.Lịch tháng: lưới T2–CN; mỗi ô là vòng tròn (+) hoặc số tiền chi/thu của ngày đó; ngày hôm nay có chấm xanh, ngày tương lai bị mờ (BR05).
8.Nút (+) nổi ở góc dưới phải; thanh 5 tab dưới cùng.
9.Coachmark: lớp phủ tối làm nổi nút (+) kèm thẻ "Bước 1/9 – Thêm khoản CHI" và nút "Bỏ qua".
6.5. Thiết kế giao diện các chức năng chính
Bảng 6.2. Danh sách màn hình (cột "Ảnh" là số thứ tự ảnh chụp tham chiếu)
Mã	Màn hình	Ảnh	Use Case
S01	Hồ sơ lần đầu	1	UC01
S02–S07	Onboarding (6 trang)	2–7	UC01
S08	Trang chủ – lịch tháng (+ coachmark)	14, 8	UC05
S09	Camera ghi nhanh (flash, đổi camera, 1x/2x, Chọn từ thư viện, Bỏ qua ảnh)	9	UC02
S10	Xác nhận giao dịch (số tiền, Thêm chi tiết, Danh mục, Tài khoản, Chi/Thu, Ngày, Share, ✓, Thêm)	10	UC02
S11	Danh sách chọn danh mục	11	UC02, UC16
S12	Danh sách chọn tài khoản	12	UC02, UC09
S13	Nhập hàng loạt	13	UC04
S14	Thống kê (Tuần/Tháng/Năm; Danh mục/Bản đồ)	15	UC08
S15	Tài khoản (Tài khoản/Khoản vay/Đầu tư; Sổ tiết kiệm; Chuyến du lịch)	16	UC09, UC10, UC14, UC15
S16	Quản lý ngân sách (mẫu "Chi tiêu hàng tháng")	17	UC11
S17	Cá nhân (hồ sơ, Nâng cấp, Đăng nhập Apple, Bạn bè/Nhóm/Chia tiền, Công cụ, Tùy chọn, Hỗ trợ, phiên bản)	18–20	UC17–UC27
S18	Tìm kiếm giao dịch	–	UC07
S19	Chi tiết ngày / sửa giao dịch	–	UC05, UC06
S20	Soạn ngân sách	–	UC11
S21	Giao dịch định kỳ, Danh mục, Quy đổi giờ làm	–	UC13, UC16, UC24
S22	Bạn bè, Nhóm, Chia tiền, Giao dịch được chia sẻ	–	UC17–UC19
S23	Paywall (gói Pro)	–	UC27
S24	Xuất báo cáo	–	UC21
Mô tả nhanh các màn hình chính:
•S09 Camera: khung xem trước bo góc lớn; nút chụp tròn viền gradient tím–hồng ở giữa; nút danh sách ở bên phải mở S13; "Chọn từ thư viện" và "Bỏ qua ảnh" ở dưới; "Hủy" ở góc trái.
•S10 Xác nhận: ảnh vừa chụp làm nền, thẻ kính mờ chứa số tiền lớn (có dấu −/+) và ô "Thêm chi tiết"; hai chip Danh mục và Tài khoản; công tắc Chi/Thu; chip ngày (ví dụ 04/10); dải nút dưới: Share, ✓ (lưu), Thêm.
•S13 Nhập hàng loạt: chip ngày và tài khoản; vùng văn bản có ví dụ "cà phê 35k / grab 52k / ăn trưa 1tr2 / +lương 15tr"; nút Nói, Dán, các chip quy ước; trạng thái rỗng "Ghi nhanh như gõ ghi chú"; nút Lưu nằm dưới cùng.
•S14 Thống kê: ba tab Tuần/Tháng/Năm; thẻ Thu nhập và Chi tiêu (thẻ đang chọn có viền đỏ); thẻ Số dư; chuyển Danh mục/Bản đồ; khi chưa có dữ liệu hiện "Chưa có dữ liệu".
•S15 Tài khoản: thẻ tổng số dư có bộ lọc Tháng này/Năm nay/Tất cả; hàng thẻ tài khoản cuộn ngang (Wallet có dấu sao mặc định, Bank, ô "thêm" nét đứt); các khối Sổ tiết kiệm và Chuyến du lịch có trạng thái rỗng.
•S16 Ngân sách: trạng thái rỗng, nút (+), nút mời người thân, thẻ mẫu "Chi tiêu hàng tháng – Một hạn mức chung cho mọi khoản chi".
6.6. Thiết kế giao diện quản trị
Quản trị là bảng điều khiển web nhỏ (Flutter Web hoặc Firebase Console kèm trang tùy chỉnh), chỉ dành cho nhân sự được cấp quyền:
•Danh mục mẫu: thêm/sửa danh mục hệ thống theo ngôn ngữ.
•Góp ý người dùng: xem, gán trạng thái, trả lời.
•Hỗ trợ đăng ký: tra cứu trạng thái Pro, khôi phục quyền khi có sự cố.
•Báo cáo vận hành: số người dùng hoạt động, tỉ lệ lỗi, tỉ lệ chuyển đổi Pro (dữ liệu ẩn danh, tổng hợp).
•Cờ tính năng (feature flags): bật/tắt tính năng theo phiên bản.
6.7. Thiết kế giao diện tìm kiếm và thống kê
•Tìm kiếm (S18): ô nhập tự lấy tiêu điểm; gợi ý lịch sử tìm kiếm; bộ lọc: khoảng ngày, danh mục, tài khoản, loại, khoảng tiền; kết quả nhóm theo ngày, hiển thị tổng; chạm để mở S19.
•Thống kê (S14): biểu đồ tròn/cột theo danh mục (fl_chart), danh sách danh mục xếp theo tỉ lệ; biểu đồ đường xu hướng theo kỳ; tab Bản đồ hiển thị các điểm chi tiêu theo vị trí, chạm điểm xem giao dịch; trạng thái rỗng và trạng thái đang tải dùng khung xương (skeleton).
6.8. Mô tả các thành phần giao diện
Bảng 6.3. Thành phần giao diện dùng lại
Thành phần	Mô tả	Dùng ở
AppCard	Thẻ nền xám đậm, bo góc lớn	Hầu hết màn hình
SectionLabel	Nhãn chữ hoa nhỏ phía trên nhóm	S01, S17
MoneyText	Hiển thị tiền theo tiền tệ, hỗ trợ che số	S08, S14, S15
AmountInput	Ô nhập số tiền lớn, hiểu "k", "tr"	S10, S13
CategoryChip / AccountChip	Chip có biểu tượng, mở danh sách chọn bên dưới	S10
TypeToggle	Công tắc Chi (đỏ)/Thu (xanh)	S10
CalendarGrid	Lưới ngày với vòng (+) hoặc số tiền	S08
ProBadge	Nhãn vàng có vương miện	Mọi chỗ khóa Pro
EmptyState	Biểu tượng + tiêu đề + gợi ý + nút	S14–S16
PrimaryGradientButton	Nút gradient tím–hồng	Onboarding, nút chụp
Coachmark	Lớp phủ làm nổi phần tử + thẻ bước	S08
6.9. Thiết kế thông báo và xử lý lỗi giao diện
Bảng 6.4. Thông báo và xử lý lỗi
Tình huống	Kiểu hiển thị	Nội dung ví dụ
Lưu thành công	Snackbar ngắn + rung nhẹ	"Đã lưu 35.000đ vào Wallet"
Số tiền không hợp lệ	Viền đỏ và dòng lỗi dưới ô	"Nhập số tiền lớn hơn 0"
Từ chối quyền camera	Hộp thoại + nút "Mở cài đặt"	"Cần quyền camera để chụp; bạn có thể bỏ qua ảnh"
Mất mạng	Chấm "chờ đồng bộ" ở góc, không chặn thao tác	"Sẽ đồng bộ khi có mạng"
Cảnh báo ngân sách	Thông báo đẩy + thẻ ở tab Ngân sách	"Bạn đã dùng 80% ngân sách tháng này"
Tính năng Pro	Bottom sheet paywall	"Mở khóa xuất PDF/Excel"
Lỗi không mong đợi	Màn hình lỗi thân thiện + nút "Thử lại" + gửi báo cáo ẩn danh	"Có lỗi xảy ra. Thử lại"
Xóa dữ liệu	Hộp thoại xác nhận + "Hoàn tác" 5 giây	"Đã xóa giao dịch – Hoàn tác"
6.10. Thiết kế trải nghiệm người dùng (UX) và khả năng tiếp cận
•Hiệu quả: từ khi mở app đến khi lưu một khoản chi ≤ 3 chạm; ghi nhớ danh mục và tài khoản dùng gần nhất.
•Học nhanh: onboarding 6 trang có thể bỏ qua và coachmark 9 bước có thể bật lại trong cài đặt.
•Phản hồi: hiệu ứng chuyển cảnh ngắn, rung xúc giác khi lưu, trạng thái tải rõ ràng.
•Đa ngôn ngữ: mọi chuỗi qua ARB; định dạng số/ngày theo vùng; kiểm tra tràn chữ tiếng Nhật/Trung/Việt.
•Tiếp cận: nhãn ngữ nghĩa cho VoiceOver/TalkBack, vùng chạm ≥ 44×44 pt, hỗ trợ Dynamic Type, không dùng màu làm tín hiệu duy nhất (kèm mũi tên/dấu +/−).
•Thích ứng: bố cục chạy tốt từ iPhone SE đến máy lớn; hỗ trợ xoay ngang cho thống kê (tùy chọn).

CHƯƠNG 7. THIẾT KẾ API VÀ TÍCH HỢP HỆ THỐNG
7.1. Tổng quan giao tiếp giữa các thành phần
Phần lớn nghiệp vụ chạy cục bộ. Ứng dụng chỉ giao tiếp mạng cho: xác thực, đồng bộ, tải media, chức năng xã hội, kiểm tra đăng ký, xuất báo cáo lớn và thông báo đẩy. Giao tiếp gồm hai kênh: (1) Firebase SDK cho Auth/Firestore/Storage (đồng bộ bản ghi theo từng người dùng); (2) HTTPS API do Cloud Functions cung cấp cho các thao tác cần kiểm soát phía máy chủ (chia bill, xác thực biên lai, quan hệ bạn bè).
7.2. Nguyên tắc thiết kế API
•Kiểu REST, dữ liệu JSON UTF-8, tiền tố phiên bản /v1.
•Tài nguyên dạng danh từ số nhiều, hành động bằng HTTP method; thao tác không chuẩn REST dùng :action.
•Idempotent: mọi yêu cầu ghi nhận tiêu đề Idempotency-Key để thử lại an toàn khi mất mạng.
•Thời gian dùng ISO 8601/UTC; tiền là số nguyên kèm currency.
•Phân trang bằng cursor; giới hạn tốc độ theo người dùng; phản hồi lỗi thống nhất (7.6).
•Mọi endpoint yêu cầu Authorization: Bearer <Firebase ID Token> (trừ webhook có chữ ký riêng).
7.3. Danh sách các API hoặc giao diện tích hợp
Bảng 7.1. Danh sách API
Mã	Method & đường dẫn	Mô tả	Pro
API01	POST /v1/auth/link	Liên kết tài khoản ẩn danh với Apple/Google	
API02	DELETE /v1/account	Xóa tài khoản và toàn bộ dữ liệu	
API03	POST /v1/sync/push	Đẩy lô bản ghi thay đổi	
API04	GET /v1/sync/pull?since=	Kéo thay đổi mới kể từ mốc	
API05	POST /v1/media:upload-url	Lấy URL ký sẵn để tải ảnh/clip	
API06	POST /v1/friends:invite	Gửi lời mời kết bạn	
API07	POST /v1/friends/{id}:respond	Chấp nhận/từ chối	
API08	POST /v1/groups	Tạo nhóm; GET/PATCH/DELETE /v1/groups/{id}	
API09	POST /v1/split-bills	Tạo hóa đơn chia	
API10	PATCH /v1/split-bills/{id}/shares/{shareId}	Đánh dấu đã trả	
API11	POST /v1/shared-transactions	Gửi giao dịch cho bạn	
API12	POST /v1/messages	Gửi tin nhắn trong app	
API13	POST /v1/subscriptions:verify	Xác thực biên lai IAP, cập nhật quyền	
API14	POST /v1/budgets/{id}/members	Chia sẻ ngân sách/tài khoản	✔
API15	POST /v1/reports:export	Xuất báo cáo PDF/Excel cỡ lớn (tùy chọn phía máy chủ)	✔
API16	POST /v1/ocr:extract	OCR đám mây (tùy chọn, khi OCR cục bộ thất bại)	
API17	POST /v1/feedback	Gửi góp ý	
7.4. Đặc tả các endpoint
API03 – POST /v1/sync/push
•Mục đích: gửi các bản ghi đã thay đổi trên thiết bị.
•Header: Authorization, Idempotency-Key, Content-Type: application/json.
•Phản hồi thành công: 200 kèm danh sách accepted và conflicts.
7.5. Thiết kế dữ liệu request và response
Yêu cầu đẩy đồng bộ (API03):
{
  "deviceId": "d-7f3a",
  "changes": [
    {
      "entity": "transaction",
      "op": "upsert",
      "id": "01J9Z6Q8K3M2X5T7V1N4B0C9DA",
      "updatedAt": "2026-10-28T13:30:12Z",
      "data": {
        "accountId": "01J9Z...", "categoryId": "01J9Y...", "type": "expense",
        "amount": 35000, "currency": "VND", "note": "cà phê",
        "occurredAt": "2026-10-28T13:29:50Z", "source": "bulk"
      }
    }
  ]
}
Phản hồi:
{ "accepted": ["01J9Z6Q8K3M2X5T7V1N4B0C9DA"], "conflicts": [], "serverTime": "2026-10-28T13:30:13Z" }
Yêu cầu tạo hóa đơn chia (API09):
{
  "groupId": "g-91", "payerUid": "u-12", "total": 900000, "currency": "VND",
  "method": "equal", "note": "Ăn tối nhóm",
  "shares": [ { "uid": "u-12", "amount": 300000 }, { "uid": "u-34", "amount": 300000 }, { "name": "Lan", "amount": 300000 } ]
}
Phản hồi 201: { "id": "sb-55", "status": "open", "shares": [ { "id": "sh-1", "status": "paid" }, { "id": "sh-2", "status": "unpaid" }, { "id": "sh-3", "status": "unpaid" } ] }
Yêu cầu xác thực biên lai (API13): { "platform": "ios", "receipt": "<base64>", "productId": "snapmoney.pro.yearly" } → Phản hồi: { "plan": "pro_yearly", "status": "active", "expiresAt": "2027-10-28T00:00:00Z" }
7.6. Quy ước mã trạng thái và thông báo lỗi
Bảng 7.2. Mã trạng thái
Mã	Ý nghĩa	Ví dụ
200 / 201 / 204	Thành công / đã tạo / không có nội dung	Đồng bộ, tạo bill, xóa
400	Dữ liệu không hợp lệ	Tổng các phần chia ≠ tổng bill
401	Thiếu hoặc hết hạn token	Cần đăng nhập lại
403	Không đủ quyền	Tính năng Pro khi chưa đăng ký
404	Không tìm thấy	Nhóm không tồn tại
409	Xung đột phiên bản	Bản ghi đã được sửa ở thiết bị khác
422	Vi phạm quy tắc nghiệp vụ	Ngày giao dịch ở tương lai
429	Vượt giới hạn tốc độ	Gửi quá nhiều yêu cầu
500 / 503	Lỗi máy chủ / dịch vụ tạm ngưng	Thử lại với backoff
Định dạng lỗi thống nhất:
{ "error": { "code": "SPLIT_TOTAL_MISMATCH", "message": "Tổng các phần chia phải bằng tổng hóa đơn.", "details": { "expected": 900000, "actual": 850000 }, "requestId": "r-8a1c" } }
7.7. Cơ chế xác thực và phân quyền API
•Xác thực: Firebase ID Token (JWT, hiệu lực 1 giờ, tự làm mới); người dùng ẩn danh vẫn có uid, khi liên kết Apple thì giữ nguyên uid.
•Phân quyền: kiểm tra ở Cloud Functions và Firestore Security Rules theo uid và quyền (owner/member); kiểm tra entitlement phía máy chủ cho API Pro.
•Chống lạm dụng: giới hạn tốc độ, App Check (Apple App Attest/Play Integrity) để chỉ ứng dụng chính thức được gọi.
7.8. Tích hợp với các hệ thống bên ngoài
Bảng 7.3. Tích hợp bên ngoài
Hệ thống	Mục đích	Cách tích hợp	Dữ liệu trao đổi
Sign in with Apple / Google	Đăng nhập, liên kết	SDK + Firebase Auth	Token danh tính, email (có thể ẩn)
App Store / Google Play (IAP)	Thanh toán gói Pro	StoreKit/Billing qua RevenueCat hoặc in_app_purchase + API13	Biên lai, trạng thái đăng ký
APNs / FCM	Thông báo đẩy	Firebase Messaging	Token thiết bị
Dịch vụ bản đồ	Bản đồ chi tiêu, địa chỉ từ tọa độ	google_maps_flutter, geocoding	Tọa độ, tên địa điểm
Nhận dạng giọng nói của hệ điều hành	Nhập bằng giọng nói	speech_to_text	Âm thanh xử lý trên thiết bị hoặc theo chính sách hệ điều hành
App Intents / Shortcuts / WidgetKit	Siri, widget, lưu từ ảnh chụp màn hình	Platform channel + App Group chia sẻ dữ liệu	Số dư, ngân sách (chỉ đọc)
iCloud Drive / Tệp	Sao lưu thủ công	share_plus, file_picker	Tệp sao lưu

CHƯƠNG 8. THIẾT KẾ BẢO MẬT VÀ XỬ LÝ LỖI
8.1. Thiết kế xác thực người dùng
•Ẩn danh trước, liên kết sau: tạo tài khoản ẩn danh khi mở app lần đầu; liên kết Apple/Google để sao lưu và dùng nhiều thiết bị mà không mất dữ liệu.
•Khóa ứng dụng: Face ID/Touch ID/vân tay (local_auth) hoặc mã PIN 4–6 số; khóa lại sau khi app ở nền quá thời gian chọn (ngay lập tức, 1 phút, 5 phút).
•Phiên: token lưu bằng flutter_secure_storage (Keychain/Keystore); đăng xuất xóa token và khóa cục bộ.
•Xóa tài khoản: có nút trong Cài đặt, xóa dữ liệu đám mây và media (API02), theo yêu cầu của Apple và Nghị định 13/2023/NĐ-CP.
8.2. Thiết kế phân quyền truy cập
Bảng 8.1. Phân quyền
Tài nguyên	Chủ sở hữu	Thành viên nhóm / bạn bè	Quản trị	Ghi chú
Giao dịch, tài khoản, ngân sách cá nhân	Đọc/ghi	Không	Không	Cô lập theo uid
Ngân sách/tài khoản được chia sẻ (Pro)	Đọc/ghi	Theo vai trò: xem hoặc sửa	Không	Chủ sở hữu thu hồi được
Nhóm	Quản lý	Xem, rời nhóm	Không	Chủ nhóm mời/xóa thành viên
Hóa đơn chia	Người tạo ghi	Thành viên xem phần của mình, đánh dấu phần mình	Không	
Giao dịch được chia sẻ	Người gửi	Người nhận chỉ đọc bản chụp	Không	
Danh mục mẫu, cờ tính năng	Chỉ đọc	Chỉ đọc	Đọc/ghi	Qua trang quản trị
Đăng ký Pro	Chỉ đọc	Không	Đọc/ghi (hỗ trợ)	Ghi chủ yếu do máy chủ
8.3. Bảo vệ dữ liệu nhạy cảm
Bảng 8.2. Dữ liệu nhạy cảm và biện pháp
Dữ liệu	Mức nhạy cảm	Biện pháp
Số tiền, giao dịch, số dư	Cao	CSDL cục bộ có thể mã hóa (SQLCipher); TLS khi truyền; Firestore Rules theo uid
Ảnh hóa đơn, clip	Cao	Lưu trong thư mục riêng của app, bucket riêng tư, URL ký sẵn hết hạn ngắn
Vị trí	Trung bình	Chỉ lưu khi người dùng bật; làm tròn tọa độ nếu chia sẻ
Token, khóa	Cao	Keychain/Keystore, không ghi log
Email, tên	Trung bình	Dùng "Hide My Email" của Apple khi có thể
Ảnh chụp màn hình app	Trung bình	Làm mờ giao diện ở trình chuyển app khi khóa bảo mật bật
Ứng dụng không lưu mật khẩu/thẻ ngân hàng; chỉ lưu dữ liệu người dùng tự nhập.
8.4. Kiểm tra và xác thực dữ liệu đầu vào
•Phía thiết bị: kiểm tra kiểu và biên (số tiền > 0 và ≤ 10^13, độ dài ghi chú ≤ 500 ký tự, ngày hợp lệ); chuẩn hóa văn bản khi phân tích hàng loạt (bỏ ký tự điều khiển, hiểu "k", "tr", dấu chấm/phẩy).
•Phía máy chủ: kiểm tra lại toàn bộ bằng lược đồ (JSON Schema) và quy tắc nghiệp vụ (BR13, BR05); không tin dữ liệu từ máy khách.
•Tệp media: giới hạn loại (JPEG/HEIC/MP4), dung lượng (ảnh ≤ 10 MB, clip ≤ 20 MB, ≤ 3 giây) và quét kiểu nội dung.
•Truy vấn SQL: dùng tham số hóa của Drift, không nối chuỗi.
8.5. Phòng chống các lỗ hổng bảo mật
Bảng 8.3. Lỗ hổng và biện pháp (tham chiếu OWASP Mobile Top 10)
Rủi ro	Biện pháp
Lưu trữ dữ liệu không an toàn	Secure storage, mã hóa CSDL, loại trừ khỏi sao lưu không mã hóa của hệ điều hành nếu cần
Truyền thông không an toàn	Bắt buộc HTTPS/TLS 1.2+; cân nhắc certificate pinning cho API riêng
Xác thực/ủy quyền yếu	Firebase Auth, Rules theo uid, kiểm tra quyền phía máy chủ
Tiêm mã (SQL, JSON)	Truy vấn tham số hóa; kiểm tra lược đồ
Dịch ngược / sửa ứng dụng	Obfuscate Dart (--obfuscate --split-debug-info), App Check, không đặt khóa bí mật trong app
Lạm dụng tính năng Pro	Kiểm tra entitlement phía máy chủ; xác thực biên lai
Rò rỉ qua log/ảnh chụp màn hình	Không ghi dữ liệu nhạy cảm vào log; làm mờ ở trình chuyển app
Thư viện bên thứ ba có lỗ hổng	Cố định phiên bản, quét phụ thuộc (Dependabot), kiểm tra giấy phép
8.6. Thiết kế ghi log và giám sát
•Log cục bộ: mức debug/info/warn/error qua một lớp AppLogger; bản phát hành chỉ giữ warn/error, không ghi số tiền, ghi chú, vị trí.
•Giám sát lỗi: Firebase Crashlytics (crash và lỗi không gây crash), kèm phiên bản app và hệ điều hành.
•Chỉ số sử dụng ẩn danh: Firebase Analytics, người dùng có thể tắt trong Cài đặt.
•Nhật ký máy chủ: Cloud Logging cho Cloud Functions, đặt cảnh báo khi tỉ lệ lỗi 5xx tăng.
•Nhật ký đồng bộ: lưu số lần thử và mã lỗi trong sync_queue để chẩn đoán.
8.7. Xử lý ngoại lệ và phục hồi khi xảy ra lỗi
•Dùng kiểu Result<T, AppError> ở tầng domain/data thay vì ném ngoại lệ lên giao diện; AppError gồm Validation, Permission, Network, Storage, Auth, Entitlement, Unknown.
•Bắt lỗi toàn cục bằng FlutterError.onError và PlatformDispatcher.instance.onError, gửi lên Crashlytics và hiển thị màn hình lỗi thân thiện.
•Ghi dữ liệu theo transaction SQLite; nếu lỗi sẽ rollback, không để dữ liệu dở dang.
•Đồng bộ lỗi tạm thời dùng retry với exponential backoff (1s, 2s, 4s… tối đa 15 phút); lỗi vĩnh viễn (400/422) đưa vào danh sách "cần xem lại" thay vì thử mãi.
•Khi CSDL hỏng: chạy PRAGMA integrity_check, nếu thất bại thì đề nghị khôi phục từ bản sao lưu gần nhất.
8.8. Sao lưu và khôi phục hệ thống
Áp dụng chiến lược ở mục 5.12 cho dữ liệu người dùng. Với hạ tầng: bật sao lưu định kỳ hằng ngày cho Firestore (Point-in-time recovery), lưu phiên bản đối tượng (object versioning) cho Storage, quản lý mã Cloud Functions và Security Rules trong Git; có kịch bản khôi phục được diễn tập mỗi quý.

CHƯƠNG 9. KIỂM THỬ HỆ THỐNG
9.1. Mục tiêu kiểm thử
•Xác nhận hệ thống đáp ứng các yêu cầu chức năng (FR) và phi chức năng (NFR) ở Chương 2.
•Bảo đảm tính đúng đắn của các phép tính tài chính: số dư, ngân sách, lãi tiết kiệm, chia bill.
•Phát hiện sớm lỗi về đồng bộ, offline, quyền truy cập và bảo mật.
•Bảo đảm trải nghiệm ổn định trên các thiết bị và phiên bản hệ điều hành mục tiêu.
9.2. Phạm vi kiểm thử
Bảng 9.1. Phạm vi kiểm thử
Thuộc phạm vi	Ngoài phạm vi (giai đoạn này)
Toàn bộ FR mức Must/Should; các NFR hiệu năng, offline, bảo mật cơ bản, đa ngôn ngữ, tiếp cận	Kiểm thử tải quy mô hàng triệu người dùng
Cloud Functions, Firestore Rules, luồng IAP (môi trường sandbox)	Kiểm thử xâm nhập (penetration test) do bên thứ ba
iOS 15+ và Android 8.0+ trên thiết bị đại diện	Thiết bị gập, Android TV, đồng hồ
9.3. Môi trường kiểm thử
•Thiết bị: iPhone SE (2020), iPhone 13, iPhone 15 Pro; Google Pixel 6, Samsung Galaxy A-series (tầm trung); bổ sung giả lập iOS/Android cho các phiên bản cũ.
•Backend: dự án Firebase staging riêng, dùng Firebase Emulator Suite cho kiểm thử tự động (Auth, Firestore, Functions, Storage).
•Thanh toán: App Store Sandbox và Google Play license testers.
•Công cụ: flutter_test, mocktail, integration_test, Firebase Test Lab, GitHub Actions.
9.4. Chiến lược và phương pháp kiểm thử
•Kim tự tháp kiểm thử: khoảng 70% kiểm thử đơn vị, 20% kiểm thử widget/tích hợp, 10% kiểm thử đầu cuối (E2E).
•Dựa trên rủi ro: ưu tiên các vùng nhạy cảm (tính tiền, đồng bộ, quyền Pro, xóa dữ liệu).
•Hộp đen + hộp trắng: hộp trắng cho domain/data; hộp đen cho luồng người dùng.
•Tự động hóa: mọi pull request chạy phân tích tĩnh (flutter analyze), unit/widget test; nhánh chính chạy thêm integration test.
•Kiểm thử hồi quy: bộ test tự động chạy lại trước mỗi bản phát hành.
9.5. Kiểm thử đơn vị (Unit Testing)
Đối tượng kiểm thử: Money (cộng trừ, định dạng), ParseBulkInput (cú pháp 35k, 1tr2, +lương 15tr), Categorizer, BudgetProgress, SplitBill (làm tròn, kiểm tổng), công thức lãi tiết kiệm, RecurringScheduler (tạo bù), EntitlementService.
test('ParseBulkInput hiểu 35k, 1tr2 và dòng thu nhập', () {
  final r = ParseBulkInput(Categorizer.vi())('cà phê 35k\năn trưa 1tr2\n+lương 15tr');
  expect(r[0].amount, 35000);
  expect(r[1].amount, 1200000);
  expect(r[2].type, TxType.income);
  expect(r[2].amount, 15000000);
});
9.6. Kiểm thử tích hợp (Integration Testing)
•Repository + Drift (CSDL trong bộ nhớ): thêm/sửa/xóa mềm, truy vấn thống kê, migration giữa các phiên bản.
•SyncEngine + Firebase Emulator: đẩy/kéo, mất mạng giữa chừng, xung đột bản ghi, thử lại.
•Notifier + UseCase + Repository: luồng lưu giao dịch cập nhật ngân sách và số dư.
•Cloud Functions: chia bill, xác thực biên lai (dùng dữ liệu sandbox), Security Rules (người dùng A không đọc được dữ liệu người dùng B).
9.7. Kiểm thử hệ thống (System Testing)
•E2E: kịch bản từ hồ sơ lần đầu → ghi giao dịch → xem thống kê → tạo ngân sách → nâng cấp Pro.
•Hiệu năng: đo khởi động, thời gian mở camera/form, cuộn lịch với 100.000 giao dịch (NFR01, NFR02).
•Offline/đồng bộ: bật chế độ máy bay, ghi nhiều giao dịch, bật mạng và đối chiếu (NFR04).
•Bảo mật: thử truy cập chéo người dùng, token hết hạn, chỉnh sửa gói tin; kiểm tra dữ liệu không xuất hiện trong log.
•Đa ngôn ngữ và tiếp cận: duyệt 4 ngôn ngữ, Dynamic Type lớn nhất, VoiceOver/TalkBack.
•Tương thích: chạy bộ smoke test trên ma trận thiết bị (9.3).
9.8. Kiểm thử chấp nhận (Acceptance Testing)
•10–15 người dùng mục tiêu (sinh viên, nhân viên văn phòng) dùng thử 1–2 tuần qua TestFlight/Firebase App Distribution.
•Nhiệm vụ mẫu: ghi 5 khoản chi bằng 3 cách (ảnh, bulk, giọng nói), tạo ngân sách, chia bill với bạn.
•Chỉ số: tỉ lệ hoàn thành nhiệm vụ ≥ 90%, thời gian ghi một khoản chi trung bình ≤ 10 giây, điểm SUS ≥ 75, không còn lỗi mức Critical/High.
9.9. Danh sách các trường hợp kiểm thử (Test Cases)
Bảng 9.2. Danh sách test case
Mã	Mô tả và dữ liệu vào	Kết quả mong đợi	FR
TC01	Hồ sơ lần đầu, tên rỗng	Nút "Bắt đầu" bị vô hiệu	FR01
TC02	Nhập tên hợp lệ, chọn English	Giao diện chuyển English; ngôn ngữ nói mặc định phù hợp	FR01, FR05
TC03	Nhấn "Bỏ qua" ở onboarding	Vào thẳng trang chủ, coachmark vẫn hiển thị	FR02
TC04	Lưu chi 35.000đ vào Wallet	Số dư Wallet giảm 35.000đ; lịch ngày hiển thị số tiền	FR06
TC05	Lưu với số tiền 0	Báo lỗi, không tạo giao dịch	FR06
TC06	Chọn ngày trong tương lai	Ô bị mờ/không chọn được	FR06
TC07	Từ chối quyền camera	Hiện hướng dẫn; "Bỏ qua ảnh" vẫn dùng được	FR07
TC08	Chọn ảnh từ thư viện	Ảnh được lưu kèm giao dịch	FR07
TC09	Quét hóa đơn có dòng "TỔNG 125.000"	Số tiền tự điền 125.000	FR08
TC10	Ảnh mờ, OCR không đọc được	Số tiền = 0, cho nhập tay	FR08
TC11	Free bấm quay clip 3 giây	Hiện paywall	FR09, FR40
TC12	Bulk: "cà phê 35k"	35.000đ, danh mục đoán là Cà phê	FR11
TC13	Bulk: "ăn trưa 1tr2"	1.200.000đ	FR11
TC14	Bulk: "+lương 15tr"	Khoản thu 15.000.000đ	FR11
TC15	Bulk có dòng không có số	Đánh dấu lỗi dòng đó, bỏ qua khi lưu	FR11
TC16	Bulk 4 dòng, giả lập lỗi ghi ở dòng 3	Rollback, không có giao dịch nào được tạo	FR11
TC17	Nói "grab năm mươi hai nghìn" (ngôn ngữ nói: Tiếng Việt)	Văn bản được nhận và đưa vào ô nhập	FR12
TC18	Xóa giao dịch rồi khôi phục	Biến mất khỏi danh sách; khôi phục được trong 30 ngày	FR13
TC19	Tìm "cà phê" lọc theo tháng	Chỉ trả các giao dịch khớp, tổng đúng	FR14
TC20	Chuyển 500.000đ từ Bank sang Wallet	Số dư hai bên đổi đúng; tổng Chi/Thu không đổi	FR17
TC21	Đặt tài khoản mặc định khác	Chỉ một tài khoản có dấu sao	FR17
TC22	Ngân sách 1.000.000đ, chi 800.000đ	Hiện cảnh báo 80%	FR23, FR24
TC23	Định kỳ hằng tháng, mở app trễ 2 kỳ	Tạo bù 2 giao dịch đúng ngày	FR26
TC24	Thống kê tháng có 3 danh mục	Tổng và tỉ lệ phần trăm khớp dữ liệu	FR27
TC25	Xuất PDF khi Free / khi Pro	Free: paywall; Pro: tạo được tệp	FR29, FR40
TC26	Chia bill 100.000đ cho 3 người	Phần dư 1đ gán cho người trả; tổng = 100.000đ	FR33
TC27	Máy bay: ghi 5 giao dịch rồi bật mạng	Đồng bộ đủ 5 giao dịch trong 30 giây	FR41
TC28	Sửa cùng giao dịch trên 2 thiết bị	Bản có updated_at mới hơn thắng	FR41
TC29	Bật khóa Face ID rồi mở lại app	Yêu cầu xác thực sinh trắc học	FR04
TC30	Mua Pro (sandbox) rồi khôi phục trên máy mới	Quyền Pro hiệu lực	FR40
TC31	Widget số dư, bật "ẩn số"	Widget không lộ số tiền	FR36, FR16
TC32	Tạo nhắc hóa đơn lúc 20:00	Thông báo hiện đúng giờ	FR38
9.10. Tiêu chí đánh giá kết quả kiểm thử
•Tỉ lệ test case đạt ≥ 95%; không còn lỗi mức Critical/High; lỗi Medium có kế hoạch sửa.
•Độ phủ mã: domain ≥ 80%, toàn dự án ≥ 60%.
•Hiệu năng đạt NFR01–NFR03; crash-free ≥ 99,5% trong thử nghiệm kín.
•Mọi FR mức Must có ít nhất một test case đạt (xem ma trận truy vết).
9.11. Ma trận truy vết yêu cầu và kiểm thử
Bảng 9.3. Ma trận truy vết FR → Use Case → Test Case
FR	Use Case	Test case	Ghi chú
FR01, FR02	UC01	TC01–TC03	
FR03, FR41	UC25	TC27, TC28	Bổ sung TC liên kết Apple khi hoàn thiện
FR04	UC26	TC29	
FR05	UC26	TC02	
FR06, FR07	UC02	TC04–TC08	
FR08	UC02	TC09, TC10	
FR09	UC03	TC11	
FR11, FR12	UC04	TC12–TC17	
FR13	UC06	TC18	
FR14	UC07	TC19	
FR16, FR36	UC05, UC22	TC31	
FR17	UC09	TC20, TC21	
FR23, FR24	UC11	TC22	
FR26	UC13	TC23	
FR27	UC08	TC24	
FR29	UC21	TC25	
FR33	UC18	TC26	
FR38	UC23	TC32	
FR40	UC27	TC11, TC25, TC30	
FR10, FR15, FR18–FR22, FR25, FR28, FR30–FR32, FR34, FR35, FR37, FR39, FR42	UC05, UC10, UC12, UC14–UC17, UC19, UC20, UC22, UC24	Chưa có	Sẽ bổ sung test case ở các sprint tương ứng

CHƯƠNG 10. TRIỂN KHAI VÀ VẬN HÀNH HỆ THỐNG
10.1. Yêu cầu phần cứng và phần mềm
Bảng 10.1. Yêu cầu phần cứng/phần mềm
Đối tượng	Yêu cầu
Máy phát triển	macOS (bắt buộc để build iOS) hoặc Windows/Linux (Android); RAM ≥ 16 GB; SSD ≥ 50 GB
Phần mềm phát triển	Flutter SDK 3.x, Dart 3.x, Xcode (bản mới nhất được Apple hỗ trợ), Android Studio, CocoaPods, Node.js LTS (Cloud Functions), Firebase CLI
Điện thoại người dùng	iOS 15+ (iPhone 6s trở lên); Android 8.0+ (API 26), RAM ≥ 3 GB; camera sau; micro (tùy chọn); GPS (tùy chọn)
Máy chủ	Không có máy chủ riêng; dùng dịch vụ Firebase (gói Blaze trả theo mức dùng khi cần Cloud Functions)
Tài khoản	Apple Developer Program, Google Play Console, dự án Firebase, tài khoản RevenueCat (nếu dùng)
10.2. Môi trường triển khai
Bảng 10.2. Môi trường
Môi trường	Mục đích	Backend	Phân phối
dev	Phát triển, thử nhanh	Firebase Emulator / dự án dev	Chạy trực tiếp trên máy/giả lập
staging	Kiểm thử, UAT	Dự án Firebase staging	TestFlight, Firebase App Distribution
production	Người dùng thật	Dự án Firebase production	App Store, Google Play
Mỗi môi trường tương ứng một flavor của Flutter (bundle ID/applicationId riêng, biểu tượng riêng) để cài song song.
10.3. Cấu hình hệ thống
•pubspec.yaml cố định phiên bản thư viện; analysis_options.yaml bật lint nghiêm ngặt.
•Cấu hình Firebase theo flavor (GoogleService-Info.plist, google-services.json).
•Quyền hệ điều hành: NSCameraUsageDescription, NSMicrophoneUsageDescription, NSSpeechRecognitionUsageDescription, NSLocationWhenInUseUsageDescription, NSFaceIDUsageDescription (iOS); quyền tương ứng trong AndroidManifest.xml.
•Bật Capabilities: Sign in with Apple, Push Notifications, App Groups (widget), iCloud (sao lưu tệp), In-App Purchase.
•Remote Config cho cờ tính năng và phiên bản tối thiểu.
10.4. Quy trình cài đặt
# 1. Lấy mã nguồn và phụ thuộc
git clone <repo> && cd snapmoney
flutter pub get

# 2. Sinh mã (Drift, Riverpod, l10n)
dart run build_runner build --delete-conflicting-outputs
flutter gen-l10n

# 3. Cấu hình Firebase theo môi trường
firebase use staging
flutterfire configure --project=<staging-project-id>

# 4. Chạy thử
flutter run --flavor dev --dart-define-from-file=env/dev.json

# 5. Triển khai backend
cd functions && npm ci && firebase deploy --only functions,firestore:rules,storage
10.5. Quy trình khởi chạy hệ thống
Thứ tự khởi tạo trong main(): (1) WidgetsFlutterBinding.ensureInitialized; (2) nạp cấu hình môi trường; (3) khởi tạo Firebase, App Check, Crashlytics; (4) mở CSDL Drift và chạy migration; (5) khởi tạo kênh thông báo, đọc cài đặt khóa/ngôn ngữ/giao diện; (6) đăng nhập ẩn danh nếu chưa có phiên; (7) khởi động SyncEngine ở nền; (8) runApp(ProviderScope(...)). Màn hình chờ (splash) giữ đến khi bước (4) hoàn tất để tránh hiển thị dữ liệu thiếu.
10.6. Cấu hình và khởi tạo cơ sở dữ liệu
•Lần đầu: tạo lược đồ phiên bản hiện hành, gieo dữ liệu mặc định gồm tài khoản "Wallet" (mặc định) và "Bank", bộ danh mục hệ thống theo ngôn ngữ (Ăn uống, Mua sắm, Di chuyển, Cà phê, …, và nhóm thu nhập như Lương).
•Nâng cấp: migration tuần tự theo số phiên bản, kèm kiểm thử trên bản sao dữ liệu đã ẩn danh.
•Firestore: triển khai firestore.rules và firestore.indexes.json; tạo bucket Storage riêng tư.
10.7. Quản lý cấu hình và biến môi trường
Bảng 10.3. Biến môi trường
Biến	Nơi lưu	Ví dụ	Ghi chú
ENV	--dart-define-from-file	dev / staging / prod	Chọn môi trường
API_BASE_URL	env/*.json	https://<region>-<project>.cloudfunctions.net	Công khai
REVENUECAT_PUBLIC_KEY	env/*.json	appl_xxx	Khóa công khai của SDK
MAPS_API_KEY	Cấu hình native, giới hạn theo bundle ID	–	Giới hạn theo ứng dụng
APPLE_SHARED_SECRET, SERVICE_ACCOUNT	Secret Manager (Cloud Functions)	–	Không đưa vào app hay Git
FEATURE_FLAGS	Firebase Remote Config	{"clip": true}	Bật/tắt từ xa
Hình 10.1. Sơ đồ triển khai và CI/CD
Lập trình viên --push/PR--> GitHub --Actions--> [analyze + test + build]
                                  |-- nhánh develop --> Firebase App Distribution (staging)
                                  |-- tag vX.Y.Z   --> Fastlane --> TestFlight --> App Store
                                  |                              \--> Google Play (internal -> production)
                                  \-- thư mục functions/ --> firebase deploy (staging -> production)

Thiết bị người dùng <--HTTPS--> Firebase (Auth, Firestore, Storage, Functions, FCM) <--> Apple/Google
10.8. Hướng dẫn sử dụng cơ bản
1.Bắt đầu: mở app, nhập tên, chọn ngôn ngữ và tiền tệ, nhấn "Bắt đầu"; xem hoặc bỏ qua phần giới thiệu.
2.Ghi một khoản chi: nhấn nút (+) → chụp ảnh (hoặc "Bỏ qua ảnh") → nhập/kiểm tra số tiền → chọn danh mục và tài khoản → nhấn ✓.
3.Ghi nhiều khoản: nhấn biểu tượng danh sách cạnh nút chụp → gõ mỗi dòng một khoản (ví dụ cà phê 35k) hoặc nhấn "Nói" → "Lưu".
4.Xem tình hình: tab Trang chủ xem lịch; tab Thống kê xem theo danh mục/bản đồ.
5.Đặt ngân sách: tab Ngân sách → (+) hoặc chọn mẫu "Chi tiêu hàng tháng".
6.Sao lưu: tab Cá nhân → "Đăng nhập với Apple" để liên kết và sao lưu.
7.Chia tiền: tab Cá nhân → "Chia tiền" → chọn bạn/nhóm và cách chia.
10.9. Giám sát, bảo trì và cập nhật
•Giám sát: Crashlytics (lỗi), Analytics (sử dụng ẩn danh), Cloud Monitoring (Functions); cảnh báo qua email/Slack khi tỉ lệ lỗi vượt ngưỡng.
•Phát hành: đánh phiên bản ngữ nghĩa (ví dụ 1.7.9 = chính.phụ.vá); chu kỳ 2 tuần; phát hành từng phần (staged rollout) 10% → 50% → 100%.
•Bảo trì: cập nhật Flutter và thư viện hằng tháng, chạy lại toàn bộ test; xử lý góp ý trong vòng 1 tuần.
•Bắt buộc cập nhật: Remote Config đặt phiên bản tối thiểu khi có thay đổi lược đồ/API không tương thích.
•Tương thích ngược: API có /v1; ứng dụng bản cũ vẫn dùng được trong thời gian chuyển tiếp ≥ 90 ngày.
10.10. Phương án sao lưu và phục hồi khi có sự cố
Sự cố	Phương án	Mục tiêu
Người dùng mất/đổi điện thoại	Đăng nhập Apple/Google → kéo lại dữ liệu; hoặc nhập tệp sao lưu	Phục hồi trong vài phút
Lỗi dữ liệu do bản phát hành	Dừng staged rollout, phát hành bản vá; dữ liệu server khôi phục theo thời điểm	RTO ≤ 24 giờ
Firebase gián đoạn	App vẫn dùng bình thường (offline-first); hàng đợi đồng bộ tự chạy lại	Không gián đoạn ghi chép
Xóa nhầm dữ liệu	Khôi phục từ "thùng rác" 30 ngày	RPO ≈ 0 trong 30 ngày
Lộ khóa bí mật	Thu hồi khóa trong Secret Manager, phát hành lại Functions	Xử lý ≤ 4 giờ

CHƯƠNG 11. ĐÁNH GIÁ VÀ HƯỚNG PHÁT TRIỂN
11.1. Kết quả đạt được
Tài liệu hoàn thành toàn bộ các giai đoạn phân tích và thiết kế cho ứng dụng SnapMoney: xác định 8 tác nhân, 42 yêu cầu chức năng, 13 yêu cầu phi chức năng, 17 quy tắc nghiệp vụ, 27 Use Case (4 Use Case được đặc tả chi tiết); 6 sơ đồ phân tích, kiến trúc 4 tầng theo Clean Architecture; lược đồ CSDL 28 bảng chuẩn hóa 3NF; 24 nhóm màn hình; 17 API; bộ 32 test case có ma trận truy vết. (Khi dự án đi vào lập trình, mục này được cập nhật bằng kết quả thực tế: số tính năng đã hoàn thành, độ phủ test, số liệu hiệu năng.)
11.2. Đánh giá mức độ đáp ứng yêu cầu
Bảng 11.1. Mức độ đáp ứng yêu cầu của thiết kế
Nhóm yêu cầu	Mức đáp ứng	Nhận xét
Hồ sơ, onboarding, bảo mật (FR01–FR05)	Đầy đủ	Có thiết kế màn hình, luồng xác thực, khóa sinh trắc học
Ghi nhận giao dịch (FR06–FR16)	Đầy đủ	Có use case chi tiết, thuật toán phân tích văn bản, test case
Tài khoản và tài sản (FR17–FR22)	Đầy đủ ở mức mô hình dữ liệu; một phần ở mức giao diện	Giao diện chi tiết thẻ tín dụng, đầu tư cần bổ sung khi vào sprint
Ngân sách, mục tiêu, định kỳ (FR23–FR26)	Đầy đủ	Có quy tắc cảnh báo và tạo bù giao dịch
Thống kê, báo cáo (FR27–FR30)	Đầy đủ	Bản đồ và quy đổi giờ làm ở mức mô tả
Xã hội hóa (FR31–FR35)	Một phần	Cần thiết kế sâu luồng mời bạn bè, giới hạn quyền chia sẻ
Tích hợp hệ điều hành (FR36–FR38)	Một phần	Phụ thuộc giới hạn của widget/App Intents trên từng nền tảng
Gói Pro, đồng bộ (FR40–FR41)	Đầy đủ	Có API, trạng thái, test case
Phi chức năng (NFR01–NFR13)	Đầy đủ về mục tiêu và cách đo	Cần đo thực tế sau khi có bản chạy
11.3. Ưu điểm của hệ thống
•Trải nghiệm ghi nhanh: ảnh, văn bản tự nhiên, giọng nói, widget, Siri trong cùng một mô hình dữ liệu.
•Kiến trúc rõ ràng, tách nghiệp vụ khỏi giao diện nên dễ kiểm thử và mở rộng.
•Offline-first: dùng được mọi lúc, đồng bộ an toàn và có phương án phục hồi.
•Mô hình Freemium cân bằng: tính năng cốt lõi miễn phí, tính năng nâng cao thuộc gói Pro.
•Quan tâm riêng tư: OCR trên thiết bị, ẩn số tiền, khóa sinh trắc học, tài khoản ẩn danh.
11.4. Hạn chế còn tồn tại
•Thiết kế dựa trên ảnh chụp màn hình nên một số hành vi (ví dụ ý nghĩa bộ đếm "3/3", thuật toán đoán danh mục) là giả định cần xác nhận với người dùng/chủ sản phẩm.
•Chưa kết nối tự động với ngân hàng; người dùng vẫn phải ghi chép (dù đã rút ngắn).
•Đồng bộ "bản mới nhất thắng" có thể mất một chỉnh sửa trong trường hợp xung đột hiếm gặp.
•OCR trên thiết bị có độ chính xác hạn chế với hóa đơn nhăn, chữ viết tay hoặc bố cục lạ.
•Một số tính năng iOS (App Intents, widget) cần mã native riêng, làm tăng chi phí cho Android.
•Phụ thuộc Firebase/Apple/Google về chi phí và chính sách.
11.5. Hướng phát triển trong tương lai
1.Tự động hóa nhập liệu: đọc thông báo/SMS ngân hàng (khi hệ điều hành cho phép) hoặc kết nối Open Banking khi khung pháp lý tại Việt Nam sẵn sàng.
2.AI nâng cao: phân loại danh mục học từ thói quen, trợ lý hỏi đáp "tháng này tôi chi cho cà phê bao nhiêu?", cảnh báo chi tiêu bất thường.
3.Đa tiền tệ đầy đủ với tỉ giá tự động và báo cáo hợp nhất.
4.Mã hóa đầu cuối cho dữ liệu đồng bộ và chia sẻ.
5.Mở rộng nền tảng: Apple Watch, Wear OS, web dashboard, đọc dữ liệu từ ví điện tử.
6.Gamification: chuỗi ngày ghi chép, huy hiệu, mục tiêu tiết kiệm có thử thách.
7.Xã hội: hóa đơn chia lặp lại, nhắc trả nợ tự động, thanh toán/đối soát qua mã QR.

KẾT LUẬN
Tài liệu đã trình bày đầy đủ quy trình phân tích và thiết kế một ứng dụng quản lý chi tiêu cá nhân bằng Flutter, lấy cảm hứng từ ứng dụng CapMoney. Từ nhu cầu "ghi nhanh, xem rõ, kiểm soát được", hệ thống được cụ thể hóa thành các yêu cầu, mô hình nghiệp vụ, kiến trúc phân tầng offline-first, cơ sở dữ liệu quan hệ, giao diện năm tab, API và kế hoạch kiểm thử, triển khai. Các quyết định thiết kế chính (Clean Architecture + Riverpod, SQLite/Drift làm nguồn dữ liệu, Firebase làm backend, kiểm soát tính năng Pro qua entitlement) cân bằng giữa tốc độ phát triển, trải nghiệm và chi phí.
Bước tiếp theo đề xuất: (1) xác nhận các giả định với người dùng thực; (2) dựng nguyên mẫu Figma cho các màn hình còn thiếu (S18–S24); (3) hiện thực hóa MVP theo thứ tự ưu tiên ở Phụ lục A; (4) triển khai CI/CD và bộ test tự động ngay từ sprint đầu.

TÀI LIỆU THAM KHẢO
1.Google. Flutter documentation. https://docs.flutter.dev
2.Dart team. Dart language documentation. https://dart.dev/guides
3.Remi Rousselet. Riverpod documentation. https://riverpod.dev
4.Simon Binder. Drift – Reactive persistence for Dart. https://drift.simonbinder.eu
5.Google. Firebase documentation (Authentication, Cloud Firestore, Cloud Functions, Crashlytics). https://firebase.google.com/docs
6.Apple. Human Interface Guidelines; App Store Review Guidelines; Sign in with Apple. https://developer.apple.com
7.Google. Material Design 3. https://m3.material.io
8.OWASP. Mobile Application Security Verification Standard (MASVS) và Mobile Top 10. https://owasp.org
9.Chính phủ Việt Nam. Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân.
10.Ian Sommerville. Software Engineering, Pearson.
11.Martin Fowler. UML Distilled, Addison-Wesley.
12.Robert C. Martin. Clean Architecture, Prentice Hall.
13.ISO/IEC/IEEE 29148. Systems and software engineering – Life cycle processes – Requirements engineering.
14.Ứng dụng CapMoney, phiên bản 1.7.9 (nguồn: 20 ảnh chụp màn hình do người dùng cung cấp, truy cập tháng 10/2026).

PHỤ LỤC
Phụ lục A: Danh sách yêu cầu chức năng
Chi tiết 42 yêu cầu xem Bảng 2.3. Thống kê và lộ trình ưu tiên (MoSCoW):
Giai đoạn	Nội dung	Yêu cầu
MVP (16 yêu cầu Must)	Hồ sơ, đăng nhập/sao lưu, thêm giao dịch, ảnh, nhập hàng loạt, sửa/xóa, tìm kiếm, lịch, tài khoản, ngân sách, định kỳ, thống kê, danh mục, Pro, đồng bộ	FR01, 03, 05, 06, 07, 11, 13, 14, 15, 17, 23, 26, 27, 39, 40, 41
Giai đoạn 2 (17 yêu cầu Should)	Onboarding, khóa bảo mật, OCR, cảm xúc/vị trí, giọng nói, thẻ/tiết kiệm/vay, cảnh báo, quỹ, xuất báo cáo, bạn bè/nhóm/chia bill, widget, nhắc nhở	FR02, 04, 08, 10, 12, 16, 18, 19, 21, 24, 25, 29, 31, 32, 33, 36, 38
Giai đoạn 3 (9 yêu cầu Could)	Clip 3 giây, đầu tư, chuyến du lịch, bản đồ, quy đổi giờ làm, nhắn tin, chia sẻ tài khoản, Siri, bộ đếm lượt	FR09, 20, 22, 28, 30, 34, 35, 37, 42
Các yêu cầu thuộc gói Pro: FR09, FR18, FR19, FR20, FR21, FR29, FR35.
Phụ lục B: Đặc tả chi tiết Use Case (tóm tắt các UC còn lại)
Bốn Use Case UC02, UC04, UC11, UC18 được đặc tả đầy đủ ở mục 2.12. Các Use Case còn lại:
UC	Mục tiêu	Luồng chính (tóm tắt)	Ngoại lệ chính
UC01	Tạo hồ sơ và làm quen	Nhập tên → chọn ngôn ngữ/tiền tệ/ngôn ngữ nói → Bắt đầu → xem onboarding	Tên rỗng; từ chối quyền ảnh đại diện
UC03	Quay clip 3 giây	Chọn chế độ clip → quay → lưu kèm giao dịch	Không phải Pro → paywall; hết dung lượng
UC05	Xem lịch/danh sách	Chọn Ngày/Tháng → chọn tài khoản → chạm ngày để xem chi tiết	Tháng không có dữ liệu → trạng thái rỗng
UC06	Sửa/xóa/khôi phục	Mở giao dịch → sửa hoặc xóa → hoàn tác trong 5 giây hoặc khôi phục từ thùng rác	Xung đột phiên bản khi đồng bộ
UC07	Tìm kiếm/lọc	Nhập từ khóa → chọn bộ lọc → xem kết quả	Không có kết quả
UC08	Thống kê	Chọn kỳ → xem Thu/Chi/Số dư → chuyển Danh mục/Bản đồ	Chưa có dữ liệu; từ chối vị trí
UC09	Quản lý tài khoản	Thêm/sửa/lưu trữ/đặt mặc định; chuyển tiền	Xóa tài khoản đang có giao dịch → chỉ cho lưu trữ
UC10	Thẻ/tiết kiệm/đầu tư	Tạo thẻ (hạn mức, sao kê) hoặc sổ tiết kiệm (lãi, kỳ hạn)	Không phải Pro; dữ liệu lãi suất không hợp lệ
UC12	Quỹ mục tiêu	Tạo quỹ → nạp/rút → theo dõi % hoàn thành	Rút quá số dư quỹ
UC13	Giao dịch định kỳ	Tạo quy tắc → hệ thống sinh giao dịch đúng kỳ	Lỡ kỳ → tạo bù (BR14)
UC14	Khoản vay	Tạo khoản vay/cho vay → ghi lần trả → tất toán	Trả vượt dư nợ
UC15	Chuyến du lịch	Tạo chuyến → gắn giao dịch → xem tổng chi	Ngày kết thúc trước ngày bắt đầu
UC16	Danh mục	Thêm/sửa/sắp xếp/ẩn/gộp	Xóa danh mục đang dùng (BR08)
UC17	Bạn bè và nhóm	Mời → chấp nhận → tạo nhóm → thêm thành viên	Lời mời hết hạn; người dùng không tồn tại
UC19	Chia sẻ giao dịch	Chọn bạn → gửi bản chụp giao dịch → trò chuyện	Người nhận chặn người gửi
UC20	Chia sẻ tài khoản/ngân sách	Mời người thân → chọn vai trò xem/sửa	Hết quyền Pro; thu hồi quyền
UC21	Xuất báo cáo	Chọn kỳ và định dạng → tạo tệp → chia sẻ	Không phải Pro; dữ liệu quá lớn
UC22	Widget/Siri	Thêm widget; tạo shortcut "Ghi chi tiêu"; lưu từ ảnh chụp màn hình	Phiên bản hệ điều hành không hỗ trợ
UC23	Nhắc nhở	Tạo nhắc hoặc tự sinh từ thẻ/vay → thông báo đúng giờ	Từ chối quyền thông báo
UC24	Quy đổi giờ làm	Nhập lương theo giờ → xem số tiền quy ra số giờ lao động	Lương = 0
UC25	Đăng nhập và sao lưu	Chọn Apple/Google → liên kết → đồng bộ dữ liệu	Tài khoản đã gắn với người dùng khác → hỏi hợp nhất
UC26	Khóa và cài đặt	Bật Face ID/PIN; đổi ngôn ngữ, giao diện, tiền tệ	Thiết bị không có sinh trắc học → dùng PIN
UC27	Nâng cấp Pro	Xem gói → mua → xác thực biên lai → mở quyền; khôi phục giao dịch	Thanh toán thất bại/hủy; biên lai không hợp lệ
Phụ lục C: Các sơ đồ UML
Các sơ đồ dạng văn bản trong tài liệu: Hình 2.1 (Use Case), Hình 3.1 (Activity), Hình 3.2–3.3 (Sequence), Hình 3.4 (Domain), Hình 3.5–3.6 (State), Hình 4.3 (Class). Khuyến nghị chuyển sang công cụ vẽ (draw.io/PlantUML/StarUML) khi nộp bản chính thức; ví dụ PlantUML cho luồng lưu giao dịch:
@startuml
actor User
User -> AddTxScreen: nhấn ✓
AddTxScreen -> AddTransactionNotifier: submit()
AddTransactionNotifier -> AddTransaction: call(newTx)
AddTransaction -> TransactionRepository: add(tx)
TransactionRepository -> LocalDB: insert + enqueue (atomic)
TransactionRepository --> AddTransaction: Result.ok(tx)
AddTransaction --> AddTransactionNotifier: ok
AddTransactionNotifier --> AddTxScreen: đóng màn hình
@enduml
Phụ lục D: Sơ đồ và cấu trúc cơ sở dữ liệu
Sơ đồ ERD ở Hình 5.1; từ điển dữ liệu ở Bảng 5.3–5.7. Câu lệnh tạo bảng mẫu (SQLite):
PRAGMA foreign_keys = ON;

CREATE TABLE accounts (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  type TEXT NOT NULL CHECK (type IN ('cash','bank','ewallet','credit_card')),
  currency TEXT NOT NULL DEFAULT 'VND',
  opening_balance INTEGER NOT NULL DEFAULT 0,
  credit_limit INTEGER, statement_day INTEGER, due_day INTEGER,
  is_default INTEGER NOT NULL DEFAULT 0, archived INTEGER NOT NULL DEFAULT 0,
  created_at INTEGER NOT NULL, updated_at INTEGER NOT NULL, deleted_at INTEGER
);
CREATE UNIQUE INDEX ux_accounts_default ON accounts(is_default) WHERE is_default = 1;

CREATE TABLE transactions (
  id TEXT PRIMARY KEY,
  account_id TEXT NOT NULL REFERENCES accounts(id) ON DELETE RESTRICT,
  to_account_id TEXT REFERENCES accounts(id) ON DELETE RESTRICT,
  category_id TEXT REFERENCES categories(id) ON DELETE RESTRICT,
  type TEXT NOT NULL CHECK (type IN ('expense','income','transfer')),
  amount INTEGER NOT NULL CHECK (amount > 0),
  currency TEXT NOT NULL DEFAULT 'VND',
  note TEXT, occurred_at INTEGER NOT NULL, tz TEXT,
  emotion TEXT, lat REAL, lng REAL, place_name TEXT,
  source TEXT NOT NULL DEFAULT 'manual',
  created_at INTEGER NOT NULL, updated_at INTEGER NOT NULL, deleted_at INTEGER,
  sync_state TEXT NOT NULL DEFAULT 'pending',
  CHECK ((type = 'transfer' AND to_account_id IS NOT NULL AND to_account_id <> account_id)
      OR (type <> 'transfer' AND category_id IS NOT NULL))
);
CREATE INDEX ix_tx_time ON transactions(deleted_at, occurred_at DESC);
CREATE INDEX ix_tx_account_time ON transactions(account_id, occurred_at);
Phụ lục E: Danh sách API
Danh sách đầy đủ ở Bảng 7.1 (API01–API17). Đặc tả mẫu theo OpenAPI 3:
openapi: 3.0.3
info: { title: SnapMoney API, version: 1.0.0 }
paths:
  /v1/sync/push:
    post:
      security: [ { bearerAuth: [] } ]
      parameters: [ { in: header, name: Idempotency-Key, required: true, schema: { type: string } } ]
      responses: { '200': { description: OK }, '409': { description: Xung đột phiên bản } }
Phụ lục F: Các giao diện minh họa
Tương ứng 20 ảnh chụp màn hình tham chiếu:
Ảnh	Nội dung
1	Thiết lập hồ sơ lần đầu (tên, ảnh, tiền tệ, ngôn ngữ, ngôn ngữ nói)
2–7	Sáu trang giới thiệu: ghi nhanh bằng ảnh; mọi ví tiền; ngân sách; thống kê; tiền chung; ghi nhanh mọi lúc
8	Hướng dẫn coachmark "Bước 1/9 – Thêm khoản CHI"
9	Camera ghi nhanh
10–12	Xác nhận giao dịch; chọn danh mục; chọn tài khoản
13	Nhập hàng loạt
14	Trang chủ – lịch tháng
15	Thống kê
16	Tài khoản (Tài khoản/Khoản vay/Đầu tư)
17	Quản lý ngân sách
18–20	Cá nhân: hồ sơ, nâng cấp, đăng nhập Apple, bạn bè/nhóm/chia tiền, công cụ, tùy chọn, hỗ trợ, phiên bản 1.7.9
Phụ lục G: Danh sách trường hợp kiểm thử
32 test case ở Bảng 9.2. Mẫu ghi nhận kết quả:
Mã TC	Ngày	Phiên bản	Thiết bị	Kết quả (Đạt/Không đạt)	Mã lỗi	Ghi chú
						
Phụ lục H: Các tài liệu liên quan khác
H.1. Quy ước mã nguồn và quy trình: theo Effective Dart; nhánh main, develop, feature/*; commit theo Conventional Commits; mỗi pull request cần ít nhất một người duyệt và CI xanh; Definition of Done: có test, có cập nhật tài liệu, đạt lint, không cảnh báo hiệu năng.
H.2. Rủi ro dự án:
Rủi ro	Khả năng	Ảnh hưởng	Giảm thiểu
Bị từ chối khi duyệt App Store (quyền, IAP, đăng nhập)	Trung bình	Cao	Đọc kỹ hướng dẫn, mô tả quyền rõ ràng, dùng IAP/Sign in with Apple đúng quy định
OCR kém chính xác với hóa đơn Việt Nam	Cao	Trung bình	Luôn cho sửa tay; thu thập dữ liệu ẩn danh có đồng ý để cải thiện
Mất dữ liệu khi đồng bộ	Thấp	Cao	Xóa mềm, sao lưu, kiểm thử xung đột, bản sao lưu thủ công
Vượt hạn mức chi phí Firebase	Trung bình	Trung bình	Giới hạn kích thước media, đồng bộ theo lô, cảnh báo ngân sách đám mây
Trễ tiến độ do phạm vi quá rộng	Cao	Cao	Chốt MVP theo Phụ lục A, đưa tính năng Could vào giai đoạn sau
H.3. Từ điển danh mục mặc định (gợi ý): Chi – Ăn uống, Cà phê, Mua sắm, Di chuyển, Hóa đơn & tiện ích, Nhà ở, Sức khỏe, Giáo dục, Giải trí, Quà tặng, Khác. Thu – Lương, Thưởng, Bán đồ, Tiền lãi, Khác.