## Tiến trình đăng chổ nghỉ lên hệ thống và cấu trúc file của tiến trình này.

### 1. Màn hình giới thiệu(intro).

>>>
Sau khi đăng nhập thành công thì hệ thống sẽ tự động chuyển vào màn hình [intro](https://host.lodyhelp.com/vi/submit-property/intro) để chọn loại chổ nghỉ và thể loại của chổ nghỉ.
>>>

Thông tin liên quan đến HTML và xử lý API cho màn hình này nằm tổng hợp ở danh sách files như sau
```
pages/_lang/submit-property/intro.vue
```

File này chứa nội dung chính của màn hình intro, trong page này có gọi component chứa những chuyên mục của chổ nghỉ.

```
components/submit-property/PropertyType.vue
```

Sau chọn đầy đủ thông tin hệ thống sẽ chuyển sang bước nhập thông tin cơ bản. Dưới đây là đoạn code để chuyển sang màn hình nhập thông tin cơ bản của property

```javascript
nextStep(){
  this.setCurrentStep(2)
  this.$router.push({name: 'lang-submit-property-basic', params: {lang: this.locale}})
}
```

### 2. Màn hình nhập thông tin cơ bản(basic step).
>>>
Màn hình này sẽ gọi những danh sách APIs trước khi hiển thị form như bên dưới:
 - API lấy danh sách tỉnh/thành phố
 - API lấy danh sách ngôn ngữ của host
>>>

```
pages/_lang/submit-property/basic.vue
```

Đây là file chứa cái page của basic step nhưng thực chất nó sẽ gọi component

```
components/submit-property/BasicStep.vue
```

Vì tất các page như cập nhập chổ nghỉ, tiếp tục đăng chổ nghỉ chưa hoàn tất hoặc admin verify chổ nghỉ trước khi approve đều xài component này. Vậy nên tách ra component cho tiện và có thể sử dụng lại vì cơ bản nó khá giống nhau chỉ một vài chi tiết nhỏ thôi.

>>>
Mọi thông liên quan đến HTML hay call API như nào thì có thể tham khảo ở component BasicStep.vue
Hầu như các chức năng khác liên quan đến đăng chổ nghỉ hay sửa chổ nghỉ có liên quan đến step basic thì đều sử dụng component này hết.
Sau khi click tiếp theo thì sẽ được chuyển sang màn hình chi tiết chổ ở và giá như bên dưới.
>>>

### 3. Màn hình chi tiết chổ ở và giá(detail price).
>>>
Màn hình hình mình sẽ gọi những API dưới:
 - Lấy danh sách amenities
>>>

```
pages/_lang/submit-property/detail-price.vue
```
Đây cũng là trang sử component `components/submit-property/DetailPriceStep.vue` riêng step này có một vài component khác không chỉ đơn thuần là component chứa cái nội dung của step.

Mình sẽ xài thêm 2 components nữa là `SingleRoom.vue` và `MultipleRoom.vue`. Single room là chổ nghỉ không có room mà mọi thông tin đều là của property luôn thay vì lưu ở trong room. Conf MultipleRoom là ngược lại. Một chổ nghỉ có nhiều phòng và mỗi phòng có một vài thông số đặc biệt khác nhau như giá hoặc tiện nghỉ phòng.
```
components/submit-property/SingleRoom.vue
components/submit-property/MultipleRoom.vue
```

>>>
Sau khi click tiếp theo thì sẽ chuyển sang màn hình tiện ích và quy định chổ ở bên dưới.
>>>

### 4. Màn hình tiện nghi và quy định chổ ở.
>>>
Màn hình này để chọn tiện ích và quy định chổ ở cũng như nội dung chi tiết của chổ ở muốn đăng
Màn hình này sẽ gọi 2 APIs sau
 - Lấy danh sách tiện ích chung
 - Lấy danh sách quy định chổ ở
>>>

Màn hình này sẽ sử dụng file `pages/_lang/submit-property/facility-requirement.vue` và trang này cũng sử dụng thêm component `components/submit-property/FacilityRequirementStep.vue`.

>>>
Sau khi nhập đầy đủ thông tin rồi click nút tiếp theo sẽ chuyển hướng sang màn hình tiếp theo là màn hình đăng tải hình ảnh của chổ nghỉ.
>>>

### 5. Màn hình đăng tải hình ảnh cho chổ nghỉ(image step)

>>>
Màn hình này để đăng tải hình ảnh của chổ nghỉ lên. Tất cả hình ảnh đều được đẩy lên Amazon hết
>>>

```
pages/_lang/submit-property/image.vue
```
Đây là file của màn hình image. Nhưng màn hình này cũng sử dụng component `components/submit-property/ImageStep.vue`
Đây là trang khá đơn giản nên vào đọc chút chắc có thể cập nhật hoặc chức năng dễ dàng.

>>>
Sau khi chọn hình ảnh đăng lên rồi click vào nút tiếp theo thì sẽ được chuyển hướng sang màn hình thanh toán
>>>

### 6. Màn hình thanh toán(payment step)
Màn hình này sẽ sử dụng file `pages/_lang/submit-property/payment.vue` và sẽ xài component `components/submit-property/PaymentStep.vue` sau khi submit thì sẽ tự động chuyển sang màn hình quản lý chổ nghỉ của bạn.

Đó là tất cả các bước tạo một chổ nghỉ cũng cấu trúc thư mục để có thể modify chúng.