# Báo cáo Phân tích Lỗi Artifact & GPU Process Crash (Technical Analysis)

Tài liệu này phân tích nguyên nhân kỹ thuật khiến trình duyệt Chrome bị treo (màn hình đỏ/màu sắc bất thường) trên trang chủ Dashboard và các giải pháp tối ưu hóa.

## 1. Nguyên nhân Phân tích

Lỗi này được xác định là **Renderer/GPU Process Crash** (thường được gọi trong ngành là **Lỗi Artifact** hoặc **Composite Storm**). Nó xảy ra khi trình duyệt gặp hiện tượng bùng nổ lớp (**Layer Explosion**), yêu cầu GPU xử lý các tác vụ đồ họa vượt quá ngưỡng an toàn hoặc bị xung đột bộ nhớ đệm hình ảnh.

### A. Lạm dụng `transition: all`
- **Vấn đề**: Thuộc tính `transition: all` yêu cầu trình duyệt theo dõi và nội suy giá trị cho **toàn bộ** thuộc tính CSS (kể cả những cái không thay đổi).
- **Hệ quả**: Khi người dùng di chuyển chuột nhanh qua nhiều card, hàng ngàn phép tính nội suy được kích hoạt cùng lúc, gây quá tải CPU/GPU.

### B. Xung đột Phức hợp Hiệu ứng (Layer Complexity)
Trang chủ sử dụng đồng thời nhiều hiệu ứng nặng đồ họa trên cùng một lớp:
1.  **Fixed Background Mesh**: Ảnh nền Gradient lớn cố định.
2.  **Double Box Shadows**: Bóng đổ đổ đa lớp.
3.  **Shimmer Animation**: Hiệu ứng ánh kim dùng `linear-gradient` di chuyển bằng `translateX`.
4.  **Transform Movement**: Di chuyển card theo trục Y (`translateY`).
5.  **Background Clip Text**: Đổ màu gradient cho chữ.

### C. Cơ chế vẽ lại (Repaint & Composite)
Mỗi card khi di chuột qua sẽ kích hoạt một chuỗi vẽ lại (Repaint). Trình duyệt phải vẽ lại một vùng lớn của màn hình liên tục 60-120 lần mỗi giây.

## 2. Giải pháp Khắc phục

### ✅ Tối ưu hóa Transition
Thay vì dùng `all`, chúng ta giới hạn trình duyệt chỉ tập trung vào các thuộc tính cần thiết:
```css
transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
```

### ✅ Kích hoạt Tăng tốc Phần cứng (Hardware Acceleration)
Sử dụng thuộc tính `will-change` để báo trước cho trình duyệt tạo ra một layer GPU riêng cho card:
```css
.card {
    will-change: transform, box-shadow;
}
```

### ✅ Giảm tải Render
Bằng cách tách biệt các thuộc tính đồ họa, GPU sẽ sử dụng cơ chế "Composite Only", giúp giảm lượng "Repaint" xuống mức tối thiểu.

## 3. Kết luận
Các thay đổi trên đã được áp dụng vào `index.html`. Giao diện vẫn giữ nguyên vẻ đẹp hiện đại nhưng đã được tối ưu hóa để hoạt động ổn định và mượt mà nhất.

## 4. Câu lệnh dành cho AI (AI Fix Prompt)

Bạn có thể sử dụng câu lệnh sau để yêu cầu AI hỗ trợ fix lỗi này một cách tự động:

**Tiếng Anh:**
> "Fix Renderer Process Crash and Graphical Artifacts caused by a **Composite Storm** (or Layer Explosion). Optimize the CSS by replacing `transition: all` with specific properties (transform, box-shadow), using `will-change` to manage compositing layers, and reducing VRAM pressure to prevent GPU memory leaks."

**Tiếng Việt:**
> "Sửa lỗi Renderer Process Crash và hiện tượng **Artifact** (rác hình/màn hình đỏ) do **Composite Storm** (Layer Explosion). Tối ưu hóa CSS bằng cách thay thế `transition: all` bằng các thuộc tính cụ thể, quản lý lớp bằng `will-change` và giảm áp lực bộ nhớ VRAM để tránh rò rỉ bộ nhớ GPU."
