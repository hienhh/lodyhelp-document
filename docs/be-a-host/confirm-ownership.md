## Xác nhận quyền sở hữu cho chổ nghỉ của bạn
>>>
Màn hình này dùng để xác nhận quyền sở hữu cho chổ nghỉ để tăng độ tin cậy của người dùng cho chổ nghỉ này.
>>>

##### Đường dẫn của trang này như sau
```
https://host.lodyhelp.com/vi/property/1/confirm-owner-ship
```
Trong đó số 1 là id của property muốn gửi hỉnh xác nhận quyền sở hữu.
Dưới đây là đường dẫn của file cho màn hình xác nhận quyền sở hữu.
```
pages/_lang/property/_id/confirm-owner-ship.vue
```

Lấy tất cả hình ảnh xác nhận quyền sở hữu cho chổ nghỉ này nếu đã đăng trước đó
```javascript
async asyncData({route, store}){
  let {data} = await store.dispatch('property/getConfirmOwnerShip', route.params.id)
  if(typeof data !== 'undefined' && typeof data[0] !== 'undefined' && typeof data[0].files !== 'undefined' && data[0].files.length > 0 ){
    return {images: data[0].files}
  }
},
```

Đăng tải hình ảnh xác nhận quyền sở hữu
```javascript
loadImg: function(){
  this.errors = []
  if(typeof this.images != 'undefined' && Array.isArray(this.images) && this.images.length + event.target.files.length > 10){
    this.errors.push(this.$t('room.imageLimit'))
    return;
  }else if(!Array.isArray(this.images)){
    this.images = []
  }
  if(this.isUploading) return
  var form = new FormData();
  form.append('type', this.uploadType)
  var imageCount = 0
  for (var i = 0; i < event.target.files.length; i++) {
    var size = (event.target.files[i].size / 1024) / 1024
    if(size <= 2){
      form.append('files['+i+']', event.target.files[i]);
      imageCount++
    }else{
      this.errors.push(this.$t('room.imageMaxFileSize', {name: event.target.files[i].name}))
    }
  }
  var dis = this
  if(!imageCount){
    return
  }
  this.isUploading = true
  if(this.uploadType === 'aws'){
    this.$axios.$post('/be-a-host/images', form).then((response) => {
      response.data.forEach(function(element) {
        dis.images.push(element);
      })
      this.isUploading = false
    })
    .catch((error) => {
      this.isUploading = false
    });
  }else{
    this.$axios.$post('/be-a-host/images', form).then((response) => {
      response.data.forEach(function(element) {
        dis.images.push(element.url);
      });
      this.isUploading = false
    })
    .catch((error) => {
      this.isUploading = false
    });
  }
}
```

Xóa một hình ảnh nào đó
```javascript
removeImage(index){
  this.form.delete('files['+index+']');
  this.images.splice(index, 1);
}
```

Xử lý cập nhật lại thông tin khi nhấn nút cập nhật
```javascript
async submitForm(){
  if (this.$refs.ownerShipForm.validate()) {
    this.errors = []
    this.isLoading = true
    var form = {
      property_id: parseInt(this.$route.params.id),
      files: this.images,
      status: 'pending'
    }
    this.$axios.$post('/be-a-host/confirm/create-or-update', form).then((response) => {
      this.isLoading = false
      this.$router.push({name: 'lang-property', params: {lang: this.locale}})
    })
    .catch((error, data) => {
      this.isLoading = false
      this.errors = error.response.data.errors
    });
  }
}
```