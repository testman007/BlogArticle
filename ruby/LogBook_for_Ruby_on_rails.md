#LogBook for Ruby on Rails
##Date:2017-12-22

##相關資源
###網站：
[Rails Guild](https://rails.ruby.tw/form_helpers.html)   
[精進 PM 和 UX、UI 知識，這 15 個網站你可千萬不要漏](https://buzzorange.com/techorange/2017/12/18/learning-source-for-pm-ux-ui/)   
[為什麼寫程式這麼難？](https://buzzorange.com/techorange/2017/12/19/learn-coding-4-steps/)
[Ruby 語法放大鏡之「attr_accessor 是幹嘛的?」](https://kaochenlong.com/2015/03/21/attr_accessor/)
[Rails server says port already used, how to kill that process?
](https://stackoverflow.com/questions/4473229/rails-server-says-port-already-used-how-to-kill-that-process)     
[Emmet快捷鍵列表](https://docs.emmet.io/cheat-sheet/)        
[增進程式碼效能與整潔度 clean code](https://www.tenlong.com.tw/products/9780132350884)

##Issue
~~~
Q1:How to add a class to a erb ruby element? 
Q2:How to add check box?
Q3:How to add Flash message?

~~~


###Q1:How to add a class to a erb ruby element? 

###A1:
~~~css
<%= form.submit "Save Item", :class => 'class' %>
~~~

###Q2:How to add check box?

###A2:需用migration在DB新增欄位，並修改 1.routes, 2.views(index.html.erb), 3.controllers(photos_controller.rb)
**File location =>** config/routes.rb        

~~~ruby
 Rails.application.routes.draw do
   root "photos#index"
   
-  resources :photos
+  resources :photos do
+    member do
+      post :is_public
+    end
+  end
 end
~~~

**File Location =>** app/views/photos/index.html.erb

~~~ruby
 <div class="container">
   <table class="table table-striped table-condensed">
     <tr>
+      <th>is_public</th>
       <th>Title</th>
       <th>Show</th>
       <th>Edit</th>
 @@ -9,6 +10,9 @@
 
     <% @photos.each do |photo| %>
       <tr>
+        <td>
+          <%= check_box_tag :is_public, true , photo.is_public, data: { url: is_public_photo_path(photo), method: :post, remote: true } %>
+        </td>
         <td><%= photo.title %>
         <td><%= link_to "Show", photo_path(photo) %>
         <td><%= link_to "Edit", edit_photo_path(photo) %>
~~~

**File Location =>** app/controllers/photos_controller.rb

~~~ruby
 class PhotosController < ApplicationController
-  before_action :set_photo, only: [:show, :edit, :update, :destroy]
+  before_action :set_photo, only: [:show, :edit, :update, :destroy, :is_public]
 
   def index
     @photos = Photo.all
 @@ -38,6 +38,10 @@ def destroy
     redirect_to photos_path
   end
 
+  def is_public
+    @photo.update(is_public: !(@photo.is_public))
+  end
+
   private
 
   def set_photo
~~~


###Q3:How to add Flash message?
###A3:
我們在Part1示範過用Flash來傳遞訊息。它的用處在於redirect時，能夠從這一個request傳遞文字訊息到下一個request，例如從create Action傳遞「成功建立」的訊息到show Action。

flash是一個Hash，其中的鍵你可以自定，常用:notice、:warning或:error等。例如我們在第一個Action中設定它：

```ruby
def create
  @event = Event.create(params[:event])
  flash[:notice] = "成功建立"
  redirect_to event_url(@event)
end
```
那麼在下一個Action中，我們就可以在Template中讀取到這個訊息，通常我們會放在Layout中：

```css
<p><%= flash[:notice] %></p>
```

或是直接用notice這個Helper：

```css
<p><%= notice %></p>
```

使用過一次之後，Rails就會自動清除flash。

另外，有時候你等不及到下一個Action，就想讓Template在同一個Action中讀取到flash值，這時候你可以寫成：

```flash.now[:notice] = "foobar"```
最後，Rails預設針對notice和alert這兩個類型可以直接塞進redirect_to當作參數，例如：
```
redirect_to event_url(@event), :notice => "成功建立"
```
你也可以自行擴充，例如新增一個warning：

# app/controllers/application_controller.rb
```ruby
class ApplicationController < ActionController::Base
  add_flash_types :warning
  #...
end
```

# in your controller
```ruby
redirect_to user_path(@user), warning: "Incomplete profile"
```

# in your view
```ruby
<%= warning %>
```