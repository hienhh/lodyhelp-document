## Quản lý đặt phòng chổ nghỉ của bạn.
Đường dẫn đến trang quản lý đặt phòng chổ nghỉ là
```
https://host.lodyhelp.com/vi/property/1/management-booking
```
File lập trình cho màn hình quản lý đặt phòng chổ nghỉ của bạn là
```
pages/_lang/property/_id/management-booking.vue
```

Trước khi vào màn hình này chúng ta lấy thông tin chổ nghỉ hiện tại với các thông tin liên quan đến chổ nghỉ cũng như số lượng phòng nếu loại chổ nghỉ có nhiều phòng.

```javascript
async asyncData({route, store}) {
  var res = await store.dispatch('property/getPropertyContinue', route.params.id) // Lấy thông tin của property từ một action của vuex
  if(typeof res.data !== 'undefined'){
    var data = res.data
    // Cấu hình lại avalable hoặc not available
    if(data.not_available === '0' || data.not_available === 0){
      data.not_available = false
    }else{
      data.not_available = true
    }
    // Gán lại dữ liệu đã lấy cho biến propery
    return {property: data}
  }
}
```

Cập nhật lại thông tin của phòng hoặc tắt hiển thị cho chổ nghỉ này được sử dụng ở phương thức sau

```javascript
savePropertyInformationData(){
  this.isSubmit = true // Báo cho người dùng biết là đang xử lý cập nhật dữ liệu
  let url = '/be-a-host/property/'+this.$route.params.id+'/update-manage-booking' // Khai báo biến đường dẫn API để xử lý cập nhật dữ liệu
  this.$axios.post(url, this.property)
    .then((response) => {
      this.isSubmit = false // Nếu xử lý thành công thì cập nhật lại trạng thái của xử lý dữ liệu
      this.$router.push({name: 'lang-property', params: {lang: this.locale}}) // Chuyển sang màn hình chổ nghỉ của tôi nếu xử lý thành công
    })
    .catch((error) => {
      this.isSubmit = false
    })
}
```